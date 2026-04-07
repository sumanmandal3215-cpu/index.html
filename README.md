<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUMON'S STRATEGY - AI TRADER</title>
    <style>
        body { background-color: transparent; font-family: sans-serif; display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100vh; margin: 0; color: white; overflow: hidden; }
        
        /* আপনার দেওয়া লোগো স্টাইল */
        .logo-container { width: 120px; height: 120px; margin-bottom: 15px; border-radius: 50%; box-shadow: 0 0 15px rgba(255, 215, 0, 0.5); }
        .logo-img { width: 100%; height: 100%; border-radius: 50%; }

        /* লগইন বক্স ডিজাইন */
        #login-screen { background: rgba(10, 10, 20, 0.95); padding: 25px; border-radius: 20px; text-align: center; width: 280px; border: 2px solid #d4af37; box-shadow: 0 0 25px rgba(212, 175, 55, 0.4); }
        .brand-header { color: #d4af37; font-weight: bold; font-size: 16px; margin-bottom: 5px; text-transform: uppercase; letter-spacing: 1px; }
        .alert-top { color: #ff4757; font-weight: bold; font-size: 10px; margin-top: 5px; text-transform: uppercase; }
        
        input { width: 100%; padding: 10px; margin: 8px 0; border-radius: 5px; border: 1px solid #d4af37; background: #0f0f1e; color: white; box-sizing: border-box; }
        .btn-prime { width: 100%; padding: 12px; border-radius: 8px; border: none; background: #d4af37; color: black; font-weight: bold; cursor: pointer; transition: 0.3s; }
        .btn-prime:hover { background: #f1c40f; }

        /* মেইন ইন্টারফেস (গোল AI) */
        #main-app { display: none; text-align: center; }
        .ai-sphere-container { 
            width: 180px; height: 260px; background: rgba(10, 10, 25, 0.98); 
            border-radius: 30px; padding: 10px; border: 2px solid #d4af37; 
            box-shadow: 0 0 30px rgba(212, 175, 55, 0.5); 
        }
        
        /* বেগুনি থেকে গোল্ডেন এনিমেশন */
        .ai-circle { 
            width: 110px; height: 110px; border-radius: 50%; 
            background: radial-gradient(circle, #f1c40f, #d4af37, #b8860b); 
            margin: 15px auto; position: relative; 
            box-shadow: 0 0 35px #d4af37; 
            animation: pulse-glow 2.5s infinite ease-in-out; 
        }
        
        @keyframes pulse-glow {
            0% { transform: scale(1); box-shadow: 0 0 20px #d4af37; }
            50% { transform: scale(1.08); box-shadow: 0 0 50px #f1c40f; }
            100% { transform: scale(1); box-shadow: 0 0 20px #d4af37; }
        }

        .verified-tag { font-size: 10px; color: #d4af37; margin-bottom: 5px; }
        .market-tag { font-size: 13px; font-weight: bold; color: #f1c40f; margin-top: 5px; }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="logo-container">
            <img src="your_logo.png" alt="Sumon Logo" class="logo-img">
        </div>
        <div class="brand-header">SUMON'S STRATEGY</div>
        <div class="alert-top">ONLY NEW ACCOUNT ACCESS</div>
        <input type="text" id="username" placeholder="ইউজার নাম">
        <input type="number" id="trader-id" placeholder="ট্রেডার আইডি (নাম্বার)">
        <input type="password" id="user-pass" placeholder="পাসওয়ার্ড">
        <button class="btn-prime" onclick="handleLogin()">START JOURNEY</button>
    </div>

    <div id="main-app">
        <div class="ai-sphere-container">
            <div class="verified-tag">By SUMON</div>
            <div class="ai-circle"></div>
            <div class="market-tag" id="display-market">Select Market</div>
            
            <input type="text" id="market-input" placeholder="মার্কেট নাম" style="font-size: 11px; padding: 5px; width: 80%;">
            <button onclick="saveMarket()" style="background: #2ed573; border: none; font-size: 10px; color: black; cursor: pointer;">SAVE</button>
            
            <select id="timer-select" style="font-size: 11px; padding: 5px; width: 90%;">
                <option value="1">1 Minute</option>
                <option value="3">3 Minutes</option>
                <option value="5">5 Minutes</option>
            </select>
            
            <button class="btn-prime" style="margin-top: 5px; font-size: 11px; border-radius: 5px;" onclick="runAI()">START SIGNAL</button>
        </div>
    </div>

    <script>
        function speak(text) {
            window.speechSynthesis.cancel();
            const speech = new SpeechSynthesisUtterance(text);
            speech.lang = 'bn-BD';
            const voices = window.speechSynthesis.getVoices();
            speech.voice = voices.find(v => v.name.includes('Female') || v.name.includes('Google')) || voices[0];
            window.speechSynthesis.speak(speech);
        }

        function handleLogin() {
            const user = document.getElementById('username').value;
            const tid = document.getElementById('trader-id').value;

            if(user && tid) {
                document.getElementById('login-screen').style.display = 'none';
                document.getElementById('main-app').style.display = 'block';
                
                // ভয়েস কমান্ডে আপনার নাম যুক্ত করা হয়েছে
                speak("হ্যালো " + user + "। সুমনস স্ট্র্যাটেজি এআই ট্রেডারে স্বাগতম। আপনার আইডি " + tid + " অ্যাক্টিভেট করা হয়েছে। শুভ ট্রেডিং!");
            } else {
                alert("তথ্য সঠিকভাবে দিন।");
            }
        }

        function saveMarket() {
            const mName = document.getElementById('market-input').value;
            if(mName) {
                document.getElementById('display-market').innerText = mName;
                speak(mName + " মার্কেটটি সুমনস স্ট্র্যাটেজি সিস্টেমে সেভ করা হয়েছে।");
            }
        }

        function runAI() {
            const time = document.getElementById('timer-select').value;
            speak("সুমনস স্ট্র্যাটেজি এআই এখন এনালাইসিস করছে। " + time + " মিনিট অপেক্ষা করুন।");
        }

        window.speechSynthesis.onvoiceschanged = () => {};
    </script>
</body>
</html>
