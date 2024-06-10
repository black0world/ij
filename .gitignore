const express = require('express');
const http = require('http');
const socketIo = require('socket.io');
const path = require('path');
const fs = require('fs');

const app = express();
const server = http.createServer(app);
const io = socketIo(server);

let messages = [];

// Serve static files from the current directory
app.use(express.static(path.join(__dirname)));

// Serve the HTML file on the root route
app.get('/', (req, res) => {
    res.send(`
    <!DOCTYPE html>
    <html>
    <head>
        <title>Black World</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                display: flex;
                justify-content: center;
                align-items: center;
                height: 100vh;
                margin: 0;
                background-color: #f0f0f0;
            }

            .hidden {
                display: none;
            }

            #login-container, #chat-container {
                background: white;
                padding: 20px;
                border-radius: 5px;
                box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            }

            #login-container {
                text-align: center;
            }

            #login-container h1 {
                margin-bottom: 20px;
            }

            #login-form input {
                padding: 10px;
                border: 1px solid #ccc;
                border-radius: 5px;
                margin-bottom: 10px;
            }

            #login-form button {
                padding: 10px 20px;
                border: none;
                background: #007bff;
                color: white;
                border-radius: 5px;
                cursor: pointer;
            }

            #chat-container {
                max-width: 500px;
                width: 100%;
            }

            #messages {
                list-style-type: none;
                padding: 0;
                margin-bottom: 10px;
                max-height: 300px;
                overflow-y: auto;
            }

            #messages li {
                padding: 8px;
                margin-bottom: 10px;
                background: #f1f1f1;
                border-radius: 5px;
            }

            #chat-form {
                display: flex;
            }

            #chat-form input {
                flex: 1;
                padding: 10px;
                border: 1px solid #ccc;
                border-radius: 5px;
                margin-right: 10px;
            }

            #chat-form button {
                padding: 10px 20px;
                border: none;
                background: #007bff;
                color: white;
                border-radius: 5px;
                cursor: pointer;
            }

            #chat-container {
                width: 90%;
                max-width: 1000px;
                margin: 50px auto;
                padding: 20px;
                border: 1px solid #ccc;
                background-color: white;
            }

            #messages {
                height: 500px;
                overflow-y: scroll;
                border: 1px solid #ccc;
                padding: 10px;
                margin-bottom: 10px;
            }

            #message-input {
                width: calc(100% - 80px); /* Adjusted for emoji button */
                padding: 10px;
            }

            #send-button {
                padding: 10px 20px;
            }

            #emoji-button {
                padding: 10px 20px;
            }

            .emoji-picker {
                display: none;
                position: absolute;
                bottom: 50px;
                left: 10px;
                border: 1px solid #ccc;
                background-color: white;
                padding: 10px;
                box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            }
        </style>
    </head>
    <body>
        <div id="login-container">
            <h1>Black World</h1>
            <form id="login-form">
                <input id="username" autocomplete="off" placeholder="Enter username" /><button>Login</button>
            </form>
        </div>
        <div id="chat-container" class="hidden">
            <h1>Black World Chat</h1>
            <ul id="messages"></ul>
            <form id="chat-form" action="">
                <input id="m" autocomplete="off" /><button id="emoji-button" type="button">😀</button><button>Send</button>
            </form>
            <div id="emoji-picker" class="emoji-picker"></div>
        </div>
        <script src="https://cdn.socket.io/4.0.0/socket.io.min.js"></script>
        <script>
            const socket = io();

            let username = '';

            // Handle receiving message history
            socket.on('message history', (messages) => {
              messages.forEach((data) => {
                const item = document.createElement('li');
                item.textContent = \`\${data.username}: \${data.message}\`;
                messagesList.appendChild(item);
              });
            });

            const loginForm = document.getElementById('login-form');
            const chatForm = document.getElementById('chat-form');
            const usernameInput = document.getElementById('username');
            const messageInput = document.getElementById('m');
            const messagesList = document.getElementById('messages'); // Ensure this is the same as 'messages'
            const loginContainer = document.getElementById('login-container');
            const chatContainer = document.getElementById('chat-container');

            loginForm.addEventListener('submit', (e) => {
                e.preventDefault();
                username = usernameInput.value.trim();
                if (username) {
                    socket.emit('set username', username);
                    loginContainer.classList.add('hidden');
                    chatContainer.classList.remove('hidden');
                }
            });

            chatForm.addEventListener('submit', (e) => {
                e.preventDefault();
                if (messageInput.value) {
                    socket.emit('chat message', { username: username, message: messageInput.value });
                    messageInput.value = ''; // يجب تفريغ حقل الرسالة بعد إرسال الرسالة
                }
            });

            socket.on('chat message', (data) => {
                const item = document.createElement('li');
                item.textContent = \`\${data.username}: \${data.message}\`;
                messagesList.appendChild(item); // Ensure this is using 'messagesList'
                window.scrollTo(0, document.body.scrollHeight);
            });

            // Emoji picker functionality
            const emojiButton = document.getElementById('emoji-button');
            const emojiPicker = document.getElementById('emoji-picker');

            emojiButton.addEventListener('click', () => {
              if (emojiPicker.style.display === 'none' || emojiPicker.style.display === '') {
                emojiPicker.style.display = 'block';
                emojiPicker.innerHTML = '';
const emojis = [
  '😀', '😃', '😄', '😁', '😆', '😅', '🤣', '😂', '🙂', '🙃', 
  '😉', '😊', '😇', '🥰', '😍', '🤩', '😘', '😗', '😚', '😙', 
  '😋', '😛', '😜', '🤪', '😝', '🤑', '🤗', '🤭', '🤫', '🤔', 
  '🤐', '🤨', '😐', '😑', '😶', '😏', '😒', '🙄', '😬', '🤥', 
  '😌', '😔', '😪', '🤤', '😴', '😷', '🤒', '🤕', '🤢', '🤮', 
  '🤧', '😵', '🤯', '🤠', '🥳', '😎', '🤓', '🧐', '😕', '😟', 
  '🙁', '☹️', '😮', '😯', '😲', '😳', '🥺', '😦', '😧', '😨', 
  '😰', '😥', '😢', '😭', '😱', '😖', '😣', '😞', '😓', '😩', 
  '😫', '🥱', '😤', '😡', '😠', '🤬', '😈', '👿', '💀', '☠️', 
  '💩', '🤡', '👹', '👺', '👻', '👽', '👾', '🤖', '😺', '😸', 
  '😹', '😻', '😼', '😽', '🙀', '😿', '😾', '💋', '💌', '💘', 
  '💝', '💖', '💗', '💓', '💞', '💕', '💟', '❣️', '💔', '❤️', 
  '🧡', '💛', '💚', '💙', '💜', '🤎', '🖤', '🤍', '👋', '🤚', 
  '🖐️', '✋', '🖖', '👌', '🤏', '✌️', '🤞', '🤟', '🤘', 
  '🤙', '👈', '👉', '👆', '🖕', '👇', '☝️', '👍', '👎', '✊', 
  '👊', '🤛', '🤜', '👏', '🙌', '👐', '🤲', '🤝', '🙏', '💅', 
  '🤳', '💪', '🦵', '🦶', '👂', '🦻', '👃', '🧠', '🦷', '🦴', 
  '👀', '👁️', '👅', '👄', '💋', '🤑', '💰', '💵', '💴', '💶', 
  '💷', '💸', '💳', '🧾', '💹', '📈', '📉', '📊', '💹'
];

                emojis.forEach((emoji) => {
                  const emojiSpan = document.createElement('span');
                  emojiSpan.textContent = emoji;
                  emojiSpan.style.cursor = 'pointer';
                  emojiSpan.style.padding = '5px';
                  emojiSpan.addEventListener('click', () => {
                    messageInput.value += emoji; // Ensure this is using 'messageInput'
                    emojiPicker.style.display = 'none';
                  });
                  emojiPicker.appendChild(emojiSpan);
                });
              } else {
                emojiPicker.style.display = 'none';
              }
            });
        </script>
    </body>
    </html>
    `);
});

io.on('connection', (socket) => {
    console.log('a user connected');

    socket.on('set username', (username) => {
        socket.username = username;
        // Send the message history to the newly connected user
        socket.emit('message history', messages);
    });

    socket.on('chat message', (data) => {
        const messageData = { username: data.username, message: data.message };
        messages.push(messageData);
        io.emit('chat message', messageData);
    });

    socket.on('disconnect', () => {
        console.log('user disconnected');
    });
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
    console.log(`listening on *:${PORT}`);
});

