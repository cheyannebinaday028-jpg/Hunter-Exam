# Hunter-Exam
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hunter x Hunter - BMI Portal</title>
    <style>
        :root {
            --primary: #f59e0b; /* Hunter License Gold */
            --primary-hover: #d97706;
            --card-bg: rgba(20, 20, 25, 0.75); /* Dark transparent glass */
            --text: #f3f4f6;
            --text-muted: #9ca3af;
            --border: rgba(255, 255, 255, 0.15);
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            color: var(--text);
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
            background-color: #0c0a09;
        }

        /* Video Background Container */
        .video-background {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: -1;
            pointer-events: none;
            overflow: hidden;
        }

        .video-background iframe {
            width: 100vw;
            height: 56.25vw; /* 16:9 Aspect Ratio */
            min-height: 100vh;
            min-width: 177.77vh; /* 16:9 Aspect Ratio */
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }

        .container {
            width: 100%;
            max-width: 400px;
            padding: 20px;
            box-sizing: border-box;
            z-index: 10;
        }

        .card {
            background: var(--card-bg);
            border-radius: 16px;
            padding: 32px;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.5);
            border: 1px solid var(--border);
            backdrop-filter: blur(12px); /* High-tech anime HUD glass effect */
            -webkit-backdrop-filter: blur(12px);
        }

        h2 {
            margin-top: 0;
            margin-bottom: 8px;
            font-size: 24px;
            font-weight: 700;
            text-align: center;
            color: #ffffff;
            text-shadow: 0 2px 4px rgba(0,0,0,0.5);
        }

        .subtitle {
            color: var(--text-muted);
            text-align: center;
            font-size: 14px;
            margin-bottom: 24px;
        }

        .form-group {
            margin-bottom: 16px;
        }

        label {
            display: block;
            font-size: 14px;
            font-weight: 500;
            margin-bottom: 6px;
            color: #e5e7eb;
        }

        input {
            width: 100%;
            padding: 10px 12px;
            border: 1px solid var(--border);
            border-radius: 6px;
            font-size: 15px;
            box-sizing: border-box;
            transition: all 0.2s;
            background: rgba(0, 0, 0, 0.4);
            color: #fff;
        }

        input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 2px rgba(245, 158, 11, 0.3);
        }

        button {
            width: 100%;
            padding: 12px;
            background-color: var(--primary);
            color: #000;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            font-weight: 700;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        button:hover {
            background-color: var(--primary-hover);
        }

        .hidden {
            display: none !important;
        }

        .error {
            color: #f87171;
            font-size: 14px;
            text-align: center;
            margin-bottom: 16px;
        }

        .result-box {
            margin-top: 20px;
            padding: 16px;
            background-color: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            text-align: center;
            border: 1px solid rgba(255,255,255,0.05);
        }

        .bmi-val {
            font-size: 32px;
            font-weight: 800;
            color: var(--primary);
            margin: 8px 0;
        }

        .logout-btn {
            background-color: #ef4444;
            color: white;
            margin-top: 16px;
        }

        .logout-btn:hover {
            background-color: #dc2626;
        }
    </style>
</head>
<body>

<!-- Looping Hunter x Hunter Live Wallpaper -->
<div class="video-background">
    <iframe src="https://youtube.com" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

<div class="container">
    <!-- Login Screen -->
    <div id="login-screen" class="card">
        <h2>Hunter Exam Portal</h2>
        <div class="subtitle">Enter any username and password to log in</div>
        <div id="login-error" class="error hidden">Please enter both a username and password.</div>
        <form id="login-form" onsubmit="handleLogin(event)">
            <div class="form-group">
                <label for="username">Username</label>
                <input type="text" id="username" placeholder="e.g., GonFreecss">
            </div>
            <div class="form-group">
                <label for="password">Password</label>
                <input type="password" id="password" placeholder="••••••••">
            </div>
            <button type="submit">Log In</button>
        </form>
    </div>

    <!-- Dashboard Screen (BMI Calculator) -->
    <div id="dashboard-screen" class="card hidden">
        <h2>Nen BMI Calculator</h2>
        <div class="subtitle">Logged in as: <strong id="user-display" style="color: var(--primary);"></strong></div>
        
        <form id="bmi-form" onsubmit="calculateBMI(event)">
            <div class="form-group">
                <label for="weight">Weight (kg)</label>
                <input type="number" id="weight" step="0.1" placeholder="e.g., 65" required>
            </div>
            <div class="form-group">
                <label for="height">Height (cm)</label>
                <input type="number" id="height" step="1" placeholder="e.g., 168" required>
            </div>
            <button type="submit">Calculate BMI</button>
        </form>

        <div id="result" class="result-box hidden">
            <div>Your Status is</div>
            <div id="bmi-value" class="bmi-val">--</div>
            <div id="bmi-category" style="font-weight: 600;"></div>
        </div>

        <button class="logout-btn" onclick="handleLogout()">Log Out</button>
    </div>
</div>

<script>
    function handleLogin(event) {
        event.preventDefault();
        const user = document.getElementById('username').value.trim();
        const pass = document.getElementById('password').value.trim();
        const errorDiv = document.getElementById('login-error');

        if (!user || !pass) {
            errorDiv.classList.remove('hidden');
            return;
        }

        errorDiv.classList.add('hidden');
        document.getElementById('user-display').textContent = user;
        document.getElementById('login-screen').classList.add('hidden');
        document.getElementById('dashboard-screen').classList.remove('hidden');
    }

    function calculateBMI(event) {
        event.preventDefault();
        const weight = parseFloat(document.getElementById('weight').value);
        const heightCm = parseFloat(document.getElementById('height').value);
        
        if (!weight || !heightCm) return;

        const heightM = heightCm / 100;
        const bmi = weight / (heightM * heightM);
        
        let category = '';
        if (bmi < 18.5) category = 'Underweight ⚡';
        else if (bmi < 25) category = 'Optimal Condition ✨';
        else if (bmi < 30) category = 'Overweight 💥';
        else category = 'Heavy Build 🔥';

        document.getElementById('bmi-value').textContent = bmi.toFixed(1);
        document.getElementById('bmi-category').textContent = category;
        document.getElementById('result').classList.remove('hidden');
    }

    function handleLogout() {
        document.getElementById('login-form').reset();
        document.getElementById('bmi-form').reset();
        document.getElementById('result').classList.add('hidden');
        
        document.getElementById('dashboard-screen').classList.add('hidden');
        document.getElementById('login-screen').classList.remove('hidden');
    }
</script>
</body>
</html>
