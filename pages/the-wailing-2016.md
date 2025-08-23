---
title: The Wailing (2016)
layout: default
---

<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Wailing (2016)</title>
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
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
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
        .card-face { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; display: flex; justify-content: center; align-items: center; font-weight: bold; border-radius: 10px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); box-sizing: border-box; padding: 20px; font-size: 24px; }
        .card-front { background-color: var(--light-color); color: var(--dark-color); }
        .card-back { background-color: var(--primary-color); color: white; transform: rotateY(180deg); }
        .flashcard-nav { margin-top: 20px; display: flex; justify-content: space-between; align-items: center; }
        .flashcard-nav button { background-color: var(--secondary-color); color: white; border: none; padding: 10px 18px; border-radius: 5px; cursor: pointer; font-size: 1em; }
        #flashcard-progress { font-size: 1em; color: #555; }

        #quiz-question { font-weight: bold; margin-bottom: 20px; min-height: 60px; display: flex; align-items: center; justify-content: center; font-size: 22px; }
        #quiz-options { display: grid; grid-template-columns: 1fr; gap: 10px; }
        .option-btn { background-color: var(--light-color); border: 2px solid #ccc; padding: 15px; font-size: 16px; border-radius: 5px; cursor: pointer; transition: all 0.2s; width: 100%; text-align: left; }
        .option-btn:not([disabled]):hover { background-color: #e2e6ea; border-color: #aaa; }
        .option-btn.correct { background-color: var(--success-color); color: white; border-color: var(--success-color); }
        .option-btn.incorrect { background-color: var(--danger-color); color: white; border-color: var(--danger-color); }
        #quiz-feedback { margin-top: 15px; font-weight: bold; min-height: 24px; }
        #quiz-results { padding: 20px; border: 2px solid var(--primary-color); border-radius: 10px; }
        #quiz-results h2 { margin-top: 0; }

        /* List Mode Styles */
        #list-container {
            max-height: 350px;
            overflow-y: auto;
            border: 1px solid #ccc;
            padding: 10px;
            border-radius: 5px;
            text-align: left;
        }
        #list-container p {
            margin: 5px 0;
            padding: 8px 5px;
            border-bottom: 1px solid #eee;
            font-size: 16px;
        }
    </style>
</head>
<body>

    <div class="app-wrapper">
        <h1>The Wailing (2016)</h1>
        
        <div class="controls-container">
            <div class="control-group">
                <button id="mode-flashcard-btn" class="mode-btn active">Cartes Mémoire</button>
                <button id="mode-quiz-btn" class="mode-btn">Quiz</button>
                <button id="mode-list-btn" class="mode-btn">Liste</button>
            </div>
            <div id="difficulty-selector" class="control-group">
                <button class="difficulty-btn active" data-difficulty="all">Tout</button>
                <button class="difficulty-btn" data-difficulty="easy">Facile</button>
                <button class="difficulty-btn" data-difficulty="hard">Difficile</button>
            </div>
             <div class="control-group" id="main-actions">
                 <button id="invert-btn" class="mode-btn">Inverser (EN ⇄ FR)</button>
                 <div id="list-controls-container" class="hidden">
                    <button id="list-shuffle-btn" class="mode-btn">Mélanger</button>
                    <button id="list-sort-az-btn" class="mode-btn">A-Z</button>
                    <button id="list-sort-za-btn" class="mode-btn">Z-A</button>
                 </div>
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

        <div id="list-mode" class="hidden">
            <div id="list-container"></div>
        </div>

    </div>

    <script>
        const vocabulary = {
          easy: [
            { term: "What took so long?", translation: "Qu'est-ce qui a pris autant de temps ?" },
            { term: "Hurry up!", translation: "Dépêche-toi !" },
            { term: "Holy shit.", translation: "Putain de merde." },
            { term: "I'll be damned.", translation: "J'en reviens pas." },
            { term: "What the hell happened?", translation: "Qu'est-ce qui s'est passé, bordel ?" },
            { term: "Rotten bastard.", translation: "Sale bâtard." },
            { term: "Ain't that it?", translation: "C'est pas ça ?" },
            { term: "Scary, my ass.", translation: "Effrayant, mon cul." },
            { term: "Talking out of your ass.", translation: "Dire des conneries." },
            { term: "Make you go crazy.", translation: "Te rendre fou." },
            { term: "Hell no.", translation: "Absolument pas." },
            { term: "Gimme a break.", translation: "Laisse-moi tranquille." },
            { term: "For crying out loud...", translation: "Bon sang..." },
            { term: "Son of a bitch!", translation: "Fils de pute !" },
            { term: "Get cleaned up.", translation: "Va te laver." },
            { term: "Don't beat yourself up.", translation: "Ne t'en veux pas." },
            { term: "It's not your fault.", translation: "Ce n'est pas de ta faute." },
            { term: "It just hit me.", translation: "Je viens de réaliser." },
            { term: "Beats me.", translation: "Aucune idée." },
            { term: "Are you out of your mind?", translation: "Tu as perdu la tête ?" },
            { term: "Get outta here!", translation: "Dégage d'ici !" },
            { term: "What a nut job.", translation: "Quel cinglé." },
            { term: "To suck your blood dry.", translation: "Te vider de ton sang." },
            { term: "Burning up.", translation: "Avoir très chaud / Brûler de fièvre." },
            { term: "Driving me up the wall.", translation: "Ça me rend fou." },
            { term: "Shut your hole.", translation: "Ferme ta gueule." },
            { term: "Make shit up.", translation: "Inventer des conneries." },
            { term: "Backing it up.", translation: "Le prouver / Soutenir ses dires." },
            { term: "You'll regret it.", translation: "Tu le regretteras." },
            { term: "In the middle of nowhere.", translation: "Au milieu de nulle part." },
            { term: "You can't miss it.", translation: "Tu ne peux pas le rater." },
            { term: "See it through.", translation: "Aller jusqu'au bout." },
            { term: "Fuck off.", translation: "Dégage." },
            { term: "What are the chances?", translation: "Quelle est la probabilité ?" },
            { term: "Snap out of it.", translation: "Reprends-toi." },
            { term: "You're scaring her.", translation: "Tu lui fais peur." },
            { term: "Stop grilling me.", translation: "Arrête de me cuisiner (de questions)." },
            { term: "Fess up.", translation: "Avoue / Crache le morceau." },
            { term: "Blowing me off.", translation: "M'ignorer / M'envoyer balader." },
            { term: "It's nothing to worry about.", translation: "Il n'y a pas de quoi s'inquiéter." },
            { term: "Brace yourself.", translation: "Prépare-toi." },
            { term: "Back out now.", translation: "Se défiler maintenant." },
            { term: "I don't follow you.", translation: "Je ne te suis pas / Je ne comprends pas." },
            { term: "I knew it.", translation: "Je le savais." },
            { term: "This ain't a joke?", translation: "C'est pas une blague ?" },
            { term: "Put it down.", translation: "Pose ça." },
            { term: "Let go!", translation: "Lâche-moi !" },
            { term: "Get your act together.", translation: "Ressaisis-toi." },
            { term: "Pick up.", translation: "Décrocher (le téléphone)." },
            { term: "Tidy up.", translation: "Ranger." },
            { term: "Get some rest.", translation: "Repose-toi un peu." },
            { term: "Change your mind.", translation: "Changer d'avis." },
            { term: "Up to you.", translation: "Ça dépend de toi." },
            { term: "That's not it.", translation: "Ce n'est pas ça." },
            { term: "Don't do it.", translation: "Ne le fais pas." },
            { term: "It's okay.", translation: "C'est bon / Ça va." },
            { term: "Just a bite!", translation: "Juste une bouchée !" },
            { term: "No big deal.", translation: "Ce n'est pas grave." },
            { term: "Come on.", translation: "Allez." },
            { term: "Yeah.", translation: "Ouais." },
            { term: "Okay.", translation: "D'accord." }
          ],
          hard: [
            { term: "Doubts rise in your minds.", translation: "Des doutes naissent dans vos esprits." },
            { term: "A ghost does not have flesh and bones.", translation: "Un fantôme n'a ni chair ni os." },
            { term: "Ginseng grower.", translation: "Cultivateur de ginseng." },
            { term: "Hurrying won't bring back the dead.", translation: "Se dépêcher ne ramènera pas les morts." },
            { term: "Action Choreographers.", translation: "Chorégraphes des scènes d'action." },
            { term: "Crime of passion.", translation: "Crime passionnel." },
            { term: "He could be high on something.", translation: "Il pourrait être défoncé à quelque chose." },
            { term: "Fucked-up mushrooms.", translation: "Champignons hallucinogènes / défoncés." },
            { term: "Spreading those stories.", translation: "Répandre ces histoires." },
            { term: "The test results on Heung-guk came back.", translation: "Les résultats des tests de Heung-guk sont revenus." },
            { term: "Public bathhouse.", translation: "Bains publics." },
            { term: "She was totally covered in rashes and boils.", translation: "Elle était totalement couverte d'éruptions et de furoncles." },
            { term: "Mumbling gibberish.", translation: "Marmonner un charabia." },
            { term: "Dimwitted buddies.", translation: "Copains un peu bêtes." },
            { term: "The rash is the link here.", translation: "L'éruption cutanée est le lien ici." },
            { term: "Medical records.", translation: "Dossiers médicaux." },
            { term: "A shaman.", translation: "Un chaman." },
            { term: "To do some ritual.", translation: "Faire un rituel." },
            { term: "The Jap with the limp.", translation: "Le Japonais qui boite." },
            { term: "He's stalking you.", translation: "Il te harcèle / te traque." },
            { term: "Deer carcass.", translation: "Carcasse de cerf." },
            { term: "Health tonics.", translation: "Toniques pour la santé." },
            { term: "Stark naked except for a diaper.", translation: "Totalement nu à l'exception d'une couche." },
            { term: "Bladder control problems are surprisingly common.", translation: "Les problèmes de contrôle de la vessie sont étonnamment courants." },
            { term: "Incontinence.", translation: "Incontinence." },
            { term: "Chewing on the guts.", translation: "Mâcher les entrailles." },
            { term: "His eyes all bloodshot.", translation: "Ses yeux tout injectés de sang." },
            { term: "You deserve to get scorched by lightning.", translation: "Tu mérites d'être foudroyé." },
            { term: "Someone keeps banging on the door.", translation: "Quelqu'un n'arrête pas de frapper fort à la porte." },
            { term: "Something's possessed Hyo-jin.", translation: "Quelque chose a possédé Hyo-jin." },
            { term: "He's in training. A deacon.", translation: "Il est en formation. Un diacre." },
            { term: "Keep a lookout.", translation: "Faire le guet." },
            { term: "He stashed the stuff.", translation: "Il a planqué les affaires." },
            { term: "He's messing with my daughter.", translation: "Il s'en prend à ma fille." },
            { term: "Cut back on the drinking.", translation: "Réduire la consommation d'alcool." },
            { term: "Casting a deadly hex.", translation: "Lancer un sortilège mortel." },
            { term: "You can't do anything that would taint it.", translation: "Tu ne peux rien faire qui pourrait le souiller." },
            { term: "The spell will backfire.", translation: "Le sort se retournera contre toi." },
            { term: "Banish it, or kill it.", translation: "Le bannir, ou le tuer." },
            { term: "He died a long time ago.", translation: "Il est mort il y a longtemps." },
            { term: "He's a master of evil.", translation: "C'est un maître du mal." },
            { term: "He just threw out the bait, and your daughter took it.", translation: "Il a juste lancé l'appât, et ta fille l'a mordu." },
            { term: "Cross your heart, or your mom's a whore?", translation: "Croix de bois, croix de fer, si je mens, je vais en enfer ?" },
            { term: "The sow is farrowing.", translation: "La truie met bas." },
            { term: "He's a renowned professor.", translation: "C'est un professeur de renom." },
            { term: "To annihilate your family.", translation: "Pour anéantir ta famille." },
            { term: "Death cannot touch him.", translation: "La mort ne peut pas l'atteindre." },
            { term: "I misread the divination.", translation: "J'ai mal interprété la divination." },
            { term: "They're in on it together.", translation: "Ils sont de mèche." },
            { term: "The rooster will cry three times.", translation: "Le coq chantera trois fois." },
            { term: "Do not waver.", translation: "Ne faiblis pas." },
            { term: "Her father has sinned.", translation: "Son père a péché." },
            { term: "He tried to kill him, and finally succeeded.", translation: "Il a essayé de le tuer, et a finalement réussi." },
            { term: "To confirm your suspicions.", translation: "Pour confirmer tes soupçons." },
            { term: "Reveal your true form.", translation: "Révèle ta vraie forme." },
            { term: "The devil.", translation: "Le diable." },
            { term: "My daughter got sick first!", translation: "Ma fille est tombée malade en premier !" }
          ]
        };

        // --- State Variables ---
        let currentWordList = [];
        let listDisplayList = []; 
        let currentFlashcardIndex = 0;
        let currentQuizIndex = 0;
        let quizScore = 0;
        let isEngToFr = true;
        let currentDifficulty = 'all';
        let currentTimeout = null;

        // --- DOM Elements ---
        const modeFlashcardBtn = document.getElementById('mode-flashcard-btn');
        const modeQuizBtn = document.getElementById('mode-quiz-btn');
        const modeListBtn = document.getElementById('mode-list-btn');
        const invertBtn = document.getElementById('invert-btn');
        const shuffleBtn = document.getElementById('shuffle-btn');
        const difficultyButtons = document.querySelectorAll('.difficulty-btn');
        
        const flashcardModeDiv = document.getElementById('flashcard-mode');
        const quizModeDiv = document.getElementById('quiz-mode');
        const listModeDiv = document.getElementById('list-mode');

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

        const listContainer = document.getElementById('list-container');
        const listControlsContainer = document.getElementById('list-controls-container');
        const listShuffleBtn = document.getElementById('list-shuffle-btn');
        const listSortAzBtn = document.getElementById('list-sort-az-btn');
        const listSortZaBtn = document.getElementById('list-sort-za-btn');

        // --- Helper Functions ---
        function shuffleArray(array) {
            let newArray = [...array];
            for (let i = newArray.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
            }
            return newArray;
        }

        function adjustFontSize(element, text) {
            if (!element) return;
            
            element.style.fontSize = '24px';
            if (text.length > 100) {
                element.style.fontSize = '14px';
            } else if (text.length > 80) {
                element.style.fontSize = '16px';
            } else if (text.length > 50) {
                element.style.fontSize = '18px';
            }
        }
        
        // --- Core Logic ---
        function setWordListByDifficulty() {
            let list = [];
            if (currentDifficulty === 'easy') {
                list = [...vocabulary.easy];
            } else if (currentDifficulty === 'hard') {
                list = [...vocabulary.hard];
            } else {
                list = [...vocabulary.easy, ...vocabulary.hard];
            }
            currentWordList = shuffleArray(list);
            listDisplayList = [...currentWordList];
            currentFlashcardIndex = 0;
            updateView();
        }

        function updateView() {
            const activeMode = document.querySelector('.mode-btn.active')?.id;
            if (activeMode === 'mode-flashcard-btn') showFlashcard();
            else if (activeMode === 'mode-quiz-btn') startQuiz();
            else if (activeMode === 'mode-list-btn') showList();
        }

        // --- Mode Switching ---
        function switchMode(mode) {
            if (currentTimeout) {
                clearTimeout(currentTimeout);
                currentTimeout = null;
            }
            
            flashcardModeDiv.classList.toggle('hidden', mode !== 'flashcard');
            quizModeDiv.classList.toggle('hidden', mode !== 'quiz');
            listModeDiv.classList.toggle('hidden', mode !== 'list');
            
            modeFlashcardBtn.classList.toggle('active', mode === 'flashcard');
            modeQuizBtn.classList.toggle('active', mode === 'quiz');
            modeListBtn.classList.toggle('active', mode === 'list');

            shuffleBtn.classList.toggle('hidden', mode === 'list');
            listControlsContainer.classList.toggle('hidden', mode !== 'list');
            
            updateView();
        }
        
        // --- Controls Event Listeners ---
        invertBtn.addEventListener('click', () => {
            isEngToFr = !isEngToFr;
            updateView();
        });

        shuffleBtn.addEventListener('click', () => {
            currentWordList = shuffleArray(currentWordList);
            currentFlashcardIndex = 0;
            updateView();
        });

        difficultyButtons.forEach(button => {
            button.addEventListener('click', () => {
                difficultyButtons.forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
                currentDifficulty = button.dataset.difficulty;
                setWordListByDifficulty();
            });
        });
        
        modeFlashcardBtn.addEventListener('click', () => switchMode('flashcard'));
        modeQuizBtn.addEventListener('click', () => switchMode('quiz'));
        modeListBtn.addEventListener('click', () => switchMode('list'));

        // --- Flashcard Logic ---
        function showFlashcard() {
            if (currentWordList.length === 0) {
                if (flashcardFront) flashcardFront.textContent = "Aucun mot à afficher.";
                if (flashcardBack) flashcardBack.textContent = "";
                if (flashcardProgress) flashcardProgress.textContent = "0 / 0";
                return;
            }
            
            if (flashcard) flashcard.classList.remove('is-flipped');
            const item = currentWordList[currentFlashcardIndex];
            
            const frontText = isEngToFr ? item.term : item.translation;
            const backText = isEngToFr ? item.translation : item.term;

            if (flashcardFront) {
                flashcardFront.textContent = frontText;
                adjustFontSize(flashcardFront, frontText);
            }
            if (flashcardBack) {
                flashcardBack.textContent = backText;
                adjustFontSize(flashcardBack, backText);
            }

            if (flashcardProgress) {
                flashcardProgress.textContent = `${currentFlashcardIndex + 1} / ${currentWordList.length}`;
            }
        }

        if (flashcard) {
            flashcard.addEventListener('click', () => flashcard.classList.toggle('is-flipped'));
        }
        
        if (nextBtn) {
            nextBtn.addEventListener('click', () => {
                if (currentWordList.length === 0) return;
                currentFlashcardIndex = (currentFlashcardIndex + 1) % currentWordList.length;
                showFlashcard();
            });
        }
        
        if (prevBtn) {
            prevBtn.addEventListener('click', () => {
                if (currentWordList.length === 0) return;
                currentFlashcardIndex = (currentFlashcardIndex - 1 + currentWordList.length) % currentWordList.length;
                showFlashcard();
            });
        }

        // --- Quiz Logic ---
        function startQuiz() {
            currentQuizIndex = 0;
            quizScore = 0;
            if (quizScoreEl) quizScoreEl.textContent = `Score: ${quizScore}`;
            if (quizContainer) quizContainer.classList.remove('hidden');
            if (quizResultsEl) quizResultsEl.classList.add('hidden');
            showQuestion();
        }

        function showQuestion() {
            if (currentWordList.length === 0 || currentQuizIndex >= currentWordList.length) {
                if (currentWordList.length > 0) showResults();
                else {
                    if (quizQuestionEl) quizQuestionEl.textContent = "Aucune question à afficher.";
                    if (quizOptionsEl) quizOptionsEl.innerHTML = '';
                }
                return;
            }

            if (quizOptionsEl) quizOptionsEl.innerHTML = '';
            if (quizFeedbackEl) quizFeedbackEl.textContent = '';
            
            const currentItem = currentWordList[currentQuizIndex];
            const questionText = isEngToFr ? currentItem.term : currentItem.translation;
            const correctAnswer = isEngToFr ? currentItem.translation : currentItem.term;
            
            if (quizQuestionEl) {
                quizQuestionEl.textContent = questionText;
                adjustFontSize(quizQuestionEl, questionText);
            }

            let options = [correctAnswer];
            let allPossibleAnswers = [...vocabulary.easy, ...vocabulary.hard].map(item => isEngToFr ? item.translation : item.term);
            allPossibleAnswers = [...new Set(allPossibleAnswers)];
            
            let attempts = 0;
            const maxAttempts = allPossibleAnswers.length * 2;
            
            while (options.length < 4 && options.length < allPossibleAnswers.length && attempts < maxAttempts) {
                const randomAnswer = allPossibleAnswers[Math.floor(Math.random() * allPossibleAnswers.length)];
                if (!options.includes(randomAnswer)) {
                    options.push(randomAnswer);
                }
                attempts++;
            }
            
            if (quizOptionsEl) {
                shuffleArray(options).forEach(option => {
                    const button = document.createElement('button');
                    button.textContent = option;
                    button.classList.add('option-btn');
                    button.addEventListener('click', () => checkAnswer(button, correctAnswer));
                    quizOptionsEl.appendChild(button);
                });
            }
        }
        
        function checkAnswer(selectedButton, correctAnswer) {
            const isCorrect = selectedButton.textContent === correctAnswer;
            
            if (currentTimeout) {
                clearTimeout(currentTimeout);
                currentTimeout = null;
            }
            
            if (quizOptionsEl) {
                Array.from(quizOptionsEl.children).forEach(btn => btn.disabled = true);
            }

            if (isCorrect) {
                quizScore++;
                if (quizScoreEl) quizScoreEl.textContent = `Score: ${quizScore}`;
                selectedButton.classList.add('correct');
                if (quizFeedbackEl) {
                    quizFeedbackEl.textContent = "Correct !";
                    quizFeedbackEl.style.color = "var(--success-color)";
                }
            } else {
                selectedButton.classList.add('incorrect');
                if (quizFeedbackEl) {
                    quizFeedbackEl.textContent = `Faux ! La réponse était : "${correctAnswer}"`;
                    quizFeedbackEl.style.color = "var(--danger-color)";
                }
                if (quizOptionsEl) {
                    Array.from(quizOptionsEl.children).forEach(btn => {
                        if (btn.textContent === correctAnswer) btn.classList.add('correct');
                    });
                }
            }

            const nextQuestionDelay = isCorrect ? 1200 : 2500;
            currentTimeout = setTimeout(() => {
                currentQuizIndex++;
                if (currentQuizIndex < currentWordList.length) {
                    showQuestion();
                } else {
                    showResults();
                }
                currentTimeout = null;
            }, nextQuestionDelay);
        }

        function showResults() {
            if (quizContainer) quizContainer.classList.add('hidden');
            if (quizResultsEl) quizResultsEl.classList.remove('hidden');
            if (finalScoreEl) {
                finalScoreEl.textContent = `Votre score final est de ${quizScore} sur ${currentWordList.length}.`;
            }
        }

        if (restartQuizBtn) {
            restartQuizBtn.addEventListener('click', startQuiz);
        }

        // --- List Logic ---
        function showList() {
            if (!listContainer) return;
            
            listContainer.innerHTML = '';
            if (listDisplayList.length === 0) {
                listContainer.innerHTML = '<p>Aucun mot à afficher.</p>';
                return;
            }

            listDisplayList.forEach(item => {
                const p = document.createElement('p');
                const first = isEngToFr ? item.term : item.translation;
                const second = isEngToFr ? item.translation : item.term;
                p.innerHTML = `<strong>${first}</strong> = ${second}`;
                listContainer.appendChild(p);
            });
        }

        if (listShuffleBtn) {
            listShuffleBtn.addEventListener('click', () => {
                listDisplayList = shuffleArray(listDisplayList);
                showList();
            });
        }

        if (listSortAzBtn) {
            listSortAzBtn.addEventListener('click', () => {
                const sortKey = isEngToFr ? 'term' : 'translation';
                listDisplayList.sort((a, b) => a[sortKey].localeCompare(b[sortKey]));
                showList();
            });
        }
        
        if (listSortZaBtn) {
            listSortZaBtn.addEventListener('click', () => {
                const sortKey = isEngToFr ? 'term' : 'translation';
                listDisplayList.sort((a, b) => b[sortKey].localeCompare(a[sortKey]));
                showList();
            });
        }

        // --- Initialisation ---
        function init() {
            if (!modeFlashcardBtn || !flashcardFront || !quizContainer) {
                console.error('Éléments DOM manquants');
                return;
            }
            
            setWordListByDifficulty();
        }

        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', init);
        } else {
            init();
        }
    </script>

</body>
</html>