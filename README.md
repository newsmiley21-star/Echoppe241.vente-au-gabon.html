<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 | Supply Chain & ERP</title>
    
    <!-- PWA Meta Tags & Icons -->
    <meta name="theme-color" content="#0f172a">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">
    
    <!-- Icône officielle Echoppe241 (Lien mis à jour) -->
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Urbanist:wght@300;400;700;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --brand-dark: #0f172a;
            --brand-gold: #fcd116;
            --brand-green: #00853f;
            --brand-blue: #3a75c4;
        }
        body { font-family: 'Urbanist', sans-serif; background-color: #f8fafc; color: var(--brand-dark); }
        
        /* Protection affichage logo */
        .brand-logo {
            max-height: 50px;
            width: auto;
            object-fit: contain;
        }

        .role-card { transition: all 0.3s ease; cursor: pointer; border: 2px solid transparent; }
        .role-card.active { border-color: var(--brand-gold); background: #fffbeb; transform: translateY(-5px); }

        .logistics-graph {
            background: linear-gradient(90deg, #f1f5f9 2px, transparent 2px) 0 0 / 40px 40px,
                        linear-gradient(#f1f5f9 2px, transparent 2px) 0 0 / 40px 40px;
            background-color: white;
            position: relative;
            height: 120px;
            border-radius: 20px;
        }

        .line-animate {
            position: absolute;
            height: 2px;
            background: repeating-linear-gradient(90deg, var(--brand-blue), var(--brand-blue) 10px, transparent 10px, transparent 20px);
            animation: flow 2s linear infinite;
        }

        @keyframes flow { from { background-position: 0 0; } to { background-position: 40px 0; } }
        .hidden { display: none !important; }
        
        .chat-bubble { max-width: 85%; padding: 12px 16px; border-radius: 20px; margin-bottom: 8px; font-size: 14px; }
        .chat-mine { background: var(--brand-blue); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }
        .chat-theirs { background: #f1f5f9; color: var(--brand-dark); align-self: flex-start; border-bottom-left-radius: 4px; }
    </style>
</head>
<body class="antialiased">

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-10 left-1/2 -translate-x-1/2 z-[1000] hidden">
        <div class="bg-slate-900 text-white px-6 py-3 rounded-full shadow-2xl flex items-center gap-3 border border-amber-400">
            <i class="fa-solid fa-circle-check text-amber-400"></i>
            <span id="toast-msg" class="text-xs font-bold uppercase tracking-widest">Opération réussie</span>
        </div>
    </div>

    <!-- Navigation -->
    <header class="bg-white/90 backdrop-blur-md border-b sticky top-0 z-[100]">
        <!-- Drapeaux Gabonais -->
        <div class="h-1 flex">
            <div class="flex-1 bg-[#00853f]"></div>
            <div class="flex-1 bg-[#fcd116]"></div>
            <div class="flex-1 bg-[#3a75c4]"></div>
        </div>
        <div class="max-w-7xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="location.reload()">
                <!-- LOGO PRINCIPAL -->
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Echoppe241 Logo" class="brand-logo">
                <div class="flex flex-col">
                    <span class="font-black text-xl tracking-tighter uppercase leading-none">ECHOPPE<span class="text-amber-500">241</span></span>
                    <span class="text-[8px] font-bold uppercase text-slate-400 tracking-[0.2em]">Supply Chain Gabon</span>
                </div>
            </div>
            
            <div class="flex items-center gap-3">
                <div id="user-profile-brief" class="hidden text-right hidden md:block">
                    <p id="nav-user-name" class="text-[10px] font-black uppercase text-slate-900 leading-none"></p>
                    <p id="nav-user-role" class="text-[8px] font-bold text-amber-600 uppercase"></p>
                </div>
                <button id="auth-btn" onclick="openAuthModal()" class="bg-slate-900 text-white px-5 py-2.5 rounded-xl font-black text-[10px] uppercase tracking-widest hover:bg-slate-800 transition-colors">Connexion</button>
                <button id="logout-btn" onclick="handleLogout()" class="hidden bg-red-50 text-red-500 p-2.5 rounded-xl hover:bg-red-100 transition-colors"><i class="fa-solid fa-power-off"></i></button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-4 min-h-screen">
        
        <!-- DASHBOARD ADMIN (ERP) -->
        <section id="view-admin" class="hidden space-y-6">
            <div class="flex items-center justify-between">
                <h2 class="text-2xl font-black uppercase tracking-tighter italic">Console Administration</h2>
                <div class="px-3 py-1 bg-amber-100 text-amber-700 rounded-full text-[10px] font-black uppercase tracking-widest">Mode Super-Admin</div>
            </div>
            <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
                <div class="bg-white p-5 rounded-3xl border shadow-sm">
                    <h5 class="text-[9px] font-black uppercase text-slate-400">Total Utilisateurs</h5>
                    <p class="text-2xl font-black" id="stat-users">0</p>
                </div>
                <div class="bg-white p-5 rounded-3xl border border-amber-200 bg-amber-50 shadow-sm">
                    <h5 class="text-[9px] font-black uppercase text-amber-600">Producteurs en attente</h5>
                    <p class="text-2xl font-black text-amber-700" id="stat-pending">0</p>
                </div>
                <div class="bg-white p-5 rounded-3xl border shadow-sm">
                    <h5 class="text-[9px] font-black uppercase text-slate-400">Flux de Produits</h5>
                    <p class="text-2xl font-black" id="stat-products">0</p>
                </div>
                <div class="bg-white p-5 rounded-3xl border shadow-sm">
                    <h5 class="text-[9px] font-black uppercase text-slate-400">Alertes Logistiques</h5>
                    <p class="text-2xl font-black text-blue-600">0</p>
                </div>
            </div>

            <div class="bg-white rounded-[2rem] p-6 border shadow-sm">
                <h3 class="font-black text-sm mb-4 uppercase tracking-widest flex items-center gap-2">
                    <i class="fa-solid fa-user-check text-emerald-500"></i> Validation des Partenaires
                </h3>
                <div id="admin-pending-list" class="space-y-3">
                    <p class="text-xs text-slate-400 italic text-center py-10">Chargement des demandes...</p>
                </div>
            </div>
        </section>

        <!-- DASHBOARD PRODUCTEUR (WMS) -->
        <section id="view-producer" class="hidden space-y-6">
            <div id="producer-status-banner" class="hidden bg-amber-50 border border-amber-200 p-5 rounded-3xl flex items-center gap-4">
                <div class="w-10 h-10 bg-amber-500 text-white rounded-xl flex items-center justify-center animate-pulse"><i class="fa-solid fa-clock-rotate-left"></i></div>
                <div class="flex-grow">
                    <p class="text-xs font-black text-amber-900 uppercase">Compte en cours de vérification</p>
                    <p class="text-[9px] text-amber-700 font-bold">Veuillez patienter pendant qu'un admin Echoppe241 valide vos informations.</p>
                </div>
            </div>

            <div class="bg-slate-900 text-white p-8 rounded-[2rem] flex flex-col md:flex-row justify-between items-center gap-4">
                <div>
                    <h2 class="text-3xl font-black tracking-tighter uppercase leading-none italic">Console <span class="text-emerald-500">WMS</span></h2>
                    <p class="text-slate-400 text-[10px] font-bold uppercase tracking-widest mt-2">Gestion d'entrepôt et traçabilité</p>
                </div>
                <button onclick="openPublishModal()" class="bg-white text-slate-900 px-8 py-4 rounded-2xl font-black text-[10px] uppercase tracking-widest shadow-xl hover:scale-105 transition-transform">Ajouter du Stock</button>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <div class="lg:col-span-1 space-y-6">
                    <div class="bg-white p-6 rounded-[2rem] border shadow-sm">
                        <h3 class="font-black text-[10px] mb-4 uppercase text-slate-400 tracking-widest">Flux vers Hub Libreville</h3>
                        <div class="logistics-graph border p-4 flex items-center justify-between px-6">
                            <div class="flex flex-col items-center gap-1">
                                <i class="fa-solid fa-seedling text-emerald-500"></i>
                                <span class="text-[8px] font-black uppercase">Origine</span>
                            </div>
                            <div class="flex-grow mx-2 relative h-1 bg-slate-100 rounded-full">
                                <div class="line-animate w-full h-full rounded-full"></div>
                            </div>
                            <div class="flex flex-col items-center gap-1">
                                <i class="fa-solid fa-box text-blue-500"></i>
                                <span class="text-[8px] font-black uppercase">Centre</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="lg:col-span-2 bg-white p-6 rounded-[2rem] border shadow-sm">
                    <h3 class="font-black text-[10px] mb-4 uppercase text-slate-400 tracking-widest">Inventaire Actif</h3>
                    <div id="producer-inventory" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
                </div>
            </div>
        </section>

        <!-- VUE ACHETEUR (MARKETPLACE) -->
        <section id="view-buyer" class="space-y-8">
            <!-- Hero -->
            <div class="relative bg-slate-900 rounded-[2.5rem] p-10 text-white overflow-hidden min-h-[350px] flex items-center">
                <div class="absolute inset-0 bg-cover bg-center opacity-40 grayscale-[50%]" style="background-image: url('https://images.unsplash.com/photo-1488459716781-31db52582fe9?auto=format&fit=crop&w=800&q=80');"></div>
                <div class="relative z-10 max-w-xl">
                    <div class="flex items-center gap-2 bg-emerald-500/20 text-emerald-400 border border-emerald-500/30 w-fit px-3 py-1 rounded-full text-[9px] font-black uppercase mb-4">
                        <i class="fa-solid fa-check-circle"></i>
                        Certifié Echoppe241
                    </div>
                    <h2 class="text-5xl font-black leading-none tracking-tighter mb-4 italic">Directement des <span class="text-emerald-500">Terroirs</span> à votre Table.</h2>
                    <p class="text-slate-300 text-sm font-medium mb-6 leading-relaxed">Une chaîne logistique transparente pour valoriser le travail de nos producteurs dans les 9 provinces.</p>
                    <button onclick="document.getElementById('market-grid').scrollIntoView({behavior:'smooth'})" class="bg-white text-slate-900 px-8 py-4 rounded-xl font-black text-[10px] uppercase tracking-widest shadow-2xl hover:bg-emerald-500 hover:text-white transition-all">Parcourir les arrivages</button>
                </div>
            </div>

            <div class="flex items-center justify-between px-2">
                <h3 class="font-black uppercase tracking-tighter text-xl italic">Marché de Frais</h3>
                <div class="flex gap-2">
                    <button class="w-10 h-10 bg-white border rounded-full flex items-center justify-center text-xs"><i class="fa-solid fa-sliders"></i></button>
                </div>
            </div>

            <div id="market-grid" class="grid grid-cols-2 md:grid-cols-4 gap-6">
                <!-- Produits chargés par Firebase -->
            </div>
        </section>
    </main>

    <!-- MODAL AUTH -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/80 backdrop-blur-md z-[500] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 relative shadow-2xl">
            <button onclick="closeAuthModal()" class="absolute top-6 right-6 text-slate-300 text-2xl hover:text-slate-900 transition-colors">&times;</button>
            <div class="text-center mb-8">
                <!-- LOGO DANS MODALE -->
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Logo" class="h-16 mx-auto mb-4 object-contain">
                <h2 id="auth-title" class="text-2xl font-black uppercase tracking-tighter">Bienvenue</h2>
            </div>
            
            <form id="login-form" class="space-y-4">
                <input type="email" id="l-email" placeholder="Email Professionnel" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border focus:border-emerald-500 transition-all" required>
                <input type="password" id="l-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border focus:border-emerald-500 transition-all" required>
                <button type="submit" class="w-full bg-slate-900 text-white py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest hover:bg-emerald-600 transition-all shadow-lg">Se connecter</button>
                <p class="text-center text-[10px] font-bold text-slate-400 uppercase pt-4">Nouveau partenaire ? <button type="button" onclick="toggleAuthMode('reg')" class="text-emerald-600 underline font-black">Rejoindre le réseau</button></p>
            </form>

            <form id="reg-form" class="space-y-3 hidden">
                <input type="text" id="r-name" placeholder="Nom de l'exploitation" class="w-full bg-slate-50 p-3.5 rounded-2xl outline-none font-bold text-sm border focus:border-emerald-500 transition-all" required>
                <input type="email" id="r-email" placeholder="Email" class="w-full bg-slate-50 p-3.5 rounded-2xl outline-none font-bold text-sm border focus:border-emerald-500 transition-all" required>
                <input type="tel" id="r-phone" placeholder="WhatsApp (+241)" class="w-full bg-slate-50 p-3.5 rounded-2xl outline-none font-bold text-sm border focus:border-emerald-500 transition-all" required>
                <input type="password" id="r-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-3.5 rounded-2xl outline-none font-bold text-sm border focus:border-emerald-500 transition-all" required>
                <div class="grid grid-cols-2 gap-2 py-2">
                    <div onclick="setRegRole('buyer')" id="role-b" class="role-card active p-3 rounded-2xl bg-slate-50 text-center border">
                        <i class="fa-solid fa-basket-shopping block mb-1 text-slate-400"></i>
                        <span class="text-[8px] font-black uppercase">Client</span>
                    </div>
                    <div onclick="setRegRole('producer')" id="role-p" class="role-card p-3 rounded-2xl bg-slate-50 text-center border">
                        <i class="fa-solid fa-tractor block mb-1 text-slate-400"></i>
                        <span class="text-[8px] font-black uppercase">Producteur</span>
                    </div>
                </div>
                <button type="submit" class="w-full bg-emerald-600 text-white py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest hover:bg-emerald-700 transition-all shadow-xl shadow-emerald-100">Inscription</button>
                <p class="text-center text-[10px] font-bold text-slate-400 uppercase pt-4">Déjà inscrit ? <button type="button" onclick="toggleAuthMode('login')" class="text-slate-900 underline font-black">S'identifier</button></p>
            </form>
        </div>
    </div>

    <!-- MODAL WMS STOCK -->
    <div id="publish-modal" class="fixed inset-0 bg-slate-900/80 backdrop-blur-sm z-[200] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-lg rounded-[2.5rem] p-8 relative shadow-2xl">
            <button onclick="document.getElementById('publish-modal').classList.add('hidden')" class="absolute top-6 right-6 text-slate-300 text-2xl hover:text-slate-900">&times;</button>
            <h3 class="text-2xl font-black mb-6 uppercase tracking-tighter italic"><i class="fa-solid fa-truck-ramp-box mr-2 text-emerald-500"></i>Nouvelle Entrée Stock</h3>
            <form id="product-form" class="space-y-4">
                <input type="text" id="p-name" placeholder="Désignation" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border" required>
                <div class="grid grid-cols-2 gap-4">
                    <input type="number" id="p-price" placeholder="Prix (FCFA)" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border" required>
                    <input type="number" id="p-qty" placeholder="Quantité" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border" required>
                </div>
                <select id="p-prov" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border cursor-pointer">
                    <option value="" disabled selected>Province d'origine</option>
                    <option>Estuaire</option><option>Haut-Ogooué</option><option>Moyen-Ogooué</option><option>Ngounié</option>
                    <option>Nyanga</option><option>Ogooué-Ivindo</option><option>Ogooué-Lolo</option><option>Ogooué-Maritime</option><option>Woleu-Ntem</option>
                </select>
                <button type="submit" class="w-full bg-slate-900 text-white py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest shadow-xl">Valider l'entrée</button>
            </form>
        </div>
    </div>

    <!-- CHAT BOX -->
    <div id="chat-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[600] hidden flex items-center justify-end p-4">
        <div class="bg-white w-full max-w-md h-[85vh] rounded-[2.5rem] shadow-2xl flex flex-col overflow-hidden">
            <div class="p-6 bg-slate-900 text-white flex justify-between items-center">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-emerald-500/20 rounded-full flex items-center justify-center border border-emerald-500/30">
                        <i class="fa-solid fa-user-tie text-emerald-400 text-xs"></i>
                    </div>
                    <div>
                        <h4 id="chat-target" class="font-black text-sm uppercase tracking-tighter">Messagerie</h4>
                        <p class="text-[8px] font-bold text-emerald-400 uppercase tracking-widest">Canal sécurisé Echoppe241</p>
                    </div>
                </div>
                <button onclick="closeChat()" class="text-white/40 hover:text-white text-2xl">&times;</button>
            </div>
            <div id="chat-messages" class="flex-grow p-5 overflow-y-auto flex flex-col gap-2 bg-slate-50"></div>
            <form id="chat-form" class="p-4 bg-white border-t flex gap-2">
                <input type="text" id="chat-input" placeholder="Ecrivez ici..." class="flex-grow bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border">
                <button class="bg-emerald-600 text-white w-12 h-12 rounded-2xl flex items-center justify-center shadow-lg"><i class="fa-solid fa-paper-plane"></i></button>
            </form>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, onAuthStateChanged, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, orderBy, serverTimestamp, updateDoc, where } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = JSON.parse('{"apiKey":"AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg","authDomain":"communautedugabon.firebaseapp.com","projectId":"communautedugabon","storageBucket":"communautedugabon.firebasestorage.app","messagingSenderId":"647862371022","appId":"1:647862371022:web:b209bfc8eb81accb1fc69f"}');
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'echoppe241-prod-v1';

        let currentUser = null;
        let selectedRegRole = 'buyer';
        let currentChatId = null;

        // AUTH TRACKING
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                const userRef = doc(db, 'artifacts', appId, 'users', user.uid);
                const snap = await getDoc(userRef);
                if (snap.exists()) {
                    currentUser = { uid: user.uid, ...snap.data() };
                    updateUI();
                    syncData();
                } else if (user.isAnonymous) {
                    resetUI();
                    syncData();
                }
            } else {
                signInAnonymously(auth);
            }
        });

        // ACTIONS AUTH
        document.getElementById('reg-form').onsubmit = async (e) => {
            e.preventDefault();
            const email = document.getElementById('r-email').value;
            const pass = document.getElementById('r-pass').value;
            const name = document.getElementById('r-name').value;
            const phone = document.getElementById('r-phone').value;

            try {
                const cred = await createUserWithEmailAndPassword(auth, email, pass);
                const role = selectedRegRole;
                const status = role === 'producer' ? 'pending' : 'active';
                
                await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), {
                    fullName: name, email, phone, role, status, createdAt: serverTimestamp()
                });
                closeAuthModal();
                showToast(role === 'producer' ? "Compte en attente de validation." : "Bienvenue sur Echoppe241 !");
            } catch (err) { showToast("Erreur lors de l'inscription."); }
        };

        document.getElementById('login-form').onsubmit = async (e) => {
            e.preventDefault();
            try {
                await signInWithEmailAndPassword(auth, document.getElementById('l-email').value, document.getElementById('l-pass').value);
                closeAuthModal();
            } catch (err) { showToast("Identifiants incorrects."); }
        };

        // UI MANAGEMENT
        function updateUI() {
            document.getElementById('auth-btn').classList.add('hidden');
            document.getElementById('logout-btn').classList.remove('hidden');
            document.getElementById('user-profile-brief').classList.remove('hidden');
            document.getElementById('nav-user-name').innerText = currentUser.fullName;
            document.getElementById('nav-user-role').innerText = currentUser.role;

            ['view-admin', 'view-producer', 'view-buyer'].forEach(id => document.getElementById(id).classList.add('hidden'));

            if (currentUser.role === 'admin') document.getElementById('view-admin').classList.remove('hidden');
            else if (currentUser.role === 'producer') {
                document.getElementById('view-producer').classList.remove('hidden');
                document.getElementById('producer-status-banner').classList.toggle('hidden', currentUser.status === 'active');
            } else {
                document.getElementById('view-buyer').classList.remove('hidden');
            }
        }

        function resetUI() {
            document.getElementById('auth-btn').classList.remove('hidden');
            document.getElementById('logout-btn').classList.add('hidden');
            document.getElementById('user-profile-brief').classList.add('hidden');
            document.getElementById('view-buyer').classList.remove('hidden');
        }

        // FIREBASE SYNC
        function syncData() {
            // Stats Admin
            onSnapshot(collection(db, 'artifacts', appId, 'users'), (snap) => {
                const all = snap.docs.map(d => ({id: d.id, ...d.data()}));
                document.getElementById('stat-users').innerText = all.length;
                const pending = all.filter(u => u.status === 'pending');
                document.getElementById('stat-pending').innerText = pending.length;
                if (currentUser?.role === 'admin') renderAdminPending(pending);
            });

            // Produits
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const products = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                document.getElementById('stat-products').innerText = products.length;
                renderMarket(products);
                if (currentUser?.role === 'producer') renderInventory(products.filter(p => p.ownerId === currentUser.uid));
            });
        }

        function renderMarket(products) {
            const container = document.getElementById('market-grid');
            container.innerHTML = products.length ? '' : '<p class="col-span-full py-20 text-center text-slate-400 italic">Aucun arrivage pour le moment.</p>';
            products.forEach(p => {
                const card = document.createElement('div');
                card.className = "bg-white p-4 rounded-[2.5rem] border shadow-sm group cursor-pointer hover:shadow-xl transition-all";
                card.onclick = () => openChatWith(p.ownerId, p.ownerName);
                card.innerHTML = `
                    <div class="h-32 bg-slate-50 rounded-[1.8rem] flex items-center justify-center border relative mb-3">
                         <div class="absolute top-2 left-2 bg-white px-2 py-1 rounded-lg text-[7px] font-black uppercase text-emerald-600 border">${p.province}</div>
                         <i class="fa-solid fa-box text-slate-200 text-3xl"></i>
                    </div>
                    <h4 class="font-black text-xs uppercase truncate">${p.name}</h4>
                    <p class="text-lg font-black mt-1">${p.price} <span class="text-[8px] text-slate-400 uppercase tracking-widest">FCFA</span></p>
                    <div class="flex justify-between items-center mt-3 pt-3 border-t border-dashed">
                        <span class="text-[9px] font-black text-slate-800 uppercase">${p.ownerName}</span>
                        <div class="w-8 h-8 bg-slate-900 text-white rounded-xl flex items-center justify-center text-xs"><i class="fa-solid fa-comment"></i></div>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        // UTILS
        function showToast(msg) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        window.openAuthModal = () => document.getElementById('auth-modal').classList.remove('hidden');
        window.closeAuthModal = () => document.getElementById('auth-modal').classList.add('hidden');
        window.toggleAuthMode = (m) => {
            document.getElementById('login-form').classList.toggle('hidden', m === 'reg');
            document.getElementById('reg-form').classList.toggle('hidden', m === 'login');
            document.getElementById('auth-title').innerText = m === 'reg' ? "Devenir Partenaire" : "Connexion";
        };
        window.setRegRole = (r) => {
            selectedRegRole = r;
            document.getElementById('role-b').classList.toggle('active', r === 'buyer');
            document.getElementById('role-p').classList.toggle('active', r === 'producer');
        };
        window.handleLogout = () => signOut(auth).then(() => location.reload());
        window.openPublishModal = () => document.getElementById('publish-modal').classList.remove('hidden');
        window.closeChat = () => document.getElementById('chat-modal').classList.add('hidden');

    </script>
</body>
</html>
