---
title: Friends - 2x13 - The One After The Super Bowl (2)
layout: default
---

<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Friends - 2x13 - The One After The Super Bowl (2)</title>
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
        <h1>Friends - 2x13 - The One After The Super Bowl (2)</h1>
        
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
            { term: "Closed set", translation: "Plateau de tournage fermé (au public)" },
            { term: "Good morning", translation: "Bonjour" },
            { term: "I don't get it", translation: "Je ne comprends pas" },
            { term: "Don't take it personal", translation: "Ne le prends pas personnellement" },
            { term: "Excuse me", translation: "Excusez-moi" },
            { term: "Walk with me", translation: "Marche avec moi" },
            { term: "Tell me", translation: "Dis-moi" },
            { term: "I hate actors", translation: "Je déteste les acteurs" },
            { term: "Great job", translation: "Beau travail" },
            { term: "Go talk to him", translation: "Va lui parler" },
            { term: "What's the worst that could happen?", translation: "Quel est le pire qui puisse arriver ?" },
            { term: "He could hear me", translation: "Il pourrait m'entendre" },
            { term: "Don't you dare!", translation: "N'ose même pas !" },
            { term: "Ask me on a date", translation: "M'inviter à un rendez-vous" },
            { term: "I'll be there", translation: "J'y serai" },
            { term: "What a jerk!", translation: "Quel crétin !" },
            { term: "Thanks, anyway", translation: "Merci quand même" },
            { term: "You can go out with him", translation: "Tu peux sortir avec lui" },
            { term: "Hang out with", translation: "Traîner avec" },
            { term: "Anybody need anything?", translation: "Quelqu'un a besoin de quelque chose ?" },
            { term: "That is so unfair", translation: "C'est tellement injuste" },
            { term: "You just know", translation: "Tu le sais, c'est tout" },
            { term: "We gotta go", translation: "On doit y aller" },
            { term: "Here's an idea", translation: "Voici une idée" },
            { term: "A monkey's gotta work", translation: "Un singe doit travailler" },
            { term: "It's no big deal", translation: "Ce n'est pas grave" },
            { term: "Let me talk", translation: "Laisse-moi parler" },
            { term: "Happy thoughts", translation: "Pensées positives" },
            { term: "Stop it", translation: "Arrête ça" },
            { term: "Is that what you want?", translation: "Est-ce que c'est ce que tu veux ?" },
            { term: "Fine!", translation: "D'accord ! / Très bien !" },
            { term: "Forget about it", translation: "Oublie ça" },
            { term: "How you doing?", translation: "Comment ça va ?" },
            { term: "Right here, right now", translation: "Ici et maintenant" },
            { term: "Hurry!", translation: "Dépêche-toi !" },
            { term: "Turn around", translation: "Retourne-toi" },
            { term: "What do you mean?", translation: "Qu'est-ce que tu veux dire ?" },
            { term: "Say you're sorry", translation: "Dis que tu es désolé" },
            { term: "Let's play", translation: "Jouons" },
            { term: "Stop the madness", translation: "Arrêtez cette folie" },
            { term: "I'm sorry", translation: "Je suis désolé(e)" },
            { term: "What are you doing here?", translation: "Qu'est-ce que tu fais ici ?" },
            { term: "Let me see!", translation: "Laisse-moi voir !" },
            { term: "All right", translation: "D'accord" },
            { term: "What happened?", translation: "Que s'est-il passé ?" },
            { term: "Say goodbye", translation: "Dire au revoir" },
            { term: "That's the way it goes", translation: "C'est comme ça que ça se passe" },
            { term: "Oh my God!", translation: "Oh mon Dieu !" },
            { term: "Cut!", translation: "Coupez !" },
            { term: "Keep your panties on", translation: "Reste calme (expression)" },
            { term: "To be under pressure", translation: "Être sous pression" },
            { term: "To get away from", translation: "S'éloigner de" },
            { term: "To ask someone out", translation: "Inviter quelqu'un à sortir" },
            { term: "To cancel", translation: "Annuler" },
            { term: "To fool around", translation: "Batifoler / S'amuser" },
            { term: "To tag along", translation: "Accompagner / S'incruster" },
            { term: "To take off", translation: "Partir / S'en aller" },
            { term: "To call in sick", translation: "Se déclarer malade" },
            { term: "To move on", translation: "Passer à autre chose" },
            { term: "Director's chair", translation: "Fauteuil de réalisateur" },
            { term: "Blind date", translation: "Rendez-vous à l'aveugle" }
          ],
          hard: [
            { term: "Flesh-eating virus", translation: "Virus mangeur de chair" },
            { term: "To acknowledge her mustache", translation: "Reconnaître (l'existence de) sa moustache" },
            { term: "To bleach it", translation: "La décolorer" },
            { term: "Making out with Gabe Kaplan", translation: "Embrasser langoureusement Gabe Kaplan" },
            { term: "Nice camouflage", translation: "Joli camouflage" },
            { term: "Animal crackers", translation: "Craquelins en forme d'animaux" },
            { term: "I wasn't a pimp", translation: "Je n'étais pas un proxénète" },
            { term: "You pulled up my skirt", translation: "Tu as soulevé ma jupe" },
            { term: "Defense mechanism", translation: "Mécanisme de défense" },
            { term: "The Muscles from Brussels", translation: "Les Muscles de Bruxelles (surnom)" },
            { term: "Wham-Bam-Van-Damme", translation: "(Surnom intraduisible de Van Damme)" },
            { term: "He totally changed time", translation: "Il a complètement changé le temps" },
            { term: "To blow me off for a monkey", translation: "Me poser un lapin pour un singe" },
            { term: "Hook up with a bunch of pigeons", translation: "Te mettre avec une bande de pigeons" },
            { term: "Stick a fork in me, I am done!", translation: "Plante une fourchette en moi, je suis cuit ! (Je suis fini)" },
            { term: "To do it in an elevator", translation: "Le faire dans un ascenseur" },
            { term: "Women's underwear", translation: "Sous-vêtements pour femmes" },
            { term: "Mealworm", translation: "Ver de farine" },
            { term: "To have something special planned", translation: "Avoir prévu quelque chose de spécial" },
            { term: "This is totally unjustified", translation: "C'est totalement injustifié" },
            { term: "You sold me out!", translation: "Tu m'as trahie !" },
            { term: "Did you just flick me?", translation: "Tu viens de me donner une pichenette ?" },
            { term: "I'm gonna kick some ass!", translation: "Je vais botter des culs !" },
            { term: "You guys would be, like, my bitches", translation: "Vous seriez, genre, mes salopes" },
            { term: "How you doing there, squirmy?", translation: "Comment ça va, le frétillant ?" },
            { term: "I'm hanging in. And a little out.", translation: "Je tiens le coup. Et ça dépasse un peu." },
            { term: "I don't do the casting", translation: "Je ne m'occupe pas du casting" },
            { term: "Let's see those panties", translation: "Voyons voir cette petite culotte" },
            { term: "Your shirt tucked into them", translation: "Ta chemise rentrée dedans" },
            { term: "Buns of Steel video", translation: "Vidéo de 'Fessiers d'Acier'" },
            { term: "To clench anything", translation: "Contracter quelque chose" },
            { term: "I was Susie Underpants", translation: "J'étais Susie Caleçon" },
            { term: "You're not getting these underpants back", translation: "Tu ne récupéreras pas cette culotte" },
            { term: "To have a threesome", translation: "Faire un plan à trois" },
            { term: "Drew has some ground rules", translation: "Drew a quelques règles de base" },
            { term: "Your sweater gets it", translation: "Ton pull va y passer" },
            { term: "Third-date sweater", translation: "Pull de troisième rendez-vous" },
            { term: "It's handbag marinara", translation: "C'est sac à main sauce marinara" },
            { term: "You don't have the guts", translation: "Tu n'as pas le cran" },
            { term: "She took off with my clothes", translation: "Elle est partie avec mes vêtements" },
            { term: "Someone's flossing!", translation: "Quelqu'un se fait un 'sourire du plombier' !" },
            { term: "I'm getting heat from the guy in the hot-pink thong", translation: "Je me fais critiquer par le gars en string rose fluo" },
            { term: "A virus victim called in sick", translation: "Une victime du virus s'est déclarée malade" },
            { term: "I'm dying on a gurney!", translation: "Je meurs sur un brancard !" },
            { term: "Can I borrow your G-string?", translation: "Je peux t'emprunter ta corde de Sol / ton string ?" },
            { term: "How long you been waiting to say that?", translation: "Ça fait combien de temps que tu attends pour dire ça ?" },
            { term: "This man is dying!", translation: "Cet homme est en train de mourir !" }
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