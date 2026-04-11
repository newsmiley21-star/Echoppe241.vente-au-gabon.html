<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 | Authentification Autonome</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --gab-green: #00853f;
            --gab-yellow: #fcd116;
            --gab-blue: #3a75c4;
        }
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; overflow-x: hidden; }
        
        .bg-gab-green { background-color: var(--gab-green); }
        .bg-gab-yellow { background-color: var(--gab-yellow); }
        .bg-gab-blue { background-color: var(--gab-blue); }
        .text-gab-blue { color: var(--gab-blue); }

        .cart-badge { top: -5px; right: -5px; }
        .animate-fade-in { animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    </style>
</head>
<body class="antialiased">

    <!-- Bannière Couleurs Gabon -->
    <div class="h-1 flex sticky top-0 z-[60]">
        <div class="flex-1 bg-gab-green"></div>
        <div class="flex-1 bg-gab-yellow"></div>
        <div class="flex-1 bg-gab-blue"></div>
    </div>

    <!-- Navigation -->
    <header class="bg-white/90 backdrop-blur-md shadow-sm sticky top-1 z-50 border-b border-slate-100">
        <div class="max-w-7xl mx-auto px-4 py-2 flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="showSection('marketplace')">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Echoppe241 Logo" class="h-10 md:h-12 w-auto object-contain">
                <div class="flex flex-col leading-none">
                    <span class="font-black text-lg tracking-tighter text-slate-800">ECHOPPE<span class="text-gab-blue">241</span></span>
                    <span class="text-[8px] font-bold text-gab-green uppercase tracking-widest text-nowrap">Le Marché National</span>
                </div>
            </div>

            <div class="hidden lg:flex items-center gap-8 text-xs font-bold uppercase tracking-wider text-slate-600">
                <button onclick="showSection('marketplace')" class="hover:text-gab-blue transition">Boutique</button>
                <button onclick="checkAuthAndShow('publish')" class="hover:text-gab-blue transition">Vendre</button>
                <button onclick="checkAuthAndShow('orders')" class="hover:text-gab-blue transition">Commandes</button>
            </div>

            <div class="flex items-center gap-3">
                <button onclick="toggleCart()" class="relative p-2 text-slate-600 hover:text-gab-blue">
                    <i class="fa-solid fa-cart-shopping text-xl"></i>
                    <span id="cartCount" class="absolute cart-badge bg-red-500 text-white text-[9px] font-bold w-5 h-5 flex items-center justify-center rounded-full border-2 border-white">0</span>
                </button>
                <button id="authBtn" onclick="toggleAuthModal()" class="bg-slate-100 text-slate-700 px-4 py-2 rounded-full text-xs font-bold hover:bg-gab-blue hover:text-white transition">
                    <i class="fa-solid fa-user mr-2"></i>Connexion
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-4 md:p-8">
        <!-- SECTION: MARKETPLACE -->
        <section id="section-marketplace" class="space-y-8 animate-fade-in">
            <div class="bg-gradient-to-br from-gab-blue to-blue-800 rounded-[2.5rem] p-8 md:p-12 text-white relative overflow-hidden shadow-2xl">
                <div class="relative z-10 max-w-lg">
                    <span class="bg-gab-yellow text-gab-blue px-3 py-1 rounded-full text-[10px] font-black uppercase mb-4 inline-block">Direct des Provinces</span>
                    <h1 class="text-4xl md:text-5xl font-black mb-4 leading-tight">Créez votre boutique en 2 minutes.</h1>
                    <p class="text-blue-100 mb-6 text-sm md:text-base">Inscrivez-vous dès maintenant pour vendre vos produits ou suivre vos commandes.</p>
                    <button onclick="toggleAuthModal()" class="bg-white text-gab-blue px-6 py-3 rounded-2xl font-black text-sm uppercase shadow-lg hover:bg-gab-yellow transition">Commencer</button>
                </div>
                <i class="fa-solid fa-shop absolute -right-10 -bottom-10 text-[200px] text-white/10 rotate-12"></i>
            </div>
            <div id="productGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6"></div>
        </section>

        <!-- SECTION: PUBLIER -->
        <section id="section-publish" class="hidden max-w-2xl mx-auto py-10 animate-fade-in">
            <div class="bg-white rounded-[2.5rem] shadow-xl border border-slate-100 overflow-hidden">
                <div class="bg-gab-green p-8 text-white">
                    <h2 class="text-2xl font-black uppercase">Vendre un produit</h2>
                </div>
                <form id="productForm" class="p-8 space-y-6">
                    <input type="text" id="pName" placeholder="Nom du produit" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-green" required>
                    <input type="number" id="pPrice" placeholder="Prix (FCFA)" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-green" required>
                    <select id="pProvince" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none">
                        <option>Estuaire</option><option>Woleu-Ntem</option><option>Ogooué-Maritime</option><option>Haut-Ogooué</option>
                    </select>
                    <input type="text" id="pImg" placeholder="Lien image" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none" required>
                    <button type="submit" class="w-full bg-gab-green text-white py-5 rounded-2xl font-black uppercase tracking-widest">Publier</button>
                </form>
            </div>
        </section>

        <!-- SECTION: COMMANDES -->
        <section id="section-orders" class="hidden max-w-4xl mx-auto py-10 animate-fade-in">
            <h2 class="text-3xl font-black text-slate-800 mb-8">Historique d'achats</h2>
            <div id="orderList" class="space-y-4"></div>
        </section>
    </main>

    <!-- MODAL AUTHENTIFICATION -->
    <div id="authModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-[200] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[2.5rem] shadow-2xl overflow-hidden p-8 animate-fade-in">
            <div class="flex justify-between items-center mb-8">
                <h2 id="authTitle" class="text-2xl font-black text-slate-800 uppercase italic">Connexion</h2>
                <button onclick="toggleAuthModal()" class="text-2xl text-slate-300 hover:text-red-500">&times;</button>
            </div>
            <form id="authForm" class="space-y-4">
                <div id="displayNameGroup" class="hidden">
                    <label class="block text-[10px] font-black text-slate-400 uppercase mb-1">Nom complet</label>
                    <input type="text" id="authName" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue" placeholder="Ex: Jean Marc">
                </div>
                <div>
                    <label class="block text-[10px] font-black text-slate-400 uppercase mb-1">Email</label>
                    <input type="email" id="authEmail" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue" placeholder="votre@email.com" required>
                </div>
                <div>
                    <label class="block text-[10px] font-black text-slate-400 uppercase mb-1">Mot de passe</label>
                    <input type="password" id="authPassword" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue" placeholder="••••••••" required>
                </div>
                <button type="submit" id="authSubmitBtn" class="w-full bg-gab-blue text-white py-5 rounded-2xl font-black uppercase tracking-widest hover:shadow-lg transition">Se connecter</button>
            </form>
            <div class="mt-6 text-center">
                <button id="authSwitch" class="text-xs font-bold text-slate-500 hover:text-gab-blue underline">Pas de compte ? Créer un compte</button>
            </div>
        </div>
    </div>

    <!-- SIDE CART -->
    <div id="cartSidebar" class="fixed inset-y-0 right-0 w-full md:w-96 bg-white shadow-2xl z-[100] transform translate-x-full transition-transform duration-300 flex flex-col">
        <div class="p-6 border-b flex justify-between items-center bg-slate-50">
            <h3 class="font-black text-xl">Mon Panier</h3>
            <button onclick="toggleCart()" class="text-2xl">&times;</button>
        </div>
        <div id="cartItems" class="flex-grow overflow-y-auto p-6 space-y-4"></div>
        <div class="p-6 border-t bg-slate-50">
            <button onclick="checkout()" class="w-full bg-gab-blue text-white py-5 rounded-2xl font-black uppercase tracking-widest">Commander</button>
        </div>
    </div>

    <div id="notif" class="fixed top-20 left-1/2 -translate-x-1/2 z-[300] bg-slate-800 text-white px-6 py-3 rounded-full text-xs font-bold shadow-2xl hidden"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut, updateProfile } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, onSnapshot, orderBy, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // CONFIGURATION FIREBASE FOURNIE
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            databaseURL: "https://communautedugabon-default-rtdb.firebaseio.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f",
            measurementId: "G-T7415FPS91"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = "echoppe241-prod";

        let currentUser = null;
        let isSignUpMode = false;
        let cart = [];

        const staticProducts = [
            { id: 1, name: "Huile de Palme Rouge", price: 18000, province: "Ogooué-Lolo", img: "https://images.unsplash.com/photo-1620916566398-39f1143f2c0a?auto=format&fit=crop&w=400&q=80" },
            { id: 2, name: "Atanga de Makokou", price: 3500, province: "Ogooué-Ivindo", img: "https://images.unsplash.com/photo-1615141982883-c7ad0e69fd62?auto=format&fit=crop&w=400&q=80" }
        ];

        onAuthStateChanged(auth, (user) => {
            currentUser = user;
            const btn = document.getElementById('authBtn');
            if (user) {
                btn.innerHTML = `<i class="fa-solid fa-circle-user mr-2 text-gab-green"></i> ${user.displayName || 'Mon Profil'}`;
                btn.onclick = logout;
                loadOrders();
                closeAuthModal();
            } else {
                btn.innerHTML = `<i class="fa-solid fa-user mr-2"></i> Connexion`;
                btn.onclick = toggleAuthModal;
            }
        });

        document.getElementById('authSwitch').onclick = () => {
            isSignUpMode = !isSignUpMode;
            document.getElementById('authTitle').innerText = isSignUpMode ? "Créer un compte" : "Connexion";
            document.getElementById('authSubmitBtn').innerText = isSignUpMode ? "S'inscrire" : "Se connecter";
            document.getElementById('authSwitch').innerText = isSignUpMode ? "Déjà un compte ? Se connecter" : "Pas de compte ? Créer un compte";
            document.getElementById('displayNameGroup').classList.toggle('hidden', !isSignUpMode);
        };

        document.getElementById('authForm').onsubmit = async (e) => {
            e.preventDefault();
            const email = document.getElementById('authEmail').value;
            const pass = document.getElementById('authPassword').value;
            const name = document.getElementById('authName').value;
            const btn = document.getElementById('authSubmitBtn');

            btn.disabled = true;
            btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin"></i>...`;

            try {
                if (isSignUpMode) {
                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    await updateProfile(res.user, { displayName: name });
                    notify("Compte créé avec succès !");
                } else {
                    await signInWithEmailAndPassword(auth, email, pass);
                    notify("Heureux de vous revoir !");
                }
            } catch (err) {
                notify("Erreur: " + err.message);
            } finally {
                btn.disabled = false;
                btn.innerText = isSignUpMode ? "S'inscrire" : "Se connecter";
            }
        };

        async function logout() {
            await signOut(auth);
            notify("Déconnecté.");
            showSection('marketplace');
        }

        window.showSection = (id) => {
            document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`section-${id}`).classList.remove('hidden');
        };

        window.checkAuthAndShow = (id) => {
            if (!currentUser) { toggleAuthModal(); notify("Connectez-vous pour continuer."); }
            else { showSection(id); }
        };

        window.toggleAuthModal = () => document.getElementById('authModal').classList.toggle('hidden');
        window.closeAuthModal = () => document.getElementById('authModal').classList.add('hidden');
        window.toggleCart = () => document.getElementById('cartSidebar').classList.toggle('translate-x-full');

        window.notify = (msg) => {
            const el = document.getElementById('notif');
            el.innerText = msg; el.classList.remove('hidden');
            setTimeout(() => el.classList.add('hidden'), 3000);
        };

        function renderProducts() {
            const grid = document.getElementById('productGrid');
            grid.innerHTML = staticProducts.map(p => `
                <div class="bg-white rounded-[2rem] overflow-hidden shadow-sm border border-slate-100 p-5 hover:shadow-xl transition">
                    <img src="${p.img}" class="w-full h-40 object-cover rounded-xl mb-4">
                    <h3 class="font-bold text-slate-800">${p.name}</h3>
                    <p class="text-gab-blue font-black mb-4">${p.price.toLocaleString()} FCFA</p>
                    <button onclick="addToCart(${p.id})" class="w-full bg-slate-100 py-3 rounded-xl text-[10px] font-black uppercase hover:bg-gab-blue hover:text-white transition">Ajouter</button>
                </div>
            `).join('');
        }

        window.addToCart = (id) => {
            cart.push(staticProducts.find(x => x.id === id));
            updateCartUI();
            notify("Ajouté au panier");
        };

        function updateCartUI() {
            document.getElementById('cartCount').innerText = cart.length;
            document.getElementById('cartItems').innerHTML = cart.map((item, idx) => `
                <div class="flex justify-between items-center bg-slate-50 p-3 rounded-xl">
                    <span class="text-xs font-bold">${item.name}</span>
                    <button onclick="removeFromCart(${idx})" class="text-red-400">&times;</button>
                </div>
            `).join('');
        }
        
        window.removeFromCart = (idx) => { cart.splice(idx,1); updateCartUI(); };

        window.checkout = async () => {
            if (!currentUser) return toggleAuthModal();
            if (cart.length === 0) return notify("Panier vide");
            
            await addDoc(collection(db, 'artifacts', appId, 'users', currentUser.uid, 'orders'), {
                total: cart.reduce((a,b) => a + b.price, 0),
                items: cart,
                createdAt: serverTimestamp()
            });
            cart = []; updateCartUI(); toggleCart(); showSection('orders'); notify("Succès !");
        };

        function loadOrders() {
            if (!currentUser) return;
            const q = query(collection(db, 'artifacts', appId, 'users', currentUser.uid, 'orders'), orderBy('createdAt', 'desc'));
            onSnapshot(q, (snap) => {
                const list = document.getElementById('orderList');
                list.innerHTML = snap.empty ? '<p class="text-slate-400">Aucune commande.</p>' : '';
                snap.forEach(doc => {
                    const o = doc.data();
                    list.innerHTML += `<div class="bg-white p-6 rounded-3xl border border-slate-100 flex justify-between"><div><b>${o.total.toLocaleString()} FCFA</b><p class="text-[10px] text-slate-400">#${doc.id.slice(0,5)}</p></div><span class="text-gab-green font-bold text-xs uppercase">En cours</span></div>`;
                });
            }, (err) => console.log(err));
        }

        renderProducts();
    </script>
</body>
</html>
