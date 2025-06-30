---
title: "Analyseur de Statistiques AntennaPod"
order: 7
in_menu: true
---
<style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8; /* Light blue-gray background */
            color: #2d3748; /* Darker text */
        }
        .container {
            max-width: 1200px; /* Wider container for bento layout */
            margin: 40px auto;
            padding: 20px;
            background-color: #ffffff;
            border-radius: 16px; /* More rounded */
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1); /* Stronger shadow */
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px; /* Spacing between cards */
        }
        /* Grid for main sections if needed, or keep as flow */
        @media (min-width: 768px) {
            .main-content-grid {
                display: grid;
                grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
                gap: 20px;
            }
            .full-width-card {
                grid-column: 1 / -1; /* Spans all columns */
            }
        }

        .card {
            background-color: #ffffff; /* White background for cards */
            border-radius: 12px; /* Consistent rounded corners */
            padding: 25px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08); /* Subtle shadow */
            border: 1px solid #e2e8f0; /* Light border */
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }
        .card:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
        }
        .file-input-wrapper {
            position: relative;
            overflow: hidden;
            display: inline-block;
            border: 2px dashed #60a3ac; /* Lighter blue-green dashed border */
            border-radius: 10px;
            padding: 20px 30px;
            cursor: pointer;
            transition: all 0.3s ease;
            background-color: #e6f2f3; /* Very light blue-green background */
        }
        .file-input-wrapper:hover {
            border-color: #0e6270;
            background-color: #d8e9eb;
        }
        .button {
            background-color: #0e6270; /* Primary blue-green */
            color: white;
            padding: 12px 25px;
            border-radius: 10px;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            font-weight: 600;
        }
        .button:hover {
            background-color: #0a4c57; /* Darker blue-green */
            transform: translateY(-1px);
        }
        .chart-container {
            position: relative;
            height: 300px;
            width: 100%;
            margin-top: 20px;
            /* Ensure enough height for labels on horizontal bar charts */
            min-height: 300px; /* Base minimum height */
        }
        /* Adjust chart container height dynamically for horizontal bar chart */
        #podcastEpisodeCountChart-container {
            min-height: auto; /* Remove fixed min-height */
            /* This will be dynamically set in JS based on number of labels */
        }

        .loading-overlay {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(255, 255, 255, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 16px;
            z-index: 10;
        }
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: #0e6270;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        /* Message Box */
        .message-box {
            padding: 15px;
            margin-bottom: 20px; /* Adjusted margin */
            border-radius: 8px;
            font-weight: bold;
            display: none;
            text-align: center;
        }
        .message-box.success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        .message-box.error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        .message-box.info {
            background-color: #e6f2f3; /* Lightest shade of new color */
            color: #0e6270; /* Base color */
            border: 1px solid #60a3ac; /* Lighter shade of new color */
        }
        textarea {
            width: 100%;
            height: 150px;
            padding: 15px; /* More padding */
            border-radius: 8px;
            border: 1px solid #cbd5e0; /* Lighter border */
            resize: vertical;
            background-color: #edf2f7; /* Light gray background */
            color: #2d3748;
            font-size: 0.95rem;
        }
        .podcast-item {
            display: flex;
            align-items: center;
            margin-bottom: 8px; /* Slightly less margin */
            padding: 5px 0;
        }
        .podcast-item img {
            width: 40px; /* Slightly smaller images */
            height: 40px;
            border-radius: 6px;
            margin-right: 12px;
            object-fit: cover;
            flex-shrink: 0; /* Prevent shrinking */
        }
        #wrappedContent {
            padding: 25px;
            background-color: #e2e8f0; /* Soft background for wrapped */
            border-radius: 12px;
            margin-top: 15px;
            position: relative;
            text-align: center;
            font-family: 'Inter', sans-serif; /* Ensure font is applied for capture */
            color: #2d3748;
        }
        #wrappedContent h3 {
            color: #083e45; /* A darker shade of new color */
            font-size: 1.6rem;
            margin-bottom: 15px;
            font-weight: 700;
        }
        #wrappedContent p {
            color: #4a5568;
            font-size: 1.05rem;
            line-height: 1.6;
            margin-bottom: 12px;
        }
        #wrappedContent .wrapped-list {
            list-style: none;
            padding: 0;
            text-align: left;
            max-width: 90%; /* Wider list */
            margin: 0 auto 20px auto;
        }
        #wrappedContent .wrapped-list li {
            background-color: #ffffff; /* White background for list items */
            padding: 10px 15px;
            border-radius: 8px;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            font-size: 0.95rem;
        }
        #wrappedContent .wrapped-list li .icon {
            margin-right: 10px;
            font-size: 1.3em;
            color: #0e6270; /* New primary color */
        }
        .export-buttons {
            display: flex;
            gap: 15px; /* More space */
            justify-content: center; /* Center buttons */
            margin-top: 20px;
        }
        /* Podcast search input with suggestions */
        .podcast-search-container {
            position: relative;
            width: 100%;
        }
        .podcast-suggestions {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background-color: #fff;
            border: 1px solid #cbd5e0;
            border-radius: 0 0 8px 8px;
            max-height: 200px;
            overflow-y: auto;
            z-index: 10;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
        }
        .podcast-suggestion-item {
            padding: 10px 15px;
            cursor: pointer;
            transition: background-color 0.2s ease;
        }
        .podcast-suggestion-item:hover {
            background-color: #f0f4f8; /* Lighter hover */
        }
        .search-clear-button {
            background-color: #e53e3e;
            color: white;
            padding: 10px 15px; /* Consistent padding */
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            margin-left: 10px;
            font-weight: 600;
        }
        .search-clear-button:hover {
            background-color: #c53030;
        }
        .filter-section {
            background-color: #f0f4f8; /* Light background for filter section */
            border-radius: 16px;
            padding: 25px;
            margin-bottom: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
            border: 1px solid #e2e8f0;
        }
        .filter-section h2 {
            font-size: 1.5rem;
            font-weight: 700;
            color: #083e45; /* Darker shade of new color */
            margin-bottom: 20px;
            text-align: center;
        }
        .filter-group {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
        }
        .filter-group label {
            flex-shrink: 0;
            margin-right: 15px;
            font-weight: 600;
            color: #4a5568;
            width: 150px; /* Fixed width for labels */
        }
        .filter-group select, .filter-group input[type="text"] {
            flex-grow: 1;
            padding: 10px 15px;
            border: 1px solid #cbd5e0;
            border-radius: 8px;
            background-color: #ffffff;
            color: #2d3748;
            font-size: 1rem;
        }
        .chart-period-info {
            text-align: center;
            font-size: 0.9rem;
            color: #0e6270; /* New primary color */
            margin-bottom: 10px;
            font-weight: 500;
        }
        #topPodcastsTimeListFull, #monthlyPodcastListFull, #podcastStatsListFull {
            max-height: 300px; /* Initial height for scrollable content */
            overflow-y: hidden; /* Hidden by default */
            transition: max-height 0.5s ease-out;
        }
        #topPodcastsTimeListFull.expanded, #monthlyPodcastListFull.expanded, #podcastStatsListFull.expanded {
            max-height: 1000px; /* Arbitrarily large value to show all */
            overflow-y: auto;
        }
    </style>
    <!-- Chart.js for visualizations -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- JSZip for creating zip files -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.7.1/jszip.min.js"></script>
    <!-- FileSaver.js for saving files -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>
    <!-- html2canvas for image export -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<body class="bg-gray-100 p-6">
    <div class="container">
        <h1 class="text-3xl font-bold text-center mb-6 text-gray-800">
            🎧 Analyseur de Statistiques AntennaPod
        </h1>

        <div id="messageBox" class="message-box"></div>

        <div class="card mb-6">
            <h2 class="text-xl font-semibold mb-4 text-gray-700">1. Uploader votre base de données AntennaPod (.db)</h2>
            <p class="text-gray-600 mb-4">
                Veuillez sélectionner le fichier `AntennaPod.db` de votre installation AntennaPod.
                Vous pouvez généralement le trouver dans le dossier de données de l'application sur votre appareil Android.
                <br><br>
                <span class="text-sm text-red-500">Note: Les images de couverture des podcasts sont souvent stockées localement sur votre appareil et ne peuvent pas être affichées directement dans cette application web pour des raisons de sécurité. Des images de remplacement génériques seront utilisées.</span>
            </p>
            <div class="flex flex-col items-center justify-center border-2 border-dashed border-gray-400 rounded-lg p-6 text-center bg-white hover:border-blue-400 transition-colors duration-200">
                <label for="db-file-input" class="cursor-pointer">
                    <div class="flex flex-col items-center">
                        <svg class="w-12 h-12 text-gray-500 mb-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path></svg>
                        <span class="text-lg text-gray-700 font-medium">Déposez votre fichier ici ou cliquez pour uploader</span>
                        <span id="fileName" class="text-sm text-gray-500 mt-1">Aucun fichier sélectionné</span>
                    </div>
                </label>
                <input type="file" id="db-file-input" accept=".db" class="hidden" />
            </div>
            <button id="analyzeButton" class="button w-full mt-6 flex items-center justify-center" disabled>
                <span id="analyzeButtonText">Analyser les statistiques</span>
                <div id="loadingSpinner" class="spinner ml-3 hidden"></div>
            </button>
        </div>

        <div id="statsSection" class="hidden main-content-grid">
            <div class="card full-width-card filter-section">
                <h2>Filtres de Données</h2>
                <div class="filter-group">
                    <label for="monthFilter">Sélectionnez un mois:</label>
                    <select id="monthFilter" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
                        <option value="">Toutes les données</option>
                    </select>
                </div>
                <div class="filter-group">
                    <label for="podcastSearchInput">Rechercher un podcast:</label>
                    <div class="podcast-search-container">
                        <input type="text" id="podcastSearchInput" placeholder="Tapez le nom du podcast..." class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
                        <div id="podcastSuggestions" class="podcast-suggestions hidden"></div>
                    </div>
                    <button id="clearPodcastSelection" class="search-clear-button">Effacer</button>
                    <input type="hidden" id="selectedPodcastHiddenInput"> <!-- To store the actual selected podcast title -->
                </div>
                <div class="filter-group mt-4">
                    <label for="podcastDisplayLimitInput">Nombre de podcasts à afficher :</label>
                    <input type="number" id="podcastDisplayLimitInput" value="10" min="1" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
                </div>
            </div>

            <div class="card">
                <h3 class="text-lg font-semibold mb-2 text-gray-700">Vue d'ensemble</h3>
                <p><strong>🎧 Temps total d'écoute :</strong> <span id="totalListeningTime">0 heures</span></p>
                <p><strong>📻 Épisodes écoutés :</strong> <span id="episodesListened">0</span></p>
                <p><strong>✅ Épisodes terminés (>90%) :</strong> <span id="episodesCompleted">0 (0%)</span></p>
                <p><strong>⏱️ Durée moyenne par écoute :</strong> <span id="avgListenDuration">0 minutes</span></p>
                <p><strong>📊 Progression moyenne :</strong> <span id="avgProgress">0%</span></p>
            </div>

            <div class="card">
                <h3 class="text-lg font-semibold mb-2 text-gray-700">Top Podcasts par temps d'écoute (global)</h3>
                <ul id="topPodcastsTimeListShort" class="list-disc ml-5"></ul>
                <div id="topPodcastsTimeListFull" class="hidden">
                    <ul id="topPodcastsTimeListAll" class="list-disc ml-5"></ul>
                </div>
                <button id="toggleTopPodcasts" class="button mt-4 text-sm px-4 py-2">Afficher plus</button>
            </div>

            <div class="card full-width-card">
                <h3 class="text-lg font-semibold mb-2 text-gray-700">Écoutes par mois</h3>
                <p id="monthlyChartPeriodInfo" class="chart-period-info"></p>
                <div class="chart-container">
                    <canvas id="monthlyChart"></canvas>
                </div>
            </div>

            <div id="monthlyPodcastDetail" class="card hidden">
                <h3 class="text-lg font-semibold mb-2 text-gray-700" id="monthlyPodcastDetailTitle"></h3>
                <ul id="monthlyPodcastListShort" class="ml-5"></ul>
                <div id="monthlyPodcastListFull" class="hidden">
                    <ul id="monthlyPodcastListAll" class="ml-5"></ul>
                </div>
                <button id="toggleMonthlyPodcasts" class="button mt-4 text-sm px-4 py-2">Afficher plus</button>
            </div>

            <div class="card full-width-card">
                <h3 class="text-lg font-semibold mb-2 text-gray-700">Distribution du temps d'écoute par Podcast</h3>
                <p id="podcastTimeDistributionChartPeriodInfo" class="chart-period-info"></p>
                <div class="chart-container">
                    <canvas id="podcastTimeDistributionChart"></canvas>
                </div>
            </div>

            <div class="card full-width-card">
                <h3 class="text-lg font-semibold mb-2 text-gray-700">Statistiques par Podcast</h3>
                <p id="podcastEpisodeCountChartPeriodInfo" class="chart-period-info"></p>
                
                <!-- Separated list and chart for Podcast Stats -->
                <h4 class="font-medium text-gray-700 mt-4 mb-2">Top Podcasts par épisodes écoutés (Liste)</h4>
                <ul id="podcastStatsListShort" class="ml-5"></ul>
                <div id="podcastStatsListFull" class="hidden">
                    <ul id="podcastStatsListAll" class="ml-5"></ul>
                </div>
                <button id="togglePodcastStats" class="button mt-4 text-sm px-4 py-2">Afficher plus</button>

                <h4 class="font-medium text-gray-700 mt-6 mb-2">Top Podcasts par épisodes écoutés (Graphique)</h4>
                <div id="selectedPodcastStats" class="mt-4 hidden">
                    <h4 class="font-medium text-gray-700 mb-2">Détails pour : <span id="selectedPodcastTitleDisplay" class="font-semibold"></span></h4>
                    <p><strong>🎧 Temps d'écoute sur la période :</strong> <span id="selectedPodcastTime">0 heures</span></p>
                    <p><strong>📻 Épisodes écoutés sur la période :</strong> <span id="selectedPodcastEpisodes">0</span></p>
                </div>
                <div id="podcastEpisodeCountChart-container" class="chart-container">
                    <canvas id="podcastEpisodeCountChart"></canvas>
                </div>
            </div>

            <div class="card full-width-card">
                <h3 class="text-lg font-semibold mb-2 text-gray-700">Habitudes d'écoute</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <p id="hourlyChartPeriodInfo" class="chart-period-info"></p>
                        <h4 class="font-medium text-gray-700 mb-2">Écoutes par heure de la journée</h4>
                        <div class="chart-container">
                            <canvas id="hourlyChart"></canvas>
                        </div>
                    </div>
                    <div>
                        <p id="dailyChartPeriodInfo" class="chart-period-info"></p>
                        <h4 class="font-medium text-gray-700 mb-2">Écoutes par jour de la semaine</h4>
                        <div class="chart-container">
                            <canvas id="dailyChart"></canvas>
                        </div>
                    </div>
                </div>
            </div>

            <div class="card full-width-card">
                <h3 class="text-lg font-semibold mb-2 text-gray-700">Export pour Réseaux Sociaux (Inspiré de Spotify Wrapped)</h3>
                <div id="wrappedContent">
                    <p id="wrappedPeriodInfo" class="text-sm text-gray-600 mb-2"></p>
                    <div id="wrappedTextContent" class="text-gray-800 bg-gray-50 p-4 rounded-lg text-left whitespace-pre-wrap"></div>
                </div>
                <div class="export-buttons">
                    <button id="copyWrapped" class="button">Copier le texte Wrapped</button>
                    <button id="exportWrappedImage" class="button">Exporter l'image Wrapped</button>
                </div>
            </div>

            <div class="card full-width-card">
                <h3 class="text-lg font-semibold mb-2 text-gray-700">Exporter tous les graphiques</h3>
                <p class="text-gray-600 mb-4">
                    Cliquez sur le bouton ci-dessous pour exporter tous les graphiques affichés sous forme d'images PNG dans un fichier ZIP.
                </p>
                <button id="exportAllCharts" class="button w-full">Exporter tous les graphiques (ZIP)</button>
            </div>
        </div>
    </div>

    <script type="module">
        // Importation des modules Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, onSnapshot, collection, query } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Définition des variables globales pour Firebase et l'application
        let db;
        let auth;
        let userId = 'anonymous'; // Default userId
        let app_id;
        let firebaseConfig;

        // Variables pour les graphiques Chart.js
        let monthlyChartInstance, podcastEpisodeCountChartInstance, hourlyChartInstance, dailyChartInstance, podcastTimeDistributionChartInstance;
        let globalDfHistory = null; // Stocke les données de l'historique global
        let allPodcastsTitles = []; // Global to store unique podcast titles for search suggestions
        let podcastDisplayLimit = 10; // Default display limit for top podcasts


        // --- Firebase Initialization and Authentication ---
        const initializeFirebase = async () => {
            try {
                // Initialisation de l'application Firebase
                app_id = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
                firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');

                const app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                // Authentification
                const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }

                // Récupération de l'ID utilisateur après authentification
                onAuthStateChanged(auth, (user) => {
                    if (user) {
                        userId = user.uid;
                        console.log('Utilisateur authentifié:', userId);
                        // Vous pouvez déclencher le chargement des données utilisateur ici si nécessaire
                    } else {
                        console.log('Utilisateur déconnecté ou anonyme.');
                        userId = crypto.randomUUID(); // Fallback for anonymous users
                    }
                });

                console.log("Firebase initialisé.");
            } catch (error) {
                console.error("Erreur lors de l'initialisation de Firebase:", error);
                showMessage('error', 'Erreur lors de la connexion aux services Firebase.');
            }
        };

        // Appel de l'initialisation Firebase au chargement de la page
        initializeFirebase();

        // --- DOM Elements ---
        const dbFileInput = document.getElementById('db-file-input');
        const fileNameSpan = document.getElementById('fileName');
        const analyzeButton = document.getElementById('analyzeButton');
        const analyzeButtonText = document.getElementById('analyzeButtonText');
        const loadingSpinner = document.getElementById('loadingSpinner');
        const statsSection = document.getElementById('statsSection');
        const messageBox = document.getElementById('messageBox');
        const totalListeningTimeSpan = document.getElementById('totalListeningTime');
        const episodesListenedSpan = document.getElementById('episodesListened');
        const episodesCompletedSpan = document.getElementById('episodesCompleted');
        const avgListenDurationSpan = document.getElementById('avgListenDuration');
        const avgProgressSpan = document.getElementById('avgProgress');
        
        // Top Podcasts Drawer Elements
        const topPodcastsTimeListShort = document.getElementById('topPodcastsTimeListShort');
        const topPodcastsTimeListFull = document.getElementById('topPodcastsTimeListFull');
        const topPodcastsTimeListAll = document.getElementById('topPodcastsTimeListAll');
        const toggleTopPodcastsButton = document.getElementById('toggleTopPodcasts');

        const monthFilter = document.getElementById('monthFilter');
        const monthlyPodcastDetail = document.getElementById('monthlyPodcastDetail');
        const monthlyPodcastDetailTitle = document.getElementById('monthlyPodcastDetailTitle');
        const monthlyPodcastListShort = document.getElementById('monthlyPodcastListShort'); // New for monthly drawer
        const monthlyPodcastListFull = document.getElementById('monthlyPodcastListFull'); // New for monthly drawer
        const monthlyPodcastListAll = document.getElementById('monthlyPodcastListAll'); // New for monthly drawer
        const toggleMonthlyPodcastsButton = document.getElementById('toggleMonthlyPodcasts'); // New for monthly drawer
        
        const podcastSearchInput = document.getElementById('podcastSearchInput');
        const podcastSuggestions = document.getElementById('podcastSuggestions');
        const selectedPodcastHiddenInput = document.getElementById('selectedPodcastHiddenInput');
        const clearPodcastSelectionButton = document.getElementById('clearPodcastSelection');
        const selectedPodcastStatsDiv = document.getElementById('selectedPodcastStats');
        const selectedPodcastTitleDisplay = document.getElementById('selectedPodcastTitleDisplay');
        const selectedPodcastTimeSpan = document.getElementById('selectedPodcastTime');
        const selectedPodcastEpisodesSpan = document.getElementById('selectedPodcastEpisodes');

        const podcastDisplayLimitInput = document.getElementById('podcastDisplayLimitInput'); // New input for limit

        // Removed wrappedExportTextarea from here as it's no longer in HTML
        const copyWrappedButton = document.getElementById('copyWrapped');
        const wrappedPeriodInfo = document.getElementById('wrappedPeriodInfo');
        const exportWrappedImageButton = document.getElementById('exportWrappedImage');
        const wrappedContentDiv = document.getElementById('wrappedContent');
        const wrappedTextContentDiv = document.getElementById('wrappedTextContent'); // New div for wrapped text

        const exportAllChartsButton = document.getElementById('exportAllCharts'); // New button for all charts export

        // Chart Period Info Elements
        const monthlyChartPeriodInfo = document.getElementById('monthlyChartPeriodInfo');
        const podcastTimeDistributionChartPeriodInfo = document.getElementById('podcastTimeDistributionChartPeriodInfo');
        const podcastEpisodeCountChartPeriodInfo = document.getElementById('podcastEpisodeCountChartPeriodInfo');
        const hourlyChartPeriodInfo = document.getElementById('hourlyChartPeriodInfo');
        const dailyChartPeriodInfo = document.getElementById('dailyChartPeriodInfo');

        // Chart containers for dynamic height adjustment
        const podcastEpisodeCountChartContainer = document.getElementById('podcastEpisodeCountChart-container');

        // New elements for Podcast Stats List drawer
        const podcastStatsListShort = document.getElementById('podcastStatsListShort');
        const podcastStatsListFull = document.getElementById('podcastStatsListFull');
        const podcastStatsListAll = document.getElementById('podcastStatsListAll');
        const togglePodcastStatsButton = document.getElementById('togglePodcastStats');


        // --- Chart Contexts ---
        const monthlyCtx = document.getElementById('monthlyChart').getContext('2d');
        const podcastEpisodeCountCtx = document.getElementById('podcastEpisodeCountChart').getContext('2d');
        const podcastTimeDistributionCtx = document.getElementById('podcastTimeDistributionChart').getContext('2d');
        const hourlyCtx = document.getElementById('hourlyChart').getContext('2d');
        const dailyCtx = document.getElementById('dailyChart').getContext('2d');

        // --- Utility Functions ---

        // Fonction pour afficher des messages
        function showMessage(type, message) {
            messageBox.className = `message-box ${type}`;
            messageBox.textContent = message;
            messageBox.style.display = 'block';
            setTimeout(() => {
                messageBox.style.display = 'none';
            }, 5000); // Hide after 5 seconds
        }

        // Conversion des millisecondes en heures, minutes, secondes
        function msToHMS(ms) {
            const totalSeconds = Math.floor(ms / 1000);
            const hours = Math.floor(totalSeconds / 3600);
            const minutes = Math.floor((totalSeconds % 3600) / 60);
            const seconds = totalSeconds % 60;
            return { hours, minutes, seconds };
        }

        // --- Event Listeners ---
        dbFileInput.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (file) {
                fileNameSpan.textContent = file.name;
                analyzeButton.disabled = false;
                showMessage('info', `Fichier sélectionné : ${file.name}`);
            } else {
                fileNameSpan.textContent = 'Aucun fichier sélectionné';
                analyzeButton.disabled = true;
            }
        });

        analyzeButton.addEventListener('click', async () => {
            const file = dbFileInput.files[0];
            if (!file) {
                showMessage('error', 'Veuillez sélectionner un fichier .db d\'abord.');
                return;
            }

            setLoadingState(true);
            showMessage('info', 'Analyse en cours, cela peut prendre quelques instants...');

            try {
                // Read the file as an array buffer
                const arrayBuffer = await file.arrayBuffer();
                const uint8Array = new Uint8Array(arrayBuffer);
                
                const processedData = await processAntennaPodDb(uint8Array);

                if (processedData) {
                    globalDfHistory = processedData.allHistoryData; // Store raw data for filters
                    updateUIWithStats(processedData);
                    populateFilters(processedData.allHistoryData);
                    updateCharts(); // Call updateCharts to draw with initial (all data) filters
                    statsSection.classList.remove('hidden');
                    showMessage('success', 'Analyse terminée avec succès !');
                } else {
                    showMessage('error', 'Échec de l\'analyse du fichier. Le format pourrait être invalide ou les données manquantes.');
                    statsSection.classList.add('hidden');
                }

            } catch (error) {
                console.error('Erreur lors de l\'analyse du fichier :', error);
                showMessage('error', `Une erreur est survenue lors de l'analyse : ${error.message}.`);
                statsSection.classList.add('hidden');
            } finally {
                setLoadingState(false);
            }
        });

        copyWrappedButton.addEventListener('click', async () => {
            const textToCopy = wrappedTextContentDiv.textContent;
            try {
                if (navigator.clipboard && navigator.clipboard.writeText) {
                    await navigator.clipboard.writeText(textToCopy);
                    showMessage('success', 'Rapport copié dans le presse-papiers !');
                } else {
                    // Fallback for older browsers or environments without navigator.clipboard
                    const tempTextArea = document.createElement('textarea');
                    tempTextArea.value = textToCopy;
                    document.body.appendChild(tempTextArea);
                    tempTextArea.select();
                    document.execCommand('copy');
                    document.body.removeChild(tempTextArea);
                    showMessage('success', 'Rapport copié dans le presse-papiers (via fallback) !');
                }
            } catch (err) {
                console.error('Failed to copy text: ', err);
                showMessage('error', 'Échec de la copie du rapport.');
            }
        });

        exportWrappedImageButton.addEventListener('click', async () => {
            try {
                showMessage('info', 'Génération de l\'image en cours...');
                
                // Ensure the content is visible and rendered before capturing
                wrappedContentDiv.style.display = 'block'; 
                
                const canvas = await html2canvas(wrappedContentDiv, { 
                    scale: 2, // Increase resolution for better quality
                    useCORS: true, // Needed if you have images from different origins (like placehold.co)
                    logging: false, // Disable logging if not debugging
                    backgroundColor: '#e2e8f0' // Ensure background is captured
                }); 
                
                // Convert canvas to image data URL
                const imageDataURL = canvas.toDataURL('image/png');
                
                // Create a temporary link element to trigger download
                const downloadLink = document.createElement('a');
                downloadLink.href = imageDataURL;
                downloadLink.download = 'AntennaPod_Wrapped_Report.png'; // File name
                document.body.appendChild(downloadLink); // Append to body is required for Firefox
                downloadLink.click(); // Programmatically click the link to trigger download
                document.body.removeChild(downloadLink); // Clean up
                
                showMessage('success', 'Image exportée avec succès !');
            } catch (error) {
                console.error('Erreur lors de l\'exportation de l\'image :', error);
                showMessage('error', `Erreur lors de l'exportation de l'image : ${error.message}.`);
            }
        });

        // New: Export all charts as PNGs in a ZIP file
        exportAllChartsButton.addEventListener('click', async () => {
            showMessage('info', 'Préparation de l\'exportation de tous les graphiques...');
            const zip = new JSZip();
            const chartsToExport = [
                { id: 'monthlyChart', name: 'Ecoutes_Mensuelles' },
                { id: 'podcastTimeDistributionChart', name: 'Distribution_Temps_Ecoute_Podcast' },
                { id: 'podcastEpisodeCountChart', name: 'Episodes_Ecoutes_Podcast' },
                { id: 'hourlyChart', name: 'Habitudes_Ecoute_Heure' },
                { id: 'dailyChart', name: 'Habitudes_Ecoute_Jour' }
            ];

            // Define padding for exported images
            const exportPadding = 20; // pixels

            for (const chartInfo of chartsToExport) {
                const canvasElement = document.getElementById(chartInfo.id);
                if (canvasElement) {
                    try {
                        // Get the data URL directly from the Chart.js rendered canvas
                        const chartDataURL = canvasElement.toDataURL('image/png');

                        // Create a new canvas to draw the chart with padding
                        const tempCanvas = document.createElement('canvas');
                        const ctx = tempCanvas.getContext('2d');

                        const img = new Image();
                        img.src = chartDataURL;

                        // Wait for the image to load before drawing
                        await new Promise(resolve => {
                            img.onload = () => {
                                tempCanvas.width = img.width + 2 * exportPadding;
                                tempCanvas.height = img.height + 2 * exportPadding;
                                ctx.fillStyle = '#ffffff'; // White background for padding area
                                ctx.fillRect(0, 0, tempCanvas.width, tempCanvas.height);
                                ctx.drawImage(img, exportPadding, exportPadding);
                                resolve();
                            };
                            img.onerror = (e) => {
                                console.error("Error loading image for padding:", e);
                                resolve(); // Resolve even on error to not block the loop
                            };
                        });

                        const paddedImageData = tempCanvas.toDataURL('image/png').split(',')[1];
                        zip.file(`${chartInfo.name}.png`, paddedImageData, { base64: true });

                    } catch (error) {
                        console.error(`Erreur lors de l'exportation du graphique ${chartInfo.name}:`, error);
                        showMessage('error', `Erreur lors de l'exportation du graphique ${chartInfo.name}.`);
                    }
                }
            }

            if (Object.keys(zip.files).length > 0) {
                zip.generateAsync({ type: "blob" })
                    .then(function (content) {
                        saveAs(content, "AntennaPod_Charts.zip");
                        showMessage('success', 'Tous les graphiques ont été exportés dans un fichier ZIP !');
                    })
                    .catch(error => {
                        console.error('Erreur lors de la création du fichier ZIP:', error);
                        showMessage('error', 'Erreur lors de la création du fichier ZIP.');
                    });
            } else {
                showMessage('info', 'Aucun graphique à exporter.');
            }
        });


        monthFilter.addEventListener('change', updateCharts);
        
        // Podcast search input event listeners
        podcastSearchInput.addEventListener('input', filterPodcastSuggestions);
        podcastSearchInput.addEventListener('focus', filterPodcastSuggestions); // Show suggestions on focus
        podcastSearchInput.addEventListener('blur', () => {
             // Hide suggestions after a short delay to allow click events on suggestions to fire
            setTimeout(() => {
                podcastSuggestions.classList.add('hidden');
            }, 100);
        });

        clearPodcastSelectionButton.addEventListener('click', () => {
            podcastSearchInput.value = '';
            selectedPodcastHiddenInput.value = '';
            selectedPodcastStatsDiv.classList.add('hidden'); // Hide specific podcast stats
            updateCharts(); // Re-render charts with no podcast filter
        });

        // Add a click listener to the document to hide suggestions when clicking outside
        document.addEventListener('click', (event) => {
            if (!podcastSuggestions.contains(event.target) && event.target !== podcastSearchInput) {
                podcastSuggestions.classList.add('hidden');
            }
        });

        // Toggle Top Podcasts Drawer (Global)
        toggleTopPodcastsButton.addEventListener('click', () => {
            const isExpanded = topPodcastsTimeListFull.classList.contains('expanded');
            if (isExpanded) {
                topPodcastsTimeListFull.classList.remove('expanded');
                topPodcastsTimeListFull.classList.add('hidden');
                toggleTopPodcastsButton.textContent = 'Afficher plus';
            } else {
                topPodcastsTimeListFull.classList.remove('hidden');
                topPodcastsTimeListFull.classList.add('expanded');
                toggleTopPodcastsButton.textContent = 'Afficher moins';
            }
        });

        // Toggle Monthly Podcasts Drawer
        toggleMonthlyPodcastsButton.addEventListener('click', () => {
            const isExpanded = monthlyPodcastListFull.classList.contains('expanded');
            if (isExpanded) {
                monthlyPodcastListFull.classList.remove('expanded');
                monthlyPodcastListFull.classList.add('hidden');
                toggleMonthlyPodcastsButton.textContent = 'Afficher plus';
            } else {
                monthlyPodcastListFull.classList.remove('hidden');
                monthlyPodcastListFull.classList.add('expanded');
                toggleMonthlyPodcastsButton.textContent = 'Afficher moins';
            }
        });

        // Toggle Podcast Stats Drawer (New)
        togglePodcastStatsButton.addEventListener('click', () => {
            const isExpanded = podcastStatsListFull.classList.contains('expanded');
            if (isExpanded) {
                podcastStatsListFull.classList.remove('expanded');
                podcastStatsListFull.classList.add('hidden');
                togglePodcastStatsButton.textContent = 'Afficher plus';
            } else {
                podcastStatsListFull.classList.remove('hidden');
                podcastStatsListFull.classList.add('expanded');
                togglePodcastStatsButton.textContent = 'Afficher moins';
            }
        });


        // Update podcastDisplayLimit when input changes
        podcastDisplayLimitInput.addEventListener('change', () => {
            const newValue = parseInt(podcastDisplayLimitInput.value, 10);
            if (!isNaN(newValue) && newValue > 0) {
                podcastDisplayLimit = newValue;
                updateCharts(); // Re-render charts with the new limit
            } else {
                showMessage('error', 'Veuillez entrer un nombre valide (>0) pour la limite d\'affichage.');
                podcastDisplayLimitInput.value = podcastDisplayLimit; // Revert to last valid value
            }
        });


        // --- UI Update Functions ---
        function setLoadingState(isLoading) {
            analyzeButton.disabled = isLoading;
            dbFileInput.disabled = isLoading;
            if (isLoading) {
                analyzeButtonText.textContent = 'Analyse en cours...';
                loadingSpinner.classList.remove('hidden');
            } else {
                analyzeButtonText.textContent = 'Analyser les statistiques';
                loadingSpinner.classList.add('hidden');
            }
        }

        function updateUIWithStats(stats) {
            totalListeningTimeSpan.textContent = `${stats.totalListeningTimeHours.toFixed(1)} heures (${stats.totalListeningTimeDays.toFixed(1)} jours)`;
            episodesListenedSpan.textContent = stats.episodesListened;
            episodesCompletedSpan.textContent = `${stats.episodesCompleted} (${stats.completionRate.toFixed(1)}%)`;
            avgListenDurationSpan.textContent = `${stats.avgListenDurationMinutes.toFixed(1)} minutes`;
            avgProgressSpan.textContent = `${stats.avgProgressPercent.toFixed(1)}%`;

            // Top Podcasts (global) - Short list with 'Autres'
            topPodcastsTimeListShort.innerHTML = '';
            const globalTopPodcasts = stats.topPodcastsByTime;
            const globalDisplayLimit = podcastDisplayLimit;

            globalTopPodcasts.slice(0, globalDisplayLimit).forEach((p, index) => {
                const li = document.createElement('li');
                li.textContent = `${index + 1}. ${p.podcast}: ${p.hours.toFixed(1)}h`;
                topPodcastsTimeListShort.appendChild(li);
            });

            // Top Podcasts (global) - Full list (only items beyond the limit)
            topPodcastsTimeListAll.innerHTML = '';
            let globalOtherHours = 0;
            if (globalTopPodcasts.length > globalDisplayLimit) {
                globalTopPodcasts.slice(globalDisplayLimit).forEach((p, index) => {
                    const li = document.createElement('li');
                    li.textContent = `${globalDisplayLimit + 1 + index}. ${p.podcast}: ${p.hours.toFixed(1)}h`;
                    topPodcastsTimeListAll.appendChild(li);
                });
                // Calculate other hours for the short list
                globalOtherHours = globalTopPodcasts.slice(globalDisplayLimit).reduce((sum, p) => sum + p.hours, 0);

                // Add 'Autres' to the short list if there are more
                const liAutres = document.createElement('li');
                liAutres.className = 'podcast-item';
                const imgAutres = document.createElement('img');
                imgAutres.src = `https://placehold.co/50x50/cccccc/000000?text=Autre`;
                imgAutres.alt = `Autres podcasts`;
                liAutres.appendChild(imgAutres);
                const textSpanAutres = document.createElement('span');
                textSpanAutres.textContent = `... Autres : ${globalOtherHours.toFixed(1)}h`;
                liAutres.appendChild(textSpanAutres);
                topPodcastsTimeListShort.appendChild(liAutres);
            }

            // Show/hide toggle button for global list
            if (globalTopPodcasts.length <= globalDisplayLimit) {
                toggleTopPodcastsButton.classList.add('hidden');
                topPodcastsTimeListFull.classList.add('hidden');
            } else {
                toggleTopPodcastsButton.classList.remove('hidden');
                topPodcastsTimeListFull.classList.remove('expanded'); // Ensure collapsed by default on new data load
                topPodcastsTimeListFull.classList.add('hidden');
                toggleTopPodcastsButton.textContent = 'Afficher plus';
            }
        }

        // --- Filtering and Charting Functions ---
        function populateFilters(data) {
            // Populate Month Filter
            const months = new Set();
            data.forEach(item => {
                if (item.last_played_time > 0) {
                    const date = new Date(item.last_played_time);
                    months.add(date.getFullYear() + '-' + String(date.getMonth() + 1).padStart(2, '0'));
                }
            });
            const sortedMonths = Array.from(months).sort().reverse(); // Sort descending
            monthFilter.innerHTML = '<option value="">Toutes les données</option>';
            sortedMonths.forEach(month => {
                const option = document.createElement('option');
                option.value = month;
                option.textContent = month;
                monthFilter.appendChild(option);
            });

            // Populate allPodcastsTitles for search suggestions
            const podcasts = new Set();
            data.forEach(item => {
                if (item.podcast_title) {
                    podcasts.add(item.podcast_title);
                }
            });
            allPodcastsTitles = Array.from(podcasts).sort();
        }

        function filterPodcastSuggestions() {
            const searchTerm = podcastSearchInput.value.toLowerCase();
            podcastSuggestions.innerHTML = '';
            
            if (searchTerm.length === 0) {
                podcastSuggestions.classList.add('hidden');
                return;
            }

            const filtered = allPodcastsTitles.filter(podcast =>
                podcast.toLowerCase().includes(searchTerm)
            );

            if (filtered.length > 0) {
                podcastSuggestions.classList.remove('hidden');
                filtered.forEach(podcast => {
                    const div = document.createElement('div');
                    div.textContent = podcast;
                    div.className = 'podcast-suggestion-item';
                    div.addEventListener('click', () => {
                        podcastSearchInput.value = podcast;
                        selectedPodcastHiddenInput.value = podcast; // Store the exact selected value
                        podcastSuggestions.classList.add('hidden');
                        updateCharts(); // Trigger chart update with exact podcast match
                    });
                    podcastSuggestions.appendChild(div);
                });
            } else {
                podcastSuggestions.classList.add('hidden'); // Hide if no suggestions
            }
        }

        function updateCharts() {
            if (!globalDfHistory) return;

            let filteredData = globalDfHistory;
            const selectedMonth = monthFilter.value;
            const selectedPodcast = selectedPodcastHiddenInput.value; // Use the value from the hidden input

            // Apply month filter
            if (selectedMonth) {
                filteredData = filteredData.filter(item => {
                    if (item.last_played_time > 0) {
                        const date = new Date(item.last_played_time);
                        const itemMonth = date.getFullYear() + '-' + String(date.getMonth() + 1).padStart(2, '0');
                        return itemMonth === selectedMonth;
                    }
                    return false;
                });
            }

            // Apply podcast filter
            if (selectedPodcast) {
                filteredData = filteredData.filter(item => item.podcast_title === selectedPodcast);
                // Update selected podcast stats display
                const podcastStats = processRawData(filteredData);
                selectedPodcastTitleDisplay.textContent = selectedPodcast;
                selectedPodcastTimeSpan.textContent = `${podcastStats.totalListeningTimeHours.toFixed(1)} heures`;
                selectedPodcastEpisodesSpan.textContent = podcastStats.episodesListened;
                selectedPodcastStatsDiv.classList.remove('hidden');
            } else {
                selectedPodcastStatsDiv.classList.add('hidden');
            }

            const processedFilteredData = processRawData(filteredData);

            // Update monthly podcast list (uses topPodcastsByTime from the processedFilteredData)
            updateMonthlyPodcastList(selectedMonth, processedFilteredData.topPodcastsByTime);

            // Update Podcast Stats List (by episode count)
            updatePodcastStatsList(processedFilteredData.topPodcastsByEpisodeCount);

            // Redraw charts
            drawCharts(processedFilteredData, selectedMonth, selectedPodcast);

            // Generate Wrapped Report for the filtered period
            generateWrappedReport(processedFilteredData, selectedMonth);
        }

        function updateMonthlyPodcastList(selectedMonth, topPodcastsForPeriod) {
            monthlyPodcastListShort.innerHTML = '';
            monthlyPodcastListAll.innerHTML = '';
            
            if (selectedMonth) {
                monthlyPodcastDetailTitle.textContent = `Top Podcasts pour ${selectedMonth}`;
                monthlyPodcastDetail.classList.remove('hidden'); // Ensure the monthly detail card is visible

                if (topPodcastsForPeriod.length === 0) {
                    const li = document.createElement('li');
                    li.textContent = "Aucun podcast écouté ce mois.";
                    monthlyPodcastListShort.appendChild(li);
                    toggleMonthlyPodcastsButton.classList.add('hidden');
                    monthlyPodcastListFull.classList.add('hidden');
                    return;
                }

                // Populate short list
                topPodcastsForPeriod.slice(0, podcastDisplayLimit).forEach((p, index) => {
                    const li = document.createElement('li');
                    li.className = 'podcast-item';
                    const img = document.createElement('img');
                    img.src = `https://placehold.co/50x50/ADD8E6/000000?text=${p.podcast.substring(0,2).toUpperCase()}`;
                    img.alt = `Couverture de ${p.podcast}`;
                    li.appendChild(img);
                    
                    const textSpan = document.createElement('span');
                    textSpan.textContent = `${index + 1}. ${p.podcast}: ${p.hours.toFixed(1)}h`;
                    li.appendChild(textSpan);
                    monthlyPodcastListShort.appendChild(li);
                });

                // Populate full list (only items beyond the limit)
                let otherHours = 0;
                if (topPodcastsForPeriod.length > podcastDisplayLimit) {
                    topPodcastsForPeriod.slice(podcastDisplayLimit).forEach((p, index) => {
                        const li = document.createElement('li');
                        li.className = 'podcast-item';
                        const img = document.createElement('img');
                        img.src = `https://placehold.co/50x50/ADD8E6/000000?text=${p.podcast.substring(0,2).toUpperCase()}`;
                        img.alt = `Couverture de ${p.podcast}`;
                        li.appendChild(img);
                        
                        const textSpan = document.createElement('span');
                        textSpan.textContent = `${podcastDisplayLimit + 1 + index}. ${p.podcast}: ${p.hours.toFixed(1)}h`;
                        li.appendChild(textSpan);
                        monthlyPodcastListAll.appendChild(li);
                    });
                    // Calculate other hours for the short list
                    otherHours = topPodcastsForPeriod.slice(podcastDisplayLimit).reduce((sum, p) => sum + p.hours, 0);

                    // Add "Autres" to the short list if applicable
                    const liAutres = document.createElement('li');
                    liAutres.className = 'podcast-item';
                    const imgAutres = document.createElement('img');
                    imgAutres.src = `https://placehold.co/50x50/cccccc/000000?text=Autre`;
                    imgAutres.alt = `Autres podcasts`;
                    liAutres.appendChild(imgAutres);
                    const textSpanAutres = document.createElement('span');
                    textSpanAutres.textContent = `... Autres : ${otherHours.toFixed(1)}h`;
                    liAutres.appendChild(textSpanAutres);
                    monthlyPodcastListShort.appendChild(liAutres);
                }


                // Show/hide toggle button for monthly list
                if (topPodcastsForPeriod.length <= podcastDisplayLimit) {
                    toggleMonthlyPodcastsButton.classList.add('hidden');
                    monthlyPodcastListFull.classList.add('hidden');
                } else {
                    toggleMonthlyPodcastsButton.classList.remove('hidden');
                    monthlyPodcastListFull.classList.remove('expanded'); // Ensure collapsed by default
                    monthlyPodcastListFull.classList.add('hidden');
                    toggleMonthlyPodcastsButton.textContent = 'Afficher plus';
                }

            } else {
                monthlyPodcastDetail.classList.add('hidden');
            }
        }

        function updatePodcastStatsList(topPodcastsByEpisodeCount) {
            podcastStatsListShort.innerHTML = '';
            podcastStatsListAll.innerHTML = '';

            const statsDisplayLimit = podcastDisplayLimit;

            topPodcastsByEpisodeCount.slice(0, statsDisplayLimit).forEach((p, index) => {
                const li = document.createElement('li');
                li.textContent = `${index + 1}. ${p.podcast}: ${p.count} épisodes`;
                podcastStatsListShort.appendChild(li);
            });

            let otherEpisodesCount = 0;
            if (topPodcastsByEpisodeCount.length > statsDisplayLimit) {
                topPodcastsByEpisodeCount.slice(statsDisplayLimit).forEach((p, index) => {
                    const li = document.createElement('li');
                    li.textContent = `${statsDisplayLimit + 1 + index}. ${p.podcast}: ${p.count} épisodes`;
                    podcastStatsListAll.appendChild(li);
                });
                otherEpisodesCount = topPodcastsByEpisodeCount.slice(statsDisplayLimit).reduce((sum, p) => sum + p.count, 0);

                const liAutres = document.createElement('li');
                liAutres.className = 'podcast-item';
                const imgAutres = document.createElement('img');
                imgAutres.src = `https://placehold.co/50x50/cccccc/000000?text=Autre`;
                imgAutres.alt = `Autres podcasts`;
                liAutres.appendChild(imgAutres);
                const textSpanAutres = document.createElement('span');
                textSpanAutres.textContent = `... Autres : ${otherEpisodesCount} épisodes`;
                liAutres.appendChild(textSpanAutres);
                podcastStatsListShort.appendChild(liAutres);
            }

            if (topPodcastsByEpisodeCount.length <= statsDisplayLimit) {
                togglePodcastStatsButton.classList.add('hidden');
                podcastStatsListFull.classList.add('hidden');
            } else {
                togglePodcastStatsButton.classList.remove('hidden');
                podcastStatsListFull.classList.remove('expanded');
                podcastStatsListFull.classList.add('hidden');
                togglePodcastStatsButton.textContent = 'Afficher plus';
            }
        }


        function drawCharts(stats, selectedMonth, selectedPodcast) {
            // Destroy existing chart instances to avoid memory leaks and conflicts
            if (monthlyChartInstance) monthlyChartInstance.destroy();
            if (podcastEpisodeCountChartInstance) podcastEpisodeCountChartInstance.destroy();
            if (hourlyChartInstance) hourlyChartInstance.destroy();
            if (dailyChartInstance) dailyChartInstance.destroy();
            if (podcastTimeDistributionChartInstance) podcastTimeDistributionChartInstance.destroy();

            // Determine period info text
            const periodInfoText = selectedPodcast 
                ? `Données pour "${selectedPodcast}" ${selectedMonth ? `en ${selectedMonth}` : 'pour toutes les périodes'}`
                : (selectedMonth ? `Données pour le mois de ${selectedMonth}` : 'Données pour toutes les périodes');

            // Update chart period info
            monthlyChartPeriodInfo.textContent = periodInfoText;
            podcastTimeDistributionChartPeriodInfo.textContent = periodInfoText;
            podcastEpisodeCountChartPeriodInfo.textContent = periodInfoText;
            hourlyChartPeriodInfo.textContent = periodInfoText;
            dailyChartPeriodInfo.textContent = periodInfoText;


            // Monthly Chart (Time instead of Count)
            let monthlyChartLabels;
            let monthlyChartValues;
            let monthlyChartTitle;

            if (selectedMonth) {
                // If a month is selected, show daily listening time for that month
                const dailyListeningTimeForMonth = {};
                stats.allHistoryData.forEach(item => {
                    if (item.last_played_time > 0) {
                        const date = new Date(item.last_played_time);
                        const dateKey = `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}-${String(date.getDate()).padStart(2, '0')}`;
                        dailyListeningTimeForMonth[dateKey] = (dailyListeningTimeForMonth[dateKey] || 0) + (item.played_duration || 0);
                    }
                });

                // Sort by date and fill missing days with 0
                const [year, month] = selectedMonth.split('-');
                const daysInMonth = new Date(year, month, 0).getDate();
                const sortedDailyData = [];
                for (let i = 1; i <= daysInMonth; i++) {
                    const dateKey = `${year}-${month}-${String(i).padStart(2, '0')}`;
                    sortedDailyData.push([dateKey, dailyListeningTimeForMonth[dateKey] || 0]);
                }

                monthlyChartLabels = sortedDailyData.map(([date]) => date.substring(8)); // Just day for label
                monthlyChartValues = sortedDailyData.map(([, ms]) => ms / (1000 * 3600)); // Convert ms to hours
                monthlyChartTitle = `Temps d'écoute quotidien pour ${selectedMonth} (heures)`;

            } else {
                // Otherwise, show monthly total listening time for all time
                const monthlyTotalTimeMap = {};
                stats.allHistoryData.forEach(item => {
                    if (item.last_played_time > 0) {
                        const date = new Date(item.last_played_time);
                        const monthKey = `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}`;
                        monthlyTotalTimeMap[monthKey] = (monthlyTotalTimeMap[monthKey] || 0) + (item.played_duration || 0);
                    }
                });
                const sortedMonthlyTime = Object.entries(monthlyTotalTimeMap)
                    .sort(([monthA], [monthB]) => monthA.localeCompare(monthB));

                monthlyChartLabels = sortedMonthlyTime.map(([month]) => month);
                monthlyChartValues = sortedMonthlyTime.map(([, ms]) => ms / (1000 * 3600)); // Convert ms to hours
                monthlyChartTitle = 'Temps d\'écoute mensuel (heures)';
            }

            monthlyChartInstance = new Chart(monthlyCtx, {
                type: 'bar', // Always bar chart for this now
                data: {
                    labels: monthlyChartLabels,
                    datasets: [{
                        label: 'Heures écoutées',
                        data: monthlyChartValues,
                        backgroundColor: '#0e6270', /* New color */
                        borderColor: '#0e6270', /* New color */
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: { beginAtZero: true, title: { display: true, text: 'Heures' } }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: monthlyChartTitle
                        }
                    }
                }
            });

            // Podcast Episode Count Chart (Horizontal Bar Chart)
            const podcastCounts = {};
            stats.allHistoryData.forEach(item => { // Use allHistoryData from the processed (filtered) data
                if (item.podcast_title && item.played_duration > 0) {
                    podcastCounts[item.podcast_title] = (podcastCounts[item.podcast_title] || 0) + 1;
                }
            });
            const sortedPodcastCounts = Object.entries(podcastCounts).sort(([, a], [, b]) => b - a);
            // Apply podcastDisplayLimit here for the chart
            const limitedPodcastCounts = sortedPodcastCounts.slice(0, podcastDisplayLimit);

            const podcastEpisodeLabels = limitedPodcastCounts.map(([title]) => title);
            const podcastEpisodeValues = limitedPodcastCounts.map(([, count]) => count);

            // Dynamically adjust height for horizontal bar chart
            const barHeight = 40; // Approximate height per bar including padding
            const minChartHeight = 300; // Minimum height for the chart area
            const calculatedHeight = Math.max(minChartHeight, podcastEpisodeLabels.length * barHeight + 100); // +100 for title/padding
            podcastEpisodeCountChartContainer.style.height = `${calculatedHeight}px`;

            podcastEpisodeCountChartInstance = new Chart(podcastEpisodeCountCtx, {
                type: 'bar',
                data: {
                    labels: podcastEpisodeLabels,
                    datasets: [{
                        label: 'Épisodes écoutés',
                        data: podcastEpisodeValues,
                        backgroundColor: '#0e6270', /* New color */
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y', // Make it horizontal
                    scales: {
                        x: { beginAtZero: true, title: { display: true, text: 'Nombre d\'épisodes' } },
                        y: { // Ensure labels are fully visible
                            ticks: {
                                autoSkip: false, // Prevent Chart.js from skipping labels
                                // Callback to ensure full labels are shown, Chart.js will adjust width
                                callback: function(value, index, values) {
                                    return this.getLabelForValue(value); 
                                }
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false // No legend needed for single dataset horizontal bar chart
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed.x !== null) {
                                        label += context.parsed.x;
                                    }
                                    return label;
                                },
                                title: function(context) {
                                    // Show full podcast title in tooltip
                                    return context[0].label;
                                }
                            }
                        }
                    }
                }
            });

            // Podcast Time Distribution Chart (Doughnut Chart)
            const podcastTimeDistributionData = {};
            stats.allHistoryData.forEach(item => {
                if (item.podcast_title && item.played_duration > 0) {
                    podcastTimeDistributionData[item.podcast_title] = (podcastTimeDistributionData[item.podcast_title] || 0) + item.played_duration;
                }
            });

            const sortedPodcastTimeDistribution = Object.entries(podcastTimeDistributionData)
                .sort(([, a], [, b]) => b - a); // Sort by duration in milliseconds

            const timeDistributionLabels = [];
            const timeDistributionValues = [];
            let otherHours = 0;
            // Use podcastDisplayLimit for the doughnut chart as well
            const displayLimitForDoughnut = podcastDisplayLimit; 

            for (let i = 0; i < sortedPodcastTimeDistribution.length; i++) {
                const [podcast, durationMs] = sortedPodcastTimeDistribution[i];
                const hours = durationMs / (1000 * 3600);
                if (i < displayLimitForDoughnut) {
                    timeDistributionLabels.push(podcast);
                    timeDistributionValues.push(hours);
                } else {
                    otherHours += hours;
                }
            }

            if (otherHours > 0) {
                timeDistributionLabels.push('Autres');
                timeDistributionValues.push(otherHours);
            }

            const backgroundColors = [
                '#0e6270', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF', '#FF9933', '#C9CBCE', '#A3CB38', '#5D4037', '#F44336', '#8e44ad', '#27ae60'
            ]; // More colors for variety if many podcasts
            
            podcastTimeDistributionChartInstance = new Chart(podcastTimeDistributionCtx, {
                type: 'doughnut',
                data: {
                    labels: timeDistributionLabels,
                    datasets: [{
                        data: timeDistributionValues,
                        backgroundColor: backgroundColors.slice(0, timeDistributionLabels.length),
                        hoverOffset: 4
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'right',
                            labels: {
                                boxWidth: 20, // Smaller color box to give more space for text
                                font: {
                                    size: 12 // Slightly smaller font for legend if needed
                                },
                                generateLabels: function(chart) {
                                    const data = chart.data;
                                    if (data.labels.length && data.datasets.length) {
                                        return data.labels.map((label, i) => {
                                            const meta = chart.getDatasetMeta(0);
                                            // Safely access options and provide fallback
                                            const style = (meta.data[i] && meta.data[i].options) ? meta.data[i].options : {};
                                            const fillStyle = style.backgroundColor || '#cccccc'; // Default grey
                                            const strokeStyle = style.borderColor || '#cccccc'; // Default grey

                                            const value = data.datasets[0].data[i];
                                            return {
                                                text: `${label}: ${value.toFixed(1)}h`, // Include value in legend
                                                fillStyle: fillStyle,
                                                strokeStyle: strokeStyle,
                                                lineWidth: style.borderWidth || 1, // Default border width
                                                hidden: isNaN(data.datasets[0].data[i]) || (meta.data[i] ? meta.data[i].hidden : false),
                                                index: i
                                            };
                                        });
                                    }
                                    return [];
                                }
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    const label = context.label || '';
                                    const value = context.parsed || 0;
                                    return `${label}: ${value.toFixed(1)} heures`;
                                }
                            }
                        },
                        title: {
                            display: true,
                            text: 'Distribution du Temps d\'Écoute par Podcast (en heures)'
                        }
                    }
                }
            });


            // Hourly Chart
            const hourlyLabels = stats.hourlyListening.map(item => `${item.hour}h`);
            const hourlyValues = stats.hourlyListening.map(item => item.count);
            hourlyChartInstance = new Chart(hourlyCtx, {
                type: 'bar',
                data: {
                    labels: hourlyLabels,
                    datasets: [{
                        label: 'Nombre d\'écoutes',
                        data: hourlyValues,
                        backgroundColor: '#f6ad55',
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: { beginAtZero: true, title: { display: true, text: 'Nombre d\'écoutes' } }
                    }
                }
            });

            // Daily Chart (Starts Monday)
            const dayNames = ['Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi', 'Dimanche']; // Order starts Monday
            const tempDailyDataMap = new Map(); // Temporary map for easy lookup
            stats.dailyListening.forEach(item => {
                tempDailyDataMap.set(item.day, item.count);
            });

            const dailyValues = dayNames.map(day => tempDailyDataMap.get(day) || 0); // Get counts in correct order

            dailyChartInstance = new Chart(dailyCtx, {
                type: 'bar',
                data: {
                    labels: dayNames, // Use the ordered day names
                    datasets: [{
                        label: 'Nombre d\'écoutes',
                        data: dailyValues,
                        backgroundColor: '#fc8181',
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: { beginAtZero: true, title: { display: true, text: 'Nombre d\'écoutes' } }
                    }
                }
            });
        }

        // --- Spotify Wrapped inspired report generation ---
        function generateWrappedReport(stats, selectedMonth) {
            const totalHours = stats.totalListeningTimeHours.toFixed(1);
            const periodText = selectedMonth ? `pour le mois de ${selectedMonth}` : `cette année`;
            const top5Podcasts = stats.topPodcastsByTime.slice(0, 5); // Get top 5 from filtered data

            let message = `🎉 Mon Bilan AntennaPod ${periodText} !\n\n`;
            message += `J'ai écouté des podcasts pendant ${totalHours} heures ${periodText.replace('pour ', '')} ! C'est l'équivalent de ${stats.totalListeningTimeDays.toFixed(1)} jours complets 🎧.\n\n`;
            
            if (top5Podcasts.length > 0) {
                message += `Mes top 5 podcasts ${periodText.replace('pour ', '')} :\n`;
                top5Podcasts.forEach((p, index) => {
                    // Using a simple emoji for podcast image representation in text export
                    message += `${index + 1}. 🎙️ "${p.podcast}" : ${p.hours.toFixed(1)} heures\n`;
                });
            } else {
                message += `Aucun podcast écouté ${periodText.replace('pour ', '')}.\n`;
            }

            message += `\nJ'ai découvert et écouté ${stats.episodesListened} épisodes différents 📚, avec une durée moyenne par écoute de ${stats.avgListenDurationMinutes.toFixed(1)} minutes.\n\n`;
            message += `Quelles sont vos stats ? Partagez les vôtre ! #AntennaPodStats #PodcastAddict`;

            // Update the div for html2canvas
            wrappedTextContentDiv.textContent = message; 
        }

        // --- Simulate Python Script Logic with sql.js ---
        // This part requires sql.js library to be loaded.
        // It's a client-side database engine.
        let SQL;
        async function loadSqlJs() {
            try {
                const SQL_wasm = await initSqlJs({
                    locateFile: file => `https://cdnjs.cloudflare.com/ajax/libs/sql.js/1.6.2/${file}`
                });
                SQL = SQL_wasm;
                console.log("sql.js chargé avec succès.");
            } catch (err) { 
                console.error("Erreur lors du chargement de sql.js:", err);
                showMessage('error', 'Impossible de charger la bibliothèque de base de données. Veuillez patienter et réessayer.');
            }
        }

        // Load sql.js as soon as possible
        loadSqlJs();

        async function processAntennaPodDb(uint8Array) {
            if (!SQL) {
                showMessage('error', 'La bibliothèque sql.js n\'est pas encore chargée. Veuillez patienter et réessayer.');
                return null;
            }

            try {
                const db = new SQL.Database(uint8Array);

                // Replicate the SQL query from the Python script
                // Removed f.image as it's not consistently available or web-accessible
                const query = `
                    SELECT
                        fm.id as media_id,
                        fm.duration,
                        fm.position,
                        fm.played_duration,
                        fm.last_played_time,
                        fm.playback_completion_date,
                        fi.title as episode_title,
                        fi.pubDate,
                        fi.feed as feed_id,
                        f.title as podcast_title,
                        f.author
                    FROM FeedMedia fm
                    JOIN FeedItems fi ON fm.feeditem = fi.id
                    JOIN Feeds f ON fi.feed = f.id
                    WHERE fm.played_duration > 0 OR fm.last_played_time > 0
                    ORDER BY fm.last_played_time DESC
                `;

                const res = db.exec(query);
                db.close();

                if (res.length === 0) {
                    console.warn("Aucun résultat trouvé dans la base de données AntennaPod.");
                    showMessage('info', 'Aucune donnée d\'écoute trouvée dans le fichier fourni.');
                    return null;
                }

                const columns = res[0].columns;
                const values = res[0].values;

                // Transform into array of objects (similar to DataFrame rows)
                const dfHistory = values.map(row => {
                    const rowObject = {};
                    columns.forEach((col, index) => {
                        rowObject[col] = row[index];
                    });
                    return rowObject;
                });

                return processRawData(dfHistory);

            } catch (error) {
                console.error('Erreur lors du traitement de la base de données SQLite avec sql.js:', error);
                showMessage('error', `Erreur lors de la lecture du fichier .db : ${error.message}. Assurez-vous que c'est un fichier AntennaPod.db valide.`);
                return null;
            }
        }

        // Helper function to process raw data (from sql.js or backend) into desired stats structure
        function processRawData(data) {
            const listenedEpisodes = data.filter(item => item.last_played_time > 0 && item.played_duration > 0);

            if (listenedEpisodes.length === 0) {
                 return {
                    totalListeningTimeHours: 0,
                    totalListeningTimeDays: 0,
                    episodesListened: 0,
                    episodesCompleted: 0,
                    completionRate: 0,
                    avgListenDurationMinutes: 0,
                    avgProgressPercent: 0,
                    topPodcastsByTime: [],
                    topPodcastsByEpisodeCount: [], // Added for new drawer
                    monthlyListening: [],
                    hourlyListening: [],
                    dailyListening: [],
                    allHistoryData: [] // Return the filtered data as allHistoryData
                };
            }

            // Calculate overall stats
            const totalMs = listenedEpisodes.reduce((sum, item) => sum + (item.played_duration || 0), 0);
            const totalHours = totalMs / (1000 * 3600);
            const totalDays = totalHours / 24;

            const episodesListenedCount = listenedEpisodes.length;

            const completedEpisodes = listenedEpisodes.filter(
                item => item.played_duration > 0 && item.duration > 0 && item.played_duration >= item.duration * 0.9
            );
            const completionRate = episodesListenedCount > 0 ? (completedEpisodes.length / episodesListenedCount) * 100 : 0;

            const activeEpisodesWithDuration = listenedEpisodes.filter(item => item.played_duration > 0);
            const avgDurationMs = activeEpisodesWithDuration.length > 0 ? activeEpisodesWithDuration.reduce((sum, item) => sum + item.played_duration, 0) / activeEpisodesWithDuration.length : 0;
            const avgDurationMinutes = avgDurationMs / (1000 * 60);

            const progressData = listenedEpisodes.filter(item => item.duration > 0 && item.position >= 0);
            // Corrected syntax for reduce callback and initial value
            const avgProgress = progressData.length > 0 ? progressData.reduce((sum, item) => sum + ((item.position / item.duration) * 100), 0) / progressData.length : 0;

            // Top Podcasts by Time (for the current filtered data)
            const timeByPodcast = {};
            listenedEpisodes.forEach(item => {
                if (item.podcast_title) {
                    timeByPodcast[item.podcast_title] = (timeByPodcast[item.podcast_title] || 0) + (item.played_duration || 0);
                }
            });
            const topPodcastsByTime = Object.entries(timeByPodcast)
                .sort(([, a], [, b]) => b - a)
                .map(([podcast, ms]) => ({ podcast, hours: ms / (1000 * 3600) })); // No slice here, so all podcasts are included

            // Top Podcasts by Episode Count (for the current filtered data) - NEW
            const countByPodcast = {};
            listenedEpisodes.forEach(item => {
                if (item.podcast_title) {
                    countByPodcast[item.podcast_title] = (countByPodcast[item.podcast_title] || 0) + 1;
                }
            });
            const topPodcastsByEpisodeCount = Object.entries(countByPodcast)
                .sort(([, a], [, b]) => b - a)
                .map(([podcast, count]) => ({ podcast, count }));


            // Monthly Listening (now calculates time, not count)
            const monthlyListeningMap = {};
            listenedEpisodes.forEach(item => {
                if (item.last_played_time > 0) {
                    const date = new Date(item.last_played_time);
                    const monthKey = `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}`;
                    monthlyListeningMap[monthKey] = (monthlyListeningMap[monthKey] || 0) + (item.played_duration || 0);
                }
            });
            const monthlyListening = Object.entries(monthlyListeningMap)
                .sort(([monthA], [monthB]) => monthA.localeCompare(monthB))
                .map(([month, ms]) => ({ month, count: ms / (1000 * 3600) })); // Convert ms to hours for 'count'


            // Hourly Listening
            const hourlyListeningMap = {};
            for (let i = 0; i < 24; i++) hourlyListeningMap[i] = 0; // Initialize all hours
            listenedEpisodes.forEach(item => {
                if (item.last_played_time > 0) {
                    const date = new Date(item.last_played_time);
                    const hour = date.getHours();
                    hourlyListeningMap[hour] = (hourlyListeningMap[hour] || 0) + 1;
                }
            });
            const hourlyListening = Object.entries(hourlyListeningMap).map(([hour, count]) => ({ hour: parseInt(hour), count })).sort((a, b) => a.hour - b.hour);

            // Daily Listening (Monday as first day)
            // Note: getDay() returns 0 for Sunday, 1 for Monday... 6 for Saturday
            const dayNamesOrdered = ['Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi', 'Dimanche'];
            const dailyListeningMap = new Map(); // Changed to Map
            // Initialize map with 0 for all days in the desired order
            dayNamesOrdered.forEach(day => dailyListeningMap.set(day, 0)); // Use set()

            listenedEpisodes.forEach(item => {
                if (item.last_played_time > 0) {
                    const date = new Date(item.last_played_time);
                    let dayOfWeekIndex = date.getDay(); // 0 (Sunday) to 6 (Saturday)
                    // Adjust index to start from Monday (1 becomes 0, 0 becomes 6)
                    dayOfWeekIndex = (dayOfWeekIndex === 0) ? 6 : dayOfWeekIndex - 1;
                    const dayOfWeekName = dayNamesOrdered[dayOfWeekIndex]; // Use dayNamesOrdered array
                    dailyListeningMap.set(dayOfWeekName, (dailyListeningMap.get(dayOfWeekName) || 0) + 1); // Use set() and get()
                }
            });
            const dailyListening = Array.from(dailyListeningMap.entries()).map(([day, count]) => ({ day, count }));


            return {
                totalListeningTimeHours: totalHours,
                totalListeningTimeDays: totalDays,
                episodesListened: episodesListenedCount,
                episodesCompleted: completedEpisodes.length,
                completionRate: completionRate,
                avgListenDurationMinutes: avgDurationMinutes,
                avgProgressPercent: avgProgress,
                topPodcastsByTime: topPodcastsByTime, // This now contains all sorted podcasts for the current filter
                topPodcastsByEpisodeCount: topPodcastsByEpisodeCount, // Added for new drawer
                monthlyListening: monthlyListening, // Now contains hours
                hourlyListening: hourlyListening,
                dailyListening: dailyListening,
                allHistoryData: listenedEpisodes // Return the filtered data as allHistoryData
            };
        }
    </script>

    <!-- sql.js library for client-side SQLite processing -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/sql.js/1.6.2/sql-wasm.js"></script>
</body> 