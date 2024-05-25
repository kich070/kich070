<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Real-Time Chat</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #1e1e1e;
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        #chat-container {
            width: 95%;
            max-width: 600px;
            background-color: #3b3f47;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.5);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }
        #messages {
            flex: 1;
            overflow-y: auto;
            padding: 10px;
            border-bottom: 1px solid #ccc;
        }
        #messages div {
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 5px;
            background-color: #444950;
            word-break: break-word;
        }
        #new-message {
            display: flex;
            background-color: #333;
        }
        #new-message input {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 0 0 0 8px;
            background-color: #555;
            color: white;
        }
        #new-message button {
            padding: 10px;
            border: none;
            background-color: #61dafb;
            color: black;
            cursor: pointer;
            border-radius: 0 0 8px 0;
        }
    </style>
</head>
<body>

<div id="chat-container">
    <div id="messages"></div>
    <div id="new-message">
        <input type="text" id="message" placeholder="Type a message...">
        <button onclick="sendMessage()">Send</button>
    </div>
</div>

<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/8.6.8/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.6.8/firebase-database.js"></script>
<script>
    // Your web app's Firebase configuration
    var firebaseConfig = {
        apiKey: "YOUR_API_KEY",
        authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
        databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
        projectId: "YOUR_PROJECT_ID",
        storageBucket: "YOUR_PROJECT_ID.appspot.com",
        messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
        appId: "YOUR_APP_ID"
    };
    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);

    var database = firebase.database();

    function sendMessage() {
        var message = document.getElementById("message").value;
        if (message !== "") {
            database.ref("messages").push().set({
                "message": message,
            });
            document.getElementById("message").value = "";
        }
    }

    database.ref("messages").on("child_added", function(snapshot) {
        var html = "";
        html += "<div>" + snapshot.val().message + "</div>";
        document.getElementById("messages").innerHTML += html;
    });
</script>

</body>
</html>
