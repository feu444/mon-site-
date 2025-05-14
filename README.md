<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Chat Public</title>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>
  <style>
    body {
      background-color: #121212;
      color: white;
      font-family: Arial, sans-serif;
      padding: 20px;
    }
    #chat-box {
      border: 1px solid #444;
      padding: 10px;
      height: 300px;
      overflow-y: scroll;
      background-color: #1f1f1f;
      margin-bottom: 10px;
    }
    .message {
      margin-bottom: 5px;
    }
    .timestamp {
      color: gray;
      font-size: 0.8em;
    }
  </style>
</head>
<body>
  <h1>💬 Chat Public</h1>

  <div id="chat-box"></div>

  <input type="text" id="message-input" placeholder="Votre message..." />
  <button onclick="sendMessage()">Envoyer</button>

  <script>
    // Configuration Firebase (REMPLACEZ CES INFOS PAR LES VÔTRES DE FIREBASE)
    const firebaseConfig = {
      apiKey: "VOTRE_API_KEY",
      authDomain: "VOTRE_AUTH_DOMAIN",
      databaseURL: "VOTRE_DATABASE_URL",
      projectId: "VOTRE_PROJECT_ID",
      storageBucket: "VOTRE_STORAGE_BUCKET",
      messagingSenderId: "VOTRE_MESSAGING_SENDER_ID",
      appId: "VOTRE_APP_ID"
    };

    // Initialisation
    const app = firebase.initializeApp(firebaseConfig);
    const database = firebase.database();
    const messagesRef = database.ref("messages");

    // Envoi du message
    function sendMessage() {
      const input = document.getElementById("message-input");
      const text = input.value.trim();
      if (text) {
        const now = new Date();
        const timestamp = now.toLocaleString("fr-FR");

        messagesRef.push({
          text: text,
          timestamp: timestamp
        });

        input.value = "";
      }
    }

    // Affichage en temps réel
    messagesRef.on("child_added", function(snapshot) {
      const message = snapshot.val();
      const chatBox = document.getElementById("chat-box");
      const div = document.createElement("div");
      div.className = "message";
      div.innerHTML = `<span>${message.text}</span><div class="timestamp">${message.timestamp}</div>`;
      chatBox.appendChild(div);
      chatBox.scrollTop = chatBox.scrollHeight;
    });
  </script>
</body>
</html>
