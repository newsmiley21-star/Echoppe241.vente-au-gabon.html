<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Facebook 241 - Live Production</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Segoe+UI:wght@400;600;700&display=swap');
        
        :root {
            --fb-blue: #1877F2;
            --fb-bg: #F0F2F5;
            --fb-white: #FFFFFF;
            --fb-text: #1C1E21;
            --fb-gray: #65676B;
        }

        .dark {
            --fb-bg: #18191A;
            --fb-white: #242526;
            --fb-text: #E4E6EB;
            --fb-gray: #B0B3B8;
        }

        body {
            font-family: 'Segoe UI', Helvetica, Arial, sans-serif;
            background-color: var(--fb-bg);
            color: var(--fb-text);
            transition: background-color 0.3s ease;
        }

        .fb-card {
            background-color: var(--fb-white);
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1), 0 8px 16px rgba(0, 0, 0, 0.1);
        }

        .login-input {
            border: 1px solid #dddfe2;
            border-radius: 6px;
            padding: 14px 16px;
            font-size: 17px;
            width: 100%;
            outline-color: var(--fb-blue);
        }

        .btn-green {
            background-color: #42b72a;
            color: white;
            font-weight: bold;
            transition: filter 0.2s;
        }

        .btn-green:hover { filter: brightness(0.9); }

        #auth-container, #app-container { display: none; }

        .loading-dots:after {
            content: '.';
            animation: dots 1.5s steps(5, end) infinite;
        }

        @keyframes dots {
            0%, 20% { content: '.'; }
            40% { content: '..'; }
            60% { content: '...'; }
            80%, 100% { content: ''; }
        }
    </style>
</head>
<body class="transition-colors duration-200">

    <!-- Splash Screen -->
    <div id="splash-screen" class="fixed inset-0 z-[1000] bg-white dark:bg-[#18191A] flex flex-col items-center justify-center">
        <div class="w-20 h-20 bg-[#1877F2] rounded-3xl flex items-center justify-center mb-6 shadow-xl animate-pulse">
            <span class="text-white font-bold text-3xl italic">241</span>
        </div>
        <div class="text-gray-400 font-medium">Initialisation Cloud<span class="loading-dots"></span></div>
    </div>

    <!-- Interface AUTH -->
    <div id="auth-container" class="min-h-screen flex flex-col items-center justify-center p-4 bg-[#F0F2F5] dark:bg-[#18191A]">
        <div class="max-w-[1000px] w-full flex flex-col lg:flex-row items-center gap-10 lg:gap-20">
            <div class="text-center lg:text-left flex-1">
                <h1 class="text-[#1877F2] text-5xl md:text-6xl font-bold mb-4 tracking-tighter">facebook 241</h1>
                <p class="text-2xl font-normal leading-tight text-gray-700 dark:text-gray-300">Connectez-vous à la communauté du Gabon.</p>
            </div>

            <div class="w-full max-w-[400px]">
                <div class="fb-card p-4 dark:bg-[#242526]">
                    <form id="login-form" class="space-y-4">
                        <input type="email" id="login-email" placeholder="Adresse e-mail" class="login-input dark:bg-[#3A3B3C] dark:border-none dark:text-white" required>
                        <input type="password" id="login-pass" placeholder="Mot de passe" class="login-input dark:bg-[#3A3B3C] dark:border-none dark:text-white" required>
                        <button type="submit" id="login-submit-btn" class="w-full bg-[#1877F2] text-white text-xl font-bold py-3 rounded-md hover:bg-blue-600 transition-colors">Se connecter</button>
                        <hr class="border-gray-300 dark:border-gray-700">
                        <div class="flex justify-center pt-2">
                            <button type="button" onclick="showSignup()" class="btn-green px-4 py-3 rounded-md text-lg">Créer nouveau compte</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal INSCRIPTION -->
    <div id="signup-modal" class="fixed inset-0 bg-white/80 dark:bg-black/80 backdrop-blur-sm z-[500] hidden items-center justify-center p-4">
        <div class="w-full max-w-[432px] fb-card dark:bg-[#242526]">
            <div class="p-4 border-b border-gray-300 dark:border-gray-700 flex justify-between items-start">
                <div>
                    <h2 class="text-3xl font-bold">S'inscrire</h2>
                    <p class="text-gray-500 text-sm">C'est rapide et facile.</p>
                </div>
                <button onclick="hideSignup()" class="text-gray-500 text-2xl"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <form id="signup-form" class="p-4 space-y-3">
                <div class="flex gap-3">
                    <input type="text" id="reg-prenom" placeholder="Prénom" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                    <input type="text" id="reg-nom" placeholder="Nom de famille" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                </div>
                <input type="email" id="reg-email" placeholder="Numéro mobile ou e-mail" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                <input type="password" id="reg-pass" placeholder="Nouveau mot de passe" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                <div class="flex justify-center pt-4">
                    <button type="submit" id="signup-submit-btn" class="btn-green px-12 py-2 rounded-md text-lg min-w-[194px]">S'inscrire</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Interface APPLICATION -->
    <div id="app-container">
        <header class="h-14 bg-white dark:bg-[#242526] border-b border-gray-300 dark:border-gray-700 flex items-center justify-between px-4 sticky top-0 z-[100] shadow-sm">
            <div class="flex items-center gap-2">
                <div class="w-10 h-10 bg-[#1877F2] rounded-full flex items-center justify-center shadow-md">
                    <span class="text-white font-black text-2xl italic">f</span>
                </div>
                <span class="font-bold text-xl text-[#1877F2] hidden sm:block">241</span>
            </div>
            <div class="flex items-center gap-3">
                <button onclick="toggleDarkMode()" class="w-10 h-10 rounded-full bg-gray-100 dark:bg-[#3A3B3C] flex items-center justify-center"><i id="theme-icon" class="fa-solid fa-moon"></i></button>
                <div class="flex items-center gap-2 bg-gray-100 dark:bg-[#3A3B3C] p-1.5 rounded-full pr-4">
                    <img id="user-avatar" src="" class="w-7 h-7 rounded-full bg-white" alt="me">
                    <span id="user-name-display" class="text-sm font-bold truncate max-w-[100px]"></span>
                </div>
                <button onclick="logout()" class="text-gray-500 hover:text-red-500 text-lg ml-2"><i class="fa-solid fa-right-from-bracket"></i></button>
            </div>
        </header>

        <main class="max-w-[680px] mx-auto p-4 space-y-4 pb-24">
            <div class="fb-card p-4 dark:bg-[#242526]">
                <div class="flex gap-3 items-center">
                    <img id="user-avatar-post" src="" class="w-10 h-10 rounded-full bg-white border" alt="">
                    <div onclick="openModal()" class="flex-1 bg-gray-100 dark:bg-[#3A3B3C] rounded-full px-4 py-2.5 text-gray-500 cursor-pointer hover:bg-gray-200">
                        Quoi de neuf, <span id="user-first-name"></span> ?
                    </div>
                </div>
            </div>
            <div id="posts-container" class="space-y-4"></div>
        </main>
    </div>

    <!-- Modal Publication -->
    <div id="post-modal" class="fixed inset-0 bg-white/80 dark:bg-black/80 backdrop-blur-md z-[600] hidden items-center justify-center p-4">
        <div class="w-full max-w-[500px] fb-card dark:bg-[#242526] p-4">
             <div class="flex justify-between mb-4 border-b pb-2 dark:border-gray-700">
                <h3 class="font-bold text-lg">Créer une publication</h3>
                <button onclick="closeModal()"><i class="fa-solid fa-xmark text-xl text-gray-500"></i></button>
             </div>
             <textarea id="post-input" class="w-full h-32 bg-transparent outline-none text-lg resize-none" placeholder="Partagez quelque chose avec le Gabon..."></textarea>
             <button onclick="createPost()" class="w-full bg-[#1877F2] text-white py-2 rounded-lg font-bold mt-4 disabled:opacity-50" id="publish-btn" disabled>Publier</button>
        </div>
    </div>

    <!-- Scripts Firebase -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, onAuthStateChanged, signOut, updateProfile } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, onSnapshot, serverTimestamp, doc, setDoc, getDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // Votre configuration Firebase
        const firebaseConfig = {
            apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
            authDomain: "communautedugabon.firebaseapp.com",
            projectId: "communautedugabon",
            storageBucket: "communautedugabon.firebasestorage.app",
            messagingSenderId: "647862371022",
            appId: "1:647862371022:web:b209bfc8eb81accb1fc69f",
            measurementId: "G-T7415FPS91"
        };

        // Initialisation
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'facebook241-prod';

        let userProfile = null;

        // --- AUTHENTIFICATION ---
        onAuthStateChanged(auth, async (user) => {
            const splash = document.getElementById('splash-screen');
            if (user) {
                // Récupérer les infos de profil depuis Firestore
                const userDoc = await getDoc(doc(db, 'artifacts', appId, 'public', 'users', user.uid));
                userProfile = userDoc.exists() ? userDoc.data() : { prenom: user.displayName || 'Utilisateur', nom: '' };
                
                document.getElementById('auth-container').style.display = 'none';
                document.getElementById('app-container').style.display = 'block';
                updateUI(userProfile);
                listenForPosts();
            } else {
                document.getElementById('auth-container').style.display = 'flex';
                document.getElementById('app-container').style.display = 'none';
            }
            splash.style.opacity = '0';
            setTimeout(() => splash.classList.add('hidden'), 500);
        });

        // Connexion
        document.getElementById('login-form').onsubmit = async (e) => {
            e.preventDefault();
            const email = document.getElementById('login-email').value;
            const pass = document.getElementById('login-pass').value;
            const btn = document.getElementById('login-submit-btn');
            
            try {
                btn.disabled = true;
                btn.innerText = "Connexion...";
                await signInWithEmailAndPassword(auth, email, pass);
            } catch (err) {
                alert("Erreur: " + err.message);
                btn.disabled = false;
                btn.innerText = "Se connecter";
            }
        };

        // Inscription
        document.getElementById('signup-form').onsubmit = async (e) => {
            e.preventDefault();
            const prenom = document.getElementById('reg-prenom').value;
            const nom = document.getElementById('reg-nom').value;
            const email = document.getElementById('reg-email').value;
            const pass = document.getElementById('reg-pass').value;
            const btn = document.getElementById('signup-submit-btn');

            try {
                btn.disabled = true;
                const cred = await createUserWithEmailAndPassword(auth, email, pass);
                // Sauvegarder les infos supp dans Firestore
                await setDoc(doc(db, 'artifacts', appId, 'public', 'users', cred.user.uid), {
                    prenom, nom, email, uid: cred.user.uid
                });
                await updateProfile(cred.user, { displayName: prenom });
                window.location.reload();
            } catch (err) {
                alert("Erreur d'inscription: " + err.message);
                btn.disabled = false;
            }
        };

        window.logout = () => signOut(auth);

        // --- POSTS (FIRESTORE) ---
        async function createPost() {
            const content = document.getElementById('post-input').value;
            if (!content.trim() || !auth.currentUser) return;

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'posts'), {
                    author: userProfile.prenom + " " + userProfile.nom,
                    authorId: auth.currentUser.uid,
                    content: content,
                    createdAt: serverTimestamp(),
                    likes: 0
                });
                closeModal();
                document.getElementById('post-input').value = "";
            } catch (err) {
                console.error(err);
            }
        }
        window.createPost = createPost;

        function listenForPosts() {
            const q = query(collection(db, 'artifacts', appId, 'public', 'data', 'posts'));
            onSnapshot(q, (snapshot) => {
                const posts = snapshot.docs.map(doc => ({id: doc.id, ...doc.data()}))
                    .sort((a, b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0));
                
                renderPosts(posts);
            }, (err) => console.error("Firestore Error:", err));
        }

        function renderPosts(posts) {
            const container = document.getElementById('posts-container');
            container.innerHTML = posts.map(post => `
                <div class="fb-card p-4 dark:bg-[#242526] animate-in fade-in">
                    <div class="flex gap-3 mb-3">
                        <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=${post.author}" class="w-10 h-10 rounded-full border bg-white">
                        <div>
                            <p class="font-bold text-sm">${post.author}</p>
                            <p class="text-xs text-gray-500">${post.createdAt ? new Date(post.createdAt.seconds * 1000).toLocaleString() : 'À l\'instant'}</p>
                        </div>
                    </div>
                    <p class="text-base mb-3">${post.content}</p>
                    <hr class="border-gray-200 dark:border-gray-700 mb-2">
                    <div class="flex justify-around">
                        <button class="flex-1 py-1 hover:bg-gray-100 dark:hover:bg-[#3A3B3C] rounded text-gray-500">
                            <i class="fa-solid fa-thumbs-up mr-2"></i> J'aime
                        </button>
                        <button class="flex-1 py-1 hover:bg-gray-100 dark:hover:bg-[#3A3B3C] rounded text-gray-500">
                            <i class="fa-solid fa-comment mr-2"></i> Commenter
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function updateUI(profile) {
            const avatar = `https://api.dicebear.com/7.x/avataaars/svg?seed=${profile.prenom}`;
            document.getElementById('user-avatar').src = avatar;
            document.getElementById('user-avatar-post').src = avatar;
            document.getElementById('user-name-display').textContent = `${profile.prenom} ${profile.nom}`;
            document.getElementById('user-first-name').textContent = profile.prenom;
        }

        // Helpers UI
        window.showSignup = () => document.getElementById('signup-modal').style.display = 'flex';
        window.hideSignup = () => document.getElementById('signup-modal').style.display = 'none';
        window.openModal = () => {
            document.getElementById('post-modal').style.display = 'flex';
            document.getElementById('post-input').focus();
        };
        window.closeModal = () => document.getElementById('post-modal').style.display = 'none';
        window.toggleDarkMode = () => {
            document.documentElement.classList.toggle('dark');
            const icon = document.getElementById('theme-icon');
            icon.className = document.documentElement.classList.contains('dark') ? 'fa-solid fa-sun' : 'fa-solid fa-moon';
        };

        document.getElementById('post-input').oninput = (e) => {
            document.getElementById('publish-btn').disabled = !e.target.value.trim();
        };
    </script>
</body>
</html>
