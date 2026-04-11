<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>echoppe241 B2B | Grossiste Made in Gabon</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --brand-green: #00853f; /* Vert Drapeau */
            --brand-yellow: #fcd116; /* Jaune Drapeau */
            --brand-blue: #3a75c4; /* Bleu Drapeau */
        }
        .bg-gab-green { background-color: var(--brand-green); }
        .text-gab-green { color: var(--brand-green); }
        .bg-gab-yellow { background-color: var(--brand-yellow); }
        .text-gab-yellow { color: var(--brand-yellow); }
        .bg-gab-blue { background-color: var(--brand-blue); }
        
        body {
            font-family: 'Inter', sans-serif;
        }

        .product-card {
            transition: transform 0.2s;
        }
        .product-card:hover {
            transform: scale(1.02);
        }

        .b2b-badge {
            background: linear-gradient(135deg, var(--brand-green), var(--brand-blue));
            color: white;
            padding: 2px 8px;
            border-radius: 4px;
            font-size: 0.7rem;
            font-weight: bold;
        }

        /* Modal Blur */
        .modal-overlay {
            background-color: rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(8px);
        }
        .hidden-modal { display: none; }
    </style>
</head>
<body class="bg-slate-50 text-slate-900">

    <!-- Auth Modal (B2B Specialized) -->
    <div id="authModal" class="modal-overlay fixed inset-0 z-[100] flex items-center justify-center p-4 hidden-modal">
        <div class="bg-white rounded-xl shadow-2xl w-full max-w-lg overflow-hidden relative border-t-4 border-gab-green">
            <button onclick="toggleAuthModal()" class="absolute top-4 right-4 text-slate-400 hover:text-slate-600">
                <i class="fa-solid fa-xmark text-xl"></i>
            </button>
            
            <div class="p-8">
                <div class="text-center mb-6">
                    <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Logo" class="h-12 mx-auto mb-2">
                    <h2 id="modalTitle" class="text-2xl font-black text-slate-800 uppercase tracking-tighter">Portail Professionnel</h2>
                    <p id="modalSubtitle" class="text-slate-500 italic">Accédez aux tarifs grossistes Made in Gabon</p>
                </div>

                <div class="flex border-b mb-6">
                    <button onclick="switchTab('login')" id="loginTab" class="w-1/2 py-2 font-bold border-b-2 border-gab-green text-gab-green">Connexion Pro</button>
                    <button onclick="switchTab('register')" id="registerTab" class="w-1/2 py-2 font-bold border-b-2 border-transparent text-slate-400 transition">Nouvelle Entreprise</button>
                </div>

                <!-- Login Form -->
                <form id="loginForm" class="space-y-4">
                    <div class="space-y-1">
                        <label class="text-xs font-bold uppercase text-slate-500">Email Professionnel</label>
                        <input type="email" required placeholder="contact@votre-boutique.ga" class="w-full px-4 py-3 rounded border border-slate-200 focus:ring-2 focus:ring-gab-green outline-none">
                    </div>
                    <div class="space-y-1">
                        <label class="text-xs font-bold uppercase text-slate-500">Mot de passe</label>
                        <input type="password" required placeholder="••••••••" class="w-full px-4 py-3 rounded border border-slate-200 focus:ring-2 focus:ring-gab-green outline-none">
                    </div>
                    <button type="submit" class="w-full bg-slate-900 text-white font-bold py-4 rounded-lg hover:bg-black transition">ACCÉDER AUX TARIFS</button>
                </form>

                <!-- Register Form (B2B Fields) -->
                <form id="registerForm" class="space-y-4 hidden">
                    <div class="grid grid-cols-2 gap-3">
                        <div class="space-y-1">
                            <label class="text-xs font-bold uppercase text-slate-500">Nom Entreprise / GIE</label>
                            <input type="text" placeholder="Coopérative X" class="w-full px-3 py-2 rounded border border-slate-200">
                        </div>
                        <div class="space-y-1">
                            <label class="text-xs font-bold uppercase text-slate-500">N° Commerce / NIF</label>
                            <input type="text" placeholder="GAB-123456" class="w-full px-3 py-2 rounded border border-slate-200">
                        </div>
                    </div>
                    <div class="space-y-1">
                        <label class="text-xs font-bold uppercase text-slate-500">Secteur d'activité</label>
                        <select class="w-full px-3 py-2 rounded border border-slate-200 bg-white">
                            <option>Distribution / Boutique</option>
                            <option>Hôtellerie / Restauration (CHR)</option>
                            <option>Exportation</option>
                            <option>Autre</option>
                        </select>
                    </div>
                    <div class="space-y-1">
                        <label class="text-xs font-bold uppercase text-slate-500">Téléphone (WhatsApp Pro)</label>
                        <input type="tel" placeholder="077 00 00 00" class="w-full px-3 py-2 rounded border border-slate-200">
                    </div>
                    <button type="submit" class="w-full bg-gab-green text-white font-bold py-4 rounded-lg hover:bg-green-700 transition">CRÉER COMPTE GROSSISTE</button>
                </form>
            </div>
        </div>
    </div>

    <!-- Navigation -->
    <nav class="bg-white shadow-sm sticky top-0 z-50 border-b border-slate-100">
        <div class="max-w-7xl mx-auto px-4 lg:px-8">
            <div class="flex justify-between h-20 items-center">
                <div class="flex items-center space-x-2">
                    <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="echoppe241" class="h-10">
                    <span class="bg-gab-yellow/20 text-gab-green px-2 py-0.5 rounded text-[10px] font-black tracking-widest uppercase border border-gab-yellow/30">B2B Pro</span>
                </div>
                
                <div class="hidden md:flex items-center space-x-8 text-sm font-bold uppercase tracking-tight">
                    <a href="#" class="text-gab-green">Catalogue</a>
                    <a href="#" class="text-slate-600 hover:text-gab-green">Fournisseurs Locaux</a>
                    <a href="#" class="text-slate-600 hover:text-gab-green">Logistique</a>
                </div>

                <div class="flex items-center space-x-6">
                    <button onclick="toggleAuthModal()" class="hidden sm:block text-xs font-bold text-slate-700 hover:bg-slate-100 px-4 py-2 rounded-full border border-slate-200 transition">
                        ESPACE CLIENT
                    </button>
                    <button class="relative">
                        <i class="fa-solid fa-box-open text-xl text-slate-800"></i>
                        <span class="absolute -top-2 -right-2 bg-gab-blue text-white text-[10px] rounded-full h-4 w-4 flex items-center justify-center font-bold">0</span>
                    </button>
                    <button class="md:hidden"><i class="fa-solid fa-bars-staggered text-xl"></i></button>
                </div>
            </div>
        </div>
    </nav>

    <!-- B2B Hero Section -->
    <header class="relative bg-white pt-16 pb-24 border-b border-slate-100 overflow-hidden">
        <div class="max-w-7xl mx-auto px-4 lg:px-8 flex flex-col md:flex-row items-center gap-16">
            <div class="md:w-3/5">
                <div class="inline-flex items-center space-x-2 bg-gab-green/10 text-gab-green px-3 py-1 rounded-full text-xs font-bold mb-6">
                    <i class="fa-solid fa-certificate"></i>
                    <span>100% PRODUITS GABONAIS EN GROS</span>
                </div>
                <h1 class="text-5xl md:text-7xl font-black text-slate-900 leading-none mb-6">
                    Sourcing Local <br><span class="text-gab-green">Simplifié.</span>
                </h1>
                <p class="text-xl text-slate-600 mb-10 max-w-lg leading-relaxed">
                    Connectez votre entreprise aux meilleurs producteurs du Gabon. Prix direct usine, logistique intégrée et traçabilité garantie.
                </p>
                <div class="flex flex-wrap gap-4">
                    <button onclick="toggleAuthModal(); switchTab('register');" class="bg-gab-green text-white px-8 py-4 rounded font-bold shadow-xl shadow-gab-green/20 hover:bg-green-700 transition">
                        DEVENIR REVENDEUR
                    </button>
                    <button class="bg-white border-2 border-slate-900 text-slate-900 px-8 py-4 rounded font-bold hover:bg-slate-900 hover:text-white transition">
                        CONTACTER UN COMMERCIAL
                    </button>
                </div>
            </div>
            <div class="md:w-2/5 relative">
                <div class="bg-gab-yellow/20 absolute -inset-4 rounded-full blur-3xl animate-pulse"></div>
                <img src="https://images.unsplash.com/photo-1542838132-92c53300491e?auto=format&fit=crop&w=800&q=80" alt="Local Market Bulk" class="relative rounded-2xl shadow-2xl border-8 border-white">
            </div>
        </div>
    </header>

    <!-- B2B Stats -->
    <section class="max-w-7xl mx-auto px-4 -mt-12 relative z-10">
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
            <div class="bg-slate-900 p-8 rounded-xl text-white shadow-xl">
                <div class="text-gab-yellow text-3xl font-black mb-1">50+</div>
                <div class="text-xs font-bold uppercase opacity-60 tracking-widest">Producteurs Locaux</div>
            </div>
            <div class="bg-white p-8 rounded-xl border border-slate-100 shadow-xl">
                <div class="text-gab-green text-3xl font-black mb-1">-30%</div>
                <div class="text-xs font-bold uppercase text-slate-500 tracking-widest">Prix vs Détail</div>
            </div>
            <div class="bg-white p-8 rounded-xl border border-slate-100 shadow-xl">
                <div class="text-gab-blue text-3xl font-black mb-1">48H</div>
                <div class="text-xs font-bold uppercase text-slate-500 tracking-widest">Livraison Province</div>
            </div>
            <div class="bg-white p-8 rounded-xl border border-slate-100 shadow-xl">
                <div class="text-slate-800 text-3xl font-black mb-1">GAB</div>
                <div class="text-xs font-bold uppercase text-slate-500 tracking-widest">Origine Certifiée</div>
            </div>
        </div>
    </section>

    <!-- B2B Catalog -->
    <section class="py-24 max-w-7xl mx-auto px-4">
        <div class="flex flex-col md:flex-row md:items-center justify-between mb-12 gap-4">
            <div>
                <h2 class="text-3xl font-black text-slate-900 uppercase">Catalogue Grossiste</h2>
                <div class="h-1.5 w-20 bg-gab-yellow mt-2"></div>
            </div>
            <div class="flex items-center bg-white p-2 rounded-lg border border-slate-200">
                <button class="px-4 py-2 bg-slate-100 rounded font-bold text-xs text-slate-800">TOUT</button>
                <button class="px-4 py-2 hover:bg-slate-50 rounded font-bold text-xs text-slate-500 transition">ALIMENTAIRE</button>
                <button class="px-4 py-2 hover:bg-slate-50 rounded font-bold text-xs text-slate-500 transition">ARTISANAT</button>
                <button class="px-4 py-2 hover:bg-slate-50 rounded font-bold text-xs text-slate-500 transition">COSMÉTIQUE</button>
            </div>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
            <!-- Product 1: Food -->
            <div class="product-card bg-white border border-slate-100 rounded-xl p-4 shadow-sm hover:shadow-xl transition">
                <div class="aspect-square bg-slate-100 rounded-lg mb-4 relative overflow-hidden">
                    <img src="https://images.unsplash.com/photo-1596040033229-a9821ebd058d?auto=format&fit=crop&w=500&q=80" alt="Café Gabonais" class="w-full h-full object-cover">
                    <div class="absolute top-2 left-2 b2b-badge">M.O.Q: 10 KG</div>
                </div>
                <div class="space-y-1">
                    <span class="text-[10px] font-bold text-gab-green uppercase tracking-tighter">Woleu-Ntem</span>
                    <h3 class="font-black text-slate-800 uppercase text-sm leading-tight">Café Robusta Torréfié (Grain)</h3>
                    <div class="flex items-baseline space-x-1 py-2">
                        <span class="text-xl font-black text-slate-900">7.500</span>
                        <span class="text-[10px] font-bold text-slate-500">FCFA / KG</span>
                    </div>
                    <div class="bg-slate-50 p-2 rounded text-[10px] text-slate-500 mb-4">
                        <i class="fa-solid fa-tags mr-1"></i> Prix dégressif dès 50 KG
                    </div>
                    <button class="w-full py-3 bg-gab-green text-white rounded font-black text-xs hover:bg-green-700 transition uppercase">Devis de Gros</button>
                </div>
            </div>

            <!-- Product 2: Cosmetics -->
            <div class="product-card bg-white border border-slate-100 rounded-xl p-4 shadow-sm hover:shadow-xl transition">
                <div class="aspect-square bg-slate-100 rounded-lg mb-4 relative overflow-hidden">
                    <img src="https://images.unsplash.com/photo-1620916566398-39f1143f2c0a?auto=format&fit=crop&w=500&q=80" alt="Huile de Moabi" class="w-full h-full object-cover">
                    <div class="absolute top-2 left-2 b2b-badge">CARTON: 24 UNITES</div>
                </div>
                <div class="space-y-1">
                    <span class="text-[10px] font-bold text-gab-green uppercase tracking-tighter">Ogooué-Maritime</span>
                    <h3 class="font-black text-slate-800 uppercase text-sm leading-tight">Huile de Moabi Pur (250ml)</h3>
                    <div class="flex items-baseline space-x-1 py-2">
                        <span class="text-xl font-black text-slate-900">3.200</span>
                        <span class="text-[10px] font-bold text-slate-500">FCFA / UNITÉ</span>
                    </div>
                    <div class="bg-slate-50 p-2 rounded text-[10px] text-slate-500 mb-4">
                        <i class="fa-solid fa-boxes-stacked mr-1"></i> Stock disponible: 500+
                    </div>
                    <button class="w-full py-3 bg-gab-green text-white rounded font-black text-xs hover:bg-green-700 transition uppercase">Ajouter au Lot</button>
                </div>
            </div>

            <!-- Product 3: Beverages -->
            <div class="product-card bg-white border border-slate-100 rounded-xl p-4 shadow-sm hover:shadow-xl transition">
                <div class="aspect-square bg-slate-100 rounded-lg mb-4 relative overflow-hidden">
                    <img src="https://images.unsplash.com/photo-1595981267035-7b04ca84a82d?auto=format&fit=crop&w=500&q=80" alt="Jus Naturel" class="w-full h-full object-cover">
                    <div class="absolute top-2 left-2 b2b-badge">PALETTE: 12 PACKS</div>
                </div>
                <div class="space-y-1">
                    <span class="text-[10px] font-bold text-gab-green uppercase tracking-tighter">Estuaire</span>
                    <h3 class="font-black text-slate-800 uppercase text-sm leading-tight">Pack Jus de Bissap Artisanal</h3>
                    <div class="flex items-baseline space-x-1 py-2">
                        <span class="text-xl font-black text-slate-900">12.000</span>
                        <span class="text-[10px] font-bold text-slate-500">FCFA / PACK</span>
                    </div>
                    <div class="bg-slate-50 p-2 rounded text-[10px] text-slate-500 mb-4">
                        <i class="fa-solid fa-truck-moving mr-1"></i> Franco de port dès 5 palettes
                    </div>
                    <button class="w-full py-3 bg-gab-green text-white rounded font-black text-xs hover:bg-green-700 transition uppercase">Commander</button>
                </div>
            </div>

            <!-- Product 4: Construction/Eco -->
            <div class="product-card bg-white border border-slate-100 rounded-xl p-4 shadow-sm hover:shadow-xl transition">
                <div class="aspect-square bg-slate-100 rounded-lg mb-4 relative overflow-hidden">
                    <img src="https://images.unsplash.com/photo-1589939705384-5185137a7f0f?auto=format&fit=crop&w=500&q=80" alt="Briques Terre Cuite" class="w-full h-full object-cover">
                    <div class="absolute top-2 left-2 b2b-badge">CHANTIER: 1000+</div>
                </div>
                <div class="space-y-1">
                    <span class="text-[10px] font-bold text-gab-green uppercase tracking-tighter">Haut-Ogooué</span>
                    <h3 class="font-black text-slate-800 uppercase text-sm leading-tight">Briques Éco-Terre compressées</h3>
                    <div class="flex items-baseline space-x-1 py-2">
                        <span class="text-xl font-black text-slate-900">450</span>
                        <span class="text-[10px] font-bold text-slate-500">FCFA / PIÈCE</span>
                    </div>
                    <div class="bg-slate-50 p-2 rounded text-[10px] text-slate-500 mb-4">
                        <i class="fa-solid fa-hourglass-half mr-1"></i> Délai prod: 15 jours
                    </div>
                    <button class="w-full py-3 bg-slate-800 text-white rounded font-black text-xs hover:bg-black transition uppercase">Demander Devis</button>
                </div>
            </div>
        </div>
    </section>

    <!-- Logistics Section -->
    <section class="bg-slate-900 py-24 text-white">
        <div class="max-w-7xl mx-auto px-4 grid grid-cols-1 md:grid-cols-2 gap-20 items-center">
            <div>
                <h2 class="text-4xl font-black mb-8 uppercase tracking-tighter">Logistique <span class="text-gab-yellow">Omni-Gabon</span></h2>
                <div class="space-y-8">
                    <div class="flex gap-6">
                        <div class="h-12 w-12 bg-gab-green rounded flex items-center justify-center shrink-0">
                            <i class="fa-solid fa-train text-xl"></i>
                        </div>
                        <div>
                            <h4 class="font-bold text-lg">Transport SETRAG</h4>
                            <p class="text-slate-400 text-sm">Gestion des expéditions lourdes via le chemin de fer pour le Haut-Ogooué et l'Ogooué-Lolo.</p>
                        </div>
                    </div>
                    <div class="flex gap-6">
                        <div class="h-12 w-12 bg-gab-blue rounded flex items-center justify-center shrink-0">
                            <i class="fa-solid fa-ship text-xl"></i>
                        </div>
                        <div>
                            <h4 class="font-bold text-lg">Cabotage Maritime</h4>
                            <p class="text-slate-400 text-sm">Liaisons régulières Libreville - Port-Gentil - Gamba pour vos stocks volumineux.</p>
                        </div>
                    </div>
                    <div class="flex gap-6">
                        <div class="h-12 w-12 bg-gab-yellow rounded flex items-center justify-center shrink-0 text-slate-900">
                            <i class="fa-solid fa-van-shuttle text-xl"></i>
                        </div>
                        <div>
                            <h4 class="font-bold text-lg">Dernier Kilomètre</h4>
                            <p class="text-slate-400 text-sm">Flotte de camionnettes pour livraison directe en boutique dans les grands centres urbains.</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="bg-white/5 p-8 rounded-3xl border border-white/10">
                <h3 class="text-2xl font-bold mb-6 italic text-center text-gab-yellow">"Le futur de la distribution au Gabon passe par le local."</h3>
                <div class="space-y-4">
                    <div class="bg-white/10 p-4 rounded-lg">
                        <p class="text-sm font-bold uppercase mb-1">Total Commandes Pro 2024</p>
                        <div class="h-2 w-full bg-white/10 rounded-full overflow-hidden">
                            <div class="h-full bg-gab-green w-3/4"></div>
                        </div>
                        <p class="text-[10px] mt-2 text-slate-400">Objectif: 1 Milliard FCFA de flux local</p>
                    </div>
                    <img src="https://images.unsplash.com/photo-1586528116311-ad8dd3c8310d?auto=format&fit=crop&w=500&q=80" alt="Warehouse" class="rounded-xl opacity-60">
                </div>
            </div>
        </div>
    </section>

    <!-- Simple B2B Footer -->
    <footer class="bg-white border-t border-slate-200 py-12">
        <div class="max-w-7xl mx-auto px-4 flex flex-col md:flex-row justify-between items-center gap-8">
            <div class="flex items-center space-x-2">
                <img src="https://i.ibb.co/2Q73j3X/echoppe241-logo.png" alt="Logo" class="h-8 opacity-50">
                <span class="text-xs font-bold text-slate-400">B2B DIVISION GABON</span>
            </div>
            <div class="flex space-x-8 text-xs font-bold text-slate-500 uppercase tracking-widest">
                <a href="#" class="hover:text-gab-green">Conditions Grossistes</a>
                <a href="#" class="hover:text-gab-green">Devenir Fournisseur</a>
                <a href="#" class="hover:text-gab-green">API & Intégration</a>
            </div>
            <div class="text-xs text-slate-400">
                &copy; 2024 echoppe241 - L'Union, le Travail, la Justice.
            </div>
        </div>
    </footer>

    <script>
        // Modal & Navigation Logic
        const authModal = document.getElementById('authModal');
        const loginForm = document.getElementById('loginForm');
        const registerForm = document.getElementById('registerForm');
        const loginTab = document.getElementById('loginTab');
        const registerTab = document.getElementById('registerTab');

        function toggleAuthModal() {
            authModal.classList.toggle('hidden-modal');
        }

        // Close modal when clicking outside
        authModal.addEventListener('click', (e) => {
            if (e.target === authModal) toggleAuthModal();
        });

        function switchTab(tab) {
            if (tab === 'login') {
                loginForm.classList.remove('hidden');
                registerForm.classList.add('hidden');
                loginTab.classList.add('border-gab-green', 'text-gab-green');
                loginTab.classList.remove('border-transparent', 'text-slate-400');
                registerTab.classList.remove('border-gab-green', 'text-gab-green');
                registerTab.classList.add('border-transparent', 'text-slate-400');
                document.getElementById('modalTitle').innerText = "Portail Professionnel";
            } else {
                loginForm.classList.add('hidden');
                registerForm.classList.remove('hidden');
                registerTab.classList.add('border-gab-green', 'text-gab-green');
                registerTab.classList.remove('border-transparent', 'text-slate-400');
                loginTab.classList.remove('border-gab-green', 'text-gab-green');
                loginTab.classList.add('border-transparent', 'text-slate-400');
                document.getElementById('modalTitle').innerText = "Demander un Compte Pro";
            }
        }

        // Form Submission Mocks
        loginForm.onsubmit = (e) => {
            e.preventDefault();
            alert("Accès en cours de vérification par un administrateur...");
            toggleAuthModal();
        };

        registerForm.onsubmit = (e) => {
            e.preventDefault();
            alert("Demande d'inscription envoyée ! Un commercial vous contactera sous 24h.");
            toggleAuthModal();
        };
    </script>
</body>
</html>
