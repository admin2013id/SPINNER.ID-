<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roda Sultan: Math Event</title>
    <style>
        /* --- CSS STYLING --- */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #2c3e50;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            color: white;
            padding-bottom: 50px;
            position: relative;
        }

        /* LOGIN SCREEN OVERLAY */
        #loginOverlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: #2c3e50;
            z-index: 9999;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }

        .login-box {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.5);
            text-align: center;
            width: 90%;
            max-width: 350px;
            color: #333;
        }

        .login-title {
            font-size: 1.5rem;
            font-weight: bold;
            margin-bottom: 20px;
            color: #2c3e50;
        }

        .input-group {
            margin-bottom: 15px;
            text-align: left;
        }

        .input-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            font-size: 0.9rem;
        }

        .input-group input {
            width: 100%;
            padding: 10px;
            border: 2px solid #bdc3c7;
            border-radius: 8px;
            box-sizing: border-box;
            font-size: 1rem;
        }

        .btn-login {
            width: 100%;
            padding: 12px;
            background-color: #27ae60;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
        }

        .btn-login:hover {
            background-color: #2ecc71;
        }

        .login-error {
            color: #e74c3c;
            font-size: 0.9rem;
            margin-top: 10px;
            display: none;
            font-weight: bold;
        }

        /* GAME CONTENT (Hidden by default) */
        #gameContent {
            display: none; /* Hidden until login */
            width: 100%;
            flex-direction: column;
            align-items: center;
        }

        /* TOMBOL EVENT KIRI ATAS */
        .event-btn-top {
            position: absolute;
            top: 20px;
            left: 20px;
            background: linear-gradient(45deg, #8e44ad, #9b59b6);
            color: white;
            border: 2px solid #fff;
            padding: 10px 15px;
            border-radius: 20px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 0 10px #8e44ad;
            z-index: 50;
            transition: all 0.3s;
        }
        .event-btn-top.active {
            background: #e74c3c;
            box-shadow: 0 0 10px #e74c3c;
        }
        .event-btn-top:disabled {
            background: #7f8c8d;
            box-shadow: none;
            cursor: not-allowed;
            opacity: 0.7;
        }

        h1 {
            margin-top: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
            color: #f1c40f;
            text-align: center;
        }

        /* Header Saldo & Tools */
        .header-section {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            margin-bottom: 20px;
            margin-top: 40px; 
        }

        .currency-container {
            display: flex;
            gap: 15px;
        }

        .balance-box {
            background: rgba(0,0,0,0.5);
            padding: 8px 15px;
            border-radius: 15px;
            border: 1px solid #7f8c8d;
            text-align: center;
            min-width: 100px;
        }

        .balance-label { font-size: 0.8rem; color: #bdc3c7; }
        .balance-value { font-size: 1.1rem; font-weight: bold; }
        .diamond-text { color: #3498db; }
        .token-text { color: #f1c40f; }
        
        /* Style Khusus Admin */
        .admin-mode .balance-value {
            color: #e74c3c;
            text-shadow: 0 0 5px #e74c3c;
            animation: glow 1.5s infinite alternate;
        }
        @keyframes glow { from { text-shadow: 0 0 5px #e74c3c; } to { text-shadow: 0 0 20px #ff0000; } }

        .action-buttons {
            display: flex;
            gap: 10px;
        }

        .tool-btn {
            background-color: #27ae60;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            font-weight: bold;
            font-size: 0.9rem;
            box-shadow: 0 3px 0 #219150;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        .tool-btn.bag { background-color: #e67e22; box-shadow: 0 3px 0 #d35400; }
        .tool-btn:active { transform: translateY(3px); box-shadow: none; }

        .spin-info {
            font-size: 0.9rem;
            color: #ecf0f1;
            background: rgba(231, 76, 60, 0.8);
            padding: 5px 15px;
            border-radius: 10px;
            margin-bottom: 10px;
            transition: background 0.3s;
        }

        /* TIMER COUNTDOWN */
        .timer-container {
            background: rgba(0,0,0,0.6);
            border: 1px solid #f1c40f;
            padding: 10px 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            text-align: center;
            font-family: monospace;
            font-size: 1.2rem;
            color: #f1c40f;
            box-shadow: 0 0 10px rgba(241, 196, 15, 0.2);
        }
        .timer-label { font-size: 0.8rem; color: #bdc3c7; display: block; margin-bottom: 5px; font-family: sans-serif; }

        /* Bar Misi (Hanya muncul saat Event) */
        .mission-bar {
            background-color: rgba(142, 68, 173, 0.3);
            padding: 10px 20px;
            border-radius: 15px;
            margin-bottom: 20px;
            text-align: center;
            border: 1px solid #9b59b6;
            width: 90%;
            max-width: 400px;
            display: none; 
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }

        .mission-text { font-size: 0.9rem; margin-bottom: 5px; color: #ecf0f1; }
        .progress-container { width: 100%; background-color: #34495e; border-radius: 10px; height: 10px; overflow: hidden; }
        .progress-fill { height: 100%; background-color: #e74c3c; width: 0%; transition: width 0.3s ease; }

        .wheel-container {
            position: relative;
            width: 350px;
            height: 350px;
            margin: 10px 0;
        }

        .pointer {
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
            width: 40px;
            height: 40px;
            background-color: #e74c3c;
            clip-path: polygon(50% 100%, 0 0, 100% 0);
            z-index: 10;
            filter: drop-shadow(0 4px 2px rgba(0,0,0,0.3));
        }

        .wheel {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            border: 10px solid #fff;
            box-sizing: border-box;
            position: relative;
            transition: transform 5s cubic-bezier(0.25, 0.1, 0.25, 1);
            overflow: hidden;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
        }

        .wheel-label {
            position: absolute;
            top: 50%;
            left: 50%;
            transform-origin: 0 0; 
            color: white;
            font-weight: bold;
            font-size: 12px;
            text-shadow: 1px 1px 2px black;
            width: 50%; 
            padding-left: 50px; 
            box-sizing: border-box;
            text-align: right;
            pointer-events: none;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 20px;
            z-index: 2;
        }

        .prize-overlay {
            position: absolute;
            top: 50%;
            left: 50%;
            transform-origin: 0 0;
            width: 50%;
            height: 50%;
            background: rgba(255, 0, 0, 0.3);
            border: 2px solid red;
            box-sizing: border-box;
            pointer-events: none;
            z-index: 1;
            clip-path: polygon(0 45%, 100% 0, 100% 100%, 0 55%);
        }

        .center-circle {
            position: absolute;
            width: 50px;
            height: 50px;
            background-color: white;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 5;
            box-shadow: 0 0 5px rgba(0,0,0,0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #333;
            border: 4px solid #ecf0f1;
        }

        #spinBtn {
            padding: 15px 40px;
            font-size: 1.2rem;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 5px 0 #2980b9;
            transition: all 0.1s;
            margin-bottom: 20px;
        }
        #spinBtn:active { transform: translateY(5px); box-shadow: none; }
        #spinBtn:disabled { background-color: #95a5a6; cursor: not-allowed; box-shadow: none; }

        .redeem-section {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 30px;
            text-align: center;
            width: 90%;
            max-width: 400px;
            border: 1px solid #7f8c8d;
        }
        .redeem-input { padding: 10px; width: 60%; border-radius: 5px; border: none; text-transform: uppercase; }
        .redeem-btn { padding: 10px 15px; background-color: #27ae60; color: white; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; }
        #result { margin-top: 10px; font-size: 1.2rem; font-weight: bold; color: #f1c40f; min-height: 30px; text-align: center; }
        #redeemMsg { margin-top: 10px; font-size: 0.9rem; font-weight: bold; }

        /* SHOP */
        .shop-container {
            width: 90%;
            max-width: 800px;
            background-color: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            padding: 20px;
            border: 1px solid #f1c40f;
            margin-top: 20px;
        }
        .shop-title { text-align: center; margin-bottom: 20px; color: #f1c40f; text-transform: uppercase; letter-spacing: 2px; }
        .shop-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; }
        .shop-item { background-color: #ecf0f1; color: #2c3e50; padding: 15px; border-radius: 10px; text-align: center; display: flex; flex-direction: column; justify-content: space-between; transition: transform 0.2s; }
        .shop-item:hover { transform: scale(1.03); }
        .item-name { font-weight: bold; margin-bottom: 5px; font-size: 0.9rem; height: 40px; display: flex; align-items: center; justify-content: center; }
        .item-price { color: #d35400; font-size: 0.85rem; margin-bottom: 10px; font-weight: bold; }
        .buy-btn { background-color: #27ae60; color: white; border: none; padding: 8px; border-radius: 5px; cursor: pointer; font-size: 0.9rem; width: 100%; }
        .buy-btn.disabled { background-color: #95a5a6; cursor: not-allowed; opacity: 0.7; }

        /* MODALS */
        .modal { display: none; position: fixed; z-index: 100; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.85); justify-content: center; align-items: center; }
        .modal-content { background-color: #fefefe; color: #333; padding: 25px; border-radius: 15px; width: 90%; max-width: 400px; text-align: center; position: relative; max-height: 90vh; overflow-y: auto; }
        .close-modal { position: absolute; top: 10px; right: 15px; color: #aaa; font-size: 28px; font-weight: bold; cursor: pointer; }
        
        /* Tab Navigation in Modal */
        .tab-nav {
            display: flex;
            justify-content: center;
            margin-bottom: 15px;
            border-bottom: 2px solid #eee;
        }
        .tab-btn {
            flex: 1;
            padding: 10px;
            border: none;
            background: none;
            font-weight: bold;
            cursor: pointer;
            color: #7f8c8d;
            transition: 0.3s;
        }
        .tab-btn.active {
            color: #2c3e50;
            border-bottom: 3px solid #2c3e50;
        }

        /* Styles for Real Top Up */
        .payment-method {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: #f8f9fa;
            border: 1px solid #ddd;
            padding: 10px 15px;
            margin: 8px 0;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.2s;
        }
        .payment-method:hover, .payment-method.selected {
            border-color: #3498db;
            background: #ebf5fb;
        }
        .payment-logo { font-weight: bold; color: #2c3e50; }
        .pay-btn {
            background-color: #2980b9;
            color: white;
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: bold;
            margin-top: 15px;
            cursor: pointer;
        }
        .pay-btn:disabled { background-color: #bdc3c7; }

        .btn-serious { background-color: #e67e22; color: white; border: none; padding: 12px 25px; border-radius: 8px; font-size: 1rem; cursor: pointer; margin-top: 15px; width: 100%; font-weight: bold; }
        .btn-final-buy { background-color: #c0392b; color: white; border: none; padding: 15px 40px; border-radius: 50px; font-size: 2rem; font-weight: 900; cursor: pointer; margin-top: 20px; box-shadow: 0 5px 0 #962d22; text-transform: uppercase; }
        .btn-final-buy:active { transform: translateY(5px); box-shadow: none; }

        /* INVENTORY / TAS STYLE */
        .inventory-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-top: 20px;
        }
        .inventory-slot {
            background: #ecf0f1;
            border-radius: 10px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            align-items: center;
            cursor: pointer;
            transition: 0.2s;
            border: 2px solid transparent;
        }
        .inventory-slot:hover {
            border-color: #f1c40f;
            transform: scale(1.05);
        }
        .inv-icon { font-size: 3rem; margin-bottom: 5px; }
        .inv-count { 
            background: #e74c3c; 
            color: white; 
            padding: 2px 8px; 
            border-radius: 10px; 
            font-size: 0.8rem; 
            font-weight: bold;
        }
        .inv-name { font-size: 0.8rem; color: #2c3e50; font-weight: bold; margin-top: 5px; }
        .empty-msg { color: #7f8c8d; font-style: italic; margin-top: 20px; }

        .btn-open-item {
            background: linear-gradient(45deg, #f1c40f, #f39c12);
            border: none;
            padding: 10px 30px;
            border-radius: 50px;
            color: #2c3e50;
            font-weight: 900;
            font-size: 1.2rem;
            cursor: pointer;
            box-shadow: 0 5px 0 #d35400;
            margin-top: 15px;
        }
        .btn-open-item:active { transform: translateY(5px); box-shadow: none; }

        /* MATH EVENT STYLES */
        .math-header {
            display: flex;
            justify-content: space-between;
            width: 100%;
            margin-bottom: 20px;
            font-size: 1.2rem;
            font-weight: bold;
        }
        .hearts-display { color: #e74c3c; }
        .math-question {
            font-size: 3rem;
            font-weight: bold;
            margin: 30px 0;
            color: #2c3e50;
            background: #ecf0f1;
            padding: 20px;
            border-radius: 15px;
            width: 100%;
            text-align: center;
        }
        .math-input {
            width: 100%;
            padding: 15px;
            font-size: 1.5rem;
            text-align: center;
            border: 2px solid #bdc3c7;
            border-radius: 10px;
            margin-bottom: 20px;
        }
        .btn-submit-math {
            width: 100%;
            padding: 15px;
            background-color: #27ae60;
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
        }
        .btn-buy-life {
            background-color: #e67e22;
            color: white;
            border: none;
            padding: 15px;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
            margin-top: 10px;
        }

    </style>
</head>
<body>

    <!-- LOGIN OVERLAY -->
    <div id="loginOverlay">
        <div class="login-box">
            <div class="login-title">🔐 LOGIN RODA SULTAN</div>
            <div class="input-group">
                <label>Username</label>
                <input type="text" id="loginUser" placeholder="Masukkan Username...">
            </div>
            <div class="input-group">
                <label>Password</label>
                <input type="password" id="loginPass" placeholder="Masukkan Password...">
            </div>
            <button class="btn-login" onclick="attemptLogin()">MASUK GAME</button>
            <div id="loginError" class="login-error">Username atau Password Salah!</div>
        </div>
    </div>

    <!-- GAME CONTENT (Awalnya Tersembunyi) -->
    <div id="gameContent">
        <!-- TOMBOL EVENT -->
        <button id="eventBtn" class="event-btn-top" onclick="toggleEvent()" disabled>🎲 Ikuti Event</button>

        <h1>Roda Sultan & Inventory</h1>

        <!-- Header Saldo & Tools -->
        <div class="header-section">
            <div class="currency-container">
                <div class="balance-box">
                    <div class="balance-label">💎 Diamond</div>
                    <div class="balance-value diamond-text" id="diamondCount">0</div>
                </div>
                <div class="balance-box">
                    <div class="balance-label">🪙 Token</div>
                    <div class="balance-value token-text" id="tokenCount">0</div>
                </div>
            </div>
            <div class="action-buttons">
                <button class="tool-btn" onclick="openUnifiedTopUp()">+ Top Up</button>
                <button class="tool-btn bag" onclick="openInventory()">🎒 Tas Penyimpanan</button>
            </div>
        </div>

        <!-- Info Spin -->
        <div id="spinStatus" class="spin-info">Biaya Spin: 50.000 Token</div>

        <!-- TIMER COUNTDOWN -->
        <div class="timer-container">
            <span class="timer-label" id="timerLabel">Menuju Event Berikutnya...</span>
            <span id="countdownDisplay">00:00:00</span>
        </div>

        <!-- Bar Misi (Hanya Event) -->
        <div id="missionBarContainer" class="mission-bar">
            <div class="mission-text">🔥 MISI EVENT: <span id="spinCountDisplay">0</span>/15 Spin (Hadiah: Misteri Box)</div>
            <div class="progress-container">
                <div class="progress-fill" id="missionProgress"></div>
            </div>
        </div>

        <div class="wheel-container">
            <div class="pointer"></div>
            <div class="wheel" id="wheel"></div>
            <div class="center-circle">SPIN</div>
        </div>

        <button id="spinBtn" onclick="spinWheel()">PUTAR RODA!</button>
        <div id="result"></div>

        <!-- Bagian Redeem Code -->
        <div class="redeem-section">
            <h3>Punya Kode Redeem?</h3>
            <input type="text" id="redeemInput" class="redeem-input" placeholder="KODE...">
            <button class="redeem-btn" onclick="claimRedeem()">CLAIM</button>
            <div id="redeemMsg"></div>
        </div>

        <!-- BAGIAN TOKO -->
        <div class="shop-container">
            <h2 class="shop-title">🛒 Tukar Token Hadiah</h2>
            <div class="shop-grid">
                <div class="shop-item">
                    <div class="item-name">AKUN FF SULTAN</div>
                    <div class="item-price">100.000.000 Token</div>
                    <button class="buy-btn" onclick="initiateBuy('AKUN FF SULTAN', 100000000)">Beli</button>
                </div>
                <div class="shop-item">
                    <div class="item-name">AKUN ML SULTAN</div>
                    <div class="item-price">100.000.000 Token</div>
                    <button class="buy-btn" onclick="initiateBuy('AKUN ML SULTAN', 100000000)">Beli</button>
                </div>
                <div class="shop-item">
                    <div class="item-name">AKUN PUBG SULTAN</div>
                    <div class="item-price">100.000.000 Token</div>
                    <button class="buy-btn" onclick="initiateBuy('AKUN PUBG SULTAN', 100000000)">Beli</button>
                </div>
                <div class="shop-item">
                    <div class="item-name">VOUCHER</div>
                    <div class="item-price">100.000 Token</div>
                    <button class="buy-btn" onclick="initiateBuy('VOUCHER', 100000)">Beli</button>
                </div>
            </div>
        </div>

        <!-- MODAL UNIFIED TOP UP (GABUNGAN) -->
        <div id="unifiedTopUpModal" class="modal">
            <div class="modal-content">
                <span class="close-modal" onclick="closeAllModals()">&times;</span>
                <h2 style="color:#2c3e50;">Top Up Center</h2>
                
                <div class="tab-nav">
                    <button class="tab-btn active" onclick="switchTab('token')">Top Up Token</button>
                    <button class="tab-btn" onclick="switchTab('diamond')">Top Up Diamond</button>
                </div>

                <!-- TAB TOKEN -->
                <div id="tab-token" class="tab-content">
                    <p>Beli Token Langsung (Rupiah):</p>
                    <div class="payment-method" onclick="selectDirectPackage(1000000, 10000, this)">
                        <span class="payment-logo">1.000.000 Token</span>
                        <span style="font-weight:bold;">Rp 10.000</span>
                    </div>
                    <div class="payment-method" onclick="selectDirectPackage(5000000, 45000, this)">
                        <span class="payment-logo">5.000.000 Token</span>
                        <span style="font-weight:bold;">Rp 45.000</span>
                    </div>
                    <div class="payment-method" onclick="selectDirectPackage(10000000, 90000, this)">
                        <span class="payment-logo">10.000.000 Token</span>
                        <span style="font-weight:bold;">Rp 90.000</span>
                    </div>
                </div>

                <!-- TAB DIAMOND -->
                <div id="tab-diamond" class="tab-content" style="display:none;">
                    <p>Beli Diamond (Rupiah):</p>
                    <div class="payment-method" onclick="selectDiamondPackage(50, 10000, this)">
                        <span class="payment-logo">50 💎 Diamond</span>
                        <span style="font-weight:bold;">Rp 10.000</span>
                    </div>
                    <div class="payment-method" onclick="selectDiamondPackage(100, 20000, this)">
                        <span class="payment-logo">100 💎 Diamond</span>
                        <span style="font-weight:bold;">Rp 20.000</span>
                    </div>
                    <div class="payment-method" onclick="selectDiamondPackage(250, 50000, this)">
                        <span class="payment-logo">250 💎 Diamond</span>
                        <span style="font-weight:bold;">Rp 50.000</span>
                    </div>
                    
                    <hr style="margin: 15px 0; border: 0; border-top: 1px solid #eee;">
                    <p style="font-size:0.9rem; color:#7f8c8d;">Atau tukar Diamond ke Token di menu Tas/Money Changer.</p>
                </div>

                <hr style="margin: 15px 0; border: 0; border-top: 1px solid #eee;">
                
                <p>Pilih Metode Pembayaran:</p>
                <div class="payment-method" onclick="selectPayment('DANA', this)">
                    <span class="payment-logo">DANA</span>
                    <span>💳</span>
                </div>
                <div class="payment-method" onclick="selectPayment('OVO', this)">
                    <span class="payment-logo">OVO</span>
                    <span>💳</span>
                </div>
                <div class="payment-method" onclick="selectPayment('QRIS', this)">
                    <span class="payment-logo">QRIS (All Payment)</span>
                    <span>📷</span>
                </div>

                <button id="payNowBtn" class="pay-btn" disabled onclick="processPayment()">Bayar Sekarang</button>
            </div>
        </div>

        <!-- MODAL EXCHANGE (TUKAR DIAMOND KE TOKEN) -->
        <div id="exchangeModal" class="modal">
            <div class="modal-content">
                <span class="close-modal" onclick="closeAllModals()">&times;</span>
                <h2 style="color:#2c3e50;">Money Changer</h2>
                <p>Kurs: 50 Diamond = 1.000.000 Token</p>
                <p>Saldo Diamond: <span id="modalDiamondBalance" style="font-weight:bold; color:#3498db;">0</span></p>
                
                <div class="payment-method" onclick="processDiamondExchange(1000000, 50)">
                    <span class="payment-logo">1.000.000 Token</span>
                    <span style="color:#e74c3c; font-weight:bold;">-50 💎</span>
                </div>
                <div class="payment-method" onclick="processDiamondExchange(5000000, 250)">
                    <span class="payment-logo">5.000.000 Token</span>
                    <span style="color:#e74c3c; font-weight:bold;">-250 💎</span>
                </div>
            </div>
        </div>

        <!-- MODAL INVENTORY / TAS -->
        <div id="inventoryModal" class="modal">
            <div class="modal-content">
                <span class="close-modal" onclick="closeAllModals()">&times;</span>
                <h2 style="color:#e67e22;">🎒 Tas Penyimpanan</h2>
                <p>Item yang bisa dibuka:</p>
                
                <div id="inventoryGrid" class="inventory-grid">
                    <!-- Item akan digenerate oleh JS -->
                </div>
            </div>
        </div>

        <!-- MODAL PEMILIHAN ITEM (PRE-OPEN) -->
        <div id="itemSelectModal" class="modal">
            <div class="modal-content">
                <span class="close-modal" onclick="closeAllModals()">&times;</span>
                <h2 style="color:#8e44ad;">MISTERI BOX</h2>
                <div style="font-size: 5rem;">🎁</div>
                <p>Anda memiliki item ini di tas.</p>
                <p>Ingin membukanya sekarang?</p>
                <button class="btn-open-item" onclick="confirmOpenItem()">BUKA</button>
            </div>
        </div>

        <!-- MODAL HASIL BUKA ITEM -->
        <div id="boxResultModal" class="modal">
            <div class="modal-content">
                <h2 id="boxResultTitle" style="margin-bottom:10px;">HADIAH</h2>
                <div id="boxResultIcon" style="font-size:4rem; margin:10px 0;">🎉</div>
                <p id="boxResultText" style="font-size:1.2rem; font-weight:bold;"></p>
                <button class="btn-serious" onclick="closeAllModals()">Ambil Hadiah</button>
            </div>
        </div>

        <!-- MODAL KONFIRMASI BELI ITEM TOKO -->
        <div id="seriousModal" class="modal">
            <div class="modal-content">
                <span class="close-modal" onclick="closeAllModals()">&times;</span>
                <h3 class="confirm-title">Konfirmasi Penukaran</h3>
                <div id="confirmItemName" class="confirm-item-name">ITEM</div>
                <p>Harga: <span id="confirmItemPrice">0</span> Token</p>
                <button class="btn-serious" onclick="goToFinalStep()">Saya sudah yakin</button>
            </div>
        </div>
        <div id="finalBuyModal" class="modal">
            <div class="modal-content" style="background: transparent; box-shadow: none; border: none;">
                <h2 style="color: white; text-shadow: 0 2px 4px black;">TEKAN UNTUK MEMBELI</h2>
                <button class="btn-final-buy" onclick="executePurchase()">BELI</button>
            </div>
        </div>

        <!-- MODAL EVENT MATEMATIKA -->
        <div id="mathEventModal" class="modal">
            <div class="modal-content">
                <span class="close-modal" onclick="exitMathEvent()">&times;</span>
                <h2 style="color:#8e44ad;">🧠 Event Matematika</h2>
                <div class="math-header">
                    <span id="mathHearts" class="hearts-display">❤️❤️❤️</span>
                    <span style="color:#27ae60;">Hadiah: 500rb Token</span>
                </div>
                
                <div id="mathGameArea">
                    <div class="math-question" id="mathQuestion">5 x 5 = ?</div>
                    <input type="number" id="mathAnswer" class="math-input" placeholder="Jawaban...">
                    <button class="btn-submit-math" onclick="checkMathAnswer()">JAWAB</button>
                </div>

                <div id="mathGameOver" style="display:none;">
                    <h3 style="color:red;">NYAWA HABIS! 😭</h3>
                    <p>Event otomatis berakhir.</p>
                    <p>Ingin lanjut? Beli Nyawa:</p>
                    <button class="btn-buy-life" onclick="buyLife()">Beli 1 Nyawa (3jt Token)</button>
                </div>
            </div>
        </div>

    </div>

    <script>
        // --- KONFIGURASI ---
        const ADMIN_WA_NUMBER = "6285882382854"; // Nomor WA: 0858-8238-2854
        const SPIN_COST = 50000; // Biaya Spin 50.000
        const MISSION_TARGET = 15; 
        
        // State
        let userTokens = 5000000; 
        let userDiamonds = 0; 
        let spinCounter = 0;
        let mysteryBoxes = 0; 
        let isEventActive = false;
        let isAdminMode = false; // Status Admin
        
        // Math Event State
        let mathLives = 3;
        let currentMathAnswer = 0;

        let pendingItemName = "";
        let pendingItemPrice = 0;
        let currentRotation = 0;
        let isSpinning = false;

        // Variables for Top Up Selection
        let selectedGemAmount = 0;
        let selectedTokenAmount = 0;
        let selectedPrice = 0;
        let selectedPaymentMethod = "";
        let currentTopUpType = ""; // 'token' or 'diamond'

        // Data Roda
        const prizes = [
            { text: "Zonk", color: "#e74c3c", isPrize: false },      
            { text: "100rb Token", color: "#3498db", isPrize: true }, 
            { text: "Coba Lagi", color: "#95a5a6", isPrize: false }, 
            { text: "Voucher", color: "#9b59b6", isPrize: true },   
            { text: "AKUN PUBG", color: "#f1c40f", isPrize: true }, 
            { text: "Rp 50.000", color: "#2ecc71", isPrize: true } 
        ];

        const weightedWinners = [
            { text: "Zonk", weight: 30 },
            { text: "100rb Token", weight: 30 },
            { text: "Coba Lagi", weight: 20 },
            { text: "Voucher", weight: 10 }
        ];

        const redeemCodes = {
            "AZFER.ID": { type: 'token', amount: 1000000, claimed: false },
            "SPIRIT_GO": { type: 'token', amount: 5000000, claimed: false },
            "FREEGEMS": { type: 'diamond', amount: 100, claimed: false },
            "ADMIN_AZFER": { type: 'admin', amount: 0, claimed: false } // Kode Rahasia Owner
        };

        // Elemen DOM
        const els = {
            token: document.getElementById('tokenCount'),
            diamond: document.getElementById('diamondCount'),
            spinBtn: document.getElementById('spinBtn'),
            result: document.getElementById('result'),
            wheel: document.getElementById('wheel'),
            missionText: document.getElementById('spinCountDisplay'),
            missionBar: document.getElementById('missionProgress'),
            missionContainer: document.getElementById('missionBarContainer'),
            spinStatus: document.getElementById('spinStatus'),
            modalDiamond: document.getElementById('modalDiamondBalance'),
            eventBtn: document.getElementById('eventBtn'),
            timerLabel: document.getElementById('timerLabel'),
            countdownDisplay: document.getElementById('countdownDisplay')
        };

        // --- SISTEM LOGIN ---
        function attemptLogin() {
            const u = document.getElementById('loginUser').value.trim();
            const p = document.getElementById('loginPass').value.trim();
            const err = document.getElementById('loginError');

            // Cek Kredensial
            if (u === "AKUN_GACHA1" && p === "GACHA1") {
                // Login Sukses
                document.getElementById('loginOverlay').style.display = 'none';
                document.getElementById('gameContent').style.display = 'flex';
                
                // Init Game setelah login
                drawWheel();
                initExchangeButton(); 
                updateUI();
            } else {
                // Login Gagal
                err.style.display = 'block';
                // Shake effect
                const box = document.querySelector('.login-box');
                box.style.transform = 'translateX(10px)';
                setTimeout(() => box.style.transform = 'translateX(-10px)', 100);
                setTimeout(() => box.style.transform = 'translateX(0)', 200);
            }
        }

        function updateUI() {
            // Logika Tampilan Unlimited untuk Admin
            if (isAdminMode) {
                els.token.innerText = "∞ UNLIMITED";
                els.diamond.innerText = "∞ UNLIMITED";
                els.modalDiamond.innerText = "∞ UNLIMITED";
                document.body.classList.add('admin-mode');
            } else {
                els.token.innerText = userTokens.toLocaleString('id-ID');
                els.diamond.innerText = userDiamonds.toLocaleString('id-ID');
                els.modalDiamond.innerText = userDiamonds.toLocaleString('id-ID');
                document.body.classList.remove('admin-mode');
            }
            
            // Update Mission (Only visual if active)
            els.missionText.innerText = spinCounter;
            els.missionBar.style.width = (spinCounter / MISSION_TARGET * 100) + "%";
            
            // Show/Hide Mission Bar based on Event Status
            if (isEventActive) {
                els.missionContainer.style.display = "block";
            } else {
                els.missionContainer.style.display = "none";
            }

            // Update Spin Status & Button
            if (isEventActive) {
                els.spinStatus.innerText = "EVENT AKTIF: SPIN GRATIS! 🎉";
                els.spinStatus.style.background = "#8e44ad";
                els.spinBtn.disabled = false;
                els.spinBtn.innerText = "PUTAR RODA (GRATIS)";
                els.spinBtn.style.backgroundColor = "#8e44ad";
            } else {
                els.spinStatus.innerText = "Biaya Spin: 50.000 Token";
                els.spinStatus.style.background = "rgba(231, 76, 60, 0.8)";
                
                // Jika Admin, selalu bisa spin
                if (isAdminMode) {
                     els.spinBtn.disabled = false;
                     els.spinBtn.innerText = "PUTAR RODA (ADMIN)";
                     els.spinBtn.style.backgroundColor = "#e74c3c";
                } else if (userTokens < SPIN_COST) {
                    els.spinBtn.disabled = true;
                    els.spinBtn.innerText = "Saldo Kurang";
                    els.spinBtn.style.backgroundColor = "#95a5a6";
                } else {
                    if (!isSpinning) {
                        els.spinBtn.disabled = false;
                        els.spinBtn.innerText = "PUTAR (-50rb)";
                        els.spinBtn.style.backgroundColor = "#3498db";
                    }
                }
            }
            
            checkShopAvailability();
        }

        function drawWheel() {
            const numSegments = prizes.length;
            const segmentAngle = 360 / numSegments;
            let gradientString = 'conic-gradient(';
            prizes.forEach((prize, index) => {
                const startAngle = index * segmentAngle;
                const endAngle = (index + 1) * segmentAngle;
                gradientString += `${prize.color} ${startAngle}deg ${endAngle}deg,`;
            });
            gradientString = gradientString.slice(0, -1) + ')';
            els.wheel.style.background = gradientString;

            document.querySelectorAll('.wheel-label, .prize-overlay').forEach(el => el.remove());

            prizes.forEach((prize, index) => {
                const label = document.createElement('div');
                label.className = 'wheel-label';
                label.innerText = prize.text;
                label.style.transform = `rotate(${(index * segmentAngle) + (segmentAngle / 2)}deg)`;
                els.wheel.appendChild(label);

                if (prize.isPrize) {
                    const overlay = document.createElement('div');
                    overlay.className = 'prize-overlay';
                    overlay.style.transform = `rotate(${index * segmentAngle}deg)`;
                    els.wheel.appendChild(overlay);
                }
            });
        }

        // --- LOGIKA SPIN ---
        function spinWheel() {
            // Admin tidak perlu bayar
            if (!isAdminMode && !isEventActive && userTokens < SPIN_COST) {
                alert("Saldo tidak cukup! Silakan Top Up.");
                openUnifiedTopUp();
                return;
            }

            if (isSpinning) return;
            
            // Potong saldo jika bukan event dan bukan admin
            if (!isEventActive && !isAdminMode) {
                userTokens -= SPIN_COST;
            }

            // Misi HANYA berjalan jika Event Aktif
            if (isEventActive) {
                spinCounter++;
                // Cek Misi
                if (spinCounter >= MISSION_TARGET) {
                    mysteryBoxes++; // Masuk ke Tas
                    spinCounter = 0; // Reset progress misi
                    setTimeout(() => {
                        alert(`MISI SELESAI! 1 Misteri Box telah masuk ke Tas Penyimpanan 🎒`);
                        renderInventory(); 
                    }, 500);
                }
            }

            isSpinning = true;
            updateUI();
            els.result.innerText = "Sedang memutar...";
            els.result.style.color = "#fff";

            const winnerText = getWeightedWinner();
            const targetIndex = prizes.findIndex(p => p.text === winnerText); 
            
            const segmentAngle = 360 / prizes.length;
            const stopAngle = (targetIndex * segmentAngle) + (segmentAngle / 2);
            const extraSpins = 360 * 5; 
            const totalRotation = extraSpins + (360 - stopAngle) + (Math.random() * 20 - 10);

            currentRotation += totalRotation;
            els.wheel.style.transform = `rotate(-${currentRotation}deg)`;

            setTimeout(() => {
                finishSpin(winnerText);
                isSpinning = false;
                updateUI();
            }, 5000);
        }

        function getWeightedWinner() {
            let random = Math.random() * 100; 
            let currentSum = 0;
            for (let item of weightedWinners) {
                currentSum += item.weight;
                if (random <= currentSum) return item.text;
            }
            return "Zonk"; 
        }

        function finishSpin(text) {
            if (text === "100rb Token") {
                if(!isAdminMode) userTokens += 100000;
                els.result.innerText = `+100.000 Token`;
            } else {
                els.result.innerText = `Hasil: ${text}`;
            }
            els.result.style.color = (text === "Zonk" || text === "Coba Lagi") ? "#e74c3c" : "#f1c40f";
        }

        // --- LOGIKA EVENT & TIMER (DAILY 13:30 - 00:00) ---
        function checkEventTime() {
            const now = new Date();
            const currentHour = now.getHours();
            const currentMin = now.getMinutes();
            
            // Konfigurasi Waktu Harian
            const START_HOUR = 13;
            const START_MIN = 30;
            
            const END_HOUR = 24; // 00:00
            
            const startTimeVal = (START_HOUR * 60) + START_MIN; // 13:30 = 810 menit
            const endTimeVal = (END_HOUR * 60); // 24:00 = 1440 menit
            const currentTimeVal = (currentHour * 60) + currentMin;
            
            let isEventTime = false;
            let nextEventTime = new Date();
            let label = "";

            // Logika: Jika waktu sekarang >= 13:30 DAN < 24:00
            if (currentTimeVal >= startTimeVal && currentTimeVal < endTimeVal) {
                isEventTime = true;
                label = "Event Berakhir Dalam:";
                
                // Hitung mundur ke 24:00 (Tengah Malam)
                nextEventTime.setHours(24, 0, 0, 0);
            } else {
                isEventTime = false;
                label = "Event Dimulai Dalam:";
                
                // Hitung mundur ke 13:30 hari ini
                nextEventTime.setHours(START_HOUR, START_MIN, 0, 0);
                
                // Jika sekarang sudah lewat 13:30 (artinya sudah tengah malam/lewat 00:00), targetnya besok jam 13:30
                if (currentTimeVal >= startTimeVal) {
                     nextEventTime.setDate(nextEventTime.getDate() + 1);
                }
            }

            // Update UI Button
            if (isEventTime) {
                els.eventBtn.disabled = false;
                if (!isEventActive) {
                    els.eventBtn.innerText = "🎲 Ikuti Event";
                    els.eventBtn.classList.remove('active');
                }
            } else {
                els.eventBtn.disabled = true;
                els.eventBtn.innerText = "🔒 Event Tutup";
                els.eventBtn.classList.remove('active');
                // Force exit event if time runs out
                if (isEventActive) toggleEvent();
            }

            // Update Countdown Text
            els.timerLabel.innerText = label;
            
            const diff = nextEventTime - now;
            if (diff > 0) {
                const h = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                const s = Math.floor((diff % (1000 * 60)) / 1000);
                els.countdownDisplay.innerText = 
                    (h < 10 ? "0" + h : h) + ":" + 
                    (m < 10 ? "0" + m : m) + ":" + 
                    (s < 10 ? "0" + s : s);
            } else {
                els.countdownDisplay.innerText = "00:00:00";
            }
        }

        // Jalankan timer setiap detik
        setInterval(checkEventTime, 1000);
        checkEventTime(); // Initial call

        function toggleEvent() {
            // Double check time logic inside toggle just to be safe
            const now = new Date();
            const currentHour = now.getHours();
            const currentMin = now.getMinutes();
            const currentTimeVal = (currentHour * 60) + currentMin;
            const startTimeVal = (13 * 60) + 30;
            
            // Hanya boleh join jika jam >= 13:30 dan < 24:00
            if (currentTimeVal < startTimeVal || currentTimeVal >= 1440) {
                alert("Event sedang tutup! Tunggu jadwal harian (13:30 - 00:00).");
                return;
            }

            // Buka Modal Matematika alih-alih langsung aktif
            openMathEvent();
        }

        // --- LOGIKA MATEMATIKA EVENT ---
        function openMathEvent() {
            if(mathLives <= 0) {
                // Jika nyawa 0, tampilkan opsi beli nyawa dulu
                document.getElementById('mathGameArea').style.display = 'none';
                document.getElementById('mathGameOver').style.display = 'block';
            } else {
                document.getElementById('mathGameArea').style.display = 'block';
                document.getElementById('mathGameOver').style.display = 'none';
                generateMathQuestion();
            }
            document.getElementById('mathEventModal').style.display = 'flex';
            updateMathUI();
        }

        function generateMathQuestion() {
            const isMultiplication = Math.random() > 0.5;
            let a, b, symbol;

            if (isMultiplication) {
                // Perkalian: 2-12
                a = Math.floor(Math.random() * 11) + 2;
                b = Math.floor(Math.random() * 11) + 2;
                currentMathAnswer = a * b;
                symbol = "x";
            } else {
                // Pembagian: Hasil bulat
                b = Math.floor(Math.random() * 10) + 2; // Pembagi 2-11
                currentMathAnswer = Math.floor(Math.random() * 10) + 2; // Jawaban 2-11
                a = b * currentMathAnswer; // Angka yang dibagi
                symbol = ":";
            }

            document.getElementById('mathQuestion').innerText = `${a} ${symbol} ${b} = ?`;
            document.getElementById('mathAnswer').value = "";
            document.getElementById('mathAnswer').focus();
        }

        function checkMathAnswer() {
            const userAns = parseInt(document.getElementById('mathAnswer').value);
            
            if (isNaN(userAns)) {
                alert("Masukkan angka!");
                return;
            }

            if (userAns === currentMathAnswer) {
                // BENAR
                if(!isAdminMode) userTokens += 500000;
                alert("BENAR! +500.000 Token");
                // Lanjut soal berikutnya
                generateMathQuestion();
            } else {
                // SALAH
                mathLives--;
                alert(`SALAH! Jawaban benar: ${currentMathAnswer}. Nyawa berkurang.`);
                if (mathLives <= 0) {
                    // GAME OVER
                    isEventActive = false; // Paksa keluar event
                    els.eventBtn.innerText = "🔒 Event Tutup";
                    els.eventBtn.classList.remove('active');
                    els.eventBtn.disabled = true;
                    
                    document.getElementById('mathGameArea').style.display = 'none';
                    document.getElementById('mathGameOver').style.display = 'block';
                } else {
                    generateMathQuestion();
                }
            }
            updateMathUI();
            updateUI();
        }

        function buyLife() {
            const cost = 3000000;
            if (isAdminMode || userTokens >= cost) {
                if(confirm(`Beli 1 Nyawa seharga ${cost.toLocaleString()} Token?`)) {
                    if(!isAdminMode) userTokens -= cost;
                    mathLives = 1; // Tambah 1 nyawa
                    
                    // Reset UI
                    document.getElementById('mathGameArea').style.display = 'block';
                    document.getElementById('mathGameOver').style.display = 'none';
                    
                    // Aktifkan event kembali
                    isEventActive = true;
                    els.eventBtn.innerText = "🔴 Keluar Event";
                    els.eventBtn.classList.add('active');
                    els.eventBtn.disabled = false;
                    
                    generateMathQuestion();
                    updateMathUI();
                    updateUI();
                    alert("Nyawa dibeli! Event dilanjutkan.");
                }
            } else {
                alert("Token tidak cukup untuk membeli nyawa!");
            }
        }

        function updateMathUI() {
            let heartsStr = "";
            for(let i=0; i<mathLives; i++) heartsStr += "❤️";
            document.getElementById('mathHearts').innerText = heartsStr;
        }

        function exitMathEvent() {
            closeAllModals();
            // Jika keluar manual, event dianggap tidak aktif (atau bisa dibuat tetap aktif, tapi demi keamanan kita nonaktifkan)
            isEventActive = false;
            els.eventBtn.innerText = "🎲 Ikuti Event";
            els.eventBtn.classList.remove('active');
            updateUI();
        }


        // --- LOGIKA INVENTORY / TAS ---
        function openInventory() {
            renderInventory();
            document.getElementById('inventoryModal').style.display = "flex";
        }

        function renderInventory() {
            const grid = document.getElementById('inventoryGrid');
            grid.innerHTML = ""; // Clear

            if (mysteryBoxes > 0) {
                // Buat Slot Misteri Box
                const slot = document.createElement('div');
                slot.className = 'inventory-slot';
                slot.onclick = selectInventoryItem; 
                
                slot.innerHTML = `
                    <div class="inv-icon">🎁</div>
                    <div class="inv-count">${mysteryBoxes}</div>
                    <div class="inv-name">Misteri Box</div>
                `;
                grid.appendChild(slot);
            } else {
                grid.innerHTML = `<div class="empty-msg" style="grid-column: span 3;">Tas Kosong. Ikuti Event untuk mendapatkan Misteri Box!</div>`;
            }
        }

        function selectInventoryItem() {
            document.getElementById('inventoryModal').style.display = "none";
            document.getElementById('itemSelectModal').style.display = "flex";
        }

        function confirmOpenItem() {
            if (mysteryBoxes > 0) {
                mysteryBoxes--; 
                document.getElementById('itemSelectModal').style.display = "none";
                
                // Gacha Logic
                const rand = Math.random() * 100;
                let title, text, icon, color;

                if (rand < 5) { 
                    const amount = Math.floor(Math.random() * (30 - 1) + 1) * 1000000; 
                    if(!isAdminMode) userTokens += amount;
                    title = "JACKPOT TOKEN!";
                    text = `+${amount.toLocaleString()} Token`;
                    icon = "💰";
                    color = "#f1c40f";
                } else if (rand < 35) { 
                    const gems = Math.floor(Math.random() * (10 - 5) + 5); 
                    if(!isAdminMode) userDiamonds += gems;
                    title = "HADIAH DIAMOND";
                    text = `+${gems} Diamond 💎`;
                    icon = "💎";
                    color = "#3498db";
                } else { 
                    title = "ZONK";
                    text = "Coba lagi lain kali!";
                    icon = "💨";
                    color = "#95a5a6";
                }

                const resModal = document.getElementById('boxResultModal');
                document.getElementById('boxResultTitle').innerText = title;
                document.getElementById('boxResultTitle').style.color = color;
                document.getElementById('boxResultText').innerText = text;
                document.getElementById('boxResultIcon').innerText = icon;
                
                resModal.style.display = "flex";
                updateUI();
            }
        }

        // --- LOGIKA UNIFIED TOP UP ---
        function openUnifiedTopUp() {
            if(isAdminMode) {
                alert("Halo Owner! Anda sudah memiliki saldo UNLIMITED. Tidak perlu Top Up.");
                return;
            }
            // Reset selections
            selectedGemAmount = 0;
            selectedTokenAmount = 0;
            selectedPrice = 0;
            selectedPaymentMethod = "";
            currentTopUpType = "token"; // Default tab
            
            document.getElementById('payNowBtn').disabled = true;
            document.getElementById('payNowBtn').innerText = "Bayar Sekarang";
            document.querySelectorAll('.payment-method').forEach(el => el.classList.remove('selected'));
            
            switchTab('token');
            document.getElementById('unifiedTopUpModal').style.display = "flex";
        }

        function switchTab(type) {
            currentTopUpType = type;
            const tokenTab = document.getElementById('tab-token');
            const diamondTab = document.getElementById('tab-diamond');
            const btns = document.querySelectorAll('.tab-btn');
            
            if(type === 'token') {
                tokenTab.style.display = 'block';
                diamondTab.style.display = 'none';
                btns[0].classList.add('active');
                btns[1].classList.remove('active');
            } else {
                tokenTab.style.display = 'none';
                diamondTab.style.display = 'block';
                btns[0].classList.remove('active');
                btns[1].classList.add('active');
            }
            // Reset selection when switching tabs
            selectedPrice = 0;
            selectedGemAmount = 0;
            selectedTokenAmount = 0;
            document.querySelectorAll('.payment-method').forEach(el => el.classList.remove('selected'));
            checkPaymentReady();
        }

        function selectDirectPackage(amount, price, element) {
            selectedTokenAmount = amount;
            selectedPrice = price;
            selectedGemAmount = 0; // Reset diamond
            
            // Visual selection logic specific to tab
            const parent = document.getElementById('tab-token');
            parent.querySelectorAll('.payment-method').forEach(el => el.classList.remove('selected'));
            element.classList.add('selected');
            
            checkPaymentReady();
        }

        function selectDiamondPackage(amount, price, element) {
            selectedGemAmount = amount;
            selectedPrice = price;
            selectedTokenAmount = 0; // Reset token
            
            // Visual selection logic specific to tab
            const parent = document.getElementById('tab-diamond');
            parent.querySelectorAll('.payment-method').forEach(el => el.classList.remove('selected'));
            element.classList.add('selected');
            
            checkPaymentReady();
        }

        function selectPayment(method, element) {
            selectedPaymentMethod = method;
            // Simple visual toggle for payment methods
             element.classList.add('selected');
             checkPaymentReady();
        }

        function checkPaymentReady() {
            const btn = document.getElementById('payNowBtn');
            if ((selectedTokenAmount > 0 || selectedGemAmount > 0) && selectedPaymentMethod !== "") {
                btn.disabled = false;
                btn.innerText = `Bayar Rp ${selectedPrice.toLocaleString()}`;
            } else {
                btn.disabled = true;
                btn.innerText = "Bayar Sekarang";
            }
        }

        function processPayment() {
            const btn = document.getElementById('payNowBtn');
            btn.disabled = true;
            btn.innerText = "Memproses Pembayaran...";
            
            setTimeout(() => {
                if(selectedTokenAmount > 0) {
                    userTokens += selectedTokenAmount;
                    alert(`Pembayaran Berhasil!\n+${selectedTokenAmount.toLocaleString()} Token telah ditambahkan.`);
                } else if (selectedGemAmount > 0) {
                    userDiamonds += selectedGemAmount;
                    alert(`Pembayaran Berhasil!\n+${selectedGemAmount} Diamond telah ditambahkan.`);
                }
                
                closeAllModals();
                updateUI();
            }, 2000);
        }

        // --- LOGIKA TUKAR DIAMOND KE TOKEN ---
        function initExchangeButton() {
            const headerActions = document.querySelector('.action-buttons');
            if (!document.getElementById('exchangeBtn')) {
                const exBtn = document.createElement('button');
                exBtn.id = 'exchangeBtn';
                exBtn.className = 'tool-btn';
                exBtn.style.backgroundColor = '#8e44ad';
                exBtn.style.boxShadow = '0 3px 0 #8e44ad';
                exBtn.innerText = 'Tukar Diamond';
                exBtn.onclick = openExchangeModal;
                headerActions.appendChild(exBtn);
            }
        }

        function openExchangeModal() {
             if(isAdminMode) {
                 alert("Admin tidak perlu menukar, saldo sudah unlimited!");
                 return;
             }
             updateUI();
             document.getElementById('exchangeModal').style.display = "flex";
        }

        function processDiamondExchange(tokenAmount, diamondCost) {
            if (isAdminMode || userDiamonds >= diamondCost) {
                if(confirm(`Tukar ${diamondCost} Diamond menjadi ${tokenAmount.toLocaleString()} Token?`)) {
                    if(!isAdminMode) userDiamonds -= diamondCost;
                    if(!isAdminMode) userTokens += tokenAmount;
                    alert("Pertukaran Berhasil!");
                    closeAllModals();
                    updateUI();
                }
            } else {
                alert("Diamond tidak cukup!");
            }
        }

        // --- LOGIKA REDEEM (TERMASUK ADMIN) ---
        function claimRedeem() {
            const code = document.getElementById('redeemInput').value.trim().toUpperCase();
            const msg = document.getElementById('redeemMsg');
            
            if (!code) { msg.innerText = "Masukkan kode!"; msg.style.color = "red"; return; }

            if (redeemCodes[code]) {
                if (redeemCodes[code].claimed) {
                    msg.innerText = "Kode sudah dipakai!"; msg.style.color = "red";
                } else {
                    const reward = redeemCodes[code];
                    
                    // DETEKSI ADMIN
                    if (reward.type === 'admin') {
                        isAdminMode = true;
                        userTokens = 999999999999;
                        userDiamonds = 999999999999;
                        
                        msg.innerText = "SELAMAT DATANG, OWNER AZFER! MODE TAK TERBATAS DIAKTIFKAN.";
                        msg.style.color = "#e74c3c";
                        msg.style.fontSize = "1.1rem";
                        
                        // Sapaan Sopan
                        setTimeout(() => {
                            alert("Halo, Kak Azfer! 👋\n\nTerima kasih telah menggunakan kode rahasia Owner.\n\nSistem telah mendeteksi kehadiran Kakak. Semua fasilitas Token dan Diamond kini telah dibuka tanpa batas (Unlimited).\n\nSilakan nikmati fitur roda sultan sepuasnya. Hormat kami, Sistem.");
                        }, 500);
                        
                    } else {
                        // Reward Biasa
                        if (reward.type === 'token') userTokens += reward.amount;
                        if (reward.type === 'diamond') userDiamonds += reward.amount;
                        msg.innerText = `SUAKSES! +${reward.amount} ${reward.type === 'token' ? 'Token' : 'Diamond'}`;
                        msg.style.color = "#2ecc71";
                    }
                    
                    redeemCodes[code].claimed = true;
                    document.getElementById('redeemInput').value = "";
                    updateUI();
                }
            } else {
                msg.innerText = "Kode salah!"; msg.style.color = "red";
            }
        }

        // --- LOGIKA TOKO ---
        function initiateBuy(name, price) {
            // Admin bisa beli apapun tanpa cek saldo
            if (isAdminMode || userTokens >= price) {
                pendingItemName = name;
                pendingItemPrice = price;
                document.getElementById('confirmItemName').innerText = name;
                document.getElementById('confirmItemPrice').innerText = isAdminMode ? "GRATIS (ADMIN)" : price.toLocaleString();
                document.getElementById('seriousModal').style.display = "flex";
            } else {
                if(confirm(`Token kurang! Ingin Top Up?`)) openUnifiedTopUp();
            }
        }
        function goToFinalStep() {
            document.getElementById('seriousModal').style.display = "none";
            document.getElementById('finalBuyModal').style.display = "flex";
        }
        function executePurchase() {
            if(!isAdminMode) userTokens -= pendingItemPrice;
            document.getElementById('finalBuyModal').style.display = "none";
            setTimeout(() => {
                alert(`Terima kasih sudah membeli: ${pendingItemName}!`);
                if (pendingItemName.includes("AKUN")) sendToWA(pendingItemName);
                else if (pendingItemName === "VOUCHER") alert("SS INI DAN CHAT WA: 0858-8238-2854");
                updateUI();
            }, 300);
        }
        function sendToWA(item) {
            window.open(`https://wa.me/${ADMIN_WA_NUMBER}?text=${encodeURIComponent(`Halo Admin, saya mau klaim ${item}. Kode: GO_SPIRIT`)}`, '_blank');
        }
        function checkShopAvailability() {
            document.querySelectorAll('.buy-btn').forEach(btn => {
                // Jika admin, semua tombol aktif
                if(isAdminMode) {
                    btn.classList.remove('disabled');
                    return;
                }
                const price = parseInt(btn.getAttribute('onclick').split(',')[1]);
                btn.classList.toggle('disabled', userTokens < price);
            });
        }
        function closeAllModals() {
            document.querySelectorAll('.modal').forEach(m => m.style.display = "none");
        }

        // Init dipanggil SETELAH login sukses di attemptLogin()
    </script>
</body>
</html>
