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
        .brand-name { color: #d4af37; font-weight: bold; font-size: 16px; margin-bottom: 5px; text-transform: uppercase; }
        input, select { width: 100%; padding: 10px; margin: 6px 0; border-radius: 8px; border: 1px solid #d4af37; background: #050510; color: white; box-sizing: border-box; font-size: 13px; }
        .main-btn { width: 100%; padding: 12px; border-radius: 8px; border: none; background: linear-gradient(45deg, #d4af37, #f1c40f); color: black; font-weight: bold; cursor: pointer; margin-top: 10px; }
        #main-app { display: none; text-align: center; }
        .ai-sphere { width: 110px; height: 110px; border-radius: 50%; background: radial-gradient(circle, #f1c40f, #d4af37, #000); margin: 15px auto; box-shadow: 0 0 30px #d4af37; animation: glow 2s infinite ease-in-out; cursor: pointer; }
        @keyframes glow { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.1); } }
        .mic-on { border: 3px solid #ff4757 !important; }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="logo-box"><img src="1000008884.jpg" alt="Logo"></div>
        <div class="brand-name">SUMON'S STRATEGY AI</div>
        <input type="text" id="uname" placeholder="ইউজার নাম / Name">
        <input type="number" id="tid" placeholder="ট্রেডার আইডি / Trader ID">
        <select id="lang-select">
            <option value="bn-BD">Bengali (বাংলা)</option>
            <option value="en-US">English (US)</option>
            <option value="hi-IN">Hindi (हिन्दी)</option>
        </select>
        <button class="main-btn" onclick="start()">LOGIN & START</button>
    </div>

    <div id="main-app">
        <div style="background: rgba(15, 15, 25, 0.9); border-radius: 30px; padding: 15px; border: 2px solid #d4af37; width: 220px;">
            <div style="font-size: 11px; color: #d4af37; font-weight: bold;">VERIFIED BY SUMON</div>
            <div class="ai-sphere" id="ai-sphere" onclick="toggleMic()"></div>
            <div id="m-display" style="color:#f1c40f; font-weight:bold; font-size: 14px;">Market Ready</div>
            <p id="mic-status" style="font-size: 10px; color: #aaa;">Tap AI to Speak</p>
            
            <button class="main-btn" onclick="run()">GET SIGNAL</button>
        </div>
    </div>

    <script>
        let selectedLang = 'bn-BD';
        const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        recognition.continuous = false;
        recognition.interimResults = false;

        function speak(text) {
            window.speechSynthesis.cancel();
            const speech = new SpeechSynthesisUtterance(text);
            speech.lang = selectedLang;
            const voices = window.speechSynthesis.getVoices();
            let voiceFound = voices.find(v => v.lang === selectedLang && (v.name.includes('Female') || v.name.includes('Google')));
            speech.voice = voiceFound || voices[0];
            window.speechSynthesis.speak(speech);
        }

        function start() {
            const u = document.getElementById('uname').value;
            selectedLang = document.getElementById('lang-select').value;
            recognition.lang = selectedLang;
            
            if(u) {
                document.getElementById('login-screen').style.display='none';
                document.getElementById('main-app').style.display='block';
                speak(selectedLang === 'bn-BD' ? "স্বাগতম " + u + "। কথা বলতে এআই এর ওপর চাপ দিন।" : "Welcome. Tap AI to speak.");
            }
        }

        function toggleMic() {
            try {
                recognition.start();
                document.getElementById('ai-sphere').classList.add('mic-on');
                document.getElementById('mic-status').innerText = "Listening...";
            } catch (e) { recognition.stop(); }
        }

        recognition.onresult = (event) => {
            const result = event.results[0][0].transcript.toLowerCase();
            document.getElementById('ai-sphere').classList.remove('mic-on');
            document.getElementById('mic-status').innerText = "You said: " + result;

            if(result.includes("market") || result.includes("মার্কেট")) {
                document.getElementById('m-display').innerText = result;
                speak(selectedLang === 'bn-BD' ? "মার্কেট সেট করা হয়েছে।" : "Market set successfully.");
            } else if (result.includes("start") || result.includes("শুরু")) {
                run();
            } else {
                speak(selectedLang === 'bn-BD' ? "আমি দুঃখিত, আবার বলুন।" : "Sorry, please say again.");
            }
        };

        recognition.onend = () => {
            document.getElementById('ai-sphere').classList.remove('mic-on');
            document.getElementById('mic-status').innerText = "Tap AI to Speak";
        };

        function run() {
            speak(selectedLang === 'bn-BD' ? "সুমনস স্ট্র্যাটেজি এআই এখন এনালাইসিস করছে।" : "Analyzing now.");
        }
    </script>
</body>
</html>
