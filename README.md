<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>241 - Communauté Gabon</title>
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
            overflow-x: hidden;
        }

        .fb-card {
            background-color: var(--fb-white);
            border-radius: 8px;
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
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

        .post-enter { animation: slideUp 0.4s ease-out; }
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #bcc0c4; border-radius: 10px; }
        .dark ::-webkit-scrollbar-thumb { background: #3e4042; }

        .comment-input {
            background-color: #F0F2F5;
            border-radius: 20px;
            padding: 8px 12px;
            font-size: 14px;
        }
        .dark .comment-input { background-color: #3A3B3C; }
    </style>
</head>
<body class="transition-colors duration-200">

    <!-- Splash Screen -->
    <div id="splash-screen" class="fixed inset-0 z-[1000] bg-white dark:bg-[#18191A] flex flex-col items-center justify-center">
        <div class="w-20 h-20 bg-[#1877F2] rounded-3xl flex items-center justify-center mb-6 shadow-xl animate-bounce">
            <span class="text-white font-bold text-3xl italic">241</span>
        </div>
        <div class="text-gray-400 font-medium">Synchronisation du flux<span class="loading-dots"></span></div>
    </div>

    <!-- Authentification -->
    <div id="auth-container" class="min-h-screen flex flex-col items-center justify-center p-4 bg-[#F0F2F5] dark:bg-[#18191A]">
        <div class="max-w-[1000px] w-full flex flex-col lg:flex-row items-center gap-10 lg:gap-20">
            <div class="text-center lg:text-left flex-1">
                <h1 class="text-[#1877F2] text-5xl md:text-6xl font-bold mb-4 tracking-tighter">facebook 241</h1>
                <p class="text-2xl font-normal leading-tight text-gray-700 dark:text-gray-300">Retrouvez vos proches et partagez l'actualité du Gabon.</p>
            </div>
            <div class="w-full max-w-[400px]">
                <div class="fb-card p-4 dark:bg-[#242526]">
                    <form id="login-form" class="space-y-4">
                        <input type="email" id="login-email" placeholder="Adresse e-mail" class="login-input dark:bg-[#3A3B3C] dark:border-none dark:text-white" required>
                        <input type="password" id="login-pass" placeholder="Mot de passe" class="login-input dark:bg-[#3A3B3C] dark:border-none dark:text-white" required>
                        <button type="submit" id="login-btn" class="w-full bg-[#1877F2] text-white text-xl font-bold py-3 rounded-md hover:bg-blue-600">Se connecter</button>
                        <hr class="border-gray-300 dark:border-gray-700">
                        <div class="flex justify-center pt-2">
                            <button type="button" onclick="showSignup()" class="btn-green px-4 py-3 rounded-md text-lg">Créer un compte</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Inscription -->
    <div id="signup-modal" class="fixed inset-0 bg-white/80 dark:bg-black/80 backdrop-blur-sm z-[500] hidden items-center justify-center p-4">
        <div class="w-full max-w-[432px] fb-card dark:bg-[#242526] overflow-hidden shadow-2xl">
            <div class="p-4 border-b border-gray-300 dark:border-gray-700 flex justify-between items-start">
                <div>
                    <h2 class="text-3xl font-bold">S'inscrire</h2>
                    <p class="text-gray-500 text-sm">Simple et gratuit.</p>
                </div>
                <button onclick="hideSignup()" class="text-gray-500 text-2xl hover:bg-gray-100 dark:hover:bg-gray-700 w-8 h-8 rounded-full">&times;</button>
            </div>
            <form id="signup-form" class="p-4 space-y-3">
                <div class="flex gap-3">
                    <input type="text" id="reg-prenom" placeholder="Prénom" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                    <input type="text" id="reg-nom" placeholder="Nom" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                </div>
                <input type="email" id="reg-email" placeholder="E-mail" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                <input type="password" id="reg-pass" placeholder="Mot de passe (min 6 car.)" class="login-input bg-[#F5F6F7] dark:bg-[#3A3B3C] dark:border-none py-2" required>
                <div class="flex justify-center pt-4">
                    <button type="submit" id="signup-btn" class="btn-green px-12 py-2 rounded-md text-lg">S'inscrire</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Interface Principale -->
    <div id="app-container">
        <header class="h-14 bg-white dark:bg-[#242526] border-b border-gray-300 dark:border-gray-700 flex items-center justify-between px-4 sticky top-0 z-[100] shadow-sm">
            <div class="flex items-center gap-2">
                <div class="w-10 h-10 bg-[#1877F2] rounded-full flex items-center justify-center shadow-lg cursor-pointer" onclick="window.scrollTo({top:0, behavior:'smooth'})">
                    <span class="text-white font-black text-2xl italic">f</span>
                </div>
                <div class="relative hidden sm:block">
                    <input type="text" placeholder="Rechercher sur 241" class="bg-gray-100 dark:bg-[#3A3B3C] rounded-full py-2 pl-10 pr-4 text-sm outline-none focus:ring-2 ring-blue-500">
                    <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-gray-500"></i>
                </div>
            </div>
            
            <div class="flex items-center gap-3">
                <button onclick="toggleDarkMode()" class="w-10 h-10 rounded-full bg-gray-100 dark:bg-[#3A3B3C] flex items-center justify-center hover:bg-gray-200"><i id="theme-icon" class="fa-solid fa-moon"></i></button>
                <div class="flex items-center gap-2 bg-gray-100 dark:bg-[#3A3B3C] p-1 rounded-full pr-3 cursor-pointer">
                    <img id="user-avatar" src="" class="w-8 h-8 rounded-full bg-white object-cover">
                    <span id="user-name-display" class="text-sm font-bold max-w-[80px] truncate">...</span>
                </div>
                <button onclick="logout()" class="w-10 h-10 rounded-full bg-gray-100 dark:bg-[#3A3B3C] text-gray-500 hover:text-red-500 flex items-center justify-center"><i class="fa-solid fa-power-off"></i></button>
            </div>
        </header>

        <main class="max-w-[600px] mx-auto p-4 space-y-4">
            <!-- Créer Post -->
            <div class="fb-card p-4 dark:bg-[#242526]">
                <div class="flex gap-3 items-center">
                    <img id="user-avatar-post" src="" class="w-10 h-10 rounded-full bg-white border">
                    <div onclick="openModal()" class="flex-1 bg-gray-100 dark:bg-[#3A3B3C] rounded-full px-4 py-2.5 text-gray-500 cursor-pointer hover:bg-gray-200 transition-colors">
                        Exprimez-vous, <span id="user-first-name"></span>...
                    </div>
                </div>
                <div class="flex justify-around mt-4 pt-3 border-t dark:border-gray-700">
                    <button class="flex items-center gap-2 text-gray-500 font-semibold p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><i class="fa-solid fa-video text-red-500"></i> Direct</button>
                    <button onclick="openModal()" class="flex items-center gap-2 text-gray-500 font-semibold p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><i class="fa-solid fa-camera text-green-500"></i> Photo</button>
                    <button class="flex items-center gap-2 text-gray-500 font-semibold p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><i class="fa-solid fa-face-smile text-yellow-500"></i> Humeur</button>
                </div>
            </div>

            <!-- Fil d'actualité -->
            <div id="posts-container" class="space-y-4 pb-10"></div>
        </main>
    </div>

    <!-- Modal Post -->
    <div id="post-modal" class="fixed inset-0 bg-white/95 dark:bg-black/95 z-[600] hidden items-center justify-center p-4">
        <div class="w-full max-w-[500px] fb-card dark:bg-[#242526] p-4 relative">
             <div class="flex justify-between items-center mb-4 border-b pb-3 dark:border-gray-700">
                <h3 class="font-bold text-xl text-center w-full">Créer une publication</h3>
                <button onclick="closeModal()" class="absolute right-4 top-4 text-gray-500 text-2xl">&times;</button>
             </div>
             <div class="flex items-center gap-3 mb-4">
                <img id="modal-avatar" src="" class="w-11 h-11 rounded-full border">
                <div>
                    <p id="modal-user-name" class="font-bold">...</p>
                    <span class="bg-gray-200 dark:bg-gray-700 px-2 py-0.5 rounded text-[10px] font-bold uppercase"><i class="fa-solid fa-globe"></i> Public</span>
                </div>
             </div>
             <textarea id="post-input" class="w-full h-32 bg-transparent outline-none text-xl resize-none placeholder-gray-400" placeholder="Quoi de neuf ?"></textarea>
             
             <div id="image-preview-container" class="hidden mb-4 relative">
                <img id="image-preview" src="" class="w-full rounded-lg max-h-60 object-cover border">
                <button onclick="clearImage()" class="absolute top-2 right-2 bg-black/50 text-white w-6 h-6 rounded-full">&times;</button>
             </div>

             <div class="border rounded-lg p-3 flex items-center justify-between dark:border-gray-700 mb-4">
                <span class="font-bold text-sm">Ajouter à votre publication</span>
                <div class="flex gap-2">
                    <button onclick="triggerImagePrompt()" class="w-8 h-8 rounded-full flex items-center justify-center hover:bg-gray-100 dark:hover:bg-gray-700 text-green-500"><i class="fa-solid fa-image"></i></button>
                    <button class="w-8 h-8 rounded-full flex items-center justify-center hover:bg-gray-100 dark:hover:bg-gray-700 text-blue-500"><i class="fa-solid fa-user-tag"></i></button>
                    <button class="w-8 h-8 rounded-full flex items-center justify-center hover:bg-gray-100 dark:hover:bg-gray-700 text-yellow-500"><i class="fa-solid fa-location-dot"></i></button>
                </div>
             </div>

             <button onclick="createPost()" id="publish-btn" class="w-full bg-[#1877F2] text-white py-2 rounded-lg font-bold disabled:opacity-50">Publier</button>
        </div>
    </div>

    <!-- Firebase -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, onAuthStateChanged, signOut, updateProfile } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, onSnapshot, serverTimestamp, doc, setDoc, getDoc, updateDoc, arrayUnion, arrayRemove } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'fb241-prod';

        let userProfile = null;
        let selectedImageUrl = null;

        // --- AUTH LOGIC ---
        onAuthStateChanged(auth, async (user) => {
            const splash = document.getElementById('splash-screen');
            if (user) {
                const userRef = doc(db, 'artifacts', appId, 'public', 'data', 'users', user.uid);
                const snap = await getDoc(userRef);
                userProfile = snap.exists() ? snap.data() : { prenom: user.displayName || 'User', uid: user.uid, avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${user.uid}` };
                
                updateUI(userProfile);
                document.getElementById('auth-container').style.display = 'none';
                document.getElementById('app-container').style.display = 'block';
                startListening();
            } else {
                document.getElementById('auth-container').style.display = 'flex';
                document.getElementById('app-container').style.display = 'none';
            }
            splash.style.opacity = '0';
            setTimeout(() => splash.classList.add('hidden'), 500);
        });

        // Login / Signup
        document.getElementById('login-form').onsubmit = async (e) => {
            e.preventDefault();
            const btn = document.getElementById('login-btn');
            try {
                btn.disabled = true;
                await signInWithEmailAndPassword(auth, document.getElementById('login-email').value, document.getElementById('login-pass').value);
            } catch (err) { alert("Erreur de connexion."); btn.disabled = false; }
        };

        document.getElementById('signup-form').onsubmit = async (e) => {
            e.preventDefault();
            const btn = document.getElementById('signup-btn');
            try {
                btn.disabled = true;
                const prenom = document.getElementById('reg-prenom').value;
                const nom = document.getElementById('reg-nom').value;
                const cred = await createUserWithEmailAndPassword(auth, document.getElementById('reg-email').value, document.getElementById('reg-pass').value);
                
                const profile = { 
                    prenom, nom, uid: cred.user.uid, 
                    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${cred.user.uid}`,
                    email: cred.user.email 
                };
                await setDoc(doc(db, 'artifacts', appId, 'public', 'data', 'users', cred.user.uid), profile);
                await updateProfile(cred.user, { displayName: prenom });
                hideSignup();
            } catch (err) { alert(err.message); btn.disabled = false; }
        };

        window.logout = () => signOut(auth);

        // --- POSTS & COMMENTS ---
        async function createPost() {
            const content = document.getElementById('post-input').value.trim();
            if (!content && !selectedImageUrl) return;

            const btn = document.getElementById('publish-btn');
            btn.disabled = true;
            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'posts'), {
                    content,
                    imageUrl: selectedImageUrl,
                    authorName: `${userProfile.prenom} ${userProfile.nom || ''}`.trim(),
                    authorId: auth.currentUser.uid,
                    authorAvatar: userProfile.avatar,
                    createdAt: serverTimestamp(),
                    likes: [],
                    commentCount: 0
                });
                closeModal();
            } catch (e) { alert("Erreur lors de la publication."); }
            btn.disabled = false;
        }

        async function addComment(postId) {
            const input = document.getElementById(`comment-input-${postId}`);
            const text = input.value.trim();
            if (!text) return;

            try {
                await addDoc(collection(db, 'artifacts', appId, 'public', 'data', 'posts', postId, 'comments'), {
                    text,
                    authorName: userProfile.prenom,
                    authorAvatar: userProfile.avatar,
                    createdAt: serverTimestamp()
                });
                input.value = "";
                // On met à jour le compteur localement ou via Firestore
                const postRef = doc(db, 'artifacts', appId, 'public', 'data', 'posts', postId);
                const postSnap = await getDoc(postRef);
                await updateDoc(postRef, { commentCount: (postSnap.data().commentCount || 0) + 1 });
            } catch (e) { console.error(e); }
        }

        function startListening() {
            onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'posts')), (snap) => {
                const posts = snap.docs.map(d => ({ id: d.id, ...d.data() }))
                    .sort((a,b) => (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0));
                renderPosts(posts);
            });
        }

        function renderPosts(posts) {
            const container = document.getElementById('posts-container');
            container.innerHTML = posts.map(post => `
                <div class="fb-card dark:bg-[#242526] post-enter mb-4">
                    <div class="p-4">
                        <div class="flex items-center gap-3 mb-3">
                            <img src="${post.authorAvatar}" class="w-10 h-10 rounded-full border bg-white">
                            <div>
                                <p class="font-bold text-sm">${post.authorName}</p>
                                <p class="text-[11px] text-gray-500">${post.createdAt ? new Date(post.createdAt.seconds*1000).toLocaleString('fr-FR') : 'À l\'instant'}</p>
                            </div>
                        </div>
                        <p class="text-sm mb-3">${post.content}</p>
                        ${post.imageUrl ? `<img src="${post.imageUrl}" class="w-full rounded-lg mb-3 max-h-96 object-cover border dark:border-gray-700">` : ''}
                        
                        <div class="flex justify-between items-center text-gray-500 text-xs py-2 border-b dark:border-gray-700">
                            <div class="flex items-center gap-1">
                                <span class="bg-blue-500 text-white rounded-full w-4 h-4 flex items-center justify-center text-[8px]"><i class="fa-solid fa-thumbs-up"></i></span>
                                <span>${post.likes?.length || 0}</span>
                            </div>
                            <span>${post.commentCount || 0} commentaires</span>
                        </div>

                        <div class="flex justify-around py-1 mt-1">
                            <button onclick="handleLike('${post.id}', ${post.likes?.includes(auth.currentUser.uid)})" class="flex-1 py-2 flex items-center justify-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg text-sm font-semibold ${post.likes?.includes(auth.currentUser.uid) ? 'text-blue-500' : 'text-gray-500'}">
                                <i class="fa-${post.likes?.includes(auth.currentUser.uid) ? 'solid' : 'regular'} fa-thumbs-up"></i> J'aime
                            </button>
                            <button onclick="toggleComments('${post.id}')" class="flex-1 py-2 flex items-center justify-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg text-sm font-semibold text-gray-500">
                                <i class="fa-regular fa-message"></i> Commenter
                            </button>
                        </div>
                    </div>

                    <!-- Zone Commentaires -->
                    <div id="comment-section-${post.id}" class="hidden p-4 bg-gray-50 dark:bg-[#1C1D1F] border-t dark:border-gray-700 rounded-b-lg">
                        <div id="comments-list-${post.id}" class="space-y-3 mb-4 max-h-40 overflow-y-auto"></div>
                        <div class="flex gap-2">
                            <img src="${userProfile.avatar}" class="w-8 h-8 rounded-full border">
                            <input type="text" id="comment-input-${post.id}" onkeypress="if(event.key==='Enter') window.addComment('${post.id}')" placeholder="Écrire un commentaire..." class="comment-input flex-1 outline-none dark:text-white">
                        </div>
                    </div>
                </div>
            `).join('');
        }

        async function toggleComments(postId) {
            const section = document.getElementById(`comment-section-${postId}`);
            const isHidden = section.classList.toggle('hidden');
            if (!isHidden) {
                const list = document.getElementById(`comments-list-${postId}`);
                list.innerHTML = `<div class="text-center py-2"><i class="fa-solid fa-circle-notch fa-spin text-gray-400"></i></div>`;
                
                onSnapshot(query(collection(db, 'artifacts', appId, 'public', 'data', 'posts', postId, 'comments')), (snap) => {
                    const comments = snap.docs.map(d => d.data()).sort((a,b) => (a.createdAt?.seconds || 0) - (b.createdAt?.seconds || 0));
                    list.innerHTML = comments.map(c => `
                        <div class="flex gap-2 items-start">
                            <img src="${c.authorAvatar}" class="w-7 h-7 rounded-full border">
                            <div class="bg-gray-200 dark:bg-[#3A3B3C] p-2 rounded-2xl max-w-[85%]">
                                <p class="text-[12px] font-bold leading-none mb-1">${c.authorName}</p>
                                <p class="text-[13px] leading-tight">${c.text}</p>
                            </div>
                        </div>
                    `).join('') || '<p class="text-xs text-center text-gray-400">Aucun commentaire</p>';
                });
            }
        }

        async function handleLike(postId, isLiked) {
            const ref = doc(db, 'artifacts', appId, 'public', 'data', 'posts', postId);
            await updateDoc(ref, { likes: isLiked ? arrayRemove(auth.currentUser.uid) : arrayUnion(auth.currentUser.uid) });
        }

        // --- HELPERS ---
        window.triggerImagePrompt = () => {
            const url = prompt("Entrez l'URL de l'image (ou laissez vide pour un placeholder Gabon) :");
            if (url) {
                selectedImageUrl = url;
                document.getElementById('image-preview').src = url;
                document.getElementById('image-preview-container').classList.remove('hidden');
            } else if (url === "") {
                selectedImageUrl = "https://images.unsplash.com/photo-1549417229-aa67d3263c09?q=80&w=600&auto=format&fit=crop";
                document.getElementById('image-preview').src = selectedImageUrl;
                document.getElementById('image-preview-container').classList.remove('hidden');
            }
        };

        window.clearImage = () => {
            selectedImageUrl = null;
            document.getElementById('image-preview-container').classList.add('hidden');
        };

        function updateUI(profile) {
            const elements = ['user-avatar', 'user-avatar-post', 'modal-avatar'];
            elements.forEach(id => document.getElementById(id).src = profile.avatar);
            document.getElementById('user-name-display').textContent = profile.prenom;
            document.getElementById('user-first-name').textContent = profile.prenom;
            document.getElementById('modal-user-name').textContent = `${profile.prenom} ${profile.nom || ''}`;
        }

        window.showSignup = () => document.getElementById('signup-modal').style.display = 'flex';
        window.hideSignup = () => document.getElementById('signup-modal').style.display = 'none';
        window.openModal = () => { document.getElementById('post-modal').style.display = 'flex'; document.getElementById('post-input').focus(); };
        window.closeModal = () => { document.getElementById('post-modal').style.display = 'none'; clearImage(); document.getElementById('post-input').value = ""; };
        window.toggleDarkMode = () => { 
            const isDark = document.documentElement.classList.toggle('dark');
            document.getElementById('theme-icon').className = isDark ? 'fa-solid fa-sun' : 'fa-solid fa-moon';
        };
        window.addComment = addComment;
        window.toggleComments = toggleComments;
        window.handleLike = handleLike;
        window.createPost = createPost;
    </script>
</body>
</html>
