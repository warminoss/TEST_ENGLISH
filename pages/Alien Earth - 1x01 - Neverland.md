---
title: Alien Earth - 1x01 - Neverland
layout: default
---

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Alien Earth - 1x01 - Neverland</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

        :root {
            --primary-color: #007bff;
            --secondary-color: #6c757d;
            --success-color: #28a745;
            --danger-color: #dc3545;
            --light-color: #f8f9fa;
            --dark-color: #343a40;
            --background-color: #e9ecef;
            --card-background: #ffffff;
        }

        body {
            font-family: 'Roboto', sans-serif;
            background-color: var(--background-color);
            color: var(--dark-color);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .app-wrapper {
            width: 100%;
            max-width: 600px;
            background-color: var(--card-background);
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            padding: 25px;
            text-align: center;
        }

        h1 {
            color: var(--primary-color);
            margin-bottom: 20px;
            font-size: 1.8em;
        }

        .controls-container {
            margin-bottom: 25px;
            border-bottom: 2px solid #ddd;
            padding-bottom: 20px;
        }

        .control-group {
            margin-bottom: 10px;
        }
        
        .control-group:last-child {
            margin-bottom: 0;
        }

        .mode-btn, .difficulty-btn {
            background-color: var(--secondary-color);
            color: white;
            border: none;
            padding: 10px 18px;
            margin: 5px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 15px;
            transition: background-color 0.3s, transform 0.1s;
        }

        .mode-btn:hover, .difficulty-btn:hover {
            background-color: #5a6268;
        }
        
        .mode-btn:active, .difficulty-btn:active {
            transform: scale(0.98);
        }

        .mode-btn.active, .difficulty-btn.active {
            background-color: var(--primary-color);
            font-weight: bold;
        }

        .hidden { display: none; }

        #flashcard-container { perspective: 1000px; }
        #flashcard { width: 100%; height: 250px; position: relative; transform-style: preserve-3d; transition: transform 0.6s; cursor: pointer; background-color: transparent; }
        #flashcard.is-flipped { transform: rotateY(180deg); }
        .card-face { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; display: flex; justify-content: center; align-items: center; font-size: 24px; font-weight: bold; border-radius: 10px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); box-sizing: border-box; padding: 20px; }
        .card-front { background-color: var(--light-color); color: var(--dark-color); }
        .card-back { background-color: var(--primary-color); color: white; transform: rotateY(180deg); }
        .flashcard-nav { margin-top: 20px; display: flex; justify-content: space-between; align-items: center; }
        .flashcard-nav button { background-color: var(--secondary-color); color: white; border: none; padding: 10px 18px; border-radius: 5px; cursor: pointer; font-size: 1em; }
        #flashcard-progress { font-size: 1em; color: #555; }

        #quiz-question { font-size: 22px; font-weight: bold; margin-bottom: 20px; min-height: 60px; }
        #quiz-options { display: grid; grid-template-columns: 1fr; gap: 10px; }
        .option-btn { background-color: var(--light-color); border: 2px solid #ccc; padding: 15px; font-size: 16px; border-radius: 5px; cursor: pointer; transition: all 0.2s; width: 100%; text-align: left; }
        .option-btn:not([disabled]):hover { background-color: #e2e6ea; border-color: #aaa; }
        .option-btn.correct { background-color: var(--success-color); color: white; border-color: var(--success-color); }
        .option-btn.incorrect { background-color: var(--danger-color); color: white; border-color: var(--danger-color); }
        #quiz-feedback { margin-top: 15px; font-weight: bold; min-height: 24px; }
        #quiz-results { padding: 20px; border: 2px solid var(--primary-color); border-radius: 10px; }
        #quiz-results h2 { margin-top: 0; }
    </style>
</head>
<body>

    <div class="app-wrapper">
        <h1>Alien Earth - 1x01 - Neverland</h1>
        
        <div class="controls-container">
            <div class="control-group">
                <button id="mode-flashcard-btn" class="mode-btn active">Cartes Mémoire</button>
                <button id="mode-quiz-btn" class="mode-btn">Quiz</button>
            </div>
            <div id="difficulty-selector" class="control-group">
                <button class="difficulty-btn active" data-difficulty="all">Tout</button>
                <button class="difficulty-btn" data-difficulty="easy">Facile</button>
                <button class="difficulty-btn" data-difficulty="hard">Difficile</button>
            </div>
            <div class="control-group">
                 <button id="invert-btn" class="mode-btn">Inverser (EN ⇄ FR)</button>
                 <button id="shuffle-btn" class="mode-btn">Mélanger</button>
            </div>
        </div>

        <div id="flashcard-mode">
            <div id="flashcard-container">
                <div id="flashcard">
                    <div class="card-face card-front" id="flashcard-front"></div>
                    <div class="card-face card-back" id="flashcard-back"></div>
                </div>
            </div>
            <div class="flashcard-nav">
                <button id="prev-btn">Précédent</button>
                <span id="flashcard-progress"></span>
                <button id="next-btn">Suivant</button>
            </div>
        </div>

        <div id="quiz-mode" class="hidden">
            <div id="quiz-container">
                <p id="quiz-score">Score: 0</p>
                <div id="quiz-question"></div>
                <div id="quiz-options"></div>
                <div id="quiz-feedback"></div>
            </div>
            <div id="quiz-results" class="hidden">
                <h2>Quiz Terminé !</h2>
                <p id="final-score"></p>
                <button id="restart-quiz-btn" class="mode-btn">Recommencer</button>
            </div>
        </div>
    </div>

    <script>
        const vocabulary = {
          easy: [
            { term: "You gonna eat that, mate?", translation: "Tu vas manger ça, mon pote ?" },
            { term: "Fair enough", translation: "C'est juste / D'accord" },
            { term: "You're not a doctor", translation: "Tu n'es pas médecin" },
            { term: "You all right there?", translation: "Ça va, toi ?" },
            { term: "Creeps me out", translation: "Ça me fait flipper" },
            { term: "Fucking hell", translation: "Putain de merde" },
            { term: "Of course", translation: "Bien sûr" },
            { term: "Company policy", translation: "Politique de l'entreprise" },
            { term: "That's who we work for", translation: "C'est pour eux qu'on travaille" },
            { term: "I still don't get it", translation: "Je ne comprends toujours pas" },
            { term: "Poor baby", translation: "Pauvre bébé" },
            { term: "Did my job", translation: "Fait mon travail" },
            { term: "Such a jerk", translation: "Quel con" },
            { term: "Are you all good, Captain?", translation: "Tout va bien, Capitaine ?" },
            { term: "It's probably just a glitch", translation: "C'est probablement juste un bug" },
            { term: "Shut down", translation: "Arrêter / Éteindre" },
            { term: "Easy for you to say", translation: "Facile à dire pour toi" },
            { term: "That is not a real thing", translation: "Ça n'est pas un truc réel" },
            { term: "Period", translation: "Point final" },
            { term: "Got it", translation: "Compris" },
            { term: "Will do", translation: "Ce sera fait" },
            { term: "See you later, mate", translation: "À plus tard, mon pote" },
            { term: "What is it?", translation: "Qu'est-ce que c'est ?" },
            { term: "It was in the mess", translation: "C'était dans le réfectoire" },
            { term: "Squash it", translation: "Écrase-le" },
            { term: "It scares me", translation: "Ça me fait peur" },
            { term: "It's time", translation: "C'est l'heure" },
            { term: "Do we have to?", translation: "On est obligés ?" },
            { term: "I'll be here when you wake up", translation: "Je serai là quand tu te réveilleras" },
            { term: "It's just like falling asleep", translation: "C'est juste comme s'endormir" },
            { term: "Will I dream?", translation: "Vais-je rêver ?" },
            { term: "We talked about this", translation: "On en a parlé" },
            { term: "Now focus", translation: "Maintenant, concentre-toi" },
            { term: "She's pretty", translation: "Elle est jolie" },
            { term: "That's right", translation: "C'est exact" },
            { term: "I want my brother", translation: "Je veux mon frère" },
            { term: "It's a secret", translation: "C'est un secret" },
            { term: "Come on", translation: "Allez / Viens" },
            { term: "Race you", translation: "Je fais la course avec toi" },
            { term: "I don't know", translation: "Je ne sais pas" },
            { term: "So, what am I?", translation: "Alors, je suis quoi ?" },
            { term: "I'm not alone anymore", translation: "Je ne suis plus seule" },
            { term: "You're never alone", translation: "Tu n'es jamais seule" },
            { term: "Don't be sad", translation: "Ne sois pas triste" },
            { term: "Better than new", translation: "Mieux que neuf" },
            { term: "Promise?", translation: "Promis ?" },
            { term: "Go around", translation: "Fais le tour" },
            { term: "Let me in!", translation: "Laisse-moi entrer !" },
            { term: "Hurry up", translation: "Dépêche-toi" },
            { term: "You fucking asshole!", translation: "Putain de connard !" },
            { term: "See ya", translation: "À plus" },
            { term: "Where are you going?", translation: "Où vas-tu ?" },
            { term: "What's for dinner?", translation: "Qu'est-ce qu'on mange ce soir ?" },
            { term: "I miss eating", translation: "Manger me manque" },
            { term: "Good night, Joe", translation: "Bonne nuit, Joe" },
            { term: "Holy shit!", translation: "Putain de merde !" },
            { term: "What the fuck?", translation: "C'est quoi ce bordel ?" },
            { term: "Call it in", translation: "Signale-le" },
            { term: "Everyone okay?", translation: "Tout le monde va bien ?" },
            { term: "Yes, sir!", translation: "Oui, mon capitaine !" },
            { term: "Keep moving", translation: "Continuez d'avancer" },
            { term: "Anybody hurt?", translation: "Quelqu'un est blessé ?" },
            { term: "Search and rescue", translation: "Recherche et sauvetage" },
            { term: "Fucking hell", translation: "Putain de merde" },
            { term: "I should have called in sick", translation: "J'aurais dû me faire porter pâle" },
            { term: "Lock that shit down", translation: "Sécurise ce truc" },
            { term: "Copy that", translation: "Bien reçu" },
            { term: "Let's move", translation: "On y va" },
            { term: "Let's go", translation: "Allons-y" },
            { term: "Don't turn around", translation: "Ne te retourne pas" },
            { term: "On the floor", translation: "Par terre" },
            { term: "I'm not your brother", translation: "Je ne suis pas ton frère" },
            { term: "We keep going", translation: "On continue" },
            { term: "I'm gonna save him", translation: "Je vais le sauver" },
            { term: "Eyes on the ball", translation: "Reste concentré" }
          ],
          hard: [
            { term: "A heavy meal before cryo", translation: "Un repas lourd avant la cryogénisation" },
            { term: "His little beady eyes", translation: "Ses petits yeux perçants" },
            { term: "He won't be a problem", translation: "Il ne posera pas de problème" },
            { term: "Youngest trillionaire ever", translation: "Le plus jeune trillionaire de tous les temps" },
            { term: "Checked the nav computer", translation: "Vérifié l'ordinateur de navigation" },
            { term: "Log files are corrupted", translation: "Les fichiers journaux sont corrompus" },
            { term: "Burning way too much fuel", translation: "Consommer beaucoup trop de carburant" },
            { term: "The specimens are the mission", translation: "Les spécimens sont la mission" },
            { term: "Restrict you to your quarters", translation: "Te consigner dans tes quartiers" },
            { term: "On final approach", translation: "En approche finale" },
            { term: "Menaced by giants", translation: "Menacé par des géants" },
            { term: "Transition from a human body to a synthetic", translation: "Passer d'un corps humain à un synthétique" },
            { term: "The human body manufactures hormones", translation: "Le corps humain fabrique des hormones" },
            { term: "Simulating the human experience is critical", translation: "Simuler l'expérience humaine est crucial" },
            { term: "A human mind, on its path to endless life", translation: "Un esprit humain, sur son chemin vers la vie éternelle" },
            { term: "The boy genius came along", translation: "Le garçon de génie est arrivé" },
            { term: "Grown-up minds are too stiff", translation: "Les esprits adultes sont trop rigides" },
            { term: "Collision imminent", translation: "Collision imminente" },
            { term: "Impact in T-minus ten seconds", translation: "Impact dans T-moins dix secondes" },
            { term: "Downed spacecraft", translation: "Vaisseau spatial écrasé" },
            { term: "Multiple casualties", translation: "Nombreuses victimes" },
            { term: "Heading to the crash site", translation: "En direction du lieu du crash" },
            { term: "By the book", translation: "Selon les règles / Dans les règles de l'art" },
            { term: "Locate the survivors", translation: "Localiser les survivants" },
            { term: "Evacuate the wounded", translation: "Évacuer les blessés" },
            { term: "Secure the site", translation: "Sécuriser le site" },
            { term: "Fire suppression unit", translation: "Unité d'extinction d'incendie" },
            { term: "Science vessel", translation: "Vaisseau scientifique" },
            { term: "Weyland-Yutani", translation: "Weyland-Yutani (nom de la corpo)" },
            { term: "Is there a finder's fee?", translation: "Y a-t-il une prime de découverte ?" },
            { term: "The ship has been boarded", translation: "Le vaisseau a été abordé" },
            { term: "Status of cargo unknown", translation: "Statut de la cargaison inconnu" },
            { term: "Until reinforcements can arrive", translation: "Jusqu'à ce que les renforts puissent arriver" },
            { term: "I have a low sperm count", translation: "J'ai une faible numération des spermatozoïdes" },
            { term: "Beyond all human frequencies", translation: "Au-delà de toutes les fréquences humaines" },
            { term: "Deep space research vessel", translation: "Vaisseau de recherche en espace lointain" },
            { term: "Triage the rescue by income bracket", translation: "Trier les secours par tranche de revenus" },
            { term: "Call up the reserves", translation: "Faire appel aux réservistes" },
            { term: "A medic with the tactical unit", translation: "Un médecin de l'unité tactique" },
            { term: "They're still in the prototype stage", translation: "Ils sont encore au stade de prototype" },
            { term: "The loss of a single unit will be devastating", translation: "La perte d'une seule unité sera dévastatrice" },
            { term: "Monitor brain function", translation: "Surveiller la fonction cérébrale" },
            { term: "Enhanced physiognomy", translation: "Physiognomie améliorée" },
            { term: "Follow orders", translation: "Suivre les ordres" },
            { term: "Sir, yes, sir", translation: "Monsieur, oui, monsieur" },
            { term: "Off you pop", translation: "Allez, filez" },
            { term: "No immune system", translation: "Pas de système immunitaire" },
            { term: "Healthy as a horse", translation: "En pleine forme / Sain comme un charme" },
            { term: "Our batteries just run out", translation: "Nos batteries s'épuisent tout simplement" },
            { term: "A life-or-death mission", translation: "Une mission de vie ou de mort" },
            { term: "He's been shot", translation: "Il a été abattu par balle" },
            { term: "Two to the body, one in the head", translation: "Deux dans le corps, une dans la tête" },
            { term: "Let's get the fuck out of here", translation: "Tirons-nous de là, putain" },
            { term: "Prodigy search and rescue", translation: "Recherche et sauvetage de Prodigy" },
            { term: "Side arms", translation: "Armes de poing" },
            { term: "Cuff him to the pipe", translation: "Menottez-le au tuyau" },
            { term: "How the fuck would I know?", translation: "Comment diable le saurais-je ?" },
            { term: "He's a cyborg", translation: "C'est un cyborg" },
            { term: "Lab specimens seem intact", translation: "Les spécimens de laboratoire semblent intacts" },
            { term: "Moving to track the fugitives", translation: "En mouvement pour traquer les fugitifs" },
            { term: "Cryo-chamber", translation: "Chambre de cryogénisation" },
            { term: "Looks like no survivors", translation: "On dirait qu'il n'y a pas de survivants" },
            { term: "The coagulation of the blood", translation: "La coagulation du sang" },
            { term: "Something crawled out of here", translation: "Quelque chose est sorti en rampant d'ici" },
            { term: "Some kind of big-ass bug", translation: "Une sorte d'énorme insecte" },
            { term: "It fucking bit me!", translation: "Ça m'a mordu, putain !" },
            { term: "Your lives were short and filled with fear", translation: "Vos vies étaient courtes et remplies de peur" },
            { term: "You built tools to conquer nature", translation: "Vous avez construit des outils pour conquérir la nature" },
            { term: "In the animal kingdom", translation: "Dans le règne animal" },
            { term: "Injury, illness, old age", translation: "Blessure, maladie, vieillesse" }
          ]
        };

        // --- State Variables ---
        let activeWordList = [];
        let currentFlashcardIndex = 0;
        let currentQuizIndex = 0;
        let quizScore = 0;
        let isEngToFr = true;
        let currentDifficulty = 'all';

        // --- DOM Elements ---
        const modeFlashcardBtn = document.getElementById('mode-flashcard-btn');
        const modeQuizBtn = document.getElementById('mode-quiz-btn');
        const invertBtn = document.getElementById('invert-btn');
        const shuffleBtn = document.getElementById('shuffle-btn');
        const difficultyButtons = document.querySelectorAll('.difficulty-btn');
        const flashcardModeDiv = document.getElementById('flashcard-mode');
        const quizModeDiv = document.getElementById('quiz-mode');
        const flashcard = document.getElementById('flashcard');
        const flashcardFront = document.getElementById('flashcard-front');
        const flashcardBack = document.getElementById('flashcard-back');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const flashcardProgress = document.getElementById('flashcard-progress');
        const quizContainer = document.getElementById('quiz-container');
        const quizScoreEl = document.getElementById('quiz-score');
        const quizQuestionEl = document.getElementById('quiz-question');
        const quizOptionsEl = document.getElementById('quiz-options');
        const quizFeedbackEl = document.getElementById('quiz-feedback');
        const quizResultsEl = document.getElementById('quiz-results');
        const finalScoreEl = document.getElementById('final-score');
        const restartQuizBtn = document.getElementById('restart-quiz-btn');

        // --- Core Logic ---
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }
        
        function updateActiveWordList() {
            let list = [];
            if (currentDifficulty === 'easy') {
                list = [...vocabulary.easy];
            } else if (currentDifficulty === 'hard') {
                list = [...vocabulary.hard];
            } else { // 'all'
                list = [...vocabulary.easy, ...vocabulary.hard];
            }
            activeWordList = shuffleArray(list);
            resetViews();
        }

        function resetViews() {
            currentFlashcardIndex = 0;
            if (!flashcardModeDiv.classList.contains('hidden')) {
                showFlashcard();
            }
            if (!quizModeDiv.classList.contains('hidden')) {
                startQuiz();
            }
        }

        // --- Mode Switching ---
        function switchMode(mode) {
            flashcardModeDiv.classList.toggle('hidden', mode !== 'flashcard');
            quizModeDiv.classList.toggle('hidden', mode !== 'quiz');
            modeFlashcardBtn.classList.toggle('active', mode === 'flashcard');
            modeQuizBtn.classList.toggle('active', mode === 'quiz');
            resetViews();
        }
        
        modeFlashcardBtn.addEventListener('click', () => switchMode('flashcard'));
        modeQuizBtn.addEventListener('click', () => switchMode('quiz'));

        // --- Controls Event Listeners ---
        invertBtn.addEventListener('click', () => {
            isEngToFr = !isEngToFr;
            if (!flashcardModeDiv.classList.contains('hidden')) showFlashcard();
            if (!quizModeDiv.classList.contains('hidden') && !quizContainer.classList.contains('hidden')) showQuestion();
        });

        shuffleBtn.addEventListener('click', () => {
            shuffleArray(activeWordList);
            resetViews();
        });

        difficultyButtons.forEach(button => {
            button.addEventListener('click', () => {
                difficultyButtons.forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
                currentDifficulty = button.dataset.difficulty;
                updateActiveWordList();
            });
        });

        // --- Flashcard Logic ---
        function showFlashcard() {
            if (activeWordList.length === 0) {
                flashcardFront.textContent = "Aucun mot à afficher.";
                flashcardBack.textContent = "";
                flashcardProgress.textContent = "0 / 0";
                return;
            }
            flashcard.classList.remove('is-flipped');
            const item = activeWordList[currentFlashcardIndex];
            flashcardFront.textContent = isEngToFr ? item.term : item.translation;
            flashcardBack.textContent = isEngToFr ? item.translation : item.term;
            flashcardProgress.textContent = `${currentFlashcardIndex + 1} / ${activeWordList.length}`;
        }

        flashcard.addEventListener('click', () => flashcard.classList.toggle('is-flipped'));
        nextBtn.addEventListener('click', () => {
            if (activeWordList.length === 0) return;
            currentFlashcardIndex = (currentFlashcardIndex + 1) % activeWordList.length;
            showFlashcard();
        });
        prevBtn.addEventListener('click', () => {
            if (activeWordList.length === 0) return;
            currentFlashcardIndex = (currentFlashcardIndex - 1 + activeWordList.length) % activeWordList.length;
            showFlashcard();
        });

        // --- Quiz Logic ---
        function startQuiz() {
            currentQuizIndex = 0;
            quizScore = 0;
            quizScoreEl.textContent = `Score: ${quizScore}`;
            quizContainer.classList.remove('hidden');
            quizResultsEl.classList.add('hidden');
            showQuestion();
        }

        function showQuestion() {
            if (activeWordList.length === 0 || currentQuizIndex >= activeWordList.length) {
                if (activeWordList.length > 0) showResults();
                else quizQuestionEl.textContent = "Aucune question à afficher.";
                quizOptionsEl.innerHTML = '';
                return;
            }

            quizOptionsEl.innerHTML = '';
            quizFeedbackEl.textContent = '';
            const currentItem = activeWordList[currentQuizIndex];
            
            const question = isEngToFr ? currentItem.term : currentItem.translation;
            const correctAnswer = isEngToFr ? currentItem.translation : currentItem.term;
            quizQuestionEl.textContent = question;

            let options = [correctAnswer];
            let allPossibleAnswers = [...vocabulary.easy, ...vocabulary.hard].map(item => isEngToFr ? item.translation : item.term);
            allPossibleAnswers = [...new Set(allPossibleAnswers)]; // Remove duplicates
            
            while (options.length < 4 && options.length < allPossibleAnswers.length) {
                const randomAnswer = allPossibleAnswers[Math.floor(Math.random() * allPossibleAnswers.length)];
                if (!options.includes(randomAnswer)) {
                    options.push(randomAnswer);
                }
            }
            
            shuffleArray(options).forEach(option => {
                const button = document.createElement('button');
                button.textContent = option;
                button.classList.add('option-btn');
                button.addEventListener('click', () => checkAnswer(button, correctAnswer));
                quizOptionsEl.appendChild(button);
            });
        }
        
        function checkAnswer(selectedButton, correctAnswer) {
            const isCorrect = selectedButton.textContent === correctAnswer;
            Array.from(quizOptionsEl.children).forEach(btn => btn.disabled = true);

            if (isCorrect) {
                quizScore++;
                quizScoreEl.textContent = `Score: ${quizScore}`;
                selectedButton.classList.add('correct');
                quizFeedbackEl.textContent = "Correct !";
                quizFeedbackEl.style.color = "var(--success-color)";
            } else {
                selectedButton.classList.add('incorrect');
                quizFeedbackEl.textContent = `Faux ! La bonne réponse était : "${correctAnswer}"`;
                quizFeedbackEl.style.color = "var(--danger-color)";
                Array.from(quizOptionsEl.children).forEach(btn => {
                    if (btn.textContent === correctAnswer) btn.classList.add('correct');
                });
            }

            const nextQuestionDelay = isCorrect ? 1200 : 2500; // Dynamic delay
            setTimeout(() => {
                currentQuizIndex++;
                if (currentQuizIndex < activeWordList.length) {
                    showQuestion();
                } else {
                    showResults();
                }
            }, nextQuestionDelay);
        }

        function showResults() {
            quizContainer.classList.add('hidden');
            quizResultsEl.classList.remove('hidden');
            finalScoreEl.textContent = `Votre score final est de ${quizScore} sur ${activeWordList.length}.`;
        }

        restartQuizBtn.addEventListener('click', startQuiz);

        // --- Initialisation ---
        function init() {
            updateActiveWordList();
        }

        init();
    </script>

</body>
</html>