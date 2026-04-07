<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Trader Pro</title>
    <style>
        /* ডিজাইন ও এনিমেশন */
        body { background-color: transparent; font-family: sans-serif; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; color: white; overflow: hidden; }
        
        /* লগইন বক্স ডিজাইন */
        #login-screen { background: #1a1a2e; padding: 25px; border-radius: 15px; text-align: center; width: 280px; border: 2px solid #4834d4; box-shadow: 0 0 20px rgba(72, 52, 212, 0.5); }
        .alert-top { color: #ff4757; font-weight: bold; font-size: 12px; margin-bottom: 10px; text-transform: uppercase; }
        input, select { width: 100%; padding: 10px; margin: 8px 0; border-radius: 5px; border: 1px solid #4834d4; background: #0f0f1e; color: white; box-sizing: border-box; }
        .btn-prime { width: 100%; padding: 10px; border-radius: 5px; border: none; background: #4834d4; color: white; font-weight: bold; cursor: pointer; }

        /* মেইন এআই ইন্টারফেস (ছবির মতো গোল) */
        #main-app { display: none; text-align: center; }
        .ai-sphere-container { 
            width: 180px; height: 230px; background: rgba(10, 10, 25, 0.9); 
            border-radius: 30px; padding: 10px; border: 1px solid #6c5ce7; 
            box-shadow: 0 0 25px rgba(108, 92, 231, 0.6); 
        }
        
        /* সেই বেগুনি গোল এনিমেশন */
        .ai-circle { 
            width: 110px; height: 110px; border-radius: 50%; 
            background: radial-gradient(circle, #a29bfe, #6c5ce7, #4834d4); 
            margin: 15px auto; position: relative; 
            box-shadow: 0 0 35px #6c5ce7; 
            animation: pulse-glow 2s infinite ease-in-out; 
            display: flex; align-items: center; justify-content: center;
        }
        
        @keyframes pulse-glow {
            0% { transform: scale(1); box-shadow: 0 0 20px #6c5ce7; }
            50% { transform: scale(1.1); box-shadow: 0 0 50px #a29bfe; }
            100% { transform: scale(1); box-shadow: 0 0 20px #6c5ce7; }
        }

        .market-tag { font-size: 14px; font-weight: bold; color: #a29bfe; margin-top: 5px; }
        .save-btn-small { background: #2ed573; border: none; padding: 5px 10px; border-radius: 4px; color: black; font-size: 10px; font-weight: bold; cursor: pointer; }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="alert-top">ONLY NEW ACCOUNT ACCESS</div>
        <input type="text" id="username" placeholder="ইউজার নাম">
        <input type="number" id="trader-id" placeholder="ট্রেডার আইডি (নাম্বার)">
        <input type="password" id="user-pass" placeholder="পাসওয়ার্ড">
        <button class="btn-prime" onclick="handleLogin()">LOGIN & START</button>
    </div>

    <div id="main-app">
        <div class="ai-sphere-container">
            <div class="ai-circle" id="ai-pulse"></div>
            <div class="market-tag" id="display-market">No Market</div>
            
            <input type="text" id="market-input" placeholder="মার্কেট নাম লিখুন" style="font-size: 11px; padding: 5px;">
            <button class="save-btn-small" onclick="saveMarket()">SAVE MARKET</button>
            
            <select id="timer-select" style="font-size: 11px; padding: 5px;">
                <option value="1">1 Minute</option>
                <option value="2">2 Minutes</option>
                <option value="3">3 Minutes</option>
                <option value="5" selected>5 Minutes</option>
            </select>
            
            <button class="btn-prime" style="margin-top: 5px; font-size: 11px;" onclick="runAI()">START SIGNAL</button>
        </div>
    </div>

    <script>
        // ভয়েস ফাংশন (বাংলা এবং মাল্টি-ল্যাঙ্গুয়েজ সাপোর্ট)
        function speak(text) {
            window.speechSynthesis.cancel();
            const speech = new SpeechSynthesisUtterance(text);
            speech.lang = 'bn-BD'; // ডিফল্ট বাংলা সেট করা
            const voices = window.speechSynthesis.getVoices();
            // ফিমেল ভয়েস খুঁজে নেওয়া
            speech.voice = voices.find(v => v.name.includes('Female') || v.name.includes('Google')) || voices[0];
            window.speechSynthesis.speak(speech);
        }

        // লগইন লজিক
        function handleLogin() {
            const user = document.getElementById('username').value;
            const tid = document.getElementById('trader-id').value;
            const pass = document.getElementById('user-pass').value;

            if(user && tid && pass) {
                // ব্রাউজারে ডাটা সেভ রাখা
                localStorage.setItem('trader_name', user);
                localStorage.setItem('trader_id', tid);
                
                document.getElementById('login-screen').style.display = 'none';
                document.getElementById('main-app').style.display = 'block';
                
                speak("স্বাগতম " + user + "। আপনার ট্রেডার আইডি " + tid + " সেভ করা হয়েছে। আমি এখন তৈরি।");
            } else {
                alert("দয়া করে নাম, আইডি এবং পাসওয়ার্ড দিন।");
            }
        }

        // মার্কেট সেভ ও এডিট
        function saveMarket() {
            const mName = document.getElementById('market-input').value;
            if(mName) {
                document.getElementById('display-market').innerText = mName;
                speak(mName + " মার্কেট সেভ করা হয়েছে। আপনি এখন সিগন্যাল শুরু করতে পারেন।");
            }
        }

        // এআই সিগন্যাল রান
        function runAI() {
            const time = document.getElementById('timer-select').value;
            const market = document.getElementById('display-market').innerText;
            speak(market + " মার্কেটে " + time + " মিনিটের এনালাইসিস শুরু করছি। অনুগ্রহ করে অপেক্ষা করুন।");
        }

        // ভয়েস ইঞ্জিন লোড করা
        window.speechSynthesis.onvoiceschanged = () => {};
    </script>
</body>
</html>
