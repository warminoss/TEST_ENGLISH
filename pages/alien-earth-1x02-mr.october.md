---
title: Alien Earth - 1x02 - Mr. October
layout: default
---

<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Alien Earth - 1x02 - Mr. October</title>
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
        <h1>Alien Earth - 1x02 - Mr. October</h1>
        
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
            { term: "Fuck me.", translation: "Putain." },
            { term: "Hey.", translation: "Hé." },
            { term: "Come on.", translation: "Allez." },
            { term: "Up or down?", translation: "En haut ou en bas ?" },
            { term: "Let's move.", translation: "On bouge." },
            { term: "I hate it.", translation: "Je déteste ça." },
            { term: "I'm scared.", translation: "J'ai peur." },
            { term: "It'll be worth it.", translation: "Ça en vaudra la peine." },
            { term: "Uh-huh.", translation: "Ouais." },
            { term: "I'm serious.", translation: "Je suis sérieux." },
            { term: "Thanks.", translation: "Merci." },
            { term: "You're smart.", translation: "Tu es intelligent." },
            { term: "Maybe.", translation: "Peut-être." },
            { term: "Don't think about that.", translation: "Ne pense pas à ça." },
            { term: "It's a secret.", translation: "C'est un secret." },
            { term: "Okay?", translation: "D'accord ?" },
            { term: "Don't worry.", translation: "Ne t'inquiète pas." },
            { term: "Find who?", translation: "Trouver qui ?" },
            { term: "Fuck that.", translation: "Va te faire foutre." },
            { term: "Watch this.", translation: "Regarde ça." },
            { term: "I got you.", translation: "Je te tiens." },
            { term: "All right. Okay.", translation: "D'accord. Okay." },
            { term: "Oh, shit.", translation: "Oh, merde." },
            { term: "Fuck. Fuck.", translation: "Putain. Putain." },
            { term: "Right.", translation: "C'est ça." },
            { term: "What just happened?", translation: "Que vient-il de se passer ?" },
            { term: "Do you hear that?", translation: "Tu entends ça ?" },
            { term: "What?", translation: "Quoi ?" },
            { term: "This way.", translation: "Par ici." },
            { term: "Me?", translation: "Moi ?" },
            { term: "You're telling me.", translation: "À qui le dis-tu." },
            { term: "Of course.", translation: "Bien sûr." },
            { term: "I miss baths.", translation: "Les bains me manquent." },
            { term: "Quiet now.", translation: "Silence maintenant." },
            { term: "Now move.", translation: "Bougez maintenant." },
            { term: "Yo, what is that?", translation: "Yo, c'est quoi ça ?" },
            { term: "Wait, was that blood?", translation: "Attends, c'était du sang ?" },
            { term: "Gross.", translation: "Dégueu." },
            { term: "What is that?", translation: "Qu'est-ce que c'est ?" },
            { term: "Yuck!", translation: "Beurk !" },
            { term: "Please?", translation: "S'il te plaît ?" },
            { term: "Dude.", translation: "Mec." },
            { term: "It's me.", translation: "C'est moi." },
            { term: "I found you.", translation: "Je t'ai trouvé." },
            { term: "You remember?", translation: "Tu te souviens ?" },
            { term: "I'm sorry.", translation: "Je suis désolé(e)." },
            { term: "It is my fault.", translation: "C'est de ma faute." },
            { term: "No, no, no.", translation: "Non, non, non." },
            { term: "You died.", translation: "Tu es mort." },
            { term: "I don't understand.", translation: "Je ne comprends pas." },
            { term: "I could have done that.", translation: "J'aurais pu faire ça." },
            { term: "Guys?", translation: "Les gars ?" },
            { term: "Call it in.", translation: "Signale-le." },
            { term: "No!", translation: "Non !" },
            { term: "Stay here.", translation: "Reste ici." },
            { term: "Look at me.", translation: "Regarde-moi." },
            { term: "Open fire!", translation: "Ouvrez le feu !" },
            { term: "Keep up.", translation: "Suis le rythme." },
            { term: "Calm down.", translation: "Calme-toi." },
            { term: "Hands. Now!", translation: "Les mains. Maintenant !" },
            { term: "Damn.", translation: "Zut." },
            { term: "Who are you talking to?", translation: "À qui parles-tu ?" },
            { term: "You're hurt.", translation: "Tu es blessé." },
            { term: "Yeah, okay.", translation: "Ouais, d'accord." },
            { term: "Go.", translation: "Va." },
            { term: "Let me.", translation: "Laisse-moi faire." }
          ],
          hard: [
            { term: "Who does surgery on a crashing ship?", translation: "Qui opère sur un vaisseau qui s'écrase ?" },
            { term: "Looks like suffocation.", translation: "On dirait une suffocation." },
            { term: "Cabin depressurized.", translation: "Cabine dépressurisée." },
            { term: "Some kind of toxin.", translation: "Une sorte de toxine." },
            { term: "Foreign bodies lining his GI tract.", translation: "Des corps étrangers tapissant son tube digestif." },
            { term: "There's an incident code.", translation: "Il y a un code d'incident." },
            { term: "Let forensics Agatha Christie this shit.", translation: "Laissons la police scientifique jouer les Agatha Christie sur cette merde." },
            { term: "This day just gets better and better.", translation: "Cette journée ne fait que s'améliorer." },
            { term: "I'm really looking forward to not being sick anymore.", translation: "J'ai vraiment hâte de ne plus être malade." },
            { term: "I could solve any problem.", translation: "Je pourrais résoudre n'importe quel problème." },
            { term: "Such potential.", translation: "Un tel potentiel." },
            { term: "You're talking about achievement.", translation: "Tu parles de réussite." },
            { term: "It's about becoming.", translation: "Il s'agit de devenir." },
            { term: "The fear with artificial intelligence is that...", translation: "La peur avec l'intelligence artificielle, c'est que..." },
            { term: "Exploding human potential.", translation: "Faire exploser le potentiel humain." },
            { term: "It's an intelligence race.", translation: "C'est une course à l'intelligence." },
            { term: "We ended death.", translation: "Nous avons mis fin à la mort." },
            { term: "Make consumers immortal.", translation: "Rendre les consommateurs immortels." },
            { term: "Data cross-references with the musings of every great philosopher.", translation: "Les données sont croisées avec les réflexions de tous les grands philosophes." },
            { term: "That's not wisdom.", translation: "Ce n'est pas de la sagesse." },
            { term: "Debate with someone who blows my mind.", translation: "Débattre avec quelqu'un qui m'épate." },
            { term: "A supercomputer for a brain.", translation: "Un superordinateur en guise de cerveau." },
            { term: "He can break and bleed and burn.", translation: "Il peut se casser, saigner et brûler." },
            { term: "He's not premium.", translation: "Il n'est pas de qualité supérieure." },
            { term: "We burned him on this pyre, which is very holy.", translation: "Nous l'avons brûlé sur ce bûcher, ce qui est très sacré." },
            { term: "Ready yourselves. Gear up.", translation: "Préparez-vous. Équipez-vous." },
            { term: "We're gonna have to jump.", translation: "On va devoir sauter." },
            { term: "Don't you fucking drop me.", translation: "Ne me lâche surtout pas, putain." },
            { term: "Haul ass!", translation: "Magne-toi le cul !" },
            { term: "Requesting backup.", translation: "Demande de renforts." },
            { term: "She's loose, approximately eight feet tall.", translation: "Elle est en liberté, environ 2m40 de haut." },
            { term: "Armored.", translation: "Blindé." },
            { term: "Does anyone read me?", translation: "Quelqu'un me reçoit ?" },
            { term: "I'm following up on forms I submitted on resigning.", translation: "Je fais suite aux formulaires que j'ai soumis pour démissionner." },
            { term: "Special dispensation on account of...", translation: "Une dérogation spéciale en raison de..." },
            { term: "She rewrote the code somehow.", translation: "Elle a réécrit le code d'une manière ou d'une autre." },
            { term: "Data streams in both directions.", translation: "Les flux de données vont dans les deux sens." },
            { term: "It never occurred to us that...", translation: "Il ne nous est jamais venu à l'esprit que..." },
            { term: "Your request has been denied.", translation: "Votre demande a été refusée." },
            { term: "Crisis response team.", translation: "Équipe d'intervention de crise." },
            { term: "Comms are still down.", translation: "Les communications sont toujours coupées." },
            { term: "The fuel source on the ship, that it's unstable.", translation: "La source de carburant du vaisseau, qu'elle soit instable." },
            { term: "Reinforcements from the city.", translation: "Des renforts de la ville." },
            { term: "A special unit here under orders from the founder.", translation: "Une unité spéciale ici sous les ordres du fondateur." },
            { term: "Assess the damage, rescue survivors.", translation: "Évaluer les dégâts, secourir les survivants." },
            { term: "Data's pouring in.", translation: "Les données affluent." },
            { term: "It's been a very expensive day.", translation: "La journée a été très coûteuse." },
            { term: "Reparations will be made.", translation: "Des réparations seront effectuées." },
            { term: "Everything on board should be considered proprietary.", translation: "Tout ce qui est à bord doit être considéré comme confidentiel." },
            { term: "The tower is still too unstable.", translation: "La tour est encore trop instable." },
            { term: "I must insist.", translation: "Je dois insister." },
            { term: "Any incursion onto Prodigy territory will be construed as a hostile act.", translation: "Toute incursion sur le territoire de Prodigy sera interprétée comme un acte hostile." },
            { term: "Fear is for animals.", translation: "La peur est pour les animaux." },
            { term: "This is a non-terrestrial species.", translation: "C'est une espèce non terrestre." },
            { term: "We don't know what it's capable of.", translation: "Nous ne savons pas de quoi elle est capable." },
            { term: "You're on point.", translation: "Tu ouvres la marche." },
            { term: "Find something to capture it with.", translation: "Trouvez quelque chose pour le capturer." },
            { term: "Secure the area.", translation: "Sécurisez la zone." },
            { term: "Nobody gets in or out until the hazmat team arrives.", translation: "Personne n'entre ou ne sort jusqu'à l'arrivée de l'équipe NRBC." },
            { term: "Warrior mode.", translation: "Mode guerrier." },
            { term: "He's answered the whole world.", translation: "Il a répondu au monde entier." },
            { term: "What are the odds?", translation: "Quelle est la probabilité ?" },
            { term: "What a colossal blow.", translation: "Quel coup colossal." },
            { term: "The old highlight reels.", translation: "Les vieilles bobines des meilleurs moments." },
            { term: "He said sometimes the world gives you a moment to shine.", translation: "Il disait que parfois le monde vous donne un moment pour briller." },
            { term: "These are invasive species.", translation: "Ce sont des espèces envahissantes." },
            { term: "If we don't lock them down, they could escape, breed.", translation: "Si on ne les confine pas, elles pourraient s'échapper, se reproduire." },
            { term: "Lives are at stake.", translation: "Des vies sont en jeu." },
            { term: "Guard the omelet.", translation: "Garde l'omelette." },
            { term: "The sound of metal bending.", translation: "Le bruit du métal qui se plie." },
            { term: "He's a cyborg.", translation: "C'est un cyborg." },
            { term: "It presents as flora, but it may be fauna.", translation: "Ça se présente comme de la flore, mais c'est peut-être de la faune." }
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