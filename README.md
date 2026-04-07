<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUMON'S STRATEGY AI</title>
    <style>
        body { background-color: #000; font-family: sans-serif; display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100vh; margin: 0; color: white; overflow: hidden; }
        .logo-box { width: 90px; height: 90px; margin-bottom: 10px; border: 2px solid #d4af37; border-radius: 50%; padding: 3px; box-shadow: 0 0 15px #d4af37; }
        .logo-box img { width: 100%; height: 100%; border-radius: 50%; object-fit: cover; }
        #login-screen { background: rgba(20, 20, 30, 0.95); padding: 20px; border-radius: 20px; text-align: center; width: 280px; border: 1.5px solid #d4af37; box-shadow: 0 0 30px rgba(212, 175, 55, 0.3); }
        input, select { width: 100%; padding: 10px; margin: 6px 0; border-radius: 8px; border: 1px solid #d4af37; background: #050510; color: white; box-sizing: border-box; }
        .main-btn { width: 100%; padding: 12px; border-radius: 8px; border: none; background: linear-gradient(45deg, #d4af37, #f1c40f); color: black; font-weight: bold; cursor: pointer; }
        #main-app { display: none; text-align: center; }
        .ai-sphere { width: 120px; height: 120px; border-radius: 50%; background: radial-gradient(circle, #f1c40f, #d4af37, #000); margin: 15px auto; box-shadow: 0 0 30px #d4af37; animation: glow 2s infinite ease-in-out; cursor: pointer; border: 2px solid transparent; }
        @keyframes glow { 0%, 100% { transform: scale(1); opacity: 0.8; } 50% { transform: scale(1.1); opacity: 1; } }
        .listening { border: 3px solid #ff4757 !important; box-shadow: 0 0 40px #ff4757 !important; }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="logo-box"><img src="1000008884.jpg" alt="Logo"></div>
        <div style="color:#d4af37; font-weight:bold; margin-bottom:10px;">SUMON'S STRATEGY</div>
        <input type="text" id="uname" placeholder="আপনার নাম">
        <select id="lang-select">
            <option value="bn-BD">Bengali (বাংলা)</option>
            <option value="en-US">English (English)</option>
            <option value="hi-IN">Hindi (हिन्दी)</option>
        </select>
        <button class="main-btn" onclick="start()">LOGIN</button>
    </div>

    <div id="main-app">
        <div style="background: rgba(15, 15, 25, 0.9); border-radius: 30px; padding: 15px; border: 2px solid #d4af37;">
            <div style="font-size: 11px; color: #d4af37;">VERIFIED BY SUMON</div>
            <div class="ai-sphere" id="ai-sphere" onclick="startVoice()"></div>
            <div id="m-display" style="color:#f1c40f; font-weight:bold; margin-bottom:5px;">Market Ready</div>
            <div id="status-text" style="font-size:10px; color:#aaa;">Tap AI to Speak</div>
            <button class="main-btn" style="margin-top:10px;" onclick="runAI()">GET SIGNAL</button>
        </div>
    </div>

    <script>
        let selectedLang = 'bn-BD';
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        const recognition = new SpeechRecognition();
        recognition.continuous = false;
        recognition.interimResults = false;

        function speak(text) {
            window.speechSynthesis.cancel();
            const s = new SpeechSynthesisUtterance(text);
            s.lang = selectedLang;
            const voices = window.speechSynthesis.getVoices();
            s.voice = voices.find(v => v.lang === selectedLang && (v.name.includes('Female') || v.name.includes('Google'))) || voices[0];
            window.speechSynthesis.speak(s);
        }

        function start() {
            const u = document.getElementById('uname').value;
            selectedLang = document.getElementById('lang-select').value;
            recognition.lang = selectedLang;
            if(u) {
                document.getElementById('login-screen').style.display='none';
                document.getElementById('main-app').style.display='block';
                let welcome = selectedLang === 'bn-BD' ? "স্বাগতম " + u + "। সুমনস স্ট্র্যাটেজি এআই তৈরি।" : "Welcome " + u + ". AI is ready.";
                speak(welcome);
            }
        }

        function startVoice() {
            recognition.start();
            document.getElementById('ai-sphere').classList.add('listening');
            document.getElementById('status-text').innerText = "Listening...";
        }

        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript.toLowerCase();
            document.getElementById('ai-sphere').classList.remove('listening');
            document.getElementById('m-display').innerText = transcript;
            
            let reply = selectedLang === 'bn-BD' ? transcript + " মার্কেট সেট করা হয়েছে।" : "Market set to " + transcript;
            speak(reply);
        };

        recognition.onend = () => {
            document.getElementById('ai-sphere').classList.remove('listening');
            document.getElementById('status-text').innerText = "Tap AI to Speak";
        };

        function runAI() {
            let msg = selectedLang === 'bn-BD' ? "সুমনস স্ট্র্যাটেজি এআই এখন এনালাইসিস করছে।" : "Sumon's Strategy AI is analyzing.";
            speak(msg);
        }
    </script>
</body>
</html>
