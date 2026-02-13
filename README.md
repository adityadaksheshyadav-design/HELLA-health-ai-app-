<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HealthAI - Your Personal Health Assistant</title>
    <style>
        :root {
            --primary: #2563eb;
            --secondary: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --bg: #f8fafc;
            --card: #ffffff;
            --text: #1e293b;
            --text-light: #64748b;
        }

        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg);
            color: var(--text);
            line-height: 1.6;
            padding-bottom: 50px;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            text-align: center;
            padding: 20px 0;
            background: white;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            margin-bottom: 20px;
            border-radius: 0 0 20px 20px;
        }

        .logo {
            font-size: 24px;
            font-weight: bold;
            color: var(--primary);
        }

        .card {
            background: var(--card);
            border-radius: 16px;
            padding: 24px;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }

        h2 {
            margin-bottom: 16px;
            font-size: 1.25rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .tabs {
            display: flex;
            background: #e2e8f0;
            padding: 4px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .tab {
            flex: 1;
            text-align: center;
            padding: 10px;
            cursor: pointer;
            border-radius: 8px;
            font-weight: 600;
            transition: 0.3s;
            font-size: 14px;
        }

        .tab.active {
            background: white;
            color: var(--primary);
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        textarea {
            width: 100%;
            height: 120px;
            padding: 12px;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            resize: none;
            margin-bottom: 15px;
            font-size: 16px;
        }

        .btn {
            width: 100%;
            padding: 14px;
            border: none;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.2s;
            font-size: 16px;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            opacity: 0.9;
        }

        .file-upload-zone {
            border: 2px dashed #cbd5e1;
            padding: 30px;
            text-align: center;
            border-radius: 12px;
            cursor: pointer;
            margin-bottom: 15px;
            transition: 0.3s;
        }

        .file-upload-zone:hover {
            border-color: var(--primary);
            background: #eff6ff;
        }

        #result-area {
            display: none;
            margin-top: 20px;
            border-left: 4px solid var(--primary);
            padding-left: 15px;
        }

        .status-badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .urgent { background: #fee2e2; color: var(--danger); }
        .moderate { background: #fef3c7; color: var(--warning); }
        .healthy { background: #dcfce7; color: var(--secondary); }

        .pricing-card {
            text-align: center;
            border: 2px solid var(--primary);
        }

        .price {
            font-size: 32px;
            font-weight: 800;
            margin: 10px 0;
        }

        .footer-note {
            font-size: 11px;
            color: var(--text-light);
            text-align: center;
            padding: 20px;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 20px;
        }

        .spinner {
            width: 30px;
            height: 30px;
            border: 3px solid #f3f3f3;
            border-top: 3px solid var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .hidden { display: none; }
    </style>
</head>
<body>

    <header>
        <div class="logo">🏥 HealthAI</div>
        <p style="font-size: 12px; color: var(--text-light);">Your Smart Medical Companion</p>
    </header>

    <div class="container">
        <!-- Tab Navigation -->
        <div class="tabs">
            <div class="tab active" onclick="switchTab('symptoms')">Symptoms</div>
            <div class="tab" onclick="switchTab('reports')">Reports</div>
            <div class="tab" onclick="switchTab('premium')">Premium</div>
        </div>

        <!-- Symptoms Content -->
        <div id="tab-symptoms">
            <div class="card">
                <h2>🔍 Symptom Checker</h2>
                <p style="font-size: 14px; margin-bottom: 10px; color: var(--text-light);">
                    Describe how you feel (e.g., "I have a sharp headache and fever since yesterday").
                </p>
                <textarea id="symptom-input" placeholder="Type symptoms here..."></textarea>
                <button class="btn btn-primary" onclick="analyzeHealth('symptoms')">Analyze Symptoms</button>
            </div>
        </div>

        <!-- Reports Content -->
        <div id="tab-reports" class="hidden">
            <div class="card">
                <h2>📄 Report Analysis</h2>
                <div class="file-upload-zone" onclick="document.getElementById('file-input').click()">
                    <div style="font-size: 40px;">📁</div>
                    <p>Click to upload Blood Test, MRI, or X-Ray</p>
                    <p style="font-size: 12px; color: var(--text-light);">(PDF, JPEG, PNG supported)</p>
                    <input type="file" id="file-input" class="hidden" onchange="mockFileUpload()">
                </div>
                <div id="file-info" class="hidden" style="margin-bottom: 15px; font-size: 14px; color: var(--secondary);">
                    ✅ File ready: <span id="filename"></span>
                </div>
                <button class="btn btn-primary" onclick="analyzeHealth('reports')">Analyze Report</button>
            </div>
        </div>

        <!-- Premium Content -->
        <div id="tab-premium" class="hidden">
            <div class="card pricing-card">
                <div style="font-size: 40px;">⭐</div>
                <h2>Premium Care</h2>
                <p>Unlock detailed medical summaries and 24/7 AI chat.</p>
                <div class="price">\$5<span style="font-size: 16px; font-weight: normal;">/mo</span></div>
                <ul style="list-style: none; text-align: left; margin: 20px 0; font-size: 14px;">
                    <li>✅ Unlimited Report Analysis</li>
                    <li>✅ Doctor-Certified Remedy Suggestions</li>
                    <li>✅ Health Trend Tracking</li>
                    <li>✅ Priority AI Processing</li>
                </ul>
                <button class="btn btn-primary" onclick="alert('Subscription feature would connect to payment gateway.')">Upgrade Now</button>
            </div>
        </div>

        <!-- Common Result Area -->
        <div id="loading-state" class="loading">
            <div class="spinner"></div>
            <p id="loading-text">AI is analyzing your data...</p>
        </div>

        <div id="result-card" class="card hidden">
            <div id="result-status"></div>
            <h3 id="result-title">Analysis Result</h3>
            <p id="result-desc" style="margin: 10px 0; font-size: 15px;"></p>
            
            <div style="margin-top: 15px; border-top: 1px solid #eee; padding-top: 15px;">
                <strong style="font-size: 14px;">💡 AI Recommendations:</strong>
                <p id="result-remedy" style="font-size: 14px; color: var(--text-light); margin-top: 5px;"></p>
            </div>

            <div id="clinical-help" style="margin-top: 15px; padding: 10px; border-radius: 8px; font-weight: bold; text-align: center;">
            </div>
        </div>

        <div class="footer-note">
            ⚠️ <strong>Disclaimer:</strong> This AI assistant is for informational purposes only. It is not a substitute for professional medical advice, diagnosis, or treatment. Always seek the advice of your physician or other qualified health provider.
        </div>
    </div>

    <script>
        function switchTab(tabName) {
            // Hide all tabs
            document.getElementById('tab-symptoms').classList.add('hidden');
            document.getElementById('tab-reports').classList.add('hidden');
            document.getElementById('tab-premium').classList.add('hidden');
            
            // Remove active class from all buttons
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            
            // Show selected tab
            document.getElementById('tab-' + tabName).classList.remove('hidden');
            
            // Add active class to clicked tab
            event.currentTarget.classList.add('active');
            
            // Hide result when switching
            document.getElementById('result-card').classList.add('hidden');
        }

        function mockFileUpload() {
            const input = document.getElementById('file-input');
            const info = document.getElementById('file-info');
            const name = document.getElementById('filename');
            if(input.files.length > 0) {
                name.innerText = input.files[0].name;
                info.classList.remove('hidden');
            }
        }

        function analyzeHealth(type) {
            const resultCard = document.getElementById('result-card');
            const loading = document.getElementById('loading-state');
            const symptomText = document.getElementById('symptom-input').value.toLowerCase();
            
            if(type === 'symptoms' && symptomText.length < 5) {
                alert("Please describe your symptoms in more detail.");
                return;
            }

            // Show loading
            resultCard.classList.add('hidden');
            loading.style.display = 'block';
            
            setTimeout(() => {
                loading.style.display = 'none';
                resultCard.classList.remove('hidden');
                
                let title, status, desc, remedy, clinical, statusClass;

                // Simple Logic Engine
                if (type === 'reports') {
                    title = "Blood Report Analysis Summary";
                    status = "STABLE";
                    statusClass = "healthy";
                    desc = "AI has parsed your uploaded document. Hemoglobin and Glucose levels appear within normal biological reference ranges. Minor vitamin D deficiency noted.";
                    remedy = "Consider increased sunlight exposure and Vitamin D rich foods like eggs and fatty fish. Consult a doctor for supplementation dosage.";
                    clinical = "✅ No urgent clinical help needed.";
                } else {
                    // Symptom logic
                    if (symptomText.includes('chest') || symptomText.includes('breath') || symptomText.includes('heart')) {
                        title = "Cardiovascular Alert";
                        status = "URGENT";
                        statusClass = "urgent";
                        desc = "Your symptoms may indicate a serious cardiovascular or respiratory event.";
                        remedy = "Do not take any home remedies. Sit down, stay calm, and seek medical attention.";
                        clinical = "🚨 SEEK IMMEDIATE CLINICAL HELP (ER)";
                    } else if (symptomText.includes('fever') || symptomText.includes('cough') || symptomText.includes('headache')) {
                        title = "Viral/Infection Assessment";
                        status = "MODERATE";
                        statusClass = "moderate";
                        desc = "Symptoms suggest a common viral infection or flu-like syndrome.";
                        remedy = "Rest, hydration (water and electrolytes), and Paracetamol (Acetaminophen) for fever (if not allergic). Monitor temperature hourly.";
                        clinical = "👨‍⚕️ Schedule a clinical visit if fever persists > 48hrs.";
                    } else {
                        title = "General Wellness Check";
                        status = "MILD";
                        statusClass = "healthy";
                        desc = "Symptoms seem mild or non-specific. This could be related to stress, fatigue, or minor irritation.";
                        remedy = "Improve sleep hygiene and stay hydrated. Herbal teas like Ginger or Chamomile may help soothe minor discomfort.";
                        clinical = "💤 Rest and monitor symptoms for 24 hours.";
                    }
                }

                // Update UI
                document.getElementById('result-status').innerHTML = `<span class="status-badge ${statusClass}">${status}</span>`;
                document.getElementById('result-title').innerText = title;
                document.getElementById('result-desc').innerText = desc;
                document.getElementById('result-remedy').innerText = remedy;
                
                const clinicalDiv = document.getElementById('clinical-help');
                clinicalDiv.innerText = clinical;
                clinicalDiv.style.backgroundColor = statusClass === 'urgent' ? '#fee2e2' : (statusClass === 'moderate' ? '#fef3c7' : '#dcfce7');
                clinicalDiv.style.color = statusClass === 'urgent' ? 'var(--danger)' : (statusClass === 'moderate' ? 'var(--warning)' : 'var(--secondary)');

            }, 1500);
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cylestria - AI Health Companion</title>
    <style>
        :root {
            --bg-deep: #0a0118;
            --bg-card: #161025;
            --primary: #9333ea;
            --primary-light: #c084fc;
            --accent: #2dd4bf;
            --text-main: #ffffff;
            --text-muted: #94a3b8;
            --danger: #ef4444;
            --success: #10b981;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
        }

        body {
            background-color: var(--bg-deep);
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
        }

        /* Header / Logo Section */
        header {
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(10, 1, 24, 0.8);
            backdrop-filter: blur(10px);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .logo-container {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo-icon {
            width: 32px;
            height: 32px;
            background: var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
        }

        .logo-text {
            font-weight: 700;
            font-size: 20px;
            letter-spacing: 0.5px;
        }

        .btn-get-started {
            background: var(--primary);
            color: white;
            padding: 6px 16px;
            border-radius: 20px;
            text-decoration: none;
            font-size: 14px;
            font-weight: 600;
            border: none;
            cursor: pointer;
        }

        /* Main Container */
        .container {
            padding: 20px;
            max-width: 500px;
            margin: 0 auto;
            flex-grow: 1;
            width: 100%;
        }

        /* Hero Section */
        .hero {
            text-align: left;
            padding: 20px 0 40px 0;
        }

        .hero h1 {
            font-size: 42px;
            line-height: 1.1;
            margin-bottom: 20px;
            background: linear-gradient(to right, #ffffff, var(--primary-light), var(--accent));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero span.ai-text {
            display: block;
            color: var(--primary-light);
            -webkit-text-fill-color: initial;
        }

        .version-badge {
            display: inline-flex;
            align-items: center;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            color: var(--text-muted);
            margin-bottom: 25px;
        }

        .version-badge::before {
            content: "";
            display: inline-block;
            width: 8px;
            height: 8px;
            background: var(--success);
            border-radius: 50%;
            margin-right: 8px;
        }

        .hero-p {
            color: var(--text-muted);
            font-size: 16px;
            line-height: 1.5;
            margin-bottom: 30px;
        }

        /* Interaction Cards */
        .card {
            background: var(--bg-card);
            border-radius: 24px;
            padding: 24px;
            margin-bottom: 20px;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }

        .card h2 {
            font-size: 18px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        textarea {
            width: 100%;
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 12px;
            padding: 15px;
            color: white;
            font-size: 15px;
            resize: none;
            height: 100px;
            margin-bottom: 15px;
        }

        .upload-area {
            border: 2px dashed rgba(255, 255, 255, 0.1);
            border-radius: 16px;
            padding: 30px;
            text-align: center;
            cursor: pointer;
            transition: 0.3s;
            margin-bottom: 15px;
        }

        .upload-area:hover {
            border-color: var(--primary);
            background: rgba(147, 51, 234, 0.05);
        }

        .action-btn {
            width: 100%;
            background: var(--primary);
            color: white;
            border: none;
            padding: 14px;
            border-radius: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .action-btn:active {
            transform: scale(0.98);
        }

        /* Results UI */
        #result-display {
            display: none;
            border-left: 3px solid var(--primary);
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .status-pill {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 800;
            text-transform: uppercase;
            margin-bottom: 10px;
        }

        .status-urgent { background: rgba(239, 68, 68, 0.2); color: #fca5a5; }
        .status-ok { background: rgba(16, 185, 129, 0.2); color: #6ee7b7; }

        /* Navigation Bar */
        .bottom-nav {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: calc(100% - 40px);
            max-width: 400px;
            background: rgba(22, 16, 37, 0.9);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            display: flex;
            justify-content: space-around;
            padding: 12px;
            z-index: 1000;
        }

        .nav-item {
            color: var(--text-muted);
            text-decoration: none;
            font-size: 20px;
            padding: 8px;
            border-radius: 12px;
            transition: 0.3s;
        }

        .nav-item.active {
            color: white;
            background: var(--primary);
        }

        /* Premium Plan */
        .premium-card {
            background: linear-gradient(135deg, #161025 0%, #2e1065 100%);
            border: 1px solid var(--primary);
            text-align: center;
        }

        .price-tag {
            font-size: 32px;
            font-weight: 800;
            margin: 10px 0;
        }

        .chat-bubble {
            position: fixed;
            bottom: 100px;
            right: 25px;
            width: 55px;
            height: 55px;
            background: var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 20px rgba(147, 51, 234, 0.4);
            cursor: pointer;
            z-index: 999;
        }

        .hidden { display: none; }

        .loader {
            width: 20px;
            height: 20px;
            border: 2px solid #fff;
            border-bottom-color: transparent;
            border-radius: 50%;
            display: inline-block;
            animation: rotation 1s linear infinite;
        }

        @keyframes rotation {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>

    <header>
        <div class="logo-container">
            <div class="logo-icon">🫀</div>
            <div class="logo-text">Cylestria</div>
        </div>
        <button class="btn-get-started">Get Started</button>
    </header>

    <div class="container">
        <!-- Main Interface -->
        <div id="main-view">
            <div class="hero">
                <div class="version-badge">AI-Powered Health Analysis v2.0</div>
                <h1>Your Personal <br><span class="ai-text">AI Health</span> Companion.</h1>
                <p class="hero-p">Create your profile, describe your symptoms, and let Cylestria generate a comprehensive health report. We provide instant precautions, prescriptions, and direct medication guidance.</p>
            </div>

            <div class="card">
                <h2>📝 Analyze Symptoms</h2>
                <textarea id="symptom-input" placeholder="e.g. I have been feeling a dull pain in my lower back for 2 days..."></textarea>
                <button class="action-btn" id="analyze-btn" onclick="runAnalysis('symptoms')">Generate Report</button>
            </div>

            <div class="card">
                <h2>📁 Upload Health Report</h2>
                <div class="upload-area" onclick="document.getElementById('file-up').click()">
                    <div style="font-size: 30px; margin-bottom: 10px;">📄</div>
                    <p style="font-size: 14px;">Drop your lab reports here</p>
                    <p style="font-size: 12px; color: var(--text-muted);">Supports PDF, JPG, PNG</p>
                    <input type="file" id="file-up" class="hidden" onchange="fileSelected()">
                </div>
                <div id="file-name" style="font-size: 12px; color: var(--accent); margin-bottom: 10px; text-align: center;"></div>
                <button class="action-btn" style="background: transparent; border: 1px solid var(--primary);" onclick="runAnalysis('reports')">Analyze Document</button>
            </div>

            <div id="result-display" class="card">
                <div id="res-badge"></div>
                <h3 id="res-title" style="margin-bottom: 10px;">Report Summary</h3>
                <p id="res-body" style="font-size: 14px; color: var(--text-muted); margin-bottom: 20px; line-height: 1.6;"></p>
                
                <div style="background: rgba(255,255,255,0.05); padding: 15px; border-radius: 12px;">
                    <strong style="font-size: 13px; color: var(--accent);">💊 Suggested Action & Medication:</strong>
                    <p id="res-meds" style="font-size: 14px; margin-top: 8px;"></p>
                </div>
                
                <div id="res-clinical" style="margin-top: 20px; font-weight: bold; text-align: center; font-size: 14px;"></div>
            </div>

            <!-- Premium Plan -->
            <div class="card premium-card">
                <div style="color: var(--primary-light); font-size: 12px; font-weight: bold; margin-bottom: 5px;">CYLESTRIA PRO</div>
                <h2>Premium Care Plan</h2>
                <div class="price-tag">\$5<span style="font-size: 14px; color: var(--text-muted); font-weight: normal;">/month</span></div>
                <ul style="list-style: none; font-size: 13px; color: var(--text-muted); margin: 15px 0; text-align: left; padding-left: 20px;">
                    <li>✨ Unlimited Clinical Analysis</li>
                    <li>👩‍⚕️ Doctor-Certified Prescriptions</li>
                    <li>🧪 Deep Lab Report Breakdown</li>
                    <li>💬 24/7 Priority AI Chat</li>
                </ul>
                <button class="action-btn" onclick="alert('Proceeding to Cylestria Secure Checkout...')">Upgrade to Pro</button>
            </div>
            
            <div style="height: 100px;"></div> <!-- Spacer for nav -->
        </div>
    </div>

    <!-- Chat Bubble -->
    <div class="chat-bubble">
        <span style="font-size: 24px;">💬</span>
    </div>

    <!-- Bottom Navigation -->
    <nav class="bottom-nav">
        <a href="#" class="nav-item active">⬜</a>
        <a href="#" class="nav-item">🖥️</a>
        <a href="#" class="nav-item">🕸️</a>
        <a href="#" class="nav-item">🌐</a>
        <a href="#" class="nav-item">👤</a>
    </nav>

    <script>
        function fileSelected() {
            const file = document.getElementById('file-up').files[0];
            if(file) {
                document.getElementById('file-name').innerText = "Selected: " + file.name;
            }
        }

        function runAnalysis(mode) {
            const btn = document.getElementById('analyze-btn');
            const resultDiv = document.getElementById('result-display');
            const symptoms = document.getElementById('symptom-input').value;

            if (mode === 'symptoms' && symptoms.length < 10) {
                alert("Please describe your symptoms more thoroughly for an accurate analysis.");
                return;
            }

            // UI feedback
            const originalText = btn.innerHTML;
            btn.innerHTML = '<span class="loader"></span> Analyzing...';
            btn.disabled = true;

            setTimeout(() => {
                btn.innerHTML = originalText;
                btn.disabled = false;
                resultDiv.style.display = 'block';
                resultDiv.scrollIntoView({ behavior: 'smooth' });

                let data = {
                    title: "General Assessment",
                    badge: "STABLE",
                    badgeClass: "status-ok",
                    body: "Based on your input, Cylestria AI suggests this is likely a standard physiological response to environment or minor strain.",
                    meds: "Rest, hydration, and Vitamin C. Over-the-counter pain relief like Ibuprofen if needed.",
                    clinical: "✅ No immediate clinical help required."
                };

                const input = symptoms.toLowerCase();
                if (input.includes('chest') || input.includes('breath') || input.includes('heart')) {
                    data = {
                        title: "Urgent Medical Alert",
                        badge: "CRITICAL",
                        badgeClass: "status-urgent",
                        body: "Your described symptoms match patterns for cardiovascular or respiratory distress. This requires professional evaluation.",
                        meds: "Do not self-medicate. Stop all physical activity immediately.",
                        clinical: "🚨 EMERGENCY: PLEASE VISIT A CLINIC IMMEDIATELY."
                    };
                } else if (input.includes('fever') || input.includes('throat') || input.includes('cough')) {
                    data = {
                        title: "Infection Analysis",
                        badge: "MODERATE",
                        badgeClass: "status-urgent",
                        body: "Symptoms indicate a potential upper respiratory infection or viral flu.",
                        meds: "Paracetamol 500mg, warm salt water gargles, and increased fluid intake.",
                        clinical: "👨‍⚕️ Clinical help recommended if fever exceeds 102°F."
                    };
                }

                if(mode === 'reports') {
                    data.title = "Lab Report Analysis";
                    data.body = "Cylestria AI has parsed your document. Most biomarkers are within the standard reference range. We noticed slightly elevated cortisol levels.";
                    data.meds = "Magnesium supplements and improved sleep hygiene (8 hours/night).";
                    data.clinical = "✅ Clinical follow-up not urgent.";
                }

                document.getElementById('res-badge').innerHTML = `<span class="status-pill ${data.badgeClass}">${data.badge}</span>`;
                document.getElementById('res-title').innerText = data.title;
                document.getElementById('res-body').innerText = data.body;
                document.getElementById('res-meds').innerText = data.meds;
                document.getElementById('res-clinical').innerText = data.clinical;
                document.getElementById('res-clinical').style.color = data.badgeClass === 'status-urgent' ? 'var(--danger)' : 'var(--success)';

            }, 2000);
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cylestria AI - Personal Health Companion</title>
    <style>
        /* 
           Simulating "Soria" font style using high-quality system serifs 
           since external network calls for fonts are restricted.
        */
        :root {
            --bg-color: #0d0221;
            --card-bg: #1a1230;
            --primary-purple: #9333ea;
            --accent-teal: #2dd4bf;
            --text-white: #ffffff;
            --text-dim: #a0aec0;
            --gradient-text: linear-gradient(90deg, #7c3aed, #2dd4bf);
            --font-soria-proxy: "Soria", "Georgia", "Palatino", "Book Antiqua", serif;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-white);
            font-family: var(--font-soria-proxy);
            line-height: 1.5;
            overflow-x: hidden;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }

        /* Header Navigation */
        header {
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(13, 2, 33, 0.9);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .logo-box {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo-circle {
            width: 35px;
            height: 35px;
            background: var(--primary-purple);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
        }

        .logo-name {
            font-size: 22px;
            font-weight: bold;
            letter-spacing: 0.5px;
        }

        .btn-get-started {
            background: var(--primary-purple);
            color: white;
            border: none;
            padding: 8px 18px;
            border-radius: 20px;
            font-weight: 600;
            font-family: sans-serif;
            font-size: 13px;
            cursor: pointer;
        }

        /* Hero Content */
        .container {
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
            flex: 1;
        }

        .hero-title-main {
            font-size: 60px;
            font-weight: 900;
            text-transform: uppercase;
            margin-top: 20px;
            background: var(--gradient-text);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: -2px;
        }

        .version-tag {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 6px 15px;
            border-radius: 20px;
            font-size: 12px;
            font-family: sans-serif;
            margin: 20px 0;
            color: var(--text-dim);
        }

        .dot {
            width: 8px;
            height: 8px;
            background: #22c55e;
            border-radius: 50%;
            box-shadow: 0 0 8px #22c55e;
        }

        .headline {
            font-size: 38px;
            line-height: 1.1;
            margin-bottom: 20px;
        }

        .headline span {
            color: var(--primary-purple);
        }

        .hero-desc {
            color: var(--text-dim);
            font-size: 16px;
            margin-bottom: 30px;
            font-family: sans-serif;
        }

        /* Application Logic Cards */
        .card {
            background: var(--card-bg);
            border-radius: 24px;
            padding: 25px;
            margin-bottom: 20px;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }

        .card h3 {
            font-size: 18px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        textarea {
            width: 100%;
            height: 120px;
            background: #0d0221;
            border: 1px solid #332a4d;
            border-radius: 12px;
            padding: 15px;
            color: white;
            font-family: sans-serif;
            font-size: 14px;
            resize: none;
            margin-bottom: 15px;
        }

        .upload-zone {
            border: 2px dashed #332a4d;
            border-radius: 15px;
            padding: 30px;
            text-align: center;
            cursor: pointer;
            margin-bottom: 15px;
            transition: 0.3s;
        }

        .upload-zone:hover {
            border-color: var(--accent-teal);
            background: rgba(45, 212, 191, 0.05);
        }

        .main-btn {
            width: 100%;
            background: var(--primary-purple);
            color: white;
            border: none;
            padding: 15px;
            border-radius: 12px;
            font-weight: 800;
            cursor: pointer;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-family: sans-serif;
        }

        /* Results UI */
        #analysis-result {
            display: none;
            border-top: 4px solid var(--accent-teal);
            animation: slideUp 0.4s ease-out;
        }

        @keyframes slideUp {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        .status-pill {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 10px;
            font-size: 11px;
            font-weight: bold;
            margin-bottom: 10px;
            text-transform: uppercase;
            font-family: sans-serif;
        }

        .pill-warn { background: #fee2e2; color: #991b1b; }
        .pill-safe { background: #dcfce7; color: #166534; }

        /* Subscription Card */
        .premium-card {
            background: linear-gradient(135deg, #1a1230 0%, #3b0764 100%);
            border: 1px solid var(--primary-purple);
            text-align: center;
        }

        .price {
            font-size: 40px;
            font-weight: 900;
            margin: 15px 0;
        }

        .price span {
            font-size: 16px;
            font-weight: normal;
            color: var(--text-dim);
        }

        /* Bottom Navbar */
        .bottom-nav {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 400px;
            background: rgba(26, 18, 48, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 12px;
            display: flex;
            justify-content: space-around;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .nav-icon {
            font-size: 20px;
            color: var(--text-dim);
            cursor: pointer;
            padding: 5px 15px;
            border-radius: 10px;
        }

        .nav-icon.active {
            color: white;
            background: var(--primary-purple);
        }

        .chat-float {
            position: fixed;
            bottom: 90px;
            right: 20px;
            width: 55px;
            height: 55px;
            background: var(--primary-purple);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 10px 20px rgba(0,0,0,0.3);
            cursor: pointer;
        }

        .hidden { display: none; }
        
        #loading {
            text-align: center;
            padding: 20px;
            display: none;
        }

        .spinner {
            width: 30px;
            height: 30px;
            border: 3px solid rgba(255,255,255,0.1);
            border-top: 3px solid var(--accent-teal);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
            margin: 0 auto;
        }

        @keyframes spin { 100% { transform: rotate(360deg); } }
    </style>
</head>
<body>

    <header>
        <div class="logo-box">
            <div class="logo-circle">⚡</div>
            <div class="logo-name">Cylestria</div>
        </div>
        <button class="btn-get-started">Get Started</button>
    </header>

    <div class="container">
        <!-- Hero Section -->
        <div class="hero-title-main">CYLESTRIA</div>
        
        <div class="version-tag">
            <div class="dot"></div>
            AI-Powered Health Analysis v2.0
        </div>

        <h1 class="headline">Your Personal<br><span>AI Health</span><br>Companion.</h1>
        
        <p class="hero-desc">
            Create your profile, describe your symptoms, and let Cylestria generate a comprehensive health report. We provide instant precautions, prescriptions, and direct medication guidance.
        </p>

        <!-- Tool Section -->
        <div class="card">
            <h3>📝 Describe Symptoms</h3>
            <textarea id="symp-input" placeholder="Enter how you feel... (e.g. Sharp pain in lower abdomen, nausea, slight fever)"></textarea>
            <button class="main-btn" onclick="analyze('symptoms')">Analyze Symptoms</button>
        </div>

        <div class="card">
            <h3>📁 Health Reports</h3>
            <div class="upload-zone" onclick="document.getElementById('file-in').click()">
                <div style="font-size: 32px; margin-bottom: 10px;">📋</div>
                <p style="font-size: 14px;">Tap to upload medical reports</p>
                <p style="font-size: 11px; color: var(--text-dim); margin-top: 5px;">Supports Lab results, Scans, Blood work</p>
                <input type="file" id="file-in" class="hidden" onchange="fileAdded()">
            </div>
            <div id="file-status" style="text-align: center; font-size: 12px; color: var(--accent-teal); margin-bottom: 10px;" class="hidden"></div>
            <button class="main-btn" style="background: transparent; border: 1px solid var(--primary-purple);" onclick="analyze('report')">Deep Analysis</button>
        </div>

        <!-- Result Section -->
        <div id="loading">
            <div class="spinner"></div>
            <p style="margin-top: 10px; font-size: 12px; color: var(--text-dim);">Cylestria is processing medical data...</p>
        </div>

        <div id="analysis-result" class="card">
            <div id="res-badge"></div>
            <h2 id="res-title" style="margin-bottom: 15px;">Diagnostic Summary</h2>
            <p id="res-body" style="font-size: 14px; color: var(--text-dim); margin-bottom: 20px;"></p>
            
            <div style="background: rgba(255,255,255,0.03); border-radius: 12px; padding: 15px;">
                <strong style="font-size: 12px; color: var(--accent-teal); text-transform: uppercase;">Medical Guidance:</strong>
                <p id="res-guidance" style="font-size: 14px; margin-top: 5px;"></p>
            </div>

            <div id="res-action" style="margin-top: 20px; font-weight: bold; text-align: center;"></div>
        </div>

        <!-- Subscription Section -->
        <div class="card premium-card">
            <div style="color: var(--accent-teal); font-size: 12px; font-weight: bold; text-transform: uppercase; letter-spacing: 2px;">Cylestria Pro</div>
            <h2>Full Health Coverage</h2>
            <div class="price">\$5<span>/month</span></div>
            <div style="text-align: left; font-size: 13px; color: var(--text-dim); margin-bottom: 20px; font-family: sans-serif;">
                • Unlimited report processing<br>
                • Real-time doctor-certified remedies<br>
                • 24/7 priority clinical chat<br>
                • Family health tracking
            </div>
            <button class="main-btn" onclick="alert('Redirecting to secure payment...')">Upgrade Now</button>
        </div>

        <div style="height: 120px;"></div>
    </div>

    <!-- Floating UI -->
    <div class="chat-float">
        <span style="font-size: 24px;">💬</span>
    </div>

    <nav class="bottom-nav">
        <div class="nav-icon active">🏠</div>
        <div class="nav-icon">📊</div>
        <div class="nav-icon">🧬</div>
        <div class="nav-icon">👤</div>
    </nav>

    <script>
        function fileAdded() {
            const file = document.getElementById('file-in').files[0];
            const status = document.getElementById('file-status');
            if (file) {
                status.innerText = "✓ Attached: " + file.name;
                status.classList.remove('hidden');
            }
        }

        function analyze(type) {
            const input = document.getElementById('symp-input').value;
            const resArea = document.getElementById('analysis-result');
            const loader = document.getElementById('loading');

            if (type === 'symptoms' && input.length < 5) {
                alert("Please describe your symptoms more clearly for Cylestria to analyze.");
                return;
            }

            resArea.style.display = 'none';
            loader.style.display = 'block';

            // Mock AI Analysis Logic
            setTimeout(() => {
                loader.style.display = 'none';
                resArea.style.display = 'block';
                resArea.scrollIntoView({ behavior: 'smooth' });

                let title = "General Wellness Analysis";
                let badge = "STABLE";
                let badgeClass = "pill-safe";
                let body = "The AI has analyzed your input. It appears you may be experiencing minor fatigue or a common viral strain. No immediate danger detected.";
                let guidance = "Ensure you get at least 8 hours of sleep. Stay hydrated with electrolytes. Over-the-counter vitamins (C, D) are suggested.";
                let action = "✅ Clinical help not required right now.";

                const query = input.toLowerCase();

                if (query.includes('pain') && (query.includes('chest') || query.includes('heart') || query.includes('breath'))) {
                    title = "Critical Alert";
                    badge = "URGENT";
                    badgeClass = "pill-warn";
                    body = "Symptoms related to thoracic discomfort and breathing difficulty detected. These are high-risk indicators.";
                    guidance = "DO NOT SELF-MEDICATE. Sit in a comfortable position and keep breathing deep.";
                    action = "🚨 SEEK IMMEDIATE CLINICAL HELP / EMERGENCY ROOM";
                } else if (query.includes('fever') || query.includes('cough')) {
                    title = "Viral Detection";
                    badge = "MODERATE";
                    badgeClass = "pill-warn";
                    body = "Symptoms suggest a potential respiratory infection or common flu variant.";
                    guidance = "Paracetamol (500mg) for fever. Steam inhalation. Avoid cold drinks.";
                    action = "👨‍⚕️ Consult a clinical doctor if fever persists > 48h.";
                }

                if (type === 'report') {
                    title = "Report Breakdown";
                    body = "Cylestria has parsed your laboratory data. Glucose and Cholesterol levels are within optimal range. Slight dehydration noted in serum markers.";
                    guidance = "Increase water intake to 3L/day. Your next checkup is recommended in 6 months.";
                }

                document.getElementById('res-badge').innerHTML = `<span class="status-pill ${badgeClass}">${badge}</span>`;
                document.getElementById('res-title').innerText = title;
                document.getElementById('res-body').innerText = body;
                document.getElementById('res-guidance').innerText = guidance;
                document.getElementById('res-action').innerText = action;
                document.getElementById('res-action').style.color = badge === 'URGENT' ? '#ef4444' : '#2dd4bf';

            }, 2000);
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CELYSTRIA - AI Health Assistant</title>
    <style>
        /* 
           Simulating the "Soria" font style using high-quality system serifs.
           Soria is known for its elegant, high-contrast serif look.
        */
        :root {
            --bg-dark: #0a041a;
            --card-bg: #16102b;
            --primary: #8b5cf6;
            --accent-blue: #00d2ff;
            --text-white: #ffffff;
            --text-gray: #94a3b8;
            --logo-blue: #1a365d;
            --font-soria: "Soria", "Times New Roman", "Georgia", serif;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-white);
            font-family: var(--font-soria);
            line-height: 1.6;
            padding-bottom: 80px; /* Space for nav */
        }

        .container {
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
        }

        /* Branding Section inspired by the logo upload */
        .brand-header {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 40px 0;
            background: white; /* Header background for logo prominence */
            border-radius: 0 0 30px 30px;
            margin-bottom: 30px;
        }

        .logo-box {
            width: 70px;
            height: 70px;
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            border-radius: 18px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 35px;
            box-shadow: 0 10px 20px rgba(0, 210, 255, 0.2);
        }

        .brand-title {
            font-size: 32px;
            font-weight: 900;
            color: var(--logo-blue);
            letter-spacing: 2px;
            margin-top: 15px;
            text-transform: uppercase;
        }

        .brand-subtitle {
            font-size: 11px;
            letter-spacing: 3px;
            color: #4a5568;
            text-transform: uppercase;
            font-family: sans-serif;
            margin-top: -5px;
        }

        /* Hero Section from Screenshot 1 */
        .hero {
            padding: 20px 0;
        }

        .badge-v2 {
            display: inline-flex;
            align-items: center;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 12px;
            color: var(--text-gray);
            font-family: sans-serif;
            margin-bottom: 20px;
        }

        .badge-v2::before {
            content: "";
            width: 8px;
            height: 8px;
            background: #10b981;
            border-radius: 50%;
            margin-right: 8px;
            box-shadow: 0 0 8px #10b981;
        }

        .hero-h1 {
            font-size: 48px;
            line-height: 1.1;
            margin-bottom: 15px;
            background: linear-gradient(to right, #ffffff, #c084fc, #2dd4bf);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero-p {
            color: var(--text-gray);
            font-size: 16px;
            margin-bottom: 30px;
            font-family: sans-serif;
        }

        /* Functional Cards */
        .card {
            background: var(--card-bg);
            border-radius: 24px;
            padding: 24px;
            margin-bottom: 20px;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }

        h2 {
            font-size: 18px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        textarea {
            width: 100%;
            height: 100px;
            background: rgba(0,0,0,0.2);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 12px;
            padding: 15px;
            color: white;
            font-family: sans-serif;
            font-size: 14px;
            resize: none;
            margin-bottom: 15px;
        }

        .upload-area {
            border: 2px dashed rgba(255, 255, 255, 0.1);
            border-radius: 16px;
            padding: 30px;
            text-align: center;
            cursor: pointer;
            transition: 0.3s;
            margin-bottom: 15px;
        }

        .btn-primary {
            width: 100%;
            background: var(--primary);
            color: white;
            border: none;
            padding: 15px;
            border-radius: 12px;
            font-weight: bold;
            font-size: 16px;
            cursor: pointer;
            font-family: sans-serif;
            text-transform: uppercase;
        }

        /* Premium Section */
        .premium-card {
            background: linear-gradient(135deg, #1e1b4b 0%, #4c1d95 100%);
            border: 1px solid var(--primary);
            text-align: center;
        }

        .price-tag {
            font-size: 42px;
            font-weight: 900;
            margin: 15px 0;
        }

        .price-tag span {
            font-size: 16px;
            color: #d1d5db;
            font-weight: normal;
        }

        /* Result Area */
        #analysis-result {
            display: none;
            animation: slideUp 0.5s ease;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .pill {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 800;
            text-transform: uppercase;
            margin-bottom: 10px;
            font-family: sans-serif;
        }

        .pill-red { background: rgba(239, 68, 68, 0.2); color: #fca5a5; }
        .pill-green { background: rgba(16, 185, 129, 0.2); color: #6ee7b7; }

        /* Navigation Bar from Screenshot 1 */
        .nav-bar {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 400px;
            background: rgba(22, 16, 43, 0.95);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            display: flex;
            justify-content: space-around;
            padding: 12px;
            z-index: 1000;
        }

        .nav-item {
            color: var(--text-gray);
            font-size: 20px;
            padding: 8px;
            border-radius: 12px;
        }

        .nav-item.active {
            background: var(--primary);
            color: white;
        }

        .chat-btn {
            position: fixed;
            bottom: 95px;
            right: 25px;
            width: 55px;
            height: 55px;
            background: var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
        }

        .loader {
            display: none;
            text-align: center;
            padding: 20px;
        }

        .spinner {
            width: 30px;
            height: 30px;
            border: 3px solid rgba(255,255,255,0.1);
            border-top: 3px solid var(--accent-blue);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }

        @keyframes spin { 100% { transform: rotate(360deg); } }
    </style>
</head>
<body>

    <header class="brand-header">
        <div class="logo-box">🧠</div>
        <div class="brand-title">CELYSTRIA</div>
        <div class="brand-tag">AI HEALTH ASSISTANT</div>
    </header>

    <div class="container">
        <!-- Hero Section -->
        <section class="hero">
            <div class="badge-v2">AI-Powered Health Analysis v2.0</div>
            <h1 class="hero-h1">Your Personal<br>AI Health<br>Companion.</h1>
            <p class="hero-p">Describe your symptoms or upload medical reports. CELYSTRIA provides instant precautions, doctor-certified suggestions, and direct clinical guidance.</p>
        </section>

        <!-- Actions -->
        <div class="card">
            <h2>📝 Analyze Symptoms</h2>
            <textarea id="symptom-text" placeholder="e.g. Sharp pain in the lower abdomen, persistent cough for 3 days..."></textarea>
            <button class="btn-primary" onclick="runAnalysis('symptoms')">Generate Health Report</button>
        </div>

        <div class="card">
            <h2>📁 Medical Report</h2>
            <div class="upload-area" onclick="document.getElementById('report-file').click()">
                <div style="font-size: 30px; margin-bottom: 10px;">📄</div>
                <p style="font-size: 14px;">Drop your lab reports here</p>
                <input type="file" id="report-file" style="display: none;" onchange="fileUpdate()">
                <div id="file-name" style="margin-top:10px; font-size: 12px; color: var(--accent-blue);"></div>
            </div>
            <button class="btn-primary" style="background: transparent; border: 1px solid var(--primary);" onclick="runAnalysis('reports')">Deep Scan Report</button>
        </div>

        <!-- Result View -->
        <div id="loading" class="loader">
            <div class="spinner"></div>
            <p style="margin-top: 10px; font-size: 12px; color: var(--text-gray);">CELYSTRIA AI is processing...</p>
        </div>

        <div id="analysis-result" class="card">
            <div id="res-pill"></div>
            <h3 id="res-header" style="margin-bottom: 10px;">Analysis Result</h3>
            <p id="res-body" style="font-size: 14px; color: var(--text-gray); margin-bottom: 20px;"></p>
            
            <div style="background: rgba(255,255,255,0.05); padding: 15px; border-radius: 12px;">
                <strong style="font-size: 12px; color: var(--accent-blue); text-transform: uppercase;">AI Suggestion:</strong>
                <p id="res-guidance" style="font-size: 14px; margin-top: 5px;"></p>
            </div>
            
            <div id="res-clinical" style="margin-top: 20px; font-weight: bold; text-align: center; font-size: 15px;"></div>
        </div>

        <!-- Premium Plan -->
        <div class="card premium-card">
            <div style="font-size: 12px; font-weight: bold; color: var(--accent-blue); letter-spacing: 2px;">CELYSTRIA PRO</div>
            <h2 style="justify-content: center; margin-top: 10px;">Full Medical Insight</h2>
            <div class="price-tag">\$5<span>/mo</span></div>
            <ul style="list-style: none; font-size: 13px; color: #d1d5db; text-align: left; margin: 20px 0; padding-left: 10px;">
                <li>✨ Unlimited Report Uploads</li>
                <li>👩‍⚕️ Certified Clinical Remedies</li>
                <li>📊 Health Trend Visualizer</li>
                <li>💬 Priority AI Chat 24/7</li>
            </ul>
            <button class="btn-primary" onclick="alert('Connecting to secure payment gateway...')">Upgrade Now</button>
        </div>

    </div>

    <!-- Navigation -->
    <div class="chat-btn">💬</div>

    <nav class="nav-bar">
        <div class="nav-item active">🏠</div>
        <div class="nav-item">📊</div>
        <div class="nav-item">🧬</div>
        <div class="nav-item">👤</div>
    </nav>

    <script>
        function fileUpdate() {
            const file = document.getElementById('report-file').files[0];
            if(file) document.getElementById('file-name').innerText = "✓ " + file.name;
        }

        function runAnalysis(mode) {
            const symptoms = document.getElementById('symptom-text').value;
            const resArea = document.getElementById('analysis-result');
            const loader = document.getElementById('loading');

            if (mode === 'symptoms' && symptoms.length < 10) {
                alert("Please describe your symptoms more clearly for an accurate analysis.");
                return;
            }

            resArea.style.display = 'none';
            loader.style.display = 'block';

            setTimeout(() => {
                loader.style.display = 'none';
                resArea.style.display = 'block';
                resArea.scrollIntoView({ behavior: 'smooth' });

                let data = {
                    title: "Health Status: Stable",
                    pill: "HEALTHY",
                    pillClass: "pill-green",
                    body: "Our AI model suggests your symptoms are non-critical and likely related to common environmental factors or minor physical strain.",
                    guidance: "Increase hydration and ensure 8 hours of restful sleep. If symptoms persist for more than 3 days, consult a physician.",
                    clinical: "✅ Clinical help not immediately required."
                };

                const input = symptoms.toLowerCase();
                if (input.includes('chest') || input.includes('heart') || input.includes('breath')) {
                    data = {
                        title: "Critical Cardiovascular Alert",
                        pill: "URGENT",
                        pillClass: "pill-red",
                        body: "Symptoms detected match high-risk clinical patterns for respiratory or cardiac distress. Immediate professional intervention is advised.",
                        guidance: "Do not take any home medication. Stay calm and sit down.",
                        clinical: "🚨 SEEK IMMEDIATE CLINICAL HELP / ER"
                    };
                } else if (input.includes('fever') || input.includes('stomach') || input.includes('headache')) {
                    data = {
                        title: "Systemic Infection Analysis",
                        pill: "MODERATE",
                        pillClass: "pill-red",
                        body: "Symptoms suggest a potential viral infection or acute inflammation. Monitoring temperature is recommended.",
                        guidance: "Rest and hydration. Consider 500mg Paracetamol for fever management (if not allergic).",
                        clinical: "👨‍⚕️ Clinical visit recommended if symptoms worsen."
                    };
                }

                if(mode === 'reports') {
                    data.title = "Lab Report Parsed";
                    data.body = "CELYSTRIA AI has processed your report. Most markers are within reference ranges. WBC count shows mild elevation consistent with a recovering infection.";
                    data.guidance = "Your lab results are 92% healthy. Supplement with Vitamin C for immune support.";
                    data.clinical = "✅ No urgent clinical action needed.";
                }

                document.getElementById('res-pill').innerHTML = `<span class="pill ${data.pillClass}">${data.pill}</span>`;
                document.getElementById('res-header').innerText = data.title;
                document.getElementById('res-body').innerText = data.body;
                document.getElementById('res-guidance').innerText = data.guidance;
                document.getElementById('res-clinical').innerText = data.clinical;
                document.getElementById('res-clinical').style.color = data.pillClass === 'pill-red' ? '#fca5a5' : '#6ee7b7';

            }, 2000);
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CELYSTRIA - AI Health Assistant</title>
    <style>
        /* Soria-style serif proxy */
        :root {
            --bg-dark: #0a041a;
            --card-bg: #16102b;
            --primary: #8b5cf6;
            --accent-blue: #00d2ff;
            --text-white: #ffffff;
            --text-gray: #94a3b8;
            --logo-blue: #1a365d;
            --font-serif: "Soria", "Georgia", "Palatino", serif;
            --font-sans: "Inter", "Segoe UI", sans-serif;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-white);
            font-family: var(--font-serif);
            line-height: 1.6;
            overflow-x: hidden;
            min-height: 100vh;
        }

        .hidden { display: none !important; }

        /* Auth Screen Styling */
        #auth-screen {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
            background: linear-gradient(180deg, #ffffff 0%, #e2e8f0 100%);
            color: var(--logo-blue);
        }

        .auth-card {
            background: white;
            width: 100%;
            max-width: 400px;
            padding: 30px;
            border-radius: 30px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            text-align: center;
        }

        .auth-input {
            width: 100%;
            padding: 12px 15px;
            margin-bottom: 15px;
            border: 1px solid #cbd5e1;
            border-radius: 12px;
            font-family: var(--font-sans);
            font-size: 14px;
        }

        .auth-toggle {
            margin-top: 15px;
            font-size: 13px;
            color: #64748b;
            cursor: pointer;
            font-family: var(--font-sans);
        }

        .auth-toggle b { color: var(--primary); }

        /* Main App Header */
        header {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 30px 0;
            background: white;
            border-radius: 0 0 40px 40px;
            margin-bottom: 20px;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .logo-box {
            width: 60px;
            height: 60px;
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            box-shadow: 0 8px 15px rgba(0, 210, 255, 0.3);
        }

        .brand-title {
            font-size: 28px;
            font-weight: 900;
            color: var(--logo-blue);
            letter-spacing: 2px;
            margin-top: 10px;
            text-transform: uppercase;
        }

        .brand-subtitle {
            font-size: 10px;
            letter-spacing: 2px;
            color: #4a5568;
            text-transform: uppercase;
            font-family: var(--font-sans);
            margin-top: -5px;
        }

        .container {
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
            padding-bottom: 100px;
        }

        /* Hero Text */
        .hero-h1 {
            font-size: 44px;
            line-height: 1.1;
            margin-bottom: 15px;
            background: linear-gradient(to right, #ffffff, #c084fc, #2dd4bf);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero-p {
            color: var(--text-gray);
            font-size: 15px;
            margin-bottom: 30px;
            font-family: var(--font-sans);
        }

        /* Functional Cards */
        .card {
            background: var(--card-bg);
            border-radius: 24px;
            padding: 24px;
            margin-bottom: 20px;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }

        h2 {
            font-size: 18px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        textarea {
            width: 100%;
            height: 100px;
            background: rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 12px;
            padding: 15px;
            color: white;
            font-family: var(--font-sans);
            font-size: 14px;
            resize: none;
            margin-bottom: 15px;
        }

        .btn-primary {
            width: 100%;
            background: var(--primary);
            color: white;
            border: none;
            padding: 15px;
            border-radius: 12px;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            font-family: var(--font-sans);
            text-transform: uppercase;
            transition: 0.2s;
        }

        .btn-primary:active { transform: scale(0.98); }

        .upload-area {
            border: 2px dashed rgba(255, 255, 255, 0.1);
            border-radius: 16px;
            padding: 25px;
            text-align: center;
            cursor: pointer;
            margin-bottom: 15px;
        }

        /* Analysis Result */
        #analysis-view {
            animation: slideUp 0.5s ease;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .pill {
            display: inline-block;
            padding: 4px 10px;
            border-radius: 8px;
            font-size: 11px;
            font-weight: bold;
            margin-bottom: 10px;
            font-family: var(--font-sans);
        }

        .pill-safe { background: #dcfce7; color: #166534; }
        .pill-alert { background: #fee2e2; color: #991b1b; }

        /* Premium Plan */
        .premium-card {
            background: linear-gradient(135deg, #1e1b4b 0%, #4c1d95 100%);
            border: 1px solid var(--primary);
            text-align: center;
        }

        .price-tag {
            font-size: 38px;
            font-weight: 900;
            margin: 10px 0;
        }

        /* Bottom Nav */
        .bottom-nav {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 400px;
            background: rgba(22, 16, 43, 0.95);
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            display: flex;
            justify-content: space-around;
            padding: 12px;
            z-index: 1000;
        }

        .nav-item {
            color: var(--text-gray);
            font-size: 22px;
            padding: 5px 15px;
            border-radius: 12px;
            cursor: pointer;
        }

        .nav-item.active {
            color: white;
            background: var(--primary);
        }

        /* Profile View */
        .profile-header {
            text-align: center;
            padding: 20px 0;
        }

        .avatar {
            width: 80px;
            height: 80px;
            background: var(--primary);
            border-radius: 50%;
            margin: 0 auto 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            border: 4px solid rgba(255,255,255,0.1);
        }

        .loader {
            text-align: center;
            padding: 20px;
            display: none;
        }

        .spinner {
            width: 30px;
            height: 30px;
            border: 3px solid rgba(255,255,255,0.1);
            border-top: 3px solid var(--accent-blue);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }

        @keyframes spin { 100% { transform: rotate(360deg); } }
    </style>
</head>
<body>

    <!-- Authentication Screen -->
    <div id="auth-screen">
        <div class="auth-card">
            <div class="logo-box" style="margin: 0 auto;">🧠</div>
            <div class="brand-title">CELYSTRIA</div>
            <div class="brand-subtitle" style="margin-bottom: 25px;">AI HEALTH ASSISTANT</div>
            
            <div id="login-form">
                <h2 style="justify-content: center; color: var(--logo-blue);">Welcome Back</h2>
                <input type="email" class="auth-input" placeholder="Email Address" id="login-email">
                <input type="password" class="auth-input" placeholder="Password" id="login-pass">
                <button class="btn-primary" onclick="handleAuth('login')">Login</button>
                <div class="auth-toggle" onclick="toggleAuth()">New to Cylestria? <b>Create Account</b></div>
            </div>

            <div id="signup-form" class="hidden">
                <h2 style="justify-content: center; color: var(--logo-blue);">Create Account</h2>
                <input type="text" class="auth-input" placeholder="Full Name" id="reg-name">
                <input type="email" class="auth-input" placeholder="Email Address" id="reg-email">
                <input type="password" class="auth-input" placeholder="Password" id="reg-pass">
                <button class="btn-primary" onclick="handleAuth('signup')">Sign Up</button>
                <div class="auth-toggle" onclick="toggleAuth()">Already have an account? <b>Login</b></div>
            </div>
        </div>
    </div>

    <!-- Main App Content -->
    <div id="app-content" class="hidden">
        <header>
            <div class="logo-box">🧠</div>
            <div class="brand-title">CELYSTRIA</div>
            <div class="brand-subtitle">AI HEALTH ASSISTANT</div>
        </header>

        <!-- View: Home -->
        <div id="view-home" class="container view">
            <section class="hero">
                <div style="font-size: 12px; color: var(--accent-blue); margin-bottom: 10px; font-family: var(--font-sans);">WELCOME BACK, <span id="user-display-name">USER</span></div>
                <h1 class="hero-h1">Personal AI<br>Health Companion.</h1>
                <p class="hero-p">Analyze symptoms or upload reports for instant clinical guidance and doctor-certified remedies.</p>
            </section>

            <div class="card">
                <h2>📝 Analyze Symptoms</h2>
                <textarea id="symptom-input" placeholder="Describe your symptoms (e.g. Headache, fatigue, nausea for 2 days)..."></textarea>
                <button class="btn-primary" onclick="startAnalysis('symptoms')">Generate Health Report</button>
            </div>

            <div class="card">
                <h2>📁 Medical Report</h2>
                <div class="upload-area" onclick="document.getElementById('file-trigger').click()">
                    <div style="font-size: 30px;">📋</div>
                    <p style="font-size: 13px; margin-top: 10px; color: var(--text-gray);">Tap to upload Blood Work or Lab Results</p>
                    <input type="file" id="file-trigger" class="hidden" onchange="document.getElementById('file-status').innerText = '✓ Attached: ' + this.files[0].name">
                    <div id="file-status" style="font-size: 11px; color: var(--accent-blue); margin-top: 8px;"></div>
                </div>
                <button class="btn-primary" style="background: transparent; border: 1px solid var(--primary);" onclick="startAnalysis('reports')">Deep Scan Report</button>
            </div>

            <div id="loader" class="loader">
                <div class="spinner"></div>
                <p style="font-size: 12px; color: var(--text-gray); margin-top: 10px;">AI is processing data...</p>
            </div>

            <!-- Analysis Result Area -->
            <div id="analysis-view" class="card hidden">
                <div id="res-pill"></div>
                <h3 id="res-title" style="margin-bottom: 10px;">Assessment Result</h3>
                <p id="res-body" style="font-size: 14px; color: var(--text-gray); margin-bottom: 15px;"></p>
                <div style="background: rgba(255,255,255,0.05); padding: 15px; border-radius: 12px;">
                    <strong style="font-size: 12px; color: var(--accent-blue);">💊 SUGGESTED REMEDY:</strong>
                    <p id="res-remedy" style="font-size: 14px; margin-top: 5px;"></p>
                </div>
                <div id="res-clinical" style="margin-top: 20px; font-weight: bold; text-align: center;"></div>
            </div>
        </div>

        <!-- View: Premium -->
        <div id="view-premium" class="container view hidden">
            <div class="card premium-card">
                <div style="font-size: 12px; font-weight: bold; color: var(--accent-blue); letter-spacing: 2px;">CELYSTRIA PRO</div>
                <h2 style="justify-content: center; margin: 15px 0;">Premium Care Plan</h2>
                <div class="price-tag">\$5<span style="font-size: 16px; color: #cbd5e1; font-weight: normal;">/month</span></div>
                <ul style="list-style: none; text-align: left; font-size: 13px; color: #e2e8f0; margin: 25px 0; font-family: var(--font-sans);">
                    <li>✅ Unlimited Report Analysis</li>
                    <li>✅ Doctor-Certified Daily Suggestions</li>
                    <li>✅ Priority Clinical Chat Support</li>
                    <li>✅ Real-time Vital Tracking</li>
                </ul>
                <button class="btn-primary" onclick="alert('Proceeding to Checkout...')">Subscribe Now</button>
            </div>
        </div>

        <!-- View: Profile -->
        <div id="view-profile" class="container view hidden">
            <div class="profile-header">
                <div class="avatar">👤</div>
                <h2 id="prof-name" style="justify-content: center;">Full Name</h2>
                <p id="prof-email" style="font-size: 14px; color: var(--text-gray); font-family: var(--font-sans);">email@example.com</p>
            </div>
            
            <div class="card" style="margin-top: 20px;">
                <h3 style="font-size: 16px; margin-bottom: 15px;">Account Settings</h3>
                <div style="font-family: var(--font-sans); font-size: 14px;">
                    <div style="padding: 12px 0; border-bottom: 1px solid rgba(255,255,255,0.05);">Plan Status: <span style="color: var(--accent-blue);">Basic (Free)</span></div>
                    <div style="padding: 12px 0; border-bottom: 1px solid rgba(255,255,255,0.05);">Health Data: <span style="color: var(--text-gray);">2 Reports Analyzed</span></div>
                </div>
            </div>

            <button class="btn-primary" style="background: #ef4444; margin-top: 20px;" onclick="handleAuth('logout')">Log Out</button>
        </div>

        <!-- Navigation -->
        <nav class="bottom-nav">
            <div class="nav-item active" onclick="switchView('home', this)">🏠</div>
            <div class="nav-item" onclick="switchView('premium', this)">💎</div>
            <div class="nav-item" onclick="switchView('profile', this)">👤</div>
        </nav>
    </div>

    <script>
        let currentUser = null;

        function toggleAuth() {
            document.getElementById('login-form').classList.toggle('hidden');
            document.getElementById('signup-form').classList.toggle('hidden');
        }

        function handleAuth(type) {
            if (type === 'login') {
                const email = document.getElementById('login-email').value;
                if(!email) return alert("Please enter an email");
                currentUser = { name: email.split('@')[0], email: email };
                showApp();
            } else if (type === 'signup') {
                const name = document.getElementById('reg-name').value;
                const email = document.getElementById('reg-email').value;
                if(!name || !email) return alert("Please fill all fields");
                currentUser = { name: name, email: email };
                showApp();
            } else if (type === 'logout') {
                currentUser = null;
                document.getElementById('app-content').classList.add('hidden');
                document.getElementById('auth-screen').classList.remove('hidden');
            }
        }

        function showApp() {
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app-content').classList.remove('hidden');
            document.getElementById('user-display-name').innerText = currentUser.name.toUpperCase();
            document.getElementById('prof-name').innerText = currentUser.name;
            document.getElementById('prof-email').innerText = currentUser.email;
            switchView('home', document.querySelector('.nav-item'));
        }

        function switchView(viewName, el) {
            // Hide all views
            document.querySelectorAll('.view').forEach(v => v.classList.add('hidden'));
            // Show target view
            document.getElementById('view-' + viewName).classList.remove('hidden');
            // Nav update
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
            // Reset analysis result when switching back to home
            if (viewName !== 'home') document.getElementById('analysis-view').classList.add('hidden');
        }

        function startAnalysis(mode) {
            const input = document.getElementById('symptom-input').value;
            const resArea = document.getElementById('analysis-view');
            const loader = document.getElementById('loader');

            if (mode === 'symptoms' && input.length < 5) {
                return alert("Please describe your symptoms in more detail.");
            }

            resArea.classList.add('hidden');
            loader.style.display = 'block';

            setTimeout(() => {
                loader.style.display = 'none';
                resArea.classList.remove('hidden');
                resArea.scrollIntoView({ behavior: 'smooth' });

                let data = {
                    title: "Standard Health Assessment",
                    pill: "STABLE",
                    pillClass: "pill-safe",
                    body: "AI analysis suggests your symptoms are non-critical. Current patterns indicate common fatigue or seasonal adjustment.",
                    remedy: "High fluid intake, vitamin C supplementation, and 8 hours of rest.",
                    clinical: "✅ No immediate clinical help needed."
                };

                const text = input.toLowerCase();
                if (text.includes('chest') || text.includes('breath') || text.includes('heart')) {
                    data = {
                        title: "Urgent Medical Concern",
                        pill: "CRITICAL",
                        pillClass: "pill-alert",
                        body: "Symptoms detected relate to high-priority respiratory or cardiac indicators.",
                        remedy: "Do not self-medicate. Stay seated and avoid physical exertion.",
                        clinical: "🚨 SEEK IMMEDIATE CLINICAL HELP / EMERGENCY"
                    };
                } else if (text.includes('fever') || text.includes('infection')) {
                    data = {
                        title: "Viral Indicator Detected",
                        pill: "CAUTION",
                        pillClass: "pill-alert",
                        body: "Clinical markers match potential infection or flu. Temperature monitoring is advised.",
                        remedy: "Paracetamol 500mg (consult doctor for dosage), hydration, and rest.",
                        clinical: "👨‍⚕️ Clinical visit recommended if fever persists."
                    };
                }

                if (mode === 'reports') {
                    data.title = "Lab Result Summary";
                    data.body = "Report parsed successfully. All key markers (Glucose, Hb, WBC) are within reference ranges. Slight iron deficiency noted.";
                    data.remedy = "Spinach, legumes, or a doctor-certified iron supplement.";
                    data.clinical = "✅ No urgent clinical action required.";
                }

                document.getElementById('res-pill').innerHTML = `<span class="pill ${data.pillClass}">${data.pill}</span>`;
                document.getElementById('res-title').innerText = data.title;
                document.getElementById('res-body').innerText = data.body;
                document.getElementById('res-remedy').innerText = data.remedy;
                document.getElementById('res-clinical').innerText = data.clinical;
                document.getElementById('res-clinical').style.color = data.pillClass === 'pill-alert' ? '#ef4444' : '#2dd4bf';
            }, 1800);
        }
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CELYSTRIA - Ultimate AI Health Suite</title>
    <style>
        /* Soria serif proxy */
        :root {
            --bg-dark: #0a041a;
            --bg-light: #f8fafc;
            --card-bg: #16102b;
            --primary: #8b5cf6;
            --primary-soft: rgba(139, 92, 246, 0.1);
            --accent: #00d2ff;
            --text-white: #ffffff;
            --text-gray: #94a3b8;
            --text-dark: #1e293b;
            --success: #10b981;
            --danger: #ef4444;
            --font-serif: "Soria", "Georgia", "Palatino", serif;
            --font-sans: "Inter", -apple-system, sans-serif;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; transition: all 0.2s ease; }
        body { background-color: var(--bg-dark); color: var(--text-white); font-family: var(--font-serif); line-height: 1.5; overflow-x: hidden; }
        .hidden { display: none !important; }

        /* --- AUTH SCREEN --- */
        #auth-screen {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
            background: linear-gradient(180deg, #ffffff 0%, #cbd5e1 100%);
            color: #1a365d;
        }
        .auth-card {
            background: white;
            width: 100%;
            max-width: 400px;
            padding: 40px 30px;
            border-radius: 40px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.2);
            text-align: center;
        }
        .auth-logo { width: 70px; height: 70px; background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); border-radius: 20px; margin: 0 auto 15px; display: flex; align-items: center; justify-content: center; font-size: 35px; }
        .auth-input { width: 100%; padding: 14px; margin-bottom: 12px; border: 1px solid #e2e8f0; border-radius: 14px; font-family: var(--font-sans); }
        .btn-auth { width: 100%; background: #1a365d; color: white; padding: 15px; border-radius: 14px; border: none; font-weight: bold; cursor: pointer; font-family: var(--font-sans); margin-top: 10px; }

        /* --- LAYOUT --- */
        header { background: white; padding: 25px 20px; border-radius: 0 0 40px 40px; display: flex; flex-direction: column; align-items: center; position: sticky; top: 0; z-index: 100; box-shadow: 0 10px 30px rgba(0,0,0,0.2); }
        .header-logo { width: 50px; height: 50px; background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); border-radius: 14px; display: flex; align-items: center; justify-content: center; font-size: 25px; }
        .brand-name { color: #1a365d; font-size: 24px; font-weight: 900; letter-spacing: 2px; margin-top: 8px; text-transform: uppercase; }
        .brand-sub { color: #64748b; font-size: 9px; font-family: var(--font-sans); letter-spacing: 3px; margin-top: -4px; }

        .container { max-width: 500px; margin: 0 auto; padding: 20px 20px 100px 20px; }

        /* --- DASHBOARD / WIDGETS --- */
        .card { background: var(--card-bg); border-radius: 28px; padding: 24px; margin-bottom: 20px; border: 1px solid rgba(255, 255, 255, 0.05); }
        .pro-badge { background: linear-gradient(90deg, #f59e0b, #fbbf24); color: #451a03; padding: 2px 8px; border-radius: 6px; font-size: 10px; font-weight: 800; font-family: var(--font-sans); margin-left: 10px; }
        
        /* Vital Charts Mock */
        .vitals-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 20px; }
        .vital-box { background: rgba(255,255,255,0.03); padding: 15px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.05); }
        .vital-val { font-size: 24px; font-weight: bold; color: var(--accent); }
        .vital-label { font-size: 11px; color: var(--text-gray); font-family: var(--font-sans); }
        .mini-chart { display: flex; align-items: flex-end; gap: 3px; height: 30px; margin-top: 10px; }
        .bar { width: 4px; background: var(--primary); border-radius: 2px; }

        /* --- FEATURE VIEWS --- */
        .view-title { font-size: 32px; margin-bottom: 10px; background: linear-gradient(to right, #fff, var(--primary)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        /* Medication Tracker */
        .med-item { display: flex; align-items: center; gap: 15px; padding: 15px; background: rgba(255,255,255,0.03); border-radius: 18px; margin-bottom: 10px; }
        .med-icon { width: 40px; height: 40px; background: var(--primary-soft); border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 20px; }
        
        /* Chat Interface */
        #chat-container { height: 300px; overflow-y: auto; display: flex; flex-direction: column; gap: 10px; margin-bottom: 15px; padding-right: 5px; scrollbar-width: none; }
        .msg { max-width: 80%; padding: 12px 16px; border-radius: 18px; font-size: 14px; font-family: var(--font-sans); }
        .msg-bot { background: rgba(255,255,255,0.08); align-self: flex-start; border-bottom-left-radius: 4px; }
        .msg-user { background: var(--primary); align-self: flex-end; border-bottom-right-radius: 4px; }

        /* --- BUTTONS --- */
        .btn-main { width: 100%; background: var(--primary); color: white; border: none; padding: 16px; border-radius: 16px; font-weight: bold; font-family: var(--font-sans); cursor: pointer; text-transform: uppercase; letter-spacing: 1px; }
        .btn-main:hover { transform: translateY(-2px); box-shadow: 0 10px 20px rgba(139, 92, 246, 0.3); }
        .btn-outline { width: 100%; background: transparent; border: 1px solid var(--primary); color: white; padding: 14px; border-radius: 16px; font-family: var(--font-sans); margin-top: 10px; cursor: pointer; }

        /* --- PREMIUM MODAL --- */
        .premium-card { background: linear-gradient(135deg, #1e1b4b 0%, #4c1d95 100%); border: 2px solid #f59e0b; text-align: center; }
        .price-big { font-size: 48px; font-weight: 900; margin: 15px 0; }
        .price-big span { font-size: 16px; color: #cbd5e1; }

        /* --- NAV BAR --- */
        nav { position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); width: 92%; max-width: 450px; background: rgba(22, 16, 43, 0.95); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.1); border-radius: 25px; display: flex; justify-content: space-around; padding: 10px; z-index: 1000; box-shadow: 0 20px 40px rgba(0,0,0,0.5); }
        .nav-tab { color: var(--text-gray); font-size: 22px; padding: 10px 18px; border-radius: 18px; cursor: pointer; display: flex; flex-direction: column; align-items: center; }
        .nav-tab.active { color: white; background: var(--primary); }
        .nav-tab span { font-size: 9px; font-family: var(--font-sans); margin-top: 4px; font-weight: bold; }

        /* --- UI HELPERS --- */
        textarea { width: 100%; height: 100px; background: rgba(0,0,0,0.3); border: 1px solid rgba(255,255,255,0.1); border-radius: 16px; padding: 15px; color: white; font-family: var(--font-sans); font-size: 14px; resize: none; margin-bottom: 15px; }
        .status-pill { display: inline-block; padding: 4px 10px; border-radius: 8px; font-size: 10px; font-weight: 800; font-family: var(--font-sans); margin-bottom: 10px; text-transform: uppercase; }
        .pill-warn { background: var(--danger); color: white; }
        .pill-ok { background: var(--success); color: white; }
        .pulse { animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }
    </style>
</head>
<body>

    <!-- SECTION: AUTH -->
    <div id="auth-screen">
        <div class="auth-card">
            <div class="auth-logo">🧠</div>
            <h1 style="color: #1a365d; margin-bottom: 5px;">CELYSTRIA</h1>
            <p style="font-size: 10px; letter-spacing: 2px; color: #64748b; margin-bottom: 30px;">ULTIMATE HEALTH SUITE</p>
            
            <div id="login-view">
                <input type="email" class="auth-input" placeholder="Email Address" id="auth-email">
                <input type="password" class="auth-input" placeholder="Password" id="auth-pass">
                <button class="btn-auth" onclick="doAuth()">Login to Companion</button>
                <p style="margin-top: 20px; font-size: 13px; font-family: sans-serif; cursor: pointer;" onclick="toggleAuth(true)">New here? <b>Create Account</b></p>
            </div>

            <div id="signup-view" class="hidden">
                <input type="text" class="auth-input" placeholder="Full Name" id="reg-name">
                <input type="email" class="auth-input" placeholder="Email Address" id="reg-email">
                <input type="password" class="auth-input" placeholder="Create Password" id="reg-pass">
                <button class="btn-auth" onclick="doAuth()">Initialize Health Profile</button>
                <p style="margin-top: 20px; font-size: 13px; font-family: sans-serif; cursor: pointer;" onclick="toggleAuth(false)">Already a member? <b>Login</b></p>
            </div>
        </div>
    </div>

    <!-- SECTION: MAIN APP -->
    <div id="app-content" class="hidden">
        <header>
            <div class="header-logo">🧠</div>
            <div class="brand-name">CELYSTRIA</div>
            <div class="brand-sub">AI HEALTH ASSISTANT</div>
        </header>

        <div class="container">
            
            <!-- VIEW: HOME / DASHBOARD -->
            <div id="view-home" class="view">
                <div class="view-title">Hello, <span id="user-name">User</span><span id="pro-badge-main" class="pro-badge hidden">PRO</span></div>
                <p class="vital-label" style="margin-bottom: 20px;">Your health metrics at a glance</p>

                <div class="vitals-grid">
                    <div class="vital-box">
                        <div class="vital-label">❤️ Heart Rate</div>
                        <div class="vital-val">72 <small style="font-size: 12px;">bpm</small></div>
                        <div class="mini-chart">
                            <div class="bar" style="height: 40%;"></div>
                            <div class="bar" style="height: 60%;"></div>
                            <div class="bar" style="height: 50%;"></div>
                            <div class="bar" style="height: 80%;"></div>
                            <div class="bar pulse" style="height: 70%; background: var(--danger);"></div>
                        </div>
                    </div>
                    <div class="vital-box">
                        <div class="vital-label">💤 Sleep Score</div>
                        <div class="vital-val">84 <small style="font-size: 12px;">/100</small></div>
                        <div class="mini-chart">
                            <div class="bar" style="height: 20%;"></div>
                            <div class="bar" style="height: 30%;"></div>
                            <div class="bar" style="height: 90%;"></div>
                            <div class="bar" style="height: 85%;"></div>
                            <div class="bar" style="height: 80%; background: var(--success);"></div>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <h3 style="margin-bottom: 10px;">☀️ Daily Wellness Tip</h3>
                    <p style="font-size: 14px; color: var(--text-gray); font-family: var(--font-sans);">Drinking warm lemon water in the morning can help kickstart your metabolism and boost immunity.</p>
                </div>

                <div class="card">
                    <h3 style="margin-bottom: 15px;">🔍 AI Quick Scan</h3>
                    <textarea id="symptom-box" placeholder="Tell me your symptoms..."></textarea>
                    <button class="btn-main" onclick="runAnalysis()">Analyze Symptoms</button>
                    <div id="result-box" class="hidden" style="margin-top: 20px; padding-top: 15px; border-top: 1px solid rgba(255,255,255,0.1);">
                        <div id="res-pill" class="status-pill"></div>
                        <h4 id="res-title"></h4>
                        <p id="res-desc" style="font-size: 13px; color: var(--text-gray); margin: 10px 0;"></p>
                        <div style="background: var(--primary-soft); padding: 12px; border-radius: 12px; font-size: 13px;">
                            <b>💊 Recommended:</b> <span id="res-med"></span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- VIEW: CHAT -->
            <div id="view-chat" class="view hidden">
                <div class="view-title">AI Assistant</div>
                <div class="card" style="padding: 15px;">
                    <div id="chat-container">
                        <div class="msg msg-bot">Hello! I'm Cylestria. How can I assist you with your health today?</div>
                    </div>
                    <div style="display: flex; gap: 10px;">
                        <input type="text" id="chat-input" class="auth-input" placeholder="Ask anything..." style="margin: 0; background: rgba(255,255,255,0.05); color: white; border-color: rgba(255,255,255,0.1);">
                        <button class="btn-main" style="width: auto; padding: 0 20px;" onclick="sendChat()">🚀</button>
                    </div>
                </div>
            </div>

            <!-- VIEW: LABS / REPORTS -->
            <div id="view-labs" class="view hidden">
                <div class="view-title">Medical Lab</div>
                <div class="card" style="text-align: center;">
                    <div style="font-size: 40px; margin-bottom: 15px;">📁</div>
                    <h3>Upload New Report</h3>
                    <p style="font-size: 12px; color: var(--text-gray); margin-bottom: 20px;">Upload blood tests, MRI scans, or clinical notes.</p>
                    <button class="btn-main" onclick="alert('Feature requires camera/file access simulation.')">Scan Document</button>
                </div>
                <h3 style="margin-bottom: 15px;">History</h3>
                <div class="med-item">
                    <div class="med-icon">🩸</div>
                    <div>
                        <div style="font-size: 14px; font-weight: bold;">CBC Blood Test</div>
                        <div style="font-size: 11px; color: var(--text-gray);">Feb 08, 2026 • Analyzed</div>
                    </div>
                </div>
                <div class="med-item">
                    <div class="med-icon">🩻</div>
                    <div>
                        <div style="font-size: 14px; font-weight: bold;">Chest X-Ray</div>
                        <div style="font-size: 11px; color: var(--text-gray);">Jan 22, 2026 • Stable</div>
                    </div>
                </div>
            </div>

            <!-- VIEW: MEDS -->
            <div id="view-meds" class="view hidden">
                <div class="view-title">Reminders</div>
                <div class="card" style="background: var(--primary-soft); border-color: var(--primary);">
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <h3>Active Schedule</h3>
                        <span style="font-size: 20px;">🔔</span>
                    </div>
                </div>
                
                <div class="med-item">
                    <div class="med-icon">💊</div>
                    <div style="flex: 1;">
                        <div style="font-size: 14px; font-weight: bold;">Vitamin D3</div>
                        <div style="font-size: 11px; color: var(--text-gray);">10:00 AM • Daily</div>
                    </div>
                    <button style="background: var(--success); border: none; padding: 5px 10px; border-radius: 8px; color: white; font-size: 10px;">DONE</button>
                </div>
                <div class="med-item">
                    <div class="med-icon">💊</div>
                    <div style="flex: 1;">
                        <div style="font-size: 14px; font-weight: bold;">Omega 3</div>
                        <div style="font-size: 11px; color: var(--text-gray);">02:00 PM • Daily</div>
                    </div>
                    <button style="background: rgba(255,255,255,0.1); border: none; padding: 5px 10px; border-radius: 8px; color: white; font-size: 10px;">SKIP</button>
                </div>
                <button class="btn-outline">Add New Reminder +</button>
            </div>

            <!-- VIEW: PREMIUM -->
            <div id="view-premium" class="view hidden">
                <div class="card premium-card">
                    <div style="background: #f59e0b; color: #451a03; display: inline-block; padding: 2px 10px; border-radius: 20px; font-size: 10px; font-weight: bold; margin-bottom: 10px;">LIMITLESS ACCESS</div>
                    <h2>CELYSTRIA PRO</h2>
                    <div class="price-big">\$5<span>/month</span></div>
                    <ul style="list-style: none; text-align: left; margin: 25px 0; font-family: var(--font-sans); font-size: 13px; color: #e2e8f0;">
                        <li style="margin-bottom: 10px;">✅ Unlimited AI Report Analysis</li>
                        <li style="margin-bottom: 10px;">✅ 24/7 Priority Doctor-AI Chat</li>
                        <li style="margin-bottom: 10px;">✅ Advanced Vital Trend Tracking</li>
                        <li style="margin-bottom: 10px;">✅ Medication History Backup</li>
                        <li style="margin-bottom: 10px;">✅ Clinical Help Locator & Direct SOS</li>
                    </ul>
                    <button class="btn-main" style="background: #f59e0b; color: #451a03;" onclick="upgradePro()">Unlock Pro Features</button>
                </div>
            </div>

            <!-- VIEW: PROFILE -->
            <div id="view-profile" class="view hidden">
                <div class="view-title">Profile</div>
                <div class="card" style="text-align: center;">
                    <div style="width: 80px; height: 80px; background: var(--primary); border-radius: 50%; margin: 0 auto 15px; display: flex; align-items: center; justify-content: center; font-size: 40px;">👤</div>
                    <h2 id="prof-name">User Name</h2>
                    <p id="prof-email" style="font-size: 14px; color: var(--text-gray); font-family: var(--font-sans);">email@cylestria.com</p>
                </div>
                
                <div class="card">
                    <h3 style="margin-bottom: 15px;">Clinical History</h3>
                    <div style="font-family: var(--font-sans); font-size: 14px;">
                        <div style="padding: 10px 0; border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between;">
                            <span>Blood Type</span> <b>B+</b>
                        </div>
                        <div style="padding: 10px 0; border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between;">
                            <span>Weight</span> <b>68 kg</b>
                        </div>
                        <div style="padding: 10px 0; border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between;">
                            <span>Allergies</span> <b style="color: var(--danger);">Penicillin</b>
                        </div>
                    </div>
                </div>
                <button class="btn-main" style="background: var(--danger);" onclick="location.reload()">Log Out</button>
            </div>

        </div>
    </div>

    <!-- NAVIGATION -->
    <nav id="bottom-nav" class="hidden">
        <div class="nav-tab active" onclick="switchView('home', this)">🏠<span>HOME</span></div>
        <div class="nav-tab" onclick="switchView('chat', this)">💬<span>CHAT</span></div>
        <div class="nav-tab" onclick="switchView('labs', this)">🧬<span>LABS</span></div>
        <div class="nav-tab" onclick="switchView('meds', this)">💊<span>MEDS</span></div>
        <div class="nav-tab" onclick="switchView('profile', this)">👤<span>ME</span></div>
    </nav>

    <script>
        let isPro = false;
        let userName = "User";

        function toggleAuth(isSignup) {
            document.getElementById('login-view').classList.toggle('hidden', isSignup);
            document.getElementById('signup-view').classList.toggle('hidden', !isSignup);
        }

        function doAuth() {
            const email = document.getElementById('auth-email').value || document.getElementById('reg-email').value;
            const nameInput = document.getElementById('reg-name').value;
            
            if(!email) { alert("Please enter valid credentials"); return; }
            
            userName = nameInput || email.split('@')[0];
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app-content').classList.remove('hidden');
            document.getElementById('bottom-nav').classList.remove('hidden');
            
            document.getElementById('user-name').innerText = userName;
            document.getElementById('prof-name').innerText = userName;
            document.getElementById('prof-email').innerText = email;
        }

        function switchView(viewId, el) {
            document.querySelectorAll('.view').forEach(v => v.classList.add('hidden'));
            document.getElementById('view-' + viewId).classList.remove('hidden');
            
            document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
            el.classList.add('active');
            
            // Auto-scroll to top
            window.scrollTo(0, 0);
        }

        function upgradePro() {
            isPro = true;
            document.getElementById('pro-badge-main').classList.remove('hidden');
            alert("Congratulations! You are now a CELYSTRIA PRO member. All features unlocked.");
            switchView('home', document.querySelector('.nav-tab'));
        }

        function runAnalysis() {
            const input = document.getElementById('symptom-box').value;
            const box = document.getElementById('result-box');
            
            if(input.length < 5) return alert("Please describe more details.");

            box.classList.remove('hidden');
            box.style.opacity = '0.5';
            
            setTimeout(() => {
                box.style.opacity = '1';
                let res = { title: "Mild Fatigue", pill: "STABLE", class: "pill-ok", desc: "Common signs of exhaustion or dehydration detected.", med: "8 hours sleep, 3L Water, Vitamin C." };
                
                const q = input.toLowerCase();
                if(q.includes('chest') || q.includes('heart') || q.includes('breath')) {
                    res = { title: "Cardiac Alert", pill: "URGENT", class: "pill-warn", desc: "Indicators of potential cardiovascular distress. Emergency protocol suggested.", med: "Sit down. Do not take meds. Contact clinic immediately." };
                } else if(q.includes('fever') || q.includes('cough')) {
                    res = { title: "Viral Infection", pill: "CAUTION", class: "pill-warn", desc: "Patterns match upper respiratory viral strain. Monitor temperature.", med: "Paracetamol 500mg, Warm gargles, Hydration." };
                }

                document.getElementById('res-pill').innerText = res.pill;
                document.getElementById('res-pill').className = "status-pill " + res.class;
                document.getElementById('res-title').innerText = res.title;
                document.getElementById('res-desc').innerText = res.desc;
                document.getElementById('res-med').innerText = res.med;
                
                box.scrollIntoView({ behavior: 'smooth' });
            }, 1000);
        }

        function sendChat() {
            const input = document.getElementById('chat-input');
            const cont = document.getElementById('chat-container');
            if(!input.value) return;

            const userMsg = document.createElement('div');
            userMsg.className = "msg msg-user";
            userMsg.innerText = input.value;
            cont.appendChild(userMsg);

            const botMsg = document.createElement('div');
            botMsg.className = "msg msg-bot pulse";
            botMsg.innerText = "Processing clinical data...";
            cont.appendChild(botMsg);
            
            cont.scrollTop = cont.scrollHeight;
            const userQuery = input.value.toLowerCase();
            input.value = "";

            setTimeout(() => {
                botMsg.classList.remove('pulse');
                if(userQuery.includes('diet') || userQuery.includes('eat')) {
                    botMsg.innerText = "A balanced diet with 40% greens, 30% protein, and low sodium is recommended for your current profile.";
                } else if(userQuery.includes('headache')) {
                    botMsg.innerText = "Headaches can be caused by dehydration or screen strain. Try a 20-minute dark-room rest.";
                } else {
                    botMsg.innerText = "I have noted that in your health log. Would you like me to schedule a clinical reminder or analyze deeper?";
                }
                cont.scrollTop = cont.scrollHeight;
            }, 1500);
        }
    </script>
</body>
</html>

