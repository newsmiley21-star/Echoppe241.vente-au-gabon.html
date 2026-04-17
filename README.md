<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Echoppe241 | EchoppePay Pro</title> 
    
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

        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; color: var(--dark); margin: 0; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        h1, h2, h3, .brand-font { font-family: 'Syne', sans-serif; text-transform: lowercase; }

        .view { display: none; }
        .view.active { display: block; animation: fadeIn 0.4s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .card-neo { background: var(--white); border-radius: 28px; border: 1px solid rgba(226, 232, 240, 0.8); box-shadow: 0 10px 25px -5px rgba(58, 117, 196, 0.05); }
        .btn-blue { background: var(--primary-blue); color: var(--white); padding: 16px; border-radius: 20px; font-weight: 800; text-transform: uppercase; font-size: 11px; width: 100%; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); border: none; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-blue:active { transform: scale(0.96); opacity: 0.9; }
        .btn-blue:disabled { background: #94a3b8; cursor: not-allowed; }
        
        .input-custom { background: #f1f5f9; border-radius: 18px; padding: 16px; width: 100%; font-size: 14px; border: 2px solid transparent; outline: none; transition: 0.3s; }
        .input-custom:focus { border-color: var(--primary-blue); background: var(--white); box-shadow: 0 0 0 4px rgba(58, 117, 196, 0.1); }

        #toast { position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%); z-index: 10000; background: var(--dark); color: var(--white); padding: 14px 28px; border-radius: 50px; font-size: 12px; font-weight: 700; display: none; text-align: center; min-width: 280px; box-shadow: 0 20px 40px rgba(0,0,0,0.3); }

        .nav-item { color: #94a3b8; transition: 0.3s; text-align: center; flex: 1; cursor: pointer; border: none; background: none; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        .nav-item.active { color: var(--primary-blue); }
        .nav-item.active i { transform: translateY(-2px); }

        .modal-full { position: fixed; inset: 0; background: var(--white); z-index: 9000; overflow-y: auto; display: none; }
        
        #splash-screen { position: fixed; inset: 0; background: var(--white); z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        
        .wallet-card { background: linear-gradient(135deg, #3a75c4 0%, #1e40af 100%); border-radius: 32px; padding: 24px; color: white; position: relative; overflow: hidden; }
        .wallet-card::before { content: ''; position: absolute; top: -20%; right: -10%; width: 150px; height: 150px; background: rgba(255,255,255,0.1); border-radius: 50%; }

        .transaction-item { display: flex; align-items: center; justify-content: space-between; padding: 12px; background: white; border-radius: 20px; border: 1px solid #f1f5f9; margin-bottom: 8px; }

        .avatar-option { width: 56px; height: 56px; border-radius: 16px; cursor: pointer; border: 3px solid transparent; transition: 0.2s; }
        .avatar-option.selected { border-color: var(--primary-blue); transform: scale(1.1); box-shadow: 0 10px 15px -3px rgba(58, 117, 196, 0.2); }

        ::-webkit-scrollbar { display: none; }
    </style>
</head>
<body class="pb-24">

    <div id="toast">Action effectuée</div>

    <!-- SplashScreen -->
    <div id="splash-screen">
        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-20 h-20 mb-6 animate-pulse" alt="Echoppe241">
        <p class="text-[11px] font-black uppercase tracking-[0.3em] text-blue-900 opacity-50">Sécurisation EchoppePay...</p>
    </div>

    <!-- Interface Auth (Identique mais simplifiée pour le focus) -->
    <div id="view-auth" class="view active min-h-screen flex items-center px-8 bg-white">
        <div class="w-full max-w-sm mx-auto text-center">
            <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-16 mx-auto mb-4">
            <h1 class="text-4xl font-black text-blue-900 tracking-tighter mb-10 italic">echoppe<span class="text-[#fcd116]">241</span></h1>
            
            <div id="login-form" class="space-y-4">
                <input type="email" id="login-email" class="input-custom" placeholder="Email">
                <input type="password" id="login-pass" class="input-custom" placeholder="Mot de passe">
                <button onclick="handleLogin()" id="btn-login" class="btn-blue shadow-xl shadow-blue-100">Accéder à mon compte</button>
                <button onclick="toggleAuth('register')" class="text-xs text-blue-600 font-bold uppercase tracking-widest mt-4">Nouveau ? Créer un compte</button>
            </div>

            <div id="register-form" class="space-y-4 hidden text-left">
                <input type="text" id="reg-name" class="input-custom" placeholder="Nom Complet">
                <input type="email" id="reg-email" class="input-custom" placeholder="Email">
                <input type="password" id="reg-pass" class="input-custom" placeholder="Mot de passe (6+ car.)">
                <button onclick="handleRegister()" id="btn-reg" class="btn-blue bg-green-600">S'inscrire gratuitement</button>
                <button onclick="toggleAuth('login')" class="w-full text-center text-xs text-slate-500 font-bold mt-4 uppercase">Retour à la connexion</button>
            </div>
        </div>
    </div>

    <!-- App Principale -->
    <div id="app-content" style="display:none">
        <!-- Header -->
        <header class="fixed top-0 inset-x-0 h-20 px-6 flex items-center justify-between bg-white/80 backdrop-blur-xl z-[500] border-b border-slate-100">
            <div class="flex items-center gap-2">
                <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center text-white text-xs font-black">E</div>
                <h1 class="text-lg font-black text-blue-900 italic">echoppe<span class="text-yellow-500">241</span></h1>
            </div>
            <div onclick="navigateTo('profile')" class="flex items-center gap-3 bg-slate-50 p-1.5 pr-4 rounded-2xl cursor-pointer active:scale-95 transition">
                <img id="header-avatar" class="w-8 h-8 rounded-xl object-cover">
                <p id="header-balance" class="text-xs font-black text-blue-900">0 F</p>
            </div>
        </header>

        <main class="max-w-xl mx-auto px-6 pt-24 pb-28">
            
            <!-- VUE : ACCUEIL / SHOP -->
            <div id="view-home" class="view active">
                <div class="flex items-center justify-between mb-6">
                    <h2 class="text-2xl font-black text-blue-900 italic">Découvrir.</h2>
                    <button onclick="openModal('publish-modal')" class="px-4 py-2 bg-blue-50 text-blue-600 text-[10px] font-black uppercase rounded-xl">+ Vendre</button>
                </div>
                <div id="product-list" class="grid grid-cols-2 gap-4"></div>
            </div>

            <!-- VUE : ECHOPPE PAY (PORTefeuille) -->
            <div id="view-wallet" class="view">
                <h2 class="text-2xl font-black text-blue-900 mb-6 italic">EchoppePay.</h2>
                
                <div class="wallet-card mb-8 shadow-2xl shadow-blue-200">
                    <div class="flex justify-between items-start mb-10">
                        <div>
                            <p class="text-[10px] font-black uppercase tracking-widest opacity-60">Solde Disponible</p>
                            <h3 id="wallet-balance" class="text-3xl font-black mt-1">0 FCFA</h3>
                        </div>
                        <img src="https://i.ibb.co/RkhcRdCs/echoppe241-logo.png" class="w-10 opacity-30 grayscale brightness-200">
                    </div>
                    <div class="grid grid-cols-2 gap-3">
                        <button onclick="openModal('recharge-modal')" class="bg-white/20 hover:bg-white/30 p-3 rounded-2xl text-[10px] font-black uppercase transition flex items-center justify-center gap-2"><i class="fa-solid fa-plus-circle"></i> Recharger</button>
                        <button onclick="openModal('transfer-modal')" class="bg-white/20 hover:bg-white/30 p-3 rounded-2xl text-[10px] font-black uppercase transition flex items-center justify-center gap-2"><i class="fa-solid fa-paper-plane"></i> Transférer</button>
                    </div>
                </div>

                <div class="mb-6">
                    <h4 class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-4">Transactions Récentes</h4>
                    <div id="wallet-history" class="space-y-2">
                        <!-- Historique financier ici -->
                    </div>
                </div>
            </div>

            <!-- VUE : MESSAGES -->
            <div id="view-messages" class="view">
                <h2 class="text-2xl font-black text-blue-900 mb-6 italic">Messages.</h2>
                <div id="conversations-list" class="space-y-3"></div>
            </div>

            <!-- VUE : PROFIL -->
            <div id="view-profile" class="view">
                <div class="card-neo p-8 text-center mb-6">
                    <div class="relative w-24 h-24 mx-auto mb-4">
                        <img id="p-avatar" class="w-24 h-24 rounded-[32px] object-cover border-4 border-slate-50 shadow-xl">
                        <div id="p-badge-admin" class="hidden absolute -bottom-2 -right-2 bg-red-600 text-white text-[8px] font-black px-2 py-1 rounded-lg border-2 border-white">ADMIN</div>
                    </div>
                    <h3 id="p-name" class="font-black text-xl text-blue-900">...</h3>
                    <p id="p-email" class="text-[10px] text-slate-400 font-bold uppercase tracking-[0.2em] mt-1">...</p>
                    
                    <div class="flex gap-2 mt-6">
                        <button onclick="openModal('edit-profile-modal')" class="flex-1 p-3 bg-slate-50 text-blue-900 text-[10px] font-black uppercase rounded-xl border border-slate-100">Éditer Profil</button>
                        <button onclick="handleLogout()" class="w-12 h-12 flex items-center justify-center bg-red-50 text-red-500 rounded-xl"><i class="fa-solid fa-power-off"></i></button>
                    </div>
                </div>

                <div class="space-y-3">
                    <div class="p-4 bg-white border border-slate-100 rounded-2xl flex items-center justify-between">
                        <div>
                            <p class="text-[8px] font-black text-slate-400 uppercase">Identifiant EchoppePay</p>
                            <p id="p-uid" class="text-[10px] font-mono font-bold text-blue-600">...</p>
                        </div>
                        <button onclick="copyUID()" class="text-blue-500"><i class="fa-solid fa-copy"></i></button>
                    </div>
                    
                    <div id="admin-section" class="hidden">
                        <h4 class="text-[10px] font-black text-red-500 uppercase tracking-widest mb-3 mt-6">Administration</h4>
                        <div id="admin-tasks-list" class="space-y-2"></div>
                    </div>
                </div>
            </div>
        </main>

        <!-- Barre de Navigation -->
        <nav class="fixed bottom-0 inset-x-0 h-22 bg-white/90 backdrop-blur-2xl border-t border-slate-100 flex items-center justify-around z-[500] pb-6 px-4">
            <button onclick="navigateTo('home')" id="nav-home" class="nav-item active"><i class="fa-solid fa-house-chimney text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Shop</p></button>
            <button onclick="navigateTo('wallet')" id="nav-wallet" class="nav-item"><i class="fa-solid fa-wallet text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Pay</p></button>
            <button onclick="navigateTo('messages')" id="nav-messages" class="nav-item"><i class="fa-solid fa-comment-dots text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Chat</p></button>
            <button onclick="navigateTo('profile')" id="nav-profile" class="nav-item"><i class="fa-solid fa-circle-user text-xl"></i><p class="text-[8px] font-black mt-1 uppercase">Profil</p></button>
        </nav>
    </div>

    <!-- MODALE : TRANSFERT (ECHOPPEPAY) -->
    <div id="transfer-modal" class="modal-full p-8">
        <div class="max-w-md mx-auto">
            <h2 class="text-2xl font-black text-blue-900 mb-2 italic">Envoyer Argent.</h2>
            <p class="text-xs text-slate-400 mb-8 font-medium">Transférez instantanément vers un autre compte Echoppe241.</p>
            
            <div class="space-y-4">
                <div>
                    <p class="text-[9px] font-black text-slate-400 uppercase mb-2">ID du bénéficiaire (ou Email)</p>
                    <input type="text" id="trans-dest" class="input-custom" placeholder="Ex: 8Xy2... ou email@mail.com">
                </div>
                <div>
                    <p class="text-[9px] font-black text-slate-400 uppercase mb-2">Montant à envoyer (FCFA)</p>
                    <input type="number" id="trans-amount" class="input-custom" placeholder="0">
                </div>
                <button onclick="processTransfer()" id="btn-transfer" class="btn-blue mt-4">Confirmer l'envoi</button>
                <button onclick="closeModal('transfer-modal')" class="w-full py-4 text-xs font-bold text-slate-400 uppercase tracking-widest">Annuler</button>
            </div>
        </div>
    </div>

    <!-- MODALE : RECHARGE (CARTE/CODE) -->
    <div id="recharge-modal" class="modal-full p-8">
        <div class="max-w-md mx-auto text-center">
            <h2 class="text-2xl font-black text-blue-900 mb-6 italic">Rechargement.</h2>
            <div class="bg-blue-50 p-6 rounded-3xl mb-8 border-2 border-dashed border-blue-200">
                <p class="text-xs text-blue-800 font-bold mb-4">Saisissez le code de votre coupon de recharge Echoppe241 ou l'ID de transaction Airtel Money.</p>
                <input type="text" id="rech-code" class="input-custom text-center !bg-white uppercase font-black tracking-widest" placeholder="CODE-241-XXXX">
            </div>
            <button onclick="processRecharge()" id="btn-recharge" class="btn-blue">Valider le coupon</button>
            <button onclick="closeModal('recharge-modal')" class="w-full py-4 text-xs font-bold text-slate-400 mt-4 uppercase">Fermer</button>
        </div>
    </div>

    <!-- MODALE : EDIT PROFIL -->
    <div id="edit-profile-modal" class="modal-full p-8">
        <div class="max-w-md mx-auto">
            <h2 class="text-2xl font-black text-blue-900 mb-8 italic">Édition Profil.</h2>
            <div class="space-y-6">
                <div>
                    <p class="text-[9px] font-black text-slate-400 uppercase mb-3">Changer mon nom</p>
                    <input type="text" id="edit-name" class="input-custom">
                </div>
                <div>
                    <p class="text-[9px] font-black text-slate-400 uppercase mb-3">Avatar Premium</p>
                    <div id="avatar-list" class="flex flex-wrap gap-3"></div>
                </div>
                <button onclick="saveProfile()" id="btn-save-profile" class="btn-blue">Mettre à jour</button>
                <button onclick="closeModal('edit-profile-modal')" class="w-full py-4 text-xs font-bold text-slate-400 uppercase">Retour</button>
            </div>
        </div>
    </div>

    <!-- MODALE : CHAT (INDISPENSABLE) -->
    <div id="chat-modal" class="modal-full !p-0">
        <div class="flex flex-col h-full bg-slate-50">
            <div class="p-4 bg-white border-b flex items-center justify-between sticky top-0 z-50">
                <div class="flex items-center gap-3">
                    <button onclick="closeModal('chat-modal')" class="w-10 h-10 bg-slate-50 rounded-xl flex items-center justify-center"><i class="fa-solid fa-chevron-left"></i></button>
                    <div>
                        <h3 id="chat-user-name" class="font-black text-blue-900 text-sm">...</h3>
                        <p class="text-[8px] text-green-500 font-black uppercase">En ligne (Sécurisé)</p>
                    </div>
                </div>
                <button onclick="openModal('create-bon-modal')" class="bg-blue-600 text-white text-[9px] font-black px-4 py-2 rounded-xl">CRÉER UN BON</button>
            </div>
            <div id="chat-messages" class="flex-1 overflow-y-auto p-4 flex flex-col gap-3"></div>
            <div class="p-4 bg-white border-t flex gap-2">
                <input type="text" id="chat-input" class="input-custom !py-3 flex-1" placeholder="Écrire...">
                <button onclick="sendChatMessage()" class="w-12 h-12 bg-blue-600 text-white rounded-xl shadow-lg flex items-center justify-center"><i class="fa-solid fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <!-- MODALE : CRÉER UN BON -->
    <div id="create-bon-modal" class="modal-full p-8">
        <div class="max-w-md mx-auto">
            <h2 class="text-2xl font-black text-blue-900 mb-6 italic">Générer Bon.</h2>
            <div class="space-y-4">
                <input type="text" id="bon-title" class="input-custom" placeholder="Nom du produit/service">
                <input type="number" id="bon-price" class="input-custom" placeholder="Prix final (FCFA)">
                <button onclick="submitBon()" id="btn-bon" class="btn-blue">Envoyer au client</button>
                <button onclick="closeModal('create-bon-modal')" class="w-full py-4 text-xs font-bold text-slate-400 uppercase">Annuler</button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut, signInWithCustomToken, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, updateDoc, collection, addDoc, query, where, orderBy, serverTimestamp, increment, runTransaction, getDoc, getDocs } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // INIT FIREBASE
        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'echoppe241-pro-v2';

        let user = null;
        let userData = null;
        let activeChatId = null;

        // AUTH LISTENER
        onAuthStateChanged(auth, async (u) => {
            if (u) {
                user = u;
                // Ecouteur profil temps réel
                onSnapshot(doc(db, 'artifacts', appId, 'users', u.uid), (snap) => {
                    if (snap.exists()) {
                        userData = snap.data();
                        document.getElementById('view-auth').classList.remove('active');
                        document.getElementById('app-content').style.display = 'block';
                        updateGlobalUI();
                        initListeners();
                        initAvatarList();
                    } else {
                        // Init nouveau profil
                        const name = localStorage.getItem('tmp_name') || 'Echoppe User';
                        setDoc(doc(db, 'artifacts', appId, 'users', u.uid), {
                            fullName: name, email: u.email, walletBalance: 0, 
                            avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${u.uid}`,
                            isAdmin: false, createdAt: serverTimestamp()
                        });
                    }
                });
            } else {
                user = null;
                document.getElementById('app-content').style.display = 'none';
                document.getElementById('view-auth').classList.add('active');
            }
            hideSplash();
        });

        // UI UPDATES
        function updateGlobalUI() {
            if(!userData) return;
            document.getElementById('header-balance').innerText = `${userData.walletBalance.toLocaleString()} F`;
            document.getElementById('wallet-balance').innerText = `${userData.walletBalance.toLocaleString()} FCFA`;
            document.getElementById('header-avatar').src = userData.avatar;
            document.getElementById('p-avatar').src = userData.avatar;
            document.getElementById('p-name').innerText = userData.fullName;
            document.getElementById('p-email').innerText = userData.email;
            document.getElementById('p-uid').innerText = user.uid;
            document.getElementById('edit-name').value = userData.fullName;

            if(userData.isAdmin) {
                document.getElementById('p-badge-admin').classList.remove('hidden');
                document.getElementById('admin-section').classList.remove('hidden');
            }
        }

        // LISTENERS TEMPS RÉEL
        function initListeners() {
            if(!user) return;

            // Marché Public
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'products'), (snap) => {
                const list = document.getElementById('product-list');
                list.innerHTML = snap.docs.map(d => {
                    const p = d.data();
                    return `<div onclick="startConversation('${p.sellerId}', '${p.sellerName}')" class="card-neo overflow-hidden active:scale-95 transition cursor-pointer">
                        <div class="h-32 bg-slate-100 bg-cover bg-center" style="background-image:url('${p.image || ''}')"></div>
                        <div class="p-4">
                            <p class="text-[10px] font-black text-blue-900 uppercase truncate">${p.name}</p>
                            <p class="text-blue-600 font-bold text-xs mt-1">${p.price.toLocaleString()} F</p>
                        </div>
                    </div>`;
                }).join('');
            });

            // EchoppePay History (Privé)
            onSnapshot(collection(db, 'artifacts', appId, 'users', user.uid, 'history'), (snap) => {
                const list = document.getElementById('wallet-history');
                const items = snap.docs.map(d => d.data()).sort((a,b) => (b.timestamp?.seconds || 0) - (a.timestamp?.seconds || 0));
                if(items.length === 0) list.innerHTML = `<p class='text-[10px] italic text-slate-400'>Aucun mouvement.</p>`;
                else list.innerHTML = items.map(t => `
                    <div class="transaction-item">
                        <div class="flex items-center gap-3">
                            <div class="w-9 h-9 rounded-xl flex items-center justify-center ${t.amount > 0 ? 'bg-green-50 text-green-600' : 'bg-red-50 text-red-600'}">
                                <i class="fa-solid ${t.amount > 0 ? 'fa-arrow-down-left' : 'fa-arrow-up-right'} text-xs"></i>
                            </div>
                            <div>
                                <p class="text-[11px] font-black text-blue-900">${t.label}</p>
                                <p class="text-[8px] text-slate-400 uppercase font-bold">${t.timestamp ? new Date(t.timestamp.seconds*1000).toLocaleDateString() : 'Instant'}</p>
                            </div>
                        </div>
                        <p class="text-xs font-black ${t.amount > 0 ? 'text-green-600' : 'text-red-600'}">${t.amount > 0 ? '+' : ''}${t.amount.toLocaleString()} F</p>
                    </div>
                `).join('');
            });

            // Admin Tasks
            if(userData?.isAdmin) {
                onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'), (snap) => {
                    const list = document.getElementById('admin-tasks-list');
                    const pending = snap.docs.filter(d => d.data().status === 'pending');
                    list.innerHTML = pending.map(d => {
                        const t = d.data();
                        return `<div class="bg-white p-4 rounded-2xl border-2 border-slate-50 flex items-center justify-between">
                            <div>
                                <p class="text-[8px] font-black text-red-500 uppercase">${t.type}</p>
                                <p class="text-[10px] font-bold text-blue-900">${t.userName}</p>
                                <p class="text-[12px] font-black text-blue-600">${t.amount?.toLocaleString()} F</p>
                            </div>
                            <div class="flex gap-2">
                                <button onclick="adminAction('${d.id}', true)" class="w-8 h-8 bg-green-500 text-white rounded-lg"><i class="fa-solid fa-check"></i></button>
                                <button onclick="adminAction('${d.id}', false)" class="w-8 h-8 bg-slate-200 text-slate-500 rounded-lg"><i class="fa-solid fa-xmark"></i></button>
                            </div>
                        </div>`;
                    }).join('');
                });
            }
        }

        // --- ECHOPPE PAY FUNCTIONS ---

        window.processTransfer = async () => {
            const destId = document.getElementById('trans-dest').value.trim();
            const amount = parseInt(document.getElementById('trans-amount').value);
            const btn = document.getElementById('btn-transfer');

            if(!destId || isNaN(amount) || amount <= 0) return showToast("Données invalides");
            if(userData.walletBalance < amount) return showToast("Solde insuffisant");
            if(destId === user.uid) return showToast("Impossible vers soi-même");

            btn.disabled = true;
            try {
                // Tentative de trouver par Email si ce n'est pas un UID
                let targetUID = destId;
                if(destId.includes('@')) {
                    const usersRef = collection(db, 'artifacts', appId, 'users');
                    const q = query(usersRef, where("email", "==", destId));
                    const qs = await getDocs(q);
                    if(qs.empty) throw new Error("Utilisateur non trouvé");
                    targetUID = qs.docs[0].id;
                }

                await runTransaction(db, async (transaction) => {
                    const fromRef = doc(db, 'artifacts', appId, 'users', user.uid);
                    const toRef = doc(db, 'artifacts', appId, 'users', targetUID);
                    
                    const toSnap = await transaction.get(toRef);
                    if(!toSnap.exists()) throw new Error("Destinataire inexistant");

                    transaction.update(fromRef, { walletBalance: increment(-amount) });
                    transaction.update(toRef, { walletBalance: increment(amount) });

                    // Historiques
                    const hFrom = doc(collection(db, 'artifacts', appId, 'users', user.uid, 'history'));
                    const hTo = doc(collection(db, 'artifacts', appId, 'users', targetUID, 'history'));
                    
                    transaction.set(hFrom, { amount: -amount, label: `Transfert vers ${toSnap.data().fullName}`, timestamp: serverTimestamp() });
                    transaction.set(hTo, { amount: amount, label: `Reçu de ${userData.fullName}`, timestamp: serverTimestamp() });
                });
                showToast("Transfert EchoppePay réussi !");
                closeModal('transfer-modal');
            } catch (e) {
                showToast(e.message || "Erreur de transfert");
            } finally {
                btn.disabled = false;
            }
        };

        window.processRecharge = async () => {
            const code = document.getElementById('rech-code').value.trim();
            if(!code) return showToast("Entrez un code");
            
            showToast("Demande soumise à l'admin...");
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'), {
                type: 'RECHARGE', userId: user.uid, userName: userData.fullName, 
                amount: 5000, reference: code, status: 'pending', timestamp: serverTimestamp()
            });
            closeModal('recharge-modal');
        };

        // --- PROFILE FUNCTIONS ---

        window.initAvatarList = () => {
            const box = document.getElementById('avatar-list');
            box.innerHTML = "";
            ['Gamer', 'Office', 'Fun', 'Creative', 'Tech', 'Music'].forEach(seed => {
                const url = `https://api.dicebear.com/7.x/avataaars/svg?seed=${seed}`;
                const img = document.createElement('img');
                img.src = url;
                img.className = `avatar-option ${userData.avatar === url ? 'selected' : ''}`;
                img.onclick = () => {
                    document.querySelectorAll('.avatar-option').forEach(el => el.classList.remove('selected'));
                    img.classList.add('selected');
                    userData.tmp_avatar = url;
                };
                box.appendChild(img);
            });
        };

        window.saveProfile = async () => {
            const name = document.getElementById('edit-name').value;
            const avatar = userData.tmp_avatar || userData.avatar;
            await updateDoc(doc(db, 'artifacts', appId, 'users', user.uid), { fullName: name, avatar: avatar });
            showToast("Profil mis à jour !");
            closeModal('edit-profile-modal');
        };

        // --- CHAT & BONS ---

        window.startConversation = async (otherId, otherName) => {
            if(otherId === user.uid) return showToast("C'est votre article");
            const cid = [user.uid, otherId].sort().join('_');
            activeChatId = cid;
            
            await setDoc(doc(db, 'artifacts', appId, 'public', 'data', 'chats', cid), {
                participants: [user.uid, otherId],
                names: { [user.uid]: userData.fullName, [otherId]: otherName },
                lastUpdate: serverTimestamp()
            }, { merge: true });

            openChatUI(cid, otherName);
        };

        window.openChatUI = (cid, name) => {
            activeChatId = cid;
            document.getElementById('chat-user-name').innerText = name;
            document.getElementById('chat-modal').style.display = 'block';
            
            onSnapshot(collection(db, 'artifacts', appId, 'public', 'data', 'chats', cid, 'messages'), (snap) => {
                const box = document.getElementById('chat-messages');
                const msgs = snap.docs.map(d => ({id: d.id, ...d.data()})).sort((a,b) => (a.timestamp?.seconds || 0) - (b.timestamp?.seconds || 0));
                box.innerHTML = msgs.map(m => {
                    const isMe = m.senderId === user.uid;
                    if(m.type === 'bon') {
                        return `<div class="bg-white p-5 rounded-3xl border-2 border-blue-600 shadow-xl max-w-[90%] ${isMe ? 'self-end' : 'self-start'}">
                            <p class="text-[8px] font-black text-blue-600 uppercase mb-2">BON DE PAIEMENT</p>
                            <p class="text-sm font-bold text-blue-900">${m.title}</p>
                            <p class="text-xl font-black text-blue-600 my-2">${m.price.toLocaleString()} F</p>
                            <div class="pt-3 border-t flex justify-between items-center">
                                <span class="text-[9px] font-black uppercase ${m.status==='paid'?'text-green-500':'text-slate-400'}">${m.status==='paid'?'Payé':'En attente'}</span>
                                ${(!isMe && m.status === 'pending') ? `<button onclick="payBon('${cid}','${m.id}',${m.price},'${m.senderId}')" class="bg-blue-600 text-white text-[9px] font-black px-4 py-2 rounded-xl">PAYER</button>` : ''}
                            </div>
                        </div>`;
                    }
                    return `<div class="max-w-[80%] p-4 rounded-2xl text-sm ${isMe ? 'bg-blue-600 text-white self-end rounded-tr-none' : 'bg-white text-blue-900 self-start rounded-tl-none'}">${m.text}</div>`;
                }).join('');
                box.scrollTop = box.scrollHeight;
            });
        };

        window.sendChatMessage = async () => {
            const input = document.getElementById('chat-input');
            if(!input.value.trim()) return;
            await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'chats', activeChatId, 'messages'), {
                senderId: user.uid, text: input.value, type: 'text', timestamp: serverTimestamp()
            });
            input.value = "";
        };

        window.payBon = async (cid, mid, amount, sellerId) => {
            if(userData.walletBalance < amount) return showToast("Solde insuffisant");
            try {
                await runTransaction(db, async (transaction) => {
                    const uRef = doc(db, 'artifacts', appId, 'users', user.uid);
                    const mRef = doc(db, 'artifacts', appId, 'public', 'data', 'chats', cid, 'messages', mid);
                    transaction.update(uRef, { walletBalance: increment(-amount) });
                    transaction.update(mRef, { status: 'paid' });
                    // Tâche Admin pour escrow
                    const tRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'admin_tasks'));
                    transaction.set(tRef, { type: 'ESCROW_PAY', userId: user.uid, userName: userData.fullName, sellerId: sellerId, amount: amount, status: 'pending', timestamp: serverTimestamp() });
                });
                showToast("Payé ! L'admin valide la transaction.");
            } catch(e) { showToast("Erreur paiement"); }
        };

        // --- AUTH & HELPERS ---

        window.handleLogin = () => {
            const e = document.getElementById('login-email').value;
            const p = document.getElementById('login-pass').value;
            signInWithEmailAndPassword(auth, e, p).catch(() => showToast("Erreur connexion"));
        };

        window.handleRegister = () => {
            const n = document.getElementById('reg-name').value;
            const e = document.getElementById('reg-email').value;
            const p = document.getElementById('reg-pass').value;
            if(p.length < 6) return showToast("Pass trop court");
            localStorage.setItem('tmp_name', n);
            createUserWithEmailAndPassword(auth, e, p).catch(() => showToast("Erreur création"));
        };

        window.handleLogout = () => signOut(auth);

        window.navigateTo = (v) => {
            document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
            document.getElementById(`view-${v}`).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(`nav-${v}`).classList.add('active');
        };

        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.showToast = (m) => {
            const t = document.getElementById('toast'); t.innerText = m; t.style.display = 'block';
            setTimeout(() => t.style.display = 'none', 3000);
        };
        window.copyUID = () => {
            const el = document.createElement('textarea'); el.value = user.uid; document.body.appendChild(el);
            el.select(); document.execCommand('copy'); document.body.removeChild(el);
            showToast("ID Copié !");
        };
        function hideSplash() { setTimeout(() => { const s = document.getElementById('splash-screen'); if(s){ s.style.opacity='0'; setTimeout(()=>s.style.display='none',500); } }, 1200); }
    </script>
</body>
</html>
