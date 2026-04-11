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
            --fb-divider: #CED0D4;
        }

        .dark {
            --fb-bg: #18191A;
            --fb-white: #242526;
            --fb-text: #E4E6EB;
            --fb-gray: #B0B3B8;
            --fb-divider: #3E4042;
        }

        body {
            font-family: 'Segoe UI', Helvetica, Arial, sans-serif;
            background-color: var(--fb-bg);
            color: var(--fb-text);
            transition: background-color 0.2s ease;
            overflow-x: hidden;
        }

        .fb-card {
            background-color: var(--fb-white);
            border-radius: 8px;
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
        }

        .active-tab {
            color: var(--fb-blue) !important;
            border-bottom: 3px solid var(--fb-blue);
        }

        .loading-spinner {
            border: 3px solid rgba(0, 0, 0, 0.1);
            border-radius: 50%;
            border-top: 3px solid var(--fb-blue);
            width: 24px;
            height: 24px;
            animation: spin 0.8s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .post-enter {
            animation: slideIn 0.4s ease-out;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .shadow-text {
            text-shadow: 0 1px 4px rgba(0,0,0,0.8);
        }

        /* Masquer la scrollbar */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="transition-colors duration-200">

    <!-- Écran de chargement splash -->
    <div id="loading-screen" class="fixed inset-0 z-[1000] bg-[#F0F2F5] dark:bg-[#18191A] flex flex-col items-center justify-center transition-opacity duration-500">
        <div class="w-20 h-20 bg-[#1877F2] rounded-3xl flex items-center justify-center mb-8 shadow-2xl scale-110">
            <span class="text-white font-bold text-3xl italic">241</span>
        </div>
        <div class="loading-spinner"></div>
        <p class="mt-4 font-semibold text-gray-500 dark:text-gray-400">Initialisation de la production...</p>
    </div>

    <!-- Header -->
    <header class="h-14 bg-white dark:bg-[#242526] border-b border-gray-300 dark:border-gray-700 flex items-center justify-between px-4 sticky top-0 z-[100] transition-colors shadow-sm">
        <div class="flex items-center gap-2">
            <div onclick="window.scrollTo({top: 0, behavior: 'smooth'})" class="w-10 h-10 bg-[#1877F2] rounded-full flex items-center justify-center shadow-md cursor-pointer active:scale-90 transition-transform">
                <span class="text-white font-black text-2xl italic">f</span>
            </div>
            <div class="hidden md:flex items-center bg-[#F0F2F5] dark:bg-[#3A3B3C] rounded-full px-3 py-2 gap-2">
                <i class="fa-solid fa-search text-gray-500 text-sm"></i>
                <input type="text" placeholder="Rechercher sur Facebook" class="bg-transparent border-none outline-none text-sm w-48 text-inherit">
            </div>
        </div>

        <nav class="hidden lg:flex items-center h-full">
            <button onclick="switchTab('feed')" class="h-full px-12 transition-all hover:bg-gray-100 dark:hover:bg-[#3A3B3C] active-tab text-gray-500" id="tab-feed">
                <i class="fa-solid fa-home text-2xl"></i>
            </button>
            <button onclick="switchTab('friends')" class="h-full px-12 transition-all hover:bg-gray-100 dark:hover:bg-[#3A3B3C] text-gray-500" id="tab-friends">
                <i class="fa-solid fa-user-group text-2xl"></i>
            </button>
            <button class="h-full px-12 transition-all hover:bg-gray-100 dark:hover:bg-[#3A3B3C] text-gray-500">
                <i class="fa-solid fa-tv text-2xl"></i>
            </button>
            <button class="h-full px-12 transition-all hover:bg-gray-100 dark:hover:bg-[#3A3B3C] text-gray-500">
                <i class="fa-solid fa-store text-2xl"></i>
            </button>
        </nav>

        <div class="flex items-center gap-2">
            <button onclick="toggleDarkMode()" class="w-10 h-10 rounded-full bg-gray-200 dark:bg-[#3A3B3C] flex items-center justify-center hover:bg-gray-300 dark:hover:bg-[#4E4F50] transition-colors">
                <i id="theme-icon" class="fa-solid fa-moon"></i>
            </button>
            <button class="w-10 h-10 rounded-full bg-gray-200 dark:bg-[#3A3B3C] flex items-center justify-center hover:bg-gray-300 dark:hover:bg-[#4E4F50] transition-colors">
                <i class="fa-solid fa-message"></i>
            </button>
            <button class="w-10 h-10 rounded-full bg-gray-200 dark:bg-[#3A3B3C] flex items-center justify-center hover:bg-gray-300 dark:hover:bg-[#4E4F50] transition-colors">
                <i class="fa-solid fa-bell"></i>
            </button>
            <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Admin" class="w-10 h-10 rounded-full border border-gray-300 cursor-pointer hover:opacity-80 transition-opacity" alt="Profil">
        </div>
    </header>

    <div class="max-w-[1400px] mx-auto flex justify-center xl:justify-between px-4 pt-4 gap-8">
        
        <!-- Sidebar Gauche (Desktop) -->
        <aside class="hidden xl:flex flex-col w-72 sticky top-[72px] h-[calc(100vh-72px)] overflow-y-auto no-scrollbar">
            <div class="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-[#3A3B3C] cursor-pointer mb-2">
                <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Admin" class="w-9 h-9 rounded-full bg-white border" alt="me">
                <span class="font-bold text-sm">Utilisateur 241</span>
            </div>
            <div class="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-[#3A3B3C] cursor-pointer">
                <i class="fa-solid fa-user-friends text-blue-500 text-xl w-8 text-center"></i>
                <span class="font-semibold text-sm text-gray-700 dark:text-gray-200">Amis</span>
            </div>
            <div class="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-[#3A3B3C] cursor-pointer">
                <i class="fa-solid fa-clock-rotate-left text-blue-400 text-xl w-8 text-center"></i>
                <span class="font-semibold text-sm text-gray-700 dark:text-gray-200">Souvenirs</span>
            </div>
            <div class="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-[#3A3B3C] cursor-pointer">
                <i class="fa-solid fa-bookmark text-purple-500 text-xl w-8 text-center"></i>
                <span class="font-semibold text-sm text-gray-700 dark:text-gray-200">Enregistré</span>
            </div>
            <hr class="my-4 border-gray-300 dark:border-gray-700">
            <p class="text-xs text-gray-500 px-2 font-semibold uppercase mb-2">Vos raccourcis</p>
            <div class="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-[#3A3B3C] cursor-pointer">
                <div class="w-8 h-8 bg-blue-600 rounded-md flex items-center justify-center text-white text-xs font-bold">241</div>
                <span class="text-sm">Groupe Production 2026</span>
            </div>
        </aside>

        <!-- Main Content -->
        <main id="main-content" class="flex-1 max-w-[680px] space-y-5 pb-20">
            <!-- Section Stories -->
            <div class="flex gap-2 overflow-x-auto no-scrollbar pb-2">
                <!-- Créer Story -->
                <div class="min-w-[112px] h-48 rounded-xl relative overflow-hidden fb-card cursor-pointer group border border-gray-200 dark:border-gray-700">
                    <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Admin" class="w-full h-[75%] object-cover group-hover:scale-105 transition-transform" alt="story">
                    <div class="absolute bottom-0 w-full h-[25%] bg-white dark:bg-[#242526] flex flex-col items-center">
                        <div class="w-8 h-8 bg-[#1877F2] rounded-full border-4 border-white dark:border-[#242526] -mt-4 flex items-center justify-center text-white">
                            <i class="fa-solid fa-plus"></i>
                        </div>
                        <span class="text-[11px] font-bold mt-1">Créer story</span>
                    </div>
                </div>
                <!-- Stories fictives -->
                <div class="min-w-[112px] h-48 rounded-xl relative overflow-hidden fb-card cursor-pointer group">
                    <img src="https://picsum.photos/400/800?random=10" class="w-full h-full object-cover group-hover:scale-105 transition-transform" alt="story">
                    <div class="absolute top-3 left-3 w-8 h-8 rounded-full border-2 border-[#1877F2] overflow-hidden bg-white">
                        <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Sarra" alt="user">
                    </div>
                    <span class="absolute bottom-2 left-2 text-white text-xs font-bold shadow-text">Sarra M.</span>
                </div>
                <div class="min-w-[112px] h-48 rounded-xl relative overflow-hidden fb-card cursor-pointer group">
                    <img src="https://picsum.photos/400/800?random=22" class="w-full h-full object-cover group-hover:scale-105 transition-transform" alt="story">
                    <div class="absolute top-3 left-3 w-8 h-8 rounded-full border-2 border-[#1877F2] overflow-hidden bg-white">
                        <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Kevin" alt="user">
                    </div>
                    <span class="absolute bottom-2 left-2 text-white text-xs font-bold shadow-text">Kevin L.</span>
                </div>
            </div>

            <!-- Widget Créer Publication -->
            <div class="fb-card p-4">
                <div class="flex gap-3 items-center mb-4">
                    <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Admin" class="w-10 h-10 rounded-full border border-gray-300 shadow-sm" alt="me">
                    <div onclick="openModal()" class="flex-1 bg-[#F0F2F5] dark:bg-[#3A3B3C] rounded-full px-4 py-2.5 text-gray-500 cursor-pointer hover:bg-gray-200 dark:hover:bg-[#4E4F50] transition-colors text-sm md:text-base">
                        Que voulez-vous dire, Utilisateur 241 ?
                    </div>
                </div>
                <hr class="border-gray-200 dark:border-gray-700 mb-2">
                <div class="flex justify-around">
                    <button class="flex-1 flex items-center justify-center gap-2 py-2 hover:bg-gray-100 dark:hover:bg-[#3A3B3C] rounded-lg text-gray-500 font-semibold text-sm transition-colors">
                        <i class="fa-solid fa-video text-red-500"></i> <span class="hidden sm:inline">Vidéo en direct</span>
                    </button>
                    <button class="flex-1 flex items-center justify-center gap-2 py-2 hover:bg-gray-100 dark:hover:bg-[#3A3B3C] rounded-lg text-gray-500 font-semibold text-sm transition-colors">
                        <i class="fa-solid fa-image text-green-500"></i> <span class="hidden sm:inline">Photo/vidéo</span>
                    </button>
                    <button class="flex-1 flex items-center justify-center gap-2 py-2 hover:bg-gray-100 dark:hover:bg-[#3A3B3C] rounded-lg text-gray-500 font-semibold text-sm transition-colors">
                        <i class="fa-solid fa-face-smile text-yellow-500"></i> <span class="hidden sm:inline">Humeur/Activité</span>
                    </button>
                </div>
            </div>

            <!-- Feed Dynamique -->
            <div id="posts-container" class="space-y-4">
                <!-- Les posts s'affichent ici -->
            </div>
        </main>

        <!-- Sidebar Droite (Desktop) -->
        <aside class="hidden lg:flex flex-col w-72 sticky top-[72px] h-[calc(100vh-72px)] overflow-y-auto no-scrollbar">
            <div class="flex items-center justify-between px-2 mb-4">
                <p class="text-gray-500 font-bold text-sm">Contacts</p>
                <div class="flex gap-4 text-gray-500 text-sm">
                    <i class="fa-solid fa-video cursor-pointer hover:bg-gray-200 p-1 rounded"></i>
                    <i class="fa-solid fa-search cursor-pointer hover:bg-gray-200 p-1 rounded"></i>
                    <i class="fa-solid fa-ellipsis cursor-pointer hover:bg-gray-200 p-1 rounded"></i>
                </div>
            </div>
            <div id="contacts-list" class="space-y-1">
                <!-- Contacts dynamiques -->
            </div>
        </aside>
    </div>

    <!-- Modal de Publication (Overlay) -->
    <div id="post-modal" class="fixed inset-0 bg-white/80 dark:bg-black/80 backdrop-blur-md z-[500] hidden items-center justify-center p-4">
        <div class="w-full max-w-[500px] fb-card shadow-2xl overflow-hidden transform transition-all duration-300 scale-95 opacity-0" id="modal-box">
            <div class="p-4 border-b border-gray-200 dark:border-gray-700 flex items-center justify-between relative">
                <h3 class="font-bold text-lg mx-auto">Créer une publication</h3>
                <button onclick="closeModal()" class="absolute right-4 w-9 h-9 bg-gray-100 dark:bg-[#3A3B3C] rounded-full flex items-center justify-center hover:bg-gray-200 transition-colors">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            <div class="p-4">
                <div class="flex items-center gap-3 mb-4">
                    <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Admin" class="w-10 h-10 rounded-full border border-gray-300" alt="me">
                    <div>
                        <p class="font-bold text-sm">Utilisateur 241</p>
                        <div class="bg-gray-200 dark:bg-[#3A3B3C] px-2 py-0.5 rounded-md text-[10px] font-bold flex items-center gap-1 w-fit mt-0.5">
                            <i class="fa-solid fa-earth-americas text-[9px]"></i> Public <i class="fa-solid fa-caret-down"></i>
                        </div>
                    </div>
                </div>
                <textarea id="post-input" class="w-full h-40 bg-transparent border-none outline-none text-xl resize-none placeholder:text-gray-400 dark:text-white" placeholder="Quoi de neuf ?"></textarea>
                
                <div class="border border-gray-300 dark:border-gray-700 rounded-lg p-3 flex items-center justify-between mb-4">
                    <span class="text-sm font-bold text-gray-500">Ajouter à votre publication</span>
                    <div class="flex gap-4 text-xl">
                        <i class="fa-solid fa-image text-green-500 cursor-pointer hover:scale-110 transition-transform"></i>
                        <i class="fa-solid fa-user-plus text-blue-500 cursor-pointer hover:scale-110 transition-transform"></i>
                        <i class="fa-solid fa-face-smile text-yellow-500 cursor-pointer hover:scale-110 transition-transform"></i>
                        <i class="fa-solid fa-location-dot text-red-500 cursor-pointer hover:scale-110 transition-transform"></i>
                    </div>
                </div>
                
                <button onclick="createPost()" class="w-full bg-[#1877F2] text-white py-2.5 rounded-lg font-bold text-sm hover:bg-blue-600 transition-colors disabled:opacity-50 disabled:cursor-not-allowed shadow-lg" id="publish-btn" disabled>
                    Publier sur Facebook 241
                </button>
            </div>
        </div>
    </div>

    <!-- Navigation Mobile (Bas de l'écran) -->
    <nav class="lg:hidden fixed bottom-0 left-0 right-0 h-14 bg-white dark:bg-[#242526] border-t border-gray-200 dark:border-gray-700 flex items-center justify-around z-[400] shadow-[0_-1px_10px_rgba(0,0,0,0.05)]">
        <button onclick="switchTab('feed')" class="flex-1 flex flex-col items-center gap-0.5 text-[#1877F2]"><i class="fa-solid fa-home text-xl"></i></button>
        <button onclick="switchTab('friends')" class="flex-1 flex flex-col items-center gap-0.5 text-gray-400"><i class="fa-solid fa-user-group text-xl"></i></button>
        <button onclick="openModal()" class="w-12 h-12 bg-[#1877F2] rounded-full text-white flex items-center justify-center -mt-8 border-4 border-[#F0F2F5] dark:border-[#18191A] shadow-xl active:scale-90 transition-transform"><i class="fa-solid fa-plus text-xl"></i></button>
        <button class="flex-1 flex flex-col items-center gap-0.5 text-gray-400"><i class="fa-solid fa-bell text-xl"></i></button>
        <button class="flex-1 flex flex-col items-center gap-0.5 text-gray-400"><i class="fa-solid fa-bars text-xl"></i></button>
    </nav>

    <script>
        // --- DONNÉES ET ÉTAT ---
        const DEFAULT_POSTS = [
            {
                id: 1,
                author: "Directeur Technique",
                avatar: "https://api.dicebear.com/7.x/avataaars/svg?seed=Chief",
                content: "Les serveurs sont prêts pour le lancement de demain ! L'application 'Facebook 241' est en phase de déploiement final. Félicitations à toute l'équipe. 🥂",
                time: "Il y a 1 heure",
                likes: 42,
                comments: 5,
                liked: false
            },
            {
                id: 2,
                author: "Équipe Design",
                avatar: "https://api.dicebear.com/7.x/avataaars/svg?seed=Design",
                content: "Regardez ce nouveau mode sombre ! Nous avons optimisé chaque pixel pour une expérience utilisateur premium. Qu'en pensez-vous ?",
                time: "Il y a 3 heures",
                likes: 89,
                comments: 12,
                liked: false
            }
        ];

        let posts = JSON.parse(localStorage.getItem('fb_241_posts')) || DEFAULT_POSTS;
        let isDarkMode = localStorage.getItem('fb_241_theme') === 'dark';

        // --- INITIALISATION ---
        window.addEventListener('load', () => {
            // Appliquer le thème
            if (isDarkMode) {
                document.documentElement.classList.add('dark');
                document.getElementById('theme-icon').className = 'fa-solid fa-sun';
            }

            // Charger les contacts
            renderContacts();

            // Masquer le splash screen
            setTimeout(() => {
                const splash = document.getElementById('loading-screen');
                splash.style.opacity = '0';
                setTimeout(() => splash.classList.add('hidden'), 500);
                renderPosts();
            }, 1200);
        });

        // --- THEME ---
        function toggleDarkMode() {
            isDarkMode = !isDarkMode;
            document.documentElement.classList.toggle('dark');
            const icon = document.getElementById('theme-icon');
            icon.className = isDarkMode ? 'fa-solid fa-sun' : 'fa-solid fa-moon';
            localStorage.setItem('fb_241_theme', isDarkMode ? 'dark' : 'light');
        }

        // --- MODAL ---
        function openModal() {
            const modal = document.getElementById('post-modal');
            const box = document.getElementById('modal-box');
            modal.style.display = 'flex';
            setTimeout(() => {
                box.classList.remove('scale-95', 'opacity-0');
                box.classList.add('scale-100', 'opacity-100');
            }, 10);
            document.getElementById('post-input').focus();
        }

        function closeModal() {
            const box = document.getElementById('modal-box');
            box.classList.remove('scale-100', 'opacity-100');
            box.classList.add('scale-95', 'opacity-0');
            setTimeout(() => {
                document.getElementById('post-modal').style.display = 'none';
                document.getElementById('post-input').value = '';
                document.getElementById('publish-btn').disabled = true;
            }, 200);
        }

        document.getElementById('post-input').addEventListener('input', (e) => {
            document.getElementById('publish-btn').disabled = !e.target.value.trim();
        });

        // --- POSTS ---
        function createPost() {
            const content = document.getElementById('post-input').value;
            if(!content.trim()) return;

            const newPost = {
                id: Date.now(),
                author: "Utilisateur 241",
                avatar: "https://api.dicebear.com/7.x/avataaars/svg?seed=Admin",
                content: content,
                time: "À l'instant",
                likes: 0,
                comments: 0,
                liked: false,
                isNew: true
            };

            posts.unshift(newPost);
            savePosts();
            renderPosts();
            closeModal();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function renderPosts() {
            const container = document.getElementById('posts-container');
            container.innerHTML = posts.map(post => `
                <div class="fb-card overflow-hidden ${post.isNew ? 'post-enter' : ''}">
                    <div class="p-4 flex items-center justify-between">
                        <div class="flex gap-3">
                            <img src="${post.avatar}" class="w-10 h-10 rounded-full border border-gray-200 bg-white" alt="avatar">
                            <div>
                                <h4 class="font-bold text-sm hover:underline cursor-pointer">${post.author}</h4>
                                <p class="text-[11px] text-gray-500 flex items-center gap-1">${post.time} • <i class="fa-solid fa-earth-americas text-[9px]"></i></p>
                            </div>
                        </div>
                        <button class="w-8 h-8 flex items-center justify-center rounded-full hover:bg-gray-100 dark:hover:bg-[#3A3B3C] text-gray-500"><i class="fa-solid fa-ellipsis"></i></button>
                    </div>
                    <div class="px-4 pb-3 text-[15px] leading-snug whitespace-pre-wrap">${post.content}</div>
                    
                    <div class="px-4 py-2.5 flex items-center justify-between border-b border-gray-100 dark:border-gray-700 mx-4">
                        <div class="flex items-center gap-1.5">
                            <div class="bg-[#1877F2] rounded-full p-1 w-4.5 h-4.5 flex items-center justify-center shadow-sm">
                                <i class="fa-solid fa-thumbs-up text-white text-[9px]"></i>
                            </div>
                            <span class="text-sm text-gray-500">${post.likes}</span>
                        </div>
                        <div class="text-sm text-gray-500 hover:underline cursor-pointer">${post.comments} commentaires</div>
                    </div>

                    <div class="flex items-center justify-around p-1 mx-3 mb-1">
                        <button onclick="toggleLike(${post.id})" class="flex-1 flex items-center justify-center gap-2 py-2 rounded-md hover:bg-gray-100 dark:hover:bg-[#3A3B3C] ${post.liked ? 'text-[#1877F2]' : 'text-gray-500'} font-semibold text-sm transition-all active:scale-95">
                            <i class="${post.liked ? 'fa-solid' : 'fa-regular'} fa-thumbs-up text-lg"></i> J'aime
                        </button>
                        <button class="flex-1 flex items-center justify-center gap-2 py-2 rounded-md hover:bg-gray-100 dark:hover:bg-[#3A3B3C] text-gray-500 font-semibold text-sm transition-colors">
                            <i class="fa-regular fa-message text-lg"></i> Commenter
                        </button>
                        <button class="flex-1 flex items-center justify-center gap-2 py-2 rounded-md hover:bg-gray-100 dark:hover:bg-[#3A3B3C] text-gray-500 font-semibold text-sm transition-colors">
                            <i class="fa-solid fa-share text-lg"></i> Partager
                        </button>
                    </div>
                </div>
            `).join('');
            
            // Retirer le flag "isNew" après rendu
            posts.forEach(p => delete p.isNew);
        }

        function toggleLike(id) {
            const post = posts.find(p => p.id === id);
            if(post) {
                post.liked = !post.liked;
                post.likes += post.liked ? 1 : -1;
                savePosts();
                renderPosts();
            }
        }

        function savePosts() {
            localStorage.setItem('fb_241_posts', JSON.stringify(posts));
        }

        // --- CONTACTS ---
        function renderContacts() {
            const list = document.getElementById('contacts-list');
            const names = ["Marie Curie", "Albert Einstein", "Ada Lovelace", "Nikola Tesla", "Grace Hopper"];
            list.innerHTML = names.map(name => `
                <div class="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-[#3A3B3C] cursor-pointer transition-colors relative group">
                    <div class="relative">
                        <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=${name}" class="w-9 h-9 rounded-full bg-white border border-gray-200" alt="${name}">
                        <div class="absolute bottom-0 right-0 w-3 h-3 bg-green-500 border-2 border-white dark:border-[#242526] rounded-full"></div>
                    </div>
                    <span class="font-semibold text-sm dark:text-gray-200">${name}</span>
                </div>
            `).join('');
        }

        function switchTab(tab) {
            // Mise à jour visuelle des onglets
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('active-tab'));
            const activeBtn = document.getElementById(`tab-${tab}`);
            if(activeBtn) activeBtn.classList.add('active-tab');

            if(tab === 'friends') {
                document.getElementById('main-content').innerHTML = `
                    <div class="fb-card p-6 animate-in fade-in duration-300">
                        <h2 class="text-2xl font-bold mb-6">Invitations d'amis</h2>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div class="border border-gray-200 dark:border-gray-700 rounded-xl overflow-hidden shadow-sm">
                                <img src="https://picsum.photos/300/300?random=50" class="w-full h-40 object-cover" />
                                <div class="p-3">
                                    <p class="font-bold text-base mb-3 text-center">Inconnu du 241</p>
                                    <button class="w-full bg-[#1877F2] text-white py-2 rounded-lg font-bold mb-2">Confirmer</button>
                                    <button class="w-full bg-gray-200 dark:bg-[#3A3B3C] py-2 rounded-lg font-bold">Supprimer</button>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            } else if (tab === 'feed') {
                location.reload(); // Simple retour au feed
            }
        }
    </script>
</body>
</html>
