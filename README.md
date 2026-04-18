<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Nova AI Chatbot</title>

  <style>
    body {
      font-family: Arial;
      background: #f4f4f4;
      text-align: center;
    }

    h2 {
      margin-top: 20px;
    }

    #chatbox {
      width: 400px;
      height: 500px;
      border: 1px solid #ccc;
      overflow-y: auto;
      padding: 10px;
      background: #fff;
      margin: 10px auto;
      border-radius: 10px;
    }

    .user {
      text-align: right;
      color: blue;
      margin: 5px;
    }

    .bot {
      text-align: left;
      color: green;
      margin: 5px;
    }

    #inputArea {
      width: 400px;
      margin: auto;
      display: flex;
    }

    input {
      flex: 1;
      padding: 10px;
    }

    button {
      padding: 10px;
    }
  </style>
</head>

<body>

<h2>🤖 Nova AI Chatbot</h2>

<div id="chatbox"></div>

<div id="inputArea">
  <input type="text" id="userInput" placeholder="Type a message..." />
  <button onclick="sendMessage()">Send</button>
</div>

<script>
const chatbox = document.getElementById("chatbox");

// ✅ Welcome message
window.onload = () => {
  addMessage("Hello! I'm Nova 🤖. How can I help you today?", "bot");
};

function addMessage(message, className) {
  const div = document.createElement("div");
  div.className = className;
  div.innerText = message;
  chatbox.appendChild(div);
  chatbox.scrollTop = chatbox.scrollHeight;
}

// ✅ Simple custom replies
function getCustomReply(text) {
  text = text.toLowerCase();

  if (text.includes("hello") || text.includes("hi")) {
    return "Hey there! 👋";
  }
  if (text.includes("your name")) {
    return "I'm Nova, your AI assistant 🤖";
  }
  if (text.includes("how are you")) {
    return "I'm just code, but I'm running perfectly 😄";
  }
  if (text.includes("bye")) {
    return "Goodbye! Have a great day 👋";
  }

  return null; // fallback to AI
}

async function sendMessage() {
  const input = document.getElementById("userInput");
  const userText = input.value;

  if (!userText) return;

  addMessage(userText, "user");
  input.value = "";

  // ✅ Check custom reply first
  const customReply = getCustomReply(userText);
  if (customReply) {
    addMessage(customReply, "bot");
    return;
  }

  // ⚠️ Replace with your API key
  const API_KEY = "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";

  try {
    const response = await fetch("https://api.openai.com/v1/chat/completions", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + API_KEY
      },
      body: JSON.stringify({
        model: "gpt-4o-mini",
        messages: [
          { role: "system", content: "You are Nova, a friendly AI assistant." },
          { role: "user", content: userText }
        ]
      })
    });

    const data = await response.json();
    const botReply = data.choices[0].message.content;

    addMessage(botReply, "bot");

  } catch (error) {
    addMessage("Error: " + error.message, "bot");
  }
}
</script>

</body>
</html>
