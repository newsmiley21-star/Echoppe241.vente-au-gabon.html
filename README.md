<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Echoppe241 | Connexion</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <!-- Scripts Externes -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.23/jspdf.plugin.autotable.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    
    <style>
        :root { --gab-green: #00853f; --gab-yellow: #fcd116; --gab-blue: #3a75c4; }
        body { font-family: 'Segoe UI', system-ui, sans-serif; background-color: #f0f2f5; }
        .bg-gab-blue { background-color: var(--gab-blue); }
        .text-gab-blue { color: var(--gab-blue); }
        .product-card { background: white; border-radius: 1rem; box-shadow: 0 1px 2px rgba(0,0,0,0.1); }
        .cover-photo { height: 140px; background: linear-gradient(to bottom, var(--gab-blue), #1e40af); border-radius: 0 0 1rem 1rem; }
        .profile-avatar { width: 90px; height: 90px; border: 4px solid white; border-radius: 50%; margin-top: -45px; background: #e4e6eb; overflow: hidden; }
        #qrcode-container { visibility: hidden; position: absolute; left: -9999px; }
        
        /* Loader */
        .loader { border: 3px solid #f3f3f3; border-top: 3px solid var(--gab-blue); border-radius: 50%; width: 20px; height: 20px; animation: spin 1s linear infinite; display: inline-block; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        
        #init-loader { z-index: 300; }
    </style>
</head>
<body class="pb-20">

    <div id="qrcode-container"></div>

    <!-- ÉCRAN DE CHARGEMENT INITIAL -->
    <div id="init-loader" class="fixed inset-0 bg-white flex items-center justify-center">
        <div class="text-center">
            <div class="loader w-10 h-10 mb-4 border-t-gab-blue"></div>
            <p class="text-xs font-bold text-slate-400 uppercase tracking-widest">Initialisation...</p>
        </div>
    </div>

    <!-- ÉCRAN D'AUTH -->
    <div id="auth-screen" class="hidden fixed inset-0 bg-white z-[200] flex flex-col items-center justify-center px-6">
        <div class="text-center mb-8">
            <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Logo" class="h-16 mx-auto mb-4">
            <h1 class="text-2xl font-black uppercase tracking-tighter">ECHOPPE<span class="text-gab-blue">241</span></h1>
            <p class="text-slate-500 text-sm">Le Marché des Grossistes du Gabon</p>
        </div>

        <div class="w-full max-w-sm space-y-4">
            <div id="auth-title" class="text-center font-bold text-slate-700 uppercase text-xs tracking-widest mb-2">Connexion</div>
            
            <div id="register-fields" class="hidden space-y-4">
                <input type="text" id="auth-name" placeholder="Nom de l'entreprise / Prénom" class="w-full bg-slate-100 border-none p-4 rounded-2xl outline-none focus:ring-2 focus:ring-gab-blue transition-all">
                <input type="tel" id="auth-phone" placeholder="Numéro WhatsApp (ex: 077...)" class="w-full bg-slate-100 border-none p-4 rounded-2xl outline-none focus:ring-2 focus:ring-gab-blue transition-all">
            </div>

            <input type="email" id="auth-email" placeholder="Email" class="w-full bg-slate-100 border-none p-4 rounded-2xl outline-none focus:ring-2 focus:ring-gab-blue transition-all">
            <input type="password" id="auth-password" placeholder="Mot de passe" class="w-full bg-slate-100 border-none p-4 rounded-2xl outline-none focus:ring-2 focus:ring-gab-blue transition-all">
            
            <button id="auth-btn" onclick="handleAuth()" class="w-full bg-gab-blue text-white py-4 rounded-2xl font-black uppercase shadow-lg flex items-center justify-center gap-3">
                <span id="btn-text">Se connecter</span>
            </button>

            <p class="text-center text-sm text-slate-500">
                <span id="toggle-text">Pas encore de compte ?</span>
                <button onclick="toggleAuthMode()" id="toggle-btn" class="text-gab-blue font-bold ml-1">S'inscrire</button>
            </p>
        </div>
    </div>

    <!-- APP INTERFACE -->
    <div id="app-content" class="hidden">
        <header class="bg-white sticky top-0 z-40 shadow-sm border-b">
            <div class="max-w-7xl mx-auto px-4 py-3 flex items-center justify-between">
                <div class="flex items-center gap-2" onclick="showSection('marketplace')">
                    <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Logo" class="h-8">
                    <h1 class="font-black text-lg tracking-tighter uppercase">ECHOPPE<span class="text-gab-blue">241</span></h1>
                </div>
                <button onclick="toggleCart()" class="relative p-2 bg-slate-100 rounded-full">
                    <i class="fa-solid fa-file-invoice-dollar text-lg text-slate-600"></i>
                    <span id="cartCount" class="absolute -top-1 -right-1 bg-red-500 text-white text-[9px] font-black w-4 h-4 rounded-full flex items-center justify-center">0</span>
                </button>
            </div>
            
            <div id="searchBar" class="px-4 pb-3 flex gap-2">
                <input type="text" id="searchInput" oninput="filterProducts(this.value)" placeholder="Rechercher..." class="flex-grow bg-slate-100 rounded-xl px-4 py-2 text-sm outline-none">
            </div>
        </header>

        <main class="max-w-2xl mx-auto p-4">
            <!-- MARKETPLACE -->
            <section id="section-marketplace" class="grid grid-cols-2 gap-3"></section>

            <!-- PUBLISH -->
            <section id="section-publish" class="hidden space-y-4">
                <div class="bg-white p-6 rounded-2xl border shadow-sm">
                    <h2 class="font-black uppercase mb-4 italic">Publier un Stock</h2>
                    <form id="productForm" class="space-y-4">
                        <input type="text" id="pName" placeholder="Nom du produit" class="w-full bg-slate-50 border p-3 rounded-xl" required>
                        <input type="number" id="pPrice" placeholder="Prix HT (FCFA)" class="w-full bg-slate-50 border p-3 rounded-xl" required>
                        <input type="text" id="pMoq" placeholder="Min. Commande" class="w-full bg-slate-50 border p-3 rounded-xl" required>
                        <label class="block h-32 border-2 border-dashed rounded-xl bg-slate-50 flex items-center justify-center relative overflow-hidden cursor-pointer">
                            <i id="pCamIcon" class="fa-solid fa-camera text-2xl text-slate-300"></i>
                            <img id="pPreview" class="hidden absolute inset-0 w-full h-full object-cover">
                            <input type="file" id="pImgInput" class="hidden" accept="image/*">
                        </label>
                        <button type="submit" class="w-full bg-gab-green text-white py-4 rounded-xl font-black uppercase">Mettre en vente au Gabon</button>
                    </form>
                </div>
            </section>

            <!-- PROFILE -->
            <section id="section-profile" class="hidden">
                <div class="bg-white rounded-2xl overflow-hidden shadow-sm border mb-4">
                    <div class="cover-photo"></div>
                    <div class="px-6 pb-6">
                        <div class="flex items-end justify-between">
                            <div class="profile-avatar"><img id="userAvatar" src=""></div>
                            <button onclick="handleLogout()" class="bg-red-50 text-red-500 px-4 py-2 rounded-lg text-[10px] font-black uppercase">Déconnexion</button>
                        </div>
                        <h2 class="text-xl font-black mt-4 flex items-center gap-2">
                            <span id="profileTitleName">...</span>
                            <i class="fa-solid fa-circle-check text-gab-blue text-sm"></i>
                        </h2>
                        <p class="text-slate-500 text-sm">Vendeur vérifié • <span id="profileProvinceDisplay">Estuaire</span></p>
                        <div class="mt-4 pt-4 border-t flex gap-4">
                            <div class="flex items-center gap-2 text-xs"><i class="fa-brands fa-whatsapp text-green-500 text-lg"></i> <span id="profilePhoneDisplay">...</span></div>
                        </div>
                    </div>
                </div>
            </section>
        </main>

        <nav class="fixed bottom-0 inset-x-0 bg-white border-t h-16 flex items-center justify-around z-50">
            <button onclick="showSection('marketplace')" id="nav-marketplace" class="flex flex-col items-center text-gab-blue"><i class="fa-solid fa-house-chimney"></i><span class="text-[9px] font-bold">FLUX</span></button>
            <button onclick="showSection('publish')" id="nav-publish" class="flex flex-col items-center text-slate-400"><i class="fa-solid fa-square-plus"></i><span class="text-[9px] font-bold">VENDRE</span></button>
            <button onclick="showSection('profile')" id="nav-profile" class="flex flex-col items-center text-slate-400"><i class="fa-solid fa-circle-user"></i><span class="text-[9px] font-bold">PROFIL</span></button>
        </nav>
    </div>

    <!-- Panier -->
    <div id="cartSidebar" class="fixed inset-y-0 right-0 w-full md:w-[400px] bg-white shadow-2xl z-[100] transform translate-x-full transition-transform flex flex-col">
        <div class="p-6 border-b flex justify-between items-center"><h3 class="font-black text-gab-blue uppercase italic">Mon Devis</h3><button onclick="toggleCart()" class="text-3xl">&times;</button></div>
        <div id="cartItems" class="flex-grow overflow-y-auto p-4 space-y-3"></div>
        <div class="p-6 border-t bg-slate-50"><button id="btnOrder" onclick="finalizeOrder()" class="w-full bg-gab-blue text-white py-4 rounded-xl font-black uppercase text-xs">Commander (PDF + WhatsApp)</button></div>
    </div>

    <div id="notif" class="fixed bottom-20 left-1/2 -translate-x-1/2 bg-slate-900 text-white px-6 py-3 rounded-full text-[10px] font-black uppercase shadow-2xl hidden z-[300]"></div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, query, onSnapshot, orderBy, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = "echoppe241-prod-v1";
        const ADMIN_WA = "+24177736065";

        let currentUser = null;
        let userData = null;
        let isRegisterMode = false;
        let cart = [];
        let pImageBase64 = "";

        // AUTH MONITOR
        onAuthStateChanged(auth, async (user) => {
            document.getElementById('init-loader').classList.add('hidden');
            if (user) {
                currentUser = user;
                await syncUserProfile();
                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('app-content').classList.remove('hidden');
                initFeed();
            } else {
                document.getElementById('auth-screen').classList.remove('hidden');
                document.getElementById('app-content').classList.add('hidden');
            }
        });

        window.toggleAuthMode = () => {
            isRegisterMode = !isRegisterMode;
            document.getElementById('register-fields').classList.toggle('hidden');
            document.getElementById('auth-title').innerText = isRegisterMode ? "Inscription" : "Connexion";
            document.getElementById('btn-text').innerText = isRegisterMode ? "Créer mon compte" : "Se connecter";
            document.getElementById('toggle-text').innerText = isRegisterMode ? "Déjà un compte ?" : "Pas encore de compte ?";
            document.getElementById('toggle-btn').innerText = isRegisterMode ? "Se connecter" : "S'inscrire";
        };

        window.handleAuth = async () => {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-password').value;
            const btn = document.getElementById('auth-btn');
            
            if(!email || !pass) return notify("Remplissez les champs");
            
            btn.disabled = true;
            btn.innerHTML = `<div class="loader"></div>`;

            try {
                if (isRegisterMode) {
                    const name = document.getElementById('auth-name').value;
                    const phone = document.getElementById('auth-phone').value;
                    if(!name || !phone) throw new Error("Nom et téléphone requis");

                    const res = await createUserWithEmailAndPassword(auth, email, pass);
                    // Création immédiate du profil pour éviter les erreurs d'accès
                    await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid, 'profile', 'info'), {
                        name, phone, province: "Estuaire", avatar: "", createdAt: serverTimestamp()
                    });
                } else {
                    await signInWithEmailAndPassword(auth, email, pass);
                }
            } catch (err) {
                console.error(err);
                notify(err.message.includes("auth/") ? "Identifiants invalides" : "Erreur de connexion");
            } finally {
                btn.disabled = false;
                btn.innerHTML = `<span id="btn-text">${isRegisterMode ? "Créer mon compte" : "Se connecter"}</span>`;
            }
        };

        window.handleLogout = () => signOut(auth);

        async function syncUserProfile() {
            try {
                const profileRef = doc(db, 'artifacts', appId, 'users', currentUser.uid, 'profile', 'info');
                const snap = await getDoc(profileRef);
                
                if (snap.exists()) {
                    userData = snap.data();
                } else {
                    // Fallback si le profil n'existe pas (par ex. bug inscription)
                    userData = { name: "Utilisateur", phone: "Non défini", province: "Estuaire", avatar: "" };
                    await setDoc(profileRef, userData);
                }
                
                document.getElementById('profileTitleName').innerText = userData.name;
                document.getElementById('profilePhoneDisplay').innerText = userData.phone;
                document.getElementById('userAvatar').src = userData.avatar || `https://ui-avatars.com/api/?name=${encodeURIComponent(userData.name)}&background=3a75c4&color=fff`;
            } catch (e) {
                console.error("Profil inaccessible:", e);
                notify("Erreur profil - Vérifiez vos règles Firebase");
            }
        }

        function initFeed() {
            const q = query(collection(db, 'artifacts', appId, 'public', 'products'), orderBy('createdAt', 'desc'));
            onSnapshot(q, (snap) => {
                const grid = document.getElementById('section-marketplace');
                let html = "";
                snap.forEach(d => {
                    const p = d.data();
                    html += `
                        <div class="product-card overflow-hidden flex flex-col">
                            <img src="${p.img}" class="h-32 w-full object-cover bg-slate-100">
                            <div class="p-3">
                                <h3 class="text-[11px] font-bold truncate uppercase">${p.name}</h3>
                                <p class="text-xs font-black text-gab-blue">${(p.price || 0).toLocaleString()} F</p>
                                <div class="flex gap-1 mt-2">
                                    <button onclick="addToCart('${d.id}', '${p.name.replace(/'/g, "")}', ${p.price}, '${p.sellerPhone}')" class="flex-grow bg-slate-100 h-8 rounded-lg text-gab-blue active:scale-95 transition"><i class="fa-solid fa-cart-plus"></i></button>
                                    <button onclick="window.open('https://wa.me/241${(p.sellerPhone || '').replace(/^0/, '')}')" class="w-8 h-8 bg-green-500 text-white rounded-lg flex items-center justify-center"><i class="fa-brands fa-whatsapp"></i></button>
                                </div>
                            </div>
                        </div>`;
                });
                grid.innerHTML = html || `<p class="col-span-2 text-center py-10 text-slate-400 text-xs font-bold uppercase">Aucun stock disponible</p>`;
            }, (err) => {
                console.error("Erreur flux:", err);
                notify("Erreur d'accès aux données public");
            });
        }

        window.showSection = (id) => {
            document.querySelectorAll('main section').forEach(s => s.classList.add('hidden'));
            document.getElementById(`section-${id}`).classList.remove('hidden');
            document.querySelectorAll('nav button').forEach(b => b.classList.replace('text-gab-blue', 'text-slate-400'));
            document.getElementById(`nav-${id}`).classList.replace('text-slate-400', 'text-gab-blue');
        };

        window.toggleCart = () => document.getElementById('cartSidebar').classList.toggle('translate-x-full');

        window.addToCart = (id, name, price, sellerPhone) => {
            const item = cart.find(i => i.id === id);
            if(item) item.qty++; else cart.push({id, name, price, qty: 1, sellerPhone});
            document.getElementById('cartCount').innerText = cart.length;
            renderCart();
            notify("Ajouté au panier");
        };

        function renderCart() {
            document.getElementById('cartItems').innerHTML = cart.map((i, idx) => `
                <div class="bg-slate-50 p-3 rounded-xl flex justify-between items-center text-xs font-bold border">
                    <div class="flex flex-col">
                        <span>${i.name}</span>
                        <span class="text-gab-blue">${(i.price*i.qty).toLocaleString()} F</span>
                    </div>
                    <div class="flex items-center gap-3">
                        <span class="bg-white px-2 py-1 rounded border">x${i.qty}</span>
                        <button onclick="cart.splice(${idx},1);renderCart();document.getElementById('cartCount').innerText=cart.length" class="text-red-400 p-2"><i class="fa-solid fa-trash"></i></button>
                    </div>
                </div>
            `).join('');
        }

        document.getElementById('pImgInput').onchange = (e) => {
            const file = e.target.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = (ev) => {
                pImageBase64 = ev.target.result;
                document.getElementById('pPreview').src = pImageBase64;
                document.getElementById('pPreview').classList.remove('hidden');
                document.getElementById('pCamIcon').classList.add('hidden');
            };
            reader.readAsDataURL(file);
        };

        document.getElementById('productForm').onsubmit = async (e) => {
            e.preventDefault();
            if(!pImageBase64) return notify("Image requise");
            
            const btn = e.target.querySelector('button');
            btn.disabled = true;
            btn.innerHTML = `<div class="loader border-t-white"></div>`;

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'products'), {
                    name: document.getElementById('pName').value,
                    price: parseInt(document.getElementById('pPrice').value),
                    moq: document.getElementById('pMoq').value,
                    img: pImageBase64,
                    sellerPhone: userData.phone,
                    sellerName: userData.name,
                    createdAt: serverTimestamp()
                });
                notify("Produit publié avec succès !");
                e.target.reset();
                pImageBase64 = "";
                document.getElementById('pPreview').classList.add('hidden');
                document.getElementById('pCamIcon').classList.remove('hidden');
                showSection('marketplace');
            } catch (err) {
                notify("Erreur lors de la publication");
            } finally {
                btn.disabled = false;
                btn.innerHTML = "Mettre en vente au Gabon";
            }
        };

        window.finalizeOrder = async () => {
            if(!cart.length) return notify("Panier vide");
            const orderRef = Math.random().toString(36).substring(7).toUpperCase();
            const qrContainer = document.getElementById('qrcode-container');
            qrContainer.innerHTML = "";
            new QRCode(qrContainer, { text: `REF:${orderRef}|CLI:${userData.phone}`, width: 128, height: 128 });

            setTimeout(() => {
                const qrImg = qrContainer.querySelector('img').src;
                const docPdf = new window.jspdf.jsPDF();
                docPdf.setFontSize(18);
                docPdf.text("ECHOPPE 241 - BON DE COMMANDE", 20, 20);
                docPdf.setFontSize(10);
                docPdf.text(`Client: ${userData.name} | Tél: ${userData.phone}`, 20, 28);
                
                docPdf.autoTable({ 
                    startY: 35, 
                    head: [['Produit', 'Qté', 'Total']], 
                    body: cart.map(i => [i.name, i.qty, (i.price*i.qty).toLocaleString() + " F"]),
                    theme: 'striped'
                });
                
                docPdf.addImage(qrImg, 'PNG', 20, docPdf.lastAutoTable.finalY + 10, 30, 30);
                docPdf.save(`Commande_${orderRef}.pdf`);
                
                const waMsg = `*COMMANDE ECHOPPE241*%0ARef: ${orderRef}%0AClient: ${userData.name}%0A%0A_Veuillez trouver mon bon de commande PDF ci-joint._`;
                window.open(`https://wa.me/${ADMIN_WA.replace('+', '')}?text=${waMsg}`);
                
                cart = []; renderCart(); toggleCart(); document.getElementById('cartCount').innerText = "0";
            }, 600);
        };

        window.notify = (m) => { 
            const el = document.getElementById('notif'); 
            el.innerText = m; 
            el.classList.remove('hidden'); 
            setTimeout(() => el.classList.add('hidden'), 3500); 
        };

    </script>
</body>
</html>
