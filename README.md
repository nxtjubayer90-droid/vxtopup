<!DOCTYPE html>
<html lang="bn">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Game Top-Up Store</title>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Font Awesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        :root {
            --primary-purple: #7B61FF;
            --light-purple: #E9E7FF;
            --background-color: #F7F7FF;
            --card-bg-color: #ffffff;
            --primary-text-color: #2D3748;
            --secondary-text-color: #718096;
            --accent-green: #38E4D8;
            --accent-red: #EF4444;
            --border-radius-large: 24px;
            --border-radius-medium: 16px;
            --soft-shadow: 0 10px 30px rgba(123, 97, 255, 0.09);
            --font-family: 'Poppins', sans-serif;
        }

        /* ডার্ক মোড স্টাইল */
        body.dark-mode {
            --background-color: #1A202C;
            --card-bg-color: #2D3748;
            --primary-text-color: #F7FAFC;
            --secondary-text-color: #A0AEC0;
            --light-purple: #4A5568;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: var(--font-family);
            background-color: var(--background-color);
            color: var(--primary-text-color);
            height: 100vh;
            overflow: hidden;
            transition: background-color 0.3s, color 0.3s;
        }

        .auth-container, .app-container {
            position: relative; width: 100%; height: 100vh;
            max-width: 450px; margin: auto; overflow: hidden;
            background-color: var(--background-color);
        }
        .app-container { display: none; }

        .screen {
            position: absolute; top: 0; left: 0;
            width: 100%; height: 100%;
            background-color: var(--background-color);
            transform: translateX(100%);
            transition: transform 0.35s ease-in-out;
            overflow-y: auto; padding: 20px;
            padding-bottom: 110px;
        }
        .screen.active { transform: translateX(0); }
        #homeScreen, #loginScreen { transform: translateX(0); }

        .auth-container { display: flex; align-items: center; justify-content: center; padding: 20px; }
        .auth-wrapper { width: 100%; }
        .auth-logo-container { text-align: center; margin-bottom: 40px; }
        .auth-logo-container h1 { font-size: 36px; font-weight: 700; color: var(--primary-purple); }

        .auth-form { width: 100%; padding: 30px; border-radius: var(--border-radius-large); background-color: var(--card-bg-color); box-shadow: var(--soft-shadow); }
        .auth-form h2 { text-align: center; margin-bottom: 25px; font-weight: 600; color: var(--primary-text-color); }
        .auth-form .input-group { margin-bottom: 20px; }
        .auth-form .input-group input { width: 100%; padding: 15px; background-color: var(--background-color); border: 1px solid #E2E8F0; border-radius: var(--border-radius-medium); font-size: 16px; color: var(--primary-text-color);}
        .auth-form .form-link { text-align: center; margin-top: 20px; font-size: 14px; color: var(--secondary-text-color); }
        .auth-form .form-link span { color: var(--primary-purple); cursor: pointer; font-weight: 600; }

        .section-title { font-size: 20px; font-weight: 600; margin: 25px 0 15px 0; }
        .btn { display: block; width: 100%; background: var(--primary-purple); color: white; border: none; padding: 16px; border-radius: var(--border-radius-medium); font-size: 16px; font-weight: 600; cursor: pointer; text-align: center; transition: all 0.3s; box-shadow: 0 5px 15px rgba(123, 97, 255, 0.3); }
        .btn:hover { background-color: #6a50e4; transform: translateY(-2px); }
        .btn-danger { background: var(--accent-red); }
        .btn-secondary { background: var(--secondary-text-color); }

        /* New Payment Button Styles */
        .pay-btn {
            flex: 1;
            padding: 12px;
            border: 2px solid var(--primary-purple);
            background: transparent;
            color: var(--primary-purple);
            border-radius: var(--border-radius-medium);
            cursor: pointer;
            font-weight: 600;
            transition: 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        .pay-btn.selected {
            background: var(--primary-purple);
            color: white;
        }
        .payment-selection-box {
            margin-bottom: 20px;
        }
        .payment-selection-box p {
            margin-bottom: 10px;
            font-weight: 600;
            font-size: 14px;
        }

        .header { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; margin-bottom: 20px; }
        .header .greeting h3 { font-size: 24px; font-weight: 600; }
        .header .greeting p { font-size: 16px; color: var(--secondary-text-color); }
        .header .profile-avatar { width: 50px; height: 50px; border-radius: 50%; object-fit: cover; cursor: pointer; border: 2px solid var(--primary-purple); }

        .notice-card { background-color: var(--light-purple); border-radius: var(--border-radius-medium); padding: 20px; margin-bottom: 25px; display: flex; align-items: center; }
        .notice-card i { font-size: 24px; color: var(--primary-purple); margin-right: 15px; }
        .notice-card p { font-size: 14px; line-height: 1.5; color: var(--primary-text-color); }

        .slider-container { width: 100%; height: 160px; margin-bottom: 25px; border-radius: var(--border-radius-large); overflow: hidden; position: relative; box-shadow: var(--soft-shadow); }
        .slider { display: flex; height: 100%; transition: transform 0.5s ease-in-out; }
        .slide { min-width: 100%; height: 100%; }
        .slide img { width: 100%; height: 100%; object-fit: cover; display: block; }

        .game-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 20px; }
        .game-card { background: var(--card-bg-color); border-radius: var(--border-radius-large); text-align: center; cursor: pointer; transition: all 0.2s ease; box-shadow: var(--soft-shadow); overflow: hidden; }
        .game-card:hover { transform: translateY(-5px); }
        .game-card .icon-wrapper { height: 140px; display: flex; align-items: center; justify-content: center; padding: 15px; }
        .game-card img { width: 100%; height: 100%; object-fit: contain; }
        .game-info-container { padding: 15px; }
        .game-name { font-weight: 600; font-size: 15px; }

        .page-header { display: flex; align-items: center; margin-bottom: 20px; }
        .back-btn { font-size: 22px; cursor: pointer; margin-right: 15px; color: var(--secondary-text-color); }
        .page-header h2 { font-size: 22px; font-weight: 600; }
        .game-banner { width: 100%; height: 140px; object-fit: cover; border-radius: var(--border-radius-large); margin-bottom: 20px; }
        .topup-section { background-color: var(--card-bg-color); border-radius: var(--border-radius-large); padding: 20px; margin-bottom: 20px; box-shadow: var(--soft-shadow); }
        
        .input-group input { width: 100%; padding: 14px; background-color: var(--background-color); border: 1px solid #E2E8F0; border-radius: var(--border-radius-medium); color: var(--primary-text-color); font-size: 16px; }
        .input-group label { display: block; margin-bottom: 8px; font-weight: 500; font-size: 14px; }
        
        .package-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; }
        .package-item { background-color: var(--background-color); border: 2px solid transparent; border-radius: var(--border-radius-medium); padding: 15px; cursor: pointer; text-align: center; transition: 0.2s; }
        .package-item.selected { border-color: var(--primary-purple); background-color: var(--light-purple); }
        .package-item .amount { font-weight: 600; font-size: 16px; }
        .package-item .price { color: var(--primary-purple); font-weight: 600; }

        .wallet-card { background: linear-gradient(135deg, var(--primary-purple), #9f8eff); color: white; padding: 30px; border-radius: var(--border-radius-large); text-align: center; margin-bottom: 25px; }
        .wallet-card h1 { font-size: 44px; margin: 10px 0; }

        .bottom-nav { position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%); width: 90%; display: flex; justify-content: space-around; background-color: rgba(255, 255, 255, 0.8); backdrop-filter: blur(10px); padding: 10px 0; border-radius: var(--border-radius-large); box-shadow: 0 10px 40px rgba(123, 97, 255, 0.2); z-index: 1000; }
        body.dark-mode .bottom-nav { background-color: rgba(45, 55, 72, 0.8); }
        .nav-item { cursor: pointer; display: flex; flex-direction: column; align-items: center; color: var(--secondary-text-color); flex: 1; }
        .nav-item i { font-size: 22px; margin-bottom: 4px; }
        .nav-item span { font-size: 11px; }
        .nav-item.active { color: var(--primary-purple); }

        .status-badge { padding: 6px 12px; border-radius: 20px; font-size: 12px; font-weight: 700; color: white; }
        .status-Pending { background-color: #F59E0B; }
        .status-Completed { background-color: #10B981; }

        .support-fab { position: fixed; bottom: 100px; right: 20px; width: 50px; height: 50px; background-color: var(--primary-purple); color: white; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 22px; cursor: pointer; box-shadow: 0 4px 15px rgba(0,0,0,0.2); z-index: 1001; }
    </style>
</head>
<body>

    <div class="auth-container">
        <div class="screen active" id="loginScreen">
            <div class="auth-wrapper">
                <div class="auth-logo-container"> <h1 id="authLogoText">GameStore</h1> </div>
                <div class="auth-form">
                    <h2>VX TOPUP SITE!</h2>
                    <div class="input-group"> <input type="email" id="loginEmail" placeholder="আপনার ইমেইল"> </div>
                    <div class="input-group"> <input type="password" id="loginPassword" placeholder="আপনার পাসওয়ার্ড"> </div>
                    <button class="btn" onclick="handleLogin()">লগইন করুন</button>
                    <p class="form-link">অ্যাকাউন্ট নেই? <span onclick="toggleAuthScreens('signUpScreen')">সাইন আপ করুন</span></p>
                </div>
            </div>
        </div>
        <div class="screen" id="signUpScreen">
             <div class="auth-wrapper">
                <div class="auth-logo-container"> <h1 id="authLogoTextSignUp">GameStore</h1> </div>
                <div class="auth-form">
                    <h2>নতুন অ্যাকাউন্ট তৈরি করুন</h2>
                    <div class="input-group"> <input type="text" id="signUpName" placeholder="আপনার সম্পূর্ণ নাম"> </div>
                    <div class="input-group"> <input type="email" id="signUpEmail" placeholder="আপনার ইমেইল"> </div>
                    <div class="input-group"> <input type="password" id="signUpPassword" placeholder="নতুন পাসওয়ার্ড দিন"> </div>
                    <button class="btn" onclick="handleSignUp()">সাইন আপ করুন</button>
                    <p class="form-link">ইতিমধ্যে অ্যাকাউন্ট আছে? <span onclick="toggleAuthScreens('loginScreen')">লগইন করুন</span></p>
                </div>
            </div>
        </div>
    </div>

    <div class="app-container">
        <div class="screen active" id="homeScreen">
            <header class="header">
                <div class="greeting">
                    <h3 id="greetingName">VX TOPUP SITE!</h3>
                    <p>আজকে কী টপ-আপ করবেন?</p>
                </div>
                <img id="headerProfileAvatar" src="https://i.ibb.co/74CkxSP/avatar.png" alt="Avatar" class="profile-avatar" onclick="showScreen('profileScreen', document.querySelectorAll('.nav-item')[4])">
            </header>
            <div class="slider-container"><div class="slider"></div></div>
            <div class="notice-card" id="noticeCardContainer" style="display: none;"><i class="fa-solid fa-bullhorn"></i><p id="noticeCardText"></p></div>
            <h2 class="section-title">জনপ্রিয় গেমসমূহ</h2>
            <div class="game-grid"></div>
        </div>

        <!-- Top-Up Screen (Updated with your requested snippet) -->
        <div class="screen" id="topupScreen">
            <header class="page-header"><i class="fa-solid fa-arrow-left back-btn" onclick="showScreen('homeScreen', document.querySelector('.nav-item'))"></i><h2 id="gameTitle"></h2></header>
            <img id="gameBanner" src="" alt="Game Banner" class="game-banner">
            
            <div class="topup-section">
                <div class="input-group">
                    <label>Player UID</label>
                    <input type="number" id="playerId" placeholder="Enter Game UID">
                </div>
            </div>

            <div class="topup-section">
                <h3 style="margin-bottom:15px; font-size:16px;">প্যাকেজ বেছে নিন</h3>
                <div class="package-grid" id="packageGrid"></div>
            </div>

            <div class="topup-section">
                <div class="payment-selection-box">
                    <p>Select Payment Method:</p>
                    <div style="display: flex; gap: 10px;">
                        <button class="pay-btn" id="walletPayBtn" onclick="setPaymentMethod('wallet')">
                            <i class="fas fa-wallet"></i> Pay via Wallet
                        </button>
                        <button class="pay-btn" id="adminPayBtn" onclick="setPaymentMethod('admin_gateway')">
                            <i class="fas fa-university"></i> Admin Payment
                        </button>
                    </div>
                </div>

                <div id="adminPaymentDetails" style="display: none; margin-top: 15px;">
                    <p style="font-size: 13px; color: var(--secondary-text-color); margin-bottom: 10px;">আমাদের বিকাশ/নগদ নম্বরে টাকা পাঠিয়ে ট্রানজেকশন আইডি দিন।</p>
                    <div class="input-group"><input type="text" id="transactionId" placeholder="Transaction ID"></div>
                </div>
            </div>

            <button class="btn" style="margin-top: 20px;" onclick="processOrder()">BUY NOW</button>
        </div>

        <div class="screen" id="walletScreen">
            <header class="page-header"><h2 style="margin: auto;">ওয়ালেট</h2></header>
            <div class="wallet-card">
                <p>আপনার ব্যালেন্স</p>
                <h1 id="profileWalletBalance">৳ 0.00</h1>
                 <button class="btn" style="background: rgba(255,255,255,0.2);" onclick="showScreen('addMoneyScreen')">অ্যাড মানি</button>
            </div>
            <div id="moneyRequestHistory"></div>
        </div>

        <div class="screen" id="orderHistoryScreen">
            <header class="page-header"><h2 style="margin: auto;">অর্ডার হিস্টোরি</h2></header>
            <div id="orderHistoryContainer"></div>
        </div>

        <div class="screen" id="profileScreen">
            <header class="page-header"><h2 style="margin: auto;">প্রোফাইল</h2></header>
            <div style="text-align:center; padding: 20px;">
                <img id="profileScreenAvatar" src="https://i.ibb.co/74CkxSP/avatar.png" class="profile-avatar" style="width:100px; height:100px; margin-bottom:10px;">
                <h3 id="profileNameDisplay">User</h3>
                <p id="profileIdDisplay">ID: ---</p>
                <button class="btn btn-danger" style="margin-top:30px;" onclick="handleLogout()">লগ আউট</button>
            </div>
        </div>

        <nav class="bottom-nav">
            <div class="nav-item active" onclick="showScreen('homeScreen', this)"> <i class="fa-solid fa-house"></i> <span>হোম</span> </div>
            <div class="nav-item" onclick="showScreen('orderHistoryScreen', this)"> <i class="fa-solid fa-receipt"></i> <span>অর্ডার</span> </div>
            <div class="nav-item" onclick="showScreen('walletScreen', this)"> <i class="fa-solid fa-wallet"></i> <span>ওয়ালেট</span> </div>
            <div class="nav-item" onclick="showScreen('profileScreen', this)"> <i class="fa-regular fa-user"></i> <span>প্রোফাইল</span> </div>
        </nav>
        
        <div class="support-fab" id="supportBtn"><i class="fa-solid fa-headset"></i></div>
    </div>

    <!-- Scripts -->
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>

    <script>
        const firebaseConfig = {
            apiKey: "AIzaSyDZhTcyFAPo7jFHxlrvXx4SeU7UR8dEsHA",
            authDomain: "vx-topup-5101b.firebaseapp.com",
            projectId: "vx-topup-5101b",
            storageBucket: "vx-topup-5101b.firebasestorage.app",
            messagingSenderId: "270031103512",
            appId: "1:270031103512:web:57885e0b443f31e587856d"
        };

        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const db = firebase.firestore();

        let currentUser = null;
        let userBalance = 0; // আপনার ডাটাবেস থেকে পাওয়া ব্যালেন্স
        let selectedGameData = {}, selectedPackage = null, selectedPayment = null;
        let isPaidViaAdmin = false; // অ্যাডমিন পেমেন্ট স্ট্যাটাস (ট্রানজেকশন আইডি চেক করার জন্য)

        // পেমেন্ট মেথড সেট করার ফাংশন
        function setPaymentMethod(method) {
            selectedPayment = method;
            document.querySelectorAll('.pay-btn').forEach(btn => btn.classList.remove('selected'));
            if(method === 'wallet') {
                document.getElementById('walletPayBtn').classList.add('selected');
                document.getElementById('adminPaymentDetails').style.display = 'none';
                isPaidViaAdmin = true; // ওয়ালেটের জন্য সরাসরি পেমেন্ট ট্রু
            } else {
                document.getElementById('adminPayBtn').classList.add('selected');
                document.getElementById('adminPaymentDetails').style.display = 'block';
                isPaidViaAdmin = false; // ট্রানজেকশন আইডি ইনপুট সাপেক্ষে ফলস
            }
        }

        // আপনার দেওয়া নতুন processOrder লজিক
        async function processOrder() {
            const playerId = document.getElementById('playerId').value;
            const txIdInput = document.getElementById('transactionId').value;
            
            // ১. ভ্যালিডেশন চেক
            if (!playerId) {
                alert('অনুগ্রহ করে প্লেয়ার আইডি (UID) বসান!');
                return;
            }
            if (!selectedPackage) {
                alert('অনুগ্রহ করে একটি প্যাকেজ সিলেক্ট করুন!');
                return;
            }
            if (!selectedPayment) {
                alert('অনুগ্রহ করে পেমেন্ট মেথড সিলেক্ট করুন!');
                return;
            }

            // ২. পেমেন্ট মেথড চেক
            if (selectedPayment === 'wallet') {
                if (userBalance >= selectedPackage.price) {
                    confirmOrder('Wallet');
                } else {
                    alert('আপনার ওয়ালেটে পর্যাপ্ত ব্যালেন্স নেই! দয়া করে টাকা অ্যাড করুন।');
                }
            } 
            else if (selectedPayment === 'admin_gateway') {
                // ৩. অ্যাডমিন পেমেন্ট চেক
                if (!txIdInput || txIdInput.length < 4) {
                    alert('আপনি এখনো পেমেন্ট করেননি! আগে বিকাশ/নগদে টাকা পাঠিয়ে ট্রানজেকশন আইডি দিন।');
                } else {
                    isPaidViaAdmin = true;
                    confirmOrder('Admin Direct');
                }
            }
        }

        async function confirmOrder(method) {
            try {
                const orderData = {
                    userId: currentUser.uid,
                    game: selectedGameData.name,
                    package: selectedPackage.amount,
                    price: selectedPackage.price,
                    playerId: document.getElementById('playerId').value,
                    paymentMethod: method,
                    transactionId: document.getElementById('transactionId').value || 'Wallet Payment',
                    status: 'Pending',
                    date: firebase.firestore.FieldValue.serverTimestamp()
                };

                // ওয়ালেট থেকে টাকা কাটলে আপডেট করা
                if(method === 'Wallet') {
                    const newBalance = userBalance - selectedPackage.price;
                    await db.collection('users').doc(currentUser.uid).update({ walletBalance: newBalance });
                }

                await db.collection('orders').add(orderData);
                alert('আপনার অর্ডারটি সফলভাবে সাবমিট হয়েছে!');
                showScreen('orderHistoryScreen', document.querySelectorAll('.nav-item')[1]);
            } catch (e) {
                alert('Error: ' + e.message);
            }
        }

        // Firebase Auth Listener
        auth.onAuthStateChanged(user => {
            if (user) {
                currentUser = user;
                document.querySelector('.auth-container').style.display = 'none';
                document.querySelector('.app-container').style.display = 'block';
                
                db.collection('users').doc(user.uid).onSnapshot(doc => {
                    if (doc.exists) {
                        const data = doc.data();
                        userBalance = data.walletBalance || 0;
                        document.getElementById('profileWalletBalance').innerText = `৳ ${userBalance.toFixed(2)}`;
                        document.getElementById('profileNameDisplay').innerText = data.name;
                        document.getElementById('profileIdDisplay').innerText = `ID: ${data.profileId}`;
                        document.getElementById('greetingName').innerText = `হাই, ${data.name.split(' ')[0]}`;
                    }
                });
                loadGames();
                loadSlider();
                loadOrders();
            } else {
                document.querySelector('.auth-container').style.display = 'flex';
                document.querySelector('.app-container').style.display = 'none';
            }
        });

        // অন্যান্য প্রয়োজনীয় ফাংশনগুলো
        async function loadGames() {
            const snap = await db.collection('games').where('status', '==', 'active').get();
            const grid = document.querySelector('.game-grid');
            grid.innerHTML = '';
            snap.forEach(doc => {
                const game = doc.data();
                const card = document.createElement('div');
                card.className = 'game-card';
                card.innerHTML = `<div class="icon-wrapper"><img src="${game.logo}"></div><div class="game-info-container"><div class="game-name">${game.name}</div></div>`;
                card.onclick = () => {
                    selectedGameData = game;
                    document.getElementById('gameTitle').innerText = game.name;
                    document.getElementById('gameBanner').src = game.banner;
                    const pkgGrid = document.getElementById('packageGrid');
                    pkgGrid.innerHTML = '';
                    game.packages.forEach(pkg => {
                        const div = document.createElement('div');
                        div.className = 'package-item';
                        div.innerHTML = `<div class="amount">${pkg.amount}</div><div class="price">৳ ${pkg.price}</div>`;
                        div.onclick = () => {
                            document.querySelectorAll('.package-item').forEach(p => p.classList.remove('selected'));
                            div.classList.add('selected');
                            selectedPackage = pkg;
                        };
                        pkgGrid.appendChild(div);
                    });
                    showScreen('topupScreen');
                };
                grid.appendChild(card);
            });
        }

        async function loadSlider() {
            const doc = await db.collection('settings').doc('slider').get();
            const slider = document.querySelector('.slider');
            if (doc.exists) {
                doc.data().imageUrls.forEach(url => {
                    const div = document.createElement('div');
                    div.className = 'slide';
                    div.innerHTML = `<img src="${url}">`;
                    slider.appendChild(div);
                });
            }
        }

        async function loadOrders() {
            db.collection('orders').where('userId', '==', auth.currentUser.uid).orderBy('date', 'desc').onSnapshot(snap => {
                const container = document.getElementById('orderHistoryContainer');
                container.innerHTML = '';
                snap.forEach(doc => {
                    const order = doc.data();
                    container.innerHTML += `<div class="topup-section">
                        <div style="display:flex; justify-content:space-between;">
                            <strong>${order.game} - ${order.package}</strong>
                            <span class="status-badge status-${order.status}">${order.status}</span>
                        </div>
                        <p style="font-size:12px; margin-top:5px; color:gray;">UID: ${order.playerId} | Price: ৳${order.price}</p>
                    </div>`;
                });
            });
        }

        function showScreen(id, nav) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(nav) {
                document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
                nav.classList.add('active');
            }
        }

        function toggleAuthScreens(id) {
            document.querySelectorAll('.auth-container .screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        }

        async function handleLogin() {
            const e = document.getElementById('loginEmail').value;
            const p = document.getElementById('loginPassword').value;
            try { await auth.signInWithEmailAndPassword(e, p); } catch(err) { alert(err.message); }
        }

        async function handleSignUp() {
            const n = document.getElementById('signUpName').value;
            const e = document.getElementById('signUpEmail').value;
            const p = document.getElementById('signUpPassword').value;
            try {
                const res = await auth.createUserWithEmailAndPassword(e, p);
                await db.collection('users').doc(res.user.uid).set({
                    name: n, email: e, walletBalance: 0, profileId: Math.floor(100000 + Math.random() * 900000), status: 'active'
                });
            } catch(err) { alert(err.message); }
        }

        function handleLogout() { auth.signOut(); }
    </script>
</body>
</html>
