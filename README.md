<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 | Le Marché Digital du Gabon</title>
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
        .text-gab-green { color: var(--gab-green); }
        .bg-gab-yellow { background-color: var(--gab-yellow); }
        .bg-gab-blue { background-color: var(--gab-blue); }
        .border-gab-blue { border-color: var(--gab-blue); }

        .chat-window { height: 350px; }
        .message { max-width: 85%; padding: 10px 15px; border-radius: 18px; margin-bottom: 8px; font-size: 13px; line-height: 1.4; }
        .msg-admin { background: #f1f5f9; align-self: flex-start; color: #334155; border-bottom-left-radius: 4px; }
        .msg-user { background: var(--gab-blue); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }

        .cart-badge { top: -5px; right: -5px; }
        
        /* Custom scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
    </style>
</head>
<body class="antialiased">

    <!-- Top Banner (Gabon Flag Colors) -->
    <div class="h-1 flex sticky top-0 z-[60]">
        <div class="flex-1 bg-gab-green"></div>
        <div class="flex-1 bg-gab-yellow"></div>
        <div class="flex-1 bg-gab-blue"></div>
    </div>

    <!-- Navigation -->
    <header class="bg-white/80 backdrop-blur-md shadow-sm sticky top-1 z-50 border-b border-slate-100">
        <div class="max-w-7xl mx-auto px-4 py-2 flex justify-between items-center">
            <!-- Brand with Logo Image -->
            <div class="flex items-center gap-3 cursor-pointer" onclick="showSection('marketplace')">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Echoppe241 Logo" class="h-10 md:h-12 w-auto object-contain">
                <div class="flex flex-col leading-none">
                    <span class="font-black text-lg tracking-tighter text-slate-800">ECHOPPE<span class="text-gab-blue">241</span></span>
                    <span class="text-[8px] font-bold text-gab-green uppercase tracking-widest">Le Marché National</span>
                </div>
            </div>

            <div class="hidden lg:flex items-center gap-8 text-xs font-bold uppercase tracking-wider text-slate-600">
                <button onclick="showSection('marketplace')" class="hover:text-gab-blue transition flex items-center gap-2">
                    <i class="fa-solid fa-house-chimney text-[10px]"></i> Boutique
                </button>
                <button onclick="showSection('publish')" class="hover:text-gab-blue transition flex items-center gap-2">
                    <i class="fa-solid fa-tag text-[10px]"></i> Vendre
                </button>
                <button onclick="showSection('orders')" class="hover:text-gab-blue transition flex items-center gap-2">
                    <i class="fa-solid fa-box text-[10px]"></i> Commandes
                </button>
            </div>

            <div class="flex items-center gap-3">
                <button onclick="toggleCart()" class="relative p-2 text-slate-600 hover:text-gab-blue transition">
                    <i class="fa-solid fa-cart-shopping text-xl"></i>
                    <span id="cartCount" class="absolute cart-badge bg-red-500 text-white text-[9px] font-bold w-5 h-5 flex items-center justify-center rounded-full border-2 border-white">0</span>
                </button>
                <button onclick="toggleAuth()" id="userBtn" class="bg-slate-100 text-slate-700 px-4 py-2 rounded-full text-xs font-bold hover:bg-gab-blue hover:text-white transition">
                    <i class="fa-solid fa-user mr-2"></i>Compte
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto p-4 md:p-8">
        
        <!-- SECTION: MARKETPLACE -->
        <section id="section-marketplace" class="space-y-8 animate-fade-in">
            <!-- Hero Banner -->
            <div class="bg-gradient-to-br from-gab-blue to-blue-800 rounded-[2.5rem] p-8 md:p-12 text-white relative overflow-hidden shadow-2xl">
                <div class="relative z-10 max-w-lg">
                    <span class="bg-gab-yellow text-gab-blue px-3 py-1 rounded-full text-[10px] font-black uppercase mb-4 inline-block">Nouveau : Livraison Express</span>
                    <h1 class="text-4xl md:text-5xl font-black mb-4 leading-tight">Consommez Gabonais, Soutenez Local.</h1>
                    <p class="text-blue-100 mb-6 text-sm md:text-base">Commandez les meilleurs produits de nos 9 provinces et recevez-les chez vous à Libreville, Port-Gentil ou Franceville.</p>
                    <div class="flex gap-4">
                        <button onclick="document.getElementById('productGrid').scrollIntoView({behavior:'smooth'})" class="bg-white text-gab-blue px-6 py-3 rounded-2xl font-black text-sm uppercase shadow-lg hover:bg-gab-yellow transition">Découvrir</button>
                    </div>
                </div>
                <i class="fa-solid fa-bag-shopping absolute -right-10 -bottom-10 text-[200px] text-white/10 rotate-12"></i>
            </div>

            <!-- Categories -->
            <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                <h2 class="text-2xl font-black text-slate-800">Nos pépites locales</h2>
                <div class="flex gap-2 overflow-x-auto pb-2 w-full md:w-auto">
                    <button class="bg-gab-blue text-white px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap">Tout</button>
                    <button class="bg-white border border-slate-200 px-4 py-2 rounded-xl text-xs font-bold hover:border-gab-blue transition whitespace-nowrap">Agro-alimentaire</button>
                    <button class="bg-white border border-slate-200 px-4 py-2 rounded-xl text-xs font-bold hover:border-gab-blue transition whitespace-nowrap">Artisanat</button>
                    <button class="bg-white border border-slate-200 px-4 py-2 rounded-xl text-xs font-bold hover:border-gab-blue transition whitespace-nowrap">Cosmétique</button>
                </div>
            </div>

            <!-- Product Grid -->
            <div id="productGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                <!-- Products injected by JS -->
            </div>
        </section>

        <!-- SECTION: PUBLIER (Vendre) -->
        <section id="section-publish" class="hidden max-w-2xl mx-auto py-10">
            <div class="bg-white rounded-[2.5rem] shadow-xl border border-slate-100 overflow-hidden">
                <div class="bg-gab-green p-8 text-white">
                    <h2 class="text-2xl font-black uppercase">Espace Vendeur</h2>
                    <p class="text-sm opacity-90">Mettez vos produits en ligne en quelques clics.</p>
                </div>
                <form id="productForm" class="p-8 space-y-6">
                    <div>
                        <label class="block text-[10px] font-black text-slate-400 uppercase mb-2">Nom du produit</label>
                        <input type="text" id="pName" placeholder="ex: Atanga de Makokou" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl focus:border-gab-green outline-none transition" required>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-[10px] font-black text-slate-400 uppercase mb-2">Prix (FCFA)</label>
                            <input type="number" id="pPrice" placeholder="5000" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl focus:border-gab-green outline-none transition" required>
                        </div>
                        <div>
                            <label class="block text-[10px] font-black text-slate-400 uppercase mb-2">Province</label>
                            <select id="pProvince" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl focus:border-gab-green outline-none transition">
                                <option>Estuaire</option><option>Woleu-Ntem</option><option>Ogooué-Lolo</option><option>Ngounié</option><option>Nyanga</option><option>Haut-Ogooué</option><option>Ogooué-Maritime</option><option>Moyen-Ogooué</option><option>Ogooué-Ivindo</option>
                            </select>
                        </div>
                    </div>
                    <div>
                        <label class="block text-[10px] font-black text-slate-400 uppercase mb-2">Image du produit (URL)</label>
                        <input type="text" id="pImg" placeholder="https://image.com/..." class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl focus:border-gab-green outline-none transition" required>
                    </div>
                    <button type="submit" class="w-full bg-gab-green text-white py-5 rounded-2xl font-black uppercase tracking-widest hover:shadow-lg hover:scale-[1.02] active:scale-95 transition">Publier l'annonce</button>
                </form>
            </div>
        </section>

        <!-- SECTION: ORDERS (COMMANDES) -->
        <section id="section-orders" class="hidden max-w-4xl mx-auto py-10">
            <h2 class="text-3xl font-black text-slate-800 mb-8">Mes Commandes</h2>
            <div id="orderList" class="space-y-4 text-center py-10">
                <div class="text-slate-400">
                    <i class="fa-solid fa-box-open text-6xl mb-4 opacity-20"></i>
                    <p>Vous n'avez pas encore passé de commande.</p>
                </div>
            </div>
        </section>

    </main>

    <!-- SIDE CART (PANIER) -->
    <div id="cartSidebar" class="fixed inset-y-0 right-0 w-full md:w-96 bg-white shadow-2xl z-[100] transform translate-x-full transition-transform duration-300 flex flex-col border-l border-slate-100">
        <div class="p-6 border-b flex justify-between items-center bg-slate-50">
            <h3 class="font-black text-xl text-slate-800">Votre Panier</h3>
            <button onclick="toggleCart()" class="text-2xl text-slate-400 hover:text-red-500 transition-colors">&times;</button>
        </div>
        <div id="cartItems" class="flex-grow overflow-y-auto p-6 space-y-4">
            <!-- Items injected by JS -->
        </div>
        <div class="p-6 border-t bg-slate-50 space-y-4">
            <div class="flex justify-between font-black text-lg">
                <span>Total :</span>
                <span id="cartTotal" class="text-gab-blue">0 FCFA</span>
            </div>
            <button onclick="startCheckout()" class="w-full bg-gab-blue text-white py-5 rounded-2xl font-black uppercase tracking-widest hover:bg-blue-700 transition shadow-lg">Commander maintenant</button>
        </div>
    </div>

    <!-- CHECKOUT MODAL (PAYMENT) -->
    <div id="checkoutModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-[200] hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-lg rounded-[2.5rem] shadow-2xl overflow-hidden animate-scale-up">
            <div class="p-8">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-black text-slate-800 uppercase italic">Paiement Mobile</h2>
                    <button onclick="closeCheckout()" class="text-2xl text-slate-300 hover:text-red-500">&times;</button>
                </div>
                <div class="space-y-4">
                    <div class="flex gap-4 p-4 bg-slate-50 rounded-2xl border-2 border-slate-100">
                        <label class="flex-1 cursor-pointer">
                            <input type="radio" name="payment" class="hidden peer" checked>
                            <div class="p-4 bg-white rounded-xl border-2 peer-checked:border-gab-blue text-center transition">
                                <img src="https://upload.wikimedia.org/wikipedia/commons/d/de/Airtel_logo.svg" class="h-8 mx-auto mb-2" alt="Airtel">
                                <span class="text-[10px] font-bold">Airtel Money</span>
                            </div>
                        </label>
                        <label class="flex-1 cursor-pointer">
                            <input type="radio" name="payment" class="hidden peer">
                            <div class="p-4 bg-white rounded-xl border-2 peer-checked:border-gab-blue text-center transition">
                                <div class="h-8 flex items-center justify-center font-black text-blue-900 text-sm">MOOV</div>
                                <span class="text-[10px] font-bold">Moov Money</span>
                            </div>
                        </label>
                    </div>
                    <input type="tel" id="checkoutPhone" placeholder="Numéro (ex: 077...)" class="w-full bg-slate-50 border-2 border-slate-100 p-4 rounded-2xl outline-none focus:border-gab-blue text-center font-black text-xl">
                    <button id="confirmPayBtn" onclick="confirmPayment()" class="w-full bg-gab-yellow text-gab-blue py-5 rounded-2xl font-black uppercase tracking-widest hover:scale-105 transition shadow-lg">Confirmer le paiement</button>
                </div>
            </div>
        </div>
    </div>

    <!-- FLOATING CHAT -->
    <button onclick="toggleChat()" class="fixed bottom-6 right-6 bg-gab-blue text-white w-14 h-14 rounded-2xl shadow-2xl flex items-center justify-center z-[90] hover:scale-110 transition">
        <i class="fa-solid fa-comment-dots text-2xl"></i>
    </button>

    <div id="chatBox" class="fixed bottom-24 right-6 w-80 md:w-96 bg-white rounded-3xl shadow-2xl border border-slate-100 z-[100] hidden overflow-hidden flex flex-col">
        <div class="bg-gab-blue p-4 text-white flex justify-between items-center">
            <div class="flex items-center gap-2">
                <div class="w-2 h-2 bg-green-400 rounded-full animate-pulse"></div>
                <span class="font-bold text-sm">Assistance Echoppe241</span>
            </div>
            <button onclick="toggleChat()" class="text-xl">&times;</button>
        </div>
        <div id="chatMessages" class="chat-window p-4 overflow-y-auto flex flex-col bg-slate-50"></div>
        <div class="p-3 border-t flex gap-2 bg-white">
            <input type="text" id="chatInput" placeholder="Posez une question..." class="flex-grow bg-slate-100 border-none rounded-full px-4 py-2 text-sm outline-none focus:bg-slate-200 transition">
            <button onclick="sendChatMessage()" class="bg-gab-blue text-white w-10 h-10 rounded-full flex items-center justify-center hover:scale-105 transition shadow-md">
                <i class="fa-solid fa-paper-plane"></i>
            </button>
        </div>
    </div>

    <!-- Notification system -->
    <div id="notification" class="fixed top-20 left-1/2 transform -translate-x-1/2 z-[300] bg-slate-800 text-white px-6 py-3 rounded-full text-sm font-bold shadow-2xl hidden animate-bounce"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getFirestore, collection, addDoc, query, onSnapshot, serverTimestamp, orderBy, doc, setDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";

        // FIREBASE INITIALIZATION
        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'echoppe241-prod';

        // APPLICATION STATE
        let currentUser = null;
        let cart = [];
        const products = [
            { id: 1, name: "Huile de Palme Rouge", price: 18000, province: "Ogooué-Lolo", img: "https://images.unsplash.com/photo-1620916566398-39f1143f2c0a?auto=format&fit=crop&w=400&q=80" },
            { id: 2, name: "Manioc d'Oyem (Sac 50kg)", price: 25000, province: "Woleu-Ntem", img: "https://images.unsplash.com/photo-1590779033100-9f60705a013d?auto=format&fit=crop&w=400&q=80" },
            { id: 3, name: "Miel Sauvage Naturel", price: 8500, province: "Ogooué-Ivindo", img: "https://images.unsplash.com/photo-1587049352846-4a222e784d38?auto=format&fit=crop&w=400&q=80" },
            { id: 4, name: "Poissons Salés Artisanaux", price: 12000, province: "Estuaire", img: "https://images.unsplash.com/photo-1615141982883-c7ad0e69fd62?auto=format&fit=crop&w=400&q=80" },
            { id: 5, name: "Beurre de Karité Gabon", price: 4500, province: "Ngounié", img: "https://images.unsplash.com/photo-1608248597279-f99d160bfcbc?auto=format&fit=crop&w=400&q=80" },
            { id: 6, name: "Piment Sec Broyé", price: 2500, province: "Moyen-Ogooué", img: "https://images.unsplash.com/photo-1588252303782-cb80119abd6d?auto=format&fit=crop&w=400&q=80" }
        ];

        // AUTH & LISTENERS INIT
        const init = async () => {
            if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                await signInWithCustomToken(auth, __initial_auth_token);
            } else {
                await signInAnonymously(auth);
            }
            onAuthStateChanged(auth, (user) => {
                currentUser = user;
                if (user) {
                    loadOrders();
                    loadMessages();
                    console.log("Connecté en tant que :", user.uid);
                }
            });
        };
        init();

        // NAVIGATION LOGIC
        window.showSection = (id) => {
            document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`section-${id}`).classList.remove('hidden');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };

        // CART LOGIC
        window.toggleCart = () => {
            document.getElementById('cartSidebar').classList.toggle('translate-x-full');
        };

        window.addToCart = (id) => {
            const prod = products.find(p => p.id === id);
            cart.push(prod);
            updateCartUI();
            notify(`${prod.name} ajouté au panier !`);
        };

        window.removeFromCart = (index) => {
            cart.splice(index, 1);
            updateCartUI();
        };

        function updateCartUI() {
            const container = document.getElementById('cartItems');
            const totalEl = document.getElementById('cartTotal');
            const countEl = document.getElementById('cartCount');
            
            countEl.innerText = cart.length;
            
            if (cart.length === 0) {
                container.innerHTML = `<div class="text-center text-slate-400 py-10"><i class="fa-solid fa-basket-shopping text-4xl mb-2 opacity-20"></i><p>Panier vide</p></div>`;
                totalEl.innerText = `0 FCFA`;
                return;
            }

            let total = 0;
            container.innerHTML = cart.map((item, idx) => {
                total += item.price;
                return `
                    <div class="flex items-center gap-4 bg-slate-50 p-3 rounded-2xl border border-slate-100">
                        <img src="${item.img}" class="w-16 h-16 object-cover rounded-xl shadow-sm">
                        <div class="flex-grow">
                            <h4 class="font-bold text-sm text-slate-800">${item.name}</h4>
                            <p class="text-gab-blue font-black text-xs">${item.price.toLocaleString()} FCFA</p>
                        </div>
                        <button onclick="removeFromCart(${idx})" class="text-slate-300 hover:text-red-500 p-2 transition-colors"><i class="fa-solid fa-trash-can"></i></button>
                    </div>
                `;
            }).join('');
            totalEl.innerText = `${total.toLocaleString()} FCFA`;
        }

        // CHECKOUT & PAYMENT
        window.startCheckout = () => {
            if (cart.length === 0) return notify("Votre panier est vide !");
            document.getElementById('checkoutModal').classList.remove('hidden');
        };

        window.closeCheckout = () => {
            document.getElementById('checkoutModal').classList.add('hidden');
        };

        window.confirmPayment = async () => {
            const phone = document.getElementById('checkoutPhone').value;
            if (!phone || phone.length < 8) return notify("Veuillez saisir un numéro valide");

            const btn = document.getElementById('confirmPayBtn');
            btn.disabled = true;
            btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin mr-2"></i> Traitement...`;

            // Simuler délai réseau / validation Mobile Money
            setTimeout(async () => {
                try {
                    const orderData = {
                        items: cart,
                        total: cart.reduce((acc, curr) => acc + curr.price, 0),
                        phone: phone,
                        status: 'payé',
                        uid: currentUser.uid,
                        createdAt: serverTimestamp()
                    };

                    await addDoc(collection(db, 'artifacts', appId, 'users', currentUser.uid, 'orders'), orderData);
                    
                    cart = [];
                    updateCartUI();
                    closeCheckout();
                    showSection('orders');
                    notify("Paiement réussi ! Votre commande est en route.");
                    
                    btn.disabled = false;
                    btn.innerText = "Confirmer le paiement";
                } catch (e) {
                    console.error(e);
                    notify("Erreur lors de l'enregistrement de la commande.");
                }
            }, 2000);
        };

        // DATABASE LOADERS
        function loadOrders() {
            if (!currentUser) return;
            const q = query(collection(db, 'artifacts', appId, 'users', currentUser.uid, 'orders'), orderBy('createdAt', 'desc'));
            onSnapshot(q, (snapshot) => {
                const list = document.getElementById('orderList');
                if (snapshot.empty) return;
                
                list.innerHTML = '';
                snapshot.forEach(doc => {
                    const order = doc.data();
                    const date = order.createdAt?.toDate().toLocaleDateString('fr-FR') || 'Aujourd\'hui';
                    const div = document.createElement('div');
                    div.className = "bg-white p-6 rounded-3xl shadow-sm border border-slate-100 flex flex-col md:flex-row justify-between items-center gap-4 text-left hover:shadow-md transition";
                    div.innerHTML = `
                        <div class="flex items-center gap-4">
                            <div class="bg-gab-blue/10 w-12 h-12 rounded-2xl flex items-center justify-center text-gab-blue">
                                <i class="fa-solid fa-box-archive"></i>
                            </div>
                            <div>
                                <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest">Référence #${doc.id.slice(0,6).toUpperCase()}</p>
                                <h4 class="font-bold text-slate-800">${order.items.length} produit(s) - ${order.total.toLocaleString()} FCFA</h4>
                                <p class="text-xs text-slate-500">Passée le ${date}</p>
                            </div>
                        </div>
                        <div class="flex items-center gap-3 w-full md:w-auto">
                            <span class="bg-green-100 text-green-600 px-4 py-1.5 rounded-full text-[10px] font-black uppercase text-center flex-grow md:flex-grow-0">Payé / Prêt</span>
                            <button class="bg-slate-100 p-3 rounded-xl text-slate-600 hover:bg-gab-blue hover:text-white transition shadow-sm"><i class="fa-solid fa-location-crosshairs"></i></button>
                        </div>
                    `;
                    list.appendChild(div);
                });
            }, (err) => console.error("Erreur chargement commandes:", err));
        }

        // CHAT LOGIC
        window.toggleChat = () => document.getElementById('chatBox').classList.toggle('hidden');
        
        window.sendChatMessage = async () => {
            const input = document.getElementById('chatInput');
            if (!input.value.trim() || !currentUser) return;
            
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'messages'), {
                text: input.value,
                uid: currentUser.uid,
                createdAt: serverTimestamp()
            });
            input.value = '';
        };

        function loadMessages() {
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'messages'), orderBy('createdAt', 'asc'));
            onSnapshot(q, (snapshot) => {
                const container = document.getElementById('chatMessages');
                container.innerHTML = '';
                snapshot.forEach(doc => {
                    const m = doc.data();
                    const div = document.createElement('div');
                    div.className = `message ${m.uid === currentUser.uid ? 'msg-user' : 'msg-admin'}`;
                    div.innerText = m.text;
                    container.appendChild(div);
                });
                container.scrollTop = container.scrollHeight;
            }, (err) => console.error("Erreur chargement chat:", err));
        }

        // UTILS
        function notify(msg) {
            const el = document.getElementById('notification');
            el.innerText = msg;
            el.classList.remove('hidden');
            setTimeout(() => el.classList.add('hidden'), 3500);
        }

        // VENDRE UN PRODUIT
        document.getElementById('productForm').onsubmit = async (e) => {
            e.preventDefault();
            const btn = e.target.querySelector('button');
            btn.disabled = true;
            btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin mr-2"></i> Publication...`;

            const newProd = {
                name: document.getElementById('pName').value,
                price: parseInt(document.getElementById('pPrice').value),
                province: document.getElementById('pProvince').value,
                img: document.getElementById('pImg').value,
                uid: currentUser.uid,
                createdAt: serverTimestamp()
            };

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), newProd);
                notify("Annonce publiée ! Elle apparaîtra bientôt dans la boutique.");
                e.target.reset();
                showSection('marketplace');
            } catch (e) {
                notify("Erreur lors de la publication.");
            } finally {
                btn.disabled = false;
                btn.innerText = "Publier l'annonce";
            }
        };

        // RENDER PRODUCTS
        function renderProducts() {
            const grid = document.getElementById('productGrid');
            grid.innerHTML = products.map(p => `
                <div class="bg-white rounded-[2.5rem] overflow-hidden shadow-sm border border-slate-100 hover:shadow-2xl transition-all duration-300 group">
                    <div class="h-56 overflow-hidden relative">
                        <img src="${p.img}" class="w-full h-full object-cover group-hover:scale-110 transition duration-700">
                        <div class="absolute top-4 left-4 bg-gab-yellow text-gab-blue text-[9px] font-black px-3 py-1 rounded-full uppercase shadow-lg">
                            ${p.province}
                        </div>
                    </div>
                    <div class="p-6">
                        <h3 class="font-bold text-slate-800 mb-1 h-12 overflow-hidden">${p.name}</h3>
                        <p class="text-gab-blue font-black text-xl mb-4">${p.price.toLocaleString()} <span class="text-xs uppercase">FCFA</span></p>
                        <div class="flex gap-2">
                            <button onclick="addToCart(${p.id})" class="flex-grow bg-gab-blue text-white py-3 rounded-xl text-[10px] font-black uppercase hover:bg-blue-700 transition active:scale-95 shadow-md">Ajouter au panier</button>
                            <button class="bg-slate-100 text-slate-400 p-3 rounded-xl hover:text-red-500 transition"><i class="fa-solid fa-heart"></i></button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        window.onload = () => {
            renderProducts();
            // Listener pour les produits ajoutés par les utilisateurs (temps réel)
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                if (snap.empty) return;
                // Logique pour fusionner les produits dynamiques ici si besoin
            });
        };

        window.toggleAuth = () => notify("Vous êtes actuellement connecté en mode invité.");

    </script>
</body>
</html>
