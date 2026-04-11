<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 B2B | Grossistes du Gabon</title>
    
    <!-- PWA & Mobile Optimization -->
    <meta name="theme-color" content="#3a75c4">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Echoppe241">
    <link rel="apple-touch-icon" href="https://i.ibb.co/2Q73j3X/echoppe241-logo.png">
    
    <!-- Manifest pour l'installation style WhatsApp -->
    <link rel="manifest" id="pwa-manifest">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <!-- Bibliothèques PDF -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.23/jspdf.plugin.autotable.min.js"></script>
    
    <style>
        :root { 
            --gab-green: #00853f; 
            --gab-yellow: #fcd116; 
            --gab-blue: #3a75c4; 
        }
        body { 
            font-family: 'Segoe UI', system-ui, sans-serif; 
            background-color: #f1f5f9; 
            -webkit-tap-highlight-color: transparent;
        }
        .bg-gab-green { background-color: var(--gab-green); }
        .bg-gab-yellow { background-color: var(--gab-yellow); }
        .bg-gab-blue { background-color: var(--gab-blue); }
        .text-gab-blue { color: var(--gab-blue); }
        .b2b-card:hover { transform: translateY(-5px); box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1); }
        .custom-scroll::-webkit-scrollbar { width: 4px; }
        .custom-scroll::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        
        .animate-fade-in { animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="antialiased">

    <!-- Navbar Alibaba Style -->
    <header class="bg-white border-b sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 h-20 flex items-center justify-between gap-4">
            <!-- Logo -->
            <div class="flex items-center gap-2 cursor-pointer shrink-0" onclick="showSection('marketplace')">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Echoppe241" class="h-10">
                <div class="hidden md:block">
                    <h1 class="font-black text-xl text-slate-800 tracking-tighter">ECHOPPE<span class="text-gab-blue">241</span></h1>
                    <p class="text-[8px] font-bold text-gab-green uppercase tracking-[0.2em]">B2B & Wholesale</p>
                </div>
            </div>

            <!-- Search : Fonctionnelle en temps réel -->
            <div class="flex-grow max-w-2xl hidden sm:block">
                <div class="flex items-center border-2 border-gab-blue rounded-full overflow-hidden bg-slate-50 focus-within:ring-2 focus-within:ring-blue-200 transition">
                    <input type="text" id="searchInput" oninput="filterProducts(this.value)" placeholder="Rechercher des produits ou des fournisseurs gabonais..." class="flex-grow px-6 py-2 text-sm outline-none bg-transparent">
                    <button class="bg-gab-blue text-white px-6 py-2.5 font-bold">
                        <i class="fa-solid fa-magnifying-glass"></i>
                    </button>
                </div>
            </div>

            <!-- Actions -->
            <div class="flex items-center gap-4">
                <div class="relative cursor-pointer group" onclick="toggleCart()">
                    <i class="fa-solid fa-file-invoice-dollar text-2xl text-slate-600 group-hover:text-gab-blue transition"></i>
                    <span id="cartCount" class="absolute -top-2 -right-2 bg-gab-yellow text-gab-blue text-[10px] font-black w-5 h-5 rounded-full flex items-center justify-center border-2 border-white">0</span>
                </div>
                <button onclick="checkAuthAndShow('profile')" id="userBtn" class="flex items-center gap-2 bg-slate-100 px-3 py-1.5 rounded-full border border-slate-200">
                    <i class="fa-solid fa-circle-user text-xl text-slate-400"></i>
                    <span id="userNameDisplay" class="text-[10px] font-black uppercase text-slate-600 hidden lg:block italic">Connexion...</span>
                </button>
                <button onclick="checkAuthAndShow('publish')" class="bg-gab-green text-white px-4 py-2 rounded-lg text-[10px] font-black uppercase shadow-lg shadow-green-100 hover:scale-105 transition">Vendre</button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-8">
        
        <!-- SECTION: MARKETPLACE -->
        <section id="section-marketplace" class="space-y-8 animate-fade-in">
            <!-- Bannière Dynamique -->
            <div class="bg-gradient-to-br from-gab-blue to-blue-700 rounded-[2.5rem] p-8 md:p-10 text-white relative overflow-hidden shadow-2xl">
                <div class="relative z-10 max-w-xl">
                    <span class="bg-gab-yellow text-gab-blue px-3 py-1 rounded-full text-[10px] font-black uppercase mb-4 inline-block">Production Nationale</span>
                    <h2 class="text-3xl md:text-4xl font-black italic mb-4 leading-tight">COMMENCEZ À VENDRE OU ACHETER SANS ATTENDRE</h2>
                    <p class="text-blue-100 text-sm mb-6">Plateforme autonome : créez votre profil et publiez vos stocks en 2 minutes.</p>
                    <button class="bg-white text-gab-blue px-6 py-3 rounded-xl font-black text-xs uppercase hover:bg-gab-yellow transition shadow-xl">Explorer le catalogue</button>
                </div>
                <i class="fa-solid fa-globe absolute -right-10 -bottom-10 text-[12rem] md:text-[15rem] text-white/10 rotate-12"></i>
            </div>

            <div class="flex items-center justify-between">
                <h3 class="font-black text-lg text-slate-800 uppercase italic border-l-4 border-gab-yellow pl-4">Catalogue des Grossistes</h3>
            </div>

            <div id="productGrid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4 md:gap-6">
                <!-- Les produits sont injectés ici -->
            </div>
            
            <div id="emptySearch" class="hidden text-center py-20">
                <i class="fa-solid fa-box-open text-5xl text-slate-200 mb-4"></i>
                <p class="text-slate-400 font-bold uppercase text-xs">Aucun produit ne correspond à votre recherche</p>
            </div>
        </section>

        <!-- SECTION: PUBLISH -->
        <section id="section-publish" class="hidden max-w-xl mx-auto py-6">
            <div class="bg-white rounded-[2.5rem] shadow-xl border p-8">
                <h2 class="text-2xl font-black text-slate-800 mb-6 uppercase italic">Publier un stock</h2>
                <form id="productForm" class="space-y-4">
                    <input type="text" id="pName" placeholder="Désignation du produit" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none focus:border-gab-blue" required>
                    <div class="grid grid-cols-2 gap-4">
                        <input type="number" id="pPrice" placeholder="Prix HT (FCFA)" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none" required>
                        <input type="text" id="pMoq" placeholder="Minimum (ex: 50 sacs)" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none" required>
                    </div>
                    <label class="block w-full h-48 border-2 border-dashed border-slate-200 rounded-2xl bg-slate-50 hover:bg-slate-100 transition cursor-pointer flex flex-col items-center justify-center overflow-hidden relative">
                        <i id="imgPlaceholderIcon" class="fa-solid fa-camera text-3xl text-slate-300 mb-2"></i>
                        <span id="imgPlaceholderText" class="text-[10px] font-black text-slate-400 uppercase text-center px-4">Prendre une photo de votre stock</span>
                        <img id="previewImg" class="hidden absolute inset-0 w-full h-full object-cover">
                        <input type="file" id="pImgInput" class="hidden" accept="image/*">
                    </label>
                    <button type="submit" class="w-full bg-gab-green text-white py-5 rounded-2xl font-black uppercase shadow-lg shadow-green-100 mt-4">Mettre en ligne</button>
                </form>
            </div>
        </section>

        <!-- SECTION: PROFILE -->
        <section id="section-profile" class="hidden max-w-2xl mx-auto py-6">
            <div class="bg-white rounded-[2.5rem] shadow-xl border p-8">
                <div class="flex items-center justify-between mb-8">
                    <h2 class="text-2xl font-black text-slate-800 uppercase italic">Identité Professionnelle</h2>
                    <button onclick="signOutUser()" class="text-red-500 font-black text-[10px] uppercase">Supprimer session</button>
                </div>
                <p class="text-[10px] text-slate-400 font-bold uppercase mb-6 italic">Ces informations apparaîtront sur vos bons de commande.</p>
                <form id="profileForm" class="space-y-4">
                    <input type="text" id="profName" placeholder="Nom complet ou Entreprise" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none focus:border-gab-blue" required>
                    <input type="tel" id="profPhone" placeholder="WhatsApp (ex: 077736065)" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none" required>
                    <select id="profProvince" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none font-bold">
                        <option value="Estuaire">Estuaire (Libreville)</option>
                        <option value="Ogooué-Maritime">Ogooué-Maritime (POG)</option>
                        <option value="Haut-Ogooué">Haut-Ogooué</option>
                        <option value="Woleu-Ntem">Woleu-Ntem</option>
                        <option value="Ngounié">Ngounié</option>
                        <option value="Nyanga">Nyanga</option>
                        <option value="Ogooué-Ivindo">Ogooué-Ivindo</option>
                        <option value="Ogooué-Lolo">Ogooué-Lolo</option>
                        <option value="Moyen-Ogooué">Moyen-Ogooué</option>
                    </select>
                    <button type="submit" class="w-full bg-gab-blue text-white py-4 rounded-2xl font-black uppercase shadow-lg">Enregistrer mon compte</button>
                </form>
            </div>
        </section>

    </main>

    <!-- SIDEBAR: CART -->
    <div id="cartSidebar" class="fixed inset-y-0 right-0 w-full md:w-[450px] bg-white shadow-2xl z-[100] transform translate-x-full transition-transform duration-300 flex flex-col border-l">
        <div class="p-8 border-b bg-slate-50 flex justify-between items-center">
            <div>
                <h3 class="font-black text-2xl italic text-gab-blue">VOTRE DEVIS</h3>
                <p class="text-[9px] font-bold text-slate-400 uppercase tracking-widest">Générateur Automatique</p>
            </div>
            <button onclick="toggleCart()" class="text-slate-400 text-2xl hover:text-red-500 transition">&times;</button>
        </div>
        
        <div id="cartItems" class="flex-grow overflow-y-auto p-6 md:p-8 space-y-4 custom-scroll">
            <!-- Items du panier -->
        </div>

        <div class="p-8 border-t bg-slate-50 space-y-6">
            <div class="flex justify-between items-end">
                <span class="text-xs font-black text-slate-400 uppercase">Montant Total</span>
                <span id="cartTotal" class="text-3xl font-black text-gab-blue">0 FCFA</span>
            </div>
            <button onclick="generatePDFAndSend()" class="w-full bg-gab-blue text-white py-5 rounded-2xl font-black uppercase tracking-widest flex items-center justify-center gap-3 shadow-xl hover:bg-blue-700 transition-all">
                <i class="fa-solid fa-file-pdf"></i> Envoyer au +241 77 73 60 65
            </button>
        </div>
    </div>

    <!-- NOTIF -->
    <div id="notif" class="fixed bottom-10 left-1/2 -translate-x-1/2 bg-slate-900 text-white px-8 py-4 rounded-2xl text-[10px] font-black uppercase shadow-2xl hidden z-[300]"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, query, onSnapshot, orderBy, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // CONFIGURATION FIREBASE
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
        const appId = "echoppe241-b2b-production";
        const ADMIN_WHATSAPP = "24177736065";

        let currentUser = null;
        let userData = { name: "Utilisateur", phone: "241", province: "Estuaire", setup: false };
        let cart = [];
        let currentImg = "";
        let allProducts = [];

        // AUTHENTIFICATION
        onAuthStateChanged(auth, async (user) => {
            currentUser = user;
            if (user) {
                const snap = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid, 'profile', 'info'));
                if (snap.exists()) {
                    userData = { ...snap.data(), setup: true };
                    document.getElementById('profName').value = userData.name;
                    document.getElementById('profPhone').value = userData.phone;
                    document.getElementById('profProvince').value = userData.province;
                    document.getElementById('userNameDisplay').innerText = userData.name;
                    document.getElementById('userNameDisplay').classList.remove('italic');
                } else {
                    document.getElementById('userNameDisplay').innerText = "Vendeur Invité";
                }
            } else {
                signInAnonymously(auth);
            }
        });

        // MANIFEST PWA DYNAMIQUE
        const manifest = {
            "name": "Echoppe241 B2B",
            "short_name": "Echoppe241",
            "start_url": ".",
            "display": "standalone",
            "background_color": "#ffffff",
            "theme_color": "#3a75c4",
            "icons": [{"src": "https://i.ibb.co/2Q73j3X/echoppe241-logo.png", "sizes": "512x512", "type": "image/png"}]
        };
        const blob = new Blob([JSON.stringify(manifest)], {type: 'application/json'});
        document.getElementById('pwa-manifest').setAttribute('href', URL.createObjectURL(blob));

        // NAVIGATION
        window.showSection = (id) => {
            document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`section-${id}`).classList.remove('hidden');
            window.scrollTo(0,0);
        };
        window.toggleCart = () => document.getElementById('cartSidebar').classList.toggle('translate-x-full');
        window.checkAuthAndShow = (id) => showSection(id);

        // RECHERCHE FILTRÉE
        window.filterProducts = (val) => {
            const query = val.toLowerCase();
            const filtered = allProducts.filter(p => 
                p.name.toLowerCase().includes(query) || 
                p.sellerName.toLowerCase().includes(query) ||
                p.province.toLowerCase().includes(query)
            );
            renderProducts(filtered);
            document.getElementById('emptySearch').classList.toggle('hidden', filtered.length > 0);
        };

        // PROFILS ET PUBLICATIONS
        document.getElementById('profileForm').onsubmit = async (e) => {
            e.preventDefault();
            const newData = {
                name: document.getElementById('profName').value,
                phone: document.getElementById('profPhone').value,
                province: document.getElementById('profProvince').value,
                updatedAt: serverTimestamp()
            };
            await setDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, 'profile', 'info'), newData);
            userData = { ...newData, setup: true };
            notify("Profil mis à jour !");
            document.getElementById('userNameDisplay').innerText = userData.name;
            showSection('marketplace');
        };

        document.getElementById('pImgInput').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (ev) => {
                    currentImg = ev.target.result;
                    document.getElementById('previewImg').src = currentImg;
                    document.getElementById('previewImg').classList.remove('hidden');
                    document.getElementById('imgPlaceholderIcon').classList.add('hidden');
                    document.getElementById('imgPlaceholderText').classList.add('hidden');
                };
                reader.readAsDataURL(file);
            }
        });

        document.getElementById('productForm').onsubmit = async (e) => {
            e.preventDefault();
            if(!userData.setup) {
                notify("Veuillez d'abord remplir votre Profil");
                return showSection('profile');
            }
            if(!currentImg) return notify("Photo obligatoire");
            
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'products'), {
                    name: document.getElementById('pName').value,
                    price: parseInt(document.getElementById('pPrice').value),
                    moq: document.getElementById('pMoq').value,
                    img: currentImg,
                    sellerId: currentUser.uid,
                    sellerName: userData.name,
                    province: userData.province,
                    createdAt: serverTimestamp()
                });
                notify("Produit publié !");
                showSection('marketplace');
                document.getElementById('productForm').reset();
                resetImgPreview();
            } catch (err) { notify("Erreur réseau"); }
        };

        function resetImgPreview() {
            document.getElementById('previewImg').classList.add('hidden');
            document.getElementById('imgPlaceholderIcon').classList.remove('hidden');
            document.getElementById('imgPlaceholderText').classList.remove('hidden');
            currentImg = "";
        }

        // CATALOGUE TEMPS RÉEL
        onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'products'), orderBy('createdAt', 'desc')), (snap) => {
            allProducts = [];
            snap.forEach(d => allProducts.push({ id: d.id, ...d.data() }));
            renderProducts(allProducts);
        });

        function renderProducts(list) {
            const grid = document.getElementById('productGrid');
            grid.innerHTML = list.map(p => `
                <div class="bg-white rounded-[1.8rem] overflow-hidden border border-slate-100 b2b-card transition-all p-3 md:p-4 group animate-fade-in">
                    <div class="h-32 md:h-40 rounded-2xl overflow-hidden mb-3 relative bg-slate-50">
                        <img src="${p.img}" class="w-full h-full object-cover group-hover:scale-110 transition duration-700">
                        <div class="absolute top-2 right-2 bg-gab-yellow text-gab-blue text-[7px] font-black px-2 py-1 rounded-full shadow-lg">${p.province}</div>
                        <div class="absolute bottom-2 left-2 bg-black/70 backdrop-blur-sm text-white text-[7px] md:text-[8px] font-black px-2 py-1 rounded">MOQ: ${p.moq}</div>
                    </div>
                    <h4 class="text-[9px] font-black text-gab-blue uppercase truncate tracking-tighter mb-1">${p.sellerName}</h4>
                    <h3 class="text-[11px] md:text-xs font-bold text-slate-800 uppercase line-clamp-2 h-8 leading-tight mb-2">${p.name}</h3>
                    <div class="flex items-center justify-between">
                        <p class="text-xs md:text-sm font-black text-slate-900">${p.price.toLocaleString()} <span class="text-[8px] text-slate-400 font-bold uppercase">FCFA</span></p>
                        <button onclick="addToCart('${p.id}', '${p.name.replace(/'/g, "\\'")}', ${p.price})" class="bg-gab-blue text-white w-7 h-7 md:w-8 md:h-8 rounded-full flex items-center justify-center shadow-lg active:scale-90 transition">
                            <i class="fa-solid fa-cart-plus text-[10px]"></i>
                        </button>
                    </div>
                </div>
            `).join('');
        }

        // GESTION PANIER
        window.addToCart = (id, name, price) => {
            const existing = cart.find(item => item.id === id);
            if(existing) existing.qty++;
            else cart.push({ id, name, price, qty: 1 });
            updateCartUI();
            notify("Ajouté au panier");
        };

        function updateCartUI() {
            document.getElementById('cartCount').innerText = cart.length;
            let total = 0;
            document.getElementById('cartItems').innerHTML = cart.map((item, idx) => {
                total += item.price * item.qty;
                return `
                    <div class="bg-white p-4 rounded-2xl border border-slate-100 flex items-center justify-between shadow-sm animate-fade-in">
                        <div class="flex-grow">
                            <p class="text-[10px] font-black text-slate-800 uppercase mb-1 truncate max-w-[200px]">${item.name}</p>
                            <div class="flex items-center gap-3">
                                <input type="number" value="${item.qty}" min="1" onchange="updateQty(${idx}, this.value)" class="w-12 text-center text-[10px] font-black bg-slate-100 rounded-lg py-1 border-none focus:ring-1 focus:ring-gab-blue">
                                <span class="text-[9px] font-bold text-slate-400">× ${item.price.toLocaleString()} FCFA</span>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${idx})" class="text-slate-200 hover:text-red-500 px-2 transition"><i class="fa-solid fa-trash-can text-sm"></i></button>
                    </div>
                `;
            }).join('');
            document.getElementById('cartTotal').innerText = total.toLocaleString() + " FCFA";
        }

        window.updateQty = (idx, val) => { cart[idx].qty = Math.max(1, parseInt(val)); updateCartUI(); };
        window.removeFromCart = (idx) => { cart.splice(idx, 1); updateCartUI(); };

        // PDF & WHATSAPP
        window.generatePDFAndSend = () => {
            if(cart.length === 0) return notify("Le panier est vide");
            if(!userData.setup) { notify("Complétez votre Profil"); return showSection('profile'); }

            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            const orderId = "BC-" + Math.random().toString(36).substring(2, 8).toUpperCase();

            // Style Gabonais
            doc.setFillColor(58, 117, 196); doc.rect(0, 0, 210, 40, 'F');
            doc.setTextColor(255, 255, 255); doc.setFontSize(22); doc.setFont("helvetica", "bold");
            doc.text("ECHOPPE 241 - DEVIS B2B", 20, 25);

            doc.setTextColor(60, 60, 60); doc.setFontSize(10); doc.setFont("helvetica", "normal");
            doc.text(`DATE : ${new Date().toLocaleDateString()}`, 20, 50);
            doc.text(`RÉFÉRENCE : ${orderId}`, 20, 57);
            doc.text(`CLIENT : ${userData.name}`, 20, 64);
            doc.text(`PROVINCE : ${userData.province}`, 20, 71);
            doc.text(`TÉLÉPHONE : ${userData.phone}`, 20, 78);

            const tableData = cart.map(i => [i.name, i.qty, i.price.toLocaleString() + " FCFA", (i.qty * i.price).toLocaleString() + " FCFA"]);
            doc.autoTable({
                startY: 85,
                head: [['Désignation du produit', 'Qté', 'PU HT', 'Total HT']],
                body: tableData,
                headStyles: { fillColor: [0, 133, 63] },
                styles: { fontSize: 9, cellPadding: 4 }
            });

            const total = cart.reduce((a,b) => a + (b.price * b.qty), 0);
            doc.setFontSize(14); doc.setFont("helvetica", "bold");
            doc.text(`MONTANT TOTAL HT : ${total.toLocaleString()} FCFA`, 110, doc.lastAutoTable.finalY + 15);

            doc.save(`${orderId}_Echoppe241.pdf`);

            const message = `*DEVIS B2B ECHOPPE241*%0A*Ref:* ${orderId}%0A*Client:* ${userData.name}%0A*Total:* ${total.toLocaleString()} FCFA%0A%0A_Veuillez trouver mon bon de commande ci-joint (PDF)._`;
            window.open(`https://wa.me/${ADMIN_WHATSAPP}?text=${message}`, '_blank');
            
            notify("PDF généré !");
            cart = []; updateCartUI(); toggleCart();
        };

        window.notify = (m) => {
            const el = document.getElementById('notif'); el.innerText = m; el.classList.remove('hidden');
            setTimeout(() => el.classList.add('hidden'), 3000);
        };
        window.signOutUser = () => signOut(auth).then(() => { localStorage.clear(); location.reload(); });

    </script>
</body>
</html>
