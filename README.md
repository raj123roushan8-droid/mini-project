# mini-project
 ecs project
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TechBot - AI Assistant for ECE</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* --- YOUR ORIGINAL CSS STYLES --- */
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d); min-height: 100vh; display: flex; justify-content: center; align-items: center; padding: 20px; }
        .container { width: 100%; max-width: 1200px; height: 90vh; background: rgba(255, 255, 255, 0.95); border-radius: 20px; box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3); display: flex; overflow: hidden; }
        .sidebar { width: 250px; background: #2c3e50; color: white; padding: 20px; display: flex; flex-direction: column; overflow-y: auto; }
        .logo { text-align: center; margin-bottom: 30px; }
        .logo h1 { font-size: 24px; margin-top: 10px; }
        .logo i { font-size: 32px; color: #4CAF50; }
        .topics { margin-top: 20px; }
        .topics h3 { margin-bottom: 15px; border-bottom: 1px solid rgba(255, 255, 255, 0.2); padding-bottom: 10px; }
        .topics ul { list-style: none; }
        .topics li { padding: 10px; margin-bottom: 8px; border-radius: 8px; cursor: pointer; transition: background 0.3s; }
        .topics li:hover { background: #34495e; }
        .topics li.active { background: #4CAF50; }
        .chat-container { flex: 1; display: flex; flex-direction: column; }
        .chat-header { padding: 20px; background: #f5f5f5; border-bottom: 1px solid #e0e0e0; display: flex; align-items: center; justify-content: space-between; }
        .chat-header h2 { color: #2c3e50; margin-left: 10px; }
        .chat-header i { color: #4CAF50; font-size: 24px; }
        .api-status { display: flex; align-items: center; background: #e8eaf6; padding: 5px 10px; border-radius: 20px; font-size: 14px; }
        .status-indicator { width: 10px; height: 10px; border-radius: 50%; margin-right: 5px; }
        .status-connected { background: #4CAF50; }
        .chat-messages { flex: 1; padding: 20px; overflow-y: auto; display: flex; flex-direction: column; }
        .message { max-width: 80%; padding: 15px 20px; margin-bottom: 20px; border-radius: 18px; animation: fadeIn 0.3s ease; position: relative; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .user-message { align-self: flex-end; background: #4CAF50; color: white; border-bottom-right-radius: 5px; }
        .bot-message { align-self: flex-start; background: #e8eaf6; color: #333; border-bottom-left-radius: 5px; }
        .message-time { font-size: 12px; margin-top: 8px; opacity: 0.7; text-align: right; }
        .chat-input { padding: 20px; background: #f5f5f5; border-top: 1px solid #e0e0e0; display: flex; }
        .chat-input input { flex: 1; padding: 15px 20px; border: none; border-radius: 30px; outline: none; font-size: 16px; background: white; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); }
        .chat-input button { width: 50px; height: 50px; border-radius: 50%; border: none; background: #4CAF50; color: white; margin-left: 10px; cursor: pointer; transition: background 0.3s; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); }
        .chat-input button:hover { background: #388E3C; transform: scale(1.05); }
        .suggestions { display: flex; flex-wrap: wrap; padding: 15px 20px; background: #f5f5f5; border-top: 1px solid #e0e0e0; }
        .suggestion { padding: 10px 15px; background: white; border-radius: 18px; margin: 5px; cursor: pointer; font-size: 14px; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); transition: all 0.3s; }
        .suggestion:hover { background: #e8eaf6; transform: translateY(-2px); }
        .typing-indicator { display: none; padding: 12px 16px; background: #e8eaf6; border-radius: 18px; align-self: flex-start; margin-bottom: 15px; border-bottom-left-radius: 5px; }
        .typing-indicator span { height: 8px; width: 8px; float: left; margin: 0 1px; background-color: #9E9E9E; display: block; border-radius: 50%; opacity: 0.4; }
        .typing-indicator span:nth-of-type(1) { animation: typing 1s infinite; }
        .typing-indicator span:nth-of-type(2) { animation: typing 1s 0.33s infinite; }
        .typing-indicator span:nth-of-type(3) { animation: typing 1s 0.66s infinite; }
        @keyframes typing { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-5px); } }
        
        /* 3D CSS retained */
        .figure-container { perspective: 1000px; margin: 20px 0; display: flex; justify-content: center; }
        .cube { width: 100px; height: 100px; position: relative; transform-style: preserve-3d; animation: rotate 10s infinite linear; }
        .cube-face { position: absolute; width: 100px; height: 100px; background: rgba(76, 175, 80, 0.8); border: 1px solid #388E3C; display: flex; justify-content: center; align-items: center; font-size: 20px; color: white; }
        .front { transform: translateZ(50px); }
        .back { transform: translateZ(-50px) rotateY(180deg); }
        .right { transform: translateX(50px) rotateY(90deg); }
        .left { transform: translateX(-50px) rotateY(-90deg); }
        .top { transform: translateY(-50px) rotateX(90deg); }
        .bottom { transform: translateY(50px) rotateX(-90deg); }
        @keyframes rotate { from { transform: rotateX(0) rotateY(0) rotateZ(0); } to { transform: rotateX(360deg) rotateY(360deg) rotateZ(360deg); } }
        
        /* Response Content Styling */
        .response-content { line-height: 1.6; }
        .response-content h3 { margin: 15px 0 10px 0; color: #2c3e50; border-bottom: 1px solid #ddd; padding-bottom: 5px; font-weight: bold; }
        .response-content strong { font-weight: bold; color: #1a2a6c; }
        .response-content ul { padding-left: 20px; margin: 10px 0; }
        .response-content li { margin-bottom: 8px; }
        .about-content { text-align: center; font-size: 14px; line-height: 1.6; padding: 10px; }
        .about-content .team-member { margin: 8px 0; padding-left: 5px; background: rgba(255, 255, 255, 0.1); border-radius: 5px; }
        
        @media (max-width: 768px) {
            .container { flex-direction: column; height: auto; }
            .sidebar { width: 100%; padding: 10px; max-height: 200px; }
            .topics ul { display: flex; overflow-x: auto; }
            .topics li { margin-right: 10px; white-space: nowrap; }
            .message { max-width: 90%; }
            .chat-header { flex-direction: column; align-items: flex-start; }
            .api-status { margin-top: 10px; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="sidebar">
            <div class="logo">
                 <i class="fas fa-robot"></i>
                <h1>ECS Bot</h1>
            </div>
            <div class="topics">
                <h3>Topics</h3>
                <ul>
                    <li class="active" data-topic="all">All Topics</li>
                    <li data-topic="circuits">Circuits</li>
                    <li data-topic="programming">Programming</li>
                    <li data-topic="electronics">Electronics</li>
                    <li data-topic="ai-ml">AI & ML</li>
                    <li data-topic="networking">Networking</li>
                    <li data-topic="digital-logic">Digital Logic</li>
                </ul>
            </div>
            <div class="topics">
                <h3>About</h3>
                <div class="about-content">
                    <p>Designed by:</p>
                    <div class="team-member">DIVYANSHU YADAV, 2300102719</div>
                    <div class="team-member">ARYAN SINGH 2300103317</div>
                    <div class="team-member">RAHUL KUMAR, 2300102179</div>
                </div>
            </div>
        </div>
        
        <div class="chat-container">
            <div class="chat-header">
                <div>
                    <i class="fas fa-robot"></i>
                    <h2>ECS Bot Assistant</h2>
                </div>
                <div class="api-status">
                    <div class="status-indicator status-connected"></div>
                    <span>AI Model: Gemini 2.5 Flash</span>
                </div>
            </div>
            
            <div class="chat-messages" id="chat-messages">
                <div class="message bot-message">
                    <p>Hello! I'm TechBot, your AI assistant for Electrical and Computer Engineering. I can answer questions across various topics with detailed explanations. How can I help you today?</p>
                    <div class="message-time">Just now</div>
                </div>
            </div>
            
            <div class="typing-indicator" id="typing-indicator">
                <span></span>
                <span></span>
                <span></span>
            </div>
            
            <div class="suggestions" id="suggestions">
                <!-- Suggestions will be dynamically added here -->
            </div>
            
            <div class="chat-input">
                <input type="text" id="user-input" placeholder="Ask a question about ECE...">
                <button id="send-button"><i class="fas fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <script>
    // -------------------------------------------------------------
    // STEP 1: OPENAI API KEY (your sk-proj-xxxxxxxxx key goes here)
    // -------------------------------------------------------------
    const API_KEY = "sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

    // OpenAI API endpoint
    const API_URL = "https://api.openai.com/v1/chat/completions";

    document.addEventListener('DOMContentLoaded', function () {
        const chatMessages = document.getElementById('chat-messages');
        const userInput = document.getElementById('user-input');
        const sendButton = document.getElementById('send-button');
        const typingIndicator = document.getElementById('typing-indicator');
        const suggestionsContainer = document.getElementById('suggestions');

        let activeTopic = 'all';

        const knowledgeBase = {
            "circuits": [
                { question: "What is Ohm's Law?" },
                { question: "Explain Kirchhoff's Circuit Laws" }
            ],
            "programming": [
                { question: "What is object-oriented programming?" }
            ],
            "electronics": [
                { question: "What is a transistor?" },
                { question: "How does a diode work?" }
            ],
            "ai-ml": [
                { question: "What is Machine Learning?" },
                { question: "Explain Neural Networks" }
            ],
            "networking": [
                { question: "What is the OSI Model?" },
                { question: "Difference between TCP and UDP" }
            ],
            "digital-logic": [
                { question: "What are Logic Gates?" },
                { question: "Explain Boolean Algebra" }
            ]
        };

        updateSuggestions();

        sendButton.addEventListener('click', handleUserMessage);
        userInput.addEventListener('keypress', e => {
            if (e.key === 'Enter') handleUserMessage();
        });

        document.querySelectorAll('.topics li').forEach(item => {
            item.addEventListener('click', function () {
                if (this.dataset.topic) {
                    document.querySelectorAll('.topics li').forEach(li => li.classList.remove('active'));
                    this.classList.add('active');
                    activeTopic = this.dataset.topic;
                    updateSuggestions();
                }
            });
        });

        function updateSuggestions() {
            suggestionsContainer.innerHTML = '';
            let questions = [];

            if (activeTopic === 'all') {
                for (const topic in knowledgeBase) {
                    questions = questions.concat(knowledgeBase[topic]);
                }
            } else {
                questions = knowledgeBase[activeTopic];
            }

            const selected = questions.slice(0, 4);

            selected.forEach(q => {
                const suggestion = document.createElement('div');
                suggestion.classList.add('suggestion');
                suggestion.textContent = q.question;
                suggestion.addEventListener('click', function () {
                    userInput.value = this.textContent;
                    handleUserMessage();
                });
                suggestionsContainer.appendChild(suggestion);
            });
        }

        async function handleUserMessage() {
            const message = userInput.value.trim();
            if (message === '') return;

            addMessage(message, 'user');
            userInput.value = '';
            typingIndicator.style.display = 'block';
            chatMessages.scrollTop = chatMessages.scrollHeight;

            try {
                const botResponse = await callOpenAI(message, activeTopic);
                typingIndicator.style.display = 'none';
                addMessage(botResponse, 'bot', true);
            } catch (error) {
                typingIndicator.style.display = 'none';
                addMessage(<span style="color:red">Error:</span> ${error.message}, 'bot', true);
            }
        }

        async function callOpenAI(userMessage, topic) {
            if (!API_KEY.startsWith("sk-")) {
                throw new Error("Invalid OpenAI API Key");
            }

            let systemContext = "You are ECS TechBot, an AI assistant for Electrical & Computer Engineering students.";
            if (topic !== "all") {
                systemContext += ` Focus your explanation on: ${topic.toUpperCase()}.`;
            }

            const payload = {
                model: "gpt-4o-mini",
                messages: [
                    { role: "system", content: systemContext },
                    { role: "user", content: userMessage }
                ],
                temperature: 0.7
            };

            const response = await fetch(API_URL, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Authorization": Bearer ${API_KEY}
                },
                body: JSON.stringify(payload)
            });

            if (!response.ok) {
                const err = await response.json();
                console.error(err);
                throw new Error("API Error: " + response.status);
            }

            const data = await response.json();
            let text = data.choices[0].message.content;

            text = text.replace(/\\(.?)\\*/g, "<strong>$1</strong>");
            text = text.replace(/\n/g, "<br>");

            return <div class="response-content">${text}</div>;
        }

        function addMessage(content, sender, isHTML = false) {
            const messageElement = document.createElement('div');
            messageElement.classList.add('message');
            messageElement.classList.add(sender + '-message');

            if (isHTML) {
                messageElement.innerHTML = content;
            } else {
                const p = document.createElement('p');
                p.textContent = content;
                messageElement.appendChild(p);
            }

            const time = document.createElement('div');
            time.classList.add('message-time');
            time.textContent = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
            messageElement.appendChild(time);

            chatMessages.appendChild(messageElement);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }
    });
</script>
</body>
</html>
