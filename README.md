<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 | Le B2B Made in Gabon</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --gab-green: #00853f;
            --gab-yellow: #fcd116;
            --gab-blue: #3a75c4;
        }
        body { font-family: 'Inter', 'Segoe UI', sans-serif; background-color: #f8fafc; scroll-behavior: smooth; }
        
        .bg-gab-green { background-color: var(--gab-green); }
        .text-gab-green { color: var(--gab-green); }
        .bg-gab-blue { background-color: var(--gab-blue); }
        .text-gab-blue { color: var(--gab-blue); }
        .bg-gab-yellow { background-color: var(--gab-yellow); }
        
        .brand-gradient { 
            background: linear-gradient(135deg, var(--gab-blue) 0%, #1e40af 100%); 
        }

        .product-card { transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); }
        .product-card:hover { transform: translateY(-4px); box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1); outline: 2px solid var(--gab-blue); }
        
        .cart-drawer, .auth-modal {
            transform: translateX(100%);
            transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .cart-drawer.open, .auth-modal.open { transform: translateX(0); }

        .btn-gab {
            background-color: var(--gab-blue);
            color: white;
            transition: all 0.3s;
        }
        .btn-gab:hover {
            background-color: #1e40af;
            box-shadow: 0 4px 12px rgba(58, 117, 196, 0.3);
        }

        .loader {
            border: 2px solid #f3f3f3;
            border-top: 2px solid var(--gab-blue);
            border-radius: 50%;
            width: 16px;
            height: 16px;
            animation: spin 1s linear infinite;
            display: inline-block;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: var(--gab-blue); border-radius: 10px; }
    </style>
</head>
<body class="antialiased text-slate-900">

    <!-- Top Notification Bar -->
    <div class="bg-slate-900 text-white text-[11px] py-2">
        <div class="max-w-7xl mx-auto px-4 flex justify-between items-center">
            <div class="flex space-x-6">
                <span onclick="toggleAuth('register')" class="hover:text-gab-yellow cursor-pointer transition font-medium">Devenir Vendeur Certifié</span>
                <span class="hover:text-gab-yellow cursor-pointer transition">Centre d'aide</span>
            </div>
            <div class="flex items-center space-x-4">
                <span class="hidden sm:inline"><i class="fa-solid fa-truck-fast text-gab-yellow mr-1"></i> Livraison Express 9 Provinces</span>
                <span class="font-bold flex items-center gap-1">
                    <img src="https://flagcdn.com/w20/ga.png" class="w-4 h-3 rounded-sm" alt="Gabon"> FCFA (XAF)
                </span>
            </div>
        </div>
    </div>

    <!-- Main Navigation -->
    <header class="bg-white sticky top-0 z-[60] shadow-sm border-b">
        <div class="max-w-7xl mx-auto px-4 py-4 flex items-center gap-4 lg:gap-12">
            <div class="flex-shrink-0 cursor-pointer" onclick="window.location.reload()">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Echoppe241 Logo" class="h-10 lg:h-14">
            </div>

            <div class="flex-grow max-w-2xl relative">
                <div class="flex border-2 border-gab-blue rounded-full overflow-hidden shadow-sm bg-slate-50 focus-within:ring-2 focus-within:ring-blue-100 transition-all">
                    <input type="text" id="mainSearch" placeholder="Rechercher manioc, huile de palme, produits frais..." class="w-full px-6 py-2.5 outline-none text-sm bg-transparent">
                    <button class="bg-gab-blue text-white px-6 hover:bg-blue-700 transition">
                        <i class="fa-solid fa-magnifying-glass"></i>
                    </button>
                </div>
            </div>

            <div class="flex items-center space-x-4 lg:space-x-8 text-slate-600">
                <div onclick="toggleAuth('login')" class="cursor-pointer text-center group flex flex-col items-center">
                    <div class="h-10 w-10 rounded-full bg-slate-100 flex items-center justify-center group-hover:bg-blue-50 transition">
                        <i class="fa-solid fa-user-tie text-lg group-hover:text-gab-blue transition"></i>
                    </div>
                    <p class="text-[9px] font-bold uppercase mt-1 hidden sm:block">Compte Pro</p>
                </div>
                <div onclick="toggleCart()" class="cursor-pointer text-center relative group flex flex-col items-center">
                    <div class="h-10 w-10 rounded-full bg-slate-100 flex items-center justify-center group-hover:bg-blue-50 transition">
                        <i class="fa-solid fa-cart-shopping text-lg group-hover:text-gab-blue transition"></i>
                        <span id="cartBadge" class="absolute top-0 right-0 bg-gab-green text-white text-[9px] rounded-full h-4 w-4 flex items-center justify-center font-bold ring-2 ring-white">0</span>
                    </div>
                    <p class="text-[9px] font-bold uppercase mt-1 hidden sm:block">Panier</p>
                </div>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-8">
        <!-- Hero Section -->
        <div class="flex flex-col lg:flex-row gap-8 mb-16">
            <aside class="hidden lg:block w-64 flex-shrink-0 bg-white rounded-2xl shadow-sm border border-slate-100 py-4 overflow-hidden">
                <h3 class="px-6 pb-4 mb-2 border-b font-black text-[10px] uppercase tracking-widest text-slate-400">Rayons 241</h3>
                <nav class="space-y-1">
                    <a href="#" class="flex items-center px-6 py-3 text-sm font-medium hover:bg-blue-50 hover:text-gab-blue transition"><i class="fa-solid fa-wheat-awn w-8 text-gab-green"></i> Agriculture</a>
                    <a href="#" class="flex items-center px-6 py-3 text-sm font-medium hover:bg-blue-50 hover:text-gab-blue transition"><i class="fa-solid fa-bottle-droplet w-8 text-gab-blue"></i> Transformation</a>
                    <a href="#" class="flex items-center px-6 py-3 text-sm font-medium hover:bg-blue-50 hover:text-gab-blue transition"><i class="fa-solid fa-spa w-8 text-gab-green"></i> Cosmétique</a>
                    <a href="#" class="flex items-center px-6 py-3 text-sm font-medium hover:bg-blue-50 hover:text-gab-blue transition"><i class="fa-solid fa-palette w-8 text-gab-yellow"></i> Artisanat</a>
                    <a href="#" class="flex items-center px-6 py-3 text-sm font-medium hover:bg-blue-50 hover:text-gab-blue transition"><i class="fa-solid fa-fish w-8 text-gab-blue"></i> Produits de Mer</a>
                </nav>
            </aside>

            <div class="flex-grow h-[450px] brand-gradient rounded-[2rem] p-8 lg:p-16 text-white relative overflow-hidden shadow-2xl flex items-center">
                <div class="relative z-10 max-w-xl">
                    <span class="bg-gab-yellow/20 text-gab-yellow border border-gab-yellow/30 px-4 py-1.5 rounded-full text-[10px] font-black uppercase tracking-wider mb-8 inline-block">Ouverture Officielle Demain</span>
                    <h2 class="text-5xl lg:text-7xl font-black leading-[0.9] mb-6 tracking-tighter">
                        La Boutique <br><span class="text-gab-yellow italic">du Terroir.</span>
                    </h2>
                    <p class="text-lg text-blue-100/80 mb-10 leading-relaxed font-medium">Connectez votre business aux meilleurs producteurs locaux. Transparence totale, prix usine, livraison sécurisée.</p>
                    <button class="bg-white text-gab-blue px-12 py-4 rounded-2xl font-black hover:scale-105 transition-all shadow-xl uppercase text-sm tracking-widest">Démarrer vos achats</button>
                </div>
                <div class="absolute -right-16 -bottom-16 opacity-5 text-[400px] font-black italic rotate-12 select-none">241</div>
            </div>
        </div>

        <!-- Trust Badges -->
        <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-16">
            <div class="bg-white p-6 rounded-2xl border border-slate-100 flex items-center gap-4">
                <i class="fa-solid fa-check-double text-2xl text-gab-green"></i>
                <span class="text-xs font-bold text-slate-700">Qualité Labellisée</span>
            </div>
            <div class="bg-white p-6 rounded-2xl border border-slate-100 flex items-center gap-4">
                <i class="fa-solid fa-handshake-angle text-2xl text-gab-yellow"></i>
                <span class="text-xs font-bold text-slate-700">Circuit Court</span>
            </div>
            <div class="bg-white p-6 rounded-2xl border border-slate-100 flex items-center gap-4">
                <i class="fa-solid fa-truck-ramp-box text-2xl text-gab-blue"></i>
                <span class="text-xs font-bold text-slate-700">Suivi Logistique</span>
            </div>
            <div class="bg-white p-6 rounded-2xl border border-slate-100 flex items-center gap-4">
                <i class="fa-solid fa-headset text-2xl text-slate-400"></i>
                <span class="text-xs font-bold text-slate-700">Support 24/7</span>
            </div>
        </div>

        <!-- Product Grid Header -->
        <div class="flex items-center justify-between mb-8 border-b pb-6">
            <h3 class="text-2xl font-black text-slate-800 uppercase tracking-tighter">Nos Offres de Lancement</h3>
            <div class="flex gap-2">
                <button class="px-4 py-2 text-xs font-bold border-2 border-slate-200 rounded-xl hover:bg-slate-50 transition">Filtrer par Province</button>
            </div>
        </div>
        
        <div id="productGrid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-6 mb-20">
            <!-- Injecté par JavaScript -->
        </div>
    </main>

    <!-- Footer Simple -->
    <footer class="bg-slate-900 text-slate-400 py-12 px-4">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-8">
            <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Echoppe241 Logo" class="h-10 brightness-0 invert opacity-50">
            <div class="flex gap-8 text-xs font-medium">
                <a href="#" class="hover:text-white transition">CGV</a>
                <a href="#" class="hover:text-white transition">Politique de Confidentialité</a>
                <a href="#" class="hover:text-white transition">Mentions Légales</a>
            </div>
            <p class="text-[10px]">&copy; 2024 Echoppe241. Tous droits réservés.</p>
        </div>
    </footer>

    <!-- Auth Modal (Logiciel) -->
    <div id="authModal" class="fixed inset-y-0 right-0 w-full max-w-md bg-white shadow-2xl z-[100] auth-modal flex flex-col border-l">
        <div class="p-6 border-b flex justify-between items-center bg-slate-50">
            <h2 id="modalTitle" class="font-black uppercase text-xs tracking-widest text-slate-500">Portail Echoppe241 Pro</h2>
            <button onclick="toggleAuth()" class="text-3xl text-slate-300 hover:text-red-500 transition-colors">&times;</button>
        </div>
        
        <div class="p-8 flex-grow overflow-y-auto">
            <div class="flex gap-6 mb-10 border-b">
                <button onclick="switchAuthTab('login')" id="loginTab" class="pb-3 border-b-2 border-gab-blue text-gab-blue font-black text-xs uppercase tracking-widest">Connexion</button>
                <button onclick="switchAuthTab('register')" id="registerTab" class="pb-3 border-b-2 border-transparent text-slate-400 font-black text-xs uppercase tracking-widest">Créer un compte</button>
            </div>

            <!-- Login Form -->
            <form id="loginForm" class="space-y-6">
                <div>
                    <label class="block text-[10px] font-black uppercase text-slate-400 mb-2">Identifiant (Email ou Tel)</label>
                    <input type="text" placeholder="ex: contact@entreprise.ga" class="w-full border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue transition-colors bg-slate-50" required>
                </div>
                <div>
                    <label class="block text-[10px] font-black uppercase text-slate-400 mb-2">Mot de passe</label>
                    <input type="password" placeholder="••••••••" class="w-full border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue transition-colors bg-slate-50" required>
                </div>
                <button type="submit" class="w-full btn-gab py-5 rounded-2xl font-black uppercase tracking-widest text-xs shadow-lg">Lancer la session</button>
            </form>

            <!-- Register Form -->
            <form id="registerForm" class="hidden space-y-5">
                <div>
                    <label class="block text-[10px] font-black uppercase text-slate-400 mb-1">Nom du Business / Responsable</label>
                    <input type="text" id="reg_name" placeholder="E-Commerce Gabon SARL" class="w-full border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue transition-colors bg-slate-50" required>
                </div>
                <div>
                    <label class="block text-[10px] font-black uppercase text-slate-400 mb-1">Numéro WhatsApp</label>
                    <div class="flex gap-2">
                        <span class="bg-slate-200 p-4 rounded-2xl text-xs font-bold">+241</span>
                        <input type="tel" id="reg_phone" placeholder="077000000" class="flex-grow border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue transition-colors bg-slate-50" required pattern="[0-9]{8,9}">
                    </div>
                </div>
                <div>
                    <label class="block text-[10px] font-black uppercase text-slate-400 mb-1">Activité principale</label>
                    <select id="reg_type" class="w-full border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue transition-colors bg-slate-50">
                        <option>Revendeur (Boutique)</option>
                        <option>CHR (Hôtel/Restau)</option>
                        <option>Producteur Locaux</option>
                        <option>Acheteur Particulier</option>
                    </select>
                </div>
                <button type="submit" id="reg_btn" class="w-full bg-gab-green text-white py-5 rounded-2xl font-black uppercase tracking-widest text-xs hover:bg-green-700 transition shadow-lg">Envoyer ma demande</button>
                <p class="text-[10px] text-center text-slate-400 italic">Validation de compte sous 24h après vérification par nos agents.</p>
            </form>
        </div>
    </div>

    <!-- Cart Drawer -->
    <div id="cartDrawer" class="fixed inset-y-0 right-0 w-full max-w-sm bg-white shadow-2xl z-[100] cart-drawer flex flex-col border-l">
        <div class="p-6 bg-gab-blue text-white flex justify-between items-center">
            <h2 class="font-black uppercase text-xs tracking-widest">Détails Panier</h2>
            <button onclick="toggleCart()" class="text-3xl">&times;</button>
        </div>
        <div id="cartItems" class="flex-grow p-6 overflow-y-auto space-y-4">
            <!-- Items injectés -->
        </div>
        <div class="p-8 border-t bg-slate-50">
            <div class="flex justify-between font-black text-2xl mb-8">
                <span class="text-slate-400 text-[10px] font-bold uppercase self-center">TOTAL</span>
                <span id="cartTotal" class="text-gab-blue">0 FCFA</span>
            </div>
            <button onclick="finalizeOrder()" class="w-full btn-gab py-5 rounded-2xl font-black uppercase tracking-widest text-xs shadow-xl active:scale-95 transition-transform">Valider & Payer</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, collection, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
        import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

        // Production Config
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            projectId: "communautedugabon",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        const appId = "echoppe241-prod-v1";

        const initAuth = async () => {
            try { await signInAnonymously(auth); } catch(e) { console.error("Auth failed"); }
        };
        initAuth();

        // Register Logic with Retry
        document.getElementById('registerForm').onsubmit = async (e) => {
            e.preventDefault();
            const btn = document.getElementById('reg_btn');
            const originalText = btn.innerText;
            
            btn.disabled = true;
            btn.innerHTML = `<div class="loader mr-2"></div> ENVOI...`;

            const payload = {
                name: document.getElementById('reg_name').value,
                phone: document.getElementById('reg_phone').value,
                type: document.getElementById('reg_type').value,
                timestamp: serverTimestamp()
            };

            const submit = async (retries = 3) => {
                try {
                    await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'registrations'), payload);
                    alert("Demande reçue ! Un conseiller vous contactera pour valider votre accès.");
                    document.getElementById('registerForm').reset();
                    toggleAuth();
                } catch (err) {
                    if (retries > 0) return await submit(retries - 1);
                    alert("Délai de connexion dépassé. Veuillez vérifier votre accès internet.");
                } finally {
                    btn.disabled = false;
                    btn.innerText = originalText;
                }
            };
            submit();
        };
    </script>

    <script>
        const products = [
            { id: 101, name: "Huile de Palme Rouge Premium", vendor: "E-Boutique Makokou", price: 18500, rating: 4.8, sold: "540+ livrés", unit: "Bidon 20L", img: "https://images.unsplash.com/photo-1620916566398-39f1143f2c0a?auto=format&fit=crop&w=400&q=80" },
            { id: 102, name: "Manioc Doux d'Oyem (Sélection)", vendor: "Coopérative G2", price: 12500, rating: 4.9, sold: "2.1t", unit: "Sac 50kg", img: "https://images.unsplash.com/photo-1590779033100-9f60705a013d?auto=format&fit=crop&w=400&q=80" },
            { id: 103, name: "Odika du Woleu-Ntem", vendor: "Saveurs d'Oyem", price: 7500, rating: 4.7, sold: "120 packs", unit: "Lot de 5", img: "https://images.unsplash.com/photo-1596040033229-a9821ebd058d?auto=format&fit=crop&w=400&q=80" },
            { id: 104, name: "Savon Artisanal Moabi & Miel", vendor: "BioGabon", price: 15000, rating: 5.0, sold: "800+ unités", unit: "Carton de 20", img: "https://images.unsplash.com/photo-1605264964528-06403738d6dc?auto=format&fit=crop&w=400&q=80" },
            { id: 105, name: "Piment de Mouila (Séchage Solaire)", vendor: "Mouila Market", price: 5000, rating: 4.5, sold: "45 sacs", unit: "Sachet 1kg", img: "https://images.unsplash.com/photo-1588252303782-cb80119abd6d?auto=format&fit=crop&w=400&q=80" },
            { id: 106, name: "Crevettes de Port-Gentil (Séchées)", vendor: "Pêcheries Ogooué", price: 9500, rating: 4.6, sold: "88 commandes", unit: "Sachet 500g", img: "https://images.unsplash.com/photo-1533682805518-48d1f5b8cd3a?auto=format&fit=crop&w=400&q=80" }
        ];

        let cart = [];

        function render() {
            document.getElementById('productGrid').innerHTML = products.map(p => `
                <div class="product-card bg-white rounded-[1.5rem] overflow-hidden shadow-sm border border-slate-100 flex flex-col h-full group">
                    <div class="relative overflow-hidden h-48">
                        <img src="${p.img}" class="h-full w-full object-cover group-hover:scale-110 transition-transform duration-500">
                        <div class="absolute top-3 left-3 bg-gab-yellow text-slate-900 px-2.5 py-1 rounded-lg text-[9px] font-black uppercase shadow-sm">Certifié 241</div>
                    </div>
                    <div class="p-5 flex flex-col flex-grow">
                        <p class="text-[9px] text-slate-400 font-bold uppercase tracking-widest mb-1">${p.vendor}</p>
                        <h4 class="text-sm font-bold text-slate-800 line-clamp-2 leading-tight mb-3 h-10">${p.name}</h4>
                        <div class="mt-auto">
                            <div class="flex items-baseline gap-1 mb-3">
                                <span class="text-xl font-black text-gab-blue">${p.price.toLocaleString()}</span>
                                <span class="text-[10px] font-bold text-slate-400 uppercase">FCFA / ${p.unit}</span>
                            </div>
                            <div class="flex items-center text-[10px] text-slate-400 mb-4 bg-slate-50 p-2 rounded-lg">
                                <i class="fa-solid fa-star text-gab-yellow mr-1"></i> <span class="font-bold text-slate-600">${p.rating}</span> <span class="mx-2 text-slate-200">|</span> ${p.sold}
                            </div>
                            <button onclick="addToCart(${p.id})" class="w-full btn-gab py-3 rounded-xl text-xs font-black uppercase tracking-tighter active:scale-95 transition-all">Acheter</button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function toggleCart() { document.getElementById('cartDrawer').classList.toggle('open'); }
        function toggleAuth(tab = 'login') { 
            document.getElementById('authModal').classList.toggle('open');
            if(tab) switchAuthTab(tab);
        }

        function switchAuthTab(tab) {
            const lTab = document.getElementById('loginTab');
            const rTab = document.getElementById('registerTab');
            const lForm = document.getElementById('loginForm');
            const rForm = document.getElementById('registerForm');

            if(tab === 'login') {
                lTab.classList.add('border-gab-blue', 'text-gab-blue'); lTab.classList.remove('border-transparent', 'text-slate-400');
                rTab.classList.remove('border-gab-blue', 'text-gab-blue'); rTab.classList.add('border-transparent', 'text-slate-400');
                lForm.classList.remove('hidden'); rForm.classList.add('hidden');
            } else {
                rTab.classList.add('border-gab-blue', 'text-gab-blue'); rTab.classList.remove('border-transparent', 'text-slate-400');
                lTab.classList.remove('border-gab-blue', 'text-gab-blue'); lTab.classList.add('border-transparent', 'text-slate-400');
                rForm.classList.remove('hidden'); lForm.classList.add('hidden');
            }
        }

        function addToCart(id) {
            const p = products.find(x => x.id === id);
            cart.push({...p, cartId: Date.now()});
            updateCart();
            if(!document.getElementById('cartDrawer').classList.contains('open')) toggleCart();
        }

        function updateCart() {
            document.getElementById('cartBadge').innerText = cart.length;
            const total = cart.reduce((a, b) => a + b.price, 0);
            document.getElementById('cartTotal').innerText = `${total.toLocaleString()} FCFA`;
            
            const container = document.getElementById('cartItems');
            if(cart.length === 0) {
                container.innerHTML = `
                    <div class="flex flex-col items-center justify-center py-20 text-center">
                        <div class="h-20 w-20 bg-slate-50 rounded-full flex items-center justify-center mb-4">
                            <i class="fa-solid fa-basket-shopping text-3xl text-slate-200"></i>
                        </div>
                        <p class="text-slate-400 text-sm font-medium">Votre panier est vide.</p>
                    </div>`;
            } else {
                container.innerHTML = cart.map((i, index) => `
                    <div class="flex justify-between items-center p-4 bg-white rounded-2xl border border-slate-100 shadow-sm animate-in fade-in slide-in-from-right-4">
                        <div class="flex items-center gap-4">
                            <img src="${i.img}" class="h-12 w-12 rounded-xl object-cover ring-1 ring-slate-100">
                            <div>
                                <p class="font-bold text-slate-800 text-xs line-clamp-1">${i.name}</p>
                                <p class="text-gab-blue font-black text-sm">${i.price.toLocaleString()} FCFA</p>
                            </div>
                        </div>
                        <button onclick="remove(${index})" class="h-8 w-8 flex items-center justify-center text-slate-300 hover:text-red-500 hover:bg-red-50 rounded-full transition">&times;</button>
                    </div>
                `).join('');
            }
        }

        function remove(idx) { cart.splice(idx, 1); updateCart(); }

        function finalizeOrder() {
            if(cart.length === 0) return;
            const btn = event.target;
            btn.disabled = true;
            btn.innerHTML = `<div class="loader mr-2"></div> TRAITEMENT...`;

            setTimeout(() => {
                let msg = "*COMMANDE ECHOPPE241*\n--------------------------\n";
                cart.forEach(i => msg += `• ${i.name} (${i.price.toLocaleString()} FCFA)\n`);
                msg += `--------------------------\n*TOTAL: ${cart.reduce((a,b)=>a+b.price,0).toLocaleString()} FCFA*`;
                window.open(`https://wa.me/241077736065?text=${encodeURIComponent(msg)}`);
                btn.disabled = false;
                btn.innerText = "Valider & Payer";
            }, 800);
        }

        window.onload = render;
    </script>
</body>
</html>
