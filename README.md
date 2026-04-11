<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Echoppe241 B2B | Grossistes du Gabon</title>
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
        body { font-family: 'Segoe UI', system-ui, sans-serif; background-color: #f1f5f9; }
        .bg-gab-green { background-color: var(--gab-green); }
        .bg-gab-yellow { background-color: var(--gab-yellow); }
        .bg-gab-blue { background-color: var(--gab-blue); }
        .text-gab-blue { color: var(--gab-blue); }
        .b2b-card:hover { transform: translateY(-5px); box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1); }
        .custom-scroll::-webkit-scrollbar { width: 4px; }
        .custom-scroll::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
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

            <!-- Search -->
            <div class="flex-grow max-w-2xl hidden sm:block">
                <div class="flex items-center border-2 border-gab-blue rounded-full overflow-hidden bg-slate-50">
                    <input type="text" placeholder="Rechercher des produits ou des fournisseurs gabonais..." class="flex-grow px-6 py-2 text-sm outline-none bg-transparent">
                    <button class="bg-gab-blue text-white px-6 py-2.5 font-bold hover:opacity-90 transition">
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
                    <span class="text-[10px] font-black uppercase text-slate-600 hidden lg:block">Mon Compte</span>
                </button>
                <button onclick="checkAuthAndShow('publish')" class="bg-gab-green text-white px-4 py-2 rounded-lg text-[10px] font-black uppercase shadow-lg shadow-green-100 hover:scale-105 transition">Vendre</button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-8">
        
        <!-- SECTION: MARKETPLACE -->
        <section id="section-marketplace" class="space-y-8 animate-fade-in">
            <div class="bg-gradient-to-br from-gab-blue to-blue-700 rounded-[2.5rem] p-10 text-white relative overflow-hidden">
                <div class="relative z-10 max-w-xl">
                    <span class="bg-gab-yellow text-gab-blue px-3 py-1 rounded-full text-[10px] font-black uppercase mb-4 inline-block">Sourcing Direct Gabon</span>
                    <h2 class="text-4xl font-black italic mb-4 leading-none">VOTRE PASSERELLE B2B VERS L'EXCELLENCE GABONAISE</h2>
                    <p class="text-blue-100 text-sm mb-6">Connectez-vous directement aux usines et coopératives des 9 provinces.</p>
                    <button class="bg-white text-gab-blue px-6 py-3 rounded-xl font-black text-xs uppercase hover:bg-gab-yellow transition">Demander un Devis Groupé</button>
                </div>
                <i class="fa-solid fa-boxes-stacked absolute -right-10 -bottom-10 text-[15rem] text-white/10 rotate-12"></i>
            </div>

            <div class="flex items-center justify-between">
                <h3 class="font-black text-xl text-slate-800 uppercase italic border-l-4 border-gab-yellow pl-4">Produits Recommandés</h3>
            </div>

            <div id="productGrid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-6">
                <!-- Produits injectés par Firebase -->
            </div>
        </section>

        <!-- SECTION: PUBLISH -->
        <section id="section-publish" class="hidden max-w-xl mx-auto py-10">
            <div class="bg-white rounded-[2.5rem] shadow-xl border p-10">
                <h2 class="text-2xl font-black text-slate-800 mb-6 uppercase italic">Ajouter au Catalogue</h2>
                <form id="productForm" class="space-y-4">
                    <input type="text" id="pName" placeholder="Nom du produit" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none focus:border-gab-blue" required>
                    <div class="grid grid-cols-2 gap-4">
                        <input type="number" id="pPrice" placeholder="Prix Unitaire (FCFA)" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none" required>
                        <input type="text" id="pMoq" placeholder="MOQ (ex: 100 kg)" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none" required>
                    </div>
                    <label class="block w-full h-48 border-2 border-dashed border-slate-200 rounded-2xl bg-slate-50 hover:bg-slate-100 transition cursor-pointer flex flex-col items-center justify-center">
                        <i class="fa-solid fa-image text-3xl text-slate-300 mb-2"></i>
                        <span class="text-[10px] font-black text-slate-400">PHOTO PRODUIT</span>
                        <input type="file" id="pImgInput" class="hidden" accept="image/*">
                    </label>
                    <img id="previewImg" class="hidden w-full h-48 object-cover rounded-2xl mt-2 border">
                    <button type="submit" class="w-full bg-gab-green text-white py-5 rounded-2xl font-black uppercase shadow-lg shadow-green-100 mt-4">Publier le produit</button>
                </form>
            </div>
        </section>

        <!-- SECTION: PROFILE -->
        <section id="section-profile" class="hidden max-w-2xl mx-auto py-10">
            <div class="bg-white rounded-[2.5rem] shadow-xl border p-10">
                <div class="flex items-center justify-between mb-8">
                    <h2 class="text-2xl font-black text-slate-800 uppercase italic">Profil Entreprise</h2>
                    <button onclick="signOutUser()" class="text-red-500 font-black text-[10px] uppercase">Déconnexion</button>
                </div>
                <form id="profileForm" class="space-y-4">
                    <input type="text" id="profName" placeholder="Nom commercial" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none focus:border-gab-blue" required>
                    <input type="tel" id="profPhone" placeholder="WhatsApp (ex: 24177...)" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none" required>
                    <select id="profProvince" class="w-full bg-slate-50 border p-4 rounded-2xl outline-none font-bold">
                        <option value="Estuaire">Estuaire</option>
                        <option value="Ogooué-Maritime">Ogooué-Maritime</option>
                        <option value="Haut-Ogooué">Haut-Ogooué</option>
                    </select>
                    <button type="submit" class="w-full bg-gab-blue text-white py-4 rounded-2xl font-black uppercase">Mettre à jour</button>
                </form>
            </div>
        </section>

    </main>

    <!-- SIDEBAR: BON DE COMMANDE -->
    <div id="cartSidebar" class="fixed inset-y-0 right-0 w-full md:w-[450px] bg-white shadow-2xl z-[100] transform translate-x-full transition-transform duration-300 flex flex-col border-l">
        <div class="p-8 border-b bg-slate-50 flex justify-between items-center">
            <div>
                <h3 class="font-black text-2xl italic text-gab-blue">VOTRE DEVIS</h3>
                <p class="text-[9px] font-bold text-slate-400 uppercase tracking-widest">Générateur de Bon de Commande</p>
            </div>
            <button onclick="toggleCart()" class="text-slate-400 text-2xl hover:text-red-500 transition">&times;</button>
        </div>
        
        <div id="cartItems" class="flex-grow overflow-y-auto p-8 space-y-4 custom-scroll">
            <!-- Items de commande -->
        </div>

        <div class="p-8 border-t bg-slate-50 space-y-6">
            <div class="flex justify-between items-end">
                <span class="text-xs font-black text-slate-400 uppercase">Total HT Estimé</span>
                <span id="cartTotal" class="text-3xl font-black text-gab-blue">0 FCFA</span>
            </div>
            <button onclick="generatePDFAndSend()" class="w-full bg-gab-blue text-white py-5 rounded-2xl font-black uppercase tracking-widest flex items-center justify-center gap-3 shadow-xl hover:bg-blue-700 transition-all">
                <i class="fa-solid fa-file-pdf"></i> Envoyer au +241 77 73 60 65
            </button>
        </div>
    </div>

    <!-- NOTIF -->
    <div id="notif" class="fixed bottom-6 left-1/2 -translate-x-1/2 bg-slate-900 text-white px-8 py-4 rounded-2xl text-[10px] font-black uppercase shadow-2xl hidden z-[300]"></div>

    <!-- FIREBASE LOGIC -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInAnonymously, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, query, onSnapshot, orderBy, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // CONFIGURATION OFFICIELLE FOURNIE
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
        const appId = "echoppe241-b2b-final";
        const ADMIN_WHATSAPP = "24177736065";

        let currentUser = null;
        let userData = { name: "Client B2B", phone: "+241", province: "Estuaire" };
        let cart = [];
        let currentImg = "";

        // AUTH
        onAuthStateChanged(auth, async (user) => {
            currentUser = user;
            if (user) {
                const snap = await getDoc(doc(db, 'artifacts', appId, 'users', user.uid, 'profile', 'info'));
                if (snap.exists()) {
                    userData = snap.data();
                    document.getElementById('profName').value = userData.name;
                    document.getElementById('profPhone').value = userData.phone;
                    document.getElementById('profProvince').value = userData.province;
                }
                document.getElementById('userBtn').innerHTML = `<i class="fa-solid fa-circle-check text-xl text-gab-green"></i> <span class="text-[10px] font-black uppercase text-slate-600">${userData.name.split(' ')[0]}</span>`;
            } else {
                signInAnonymously(auth);
            }
        });

        // NAVIGATION
        window.showSection = (id) => {
            document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`section-${id}`).classList.remove('hidden');
            window.scrollTo(0,0);
        };
        window.toggleCart = () => document.getElementById('cartSidebar').classList.toggle('translate-x-full');
        window.checkAuthAndShow = (id) => showSection(id);

        // IMAGE PREVIEW
        document.getElementById('pImgInput').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (ev) => {
                    currentImg = ev.target.result;
                    document.getElementById('previewImg').src = currentImg;
                    document.getElementById('previewImg').classList.remove('hidden');
                };
                reader.readAsDataURL(file);
            }
        });

        // PUBLIER PRODUIT
        document.getElementById('productForm').onsubmit = async (e) => {
            e.preventDefault();
            if(!currentImg) return notify("Veuillez ajouter une photo");
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'products'), {
                    name: document.getElementById('pName').value,
                    price: parseInt(document.getElementById('pPrice').value),
                    moq: document.getElementById('pMoq').value,
                    img: currentImg,
                    sellerId: currentUser.uid,
                    sellerName: userData.name,
                    createdAt: serverTimestamp()
                });
                notify("Produit ajouté avec succès !");
                showSection('marketplace');
                document.getElementById('productForm').reset();
                document.getElementById('previewImg').classList.add('hidden');
            } catch (err) { notify("Erreur lors de l'ajout"); }
        };

        // PROFILE UPDATE
        document.getElementById('profileForm').onsubmit = async (e) => {
            e.preventDefault();
            userData = {
                name: document.getElementById('profName').value,
                phone: document.getElementById('profPhone').value,
                province: document.getElementById('profProvince').value
            };
            await setDoc(doc(db, 'artifacts', appId, 'users', currentUser.uid, 'profile', 'info'), userData);
            notify("Profil mis à jour !");
        };

        // RENDER PRODUCTS
        onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'products'), orderBy('createdAt', 'desc')), (snap) => {
            const grid = document.getElementById('productGrid');
            grid.innerHTML = '';
            snap.forEach(d => {
                const p = d.data();
                grid.innerHTML += `
                    <div class="bg-white rounded-[2rem] overflow-hidden border border-slate-100 b2b-card transition-all p-4 group">
                        <div class="h-40 rounded-2xl overflow-hidden mb-4 relative bg-slate-50">
                            <img src="${p.img}" class="w-full h-full object-cover group-hover:scale-110 transition duration-700">
                            <div class="absolute bottom-2 left-2 bg-black/60 backdrop-blur-sm text-white text-[8px] font-black px-2 py-1 rounded">MOQ: ${p.moq}</div>
                        </div>
                        <h4 class="text-[10px] font-black text-gab-blue uppercase mb-1">${p.sellerName}</h4>
                        <h3 class="text-xs font-bold text-slate-800 uppercase line-clamp-2 h-8 leading-tight">${p.name}</h3>
                        <div class="flex items-center justify-between mt-4">
                            <p class="text-sm font-black text-slate-900">${p.price.toLocaleString()} <span class="text-[9px] text-slate-400">FCFA</span></p>
                            <button onclick="addToCart('${d.id}', '${p.name.replace(/'/g, "\\'")}', ${p.price})" class="bg-gab-blue text-white w-8 h-8 rounded-full flex items-center justify-center shadow-lg active:scale-90 transition">
                                <i class="fa-solid fa-plus text-xs"></i>
                            </button>
                        </div>
                    </div>
                `;
            });
        });

        // CART LOGIC
        window.addToCart = (id, name, price) => {
            const existing = cart.find(item => item.id === id);
            if(existing) existing.qty++;
            else cart.push({ id, name, price, qty: 1 });
            updateCartUI();
            notify("Ajouté au devis");
        };

        function updateCartUI() {
            document.getElementById('cartCount').innerText = cart.length;
            let total = 0;
            document.getElementById('cartItems').innerHTML = cart.map((item, idx) => {
                total += item.price * item.qty;
                return `
                    <div class="bg-slate-50 p-4 rounded-2xl border flex items-center justify-between animate-fade-in">
                        <div class="flex-grow">
                            <p class="text-xs font-bold text-slate-800 uppercase">${item.name}</p>
                            <div class="flex items-center gap-4 mt-2">
                                <input type="number" value="${item.qty}" min="1" onchange="updateQty(${idx}, this.value)" class="w-12 text-center text-[10px] font-black bg-white rounded-lg p-1 border">
                                <span class="text-[10px] font-bold text-slate-400">× ${item.price.toLocaleString()} FCFA</span>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${idx})" class="text-slate-300 hover:text-red-500 transition px-2"><i class="fa-solid fa-trash-can"></i></button>
                    </div>
                `;
            }).join('');
            document.getElementById('cartTotal').innerText = total.toLocaleString() + " FCFA";
        }

        window.updateQty = (idx, val) => { cart[idx].qty = parseInt(val); updateCartUI(); };
        window.removeFromCart = (idx) => { cart.splice(idx, 1); updateCartUI(); };

        // GENERATE PDF & WHATSAPP
        window.generatePDFAndSend = () => {
            if(cart.length === 0) return notify("Votre panier est vide");
            
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            const orderId = "BC-" + Math.random().toString(36).substring(2, 8).toUpperCase();

            // Style PDF
            doc.setFillColor(0, 133, 63); // Vert Gabon
            doc.rect(0, 0, 210, 30, 'F');
            doc.setTextColor(255, 255, 255);
            doc.setFontSize(20);
            doc.text("ECHOPPE 241 - BON DE COMMANDE", 20, 20);

            doc.setTextColor(40, 40, 40);
            doc.setFontSize(10);
            doc.text(`Identifiant: ${orderId}`, 20, 45);
            doc.text(`Client: ${userData.name}`, 20, 52);
            doc.text(`Contact: ${userData.phone}`, 20, 59);

            const tableData = cart.map(i => [i.name, i.qty, i.price.toLocaleString(), (i.qty * i.price).toLocaleString()]);
            doc.autoTable({
                startY: 70,
                head: [['Produit', 'Quantité', 'PU (FCFA)', 'Total (FCFA)']],
                body: tableData,
                headStyles: { fillColor: [58, 117, 196] }
            });

            const total = cart.reduce((a,b) => a + (b.price * b.qty), 0);
            doc.setFontSize(14);
            doc.text(`TOTAL HT: ${total.toLocaleString()} FCFA`, 140, doc.lastAutoTable.finalY + 15);

            doc.save(`${orderId}.pdf`);

            // Message WhatsApp
            const message = `*COMMANDE ECHOPPE241*%0A------------------%0A*Réf:* ${orderId}%0A*Client:* ${userData.name}%0A*Total:* ${total.toLocaleString()} FCFA%0A%0A_Veuillez trouver le bon de commande en pièce jointe._`;
            window.open(`https://wa.me/${ADMIN_WHATSAPP}?text=${message}`, '_blank');
            
            cart = [];
            updateCartUI();
            toggleCart();
            notify("Bon de commande généré !");
        };

        window.notify = (m) => {
            const el = document.getElementById('notif'); el.innerText = m; el.classList.remove('hidden');
            setTimeout(() => el.classList.add('hidden'), 3000);
        };
        window.signOutUser = () => signOut(auth).then(() => location.reload());

    </script>
</body>
</html>
