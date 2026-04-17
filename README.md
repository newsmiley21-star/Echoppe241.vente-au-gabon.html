<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 | Supply Chain & ERP</title>
    
    <!-- PWA Meta Tags -->
    <meta name="theme-color" content="#0f172a">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">
    <link rel="icon" type="image/png" href="https://i.ibb.co/2Q73j3X/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/2Q73j3X/echoppe241-logo.png">

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
        
        .status-badge { font-size: 9px; font-weight: 900; padding: 4px 10px; border-radius: 12px; text-transform: uppercase; }
        .status-pending { background: #fef3c7; color: #92400e; }
        .status-active { background: #d1fae5; color: #065f46; }

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
        <div class="h-1 flex">
            <div class="flex-1 bg-[#00853f]"></div>
            <div class="flex-1 bg-[#fcd116]"></div>
            <div class="flex-1 bg-[#3a75c4]"></div>
        </div>
        <div class="max-w-7xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center gap-2 cursor-pointer" onclick="location.reload()">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Echoppe241" class="h-8">
                <span class="font-black text-lg tracking-tighter uppercase">ECHOPPE<span class="text-amber-500">241</span></span>
            </div>
            
            <div class="flex items-center gap-3">
                <div id="user-profile-brief" class="hidden text-right hidden md:block">
                    <p id="nav-user-name" class="text-[10px] font-black uppercase text-slate-900 leading-none"></p>
                    <p id="nav-user-role" class="text-[8px] font-bold text-amber-600 uppercase"></p>
                </div>
                <button id="auth-btn" onclick="openAuthModal()" class="bg-slate-900 text-white px-5 py-2.5 rounded-xl font-black text-[10px] uppercase tracking-widest">Connexion</button>
                <button id="logout-btn" onclick="handleLogout()" class="hidden bg-red-50 text-red-500 p-2.5 rounded-xl"><i class="fa-solid fa-power-off"></i></button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-4 min-h-screen">
        
        <!-- DASHBOARD ADMIN (ERP) -->
        <section id="view-admin" class="hidden space-y-6">
            <h2 class="text-2xl font-black uppercase tracking-tighter italic">Administration Centrale</h2>
            <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
                <div class="bg-white p-5 rounded-3xl border">
                    <h5 class="text-[9px] font-black uppercase text-slate-400">Utilisateurs</h5>
                    <p class="text-2xl font-black" id="stat-users">0</p>
                </div>
                <div class="bg-white p-5 rounded-3xl border border-amber-200 bg-amber-50">
                    <h5 class="text-[9px] font-black uppercase text-amber-600">Producteurs en attente</h5>
                    <p class="text-2xl font-black text-amber-700" id="stat-pending">0</p>
                </div>
                <div class="bg-white p-5 rounded-3xl border">
                    <h5 class="text-[9px] font-black uppercase text-slate-400">Produits Actifs</h5>
                    <p class="text-2xl font-black" id="stat-products">0</p>
                </div>
                <div class="bg-white p-5 rounded-3xl border">
                    <h5 class="text-[9px] font-black uppercase text-slate-400">Volume Transit</h5>
                    <p class="text-2xl font-black text-blue-600" id="stat-transit">0</p>
                </div>
            </div>

            <div class="bg-white rounded-[2rem] p-6 border">
                <h3 class="font-black text-sm mb-4 uppercase tracking-widest flex items-center gap-2">
                    <i class="fa-solid fa-user-clock text-amber-500"></i> Validation Producteurs
                </h3>
                <div id="admin-pending-list" class="space-y-3">
                    <p class="text-xs text-slate-400 italic">Aucune demande en attente.</p>
                </div>
            </div>
        </section>

        <!-- DASHBOARD PRODUCTEUR (WMS) -->
        <section id="view-producer" class="hidden space-y-6">
            <div id="producer-status-banner" class="hidden bg-amber-50 border border-amber-200 p-5 rounded-3xl flex items-center gap-4">
                <div class="w-10 h-10 bg-amber-500 text-white rounded-xl flex items-center justify-center animate-pulse"><i class="fa-solid fa-shield-halved"></i></div>
                <p class="text-xs font-bold text-amber-900 uppercase">Votre compte est en cours de vérification par Echoppe241.</p>
            </div>

            <div class="bg-slate-900 text-white p-8 rounded-[2rem] flex justify-between items-center">
                <div>
                    <h2 class="text-3xl font-black tracking-tighter uppercase leading-none">Console Stock</h2>
                    <p class="text-amber-400 text-[10px] font-bold uppercase tracking-widest mt-2">Gestion WMS & Expéditions</p>
                </div>
                <button onclick="openPublishModal()" class="bg-white text-slate-900 px-6 py-4 rounded-2xl font-black text-[10px] uppercase tracking-widest shadow-xl">Ajouter Produit</button>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <div class="lg:col-span-1 space-y-6">
                    <div class="bg-white p-6 rounded-[2rem] border shadow-sm">
                        <h3 class="font-black text-[10px] mb-4 uppercase text-slate-400 tracking-widest">État du Réseau</h3>
                        <div class="logistics-graph border p-4 flex items-center justify-between px-6">
                            <div class="flex flex-col items-center gap-1">
                                <i class="fa-solid fa-warehouse text-emerald-500"></i>
                                <span class="text-[8px] font-black uppercase">Source</span>
                            </div>
                            <div class="flex-grow mx-2 relative h-1 bg-slate-100 rounded-full">
                                <div class="line-animate w-full h-full rounded-full"></div>
                            </div>
                            <div class="flex flex-col items-center gap-1">
                                <i class="fa-solid fa-truck-fast text-blue-500"></i>
                                <span class="text-[8px] font-black uppercase">Hub LBV</span>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-[2rem] border">
                        <h3 class="font-black text-[10px] mb-4 uppercase text-slate-400 tracking-widest">Discussions Clients</h3>
                        <div id="producer-chats" class="space-y-3"></div>
                    </div>
                </div>
                <div class="lg:col-span-2 bg-white p-6 rounded-[2rem] border">
                    <h3 class="font-black text-[10px] mb-4 uppercase text-slate-400 tracking-widest">Inventaire en temps réel</h3>
                    <div id="producer-inventory" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
                </div>
            </div>
        </section>

        <!-- VUE ACHETEUR (MARKETPLACE) -->
        <section id="view-buyer" class="space-y-8">
            <div class="relative bg-slate-900 rounded-[2.5rem] p-10 text-white overflow-hidden min-h-[350px] flex items-center">
                <div class="absolute inset-0 bg-cover bg-center opacity-30" style="background-image: url('https://images.unsplash.com/photo-1542601906990-b4d3fb773b09?auto=format&fit=crop&w=800&q=80');"></div>
                <div class="relative z-10 max-w-xl">
                    <span class="bg-emerald-500 text-[9px] font-black px-3 py-1 rounded-full uppercase mb-4 inline-block">Direct Producteur</span>
                    <h2 class="text-5xl font-black leading-none tracking-tighter mb-4 italic">L'Echoppe <span class="text-amber-400">Gabonaise</span> sur Mobile.</h2>
                    <p class="text-slate-300 text-sm font-medium mb-6">Soutenez l'économie locale. Commandez les produits de nos 9 provinces en un clic.</p>
                    <button onclick="document.getElementById('market-grid').scrollIntoView({behavior:'smooth'})" class="bg-white text-slate-900 px-8 py-4 rounded-xl font-black text-[10px] uppercase tracking-widest shadow-2xl">Explorer le marché</button>
                </div>
            </div>

            <div id="market-grid" class="grid grid-cols-2 md:grid-cols-4 gap-6">
                <!-- Produits dynamiques -->
            </div>
        </section>
    </main>

    <!-- MODAL AUTHENTIFICATION -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/70 backdrop-blur-sm z-[500] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 relative">
            <button onclick="closeAuthModal()" class="absolute top-6 right-6 text-slate-300 text-xl">&times;</button>
            <h2 id="auth-title" class="text-2xl font-black uppercase tracking-tighter mb-6">Connexion</h2>
            
            <form id="login-form" class="space-y-4">
                <input type="email" id="l-email" placeholder="Email" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border focus:border-blue-500" required>
                <input type="password" id="l-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border focus:border-blue-500" required>
                <button type="submit" class="w-full bg-slate-900 text-white py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest">Entrer</button>
                <p class="text-center text-[10px] font-bold text-slate-400 uppercase">Nouveau ? <button type="button" onclick="toggleAuthMode('reg')" class="text-amber-600 underline">Créer un compte</button></p>
            </form>

            <form id="reg-form" class="space-y-3 hidden">
                <input type="text" id="r-name" placeholder="Nom Complet / Entreprise" class="w-full bg-slate-50 p-3.5 rounded-2xl outline-none font-bold text-sm border focus:border-amber-500" required>
                <input type="email" id="r-email" placeholder="Email professionnel" class="w-full bg-slate-50 p-3.5 rounded-2xl outline-none font-bold text-sm border focus:border-amber-500" required>
                <input type="tel" id="r-phone" placeholder="WhatsApp (+241)" class="w-full bg-slate-50 p-3.5 rounded-2xl outline-none font-bold text-sm border focus:border-amber-500" required>
                <input type="password" id="r-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-3.5 rounded-2xl outline-none font-bold text-sm border focus:border-amber-500" required>
                <div class="grid grid-cols-2 gap-2 pt-2">
                    <div onclick="setRegRole('buyer')" id="role-b" class="role-card active p-3 rounded-xl bg-slate-50 text-center border">
                        <i class="fa-solid fa-shopping-basket block mb-1"></i>
                        <span class="text-[8px] font-black uppercase">Acheteur</span>
                    </div>
                    <div onclick="setRegRole('producer')" id="role-p" class="role-card p-3 rounded-xl bg-slate-50 text-center border">
                        <i class="fa-solid fa-industry block mb-1"></i>
                        <span class="text-[8px] font-black uppercase">Producteur</span>
                    </div>
                </div>
                <button type="submit" class="w-full bg-amber-500 text-white py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest mt-2 shadow-lg shadow-amber-100">Créer mon compte</button>
                <p class="text-center text-[10px] font-bold text-slate-400 uppercase">Déjà membre ? <button type="button" onclick="toggleAuthMode('login')" class="text-blue-600 underline">Se connecter</button></p>
            </form>
        </div>
    </div>

    <!-- MODAL PUBLICATION (WMS) -->
    <div id="publish-modal" class="fixed inset-0 bg-slate-900/80 backdrop-blur-sm z-[200] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-lg rounded-[2.5rem] p-8 relative">
            <button onclick="document.getElementById('publish-modal').classList.add('hidden')" class="absolute top-6 right-6 text-slate-300 text-xl">&times;</button>
            <h3 class="text-2xl font-black mb-6 uppercase tracking-tighter italic">Nouveau Stock</h3>
            <form id="product-form" class="space-y-4">
                <input type="text" id="p-name" placeholder="Nom du produit (ex: Piment de Bikélé)" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border" required>
                <div class="grid grid-cols-2 gap-4">
                    <input type="number" id="p-price" placeholder="Prix (FCFA)" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border" required>
                    <input type="number" id="p-qty" placeholder="Quantité dispo" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border" required>
                </div>
                <select id="p-prov" class="w-full bg-slate-50 p-4 rounded-2xl outline-none font-bold text-sm border">
                    <option value="" disabled selected>Province d'origine</option>
                    <option>Estuaire</option><option>Haut-Ogooué</option><option>Moyen-Ogooué</option><option>Ngounié</option>
                    <option>Nyanga</option><option>Ogooué-Ivindo</option><option>Ogooué-Lolo</option><option>Ogooué-Maritime</option><option>Woleu-Ntem</option>
                </select>
                <button type="submit" class="w-full bg-emerald-600 text-white py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest shadow-xl shadow-emerald-50">Publier sur le réseau</button>
            </form>
        </div>
    </div>

    <!-- MESSAGERIE CHAT -->
    <div id="chat-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[600] hidden flex items-center justify-end p-4">
        <div class="bg-white w-full max-w-md h-[85vh] rounded-[2.5rem] shadow-2xl flex flex-col overflow-hidden">
            <div class="p-6 bg-slate-900 text-white flex justify-between items-center">
                <div>
                    <h4 id="chat-target" class="font-black text-lg uppercase tracking-tighter italic">Négociation</h4>
                    <p class="text-[8px] font-bold text-amber-400 uppercase tracking-widest">Messagerie sécurisée Echoppe241</p>
                </div>
                <button onclick="closeChat()" class="text-white text-2xl">&times;</button>
            </div>
            <div id="chat-messages" class="flex-grow p-5 overflow-y-auto flex flex-col gap-2 bg-slate-50"></div>
            <form id="chat-form" class="p-4 bg-white border-t flex gap-2">
                <input type="text" id="chat-input" placeholder="Votre message..." class="flex-grow bg-slate-50 p-3.5 rounded-xl outline-none font-bold text-sm border shadow-inner">
                <button class="bg-blue-600 text-white w-12 h-12 rounded-xl flex items-center justify-center"><i class="fa-solid fa-paper-plane"></i></button>
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
        const appId = 'echoppe241-v1';

        let currentUser = null;
        let selectedRegRole = 'buyer';
        let currentChatId = null;

        // --- AUTH LOGIC ---
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
                }
            } else {
                signInAnonymously(auth);
            }
        });

        // Register
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
                    fullName: name,
                    email: email,
                    phone: phone,
                    role: role,
                    status: status,
                    createdAt: serverTimestamp()
                });
                closeAuthModal();
                showToast("Compte créé avec succès !");
            } catch (err) { showToast(err.message); }
        };

        // Login
        document.getElementById('login-form').onsubmit = async (e) => {
            e.preventDefault();
            try {
                await signInWithEmailAndPassword(auth, document.getElementById('l-email').value, document.getElementById('l-pass').value);
                closeAuthModal();
            } catch (err) { showToast("Erreur de connexion"); }
        };

        // --- UI UPDATES ---
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

        // --- DATA SYNC ---
        function syncData() {
            // Stats Globale (ERP)
            onSnapshot(collection(db, 'artifacts', appId, 'users'), (snap) => {
                document.getElementById('stat-users').innerText = snap.size;
                const pending = snap.docs.filter(d => d.data().status === 'pending');
                document.getElementById('stat-pending').innerText = pending.length;
                
                if (currentUser?.role === 'admin') renderAdminPending(pending);
            });

            // Produits (Market + WMS)
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                document.getElementById('stat-products').innerText = snap.size;
                const products = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                renderMarket(products);
                if (currentUser?.role === 'producer') renderInventory(products.filter(p => p.ownerId === currentUser.uid));
            });
        }

        function renderAdminPending(list) {
            const container = document.getElementById('admin-pending-list');
            container.innerHTML = list.length ? '' : '<p class="text-xs text-slate-400 italic">Aucune demande.</p>';
            list.forEach(docSnap => {
                const u = docSnap.data();
                const div = document.createElement('div');
                div.className = "flex justify-between items-center bg-slate-50 p-4 rounded-2xl border border-slate-100";
                div.innerHTML = `
                    <div>
                        <p class="text-xs font-black uppercase">${u.fullName}</p>
                        <p class="text-[9px] font-bold text-slate-400">${u.phone}</p>
                    </div>
                    <button onclick="approveProducer('${docSnap.id}')" class="bg-emerald-600 text-white px-4 py-2 rounded-xl text-[9px] font-black uppercase">Approuver</button>
                `;
                container.appendChild(div);
            });
        }

        window.approveProducer = async (uid) => {
            await updateDoc(doc(db, 'artifacts', appId, 'users', uid), { status: 'active' });
            showToast("Producteur activé !");
        };

        function renderMarket(products) {
            const container = document.getElementById('market-grid');
            container.innerHTML = '';
            products.forEach(p => {
                const card = document.createElement('div');
                card.className = "bg-white p-4 rounded-[2rem] border shadow-sm flex flex-col gap-3 group cursor-pointer";
                card.onclick = () => openChatWith(p.ownerId, p.ownerName);
                card.innerHTML = `
                    <div class="h-32 bg-slate-100 rounded-2xl overflow-hidden relative">
                         <div class="absolute top-3 left-3 bg-white/90 backdrop-blur px-2 py-1 rounded-lg text-[8px] font-black uppercase text-blue-600">${p.province}</div>
                    </div>
                    <div>
                        <h4 class="font-black text-xs uppercase group-hover:text-blue-600 transition truncate">${p.name}</h4>
                        <p class="text-lg font-black mt-1">${p.price} <span class="text-[10px] text-slate-400">FCFA</span></p>
                    </div>
                    <div class="flex justify-between items-center mt-auto pt-2 border-t border-dashed">
                        <span class="text-[9px] font-bold text-slate-400 uppercase tracking-tighter">${p.ownerName}</span>
                        <div class="w-8 h-8 bg-blue-50 text-blue-600 rounded-lg flex items-center justify-center text-xs"><i class="fa-solid fa-message"></i></div>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function renderInventory(myProducts) {
            const container = document.getElementById('producer-inventory');
            container.innerHTML = myProducts.length ? '' : '<p class="text-xs text-slate-400 italic">Aucun produit en stock.</p>';
            myProducts.forEach(p => {
                const div = document.createElement('div');
                div.className = "bg-slate-50 p-4 rounded-2xl border flex justify-between items-center";
                div.innerHTML = `
                    <div>
                        <p class="text-[10px] font-black uppercase">${p.name}</p>
                        <p class="text-lg font-black">${p.qty} <span class="text-[9px] text-slate-400">Unités</span></p>
                    </div>
                    <div class="flex gap-2">
                         <button onclick="updateQty('${p.id}', ${p.qty + 1})" class="w-8 h-8 bg-white border rounded-lg text-xs">+</button>
                         <button onclick="updateQty('${p.id}', ${Math.max(0, p.qty - 1)})" class="w-8 h-8 bg-white border rounded-lg text-xs">-</button>
                    </div>
                `;
                container.appendChild(div);
            });
        }

        window.updateQty = async (id, newQty) => {
            await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', id), { qty: newQty });
        };

        // --- PRODUCT PUBLISH ---
        document.getElementById('product-form').onsubmit = async (e) => {
            e.preventDefault();
            if (!currentUser || currentUser.status !== 'active') return showToast("Compte non validé");
            
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name: document.getElementById('p-name').value,
                    price: document.getElementById('p-price').value,
                    qty: Number(document.getElementById('p-qty').value),
                    province: document.getElementById('p-prov').value,
                    ownerId: currentUser.uid,
                    ownerName: currentUser.fullName,
                    createdAt: serverTimestamp()
                });
                document.getElementById('publish-modal').classList.add('hidden');
                showToast("Produit ajouté au WMS !");
            } catch (err) { showToast("Erreur de publication"); }
        };

        // --- MESSAGERIE ---
        async function openChatWith(targetId, targetName) {
            if (!currentUser || currentUser.uid === targetId) return;
            document.getElementById('chat-modal').classList.remove('hidden');
            document.getElementById('chat-target').innerText = targetName;
            
            // ID de chat unique entre deux personnes
            currentChatId = [currentUser.uid, targetId].sort().join('_');
            
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'chats', currentChatId, 'messages'), orderBy('createdAt', 'asc')), (snap) => {
                const container = document.getElementById('chat-messages');
                container.innerHTML = '';
                snap.forEach(d => {
                    const m = d.data();
                    const b = document.createElement('div');
                    b.className = `chat-bubble ${m.senderId === currentUser.uid ? 'chat-mine' : 'chat-theirs'}`;
                    b.innerText = m.text;
                    container.appendChild(b);
                });
                container.scrollTop = container.scrollHeight;
            });
        }

        document.getElementById('chat-form').onsubmit = async (e) => {
            e.preventDefault();
            const input = document.getElementById('chat-input');
            if (!input.value.trim() || !currentChatId) return;

            const targetId = currentChatId.replace(currentUser.uid, '').replace('_', '');

            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', currentChatId, 'messages'), {
                text: input.value,
                senderId: currentUser.uid,
                createdAt: serverTimestamp()
            });
            
            // Mise à jour de la liste des conversations pour les deux
            await setDoc(doc(db, 'artifacts', appId, 'public', 'data', 'user_chats', currentUser.uid, 'conversations', targetId), {
                lastMsg: input.value,
                updatedAt: serverTimestamp()
            });
             await setDoc(doc(db, 'artifacts', appId, 'public', 'data', 'user_chats', targetId, 'conversations', currentUser.uid), {
                lastMsg: input.value,
                updatedAt: serverTimestamp()
            });

            input.value = '';
        };

        // --- PWA HELPERS ---
        function showToast(msg) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3000);
        }

        window.openAuthModal = () => document.getElementById('auth-modal').classList.remove('hidden');
        window.closeAuthModal = () => document.getElementById('auth-modal').classList.add('hidden');
        window.closeChat = () => document.getElementById('chat-modal').classList.add('hidden');
        window.toggleAuthMode = (m) => {
            document.getElementById('login-form').classList.toggle('hidden', m === 'reg');
            document.getElementById('reg-form').classList.toggle('hidden', m === 'login');
            document.getElementById('auth-title').innerText = m === 'reg' ? "Inscription" : "Connexion";
        };
        window.setRegRole = (r) => {
            selectedRegRole = r;
            document.getElementById('role-b').classList.toggle('active', r === 'buyer');
            document.getElementById('role-p').classList.toggle('active', r === 'producer');
        };
        window.openPublishModal = () => document.getElementById('publish-modal').classList.remove('hidden');
        window.handleLogout = () => signOut(auth).then(() => location.reload());

        // --- PWA SETUP (Simulé pour le preview) ---
        const manifest = {
            name: "Echoppe 241 ERP",
            short_name: "Echoppe241",
            start_url: ".",
            display: "standalone",
            background_color: "#f8fafc",
            theme_color: "#0f172a",
            icons: [{ src: "https://i.ibb.co/2Q73j3X/echoppe241-logo.png", sizes: "512x512", type: "image/png" }]
        };
        const manifestBlob = new Blob([JSON.stringify(manifest)], {type: 'application/json'});
        const manifestURL = URL.createObjectURL(manifestBlob);
        const link = document.createElement('link');
        link.rel = 'manifest';
        link.href = manifestURL;
        document.head.appendChild(link);

    </script>
</body>
</html>
