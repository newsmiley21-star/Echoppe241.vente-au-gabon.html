<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Officiel</title> 
    
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { --gabon-green: #00853f; --gabon-yellow: #fcd116; --gabon-blue: #3a75c4; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; overflow-x: hidden; }
        
        /* Loader Tendance - Glassmorphism */
        #app-loader {
            position: fixed; inset: 0; z-index: 10000;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(15px); -webkit-backdrop-filter: blur(15px);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 1s ease, visibility 1s;
        }
        .loader-logo { animation: pulseLogo 2s infinite ease-in-out; width: 100px; }
        @keyframes pulseLogo { 
            0%, 100% { transform: scale(1); filter: drop-shadow(0 0 0px #00853f); } 
            50% { transform: scale(1.1); filter: drop-shadow(0 0 25px rgba(0,133,63,0.4)); } 
        }

        .glass-nav { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px); }
        .view { display: none; opacity: 0; transform: translateY(15px); transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1); }
        .view.active { display: block; opacity: 1; transform: translateY(0); }
        
        .cat-btn.active { background: var(--gabon-green); color: white; border-color: var(--gabon-green); }
        .prov-btn.active { background: #f8fafc; border-color: var(--gabon-green); color: var(--gabon-green); }

        .btn-press:active { transform: scale(0.95); }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        
        /* Custom Input Styling */
        input:focus { outline: none; box-shadow: 0 0 0 3px rgba(0, 133, 63, 0.1); }
    </style>
</head>
<body class="bg-slate-50 text-slate-900">

    <!-- ÉCRAN DE CHARGEMENT INITIAL -->
    <div id="app-loader">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="loader-logo mb-8">
        <div class="w-64 h-1 bg-slate-100 rounded-full overflow-hidden">
            <div id="loader-bar" class="h-full bg-emerald-600 w-0 transition-all duration-700 ease-out"></div>
        </div>
        <p class="mt-4 text-[11px] font-bold uppercase tracking-[0.3em] text-slate-400 italic">Plateforme Sécurisée</p>
    </div>

    <!-- HEADER NAVIGATION -->
    <nav class="sticky top-0 z-[100] glass-nav border-b border-slate-100 px-5 h-20 flex justify-between items-center">
        <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 h-10">
            <div class="flex flex-col">
                <span class="font-extrabold text-xl tracking-tighter uppercase italic leading-none">Echoppe<span class="text-emerald-600">241</span></span>
                <span class="text-[8px] font-black tracking-widest text-slate-400 uppercase">Le Gabon Proche</span>
            </div>
        </div>
        
        <div class="flex items-center gap-5">
            <button onclick="navigateTo('cart')" class="relative p-2 btn-press">
                <i class="fa-solid fa-cart-shopping text-xl text-slate-600"></i>
                <span id="cart-count" class="hidden absolute top-0 right-0 bg-red-500 text-white text-[9px] font-black w-4 h-4 rounded-full flex items-center justify-center border-2 border-white">0</span>
            </button>
            <div id="auth-nav-zone">
                <button id="btn-login-header" onclick="navigateTo('auth')" class="bg-emerald-600 text-white px-6 py-2.5 rounded-full text-[10px] font-black uppercase shadow-md hover:bg-emerald-700 transition-colors">Connexion</button>
                <div id="profile-trigger" class="hidden flex items-center gap-2 cursor-pointer btn-press" onclick="navigateTo('profile')">
                    <img id="nav-avatar" src="https://ui-avatars.com/api/?name=User" class="w-10 h-10 rounded-full border-2 border-white shadow-sm object-cover">
                </div>
            </div>
        </div>
    </nav>

    <!-- MAIN VIEWS CONTAINER -->
    <main class="min-h-screen pb-32">
        
        <!-- ACCUEIL -->
        <div id="view-home" class="view active max-w-7xl mx-auto p-4 md:p-8">
            <div class="mb-10">
                <h1 class="text-5xl font-black italic uppercase leading-none mb-8 tracking-tighter">Tout le Marché <br><span class="text-emerald-600">Gabonais</span></h1>
                
                <!-- Filtres Catégories -->
                <div class="flex gap-3 overflow-x-auto pb-6 no-scrollbar">
                    <button onclick="setCategory('all')" class="cat-btn active whitespace-nowrap border px-8 py-4 rounded-2xl font-black text-[10px] uppercase transition-all shadow-sm bg-white">Tout</button>
                    <button onclick="setCategory('Agriculture')" class="cat-btn whitespace-nowrap border px-8 py-4 rounded-2xl font-black text-[10px] uppercase transition-all shadow-sm bg-white">Agriculture</button>
                    <button onclick="setCategory('Pêche')" class="cat-btn whitespace-nowrap border px-8 py-4 rounded-2xl font-black text-[10px] uppercase transition-all shadow-sm bg-white">Pêche</button>
                    <button onclick="setCategory('Artisanat')" class="cat-btn whitespace-nowrap border px-8 py-4 rounded-2xl font-black text-[10px] uppercase transition-all shadow-sm bg-white">Artisanat</button>
                    <button onclick="setCategory('Vêtements')" class="cat-btn whitespace-nowrap border px-8 py-4 rounded-2xl font-black text-[10px] uppercase transition-all shadow-sm bg-white">Vêtements</button>
                </div>

                <!-- Filtres Provinces -->
                <div class="flex gap-2 overflow-x-auto pb-4 no-scrollbar" id="province-filters">
                    <!-- Injecté par JS -->
                </div>
            </div>

            <div id="market-grid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 md:gap-8">
                <!-- Les produits chargeront ici -->
            </div>
        </div>

        <!-- PANIER & ECHOPPEPAY -->
        <div id="view-cart" class="view max-w-2xl mx-auto p-6 mt-6">
            <h2 class="text-3xl font-black uppercase italic mb-8">Votre <span class="text-emerald-600">Panier</span></h2>
            <div id="cart-list" class="space-y-4 mb-10"></div>
            
            <div id="cart-summary" class="bg-white p-8 rounded-[40px] shadow-2xl border hidden">
                <div class="flex justify-between items-center mb-6">
                    <span class="font-bold text-slate-400 uppercase text-xs">Total à payer</span>
                    <span id="cart-total" class="text-3xl font-black text-emerald-600">0 FCFA</span>
                </div>
                <button onclick="processOrder()" id="btn-checkout" class="w-full bg-emerald-600 text-white font-black py-5 rounded-3xl shadow-xl uppercase tracking-widest btn-press">Finaliser avec EchoppePay</button>
                <p class="text-[10px] text-center mt-4 text-slate-400 font-bold uppercase">🔒 Transaction Sécurisée par Echoppe241</p>
            </div>
            
            <div id="cart-empty" class="text-center py-24">
                <i class="fa-solid fa-cart-arrow-down text-5xl text-slate-100 mb-6"></i>
                <p class="opacity-30 font-black uppercase text-xs">Le panier est vide</p>
            </div>
        </div>

        <!-- PUBLIER UNE ANNONCE -->
        <div id="view-publish" class="view max-w-2xl mx-auto p-6 mt-6">
            <div class="bg-white rounded-[40px] p-8 md:p-12 shadow-2xl">
                <h2 class="text-3xl font-black uppercase italic mb-8">Vendre un <span class="text-emerald-600">Article</span></h2>
                <div class="space-y-6">
                    <!-- Photo -->
                    <div id="publish-upload" onclick="document.getElementById('file-prod').click()" class="w-full h-56 bg-slate-50 rounded-[35px] border-4 border-dashed border-slate-100 flex flex-col items-center justify-center cursor-pointer overflow-hidden transition-all hover:border-emerald-200">
                        <div id="preview-container" class="flex flex-col items-center">
                            <i class="fa-solid fa-image text-4xl text-slate-200 mb-3"></i>
                            <p class="text-[10px] font-black text-slate-300 uppercase">Télécharger une photo claire</p>
                        </div>
                        <input type="file" id="file-prod" class="hidden" accept="image/*">
                    </div>

                    <!-- Champs -->
                    <input type="text" id="prod-name" placeholder="Nom de l'article (ex: Atanga de Moanda)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold">
                    
                    <div class="grid grid-cols-2 gap-4">
                        <div class="relative">
                            <input type="number" id="prod-price" placeholder="Prix" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold pr-16">
                            <span class="absolute right-5 top-1/2 -translate-y-1/2 font-black text-slate-300 text-xs">FCFA</span>
                        </div>
                        <select id="prod-cat" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold outline-none appearance-none">
                            <option value="Agriculture">Agriculture</option>
                            <option value="Pêche">Pêche</option>
                            <option value="Artisanat">Artisanat</option>
                            <option value="Vêtements">Vêtements</option>
                            <option value="Services">Services</option>
                        </select>
                    </div>

                    <select id="prod-province" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold outline-none appearance-none">
                        <!-- Provinces -->
                    </select>

                    <button onclick="publishArticle()" id="btn-publish-final" class="w-full bg-emerald-600 text-white font-black py-5 rounded-3xl shadow-xl uppercase tracking-widest btn-press">Mettre en vente</button>
                </div>
            </div>
        </div>

        <!-- PROFIL & ECHOPPEPAY WALLET -->
        <div id="view-profile" class="view max-w-2xl mx-auto p-6 md:p-8">
            <!-- Header Profil -->
            <div class="bg-white rounded-[40px] p-8 shadow-xl mb-6 flex flex-col items-center text-center">
                <div class="relative group cursor-pointer mb-6" onclick="document.getElementById('file-avatar').click()">
                    <img id="p-avatar-big" src="https://ui-avatars.com/api/?name=User" class="w-32 h-32 rounded-full border-4 border-white shadow-2xl object-cover">
                    <div class="absolute inset-0 bg-black/40 rounded-full opacity-0 group-hover:opacity-100 flex items-center justify-center transition-all">
                        <i class="fa-solid fa-camera text-white"></i>
                    </div>
                    <input type="file" id="file-avatar" class="hidden" accept="image/*">
                </div>
                
                <div class="flex items-center gap-3 mb-1">
                    <input type="text" id="p-full-name" class="text-2xl font-black uppercase border-none focus:ring-0 bg-transparent text-center w-full" value="Utilisateur">
                    <button onclick="saveProfileName()" class="text-emerald-600"><i class="fa-solid fa-check"></i></button>
                </div>
                <p id="p-email" class="text-slate-400 text-xs font-bold uppercase tracking-widest mb-8"></p>

                <!-- Wallet Card -->
                <div class="w-full bg-gradient-to-br from-emerald-600 to-emerald-800 rounded-[35px] p-8 text-white shadow-2xl shadow-emerald-200/50">
                    <div class="flex justify-between items-start mb-6">
                        <span class="text-[10px] font-black uppercase tracking-[0.2em] opacity-80">Portefeuille EchoppePay</span>
                        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-8 h-8 brightness-0 invert">
                    </div>
                    <h2 id="p-balance" class="text-4xl font-black italic mb-6">0 FCFA</h2>
                    <button onclick="openRecharge()" class="w-full bg-white text-emerald-900 font-black py-4 rounded-2xl text-[10px] uppercase tracking-widest btn-press">Recharger mon compte</button>
                </div>
            </div>

            <!-- Historique -->
            <h3 class="font-black uppercase text-[10px] text-slate-400 tracking-widest mb-4 px-4">Mes Acquisitions</h3>
            <div id="orders-list" class="space-y-3"></div>

            <button onclick="logout()" class="w-full mt-10 py-5 bg-red-50 text-red-600 font-black rounded-3xl text-[10px] uppercase btn-press">Déconnexion</button>
        </div>

        <!-- AUTHENTIFICATION -->
        <div id="view-auth" class="view max-w-md mx-auto p-6 mt-12">
            <div class="bg-white rounded-[45px] p-10 shadow-2xl text-center">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-20 mx-auto mb-8">
                <h2 id="auth-header" class="text-3xl font-black uppercase italic mb-8">Espace <span class="text-emerald-600">Membre</span></h2>
                
                <div class="space-y-4">
                    <div id="auth-name-box" class="hidden">
                        <input type="text" id="auth-name" placeholder="Nom Complet" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold">
                    </div>
                    <input type="email" id="auth-email" placeholder="Email" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold">
                    <input type="password" id="auth-pass" placeholder="Mot de passe" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold">
                    <button onclick="submitAuth()" id="btn-auth-main" class="w-full bg-emerald-600 text-white font-black py-5 rounded-2xl shadow-xl uppercase tracking-widest btn-press">Entrer</button>
                    <button onclick="toggleAuth()" id="btn-auth-toggle" class="w-full text-[10px] font-black uppercase text-slate-400 mt-6 underline">Créer un nouveau compte</button>
                </div>
            </div>
        </div>

    </main>

    <!-- BARRE DE NAVIGATION BASSE (MOBILE FIRST) -->
    <nav class="fixed bottom-0 left-0 right-0 glass-nav border-t h-20 flex items-center justify-around px-4 z-[1000]">
        <button onclick="navigateTo('home')" class="flex flex-col items-center gap-1">
            <i class="fa-solid fa-store text-xl text-slate-400"></i>
            <span class="text-[8px] font-black uppercase tracking-tighter">Marché</span>
        </button>
        <button onclick="navigateTo('chat-list')" class="flex flex-col items-center gap-1">
            <i class="fa-solid fa-message text-xl text-slate-400"></i>
            <span class="text-[8px] font-black uppercase tracking-tighter">Messages</span>
        </button>
        <button onclick="navigateTo('publish')" class="flex flex-col items-center -translate-y-6">
            <div class="bg-emerald-600 w-14 h-14 rounded-full flex items-center justify-center text-white shadow-2xl border-4 border-white">
                <i class="fa-solid fa-add text-xl"></i>
            </div>
            <span class="text-[8px] font-black uppercase mt-1 text-emerald-600">Vendre</span>
        </button>
        <button onclick="navigateTo('cart')" class="flex flex-col items-center gap-1">
            <i class="fa-solid fa-bag-shopping text-xl text-slate-400"></i>
            <span class="text-[8px] font-black uppercase tracking-tighter">Panier</span>
        </button>
        <button onclick="navigateTo('profile')" class="flex flex-col items-center gap-1">
            <i class="fa-solid fa-user-circle text-xl text-slate-400"></i>
            <span class="text-[8px] font-black uppercase tracking-tighter">Profil</span>
        </button>
    </nav>

    <!-- MODAL RECHARGE ECHOPPEPAY -->
    <div id="modal-recharge" class="hidden fixed inset-0 z-[2000] bg-slate-900/80 backdrop-blur-sm flex items-center justify-center p-6">
        <div class="bg-white rounded-[40px] p-8 max-w-sm w-full shadow-2xl">
            <h3 class="font-black uppercase italic text-2xl text-center mb-6">Alimenter <br><span class="text-emerald-600">EchoppePay</span></h3>
            <div class="space-y-5">
                <div class="p-4 bg-emerald-50 rounded-2xl border border-emerald-100">
                    <p class="text-[8px] font-black uppercase text-emerald-700 opacity-60">Référence de paiement</p>
                    <p class="font-black text-emerald-800">AIRTEL MONEY : 077 73 60 65</p>
                </div>
                <input type="number" id="recharge-val" placeholder="Montant (FCFA)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-black text-xl text-center">
                <input type="text" id="recharge-id" placeholder="ID Transaction / Code" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold uppercase text-center">
                <button onclick="confirmRecharge()" class="w-full bg-emerald-600 text-white font-black py-5 rounded-2xl uppercase shadow-lg btn-press">Valider Recharge</button>
                <button onclick="closeRecharge()" class="w-full text-slate-400 font-bold text-[10px] uppercase">Annuler</button>
            </div>
        </div>
    </div>

    <!-- TOAST NOTIFICATION -->
    <div id="toast" class="hidden fixed top-24 left-1/2 -translate-x-1/2 z-[3000] bg-slate-900 text-white px-8 py-4 rounded-3xl shadow-2xl flex items-center gap-3 transition-all">
        <i id="toast-icon" class="fa-solid fa-circle-check text-emerald-400"></i>
        <p id="toast-msg" class="text-[10px] font-black uppercase tracking-widest"></p>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, serverTimestamp, updateDoc, increment, query, where, orderBy, runTransaction } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // CONFIGURATION FIREBASE FUSIONNÉE
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
        const appId = "echoppe241-pro-final";

        // ÉTAT DE L'APPLICATION
        let userProfile = null;
        let cart = JSON.parse(localStorage.getItem('cart_241') || '[]');
        let filters = { category: 'all', province: 'all' };
        let authMode = 'login';
        let tempProdImg = null;
        const provinces = ["Estuaire", "Haut-Ogooué", "Moyen-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Ogooué-Maritime", "Woleu-Ntem"];

        // GESTION DU LOADER
        function startApp() {
            const bar = document.getElementById('loader-bar');
            bar.style.width = "40%";
            setTimeout(() => {
                bar.style.width = "100%";
                setTimeout(() => {
                    const loader = document.getElementById('app-loader');
                    loader.style.opacity = '0';
                    setTimeout(() => loader.style.visibility = 'hidden', 1000);
                }, 800);
            }, 500);
        }

        // NAVIGATION
        window.navigateTo = (view) => {
            const isLogged = userProfile && !auth.currentUser.isAnonymous;
            if(['publish', 'profile'].includes(view) && !isLogged) return navigateTo('auth');

            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            const target = document.getElementById(`view-${view}`);
            if(target) target.classList.add('active');

            if(view === 'home') loadMarket();
            if(view === 'cart') renderCart();
            if(view === 'profile') loadUserOrders();
            window.scrollTo(0,0);
        };

        // AUTHENTIFICATION
        window.toggleAuth = () => {
            authMode = authMode === 'login' ? 'signup' : 'login';
            document.getElementById('auth-name-box').classList.toggle('hidden', authMode === 'login');
            document.getElementById('auth-header').innerHTML = authMode === 'login' ? 'Espace <span class="text-emerald-600">Membre</span>' : 'Rejoindre <span class="text-emerald-600">Echoppe241</span>';
            document.getElementById('btn-auth-toggle').innerText = authMode === 'login' ? 'Créer un nouveau compte' : 'Déjà membre ? Se connecter';
        };

        window.submitAuth = async () => {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;
            if(!email || pass.length < 6) return showToast("Informations invalides", "error");

            try {
                if(authMode === 'login') {
                    await signInWithEmailAndPassword(auth, email, pass);
                } else {
                    const name = document.getElementById('auth-name').value;
                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    const defaultAvatar = `https://ui-avatars.com/api/?name=${name}&background=00853f&color=fff&size=128`;
                    await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid), {
                        uid: res.user.uid, 
                        fullName: name, 
                        email, 
                        walletBalance: 0, 
                        avatar: defaultAvatar,
                        createdAt: serverTimestamp()
                    });
                }
                navigateTo('home');
            } catch (e) { showToast("Identifiants incorrects", "error"); }
        };

        onAuthStateChanged(auth, (user) => {
            if (user && !user.isAnonymous) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', user.uid), (snap) => {
                    if (snap.exists()) {
                        userProfile = snap.data();
                        syncUI();
                        startApp();
                    }
                });
            } else {
                if(!user) signInAnonymously(auth);
                userProfile = null;
                syncUI();
                startApp();
            }
        });

        function syncUI() {
            const isLogged = userProfile && !auth.currentUser.isAnonymous;
            document.getElementById('btn-login-header').classList.toggle('hidden', isLogged);
            document.getElementById('profile-trigger').classList.toggle('hidden', !isLogged);
            
            if (isLogged) {
                document.getElementById('nav-avatar').src = userProfile.avatar;
                document.getElementById('p-avatar-big').src = userProfile.avatar;
                document.getElementById('p-full-name').value = userProfile.fullName;
                document.getElementById('p-email').innerText = userProfile.email;
                document.getElementById('p-balance').innerText = `${userProfile.walletBalance.toLocaleString()} FCFA`;
            }
            updateCartBadge();
        }

        // PERSONNALISATION DU PROFIL
        window.saveProfileName = async () => {
            const newName = document.getElementById('p-full-name').value;
            if(!newName) return;
            await updateDoc(doc(db, 'artifacts', appId, 'users', userProfile.uid), { fullName: newName });
            showToast("Nom enregistré !");
        };

        document.getElementById('file-avatar').addEventListener('change', (e) => {
            const file = e.target.files[0];
            const reader = new FileReader();
            reader.onload = async (re) => {
                const data = re.target.result;
                document.getElementById('p-avatar-big').src = data;
                await updateDoc(doc(db, 'artifacts', appId, 'users', userProfile.uid), { avatar: data });
                showToast("Photo de profil mise à jour !");
            };
            reader.readAsDataURL(file);
        });

        // MARCHÉ & FILTRES
        function loadMarket() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const grid = document.getElementById('market-grid');
                let items = snap.docs.map(d => ({id: d.id, ...d.data()}));
                
                if(filters.category !== 'all') items = items.filter(i => i.category === filters.category);
                if(filters.province !== 'all') items = items.filter(i => i.province === filters.province);

                if(items.length === 0) {
                    grid.innerHTML = `<div class="col-span-full py-24 text-center opacity-30 font-black uppercase text-xs">Aucun article ici</div>`;
                    return;
                }

                grid.innerHTML = items.map(p => `
                    <div class="bg-white rounded-[35px] overflow-hidden shadow-sm border border-slate-100 p-2">
                        <div class="h-44 relative overflow-hidden rounded-[28px]">
                            <img src="${p.image}" class="w-full h-full object-cover">
                            <div class="absolute top-2 left-2 bg-emerald-600/80 backdrop-blur-md px-3 py-1.5 rounded-full text-[7px] text-white font-black uppercase tracking-widest">${p.category}</div>
                        </div>
                        <div class="p-4">
                            <h3 class="font-bold text-xs uppercase truncate mb-1">${p.title}</h3>
                            <div class="flex items-center gap-1 mb-3 opacity-50">
                                <i class="fa-solid fa-location-dot text-[8px]"></i>
                                <span class="text-[8px] font-black uppercase">${p.province}</span>
                            </div>
                            <div class="flex justify-between items-center">
                                <span class="text-emerald-700 font-black text-sm">${p.price.toLocaleString()} F</span>
                                <div class="flex gap-1">
                                    <button onclick="addToCart(${JSON.stringify(p).replace(/"/g, '&quot;')})" class="w-9 h-9 bg-emerald-600 rounded-full flex items-center justify-center text-white shadow-lg btn-press">
                                        <i class="fa-solid fa-plus text-[10px]"></i>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                `).join('');
            });
        }

        window.setCategory = (cat) => {
            filters.category = cat;
            document.querySelectorAll('.cat-btn').forEach(b => {
                const isMatch = b.innerText.toLowerCase() === cat.toLowerCase() || (cat === 'all' && b.innerText === 'Tout');
                b.classList.toggle('active', isMatch);
            });
            loadMarket();
        };

        window.setProvince = (prov) => {
            filters.province = prov;
            document.querySelectorAll('.prov-btn').forEach(b => {
                b.classList.toggle('active', b.innerText === prov);
            });
            loadMarket();
        };

        // PANIER
        window.addToCart = (p) => {
            if(!userProfile || auth.currentUser.isAnonymous) return navigateTo('auth');
            if(p.sellerId === userProfile.uid) return showToast("C'est votre article", "error");
            
            cart.push(p);
            localStorage.setItem('cart_241', JSON.stringify(cart));
            updateCartBadge();
            showToast("Ajouté au panier");
        };

        function updateCartBadge() {
            const badge = document.getElementById('cart-count');
            badge.innerText = cart.length;
            badge.classList.toggle('hidden', cart.length === 0);
        }

        function renderCart() {
            const list = document.getElementById('cart-list');
            const sum = document.getElementById('cart-summary');
            const empty = document.getElementById('cart-empty');
            
            if(cart.length === 0) { list.innerHTML = ''; sum.classList.add('hidden'); empty.classList.remove('hidden'); return; }
            
            empty.classList.add('hidden');
            sum.classList.remove('hidden');
            let total = 0;
            list.innerHTML = cart.map((item, idx) => {
                total += item.price;
                return `
                    <div class="bg-white p-5 rounded-[30px] flex items-center gap-5 shadow-sm border border-slate-100">
                        <img src="${item.image}" class="w-16 h-16 rounded-2xl object-cover">
                        <div class="flex-1">
                            <p class="font-black text-xs uppercase truncate">${item.title}</p>
                            <p class="text-emerald-600 font-bold text-sm">${item.price.toLocaleString()} FCFA</p>
                        </div>
                        <button onclick="removeCartItem(${idx})" class="text-red-300 hover:text-red-500 transition-colors"><i class="fa-solid fa-circle-xmark text-xl"></i></button>
                    </div>
                `;
            }).join('');
            document.getElementById('cart-total').innerText = `${total.toLocaleString()} FCFA`;
        }

        window.removeCartItem = (idx) => { 
            cart.splice(idx, 1); 
            localStorage.setItem('cart_241', JSON.stringify(cart)); 
            renderCart(); 
            updateCartBadge(); 
        };

        // TRANSACTIONS ECHOPPEPAY
        window.processOrder = async () => {
            const total = cart.reduce((acc, i) => acc + i.price, 0);
            if(userProfile.walletBalance < total) return showToast("Solde EchoppePay insuffisant", "error");

            const btn = document.getElementById('btn-checkout');
            btn.disabled = true; btn.innerText = "Transaction en cours...";

            try {
                await runTransaction(db, async (txn) => {
                    const buyerRef = doc(db, 'artifacts', appId, 'users', userProfile.uid);
                    txn.update(buyerRef, { walletBalance: increment(-total) });

                    for(const item of cart) {
                        const sellerRef = doc(db, 'artifacts', appId, 'users', item.sellerId);
                        txn.update(sellerRef, { walletBalance: increment(item.price) });
                        
                        const orderRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'orders'));
                        txn.set(orderRef, {
                            buyerId: userProfile.uid, sellerId: item.sellerId,
                            title: item.title, price: item.price, image: item.image,
                            timestamp: serverTimestamp()
                        });
                    }
                });
                cart = []; localStorage.removeItem('cart_241');
                updateCartBadge(); showToast("Achat réussi !"); navigateTo('profile');
            } catch (e) { showToast("Échec du paiement", "error"); } finally { btn.disabled = false; btn.innerText = "Finaliser avec EchoppePay"; }
        };

        // VENTE
        document.getElementById('file-prod').addEventListener('change', (e) => {
            const reader = new FileReader();
            reader.onload = (re) => {
                tempProdImg = re.target.result;
                document.getElementById('preview-container').innerHTML = `<img src="${tempProdImg}" class="w-full h-full object-cover scale-110">`;
            };
            reader.readAsDataURL(e.target.files[0]);
        });

        window.publishArticle = async () => {
            const title = document.getElementById('prod-name').value;
            const price = parseInt(document.getElementById('prod-price').value);
            const category = document.getElementById('prod-cat').value;
            const province = document.getElementById('prod-province').value;
            
            if(!title || !price || !tempProdImg) return showToast("Veuillez remplir tous les champs", "error");

            const btn = document.getElementById('btn-publish-final');
            btn.disabled = true; btn.innerText = "Publication...";

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                title, price, category, province, image: tempProdImg,
                sellerId: userProfile.uid, sellerName: userProfile.fullName, createdAt: serverTimestamp()
            });
            
            showToast("Votre article est en ligne !");
            navigateTo('home');
            // Reset
            document.getElementById('prod-name').value = '';
            document.getElementById('prod-price').value = '';
            tempProdImg = null;
            document.getElementById('preview-container').innerHTML = `<i class="fa-solid fa-image text-4xl text-slate-200 mb-3"></i><p class="text-[10px] font-black text-slate-300 uppercase">Télécharger une photo claire</p>`;
            btn.disabled = false; btn.innerText = "Mettre en vente";
        };

        // RECHARGE
        window.openRecharge = () => document.getElementById('modal-recharge').classList.remove('hidden');
        window.closeRecharge = () => document.getElementById('modal-recharge').classList.add('hidden');
        
        window.confirmRecharge = async () => {
            const val = parseInt(document.getElementById('recharge-val').value);
            const tid = document.getElementById('recharge-id').value;
            if(!val || !tid) return showToast("Saisir montant et code", "error");
            
            await updateDoc(doc(db, 'artifacts', appId, 'users', userProfile.uid), { walletBalance: increment(val) });
            showToast(`Crédit de ${val} FCFA reçu !`);
            closeRecharge();
        };

        function loadUserOrders() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'orders'), (snap) => {
                const list = document.getElementById('orders-list');
                const mine = snap.docs.map(d => d.data()).filter(o => o.buyerId === userProfile.uid);
                
                if(mine.length === 0) { list.innerHTML = `<p class="text-center py-10 text-[10px] font-bold uppercase opacity-20">Aucun achat pour le moment</p>`; return; }
                
                list.innerHTML = mine.map(o => `
                    <div class="bg-white p-4 rounded-3xl border border-slate-100 flex items-center gap-4">
                        <img src="${o.image}" class="w-12 h-12 rounded-xl object-cover">
                        <div class="flex-1">
                            <p class="font-black text-[10px] uppercase truncate">${o.title}</p>
                            <p class="text-emerald-600 font-bold text-[10px]">${o.price.toLocaleString()} FCFA</p>
                        </div>
                        <span class="text-[7px] font-black uppercase bg-emerald-50 text-emerald-600 px-3 py-1.5 rounded-full">Livré</span>
                    </div>
                `).join('');
            });
        }

        // INITIALISATION DES FILTRES PROVINCES
        const initProv = () => {
            const sel = document.getElementById('prod-province');
            const filt = document.getElementById('province-filters');
            
            const all = document.createElement('button');
            all.innerText = "Tout le Gabon";
            all.className = "prov-btn active whitespace-nowrap border px-6 py-3 rounded-2xl font-black text-[10px] uppercase transition-all shadow-sm bg-white";
            all.onclick = () => setProvince('all');
            filt.appendChild(all);

            provinces.forEach(p => {
                const opt = document.createElement('option');
                opt.value = p; opt.innerText = p;
                sel.appendChild(opt);

                const b = document.createElement('button');
                b.innerText = p;
                b.className = "prov-btn whitespace-nowrap border px-6 py-3 rounded-2xl font-black text-[10px] uppercase transition-all shadow-sm bg-white";
                b.onclick = () => setProvince(p);
                filt.appendChild(b);
            });
        };

        function showToast(msg, type = 'success') {
            const t = document.getElementById('toast');
            const msgEl = document.getElementById('toast-msg');
            const icon = document.getElementById('toast-icon');
            
            msgEl.innerText = msg;
            t.className = `fixed top-24 left-1/2 -translate-x-1/2 z-[3000] text-white px-8 py-4 rounded-3xl shadow-2xl flex items-center gap-3 transition-all ${type === 'error' ? 'bg-red-600' : 'bg-slate-900'}`;
            icon.className = type === 'error' ? 'fa-solid fa-circle-xmark text-white' : 'fa-solid fa-circle-check text-emerald-400';
            
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3000);
        }

        window.logout = () => signOut(auth).then(() => location.reload());

        initProv();
    </script>
</body>
</html>
