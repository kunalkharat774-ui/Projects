<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Nova Assistant Chat</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
<style>
/* Reset & Font */
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif;scroll-behavior:smooth;}
body{transition:0.5s;background:linear-gradient(135deg,#0f0c29 0%,#302b63 50%,#24243e 100%);color:#fff;overflow-x:hidden;}
body.light{background:linear-gradient(135deg,#f0f0f0 0%,#c0c0ff 50%,#fff 100%);color:#111;}
/* Floating blobs */
.blob{position:absolute;border-radius:50%;opacity:0.6;animation:float 10s infinite alternate;z-index:0;}
.blob1{width:150px;height:150px;background:#ff6ec7;top:-50px;left:-50px;animation-delay:0s;}
.blob2{width:200px;height:200px;background:#6ec1ff;top:200px;right:-100px;animation-delay:2s;}
.blob3{width:120px;height:120px;background:#ffe66d;bottom:50px;left:20px;animation-delay:4s;}
@keyframes float{0%{transform:translateY(0) translateX(0);}100%{transform:translateY(-30px) translateX(30px);}}

/* Header */
header{background:rgba(0,0,0,0.5);backdrop-filter:blur(10px);color:white;padding:15px 20px;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;z-index:10;transition:0.5s;}
body.light header{background:rgba(255,255,255,0.8);color:#111;}
header h1{font-size:1.6rem;font-weight:700;letter-spacing:1px;}
header nav a{color:white;margin-left:15px;text-decoration:none;font-weight:bold;position:relative;transition:0.3s;font-size:0.95rem;}
body.light header nav a{color:#111;}
header nav a::after{content:"";position:absolute;width:0;height:2px;background:#fff;left:0;bottom:-5px;transition:0.3s;}
body.light header nav a::after{background:#111;}
header nav a:hover::after{width:100%;}

/* Dark/Light toggle button */
#modeToggle{cursor:pointer;padding:6px 12px;border-radius:6px;background:#ff6ec7;color:white;font-weight:bold;transition:0.3s;}
#modeToggle:hover{background:#6ec1ff;}

/* Hero */
.hero{text-align:center;padding:80px 15px;position:relative;z-index:1;}
.hero h2{font-size:2rem;margin-bottom:15px;background:linear-gradient(90deg,#ff6ec7,#6ec1ff,#ffe66d);-webkit-background-clip:text;-webkit-text-fill-color:transparent;animation:textGradient 5s infinite alternate;}
@keyframes textGradient{0%{background-position:0%}100%{background-position:100%}}
.hero p{font-size:1rem;margin-bottom:30px;color:#ddd;animation:fadeIn 2s ease forwards;opacity:0;animation-delay:0.5s;}
body.light .hero p{color:#333;}
.hero button{padding:12px 30px;font-size:1rem;border:none;border-radius:50px;background:#ff6ec7;color:white;cursor:pointer;font-weight:bold;transition:0.5s;box-shadow:0 4px 12px rgba(0,0,0,0.3);}
.hero button:hover{transform:scale(1.05);background:#6ec1ff;box-shadow:0 6px 18px rgba(0,0,0,0.4);}

/* Sections */
section{padding:60px 15px;max-width:900px;margin:0 auto;text-align:center;transition:0.5s;}
.about h3,.contact h3{font-size:2rem;margin-bottom:15px;}
.about p{font-size:1rem;line-height:1.6;color:#ddd;animation:fadeUp 2s ease forwards;opacity:0;}
body.light .about p{color:#333;}
.contact{background:rgba(255,255,255,0.05);}
body.light .contact{background:rgba(0,0,0,0.05);}
.contact form{max-width:400px;margin:0 auto;display:flex;flex-direction:column;gap:12px;animation:fadeUp 2s ease forwards;opacity:0;animation-delay:0.3s;}
.contact input,.contact textarea{padding:12px;font-size:0.95rem;border:1px solid #ccc;border-radius:8px;outline:none;background:rgba(255,255,255,0.1);color:#fff;}
body.light .contact input,.body.light .contact textarea{background:rgba(0,0,0,0.1);color:#111;}
.contact input::placeholder,.contact textarea::placeholder{color:#ddd;}
body.light .contact input::placeholder,.body.light .contact textarea::placeholder{color:#555;}
.contact button{padding:12px;font-size:1rem;border:none;border-radius:50px;background:#ff6ec7;color:white;cursor:pointer;font-weight:bold;transition:0.3s;}
.contact button:hover{transform:scale(1.05);background:#6ec1ff;}

/* Footer */
footer{background:#111;color:white;padding:25px 15px;text-align:center;letter-spacing:1px;font-size:0.9rem;transition:0.5s;}
body.light footer{background:#ddd;color:#111;}

/* Animations */
@keyframes fadeIn{from{opacity:0;transform:translateY(20px);}to{opacity:1;transform:translateY(0);}}
@keyframes fadeUp{from{opacity:0;transform:translateY(40px);}to{opacity:1;transform:translateY(0);}}

/* Chat Bubble & Box */
.chat-bubble{position:fixed;bottom:25px;right:25px;width:60px;height:60px;border-radius:50%;background:#ff6ec7;display:flex;justify-content:center;align-items:center;cursor:pointer;box-shadow:0 4px 15px rgba(0,0,0,0.3);z-index:20;transition:0.3s;}
.chat-bubble:hover{transform:scale(1.1);background:#6ec1ff;}
.chat-icon{font-size:1.8rem;color:white;}
.chat-box{position:fixed;bottom:100px;right:25px;width:300px;max-width:90%;background:rgba(0,0,0,0.9);border-radius:12px;box-shadow:0 4px 15px rgba(0,0,0,0.5);display:none;flex-direction:column;overflow:hidden;z-index:20;transition:0.5s;}
body.light .chat-box{background:rgba(255,255,255,0.95);}
.chat-header{padding:12px 15px;background:#6ec1ff;font-weight:bold;color:white;}
body.light .chat-header{color:#111;}
.chat-messages{padding:12px;max-height:300px;overflow-y:auto;display:flex;flex-direction:column;gap:8px;}
.chat-messages .user{align-self:flex-end;background:#ff6ec7;padding:8px 12px;border-radius:12px;color:white;max-width:80%;}
body.light .chat-messages .user{color:white;}
.chat-messages .bot{align-self:flex-start;background:#333;padding:8px 12px;border-radius:12px;color:white;max-width:80%;}
body.light .chat-messages .bot{background:#eee;color:#111;}
.typing{font-style:italic;font-size:0.85rem;color:#ccc;}
body.light .typing{color:#555;}
.chat-input{display:flex;border-top:1px solid #555;}
body.light .chat-input{border-top:1px solid #ccc;}
.chat-input input{flex:1;padding:10px;border:none;outline:none;border-radius:0;background:#222;color:white;}
body.light .chat-input input{background:#eee;color:#111;}
.chat-input button{padding:10px;background:#ff6ec7;border:none;cursor:pointer;color:white;font-weight:bold;transition:0.3s;}
.chat-input button:hover{background:#6ec1ff;}
.quick-replies{display:flex;flex-wrap:wrap;gap:5px;margin-top:8px;}
.quick-replies button{background:#6ec1ff;color:white;border:none;border-radius:20px;padding:5px 10px;font-size:0.85rem;cursor:pointer;transition:0.3s;}
.quick-replies button:hover{background:#ff6ec7;}

/* Responsive */
@media screen and (max-width:768px){header h1{font-size:1.4rem;}header nav a{font-size:0.85rem;margin-left:10px;}.hero h2{font-size:1.6rem;}.hero p{font-size:0.95rem;}.hero button{font-size:0.95rem;padding:10px 25px;}.about h3,.contact h3{font-size:1.5rem;}.about p,.contact form input,.contact form textarea{font-size:0.9rem;}.contact button{font-size:0.95rem;padding:10px 25px;}}
@media screen and (max-width:480px){.hero h2{font-size:1.3rem;}.hero p{font-size:0.85rem;}header{padding:10px 15px;}header h1{font-size:1.2rem;}header nav a{font-size:0.8rem;margin-left:8px;}.about h3,.contact h3{font-size:1.2rem;}}
</style>
</head>
<body>
<!-- Blobs -->
<div class="blob blob1"></div>
<div class="blob blob2"></div>
<div class="blob blob3"></div>
<header>
<h1>Nova Assistant</h1>
<nav><a href="#about">About</a><a href="#contact">Contact</a></nav>
<button id="modeToggle">Light Mode</button>
</header>
<section class="hero">
<h2>Let's create your custom chat assistant</h2>
<p>Nova Assistant helps you build interactive chat experiences easily.</p>
<button>Get Started</button>
</section>
<section id="about" class="about">
<h3>About Nova Assistant</h3>
<p>Nova Assistant is a powerful chat platform that lets you design your own chatbot using simple tools and intuitive design. Engage users with dynamic conversations and intelligent responses without any coding skills.</p>
</section>
<section id="contact" class="contact">
<h3>Contact Us</h3>
<form>
<input type="text" placeholder="Your Name" required>
<input type="email" placeholder="Your Email" required>
<textarea rows="5" placeholder="Your Message" required></textarea>
<button type="submit">Send Message</button>
</form>
</section>
<footer>© 2025 Nova Assistant Chat. All Rights Reserved.</footer>
<!-- Chat Bubble -->
<div class="chat-bubble" id="chatBubble"><span class="chat-icon">💬</span></div>
<!-- Chat Box -->
<div class="chat-box" id="chatBox">
<div class="chat-header">Nova Assistant</div>
<div class="chat-messages" id="chatMessages">
<div class="bot">Hi! I’m Nova. Ask me anything!</div>
<div class="quick-replies" id="quickReplies">
<button>Hi</button>
<button>How are you?</button>
<button>Help</button>
<button>Contact</button>
</div>
</div>
<div class="chat-input">
<input type="text" id="userInput" placeholder="Type your message..." />
<button id="sendBtn">Send</button>
</div>
</div>
<script>
const chatBubble=document.getElementById('chatBubble');
const chatBox=document.getElementById('chatBox');
const chatMessages=document.getElementById('chatMessages');
const userInput=document.getElementById('userInput');
const sendBtn=document.getElementById('sendBtn');
const quickReplies=document.getElementById('quickReplies');
const modeToggle=document.getElementById('modeToggle');

chatBubble.addEventListener('click',()=>{chatBox.style.display=chatBox.style.display==='flex'?'none':'flex';});
modeToggle.addEventListener('click',()=>{document.body.classList.toggle('light');modeToggle.textContent=document.body.classList.contains('light')?'Dark Mode':'Light Mode';});

const responses={
"hi":"Hello! How can I assist you today?",
"hello":"Hi there! How can I help you?",
"how are you":"I’m just a chatbot, but I’m doing great! How about you?",
"help":"Sure! I can help. Ask me anything.",
"contact":"You can reach us via the contact form on this page.",
"default":"I’m not sure about that, but I’m learning every day!"
};

function getResponse(msg){msg=msg.toLowerCase();for(let key in responses){if(msg.includes(key))return responses[key];}return responses["default"];}

function appendMessage(sender,text){const div=document.createElement('div');div.className=sender;div.textContent=text;chatMessages.appendChild(div);chatMessages.scrollTop=chatMessages.scrollHeight;}

quickReplies.querySelectorAll('button').forEach(btn=>{btn.addEventListener('click',()=>{const text=btn.textContent;appendMessage('user',text);simulateBotResponse(text);});});

sendBtn.addEventListener('click',()=>{const text=userInput.value.trim();if(!text)return;appendMessage('user',text);userInput.value='';simulateBotResponse(text);});
userInput.addEventListener('keypress',(e)=>{if(e.key==='Enter')sendBtn.click();});

function simulateBotResponse(text){const typingDiv=document.createElement('div');typingDiv.className='bot typing';typingDiv.textContent='Nova Assistant is typing...';chatMessages.appendChild(typingDiv);chatMessages.scrollTop=chatMessages.scrollHeight;setTimeout(()=>{typingDiv.remove();appendMessage('bot',getResponse(text));},1200);}
</script>
</body>
</html>
