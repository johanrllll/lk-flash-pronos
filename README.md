# lk-flash-pronos
<!DOCTYPE html>
<html lang="fr" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>lk flash - Scores en direct & Pronostics</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            green: '#00ff88',
                            cyan: '#00e5ff',
                            darkBg: '#0b0f17',
                            card: '#121824',
                            cardBorder: '#1e293b',
                            cardHover: '#1a2332'
                        }
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0b0f17;
            color: #f1f5f9;
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0f17;
        }
        ::-webkit-scrollbar-thumb {
            background: #1e293b;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #00ff88;
        }
        .pulse-live {
            box-shadow: 0 0 0 0 rgba(0, 255, 136, 0.7);
            animation: pulse-green 1.8s infinite;
        }
        @keyframes pulse-green {
            0% { box-shadow: 0 0 0 0 rgba(0, 255, 136, 0.7); }
            70% { box-shadow: 0 0 0 8px rgba(0, 255, 136, 0); }
            100% { box-shadow: 0 0 0 0 rgba(0, 255, 136, 0); }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col bg-[#0b0f17] text-slate-100 antialiased selection:bg-brand-green selection:text-black">

    <!-- HEADER / NAVIGATION -->
    <header class="sticky top-0 z-40 bg-[#0b0f17]/90 backdrop-blur-md border-b border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <!-- Logo -->
                <div class="flex items-center space-x-3 cursor-pointer" onclick="switchMainTab('matches')">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-green to-brand-cyan flex items-center justify-center shadow-lg shadow-brand-green/20">
                        <i class="fa-solid font-black text-slate-950 text-xl italic">lk</i>
                    </div>
                    <div>
                        <span class="text-2xl font-extrabold tracking-wider bg-gradient-to-r from-brand-green via-brand-cyan to-white bg-clip-text text-transparent">lk flash</span>
                        <span class="block text-[10px] text-slate-400 font-semibold tracking-widest uppercase">Live Sport & Pronos</span>
                    </div>
                </div>

                <!-- Main Navigation Tabs (Desktop) -->
                <nav class="hidden md:flex space-x-1 bg-slate-900/80 p-1 rounded-xl border border-slate-800">
                    <button onclick="switchMainTab('matches')" id="nav-btn-matches" class="px-4 py-2 rounded-lg text-sm font-semibold transition-all duration-200 bg-brand-green text-slate-950 shadow">
                        <i class="fa-solid fa-trophy mr-2"></i>Matchs
                    </button>
                    <button onclick="switchMainTab('pronos')" id="nav-btn-pronos" class="px-4 py-2 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-300 hover:text-white hover:bg-slate-800">
                        <i class="fa-solid fa-chart-line mr-2"></i>Pronos & Analyses
                    </button>
                    <button onclick="switchMainTab('chat')" id="nav-btn-chat" class="px-4 py-2 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-300 hover:text-white hover:bg-slate-800 relative">
                        <i class="fa-solid fa-comments mr-2"></i>Tchat Direct
                        <span class="absolute -top-1 -right-1 w-2.5 h-2.5 bg-brand-green rounded-full pulse-live"></span>
                    </button>
                </nav>

                <!-- Right Actions: Search & Favorites -->
                <div class="flex items-center space-x-3">
                    <div class="relative hidden sm:block">
                        <input type="text" id="searchInput" onkeyup="filterMatches()" placeholder="Rechercher une équipe, ligue..." class="w-48 lg:w-64 bg-slate-900 text-sm text-slate-200 placeholder-slate-500 rounded-xl px-4 py-2 pl-9 border border-slate-800 focus:outline-none focus:border-brand-green transition-all">
                        <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-slate-500 text-xs"></i>
                    </div>
                    <button onclick="toggleFavoritesOnly()" id="favToggleBtn" class="p-2.5 bg-slate-900 hover:bg-slate-800 border border-slate-800 rounded-xl text-slate-300 hover:text-yellow-400 transition-all relative" title="Favoris">
                        <i class="fa-solid fa-star"></i>
                        <span id="favCount" class="absolute -top-1 -right-1 bg-yellow-500 text-black font-bold text-[10px] w-4 h-4 rounded-full flex items-center justify-center hidden">0</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Navigation Bar -->
        <div class="md:hidden flex border-t border-slate-800/80 bg-slate-950/90 text-xs font-semibold">
            <button onclick="switchMainTab('matches')" id="mob-btn-matches" class="flex-1 py-3 text-center border-b-2 border-brand-green text-brand-green">
                <i class="fa-solid fa-trophy block text-sm mb-0.5"></i>Matchs
            </button>
            <button onclick="switchMainTab('pronos')" id="mob-btn-pronos" class="flex-1 py-3 text-center border-b-2 border-transparent text-slate-400">
                <i class="fa-solid fa-chart-line block text-sm mb-0.5"></i>Pronos
            </button>
            <button onclick="switchMainTab('chat')" id="mob-btn-chat" class="flex-1 py-3 text-center border-b-2 border-transparent text-slate-400 relative">
                <i class="fa-solid fa-comments block text-sm mb-0.5"></i>Tchat
            </button>
        </div>
    </header>

    <!-- MAIN CONTAINER -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-3 sm:px-6 lg:px-8 py-6">

        <!-- ================= SECTION 1: MATCH CENTER ================= -->
        <section id="section-matches" class="space-y-6">
            
            <!-- Mobile Search Bar -->
            <div class="sm:hidden mb-4">
                <div class="relative">
                    <input type="text" id="mobileSearchInput" onkeyup="filterMatchesMobile()" placeholder="Rechercher une équipe..." class="w-full bg-slate-900 text-sm text-slate-200 placeholder-slate-500 rounded-xl px-4 py-2.5 pl-9 border border-slate-800 focus:outline-none focus:border-brand-green">
                    <i class="fa-solid fa-magnifying-glass absolute left-3 top-3.5 text-slate-500 text-xs"></i>
                </div>
            </div>

            <!-- Sports Filters Header Bar -->
            <div class="flex items-center justify-between overflow-x-auto pb-2 border-b border-slate-800 scrollbar-none gap-2">
                <div class="flex space-x-2" id="sportTabs">
                    <button onclick="filterSport('all')" data-sport="all" class="sport-tab-btn active px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-brand-green text-slate-950 shadow">
                        <i class="fa-solid fa-globe mr-1.5"></i>Tous
                    </button>
                    <button onclick="filterSport('football')" data-sport="football" class="sport-tab-btn px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-slate-900 hover:bg-slate-800 text-slate-300 border border-slate-800">
                        ⚽ Football
                    </button>
                    <button onclick="filterSport('basketball')" data-sport="basketball" class="sport-tab-btn px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-slate-900 hover:bg-slate-800 text-slate-300 border border-slate-800">
                        🏀 Basketball
                    </button>
                    <button onclick="filterSport('tennis')" data-sport="tennis" class="sport-tab-btn px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-slate-900 hover:bg-slate-800 text-slate-300 border border-slate-800">
                        🎾 Tennis
                    </button>
                    <button onclick="filterSport('mma')" data-sport="mma" class="sport-tab-btn px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-slate-900 hover:bg-slate-800 text-slate-300 border border-slate-800">
                        🥊 MMA / UFC
                    </button>
                    <button onclick="filterSport('rugby')" data-sport="rugby" class="sport-tab-btn px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-slate-900 hover:bg-slate-800 text-slate-300 border border-slate-800">
                        🏈 Rugby
                    </button>
                </div>

                <!-- Status Filter Pills -->
                <div class="flex bg-slate-900 p-1 rounded-xl border border-slate-800 shrink-0">
                    <button onclick="filterStatus('all')" id="status-all" class="px-3 py-1 rounded-lg text-xs font-bold bg-slate-800 text-white">Tous</button>
                    <button onclick="filterStatus('live')" id="status-live" class="px-3 py-1 rounded-lg text-xs font-bold text-slate-400 hover:text-white flex items-center gap-1">
                        <span class="w-2 h-2 rounded-full bg-brand-green pulse-live"></span>En Direct
                    </button>
                    <button onclick="filterStatus('upcoming')" id="status-upcoming" class="px-3 py-1 rounded-lg text-xs font-bold text-slate-400 hover:text-white">À venir</button>
                </div>
            </div>

            <!-- Matches Grid -->
            <div id="matchesContainer" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                <!-- JavaScript render target -->
            </div>
            
            <div id="noMatchesMsg" class="hidden text-center py-16 bg-slate-900/40 rounded-2xl border border-slate-800/80">
                <i class="fa-regular fa-folder-open text-4xl text-slate-600 mb-3"></i>
                <p class="text-slate-400 font-medium">Aucun match ne correspond à votre recherche.</p>
            </div>
        </section>


        <!-- ================= SECTION 2: PRONOSTICS & ANALYSES ================= -->
        <section id="section-pronos" class="hidden space-y-6">
            <!-- Header Banner -->
            <div class="bg-gradient-to-r from-slate-900 via-slate-900 to-emerald-950 p-6 rounded-2xl border border-slate-800 relative overflow-hidden">
                <div class="absolute right-0 top-0 bottom-0 opacity-10 flex items-center pr-10">
                    <i class="fa-solid fa-chart-pie text-9xl text-brand-green"></i>
                </div>
                <div class="relative z-10 max-w-2xl">
                    <span class="bg-brand-green/10 text-brand-green border border-brand-green/30 text-xs px-3 py-1 rounded-full font-bold uppercase tracking-wider">
                        ⚡ Pronostics & Analyses Expert
                    </span>
                    <h2 class="text-2xl sm:text-3xl font-black mt-3 text-white">Les Conseils du Jour par lk flash</h2>
                    <p class="text-slate-400 text-sm mt-2">Analyses statistiques poussées, états de forme, absences et probabilités calculées par nos algorithmes sportifs.</p>
                </div>
            </div>

            <!-- Top Prono Sûr Highlight -->
            <div class="bg-gradient-to-br from-slate-900 to-slate-900/90 rounded-2xl p-5 border border-brand-green/40 shadow-xl shadow-brand-green/5 relative">
                <div class="flex items-center justify-between mb-4">
                    <span class="flex items-center gap-2 text-xs font-bold text-brand-green bg-brand-green/10 px-3 py-1.5 rounded-xl border border-brand-green/20">
                        <i class="fa-solid fa-fire text-amber-400"></i> PRONO SÛR DU JOUR
                    </span>
                    <span class="text-xs font-bold text-slate-400 bg-slate-800 px-3 py-1 rounded-xl">Fiabilité 94%</span>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 items-center">
                    <div class="flex items-center justify-around md:justify-start space-x-4">
                        <div class="text-center">
                            <div class="w-14 h-14 bg-slate-800 rounded-2xl flex items-center justify-center text-2xl font-bold border border-slate-700 shadow">RM</div>
                            <span class="text-xs font-bold mt-1 block">Real Madrid</span>
                        </div>
                        <span class="text-lg font-black text-slate-500">VS</span>
                        <div class="text-center">
                            <div class="w-14 h-14 bg-slate-800 rounded-2xl flex items-center justify-center text-2xl font-bold border border-slate-700 shadow">MC</div>
                            <span class="text-xs font-bold mt-1 block">Man. City</span>
                        </div>
                    </div>

                    <div class="space-y-2 bg-slate-950/60 p-4 rounded-xl border border-slate-800">
                        <div class="flex justify-between text-xs">
                            <span class="text-slate-400">Recommandation :</span>
                            <span class="font-bold text-brand-green">Victoire Real Madrid ou Nul + Plus de 1.5 buts</span>
                        </div>
                        <div class="flex justify-between text-xs">
                            <span class="text-slate-400">Cote Moyenne :</span>
                            <span class="font-bold text-brand-cyan">1.72</span>
                        </div>
                        <div class="w-full bg-slate-800 h-2 rounded-full overflow-hidden flex mt-2">
                            <div class="bg-brand-green h-full" style="width: 60%" title="Victoire Domicile 60%"></div>
                            <div class="bg-slate-500 h-full" style="width: 25%" title="Nul 25%"></div>
                            <div class="bg-red-500 h-full" style="width: 15%" title="Victoire Extérieur 15%"></div>
                        </div>
                    </div>

                    <div class="text-xs text-slate-300 space-y-2">
                        <p class="leading-relaxed"><i class="fa-solid fa-circle-check text-brand-green mr-1.5"></i> Le Real Madrid est invaincu lors de ses 12 derniers matchs à domicile.</p>
                        <p class="leading-relaxed"><i class="fa-solid fa-circle-check text-brand-green mr-1.5"></i> Man. City se déplace sans 2 défenseurs titulaires.</p>
                    </div>
                </div>
            </div>

            <!-- Pronostics Grid -->
            <div id="pronosList" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- Rendered dynamically -->
            </div>
        </section>


        <!-- ================= SECTION 3: TCHAT COMMUNAUTÉ ================= -->
        <section id="section-chat" class="hidden space-y-4">
            <div class="grid grid-cols-1 lg:grid-cols-4 gap-6 h-[calc(100vh-14rem)] min-h-[500px]">
                
                <!-- Chat Feed Card -->
                <div class="lg:col-span-3 bg-slate-900/90 rounded-2xl border border-slate-800 flex flex-col h-full overflow-hidden shadow-2xl">
                    <!-- Chat Header -->
                    <div class="p-4 bg-slate-950 border-b border-slate-800 flex items-center justify-between">
                        <div class="flex items-center space-x-3">
                            <div class="w-3 h-3 rounded-full bg-brand-green pulse-live"></div>
                            <div>
                                <h3 class="font-bold text-sm text-white">Salon de Discussion en Direct</h3>
                                <p class="text-[11px] text-slate-400"><span id="onlineUsersCount">142</span> passionnés en ligne</p>
                            </div>
                        </div>
                        <div class="flex items-center space-x-2">
                            <input type="text" id="usernameInput" value="Supporter_lk" placeholder="Votre Pseudo" class="bg-slate-800 text-xs px-3 py-1.5 rounded-lg border border-slate-700 text-slate-200 focus:outline-none focus:border-brand-green w-32 sm:w-40">
                        </div>
                    </div>

                    <!-- Messages Box -->
                    <div id="chatMessages" class="flex-1 p-4 overflow-y-auto space-y-4">
                        <!-- Messages dynamically added here -->
                    </div>

                    <!-- Chat Input Box -->
                    <form onsubmit="sendChatMessage(event)" class="p-3 bg-slate-950 border-t border-slate-800 flex gap-2 items-center">
                        <input type="text" id="chatInput" placeholder="Partagez votre avis ou votre pronostique..." class="flex-1 bg-slate-900 text-sm text-slate-200 placeholder-slate-500 px-4 py-2.5 rounded-xl border border-slate-800 focus:outline-none focus:border-brand-green">
                        <button type="submit" class="bg-gradient-to-r from-brand-green to-brand-cyan text-slate-950 font-bold px-5 py-2.5 rounded-xl hover:opacity-90 transition-all shadow-md">
                            <i class="fa-solid fa-paper-plane"></i>
                        </button>
                    </form>
                </div>

                <!-- Live Community Poll / Trends -->
                <div class="hidden lg:flex flex-col space-y-4">
                    <div class="bg-slate-900 rounded-2xl p-4 border border-slate-800">
                        <h4 class="font-bold text-xs uppercase tracking-wider text-slate-400 mb-3 flex items-center gap-2">
                            <i class="fa-solid fa-square-poll-vertical text-brand-green"></i> Sondage Flash
                        </h4>
                        <p class="text-xs font-semibold mb-3">Qui va remporter le choc Real Madrid vs Man. City ?</p>
                        <div class="space-y-2 text-xs">
                            <button onclick="votePoll('real')" class="w-full text-left p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 transition flex justify-between items-center border border-slate-700">
                                <span>Real Madrid</span>
                                <span class="font-bold text-brand-green" id="poll-real-pct">58%</span>
                            </button>
                            <button onclick="votePoll('city')" class="w-full text-left p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 transition flex justify-between items-center border border-slate-700">
                                <span>Man. City</span>
                                <span class="font-bold text-slate-400" id="poll-city-pct">32%</span>
                            </button>
                            <button onclick="votePoll('draw')" class="w-full text-left p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 transition flex justify-between items-center border border-slate-700">
                                <span>Match Nul</span>
                                <span class="font-bold text-slate-400" id="poll-draw-pct">10%</span>
                            </button>
                        </div>
                    </div>

                    <div class="bg-slate-900 rounded-2xl p-4 border border-slate-800 flex-1">
                        <h4 class="font-bold text-xs uppercase tracking-wider text-slate-400 mb-3 flex items-center gap-2">
                            <i class="fa-solid fa-shield-halved text-brand-cyan"></i> Règles du Tchat
                        </h4>
                        <ul class="text-xs text-slate-400 space-y-2 leading-relaxed">
                            <li>• Respectez tous les supporters.</li>
                            <li>• Pas de spam ni de pubs d'affiliation.</li>
                            <li>• Partagez vos pronos dans la bonne humeur !</li>
                        </ul>
                    </div>
                </div>

            </div>
        </section>

    </main>

    <!-- ================= MODAL DETAILED MATCH INFO ================= -->
    <div id="matchModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-3 sm:p-4">
        <div class="bg-slate-900 border border-slate-800 rounded-2xl w-full max-w-2xl max-h-[90vh] overflow-y-auto shadow-2xl relative">
            <!-- Close Button -->
            <button onclick="closeMatchModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white bg-slate-800 rounded-full w-8 h-8 flex items-center justify-center z-10">
                <i class="fa-solid fa-xmark"></i>
            </button>
            <div id="modalContent" class="p-6">
                <!-- Rendered dynamically -->
            </div>
        </div>
    </div>

    <!-- FOOTER -->
    <footer class="mt-auto border-t border-slate-800 bg-slate-950/80 py-6">
        <div class="max-w-7xl mx-auto px-4 text-center text-slate-500 text-xs space-y-2">
            <p class="font-bold text-slate-400">lk flash © 2026 - Plateforme Mondiale de Live Scores & Analyses Sportives</p>
            <p>Toutes les données en direct sont actualisées régulièrement à des fins d'information et de divertissement.</p>
        </div>
    </footer>


    <!-- JAVASCRIPT LOGIC -->
    <script>
        // --- DATA STORE ---
        let currentSportFilter = 'all';
        let currentStatusFilter = 'all';
        let favoritesOnly = false;
        let favorites = JSON.parse(localStorage.getItem('lk_flash_favs') || '[]');

        const matchesData = [
            {
                id: 1,
                sport: 'football',
                league: 'UEFA Champions League',
                status: 'LIVE',
                minute: '67\'',
                homeTeam: 'Real Madrid',
                homeCode: 'RM',
                awayTeam: 'Man. City',
                awayCode: 'MC',
                homeScore: 2,
                awayScore: 1,
                odds: { home: 2.10, draw: 3.40, away: 2.90 },
                h2h: '3 victoires Real, 1 Nul, 1 victoire City sur les 5 derniers affrontements.',
                stats: { possession: '54% - 46%', tirs: '14 - 9', cadred: '6 - 3' }
            },
            {
                id: 2,
                sport: 'football',
                league: 'Ligue 1 McDonald\'s',
                status: 'LIVE',
                minute: '32\'',
                homeTeam: 'PSG',
                homeCode: 'PSG',
                awayTeam: 'Marseille',
                awayCode: 'OM',
                homeScore: 1,
                awayScore: 0,
                odds: { home: 1.55, draw: 4.20, away: 5.50 },
                h2h: 'PSG invaincu à domicile lors du Classico depuis 2020.',
                stats: { possession: '62% - 38%', tirs: '8 - 2', cadred: '4 - 1' }
            },
            {
                id: 3,
                sport: 'basketball',
                league: 'NBA',
                status: 'LIVE',
                minute: 'Q3 - 04:12',
                homeTeam: 'LA Lakers',
                homeCode: 'LAL',
                awayTeam: 'Golden State',
                awayCode: 'GSW',
                homeScore: 88,
                awayScore: 92,
                odds: { home: 1.85, draw: null, away: 1.95 },
                h2h: 'Match très équilibré. Curry à 28 pts de moyenne.',
                stats: { 3pts: '42% - 48%', rebonds: '31 - 28', passes: '19 - 22' }
            },
            {
                id: 4,
                sport: 'tennis',
                league: 'Roland Garros - Demi-finale',
                status: 'UPCOMING',
                minute: 'Aujourd\'hui 15:30',
                homeTeam: 'C. Alcaraz',
                homeCode: 'ALC',
                awayTeam: 'J. Sinner',
                awayCode: 'SIN',
                homeScore: 0,
                awayScore: 0,
                odds: { home: 1.75, draw: null, away: 2.10 },
                h2h: '4-4 dans leurs confrontations directes sur terre battue.',
                stats: { acesAveraged: '6.2 - 8.1', breakPointsSaved: '68% - 71%' }
            },
            {
                id: 5,
                sport: 'mma',
                league: 'UFC 312 - Main Event',
                status: 'UPCOMING',
                minute: 'Ce soir 23:00',
                homeTeam: 'Jon Jones',
                homeCode: 'BONES',
                awayTeam: 'Tom Aspinall',
                awayCode: 'TOM',
                homeScore: 0,
                awayScore: 0,
                odds: { home: 1.65, draw: null, away: 2.25 },
                h2h: 'Premier choc chez les Poids Lourds.',
                stats: { takedownAcc: '45% - 60%', KO_Rate: '50% - 85%' }
            },
            {
                id: 6,
                sport: 'rugby',
                league: '6 Nations',
                status: 'UPCOMING',
                minute: 'Demain 16:00',
                homeTeam: 'France',
                homeCode: 'FRA',
                awayTeam: 'Angleterre',
                awayCode: 'ENG',
                homeScore: 0,
                awayScore: 0,
                odds: { home: 1.40, draw: 22.0, away: 3.10 },
                h2h: 'La France a remporté le Crunch l\'année dernière.',
                stats: { essaisMoy: '3.4 - 2.1' }
            }
        ];

        const pronosData = [
            {
                id: 1,
                match: 'PSG vs Marseille',
                sport: '⚽ Football',
                fiabilite: '88%',
                prono: 'Plus de 2.5 Buts dans le match',
                cote: '1.62',
                analyse: 'Le PSG marque en moyenne 2.4 buts par match à domicile. Marseille possède une attaque rapide en contre-attaque.'
            },
            {
                id: 2,
                match: 'LA Lakers vs Golden State',
                sport: '🏀 Basketball',
                fiabilite: '85%',
                prono: 'Golden State gagne ou perd de moins de 5 pts',
                cote: '1.80',
                analyse: 'Les Warriors sont sur une série de 4 victoires d\'affilée à l\'extérieur.'
            },
            {
                id: 3,
                match: 'Alcaraz vs Sinner',
                sport: '🎾 Tennis',
                fiabilite: '91%',
                prono: 'Plus de 3.5 Sets dans le match',
                cote: '1.50',
                analyse: 'Leurs affrontements récents ont systématiquement dépassé le seuil des 3 sets.'
            }
        ];

        // Default chat messages
        let chatMessages = JSON.parse(localStorage.getItem('lk_flash_chat') || 'null') || [
            { user: 'Coach_K', text: 'Real Madrid pousse fort en cette 2ème mi-temps !', time: '14:20', isMe: false },
            { user: 'PronoKing', text: 'Le prono Real H2H à 1.72 va passer tranquille 💪', time: '14:22', isMe: false },
            { user: 'Alex_Sport', text: 'Sinner contre Alcaraz à 15h30 ça va être incroyable.', time: '14:25', isMe: false }
        ];

        // --- INITIALIZATION ---
        window.onload = function() {
            renderMatches();
            renderPronos();
            renderChat();
            updateFavCount();
            startLiveUpdatesSim();
        };

        // --- RENDER MATCHES ---
        function renderMatches() {
            const container = document.getElementById('matchesContainer');
            const noMatches = document.getElementById('noMatchesMsg');
            const searchVal = (document.getElementById('searchInput').value || document.getElementById('mobileSearchInput').value || '').toLowerCase();

            let filtered = matchesData.filter(m => {
                const matchSport = currentSportFilter === 'all' || m.sport === currentSportFilter;
                const matchStatus = currentStatusFilter === 'all' || 
                    (currentStatusFilter === 'live' && m.status === 'LIVE') || 
                    (currentStatusFilter === 'upcoming' && m.status === 'UPCOMING');
                const isFav = !favoritesOnly || favorites.includes(m.id);
                const matchesSearch = m.homeTeam.toLowerCase().includes(searchVal) || 
                                     m.awayTeam.toLowerCase().includes(searchVal) || 
                                     m.league.toLowerCase().includes(searchVal);

                return matchSport && matchStatus && isFav && matchesSearch;
            });

            if (filtered.length === 0) {
                container.innerHTML = '';
                noMatches.classList.remove('hidden');
                return;
            } else {
                noMatches.classList.add('hidden');
            }

            container.innerHTML = filtered.map(m => {
                const isFav = favorites.includes(m.id);
                const isLive = m.status === 'LIVE';

                return `
                <div class="bg-slate-900 hover:bg-slate-900/90 border border-slate-800 hover:border-slate-700 rounded-2xl p-4 transition-all duration-200 shadow-lg relative group">
                    <!-- Top Info Bar -->
                    <div class="flex items-center justify-between text-xs text-slate-400 mb-3 pb-2 border-b border-slate-800/80">
                        <span class="font-semibold text-slate-300 truncate max-w-[180px]">
                            ${getSportEmoji(m.sport)} ${m.league}
                        </span>
                        <div class="flex items-center space-x-2">
                            ${isLive ? `
                                <span class="bg-brand-green/10 text-brand-green px-2 py-0.5 rounded-md font-bold text-[11px] flex items-center gap-1">
                                    <span class="w-1.5 h-1.5 rounded-full bg-brand-green pulse-live"></span> ${m.minute}
                                </span>
                            ` : `
                                <span class="bg-slate-800 text-slate-400 px-2 py-0.5 rounded-md font-semibold text-[11px]">
                                    ${m.minute}
                                </span>
                            `}
                            <button onclick="toggleFav(${m.id}, event)" class="text-slate-500 hover:text-yellow-400 transition ${isFav ? 'text-yellow-400' : ''}">
                                <i class="fa-${isFav ? 'solid' : 'regular'} fa-star"></i>
                            </button>
                        </div>
                    </div>

                    <!-- Teams & Scores -->
                    <div onclick="openMatchModal(${m.id})" class="cursor-pointer space-y-3 my-2">
                        <!-- Home Team -->
                        <div class="flex items-center justify-between">
                            <div class="flex items-center space-x-3">
                                <div class="w-8 h-8 bg-slate-800 border border-slate-700 rounded-lg flex items-center justify-center font-bold text-xs text-slate-200">
                                    ${m.homeCode}
                                </div>
                                <span class="font-bold text-sm text-slate-100">${m.homeTeam}</span>
                            </div>
                            <span class="text-lg font-black ${isLive ? 'text-brand-green' : 'text-slate-300'}">${m.homeScore}</span>
                        </div>

                        <!-- Away Team -->
                        <div class="flex items-center justify-between">
                            <div class="flex items-center space-x-3">
                                <div class="w-8 h-8 bg-slate-800 border border-slate-700 rounded-lg flex items-center justify-center font-bold text-xs text-slate-200">
                                    ${m.awayCode}
                                </div>
                                <span class="font-bold text-sm text-slate-100">${m.awayTeam}</span>
                            </div>
                            <span class="text-lg font-black ${isLive ? 'text-brand-green' : 'text-slate-300'}">${m.awayScore}</span>
                        </div>
                    </div>

                    <!-- Quick Odds Bar -->
                    <div class="mt-4 pt-3 border-t border-slate-800/80 flex items-center justify-between text-xs">
                        <span class="text-slate-500 font-medium">Cotes 1X2</span>
                        <div class="flex space-x-1.5">
                            <span class="bg-slate-950 px-2.5 py-1 rounded-md text-slate-300 font-bold border border-slate-800">1: <span class="text-brand-cyan">${m.odds.home}</span></span>
                            ${m.odds.draw ? `<span class="bg-slate-950 px-2.5 py-1 rounded-md text-slate-300 font-bold border border-slate-800">X: <span class="text-slate-400">${m.odds.draw}</span></span>` : ''}
                            <span class="bg-slate-950 px-2.5 py-1 rounded-md text-slate-300 font-bold border border-slate-800">2: <span class="text-brand-cyan">${m.odds.away}</span></span>
                        </div>
                    </div>
                </div>
                `;
            }).join('');
        }

        // --- RENDER PRONOSTICS ---
        function renderPronos() {
            const container = document.getElementById('pronosList');
            container.innerHTML = pronosData.map(p => `
                <div class="bg-slate-900 p-5 rounded-2xl border border-slate-800 space-y-3">
                    <div class="flex justify-between items-center">
                        <span class="text-xs font-semibold text-slate-400">${p.sport}</span>
                        <span class="text-xs font-bold bg-brand-green/10 text-brand-green px-2.5 py-1 rounded-full border border-brand-green/20">
                            Fiabilité ${p.fiabilite}
                        </span>
                    </div>
                    <h3 class="font-black text-lg text-white">${p.match}</h3>
                    <div class="bg-slate-950 p-3 rounded-xl border border-slate-800/80 flex justify-between items-center">
                        <span class="text-xs text-slate-300 font-semibold">${p.prono}</span>
                        <span class="text-xs font-black text-brand-cyan bg-slate-900 px-2.5 py-1 rounded-lg border border-slate-800">Cote ${p.cote}</span>
                    </div>
                    <p class="text-xs text-slate-400 leading-relaxed">${p.analyse}</p>
                </div>
            `).join('');
        }

        // --- CHAT LOGIC ---
        function renderChat() {
            const box = document.getElementById('chatMessages');
            box.innerHTML = chatMessages.map(m => `
                <div class="flex flex-col ${m.isMe ? 'items-end' : 'items-start'}">
                    <div class="flex items-baseline space-x-2 mb-1">
                        <span class="text-xs font-bold ${m.isMe ? 'text-brand-green' : 'text-slate-300'}">${m.user}</span>
                        <span class="text-[10px] text-slate-500">${m.time}</span>
                    </div>
                    <div class="max-w-[85%] rounded-2xl px-4 py-2.5 text-xs leading-relaxed shadow ${
                        m.isMe 
                        ? 'bg-gradient-to-r from-brand-green to-brand-cyan text-slate-950 font-medium rounded-tr-none' 
                        : 'bg-slate-800 text-slate-200 border border-slate-700/60 rounded-tl-none'
                    }">
                        ${m.text}
                    </div>
                </div>
            `).join('');
            box.scrollTop = box.scrollHeight;
        }

        function sendChatMessage(e) {
            e.preventDefault();
            const input = document.getElementById('chatInput');
            const username = document.getElementById('usernameInput').value || 'Supporter';
            const text = input.value.trim();

            if (!text) return;

            const now = new Date();
            const timeStr = `${String(now.getHours()).padStart(2, '0')}:${String(now.getMinutes()).padStart(2, '0')}`;

            const newMsg = { user: username, text: text, time: timeStr, isMe: true };
            chatMessages.push(newMsg);
            localStorage.setItem('lk_flash_chat', JSON.stringify(chatMessages));

            input.value = '';
            renderChat();
        }

        // --- FILTERS & TABS NAVIGATION ---
        function switchMainTab(tab) {
            ['matches', 'pronos', 'chat'].forEach(t => {
                document.getElementById(`section-${t}`).classList.add('hidden');
                
                // Desktop nav highlight
                const navBtn = document.getElementById(`nav-btn-${t}`);
                if (navBtn) {
                    navBtn.className = t === tab 
                        ? 'px-4 py-2 rounded-lg text-sm font-semibold transition-all duration-200 bg-brand-green text-slate-950 shadow'
                        : 'px-4 py-2 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-300 hover:text-white hover:bg-slate-800';
                }

                // Mobile nav highlight
                const mobBtn = document.getElementById(`mob-btn-${t}`);
                if (mobBtn) {
                    mobBtn.className = t === tab 
                        ? 'flex-1 py-3 text-center border-b-2 border-brand-green text-brand-green font-bold'
                        : 'flex-1 py-3 text-center border-b-2 border-transparent text-slate-400';
                }
            });

            document.getElementById(`section-${tab}`).classList.remove('hidden');
        }

        function filterSport(sport) {
            currentSportFilter = sport;
            document.querySelectorAll('.sport-tab-btn').forEach(btn => {
                if (btn.dataset.sport === sport) {
                    btn.className = 'sport-tab-btn active px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-brand-green text-slate-950 shadow';
                } else {
                    btn.className = 'sport-tab-btn px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-slate-900 hover:bg-slate-800 text-slate-300 border border-slate-800';
                }
            });
            renderMatches();
        }

        function filterStatus(status) {
            currentStatusFilter = status;
            ['all', 'live', 'upcoming'].forEach(s => {
                const btn = document.getElementById(`status-${s}`);
                if (s === status) {
                    btn.className = 'px-3 py-1 rounded-lg text-xs font-bold bg-slate-800 text-white';
                } else {
                    btn.className = 'px-3 py-1 rounded-lg text-xs font-bold text-slate-400 hover:text-white';
                }
            });
            renderMatches();
        }

        function filterMatches() {
            renderMatches();
        }

        function filterMatchesMobile() {
            renderMatches();
        }

        function toggleFavoritesOnly() {
            favoritesOnly = !favoritesOnly;
            const btn = document.getElementById('favToggleBtn');
            if (favoritesOnly) {
                btn.classList.add('border-yellow-500', 'text-yellow-400');
            } else {
                btn.classList.remove('border-yellow-500', 'text-yellow-400');
            }
            renderMatches();
        }

        function toggleFav(id, e) {
            e.stopPropagation();
            if (favorites.includes(id)) {
                favorites = favorites.filter(favId => favId !== id);
            } else {
                favorites.push(id);
            }
            localStorage.setItem('lk_flash_favs', JSON.stringify(favorites));
            updateFavCount();
            renderMatches();
        }

        function updateFavCount() {
            const badge = document.getElementById('favCount');
            if (favorites.length > 0) {
                badge.innerText = favorites.length;
                badge.classList.remove('hidden');
            } else {
                badge.classList.add('hidden');
            }
        }

        // --- MATCH DETAILS MODAL ---
        function openMatchModal(id) {
            const m = matchesData.find(item => item.id === id);
            if (!m) return;

            const modal = document.getElementById('matchModal');
            const content = document.getElementById('modalContent');

            content.innerHTML = `
                <div class="text-center border-b border-slate-800 pb-4 mb-4">
                    <span class="text-xs font-bold text-brand-green uppercase tracking-widest">${m.league}</span>
                    <div class="flex items-center justify-center space-x-6 mt-4">
                        <div class="text-center">
                            <div class="w-16 h-16 bg-slate-800 rounded-2xl flex items-center justify-center text-xl font-bold border border-slate-700 shadow mx-auto">
                                ${m.homeCode}
                            </div>
                            <span class="font-bold text-sm mt-2 block">${m.homeTeam}</span>
                        </div>
                        <div class="text-center">
                            <span class="text-3xl font-black text-brand-green">${m.homeScore} - ${m.awayScore}</span>
                            <span class="block text-xs text-slate-400 mt-1 font-semibold">${m.minute}</span>
                        </div>
                        <div class="text-center">
                            <div class="w-16 h-16 bg-slate-800 rounded-2xl flex items-center justify-center text-xl font-bold border border-slate-700 shadow mx-auto">
                                ${m.awayCode}
                            </div>
                            <span class="font-bold text-sm mt-2 block">${m.awayTeam}</span>
                        </div>
                    </div>
                </div>

                <div class="space-y-4">
                    <div>
                        <h4 class="text-xs font-bold uppercase text-slate-400 mb-2">Historique H2H</h4>
                        <p class="text-xs text-slate-300 bg-slate-950 p-3 rounded-xl border border-slate-800">${m.h2h}</p>
                    </div>

                    <div>
                        <h4 class="text-xs font-bold uppercase text-slate-400 mb-2">Statistiques de la rencontre</h4>
                        <div class="bg-slate-950 p-4 rounded-xl border border-slate-800 space-y-3">
                            ${Object.entries(m.stats).map(([key, val]) => `
                                <div class="flex justify-between text-xs">
                                    <span class="text-slate-400 capitalize">${key}</span>
                                    <span class="font-bold text-brand-cyan">${val}</span>
                                </div>
                            `).join('')}
                        </div>
                    </div>
                </div>
            `;

            modal.classList.remove('hidden');
        }

        function closeMatchModal() {
            document.getElementById('matchModal').classList.add('hidden');
        }

        // --- POLL VOTE SIMULATION ---
        function votePoll(option) {
            if (option === 'real') {
                document.getElementById('poll-real-pct').innerText = '59%';
            } else if (option === 'city') {
                document.getElementById('poll-city-pct').innerText = '33%';
            }
        }

        // --- UTILS ---
        function getSportEmoji(sport) {
            switch(sport) {
                case 'football': return '⚽';
                case 'basketball': return '🏀';
                case 'tennis': return '🎾';
                case 'mma': return '🥊';
                case 'rugby': return '🏈';
                default: return '🏆';
            }
        }

        // Live Dynamic Updates Simulation
        function startLiveUpdatesSim() {
            setInterval(() => {
                // Randomly toggle score or minutes for dynamic feel
                const liveMatch = matchesData.find(m => m.status === 'LIVE' && m.sport === 'football');
                if (liveMatch) {
                    const minNum = parseInt(liveMatch.minute) || 67;
                    if (minNum < 90) {
                        liveMatch.minute = `${minNum + 1}'`;
                        renderMatches();
                    }
                }
            }, 10000);
        }
    </script>
</body>
</html>