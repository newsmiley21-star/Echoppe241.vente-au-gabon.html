<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Business & Messagerie</title> 
    
    <!-- Configuration PWA et Mobile -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#00853f">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <!-- Polices Premium -->
    <link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@600&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --primary: #00853f; /* Vert Gabon */
            --accent: #fcd116;  /* Jaune Gabon */
            --blue: #3a75c4;    /* Bleu Gabon */
            --dark: #0f172a; 
            --bg-soft: #f1f5f9; 
            --glass: rgba(255, 255, 255, 0.85);
        }

        body { 
            font-family: 'Inter', sans-serif; 
            background-color: var(--bg-soft); 
            color: var(--dark);
            margin: 0;
            overflow-x: hidden;
        }

        /* Splash Screen */
        #splash-screen {
            position: fixed;
            inset: 0;
            background: white;
            z-index: 9999;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            transition: opacity 0.5s ease, visibility 0.5s;
        }

        .splash-logo {
            width: 120px;
            height: 120px;
            animation: pulseLogo 2s infinite ease-in-out;
        }

        @keyframes pulseLogo {
            0%, 100% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.1); opacity: 0.8; }
        }

        .loader-bar {
            width: 150px;
            height: 4px;
            background: #eee;
            border-radius: 10px;
            margin-top: 24px;
            overflow: hidden;
            position: relative;
        }

        .loader-progress {
            width: 40%;
            height: 100%;
            background: linear-gradient(90deg, var(--primary), var(--accent), var(--blue));
            position: absolute;
            animation: loadingMove 1.5s infinite linear;
        }

        @keyframes loadingMove {
            0% { left: -40%; }
            100% { left: 100%; }
        }

        /* Titres & Fontes */
        h1, h2, h3, .brand-font { 
            font-family: 'Syne', sans-serif; 
            text-transform: lowercase;
        }

        .currency { font-family: 'JetBrains Mono', monospace; }

        .view { display: none; }
        .view.active { display: block; animation: viewIn 0.4s ease-out; }
        
        @keyframes viewIn {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .glass-header { 
            background: var(--glass);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(0,0,0,0.05);
        }

        .card-neo {
            background: white;
            border-radius: 28px;
            box-shadow: 0 10px 30px -10px rgba(0,0,0,0.04);
        }

        .btn-primary {
            background: var(--primary);
            color: white;
            padding: 16px;
            border-radius: 20px;
            font-weight: 800;
            text-transform: uppercase;
            font-size: 12px;
            letter-spacing: 1px;
        }

        .nav-item.active i { color: var(--primary); }
        .nav-item.active span { color: var(--primary); }

        .msg-bubble { border-radius: 22px; padding: 14px 18px; font-size: 14px; }
        .msg-sent { background: var(--primary); color: white; border-bottom-right-radius: 4px; }
        .msg-received { background: white; border-bottom-left-radius: 4px; }

    </style>
</head>
<body class="pb-24">

    <!-- SPLASH SCREEN -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Echoppe241 Logo" class="splash-logo">
        <h2 class="mt-4 text-2xl font-black italic">echoppe<span class="text-[#00853f]">241</span></h2>
        <div class="loader-bar">
            <div class="loader-progress"></div>
        </div>
        <p class="mt-6 text-[10px] font-bold uppercase tracking-[3px] text-slate-400">Chargement sécurisé...</p>
    </div>

    <!-- HEADER -->
    <header class="fixed top-0 inset-x-0 z-[1000] h-20 px-6 flex items-center justify-between glass-header">
        <div class="flex items-center gap-2" onclick="navigateTo('home')">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-9 h-9">
            <h1 class="text-xl font-extrabold italic">echoppe<span class="text-[#00853f]">241</span></h1>
        </div>
        <div id="user-zone" class="hidden flex items-center gap-3">
            <div class="text-right">
                <p id="top-balance" class="currency text-xs font-bold text-emerald-600">0 F</p>
                <p class="text-[8px] font-black uppercase opacity-40 tracking-tighter">Portefeuille</p>
            </div>
            <img id="top-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-2xl bg-slate-100 object-cover cursor-pointer border-2 border-white shadow-sm">
        </div>
    </header>

    <main class="max-w-xl mx-auto px-6 pt-24">
        <!-- Accueil -->
        <div id="view-home" class="view active">
            <div class="mb-8">
                <h2 class="text-3xl font-extrabold leading-none mb-4">le marché en <br><span class="text-[#00853f]">direct du gabon.</span></h2>
                <div class="card-neo flex items-center p-2 border border-slate-100">
                    <i class="fa-solid fa-search text-slate-300 ml-4"></i>
                    <input type="text" placeholder="Trouver un produit, un vendeur..." class="bg-transparent border-none outline-none w-full p-4 text-sm font-medium">
                </div>
            </div>
            <div id="product-list" class="grid grid-cols-2 gap-4"></div>
        </div>

        <!-- Messagerie -->
        <div id="view-messages" class="view">
            <h2 class="text-3xl font-extrabold mb-6">vos discussions.</h2>
            <div id="chat-list" class="space-y-3"></div>
        </div>

        <!-- Profil -->
        <div id="view-profile" class="view">
            <div class="card-neo bg-slate-900 p-8 text-white relative overflow-hidden">
                <div class="relative z-10">
                    <div class="flex justify-between items-start mb-8">
                        <div>
                            <p class="text-[9px] font-black uppercase tracking-widest opacity-50">Solde EchoppePay</p>
                            <h3 id="p-balance" class="currency text-4xl font-bold">0 FCFA</h3>
                        </div>
                        <img id="p-avatar" class="w-16 h-16 rounded-3xl border-2 border-white/20">
                    </div>
                    <p id="p-name" class="brand-font text-lg font-bold">Utilisateur</p>
                    <button onclick="handleLogout()" class="mt-4 text-[9px] font-bold uppercase text-red-400 tracking-widest">Déconnexion</button>
                </div>
                <div class="absolute -right-10 -top-10 w-40 h-40 bg-[#00853f]/20 rounded-full blur-3xl"></div>
                <div class="absolute -left-10 -bottom-10 w-40 h-40 bg-[#3a75c4]/10 rounded-full blur-3xl"></div>
            </div>
        </div>
    </main>

    <!-- NAVIGATION BASSE -->
    <nav class="fixed bottom-0 inset-x-0 h-20 bg-white/95 backdrop-blur-xl border-t border-slate-100 flex items-center justify-around px-6 z-[1000]">
        <button onclick="navigateTo('home')" class="nav-item group flex flex-col items-center gap-1 active">
            <i class="fa-solid fa-house-chimney text-xl text-slate-300 group-[.active]:text-[#00853f]"></i>
            <span class="text-[9px] font-bold uppercase">Marché</span>
        </button>
        <button onclick="navigateTo('messages')" class="nav-item group flex flex-col items-center gap-1">
            <i class="fa-solid fa-message-middle text-xl text-slate-300 group-[.active]:text-[#00853f]"></i>
            <span class="text-[9px] font-bold uppercase">Messages</span>
        </button>
        <button onclick="navigateTo('profile')" class="nav-item group flex flex-col items-center gap-1">
            <i class="fa-solid fa-user-circle text-xl text-slate-300 group-[.active]:text-[#00853f]"></i>
            <span class="text-[9px] font-bold uppercase">Compte</span>
        </button>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // CONFIGURATION
        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'echoppe241-main';

        // GESTION SPLASH SCREEN
        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                splash.style.opacity = '0';
                setTimeout(() => splash.style.visibility = 'hidden', 500);
            }, 2500); // Temps d'affichage : 2.5 secondes
        });

        // AUTH & SYNC
        let user = null;
        onAuthStateChanged(auth, async (u) => {
            if (u) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if(snap.exists()) {
                        user = { uid: u.uid, ...snap.data() };
                    } else {
                        const newUser = {
                            fullName: u.isAnonymous ? "Visiteur" : u.email.split('@')[0],
                            walletBalance: 0,
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`
                        };
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), newUser);
                        user = newUser;
                    }
                    updateUI();
                });
            } else {
                signInAnonymously(auth);
            }
        });

        function updateUI() {
            if(!user) return;
            document.getElementById('user-zone').classList.remove('hidden');
            document.getElementById('top-balance').innerText = `${user.walletBalance.toLocaleString()} F`;
            document.getElementById('p-balance').innerText = `${user.walletBalance.toLocaleString()} FCFA`;
            document.getElementById('top-avatar').src = user.avatar;
            document.getElementById('p-avatar').src = user.avatar;
            document.getElementById('p-name').innerText = user.fullName;
        }

        // NAVIGATION
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(btn => {
                btn.classList.toggle('active', btn.onclick.toString().includes(v));
            });
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };

        // MOCK DATA
        const products = [
            { id: 1, name: "Manioc d'Oyem", price: 5000, img: "https://images.unsplash.com/photo-1590779033100-9f60705a2f3b?w=400" },
            { id: 2, name: "Poissons Fumés (POG)", price: 12000, img: "https://images.unsplash.com/photo-1519708227418-c8fd9a32b7a2?w=400" }
        ];

        document.getElementById('product-list').innerHTML = products.map(p => `
            <div class="card-neo overflow-hidden border border-slate-50">
                <img src="${p.img}" class="w-full h-32 object-cover">
                <div class="p-4">
                    <h4 class="brand-font text-[10px] font-bold uppercase truncate mb-1">${p.name}</h4>
                    <p class="currency text-xs font-bold text-slate-900 mb-4">${p.price.toLocaleString()} F</p>
                    <button class="w-full py-2.5 bg-slate-900 text-white rounded-xl text-[9px] font-black uppercase">Commander</button>
                </div>
            </div>
        `).join('');

        window.handleLogout = () => signOut(auth).then(() => location.reload());

    </script>
</body>
</html>
