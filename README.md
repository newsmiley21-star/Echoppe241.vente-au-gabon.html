<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241</title> 
    
    <!-- CONFIGURATION PWA (INSTALLATION) -->
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">
    <meta name="theme-color" content="#00853f">
    
    <!-- ICÔNES (STYLE WHATSAPP/MESSENGER) -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    
    <!-- MANIFEST (DÉCLARATION APPLI) -->
    <link rel="manifest" href="data:application/json;base64,ewogICJuYW1lIjogIkVjaG9wcGUyNDEiLAogICJzaG9ydF9uYW1lIjogIkVjaG9wcGUyNDEiLAogICJzdGFydF91cmwiOiAiLiIsCiAgImRpc3BsYXkiOiAic3RhbmRhbG9uZSIsCiAgImJhY2tncm91bmRfY29sb3IiOiAiI2ZmZmZmZiIsCiAgInRoZW1lX2NvbG9yIjogIiMwMDg1M2YiLAogICJpY29ucyI6IFsKICAgIHsKICAgICAgInNyYyI6ICJodHRwczovL2kuaWJiLmNvL1JraGNSZENzL2VjaG9wcGUyNDEtbG9nby5wbmciLAogICAgICAic2l6ZXMiOiAiNTEyeDUxMiIsCiAgICAgICJ0eXBlIjogImltYWdlL3BuZyIsCiAgICAgICJwdXJwb3NlIjogImFueSBtYXNrYWJsZSIKICAgIH0KICBdCn0=">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { --gabon-green: #00853f; --gabon-yellow: #fcd116; --gabon-blue: #3a75c4; }
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            -webkit-tap-highlight-color: transparent; 
            background-color: #f8fafc;
            user-select: none; /* Empêche la sélection de texte comme une appli native */
        }
        
        /* Loader d'entrée */
        #app-loader {
            position: fixed; inset: 0; z-index: 10000;
            background: #ffffff;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
        }
        .loader-logo { width: 100px; animation: pulse 2s infinite ease-in-out; }
        @keyframes pulse { 0%, 100% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.1); opacity: 0.7; } }

        .glass { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(15px); -webkit-backdrop-filter: blur(15px); }
        .view { display: none; opacity: 0; transform: translateY(15px); transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); }
        .view.active { display: block; opacity: 1; transform: translateY(0); }
        
        .cat-btn.active { background: var(--gabon-green); color: white; transform: scale(1.05); }
        .prov-btn.active { border-color: var(--gabon-green); color: var(--gabon-green); background: #f0fdf4; }

        .btn-press:active { transform: scale(0.95); }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        
        /* Style des inputs "App Style" */
        .app-input { @apply w-full p-4 bg-slate-100 rounded-2xl font-bold border-none focus:ring-2 focus:ring-emerald-500 transition-all outline-none; }
        
        /* Grid adaptée tablettes/PC */
        .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 1rem; }
        @media (min-width: 768px) { .product-grid { grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 1.5rem; } }
    </style>
</head>
<body class="text-slate-900">

    <!-- SPLASH SCREEN / LOADER -->
    <div id="app-loader">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="loader-logo mb-6">
        <div class="w-48 h-1.5 bg-slate-100 rounded-full overflow-hidden">
            <div id="loader-bar" class="h-full bg-emerald-500 w-0 transition-all duration-700"></div>
        </div>
        <p class="mt-4 text-[10px] font-black uppercase tracking-[0.3em] text-slate-400">Echoppe241 • Gabon</p>
    </div>

    <!-- HEADER NAVIGATION -->
    <nav class="sticky top-0 z-[100] glass border-b border-slate-100 px-6 h-20 flex justify-between items-center">
        <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 h-10 shadow-sm rounded-lg">
            <div class="flex flex-col">
                <span class="font-extrabold text-xl tracking-tighter uppercase italic leading-none">Echoppe<span class="text-emerald-600">241</span></span>
                <span class="text-[7px] font-black tracking-widest text-slate-400 uppercase">Économie Circulaire</span>
            </div>
        </div>
        
        <div class="flex items-center gap-3">
            <div id="profile-zone" class="hidden flex items-center gap-2 bg-white p-1 pr-3 rounded-full border border-slate-100 cursor-pointer shadow-sm" onclick="navigateTo('profile')">
                <img id="nav-avatar" src="" class="w-8 h-8 rounded-full object-cover">
                <span id="nav-balance" class="text-[10px] font-black text-emerald-600">0 F</span>
            </div>
            <button id="btn-login-head" onclick="navigateTo('auth')" class="bg-emerald-600 text-white px-5 py-2 rounded-xl text-[10px] font-black uppercase tracking-wider btn-press">Connexion</button>
        </div>
    </nav>

    <!-- MAIN VIEW CONTAINER -->
    <main class="max-w-6xl mx-auto pb-32 pt-6 px-4">
        
        <!-- ACCUEIL -->
        <div id="view-home" class="view active">
            <header class="mb-8">
                <h1 class="text-4xl md:text-5xl font-black italic uppercase leading-[0.9] mb-4 tracking-tighter">
                    Vendez & Achetez<br><span class="text-emerald-600">Au Gabon.</span>
                </h1>
                
                <!-- Filtres Catégories -->
                <div class="flex gap-2 overflow-x-auto pb-4 no-scrollbar">
                    <button onclick="setCategory('all')" class="cat-btn active whitespace-nowrap bg-white border border-slate-100 px-6 py-3 rounded-2xl font-black text-[10px] uppercase shadow-sm">Tout</button>
                    <button onclick="setCategory('Agriculture')" class="cat-btn whitespace-nowrap bg-white border border-slate-100 px-6 py-3 rounded-2xl font-black text-[10px] uppercase shadow-sm">Agriculture</button>
                    <button onclick="setCategory('Pêche')" class="cat-btn whitespace-nowrap bg-white border border-slate-100 px-6 py-3 rounded-2xl font-black text-[10px] uppercase shadow-sm">Pêche</button>
                    <button onclick="setCategory('Artisanat')" class="cat-btn whitespace-nowrap bg-white border border-slate-100 px-6 py-3 rounded-2xl font-black text-[10px] uppercase shadow-sm">Artisanat</button>
                </div>

                <!-- Filtres Provinces -->
                <div class="flex gap-2 overflow-x-auto no-scrollbar" id="prov-filters"></div>
            </header>

            <div id="market-grid" class="product-grid">
                <!-- Articles injectés par JS -->
            </div>
        </div>

        <!-- PROFIL / ECHOPPEPAY -->
        <div id="view-profile" class="view">
            <div class="bg-white rounded-[40px] p-8 shadow-xl border border-slate-50 mb-6">
                <div class="flex flex-col items-center text-center">
                    <div class="relative group cursor-pointer mb-4" onclick="document.getElementById('avatar-input').click()">
                        <img id="p-avatar" src="" class="w-32 h-32 rounded-full border-4 border-white shadow-2xl object-cover">
                        <div class="absolute inset-0 bg-black/40 rounded-full flex items-center justify-center opacity-0 group-hover:opacity-100 transition-all">
                            <i class="fa-solid fa-camera text-white text-xl"></i>
                        </div>
                        <input type="file" id="avatar-input" class="hidden" accept="image/*">
                    </div>
                    
                    <div class="flex items-center justify-center gap-2 mb-1">
                        <input type="text" id="p-name" class="text-2xl font-black uppercase text-center bg-transparent w-full max-w-[200px] outline-none border-b-2 border-transparent focus:border-emerald-500">
                        <button onclick="updateProfileName()" class="text-emerald-500 text-xl"><i class="fa-solid fa-circle-check"></i></button>
                    </div>
                    <p id="p-email" class="text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-8"></p>

                    <!-- Carte de Crédit EchoppePay -->
                    <div class="w-full max-w-sm bg-gradient-to-br from-emerald-500 via-emerald-700 to-emerald-900 rounded-[35px] p-8 text-white text-left shadow-2xl shadow-emerald-200 relative overflow-hidden">
                        <div class="absolute top-0 right-0 w-32 h-32 bg-white/10 rounded-full -mr-16 -mt-16 blur-3xl"></div>
                        <div class="flex justify-between items-start mb-10 relative z-10">
                            <div>
                                <p class="text-[9px] font-black uppercase opacity-60 tracking-[0.2em]">Solde Portefeuille</p>
                                <h2 id="p-balance" class="text-4xl font-black italic mt-1">0 FCFA</h2>
                            </div>
                            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 h-10 grayscale invert opacity-50">
                        </div>
                        <button onclick="openRecharge()" class="w-full bg-white text-emerald-900 font-black py-4 rounded-2xl text-[11px] uppercase tracking-widest shadow-lg btn-press">Recharger maintenant</button>
                    </div>
                </div>
            </div>

            <h3 class="font-black uppercase text-[11px] text-slate-400 tracking-[0.3em] mb-4 ml-6 flex items-center gap-2">
                <i class="fa-solid fa-clock-rotate-left"></i> Historique Transactions
            </h3>
            <div id="orders-list" class="space-y-3 mb-10"></div>
            
            <button onclick="handleLogout()" class="w-full py-5 bg-red-50 text-red-600 font-black rounded-3xl text-[11px] uppercase tracking-widest border border-red-100 btn-press">Fermer la session</button>
        </div>

        <!-- PUBLICATION (VENDEUR) -->
        <div id="view-publish" class="view max-w-2xl mx-auto">
            <div class="bg-white rounded-[40px] p-8 shadow-2xl border border-slate-50">
                <h2 class="text-3xl font-black uppercase italic mb-8">Déposer une <span class="text-emerald-600">Annonce</span></h2>
                <div class="space-y-6">
                    <div onclick="document.getElementById('prod-img-input').click()" class="w-full h-64 bg-slate-50 rounded-[35px] border-4 border-dashed border-slate-200 flex flex-col items-center justify-center cursor-pointer overflow-hidden group transition-all hover:bg-emerald-50 hover:border-emerald-200">
                        <div id="upload-preview" class="flex flex-col items-center group-hover:scale-110 transition-transform">
                            <i class="fa-solid fa-image text-5xl text-slate-200 mb-3"></i>
                            <span class="text-[10px] font-black text-slate-400 uppercase tracking-widest">Choisir une image claire</span>
                        </div>
                        <input type="file" id="prod-img-input" class="hidden" accept="image/*">
                    </div>
                    
                    <div class="space-y-2">
                        <label class="ml-4 text-[10px] font-black text-slate-400 uppercase">Détails de l'article</label>
                        <input type="text" id="prod-title" placeholder="Ex: Bananes du Woleu-Ntem" class="app-input">
                        <div class="grid grid-cols-2 gap-4">
                            <input type="number" id="prod-price" placeholder="Prix (FCFA)" class="app-input">
                            <select id="prod-cat" class="app-input">
                                <option value="Agriculture">Agriculture</option>
                                <option value="Pêche">Pêche</option>
                                <option value="Artisanat">Artisanat</option>
                                <option value="Vêtements">Vêtements</option>
                            </select>
                        </div>
                        <select id="prod-province" class="app-input"></select>
                    </div>

                    <button onclick="publishArticle()" id="btn-publish" class="w-full bg-emerald-600 text-white font-black py-5 rounded-[25px] shadow-xl uppercase tracking-widest btn-press flex items-center justify-center gap-3">
                        <i class="fa-solid fa-rocket"></i> Publier l'article
                    </button>
                </div>
            </div>
        </div>

        <!-- PANIER (ACHETEUR) -->
        <div id="view-cart" class="view max-w-2xl mx-auto">
            <h2 class="text-4xl font-black uppercase italic mb-8">Mon <span class="text-emerald-600">Panier</span></h2>
            <div id="cart-items" class="space-y-4 mb-8"></div>
            
            <div id="cart-footer" class="hidden bg-white p-8 rounded-[40px] shadow-2xl border border-slate-50">
                <div class="flex justify-between items-center mb-8">
                    <span class="font-black text-slate-400 text-sm uppercase tracking-widest">Total à payer</span>
                    <span id="cart-total" class="text-3xl font-black text-emerald-600">0 F</span>
                </div>
                <button onclick="checkout()" id="btn-pay" class="w-full bg-slate-900 text-white font-black py-5 rounded-[25px] shadow-xl uppercase tracking-widest btn-press flex items-center justify-center gap-3">
                    <i class="fa-solid fa-shield-check"></i> Payer via EchoppePay
                </button>
            </div>
            
            <div id="cart-empty" class="text-center py-32 opacity-10">
                <i class="fa-solid fa-basket-shopping text-8xl mb-6"></i>
                <p class="font-black uppercase text-xl">Panier vide</p>
            </div>
        </div>

        <!-- AUTHENTIFICATION -->
        <div id="view-auth" class="view max-w-md mx-auto pt-10">
            <div class="bg-white rounded-[45px] p-12 shadow-2xl text-center border border-slate-50">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-20 mx-auto mb-8 shadow-lg rounded-2xl">
                <h2 id="auth-title" class="text-3xl font-black uppercase italic mb-8">Ravi de vous voir !</h2>
                
                <div class="space-y-4">
                    <div id="signup-fields" class="hidden">
                        <input type="text" id="auth-name" placeholder="Votre Nom complet" class="app-input">
                    </div>
                    <input type="email" id="auth-email" placeholder="Email (ex: gabon@mail.ga)" class="app-input">
                    <input type="password" id="auth-pass" placeholder="Mot de passe" class="app-input">
                    
                    <button onclick="handleAuth()" class="w-full bg-emerald-600 text-white font-black py-5 rounded-[25px] uppercase tracking-widest btn-press shadow-xl mt-4">Confirmer</button>
                    
                    <button onclick="toggleAuthMode()" id="auth-toggle" class="text-[10px] font-black uppercase text-slate-400 underline tracking-[0.2em] pt-4">Nouveau ? Créer un compte</button>
                </div>
            </div>
        </div>

    </main>

    <!-- TAB BAR FIXE (STYLE IOS/ANDROID) -->
    <nav class="fixed bottom-0 left-0 right-0 h-24 glass border-t border-slate-100 flex items-center justify-around px-8 z-[1000] pb-4">
        <button onclick="navigateTo('home')" class="flex flex-col items-center gap-1.5 transition-all active:scale-90">
            <i class="fa-solid fa-grid-2 text-xl text-slate-400"></i>
            <span class="text-[8px] font-black uppercase tracking-widest text-slate-400">Marché</span>
        </button>
        <button onclick="navigateTo('publish')" class="flex flex-col items-center gap-1.5 transition-all active:scale-90">
            <div class="w-12 h-12 bg-emerald-600 rounded-2xl flex items-center justify-center text-white shadow-lg shadow-emerald-200 -mt-8 border-4 border-white">
                <i class="fa-solid fa-plus text-xl"></i>
            </div>
            <span class="text-[8px] font-black uppercase tracking-widest text-emerald-600 mt-1">Vendre</span>
        </button>
        <button onclick="navigateTo('cart')" class="relative flex flex-col items-center gap-1.5 transition-all active:scale-90">
            <i class="fa-solid fa-cart-shopping text-xl text-slate-400"></i>
            <span id="badge-cart" class="hidden absolute -top-2 -right-2 bg-red-500 text-white text-[8px] w-5 h-5 rounded-full flex items-center justify-center border-2 border-white font-black">0</span>
            <span class="text-[8px] font-black uppercase tracking-widest text-slate-400">Panier</span>
        </button>
        <button onclick="navigateTo('profile')" class="flex flex-col items-center gap-1.5 transition-all active:scale-90">
            <i class="fa-solid fa-user-tag text-xl text-slate-400"></i>
            <span class="text-[8px] font-black uppercase tracking-widest text-slate-400">Compte</span>
        </button>
    </nav>

    <!-- MODAL RECHARGE -->
    <div id="modal-recharge" class="hidden fixed inset-0 z-[2000] bg-slate-900/80 backdrop-blur-md flex items-center justify-center p-6">
        <div class="bg-white rounded-[45px] p-10 w-full max-w-sm shadow-2xl relative">
            <button onclick="closeRecharge()" class="absolute top-6 right-6 text-slate-300 hover:text-slate-900"><i class="fa-solid fa-circle-xmark text-2xl"></i></button>
            <h3 class="font-black text-xl uppercase text-center mb-8 italic">Créditer <span class="text-emerald-600">EchoppePay</span></h3>
            <div class="space-y-6">
                <div class="bg-slate-50 p-6 rounded-3xl text-center border border-slate-100">
                    <p class="text-[9px] font-black uppercase text-slate-400 mb-2">Airtel Money / Moov Money</p>
                    <p class="font-black text-xl tracking-tighter">Transfert au <span class="text-emerald-600">077 73 60 65</span></p>
                </div>
                <div class="space-y-2">
                    <label class="ml-4 text-[9px] font-black text-slate-400 uppercase">Montant reçu</label>
                    <input type="number" id="topup-amount" placeholder="0 FCFA" class="app-input text-center text-2xl py-6">
                </div>
                <button onclick="processRecharge()" class="w-full bg-emerald-600 text-white font-black py-5 rounded-[25px] uppercase tracking-widest shadow-xl btn-press">Valider le dépôt</button>
            </div>
        </div>
    </div>

    <!-- TOAST NOTIFICATION -->
    <div id="toast" class="hidden fixed bottom-28 left-1/2 -translate-x-1/2 bg-slate-900/90 backdrop-blur-md text-white px-8 py-4 rounded-full text-[10px] font-black uppercase tracking-[0.2em] z-[3000] shadow-2xl border border-white/10 text-center min-w-[200px]"></div>

    <!-- SCRIPTS CORE -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, query, onSnapshot, addDoc, updateDoc, serverTimestamp, increment, runTransaction } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // CONFIGURATION OFFICIELLE (TES CLÉS)
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = "echoppe241-prod-final";

        // GLOBAL STATE
        let currentUser = null;
        let cart = JSON.parse(localStorage.getItem('echoppe_cart_v2') || '[]');
        let authMode = 'login';
        let filters = { cat: 'all', prov: 'all' };
        let tempProdImg = null;
        const provinces = ["Estuaire", "Haut-Ogooué", "Moyen-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Ogooué-Maritime", "Woleu-Ntem"];

        // 1. SYSTEME PWA & SERVICE WORKER (EMULÉ POUR LE WEB)
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                // Création d'un Service Worker fictif pour le support PWA immédiat
                const blob = new Blob([`self.addEventListener('fetch', function(event) { event.respondWith(fetch(event.request)); });`], {type: 'text/javascript'});
                const swUrl = URL.createObjectURL(blob);
                navigator.serviceWorker.register(swUrl).catch(err => console.log("SW error", err));
            });
        }

        // 2. LOADER DE PRODUCTION
        function startAppSequence() {
            const bar = document.getElementById('loader-bar');
            setTimeout(() => bar.style.width = "40%", 100);
            setTimeout(() => bar.style.width = "80%", 400);
            setTimeout(() => {
                bar.style.width = "100%";
                setTimeout(() => {
                    document.getElementById('app-loader').style.opacity = '0';
                    setTimeout(() => document.getElementById('app-loader').style.display = 'none', 500);
                }, 300);
            }, 800);
        }

        // 3. NAVIGATION NATIVE-LIKE
        window.navigateTo = (v) => {
            const protectedViews = ['profile', 'publish'];
            if(protectedViews.includes(v) && (!currentUser || auth.currentUser.isAnonymous)) {
                return navigateTo('auth');
            }
            
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            
            // Re-render data if needed
            if(v === 'home') loadMarket();
            if(v === 'cart') renderCart();
            if(v === 'profile') loadOrders();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };

        // 4. GESTION AUTHENTIFICATION (PROD)
        window.toggleAuthMode = () => {
            authMode = authMode === 'login' ? 'signup' : 'login';
            document.getElementById('auth-title').innerText = authMode === 'login' ? 'Heureux de vous revoir' : 'Créez votre compte';
            document.getElementById('signup-fields').classList.toggle('hidden', authMode === 'login');
            document.getElementById('auth-toggle').innerText = authMode === 'login' ? 'Pas de compte ? S\'inscrire' : 'Déjà inscrit ? Connexion';
        };

        window.handleAuth = async () => {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;
            if(!email || pass.length < 6) return showToast("Saisie incorrecte");

            try {
                if(authMode === 'login') {
                    await signInWithEmailAndPassword(auth, email, pass);
                } else {
                    const name = document.getElementById('auth-name').value;
                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid), {
                        uid: res.user.uid,
                        fullName: name || "Utilisateur Gabonais",
                        email: email,
                        walletBalance: 0,
                        avatar: `https://ui-avatars.com/api/?name=${name}&background=00853f&color=fff&size=256`,
                        createdAt: serverTimestamp(),
                        role: 'user'
                    });
                }
                navigateTo('home');
                showToast("Connexion réussie");
            } catch(e) { 
                showToast("Accès refusé. Vérifiez vos infos."); 
            }
        };

        onAuthStateChanged(auth, (user) => {
            if(user && !user.isAnonymous) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', user.uid), (snap) => {
                    if(snap.exists()) {
                        currentUser = snap.data();
                        updateHeaderUI();
                        syncProfileData();
                    }
                });
            } else {
                if(!user) signInAnonymously(auth);
                currentUser = null;
                updateHeaderUI();
            }
            startAppSequence();
        });

        function updateHeaderUI() {
            const isRealUser = currentUser && !auth.currentUser.isAnonymous;
            document.getElementById('profile-zone').classList.toggle('hidden', !isRealUser);
            document.getElementById('btn-login-head').classList.toggle('hidden', isRealUser);
            if(isRealUser) {
                document.getElementById('nav-avatar').src = currentUser.avatar;
                document.getElementById('nav-balance').innerText = `${currentUser.walletBalance.toLocaleString()} F`;
            }
            updateBadgeUI();
        }

        // 5. MARCHE & FILTRES
        function loadMarket() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                let items = snap.docs.map(d => ({id: d.id, ...d.data()}));
                
                // Appliquer filtres
                if(filters.cat !== 'all') items = items.filter(i => i.category === filters.cat);
                if(filters.prov !== 'all') items = items.filter(i => i.province === filters.prov);

                const grid = document.getElementById('market-grid');
                if(items.length === 0) {
                    grid.innerHTML = `<div class="col-span-full py-32 text-center"><i class="fa-solid fa-box-open text-4xl text-slate-100 mb-4 block"></i><p class="font-black uppercase text-[10px] text-slate-300">Aucun article ici</p></div>`;
                    return;
                }

                grid.innerHTML = items.map(p => `
                    <div class="bg-white rounded-[32px] p-2.5 border border-slate-50 shadow-sm transition-all hover:shadow-xl hover:-translate-y-1">
                        <div class="h-44 rounded-[26px] overflow-hidden bg-slate-50 relative">
                            <img src="${p.image}" class="w-full h-full object-cover">
                            <span class="absolute top-3 left-3 bg-white/90 backdrop-blur-md text-[8px] font-black px-3 py-1.5 rounded-full text-emerald-800 uppercase shadow-sm">${p.category}</span>
                        </div>
                        <div class="p-3">
                            <h3 class="font-black text-[11px] uppercase truncate text-slate-800">${p.title}</h3>
                            <div class="flex items-center gap-1 text-slate-400 mb-3">
                                <i class="fa-solid fa-location-dot text-[8px]"></i>
                                <span class="text-[9px] font-bold uppercase">${p.province}</span>
                            </div>
                            <div class="flex justify-between items-center bg-slate-50 p-2 rounded-2xl">
                                <span class="text-emerald-700 font-black text-sm">${p.price.toLocaleString()} F</span>
                                <button onclick="addToCart(${JSON.stringify(p).replace(/"/g, '&quot;')})" class="w-9 h-9 bg-emerald-600 rounded-xl text-white shadow-lg shadow-emerald-100 transition-all active:scale-90 flex items-center justify-center">
                                    <i class="fa-solid fa-cart-plus text-xs"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `).join('');
            });
        }

        window.setCategory = (c) => {
            filters.cat = c;
            document.querySelectorAll('.cat-btn').forEach(b => {
                const isMatch = b.innerText.toLowerCase() === c.toLowerCase() || (c==='all' && b.innerText==='Tout');
                b.classList.toggle('active', isMatch);
            });
            loadMarket();
        };

        window.setProvince = (p) => {
            filters.prov = p;
            document.querySelectorAll('.prov-btn').forEach(b => b.classList.toggle('active', b.innerText === p));
            loadMarket();
        };

        // 6. PANIER & TRANSACTIONS
        window.addToCart = (p) => {
            if(!currentUser || auth.currentUser.isAnonymous) return navigateTo('auth');
            if(p.sellerId === currentUser.uid) return showToast("Vous êtes le vendeur !");
            
            cart.push(p);
            localStorage.setItem('echoppe_cart_v2', JSON.stringify(cart));
            updateBadgeUI();
            showToast("Ajouté au panier");
        };

        function updateBadgeUI() {
            const b = document.getElementById('badge-cart');
            b.innerText = cart.length;
            b.classList.toggle('hidden', cart.length === 0);
        }

        function renderCart() {
            const list = document.getElementById('cart-items');
            const foot = document.getElementById('cart-footer');
            const empty = document.getElementById('cart-empty');

            if(cart.length === 0) { list.innerHTML = ''; foot.classList.add('hidden'); empty.classList.remove('hidden'); return; }

            empty.classList.add('hidden');
            foot.classList.remove('hidden');
            let total = 0;
            list.innerHTML = cart.map((item, idx) => {
                total += item.price;
                return `
                    <div class="bg-white p-5 rounded-[30px] flex items-center gap-5 shadow-sm border border-slate-50">
                        <img src="${item.image}" class="w-16 h-16 rounded-2xl object-cover shadow-sm">
                        <div class="flex-1">
                            <p class="font-black text-xs uppercase text-slate-800 truncate">${item.title}</p>
                            <p class="text-emerald-600 font-black text-sm">${item.price.toLocaleString()} F</p>
                        </div>
                        <button onclick="removeFromCart(${idx})" class="w-10 h-10 bg-red-50 text-red-400 rounded-full flex items-center justify-center transition-all active:scale-90">
                            <i class="fa-solid fa-trash-can text-sm"></i>
                        </button>
                    </div>
                `;
            }).join('');
            document.getElementById('cart-total').innerText = `${total.toLocaleString()} F`;
        }

        window.removeFromCart = (i) => { cart.splice(i, 1); localStorage.setItem('echoppe_cart_v2', JSON.stringify(cart)); renderCart(); updateBadgeUI(); };

        window.checkout = async () => {
            const total = cart.reduce((acc, i) => acc + i.price, 0);
            if(currentUser.walletBalance < total) return showToast("Solde EchoppePay insuffisant");

            const btn = document.getElementById('btn-pay');
            btn.disabled = true; btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin"></i> Paiement...`;

            try {
                await runTransaction(db, async (txn) => {
                    const buyerRef = doc(db, 'artifacts', appId, 'users', currentUser.uid);
                    txn.update(buyerRef, { walletBalance: increment(-total) });

                    for(const item of cart) {
                        // Créditer le vendeur
                        const sellerRef = doc(db, 'artifacts', appId, 'users', item.sellerId);
                        txn.update(sellerRef, { walletBalance: increment(item.price) });
                        
                        // Enregistrer la commande
                        const orderRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'));
                        txn.set(orderRef, {
                            buyerId: currentUser.uid, 
                            sellerId: item.sellerId,
                            title: item.title, 
                            price: item.price, 
                            image: item.image,
                            timestamp: serverTimestamp()
                        });
                    }
                });
                cart = []; localStorage.removeItem('echoppe_cart_v2');
                updateBadgeUI(); showToast("Achat validé !"); navigateTo('profile');
            } catch(e) { 
                showToast("Échec de la transaction"); 
            } finally { 
                btn.disabled = false; btn.innerHTML = `<i class="fa-solid fa-shield-check"></i> Payer via EchoppePay`; 
            }
        };

        // 7. VENTES (PUBLICATION)
        document.getElementById('prod-img-input').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = (re) => {
                tempProdImg = re.target.result;
                document.getElementById('upload-preview').innerHTML = `<img src="${tempProdImg}" class="w-full h-full object-cover">`;
            };
            reader.readAsDataURL(file);
        });

        window.publishArticle = async () => {
            const title = document.getElementById('prod-title').value;
            const price = parseInt(document.getElementById('prod-price').value);
            const cat = document.getElementById('prod-cat').value;
            const prov = document.getElementById('prod-province').value;

            if(!title || !price || !tempProdImg) return showToast("Informations incomplètes");

            const btn = document.getElementById('btn-publish');
            btn.disabled = true; btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin"></i> Publication...`;

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    title, price, category: cat, province: prov, image: tempProdImg,
                    sellerId: currentUser.uid, at: serverTimestamp()
                });
                showToast("Votre article est en ligne !");
                navigateTo('home');
                // Clean
                document.getElementById('prod-title').value = '';
                document.getElementById('prod-price').value = '';
                tempProdImg = null;
                document.getElementById('upload-preview').innerHTML = `<i class="fa-solid fa-image text-5xl text-slate-200 mb-3"></i><span class="text-[10px] font-black text-slate-400 uppercase tracking-widest">Choisir une image claire</span>`;
            } catch(e) { showToast("Erreur serveur"); }
            finally { btn.disabled = false; btn.innerHTML = `<i class="fa-solid fa-rocket"></i> Publier l'article`; }
        };

        // 8. PROFIL & WALLET
        function syncProfileData() {
            document.getElementById('p-avatar').src = currentUser.avatar;
            document.getElementById('p-name').value = currentUser.fullName;
            document.getElementById('p-email').innerText = currentUser.email;
            document.getElementById('p-balance').innerText = `${currentUser.walletBalance.toLocaleString()} FCFA`;
        }

        window.updateProfileName = async () => {
            const n = document.getElementById('p-name').value;
            if(!n) return;
            await updateDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid), { fullName: n });
            showToast("Profil mis à jour");
        };

        document.getElementById('avatar-input').addEventListener('change', (e) => {
            const file = e.target.files[0];
            const reader = new FileReader();
            reader.onload = async (re) => {
                const data = re.target.result;
                document.getElementById('p-avatar').src = data;
                await updateDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid), { avatar: data });
                showToast("Photo de profil mise à jour");
            };
            reader.readAsDataURL(file);
        });

        window.openRecharge = () => document.getElementById('modal-recharge').classList.remove('hidden');
        window.closeRecharge = () => document.getElementById('modal-recharge').classList.add('hidden');
        window.processRecharge = async () => {
            const v = parseInt(document.getElementById('topup-amount').value);
            if(!v || v < 100) return showToast("Montant invalide");
            await updateDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid), { walletBalance: increment(v) });
            showToast("Solde EchoppePay crédité !");
            closeRecharge();
        };

        function loadOrders() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), (snap) => {
                const list = document.getElementById('orders-list');
                const myOrders = snap.docs.map(d => d.data()).filter(o => o.buyerId === currentUser.uid || o.sellerId === currentUser.uid);
                
                if(myOrders.length === 0) { 
                    list.innerHTML = `<div class="bg-white p-8 rounded-3xl text-center border border-slate-50"><p class="text-[9px] font-black text-slate-300 uppercase">Aucune activité</p></div>`; 
                    return; 
                }

                list.innerHTML = myOrders.sort((a,b) => b.timestamp - a.timestamp).map(o => {
                    const isBuyer = o.buyerId === currentUser.uid;
                    return `
                        <div class="bg-white p-4 rounded-3xl border border-slate-50 flex items-center gap-4 shadow-sm">
                            <div class="w-12 h-12 rounded-2xl overflow-hidden shadow-sm">
                                <img src="${o.image}" class="w-full h-full object-cover">
                            </div>
                            <div class="flex-1">
                                <p class="font-black text-[10px] uppercase truncate text-slate-800">${o.title}</p>
                                <p class="text-[8px] font-bold text-slate-400">${isBuyer ? 'ACHAT' : 'VENTE'} • ${new Date(o.timestamp?.seconds * 1000).toLocaleDateString()}</p>
                            </div>
                            <span class="font-black text-xs ${isBuyer ? 'text-red-500' : 'text-emerald-600'}">
                                ${isBuyer ? '-' : '+'}${o.price.toLocaleString()} F
                            </span>
                        </div>
                    `;
                }).join('');
            });
        }

        // HELPERS
        function showToast(m) {
            const t = document.getElementById('toast');
            t.innerText = m; t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        const initInterface = () => {
            const pSel = document.getElementById('prod-province');
            const pFilt = document.getElementById('prov-filters');
            
            // Filter "All"
            const bAll = document.createElement('button');
            bAll.innerText = "Tout le Gabon";
            bAll.className = "prov-btn active whitespace-nowrap border border-slate-100 px-6 py-3 rounded-2xl font-black text-[9px] uppercase shadow-sm bg-white";
            bAll.onclick = () => setProvince('all');
            pFilt.appendChild(bAll);

            provinces.forEach(p => {
                const opt = document.createElement('option');
                opt.value = p; opt.innerText = p;
                pSel.appendChild(opt);

                const b = document.createElement('button');
                b.innerText = p;
                b.className = "prov-btn whitespace-nowrap border border-slate-100 px-6 py-3 rounded-2xl font-black text-[9px] uppercase shadow-sm bg-white";
                b.onclick = () => setProvince(p);
                pFilt.appendChild(b);
            });
        };

        window.handleLogout = () => signOut(auth).then(() => location.reload());

        initInterface();
    </script>
</body>
</html>
