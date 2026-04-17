<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Plateforme Officielle</title> 
    
    <!-- Identité Visuelle & PWA -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#00853f">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --gabon-green: #00853f;
            --gabon-yellow: #fcd116;
            --gabon-blue: #3a75c4;
        }
        body { font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px); }
        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .echoppe-card { background: linear-gradient(135deg, #00853f 0%, #005f2d 100%); }
        
        /* Splash Screen */
        #splash-screen {
            position: fixed;
            inset: 0;
            z-index: 9999;
            background: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            transition: opacity 0.5s ease-out, visibility 0.5s;
        }
        .loader-dots span {
            width: 8px;
            height: 8px;
            margin: 0 4px;
            background: var(--gabon-green);
            border-radius: 50%;
            display: inline-block;
            animation: dots 1.4s infinite ease-in-out both;
        }
        .loader-dots span:nth-child(1) { animation-delay: -0.32s; }
        .loader-dots span:nth-child(2) { animation-delay: -0.16s; }
        @keyframes dots { 0%, 80%, 100% { transform: scale(0); } 40% { transform: scale(1.0); } }
    </style>
</head>
<body class="bg-[#f8fafc] text-slate-900 overflow-x-hidden">

    <!-- SPLASH SCREEN (Faux chargement initial) -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 h-24 mb-6 animate-pulse" alt="Logo">
        <h2 class="text-xl font-black uppercase italic tracking-tighter mb-4">Echoppe<span class="text-emerald-600">241</span></h2>
        <div class="loader-dots">
            <span></span><span></span><span></span>
        </div>
        <p class="text-[10px] font-bold text-slate-400 uppercase mt-8 tracking-widest">Chargement du terroir...</p>
    </div>

    <!-- NAVIGATION SUPERIEURE -->
    <nav class="sticky top-0 z-[100] glass border-b border-slate-100 px-4 h-20 flex justify-between items-center">
        <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('home')">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 h-10 object-contain" alt="Logo">
            <span class="font-extrabold text-xl tracking-tighter uppercase italic">Echoppe<span class="text-emerald-600">241</span></span>
        </div>
        
        <div class="flex items-center gap-4">
            <div id="btn-login-trigger" onclick="navigateTo('auth')" class="cursor-pointer bg-slate-100 p-3 rounded-full hover:bg-slate-200 transition-all">
                <i class="fa-solid fa-user text-slate-500"></i>
            </div>
            <div id="user-profile" class="hidden flex items-center gap-3 cursor-pointer" onclick="navigateTo('profile')">
                <div class="text-right hidden md:block">
                    <p id="user-wallet-nav" class="text-[10px] font-black text-emerald-600 uppercase">0 FCFA</p>
                    <p id="user-name-nav" class="text-xs font-bold text-slate-400 italic">Chargement...</p>
                </div>
                <div class="relative">
                    <img id="user-avatar-nav" src="https://ui-avatars.com/api/?name=User&background=00853f&color=fff" class="w-10 h-10 rounded-full border-2 border-white shadow-sm object-cover">
                    <div id="admin-badge" class="hidden absolute -top-1 -right-1 bg-yellow-400 w-4 h-4 rounded-full border-2 border-white flex items-center justify-center">
                        <i class="fa-solid fa-crown text-[6px] text-slate-900"></i>
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <!-- VUES PRINCIPALES -->
    <div id="view-home" class="view active max-w-7xl mx-auto p-4 md:p-8">
        <div class="mb-10">
            <h1 class="text-5xl font-black italic uppercase leading-none mb-4">Le Marché <span class="text-emerald-600">Digital</span></h1>
            <p class="text-slate-400 font-semibold max-w-md">Soutenez nos planteurs. Achetez local, payez en toute sécurité avec EchoppePay.</p>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-4 gap-8">
            <div class="lg:col-span-1">
                <div class="bg-white rounded-[32px] p-6 shadow-sm border border-slate-100 sticky top-28">
                    <h3 class="font-black uppercase text-xs text-slate-400 mb-6 tracking-widest">Provinces du Gabon</h3>
                    <div id="filter-provinces" class="flex flex-col gap-2">
                        <button onclick="setFilter('Tous')" class="province-btn text-left text-[11px] font-bold p-3 rounded-2xl bg-emerald-600 text-white shadow-lg">Tous le Gabon</button>
                    </div>
                </div>
            </div>
            <div class="lg:col-span-3">
                <div id="market-grid" class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6"></div>
            </div>
        </div>
    </div>

    <!-- AUTHENTIFICATION (Connexion / Inscription) -->
    <div id="view-auth" class="view max-w-md mx-auto p-6 mt-10">
        <div class="bg-white rounded-[40px] p-10 shadow-2xl border border-slate-50 relative overflow-hidden">
            <!-- Overlay de chargement pour l'auth -->
            <div id="auth-loader" class="hidden absolute inset-0 bg-white/90 z-50 flex flex-col items-center justify-center">
                <div class="loader-dots mb-4"><span></span><span></span><span></span></div>
                <p class="text-[10px] font-black uppercase text-emerald-600">Sécurisation du profil...</p>
            </div>

            <h2 class="text-3xl font-black mb-8 uppercase italic leading-tight">Rejoindre <br><span class="text-emerald-600">La Communauté</span></h2>
            <div class="space-y-4">
                <input type="text" id="auth-name" placeholder="Nom complet" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold focus:ring-2 ring-emerald-500 outline-none">
                <input type="email" id="auth-email" placeholder="Adresse Email" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold focus:ring-2 ring-emerald-500 outline-none">
                <input type="password" id="auth-pass" placeholder="Mot de passe" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-semibold focus:ring-2 ring-emerald-500 outline-none">
                
                <div class="pt-4">
                    <p class="text-[10px] font-black uppercase text-slate-400 mb-3 tracking-widest">Mon Statut</p>
                    <div class="grid grid-cols-2 gap-3">
                        <button onclick="selectRole('buyer')" id="role-buyer" class="role-btn p-4 rounded-2xl border-2 border-emerald-600 bg-emerald-50 text-emerald-700 font-black text-[10px] uppercase">Acheteur</button>
                        <button onclick="selectRole('producer')" id="role-producer" class="role-btn p-4 rounded-2xl border-2 border-slate-100 text-slate-400 font-black text-[10px] uppercase">Producteur</button>
                    </div>
                </div>
                
                <button onclick="handleAuth()" id="btn-auth-submit" class="w-full bg-[#00853f] text-white font-black py-5 rounded-2xl shadow-xl uppercase tracking-widest mt-4 flex justify-center items-center gap-3">
                    <span>Créer mon compte</span>
                </button>
                <p class="text-center text-[10px] font-bold text-slate-400 mt-4">Déjà inscrit ? Connectez-vous avec vos identifiants.</p>
            </div>
        </div>
    </div>

    <!-- PROFIL & ECHOPPEPAY -->
    <div id="view-profile" class="view max-w-2xl mx-auto p-4 md:p-8 pb-32">
        <div class="echoppe-card rounded-[40px] p-8 text-white mb-8 relative overflow-hidden shadow-2xl">
            <div class="relative z-10">
                <div class="flex justify-between items-start mb-10">
                    <div>
                        <p class="text-[10px] font-black opacity-70 uppercase tracking-widest mb-1">Mon Solde EchoppePay</p>
                        <h2 id="p-wallet-balance" class="text-5xl font-black italic tracking-tighter">0 FCFA</h2>
                    </div>
                    <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-12 h-12 brightness-200">
                </div>
                <div class="flex flex-wrap gap-3">
                    <button onclick="openRechargeModal()" class="bg-white text-emerald-900 font-black px-8 py-4 rounded-2xl text-[10px] uppercase shadow-lg">Recharger</button>
                    <button id="btn-admin-panel" onclick="navigateTo('admin')" class="hidden bg-yellow-400 text-slate-900 font-black px-8 py-4 rounded-2xl text-[10px] uppercase shadow-lg">Console Admin</button>
                    <button onclick="handleLogout()" class="bg-emerald-800/50 text-white font-black px-8 py-4 rounded-2xl text-[10px] uppercase">Déconnexion</button>
                </div>
            </div>
            <i class="fa-solid fa-wallet absolute -right-6 -bottom-6 text-white/10 text-[12rem]"></i>
        </div>

        <!-- Section Producteur -->
        <div id="view-producer" class="hidden bg-emerald-50 rounded-[40px] p-8 border-2 border-dashed border-emerald-200 mb-8">
            <h3 class="font-black uppercase text-xl mb-6 italic text-emerald-900">Publier un produit</h3>
            <div class="space-y-4">
                <div class="h-48 rounded-3xl bg-white flex flex-col items-center justify-center border-2 border-dashed border-slate-200 cursor-pointer overflow-hidden hover:border-emerald-400 transition-all" onclick="document.getElementById('prod-img-input').click()">
                    <div id="upload-preview" class="text-center">
                        <i class="fa-solid fa-camera text-4xl text-slate-200 mb-2"></i>
                        <p class="text-[10px] font-black text-slate-400 uppercase">Ajouter une photo</p>
                    </div>
                    <input type="file" id="prod-img-input" class="hidden" accept="image/*">
                </div>
                <input type="text" id="prod-name" placeholder="Nom du produit (ex: Manioc d'Oyem)" class="w-full p-4 bg-white rounded-xl border-none font-bold">
                <div class="grid grid-cols-2 gap-4">
                    <input type="number" id="prod-price" placeholder="Prix (FCFA)" class="w-full p-4 bg-white rounded-xl border-none font-bold">
                    <select id="prod-province" class="w-full p-4 bg-white rounded-xl border-none font-black text-emerald-600 uppercase text-xs"></select>
                </div>
                <button onclick="handlePublish()" id="btn-publish" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl uppercase text-xs tracking-widest shadow-lg">Mettre en vente</button>
            </div>
        </div>

        <div class="bg-white rounded-[32px] p-8 shadow-sm border border-slate-100">
            <h3 class="font-black uppercase text-xs text-slate-400 tracking-widest mb-6 italic">Historique des transactions</h3>
            <div id="recharge-history" class="space-y-4">
                <p class="text-center py-6 text-slate-300 font-bold italic text-xs">Aucune activité récente.</p>
            </div>
        </div>
    </div>

    <!-- CONSOLE ADMIN (Validation des recharges) -->
    <div id="view-admin" class="view max-w-4xl mx-auto p-4 md:p-8">
        <div class="flex justify-between items-end mb-10">
            <div>
                <h1 class="text-4xl font-black italic uppercase">Console <span class="text-emerald-600">Admin</span></h1>
                <p class="text-slate-400 font-bold text-[10px] uppercase tracking-widest mt-2">Validation des recharges clients</p>
            </div>
            <button onclick="navigateTo('profile')" class="bg-slate-100 px-6 py-3 rounded-xl text-[10px] font-black uppercase">Fermer</button>
        </div>
        <div class="bg-white rounded-[40px] p-8 shadow-xl border border-slate-100">
            <div id="admin-request-list" class="space-y-4"></div>
        </div>
    </div>

    <!-- MODAL RECHARGE ECHOPPEPAY -->
    <div id="modal-recharge" class="hidden fixed inset-0 z-[2000] bg-slate-900/90 flex items-center justify-center p-4 backdrop-blur-sm">
        <div class="bg-white rounded-[40px] p-10 max-w-md w-full shadow-2xl relative">
            <div id="recharge-loader" class="hidden absolute inset-0 bg-white/90 z-10 flex flex-col items-center justify-center rounded-[40px]">
                <div class="loader-dots mb-4"><span></span><span></span><span></span></div>
                <p class="text-[10px] font-black uppercase text-emerald-600">Envoi de la demande...</p>
            </div>
            
            <div class="text-center mb-8">
                <h3 class="font-black uppercase italic text-2xl">Recharger <br><span class="text-emerald-600">EchoppePay</span></h3>
            </div>
            <div class="space-y-5">
                <div class="p-5 bg-emerald-50 rounded-2xl border-2 border-emerald-100 space-y-2">
                    <p class="text-[10px] font-black text-emerald-800 uppercase tracking-widest">Numéros Marchands Officiels :</p>
                    <div class="flex justify-between items-center font-black text-sm"><span>Airtel Money</span><span class="text-emerald-600 font-mono">077 73 60 65</span></div>
                    <div class="flex justify-between items-center font-black text-sm"><span>Moov Money</span><span class="text-blue-600 font-mono">066 45 71 72</span></div>
                </div>
                <p class="text-[10px] font-bold text-slate-400 italic">Faites le transfert puis entrez les détails ci-dessous :</p>
                <input type="number" id="recharge-amount" placeholder="Montant transféré (FCFA)" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-black">
                <input type="text" id="recharge-txn-id" placeholder="ID ou Code de Transaction" class="w-full p-5 bg-slate-50 rounded-2xl border-none font-bold uppercase">
                <button onclick="submitRechargeRequest()" id="btn-submit-recharge" class="w-full bg-emerald-600 text-white font-black py-5 rounded-2xl uppercase shadow-lg">Confirmer le dépôt</button>
                <button onclick="closeRechargeModal()" class="w-full text-slate-400 font-bold text-[10px] uppercase tracking-widest mt-2">Annuler</button>
            </div>
        </div>
    </div>

    <!-- TOAST NOTIFICATION -->
    <div id="toast" class="hidden fixed bottom-10 left-1/2 -translate-x-1/2 z-[3000] bg-slate-900 text-white px-8 py-4 rounded-3xl shadow-2xl flex items-center gap-4 border border-slate-700">
        <div id="toast-icon" class="w-6 h-6 rounded-full bg-emerald-500 flex items-center justify-center text-[10px]"><i class="fa-solid fa-check"></i></div>
        <p id="toast-msg" class="text-[10px] font-black uppercase tracking-widest"></p>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, createUserWithEmailAndPassword, signInWithEmailAndPassword, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, serverTimestamp, updateDoc, increment, query, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

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
        const appId = "echoppe241-prod";

        let userProfile = null;
        let activeFilter = 'Tous';
        let tempProdImg = null;
        let selectedRole = 'buyer';
        
        const ADMIN_EMAIL = "admin@echoppe241.ga";
        const provinces = ["Estuaire", "Haut-Ogooué", "Moyen-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Ogooué-Maritime", "Woleu-Ntem"];

        // Splash Screen Logic
        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                splash.style.opacity = '0';
                setTimeout(() => splash.classList.add('hidden'), 500);
            }, 2500);
        });

        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(view => view.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            window.scrollTo(0, 0);
            if(v === 'admin') loadAdminDashboard();
        };

        window.selectRole = (r) => {
            selectedRole = r;
            document.querySelectorAll('.role-btn').forEach(b => {
                b.classList.remove('border-emerald-600', 'bg-emerald-50', 'text-emerald-700');
                b.classList.add('border-slate-100', 'text-slate-400');
            });
            document.getElementById(`role-${r}`).classList.add('border-emerald-600', 'bg-emerald-50', 'text-emerald-700');
        };

        // Gestion de l'Auth avec faux chargement
        window.handleAuth = async () => {
            const name = document.getElementById('auth-name').value;
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;

            if(!email || pass.length < 6) return showToast("Email/Mot de passe trop court", "error");

            document.getElementById('auth-loader').classList.remove('hidden');

            try {
                // Tentative de connexion d'abord
                let cred;
                try {
                    cred = await signInWithEmailAndPassword(auth, email, pass);
                } catch (err) {
                    // Si échec, on tente l'inscription
                    if(!name) {
                        document.getElementById('auth-loader').classList.add('hidden');
                        return showToast("Nom requis pour l'inscription", "error");
                    }
                    cred = await createUserWithEmailAndPassword(auth, email, pass);
                    const profile = {
                        uid: cred.user.uid,
                        fullName: name,
                        email: email,
                        role: selectedRole,
                        walletBalance: 0,
                        isAdmin: email === ADMIN_EMAIL,
                        createdAt: serverTimestamp()
                    };
                    await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), profile);
                }
                showToast("Succès ! Redirection...");
                setTimeout(() => navigateTo('home'), 1000);
            } catch (e) { 
                showToast("Erreur d'authentification", "error"); 
            } finally {
                document.getElementById('auth-loader').classList.add('hidden');
            }
        };

        onAuthStateChanged(auth, u => {
            if (u && !u.isAnonymous) {
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), s => {
                    if (s.exists()) {
                        userProfile = s.data();
                        updateUI();
                        loadRechargeHistory();
                    }
                });
            } else if (!u) {
                signInAnonymously(auth);
            }
        });

        // Charger le marché
        onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), s => {
            const grid = document.getElementById('market-grid');
            grid.innerHTML = "";
            s.docs.forEach(d => {
                const p = d.data();
                if(activeFilter !== 'Tous' && p.province !== activeFilter) return;
                const card = document.createElement('div');
                card.className = "bg-white rounded-[32px] overflow-hidden shadow-sm border border-slate-100 p-4 hover:shadow-xl transition-all group cursor-pointer";
                card.innerHTML = `
                    <div class="relative overflow-hidden rounded-2xl mb-4 h-48">
                        <img src="${p.image}" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500">
                        <div class="absolute top-2 left-2 bg-white/90 px-3 py-1 rounded-full text-[8px] font-black uppercase text-emerald-700">${p.province}</div>
                    </div>
                    <div class="px-2">
                        <h3 class="font-black text-lg text-slate-800">${p.name}</h3>
                        <div class="flex justify-between items-center mt-4">
                            <span class="font-black text-emerald-700">${p.price.toLocaleString()} F</span>
                            <button onclick="event.stopPropagation(); showToast('Achat direct bientôt disponible')" class="bg-slate-900 text-white px-4 py-2 rounded-xl text-[9px] font-black uppercase">Voir détails</button>
                        </div>
                    </div>
                `;
                grid.appendChild(card);
            });
        });

        window.openRechargeModal = () => document.getElementById('modal-recharge').classList.remove('hidden');
        window.closeRechargeModal = () => document.getElementById('modal-recharge').classList.add('hidden');

        window.submitRechargeRequest = async () => {
            const amount = parseInt(document.getElementById('recharge-amount').value);
            const txnId = document.getElementById('recharge-txn-id').value.trim();
            if(!amount || !txnId) return showToast("Veuillez remplir tous les champs", "error");

            document.getElementById('recharge-loader').classList.remove('hidden');

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                    userId: userProfile.uid,
                    userName: userProfile.fullName,
                    amount: amount,
                    txnId: txnId,
                    status: 'pending',
                    createdAt: serverTimestamp()
                });
                showToast("Preuve envoyée à l'administrateur !");
                closeRechargeModal();
            } catch (e) {
                showToast("Erreur réseau", "error");
            } finally {
                document.getElementById('recharge-loader').classList.add('hidden');
            }
        };

        const loadRechargeHistory = () => {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), s => {
                const list = document.getElementById('recharge-history');
                const mine = s.docs.filter(d => d.data().userId === userProfile.uid);
                if(mine.length > 0) {
                    list.innerHTML = "";
                    mine.forEach(d => {
                        const t = d.data();
                        const color = t.status === 'completed' ? 'text-emerald-600' : 'text-orange-500';
                        const item = document.createElement('div');
                        item.className = "flex justify-between p-4 bg-slate-50 rounded-2xl text-[10px] font-bold border border-slate-100";
                        item.innerHTML = `
                            <div>
                                <p class="uppercase text-slate-400">Transaction ID: ${t.txnId}</p>
                                <p class="text-slate-800">${t.amount.toLocaleString()} FCFA</p>
                            </div>
                            <div class="${color} uppercase font-black">${t.status === 'completed' ? 'Validé' : 'En attente'}</div>
                        `;
                        list.appendChild(item);
                    });
                }
            });
        };

        const loadAdminDashboard = () => {
            if(!userProfile.isAdmin) return navigateTo('home');
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), s => {
                const list = document.getElementById('admin-request-list');
                const pending = s.docs.filter(d => d.data().status === 'pending');
                list.innerHTML = pending.length ? "" : "<div class='text-center py-10 opacity-30'><i class='fa-solid fa-check-double text-4xl mb-2'></i><p>Tout est validé !</p></div>";
                pending.forEach(d => {
                    const t = d.data();
                    const item = document.createElement('div');
                    item.className = "p-6 bg-slate-50 rounded-2xl flex justify-between items-center border border-slate-200";
                    item.innerHTML = `
                        <div>
                            <p class="font-black text-emerald-800 uppercase text-xs">${t.userName}</p>
                            <p class="font-bold text-lg">${t.amount.toLocaleString()} F</p>
                            <p class="text-[9px] font-mono text-slate-400">ID: ${t.txnId}</p>
                        </div>
                        <button onclick="approveRecharge('${d.id}', '${t.userId}', ${t.amount})" class="bg-emerald-600 text-white px-6 py-3 rounded-xl font-black text-[10px] shadow-lg hover:scale-105 transition-all">APPROUVER</button>
                    `;
                    list.appendChild(item);
                });
            });
        };

        window.approveRecharge = async (docId, userId, amount) => {
            try {
                await updateDoc(doc(db, 'artifacts', appId, 'users', userId), { walletBalance: increment(amount) });
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', docId), { status: 'completed' });
                showToast("Compte client crédité avec succès !");
            } catch (e) {
                showToast("Erreur de validation", "error");
            }
        };

        function updateUI() {
            const isLogged = userProfile && !auth.currentUser.isAnonymous;
            document.getElementById('user-profile').classList.toggle('hidden', !isLogged);
            document.getElementById('btn-login-trigger').classList.toggle('hidden', isLogged);
            if(isLogged) {
                document.getElementById('user-name-nav').innerText = userProfile.fullName;
                document.getElementById('user-wallet-nav').innerText = `${userProfile.walletBalance.toLocaleString()} F`;
                document.getElementById('p-wallet-balance').innerText = `${userProfile.walletBalance.toLocaleString()} FCFA`;
                document.getElementById('view-producer').classList.toggle('hidden', userProfile.role !== 'producer');
                document.getElementById('btn-admin-panel').classList.toggle('hidden', !userProfile.isAdmin);
                document.getElementById('admin-badge').classList.toggle('hidden', !userProfile.isAdmin);
            }
        }

        window.setFilter = (p) => {
            activeFilter = p;
            document.querySelectorAll('.province-btn').forEach(b => {
                const isActive = b.innerText === p || (p === 'Tous' && b.innerText === 'Tous le Gabon');
                b.classList.toggle('bg-emerald-600', isActive);
                b.classList.toggle('text-white', isActive);
                b.classList.toggle('shadow-lg', isActive);
            });
            showToast(`Secteur : ${p}`);
        };

        function showToast(m, type = "success") {
            const t = document.getElementById('toast');
            const icon = document.getElementById('toast-icon');
            document.getElementById('toast-msg').innerText = m;
            
            if(type === 'error') {
                icon.className = "w-6 h-6 rounded-full bg-red-500 flex items-center justify-center text-[10px]";
                icon.innerHTML = '<i class="fa-solid fa-xmark"></i>';
            } else {
                icon.className = "w-6 h-6 rounded-full bg-emerald-500 flex items-center justify-center text-[10px]";
                icon.innerHTML = '<i class="fa-solid fa-check"></i>';
            }

            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        // Image Preview & Upload
        document.getElementById('prod-img-input').addEventListener('change', e => {
            if(!e.target.files[0]) return;
            const reader = new FileReader();
            reader.onload = re => {
                tempProdImg = re.target.result;
                document.getElementById('upload-preview').innerHTML = `<img src="${tempProdImg}" class="w-full h-full object-cover">`;
            };
            reader.readAsDataURL(e.target.files[0]);
        });

        window.handlePublish = async () => {
            const name = document.getElementById('prod-name').value;
            const price = parseInt(document.getElementById('prod-price').value);
            const prov = document.getElementById('prod-province').value;
            
            if(!name || !price || !tempProdImg) return showToast("Produit incomplet", "error");
            
            const btn = document.getElementById('btn-publish');
            btn.innerText = "PUBLICATION...";
            btn.disabled = true;

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name, price, province: prov, image: tempProdImg, ownerId: userProfile.uid, createdAt: serverTimestamp()
                });
                showToast("Produit en ligne !");
                navigateTo('home');
            } catch (e) {
                showToast("Erreur de publication", "error");
            } finally {
                btn.innerText = "METTRE EN VENTE";
                btn.disabled = false;
            }
        };

        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // Initialisation des listes
        const pSelect = document.getElementById('prod-province');
        provinces.forEach(p => {
            const opt = document.createElement('option'); opt.value = p; opt.innerText = p; pSelect.appendChild(opt);
            const btn = document.createElement('button');
            btn.className = "province-btn text-left text-[11px] font-bold p-3 rounded-2xl text-slate-500 hover:bg-slate-50 transition-all";
            btn.innerText = p; btn.onclick = () => setFilter(p);
            document.getElementById('filter-provinces').appendChild(btn);
        });
    </script>
</body>
</html>
