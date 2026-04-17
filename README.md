<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Marché Local Gabonais</title>
    
    <link rel="icon" type="image/png" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <link rel="apple-touch-icon" href="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { --gabon-green: #00853f; --gabon-yellow: #fcd116; --gabon-blue: #3a75c4; --deep-slate: #0f172a; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; color: var(--deep-slate); -webkit-tap-highlight-color: transparent; }
        .brand-gradient { background: linear-gradient(135deg, var(--gabon-green), #10b981); }
        .glass { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-bottom: 1px solid rgba(0,0,0,0.05); }
        .product-card { transition: all 0.25s ease; cursor: pointer; border: 1px solid #f1f5f9; }
        .product-card:active { transform: scale(0.98); }
        .hide-scroll::-webkit-scrollbar { display: none; }
        .role-btn.active { border-color: var(--gabon-green); background-color: #ecfdf5; color: #065f46; border-width: 2px; }
        
        /* Shimmer effect for loading */
        .shimmer { background: linear-gradient(90deg, #f0f0f0 25%, #f8f8f8 50%, #f0f0f0 75%); background-size: 200% 100%; animation: shimmer 1.5s infinite; }
        @keyframes shimmer { 0% { background-position: -200% 0; } 100% { background-position: 200% 0; } }
    </style>
</head>
<body>

    <!-- Notification Toast -->
    <div id="toast" class="fixed top-6 left-1/2 -translate-x-1/2 z-[3000] hidden">
        <div class="bg-slate-900 text-white px-6 py-3 rounded-2xl shadow-2xl flex items-center gap-3 border-b-2 border-amber-400">
            <span id="toast-msg" class="text-[10px] font-extrabold uppercase tracking-wider">Message</span>
        </div>
    </div>

    <!-- Header -->
    <header class="sticky top-0 z-[1000] glass">
        <div class="h-[3px] flex">
            <div class="flex-1 bg-[var(--gabon-green)]"></div>
            <div class="flex-1 bg-[var(--gabon-yellow)]"></div>
            <div class="flex-1 bg-[var(--gabon-blue)]"></div>
        </div>
        <div class="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="location.reload()">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" alt="Echoppe241" class="h-8 w-auto">
                <h1 class="font-extrabold text-xl tracking-tighter uppercase">Echoppe<span class="text-amber-500">241</span></h1>
            </div>
            <div id="nav-actions" class="flex items-center gap-2">
                <button id="btn-login-trigger" onclick="toggleAuthModal()" class="bg-slate-900 text-white px-5 py-2.5 rounded-xl font-bold text-[10px] uppercase tracking-wider">Connexion</button>
                <div id="user-profile" class="hidden flex items-center gap-3 bg-slate-50 p-1 rounded-2xl border">
                    <div id="user-initials" class="w-9 h-9 rounded-xl brand-gradient text-white flex items-center justify-center font-bold text-xs uppercase">?</div>
                    <button onclick="handleLogout()" class="pr-3 pl-1 text-slate-400 hover:text-red-500 transition-colors"><i class="fa-solid fa-power-off"></i></button>
                </div>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-6">
        <!-- Dashboard Producteur -->
        <section id="view-producer" class="hidden mb-10 space-y-6">
            <div class="brand-gradient text-white p-8 rounded-[2.5rem] flex flex-col md:flex-row justify-between items-center gap-4 shadow-lg">
                <div>
                    <h2 class="text-3xl font-black italic uppercase tracking-tighter">Tableau de Bord</h2>
                    <p class="text-emerald-100 text-[10px] font-bold uppercase tracking-widest">Vos ventes en temps réel</p>
                </div>
                <button onclick="togglePublishModal()" class="bg-white text-slate-900 px-8 py-4 rounded-2xl font-black text-[10px] uppercase tracking-widest shadow-xl hover:bg-slate-50 transition-all">
                    + Publier un produit
                </button>
            </div>
            <div id="my-inventory" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
        </section>

        <!-- Marché -->
        <section id="view-buyer">
            <div class="relative rounded-[2.5rem] overflow-hidden bg-slate-900 min-h-[220px] mb-10 flex items-center p-8 text-white shadow-2xl">
                <div class="absolute inset-0 opacity-40 bg-cover bg-center" style="background-image: url('https://images.unsplash.com/photo-1542838132-92c53300491e?auto=format&fit=crop&w=1200&q=80');"></div>
                <div class="relative z-10 max-w-lg">
                    <span class="bg-amber-500 text-slate-900 px-3 py-1 rounded-full text-[9px] font-black uppercase mb-4 inline-block">Production Nationale</span>
                    <h2 class="text-4xl font-black leading-none mb-3 italic tracking-tighter uppercase">Le terroir <br><span class="text-emerald-400">gabonais.</span></h2>
                    <p class="text-slate-300 text-xs font-medium uppercase tracking-wider">Achetez directement aux planteurs des 9 provinces.</p>
                </div>
            </div>

            <div class="flex flex-col md:flex-row gap-4 mb-10">
                <div class="relative flex-grow">
                    <i class="fa-solid fa-magnifying-glass absolute left-4 top-1/2 -translate-y-1/2 text-slate-400"></i>
                    <input type="text" id="search-input" oninput="renderMarket()" placeholder="Manioc, Banane douce, Huile de palme..." class="w-full bg-white border border-slate-200 pl-12 pr-4 py-4 rounded-2xl outline-none focus:border-emerald-500 font-bold text-sm shadow-sm transition-all">
                </div>
                <div class="flex gap-2 overflow-x-auto pb-2 hide-scroll" id="province-filters">
                    <button onclick="setFilter('all')" class="bg-slate-900 text-white px-6 py-3 rounded-2xl text-[10px] font-bold uppercase whitespace-nowrap shadow-md">Toutes</button>
                </div>
            </div>

            <div id="market-grid" class="grid grid-cols-2 lg:grid-cols-4 gap-4 md:gap-6">
                <!-- Loading State initial -->
                <div class="shimmer h-64 rounded-[2rem]"></div>
                <div class="shimmer h-64 rounded-[2rem]"></div>
                <div class="shimmer h-64 rounded-[2rem]"></div>
                <div class="shimmer h-64 rounded-[2rem]"></div>
            </div>
        </section>
    </main>

    <!-- Modal Authentification -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] p-8 md:p-10 shadow-2xl relative">
            <button onclick="toggleAuthModal()" class="absolute top-6 right-6 text-slate-300 hover:text-slate-900 text-xl transition-colors">&times;</button>
            <h2 id="auth-title" class="text-2xl font-black uppercase italic text-center mb-8 tracking-tighter">Connexion</h2>
            
            <div id="login-form" class="space-y-4">
                <input type="email" id="login-email" placeholder="Email" class="w-full bg-slate-50 p-4 rounded-xl border outline-none focus:border-emerald-500 font-bold">
                <input type="password" id="login-pass" placeholder="Mot de passe" class="w-full bg-slate-50 p-4 rounded-xl border outline-none focus:border-emerald-500 font-bold">
                <button onclick="handleLogin()" class="w-full brand-gradient text-white py-4 rounded-xl font-bold uppercase text-[10px] tracking-widest shadow-lg active:scale-95 transition-transform">Entrer</button>
                <p class="text-center text-[10px] font-extrabold text-slate-400 uppercase pt-4">Pas encore de compte ? <button onclick="switchAuth('reg')" class="text-emerald-600 underline">S'inscrire</button></p>
            </div>

            <div id="reg-form" class="space-y-4 hidden">
                <input type="text" id="reg-name" placeholder="Nom complet ou Coopérative" class="w-full bg-slate-50 p-4 rounded-xl border outline-none font-bold">
                <input type="email" id="reg-email" placeholder="Email" class="w-full bg-slate-50 p-4 rounded-xl border outline-none font-bold">
                <input type="password" id="reg-pass" placeholder="Mot de passe (min 6 car.)" class="w-full bg-slate-50 p-4 rounded-xl border outline-none font-bold">
                <div class="grid grid-cols-2 gap-3">
                    <button id="role-buyer" onclick="setRole('buyer')" class="role-btn p-3 rounded-xl border-2 active font-bold text-[10px] uppercase">Acheteur</button>
                    <button id="role-producer" onclick="setRole('producer')" class="role-btn p-3 rounded-xl border-2 font-bold text-[10px] uppercase">Producteur</button>
                </div>
                <button onclick="handleRegister()" class="w-full bg-slate-900 text-white py-4 rounded-xl font-bold uppercase text-[10px] tracking-widest shadow-lg active:scale-95 transition-transform">Créer mon compte</button>
                <p class="text-center text-[10px] font-extrabold text-slate-400 uppercase pt-4">Déjà inscrit ? <button onclick="switchAuth('login')" class="text-slate-900 underline">Se connecter</button></p>
            </div>
        </div>
    </div>

    <!-- Modal Publication -->
    <div id="publish-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-md z-[2000] hidden flex items-center justify-center p-4">
        <form id="product-form" class="bg-white w-full max-w-lg rounded-[2.5rem] p-8 shadow-2xl space-y-4">
            <h3 class="text-2xl font-black mb-4 uppercase italic tracking-tighter">Publier une récolte</h3>
            <div class="space-y-1">
                <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Nom de l'article</label>
                <input type="text" id="p-name" placeholder="ex: Manioc de Booué" class="w-full bg-slate-50 p-4 rounded-xl border font-bold" required>
            </div>
            <div class="grid grid-cols-2 gap-4">
                <div class="space-y-1">
                    <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Province d'origine</label>
                    <select id="p-province" class="w-full bg-slate-50 p-4 rounded-xl border font-bold">
                        <option>Estuaire</option><option>Woleu-Ntem</option><option>Ogooué-Maritime</option><option>Haut-Ogooué</option><option>Ngounié</option><option>Nyanga</option><option>Ogooué-Ivindo</option><option>Ogooué-Lolo</option><option>Moyen-Ogooué</option>
                    </select>
                </div>
                <div class="space-y-1">
                    <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Prix (FCFA)</label>
                    <input type="number" id="p-price" placeholder="Prix total" class="w-full bg-slate-50 p-4 rounded-xl border font-bold" required>
                </div>
            </div>
            <div class="space-y-1">
                <label class="text-[9px] font-black uppercase text-slate-400 ml-1">Quantité / Unité</label>
                <input type="text" id="p-unit" placeholder="ex: Sac de 50kg, Tas de 5, Litre" class="w-full bg-slate-50 p-4 rounded-xl border font-bold" required>
            </div>
            <div class="flex gap-2 pt-2">
                <button type="submit" class="flex-grow bg-slate-900 text-white py-4 rounded-xl font-bold uppercase text-[10px] tracking-widest shadow-md">Confirmer la mise en vente</button>
                <button type="button" onclick="togglePublishModal()" class="px-6 border rounded-xl text-slate-400 font-bold hover:text-red-500 transition-colors">ANNULER</button>
            </div>
        </form>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, collection, addDoc, onSnapshot, query, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = 'echoppe241-prod-v1';

        let userProfile = null;
        let selectedRole = 'buyer';
        let allProducts = [];
        let activeFilter = 'all';

        // --- Auth & Profile ---
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                const uDoc = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid));
                if (uDoc.exists()) {
                    userProfile = { uid: user.uid, ...uDoc.data() };
                    updateUI();
                } else {
                    userProfile = { uid: user.uid, role: 'guest', fullName: 'Invité' };
                    updateUI();
                }
                syncMarket();
            } else {
                signInAnonymously(auth).catch(err => console.error("Mode invité échoué", err));
            }
        });

        // --- Data Listeners ---
        function syncMarket() {
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                allProducts = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                renderMarket();
                if(userProfile?.role === 'producer') renderInventory();
            }, (err) => {
                console.error("Erreur Firestore Sync:", err);
                showToast("Erreur de synchronisation");
            });
        }

        function renderMarket() {
            const grid = document.getElementById('market-grid');
            const search = document.getElementById('search-input').value.toLowerCase();
            const filtered = allProducts.filter(p => {
                const matchesSearch = p.name.toLowerCase().includes(search);
                const matchesProvince = activeFilter === 'all' || p.province === activeFilter;
                return matchesSearch && matchesProvince;
            });

            grid.innerHTML = filtered.length ? '' : '<div class="col-span-full py-24 text-center"><i class="fa-solid fa-seedling text-4xl text-slate-100 mb-4 block"></i><p class="font-bold text-slate-400 text-[10px] uppercase tracking-widest">Aucun produit ne correspond</p></div>';
            
            filtered.sort((a,b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0)).forEach(p => {
                const card = document.createElement('div');
                card.className = "product-card bg-white p-4 rounded-[2rem] shadow-sm flex flex-col";
                card.onclick = () => showToast("Bientôt : Messagerie directe !");
                card.innerHTML = `
                    <div class="h-36 bg-slate-50 rounded-2xl mb-4 flex items-center justify-center relative overflow-hidden group">
                        <span class="absolute top-3 left-3 bg-white/90 backdrop-blur px-3 py-1 rounded-full text-[8px] font-black uppercase shadow-sm border border-slate-100">${p.province}</span>
                        <i class="fa-solid fa-bag-shopping text-4xl text-slate-200 group-hover:scale-110 transition-transform"></i>
                    </div>
                    <div class="flex-grow">
                        <h4 class="font-bold text-[11px] uppercase truncate mb-1 text-slate-800">${p.name}</h4>
                        <p class="text-[13px] font-black text-emerald-600 mb-4">${p.price.toLocaleString()} FCFA <span class="text-[8px] text-slate-400 font-bold uppercase">/ ${p.unit}</span></p>
                    </div>
                    <div class="flex items-center justify-between border-t border-slate-50 pt-3">
                        <div class="flex items-center gap-2 overflow-hidden">
                            <div class="w-5 h-5 rounded-full brand-gradient flex items-center justify-center text-[7px] text-white font-bold">${p.ownerName.charAt(0)}</div>
                            <span class="text-[8px] font-black uppercase text-slate-400 truncate w-20">${p.ownerName}</span>
                        </div>
                        <div class="w-8 h-8 rounded-xl bg-emerald-50 text-emerald-600 flex items-center justify-center text-[10px] hover:bg-emerald-100 transition-colors"><i class="fa-solid fa-message"></i></div>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        function renderInventory() {
            const box = document.getElementById('my-inventory');
            const mine = allProducts.filter(p => p.ownerId === userProfile.uid);
            box.innerHTML = mine.length ? '' : '<div class="col-span-full border-2 border-dashed border-slate-100 p-12 rounded-[2rem] text-center text-slate-300 font-bold uppercase text-[9px]">Votre étal est vide</div>';
            mine.forEach(p => {
                const div = document.createElement('div');
                div.className = "bg-white p-4 rounded-2xl border border-slate-100 flex items-center gap-4 shadow-sm";
                div.innerHTML = `
                    <div class="w-12 h-12 bg-emerald-50 rounded-xl flex items-center justify-center text-emerald-600"><i class="fa-solid fa-check-circle"></i></div>
                    <div class="flex-grow"><h5 class="font-bold text-[10px] uppercase text-slate-800">${p.name}</h5><p class="text-[9px] font-bold text-slate-400">${p.price.toLocaleString()} FCFA</p></div>
                    <div class="bg-emerald-500 text-white px-3 py-1 rounded-full text-[7px] font-black uppercase tracking-tighter">En ligne</div>
                `;
                box.appendChild(div);
            });
        }

        // --- Actions Handlers ---
        window.handleLogin = async () => {
            const email = document.getElementById('login-email').value;
            const pass = document.getElementById('login-pass').value;
            if(!email || !pass) return showToast("Saisissez vos identifiants");
            try {
                await signInWithEmailAndPassword(auth, email, pass);
                toggleAuthModal();
                showToast("Heureux de vous revoir !");
            } catch (e) { showToast("Identifiants incorrects"); }
        };

        window.handleRegister = async () => {
            const name = document.getElementById('reg-name').value;
            const email = document.getElementById('reg-email').value;
            const pass = document.getElementById('reg-pass').value;
            
            if(!name || !email || !pass) return showToast("Complétez le formulaire");
            if(pass.length < 6) return showToast("Mot de passe : 6 car. minimum");

            try {
                const cred = await createUserWithEmailAndPassword(auth, email, pass);
                await setDoc(doc(db, 'artifacts', appId, 'users', cred.user.uid), {
                    fullName: name,
                    role: selectedRole,
                    email: email,
                    createdAt: serverTimestamp()
                });
                toggleAuthModal();
                showToast("Bienvenue dans la communauté !");
            } catch (e) { 
                console.error(e);
                showToast("Échec : " + (e.code === 'auth/email-already-in-use' ? "Email déjà pris" : "Erreur serveur")); 
            }
        };

        window.handleLogout = () => signOut(auth).then(() => location.reload());

        document.getElementById('product-form').onsubmit = async (e) => {
            e.preventDefault();
            if(!userProfile || userProfile.role === 'guest') return toggleAuthModal();
            
            const data = {
                name: document.getElementById('p-name').value,
                province: document.getElementById('p-province').value,
                price: parseInt(document.getElementById('p-price').value),
                unit: document.getElementById('p-unit').value,
                ownerId: userProfile.uid,
                ownerName: userProfile.fullName,
                createdAt: serverTimestamp()
            };

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), data);
                togglePublishModal();
                showToast("Produit en ligne !");
                document.getElementById('product-form').reset();
            } catch (e) { showToast("Erreur de publication"); }
        };

        // --- UI Utils ---
        function updateUI() {
            const isGuest = userProfile?.role === 'guest';
            document.getElementById('btn-login-trigger').classList.toggle('hidden', !isGuest);
            document.getElementById('user-profile').classList.toggle('hidden', isGuest);
            if(!isGuest) {
                document.getElementById('user-initials').innerText = userProfile.fullName.charAt(0);
                document.getElementById('view-producer').classList.toggle('hidden', userProfile.role !== 'producer');
            }
        }

        window.toggleAuthModal = () => document.getElementById('auth-modal').classList.toggle('hidden');
        window.switchAuth = (mode) => {
            document.getElementById('login-form').classList.toggle('hidden', mode === 'reg');
            document.getElementById('reg-form').classList.toggle('hidden', mode === 'login');
            document.getElementById('auth-title').innerText = mode === 'reg' ? "Inscription" : "Connexion";
        };
        window.setRole = (role) => {
            selectedRole = role;
            document.getElementById('role-buyer').classList.toggle('active', role === 'buyer');
            document.getElementById('role-producer').classList.toggle('active', role === 'producer');
        };
        window.togglePublishModal = () => document.getElementById('publish-modal').classList.toggle('hidden');
        window.setFilter = (p) => {
            activeFilter = p;
            renderMarket();
        };

        function showToast(msg) {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            t.classList.remove('hidden');
            setTimeout(() => t.classList.add('hidden'), 3500);
        }

        // Init Provinces
        const provinces = ["Estuaire", "Woleu-Ntem", "Ogooué-Maritime", "Haut-Ogooué", "Ngounié", "Nyanga", "Ogooué-Ivindo", "Ogooué-Lolo", "Moyen-Ogooué"];
        const filterContainer = document.getElementById('province-filters');
        provinces.forEach(p => {
            const btn = document.createElement('button');
            btn.className = "bg-white border border-slate-100 px-6 py-3 rounded-2xl text-[10px] font-bold uppercase whitespace-nowrap hover:bg-slate-50 transition-colors shadow-sm";
            btn.innerText = p;
            btn.onclick = () => setFilter(p);
            filterContainer.appendChild(btn);
        });

    </script>
</body>
</html>
