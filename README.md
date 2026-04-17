<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | Officiel</title> 
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --primary-blue: #3a75c4;
            --accent-yellow: #fcd116;
            --subtle-green: #00853f;
            --white: #ffffff;
            --dark: #0f172a;
        }

        body { font-family: 'Inter', sans-serif; background-color: var(--white); color: var(--dark); margin: 0; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        h1, h2, h3, .brand-font { font-family: 'Syne', sans-serif; text-transform: lowercase; }

        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .card-neo { background: var(--white); border-radius: 24px; border: 1px solid #f1f5f9; box-shadow: 0 4px 12px rgba(58, 117, 196, 0.05); }
        
        .btn-blue { background: var(--primary-blue); color: var(--white); padding: 18px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 11px; width: 100%; transition: 0.2s; border: none; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-blue:active { transform: scale(0.96); }
        .btn-blue:disabled { opacity: 0.5; filter: grayscale(1); }
        
        .input-custom { background: #f8fafc; border-radius: 16px; padding: 16px; width: 100%; font-size: 14px; border: 2px solid transparent; outline: none; box-sizing: border-box; }
        .input-custom:focus { border-color: var(--primary-blue); background: var(--white); }

        #toast { position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%); z-index: 10000; background: var(--dark); color: var(--white); padding: 12px 24px; border-radius: 50px; font-size: 11px; font-weight: 700; display: none; box-shadow: 0 10px 25px rgba(0,0,0,0.3); }

        .modal-full { position: fixed; inset: 0; background: var(--white); z-index: 9000; overflow-y: auto; padding: 32px 24px; display: none; }
        
        .nav-item { color: #cbd5e1; flex: 1; text-align: center; border: none; background: none; cursor: pointer; }
        .nav-item.active { color: var(--primary-blue); }

        .order-badge { background: #fef9c3; color: #854d0e; font-size: 9px; padding: 4px 8px; border-radius: 6px; font-weight: 800; }
        
        .photo-capture-box { width: 100%; height: 200px; border: 2px dashed #e2e8f0; border-radius: 24px; display: flex; flex-direction: column; items-center justify-center gap-2 cursor-pointer overflow: hidden; background: #f8fafc; position: relative; }
        .photo-capture-box img { width: 100%; height: 100%; object-fit: cover; position: absolute; inset: 0; }
    </style>
</head>
<body class="pb-24">

    <div id="toast">Copié dans le presse-papier !</div>

    <!-- SPLASH -->
    <div id="splash-screen" class="fixed inset-0 bg-white z-[9999] flex flex-col items-center justify-center transition-opacity duration-500">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-24 h-24 mb-6">
        <p class="brand-font text-blue-900 font-black animate-pulse">chargement...</p>
    </div>

    <!-- VUE AUTH -->
    <div id="view-auth" class="view active min-h-screen flex items-center px-6">
        <div class="w-full max-w-md mx-auto">
            <div class="text-center mb-10">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-20 mx-auto mb-4">
                <h1 class="text-3xl font-black text-blue-900">Bienvenue.</h1>
            </div>
            <div id="login-form" class="space-y-4">
                <input type="email" id="login-email" class="input-custom" placeholder="Email">
                <input type="password" id="login-pass" class="input-custom" placeholder="Mot de passe">
                <button onclick="handleLogin()" id="btn-login" class="btn-blue">Se connecter</button>
                <p class="text-center text-xs text-slate-500 mt-4">Nouveau ? <span onclick="toggleAuth('register')" class="text-blue-600 font-bold cursor-pointer">S'inscrire</span></p>
            </div>
            <div id="register-form" class="space-y-4 hidden">
                <input type="text" id="reg-name" class="input-custom" placeholder="Nom complet">
                <input type="email" id="reg-email" class="input-custom" placeholder="Email">
                <input type="password" id="reg-pass" class="input-custom" placeholder="Mot de passe">
                <div class="flex gap-2 p-2 bg-slate-50 rounded-2xl">
                    <button onclick="setRegRole('buyer')" id="role-btn-buyer" class="flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm border border-blue-100 text-blue-600 uppercase">Acheteur</button>
                    <button onclick="setRegRole('seller')" id="role-btn-seller" class="flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase">Vendeur</button>
                </div>
                <button onclick="handleRegister()" id="btn-register" class="btn-blue bg-[#00853f]">Créer mon compte</button>
                <p class="text-center text-xs text-slate-500 mt-4">Déjà inscrit ? <span onclick="toggleAuth('login')" class="text-blue-600 font-bold cursor-pointer">Connexion</span></p>
            </div>
        </div>
    </div>

    <!-- MODAL PUBLICATION -->
    <div id="publish-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-8">
                <h2 class="text-2xl font-black text-blue-900">vendre.</h2>
                <button onclick="closeModal('publish-modal')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>
            <div class="space-y-5">
                <div onclick="triggerFileInput('pub-file')" class="photo-capture-box" id="pub-preview-box">
                    <i class="fa-solid fa-camera text-3xl text-slate-300"></i>
                    <p class="text-[10px] font-bold text-slate-400">APPAREIL PHOTO OU FICHIER</p>
                    <img id="pub-preview-img" class="hidden">
                </div>
                <input type="file" id="pub-file" accept="image/*" capture="environment" class="hidden" onchange="previewImage(this)">
                
                <input type="text" id="pub-name" class="input-custom" placeholder="Nom du produit">
                <input type="number" id="pub-price" class="input-custom" placeholder="Prix (FCFA)">
                <textarea id="pub-desc" class="input-custom h-32" placeholder="Description..."></textarea>
                <button onclick="publishProduct()" id="btn-publish-submit" class="btn-blue">Mettre en vente</button>
            </div>
        </div>
    </div>

    <!-- MODAL TRANSACTION / BON -->
    <div id="order-modal" class="modal-full">
        <div class="max-w-md mx-auto" id="order-modal-content">
            <!-- Rempli par JS -->
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="app-content" style="display:none">
        <header class="fixed top-0 inset-x-0 h-20 px-6 flex items-center justify-between bg-white/90 backdrop-blur-md border-b border-slate-50 z-[500]">
            <div class="flex items-center gap-2">
                <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-8 h-8">
                <h1 class="text-xl font-black text-blue-900">echoppe<span class="text-[#fcd116]">241</span></h1>
            </div>
            <div class="flex items-center gap-3">
                <div class="text-right">
                    <p id="header-balance" class="text-sm font-black text-blue-600">0 F</p>
                    <p class="text-[7px] font-black uppercase text-slate-400">EchoppePay</p>
                </div>
                <img id="header-avatar" onclick="navigateTo('profile')" class="w-10 h-10 rounded-2xl bg-slate-100 object-cover cursor-pointer border-2 border-white shadow-sm">
            </div>
        </header>

        <main class="max-w-xl mx-auto px-6 pt-24 pb-20">
            <!-- VUE HOME -->
            <div id="view-home" class="view active">
                <div class="flex items-center justify-between mb-6">
                    <h2 class="text-2xl font-black text-blue-900">Marché Local.</h2>
                    <button onclick="refreshProducts()" class="text-blue-600 text-xs font-bold"><i class="fa-solid fa-rotate"></i></button>
                </div>
                <div id="product-list" class="grid grid-cols-2 gap-4"></div>
            </div>

            <!-- VUE PROFILE -->
            <div id="view-profile" class="view">
                <div class="card-neo p-6 mb-6 text-center">
                    <img id="p-img" class="w-24 h-24 rounded-3xl mx-auto mb-4 object-cover border-4 border-white shadow-md bg-slate-50">
                    <h3 id="p-name" class="text-xl font-black text-blue-900 mb-1">...</h3>
                    <div id="p-role-tag" class="inline-block px-3 py-1 rounded-full text-[9px] font-black uppercase mb-6">...</div>
                    
                    <div class="bg-blue-600 p-6 rounded-3xl text-white text-left shadow-xl shadow-blue-100">
                        <div class="flex justify-between items-center mb-1">
                            <span class="text-[9px] font-black opacity-70 uppercase">Solde EchoppePay</span>
                            <i class="fa-solid fa-shield-halved text-[10px]"></i>
                        </div>
                        <p id="p-wallet" class="text-2xl font-black">0 FCFA</p>
                        <button onclick="openRecharge()" class="mt-4 w-full py-3 bg-white/20 rounded-xl text-[10px] font-black uppercase hover:bg-white/30 transition-colors">Recharger le compte</button>
                    </div>
                </div>

                <!-- ADMIN ZONE -->
                <div id="admin-zone" class="hidden space-y-6 mb-8">
                    <div class="flex items-center gap-2 text-red-500 mb-3">
                        <i class="fa-solid fa-crown"></i>
                        <h4 class="text-[10px] font-black uppercase tracking-widest">Administration</h4>
                    </div>
                    
                    <div class="space-y-4">
                        <div class="bg-slate-50 p-4 rounded-2xl">
                            <h5 class="text-[9px] font-black text-slate-400 uppercase mb-3">Recharges en attente</h5>
                            <div id="admin-recharges" class="space-y-2"></div>
                        </div>
                        <div class="bg-slate-50 p-4 rounded-2xl">
                            <h5 class="text-[9px] font-black text-slate-400 uppercase mb-3">Transactions à valider</h5>
                            <div id="admin-transactions" class="space-y-2"></div>
                        </div>
                    </div>
                </div>

                <div class="space-y-3">
                    <button id="btn-seller-sell" onclick="openModal('publish-modal')" class="hidden w-full p-5 bg-white border border-slate-100 rounded-2xl flex items-center justify-between">
                        <span class="text-xs font-bold text-slate-700">Vendre un article</span>
                        <i class="fa-solid fa-plus-circle text-blue-600"></i>
                    </button>
                    <button onclick="signOut(auth)" class="w-full p-4 text-red-500 text-[10px] font-black uppercase">Déconnexion</button>
                </div>
            </div>
        </main>

        <nav class="fixed bottom-0 inset-x-0 h-20 bg-white border-t border-slate-100 flex items-center justify-around px-4 pb-2 z-[500] shadow-2xl">
            <button onclick="navigateTo('home')" id="nav-home" class="nav-item active"><i class="fa-solid fa-house text-lg"></i><p class="text-[8px] font-black mt-1">HOME</p></button>
            <button onclick="navigateTo('messages')" id="nav-messages" class="nav-item"><i class="fa-solid fa-comment-dots text-lg"></i><p class="text-[8px] font-black mt-1">CHATS</p></button>
            <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item"><i class="fa-solid fa-user text-lg"></i><p class="text-[8px] font-black mt-1">COMPTE</p></button>
        </nav>
    </div>

    <div id="recharge-modal" class="modal-full">
        <div class="max-w-md mx-auto">
            <div class="flex items-center justify-between mb-8">
                <h2 class="text-2xl font-black text-blue-900">recharge.</h2>
                <button onclick="closeModal('recharge-modal')" class="w-10 h-10 bg-slate-50 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
            </div>
            <div class="bg-blue-50 p-6 rounded-3xl mb-6">
                <p class="text-xs font-bold text-blue-800 mb-3 underline">Instructions Airtel/Moov :</p>
                <p class="text-[10px] text-blue-700 leading-relaxed mb-4">Faites le dépôt au <b>077 73 60 65</b>. Copiez la référence de la transaction ci-dessous.</p>
                <input type="number" id="rech-amount" class="input-custom mb-3" placeholder="Montant">
                <input type="text" id="rech-ref" class="input-custom" placeholder="Référence de transaction">
            </div>
            <button onclick="submitRecharge()" id="btn-rech-submit" class="btn-blue bg-[#00853f]">Soumettre pour validation</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc, updateDoc, collection, addDoc, query, where, orderBy, serverTimestamp, increment, getDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

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
        const appId = 'echoppe241-stable-v2';

        let user = null;
        let userData = null;
        let currentRegRole = 'buyer';
        let currentPhotoBase64 = null;
        let unsubList = [];

        // --- AUTH ---
        onAuthStateChanged(auth, async (u) => {
            unsubList.forEach(unsub => unsub());
            unsubList = [];
            
            if (u) {
                user = u;
                const userRef = doc(db, 'artifacts', appId, 'users', u.uid);
                const unsubUser = onSnapshot(userRef, (snap) => {
                    if (snap.exists()) {
                        userData = snap.data();
                        updateUI();
                        if(userData.isAdmin) setupAdminListeners();
                    } else {
                        // Création automatique si premier login
                        setDoc(userRef, {
                            fullName: u.displayName || "Client Echoppe",
                            email: u.email,
                            role: 'buyer',
                            walletBalance: 0,
                            isAdmin: false,
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`,
                            createdAt: serverTimestamp()
                        });
                    }
                });
                unsubList.push(unsubUser);
                document.getElementById('view-auth').classList.remove('active');
                document.getElementById('app-content').style.display = 'block';
                loadProducts();
            } else {
                user = null;
                document.getElementById('app-content').style.display = 'none';
                document.getElementById('view-auth').classList.add('active');
            }
            document.getElementById('splash-screen').style.opacity = '0';
            setTimeout(() => document.getElementById('splash-screen').style.display = 'none', 500);
        });

        function updateUI() {
            document.getElementById('header-balance').innerText = `${userData.walletBalance.toLocaleString()} F`;
            document.getElementById('header-avatar').src = userData.avatar;
            document.getElementById('p-img').src = userData.avatar;
            document.getElementById('p-name').innerText = userData.fullName;
            document.getElementById('p-wallet').innerText = `${userData.walletBalance.toLocaleString()} FCFA`;
            
            const tag = document.getElementById('p-role-tag');
            tag.innerText = userData.isAdmin ? "Administrateur" : (userData.role === 'seller' ? "Vendeur Certifié" : "Acheteur");
            tag.className = `inline-block px-3 py-1 rounded-full text-[9px] font-black uppercase mb-6 ${userData.isAdmin ? 'bg-red-100 text-red-600' : (userData.role === 'seller' ? 'bg-green-100 text-green-600' : 'bg-blue-100 text-blue-600')}`;
            
            if(userData.isAdmin) document.getElementById('admin-zone').classList.remove('hidden');
            if(userData.role === 'seller' || userData.isAdmin) document.getElementById('btn-seller-sell').classList.remove('hidden');
        }

        // --- LOGIQUE PHOTO ---
        window.triggerFileInput = (id) => document.getElementById(id).click();
        window.previewImage = (input) => {
            const file = input.files[0];
            if(!file) return;
            const reader = new FileReader();
            reader.onload = (e) => {
                currentPhotoBase64 = e.target.result;
                const img = document.getElementById('pub-preview-img');
                img.src = currentPhotoBase64;
                img.classList.remove('hidden');
            };
            reader.readAsDataURL(file);
        };

        // --- PRODUITS ---
        function loadProducts() {
            const unsubProd = onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'products'), orderBy('createdAt', 'desc')), (snap) => {
                const list = document.getElementById('product-list');
                list.innerHTML = snap.docs.map(doc => {
                    const p = doc.data();
                    return `
                        <div onclick="showProductDetail('${doc.id}')" class="card-neo overflow-hidden flex flex-col cursor-pointer transition:active scale-95">
                            <img src="${p.image}" class="h-32 w-full object-cover">
                            <div class="p-3">
                                <p class="text-[10px] font-black uppercase truncate mb-1">${p.name}</p>
                                <p class="text-blue-600 font-bold text-xs">${p.price.toLocaleString()} F</p>
                            </div>
                        </div>
                    `;
                }).join('');
            });
            unsubList.push(unsubProd);
        }

        window.publishProduct = async () => {
            const name = document.getElementById('pub-name').value.trim();
            const price = document.getElementById('pub-price').value;
            const desc = document.getElementById('pub-desc').value.trim();
            if(!name || !price || !currentPhotoBase64) return showToast("Infos manquantes");

            const btn = document.getElementById('btn-publish-submit');
            btn.disabled = true;
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'products'), {
                    name, price: parseInt(price), description: desc, image: currentPhotoBase64,
                    sellerId: user.uid, sellerName: userData.fullName, createdAt: serverTimestamp()
                });
                closeModal('publish-modal');
                showToast("Produit en ligne !");
                // Reset
                document.getElementById('pub-name').value = "";
                document.getElementById('pub-price').value = "";
                document.getElementById('pub-desc').value = "";
                document.getElementById('pub-preview-img').classList.add('hidden');
                currentPhotoBase64 = null;
            } catch(e) { showToast("Erreur serveur"); }
            btn.disabled = false;
        };

        window.showProductDetail = async (id) => {
            const snap = await getDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', id));
            if(!snap.exists()) return;
            const p = snap.data();
            const content = document.getElementById('order-modal-content');
            content.innerHTML = `
                <div class="flex items-center justify-between mb-6">
                    <button onclick="closeModal('order-modal')" class="w-10 h-10 bg-slate-100 rounded-full flex items-center justify-center"><i class="fa-solid fa-times"></i></button>
                    <p class="text-[10px] font-black uppercase text-slate-400">Détails Article</p>
                </div>
                <img src="${p.image}" class="w-full h-56 object-cover rounded-3xl mb-6 shadow-sm">
                <h2 class="text-2xl font-black text-blue-900 mb-2">${p.name}</h2>
                <p class="text-blue-600 font-black text-xl mb-4">${p.price.toLocaleString()} FCFA</p>
                <div class="bg-slate-50 p-4 rounded-2xl mb-6">
                    <p class="text-xs text-slate-500 leading-relaxed">${p.description || "Pas de description."}</p>
                </div>
                <div class="flex flex-col gap-3">
                    <button onclick="buyProduct('${id}', ${p.price}, '${p.name.replace(/'/g, "\\'")}')" class="btn-blue bg-[#00853f]">
                        <i class="fa-solid fa-cart-shopping"></i> Acheter maintenant
                    </button>
                    <button onclick="generateBon('${id}', '${p.name.replace(/'/g, "\\'")}', ${p.price})" class="btn-blue bg-white border border-slate-200 !text-slate-800">
                        <i class="fa-solid fa-file-invoice"></i> Partager le bon de commande
                    </button>
                </div>
            `;
            openModal('order-modal');
        };

        // --- TRANSACTIONS ---
        window.buyProduct = async (prodId, price, prodName) => {
            if(userData.walletBalance < price) return showToast("Solde EchoppePay insuffisant !");
            
            if(!confirm(`Confirmer l'achat de "${prodName}" pour ${price} F ?`)) return;

            try {
                // Créer transaction en attente
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), {
                    buyerId: user.uid,
                    buyerName: userData.fullName,
                    productId: prodId,
                    productName: prodName,
                    amount: price,
                    status: 'pending_validation',
                    createdAt: serverTimestamp()
                });
                
                // Déduire du solde immédiatement (séquestre)
                await updateDoc(doc(db, 'artifacts', appId, 'users', user.uid), {
                    walletBalance: increment(-price)
                });

                showToast("Paiement envoyé ! En attente de validation Admin.");
                closeModal('order-modal');
            } catch(e) { showToast("Erreur transaction"); }
        };

        window.generateBon = (id, name, price) => {
            const bon = `BON DE COMMANDE - ECHOPPE241\n--------------------------\nProduit: ${name}\nPrix: ${price} FCFA\nRef: ${id}\nAcheteur: ${userData.fullName}\nDate: ${new Date().toLocaleDateString()}\n--------------------------\nMerci de votre confiance !`;
            const textArea = document.createElement("textarea");
            textArea.value = bon;
            document.body.appendChild(textArea);
            textArea.select();
            document.execCommand('copy');
            document.body.removeChild(textArea);
            showToast("Bon copié ! Partagez-le sur WhatsApp.");
        };

        // --- ADMINISTRATION ---
        function setupAdminListeners() {
            // Recharges
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), where('status', '==', 'pending')), (snap) => {
                const list = document.getElementById('admin-recharges');
                list.innerHTML = snap.empty ? '<p class="text-[9px] italic text-slate-300">Rien à signaler</p>' : snap.docs.map(d => {
                    const r = d.data();
                    return `
                        <div class="flex items-center justify-between bg-white p-3 rounded-xl border border-blue-50">
                            <div>
                                <p class="text-[10px] font-black">${r.amount} F</p>
                                <p class="text-[8px] text-slate-400">Ref: ${r.reference}</p>
                            </div>
                            <button onclick="approveRecharge('${d.id}', '${r.userId}', ${r.amount})" class="px-3 py-1 bg-blue-600 text-white text-[8px] font-black rounded-lg">VALIDER</button>
                        </div>
                    `;
                }).join('');
            });

            // Transactions de vente
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'transactions'), where('status', '==', 'pending_validation')), (snap) => {
                const list = document.getElementById('admin-transactions');
                list.innerHTML = snap.empty ? '<p class="text-[9px] italic text-slate-300">Aucun achat à valider</p>' : snap.docs.map(d => {
                    const t = d.data();
                    return `
                        <div class="flex items-center justify-between bg-white p-3 rounded-xl border border-green-50">
                            <div>
                                <p class="text-[10px] font-black">${t.productName}</p>
                                <p class="text-[8px] text-slate-400">Acheteur: ${t.buyerName}</p>
                            </div>
                            <button onclick="approveSale('${d.id}')" class="px-3 py-1 bg-[#00853f] text-white text-[8px] font-black rounded-lg">CONFIRMER</button>
                        </div>
                    `;
                }).join('');
            });
        }

        window.approveRecharge = async (docId, uid, amount) => {
            try {
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'recharges', docId), { status: 'approved' });
                await updateDoc(doc(db, 'artifacts', appId, 'users', uid), { walletBalance: increment(amount) });
                showToast("Recharge validée !");
            } catch(e) {}
        };

        window.approveSale = async (docId) => {
            try {
                await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'transactions', docId), { status: 'completed' });
                showToast("Vente confirmée !");
            } catch(e) {}
        };

        window.submitRecharge = async () => {
            const amount = document.getElementById('rech-amount').value;
            const ref = document.getElementById('rech-ref').value.trim();
            if(!amount || !ref) return showToast("Infos requises");
            
            const btn = document.getElementById('btn-rech-submit');
            btn.disabled = true;
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'recharges'), {
                    userId: user.uid, amount: parseInt(amount), reference: ref, status: 'pending', createdAt: serverTimestamp()
                });
                showToast("Demande envoyée !");
                closeModal('recharge-modal');
            } catch(e) {}
            btn.disabled = false;
        };

        // --- HELPERS ---
        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(`nav-${v}`).classList.add('active');
        };
        window.toggleAuth = (m) => {
            document.getElementById('login-form').classList.toggle('hidden', m === 'register');
            document.getElementById('register-form').classList.toggle('hidden', m === 'login');
        };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.openRecharge = () => openModal('recharge-modal');
        window.showToast = (m) => {
            const t = document.getElementById('toast'); t.innerText = m; t.style.display = 'block';
            setTimeout(() => t.style.display = 'none', 3000);
        };
        window.setRegRole = (r) => {
            currentRegRole = r;
            document.getElementById('role-btn-buyer').className = r === 'buyer' ? 'flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm border border-blue-100 text-blue-600 uppercase' : 'flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase';
            document.getElementById('role-btn-seller').className = r === 'seller' ? 'flex-1 py-3 rounded-xl text-[10px] font-black bg-white shadow-sm border border-green-100 text-green-600 uppercase' : 'flex-1 py-3 rounded-xl text-[10px] font-black text-slate-400 uppercase';
        };

        window.handleLogin = async () => {
            const e = document.getElementById('login-email').value;
            const p = document.getElementById('login-pass').value;
            try { await signInWithEmailAndPassword(auth, e, p); } catch(err) { showToast("Identifiants incorrects"); }
        };

        window.handleRegister = async () => {
            const n = document.getElementById('reg-name').value;
            const e = document.getElementById('reg-email').value;
            const p = document.getElementById('reg-pass').value;
            try {
                const res = await createUserWithEmailAndPassword(auth, e, p);
                await setDoc(doc(db, 'artifacts', appId, 'users', res.user.uid), {
                    fullName: n, email: e, role: currentRegRole, walletBalance: 0, isAdmin: false,
                    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${res.user.uid}`, createdAt: serverTimestamp()
                });
            } catch(err) { showToast("Erreur inscription"); }
        };
    </script>
</body>
</html>
