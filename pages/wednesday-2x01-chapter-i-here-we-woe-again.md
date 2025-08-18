---
title: Wednesday - 2x01 - Chapter I  Here We Woe Again
layout: default
---

<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Wednesday - 2x01 - Chapter I  Here We Woe Again</title>
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
        <h1>Wednesday - 2x01 - Chapter I  Here We Woe Again</h1>
        
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
            { term: "Who said nightmares don't come true?", translation: "Qui a dit que les cauchemars ne se réalisaient pas ?" },
            { term: "When she was snatched.", translation: "Quand elle a été enlevée." },
            { term: "Random screening searches.", translation: "Fouilles de contrôle aléatoires." },
            { term: "Anything else on your person?", translation: "Autre chose sur vous ?" },
            { term: "Prohibited items.", translation: "Articles interdits." },
            { term: "It's a big no-no.", translation: "C'est un grand non." },
            { term: "I'd appreciate it if you didn't waste our time.", translation: "J'apprécierais que vous ne nous fassiez pas perdre notre temps." },
            { term: "You're not to breathe a word of this.", translation: "Tu ne dois pas en souffler mot." },
            { term: "Let's play dolls.", translation: "Jouons à la poupée." },
            { term: "We must be off.", translation: "Nous devons y aller." },
            { term: "You don't want to be late.", translation: "Tu ne veux pas être en retard." },
            { term: "I feel like I've barely seen you.", translation: "J'ai l'impression de t'avoir à peine vue." },
            { term: "Chop-chop.", translation: "Vite, vite." },
            { term: "That's my boy.", translation: "C'est mon garçon." },
            { term: "It's a state of mind.", translation: "C'est un état d'esprit." },
            { term: "You need to work on your aim.", translation: "Tu dois travailler ta visée." },
            { term: "That's the spirit.", translation: "C'est le bon état d'esprit." },
            { term: "I had Thing rewire it.", translation: "J'ai fait recâbler ça par La Chose." },
            { term: "They're onto me.", translation: "Ils m'ont repérée." },
            { term: "Willingly returned to a school.", translation: "Retournée volontairement dans une école." },
            { term: "Like returning to the scene of the crime.", translation: "Comme retourner sur les lieux du crime." },
            { term: "He's in charge of this family.", translation: "Il est le chef de cette famille." },
            { term: "I always do.", translation: "Je le fais toujours." },
            { term: "Here are the ground rules.", translation: "Voici les règles de base." },
            { term: "No eye contact without permission.", translation: "Pas de contact visuel sans permission." },
            { term: "Can I get your autograph?", translation: "Puis-je avoir votre autographe ?" },
            { term: "I would expect nothing less.", translation: "Je n'en attendais pas moins." },
            { term: "You are done here. Shoo.", translation: "Vous avez fini ici. Ouste." },
            { term: "That was disturbing.", translation: "C'était dérangeant." },
            { term: "You better get used to it.", translation: "Tu ferais mieux de t'y habituer." },
            { term: "You're a big deal.", translation: "Tu es quelqu'un d'important." },
            { term: "It is an honor to meet the savior of Nevermore.", translation: "C'est un honneur de rencontrer la sauveuse de Nevermore." },
            { term: "Allow me to introduce myself.", translation: "Permettez-moi de me présenter." },
            { term: "Please consider it.", translation: "Veuillez y réfléchir." },
            { term: "I want us to be allies in this struggle.", translation: "Je veux que nous soyons alliés dans cette lutte." },
            { term: "Returning Nevermore to its glory days.", translation: "Rendre à Nevermore ses jours de gloire." },
            { term: "I hate to speak ill of the dead.", translation: "Je déteste dire du mal des morts." },
            { term: "Howdy, roomie.", translation: "Salut, coloc." },
            { term: "Catch up with you later.", translation: "On se retrouve plus tard." },
            { term: "Absolutely.", translation: "Absolument." },
            { term: "Can't wait.", translation: "J'ai hâte." },
            { term: "How was your vacay?", translation: "Comment étaient tes vacances ?" },
            { term: "I've been dying to tell you.", translation: "Je meurs d'envie de te raconter." },
            { term: "I'll spare you the details.", translation: "Je t'épargnerai les détails." },
            { term: "A bad pun.", translation: "Un mauvais jeu de mots." },
            { term: "I wanna secure my place in the pack.", translation: "Je veux assurer ma place dans la meute." },
            { term: "This one here just looks like a typo.", translation: "Celui-ci ressemble juste à une faute de frappe." },
            { term: "We seriously need to set some boundaries.", translation: "Nous devons sérieusement fixer des limites." },
            { term: "Is Enid around?", translation: "Est-ce qu'Enid est dans le coin ?" },
            { term: "Would you mind giving her this?", translation: "Ça vous dérangerait de lui donner ça ?" },
            { term: "Thanks for covering.", translation: "Merci d'avoir couvert pour moi." },
            { term: "Relationship drama.", translation: "Drame relationnel." },
            { term: "In 25 words or less. Go.", translation: "En 25 mots ou moins. Vas-y." },
            { term: "Get it over with.", translation: "En finir avec ça." },
            { term: "Holy crap!", translation: "Bon sang !" },
            { term: "You have a stalker?", translation: "Tu as un harceleur ?" },
            { term: "Don't be jealous.", translation: "Ne sois pas jalouse." },
            { term: "Provide alibis.", translation: "Fournir des alibis." },
            { term: "Perfect for late-night munchies.", translation: "Parfait pour une fringale de fin de soirée." },
            { term: "They're pets, not snacks.", translation: "Ce sont des animaux de compagnie, pas des en-cas." },
            { term: "I could tag along.", translation: "Je pourrais me joindre à vous." },
            { term: "That's not gonna happen.", translation: "Ça n'arrivera pas." },
            { term: "She finally come to her senses?", translation: "A-t-elle enfin repris ses esprits ?" },
            { term: "I realize I've ambushed you.", translation: "Je réalise que je vous ai prise en embuscade." },
            { term: "My next chapter might be.", translation: "Quel pourrait être mon prochain chapitre." },
            { term: "I will not let history repeat itself.", translation: "Je ne laisserai pas l'histoire se répéter." },
            { term: "Keep your nosy fingers out of my business.", translation: "Garde tes doigts fouineurs hors de mes affaires." },
            { term: "Getting to the bottom of this.", translation: "Aller au fond de cette affaire." },
            { term: "A brilliant boy with a fragile heart.", translation: "Un garçon brillant au cœur fragile." },
            { term: "An unmarked grave.", translation: "Une tombe anonyme." },
            { term: "No Outcast should be limited or shamed.", translation: "Aucun Paria ne devrait être limité ou avoir honte." },
            { term: "See if you can squeeze this in.", translation: "Voir si tu peux caser ça." },
            { term: "The easiest punching bags.", translation: "Les punching-balls les plus faciles." },
            { term: "Look no further.", translation: "Ne cherchez pas plus loin." },
            { term: "One of our best and brightest.", translation: "L'une de nos meilleures et plus brillantes." },
            { term: "The image suddenly flashed in my head.", translation: "L'image a soudainement flashé dans ma tête." },
            { term: "A creepy crow on a headstone.", translation: "Un corbeau effrayant sur une pierre tombale." },
            { term: "My stalker's going to burn my manuscript.", translation: "Mon harceleur va brûler mon manuscrit." },
            { term: "Go up in flames.", translation: "Partir en fumée." },
            { term: "Be on the lookout for anyone watching us.", translation: "Sois à l'affût de quiconque nous regarde." },
            { term: "I'm sneaking off to the Skull Tree.", translation: "Je me faufile jusqu'à l'Arbre du Crâne." },
            { term: "I found a few more typos.", translation: "J'ai trouvé quelques fautes de frappe de plus." },
            { term: "I left a crazy number of voicemails.", translation: "J'ai laissé un nombre fou de messages vocaux." },
            { term: "I was unplugged for a whole month.", translation: "J'ai été déconnecté pendant un mois entier." },
            { term: "It kind of feels like you've been avoiding me.", translation: "J'ai un peu l'impression que tu m'évites." },
            { term: "She never disappoints.", translation: "Elle ne déçoit jamais." },
            { term: "I'm kind of in the middle of something right now.", translation: "Je suis un peu au milieu de quelque chose en ce moment." },
            { term: "Right on cue!", translation: "Pile au bon moment !" },
            { term: "A few words of inspiration.", translation: "Quelques mots d'inspiration." },
            { term: "This is all your fault.", translation: "Tout est de ta faute." },
            { term: "I died because of you.", translation: "Je suis mort(e) à cause de toi." }
          ],
          hard: [
            { term: "It's been an eventful summer.", translation: "L'été a été mouvementé." },
            { term: "I'm tied up in a serial killer's basement.", translation: "Je suis ligotée dans le sous-sol d'un tueur en série." },
            { term: "He's under the delusion that...", translation: "Il a l'illusion que..." },
            { term: "I'll let him cherish that notion.", translation: "Je vais le laisser chérir cette idée." },
            { term: "My predicament.", translation: "Ma situation difficile." },
            { term: "Mastering my psychic ability.", translation: "Maîtriser ma capacité psychique." },
            { term: "I set my sights on an obsession.", translation: "J'ai jeté mon dévolu sur une obsession." },
            { term: "America's most elusive serial killer.", translation: "Le tueur en série le plus insaisissable d'Amérique." },
            { term: "In my crosshairs.", translation: "Dans ma ligne de mire." },
            { term: "A harrowing obstacle to overcome.", translation: "Un obstacle angoissant à surmonter." },
            { term: "It's a prosthetic hand.", translation: "C'est une main prosthétique." },
            { term: "Let me show you some of my own handiwork.", translation: "Laisse-moi te montrer une partie de mon propre travail." },
            { term: "A minor psychic glitch.", translation: "Un petit pépin psychique." },
            { term: "I indulged in my favorite passions.", translation: "Je me suis adonnée à mes passions favorites." },
            { term: "Torment and humiliation.", translation: "Tourment et humiliation." },
            { term: "A molten apocalypse.", translation: "Une apocalypse en fusion." },
            { term: "Remember where your loyalty lies.", translation: "Souviens-toi où se situe ta loyauté." },
            { term: "You're a chip off the old executioner's block.", translation: "Tu es bien la digne descendante du bourreau." },
            { term: "Lurch can attest to that.", translation: "Lurch peut en témoigner." },
            { term: "The evidence is safe at the bullpen.", translation: "Les preuves sont en sécurité au commissariat (QG de la police)." },
            { term: "I will bend this place to my will.", translation: "Je plierai cet endroit à ma volonté." },
            { term: "Control is often an illusion.", translation: "Le contrôle est souvent une illusion." },
            { term: "Bullying assistance requests.", translation: "Demandes d'assistance pour l'intimidation." },
            { term: "I only sign my name in blood.", translation: "Je ne signe mon nom qu'avec du sang." },
            { term: "You're not much of a clout chaser.", translation: "Tu n'es pas vraiment du genre à chercher le buzz." },
            { term: "The harder you repel it, the harder it comes for you.", translation: "Plus tu le repousses, plus il revient vers toi avec force." },
            { term: "Do not resuscitate.", translation: "Ne pas réanimer." },
            { term: "There's that wicked tongue I've heard about.", translation: "Voilà cette langue acérée dont j'ai entendu parler." },
            { term: "I am reinstating the Founder's Pyre ceremony.", translation: "Je réinstaure la cérémonie du Bûcher du Fondateur." },
            { term: "I'd rather be burned at the stake.", translation: "Je préférerais être brûlée sur le bûcher." },
            { term: "Weems really fudged up.", translation: "Weems a vraiment tout fait rater." },
            { term: "I thought you'd love the literary reference.", translation: "Je pensais que tu adorerais la référence littéraire." },
            { term: "It's made from real human hair.", translation: "C'est fait avec de vrais cheveux humains." },
            { term: "Personal evolution.", translation: "Évolution personnelle." },
            { term: "I don't evolve. I cocoon.", translation: "Je n'évolue pas. Je m'enferme dans un cocon." },
            { term: "Death by a thousand notes.", translation: "La mort par mille notes (critiques)." },
            { term: "Old English spelling for dismemberment.", translation: "L'orthographe en vieil anglais pour démembrement." },
            { term: "They'll have to pry it out of my cold, dead hands.", translation: "Ils devront l'arracher de mes mains froides et mortes." },
            { term: "Brattish autograph hounds.", translation: "Sales gosses chasseurs d'autographes." },
            { term: "I scalped a serial killer.", translation: "J'ai scalpé un tueur en série." },
            { term: "I threatened to neuter them.", translation: "Je les ai menacés de les castrer." },
            { term: "Their most dismal moments.", translation: "Leurs moments les plus lugubres." },
            { term: "My old stomping ground.", translation: "Mon ancien terrain de jeu." },
            { term: "Let's just see how this pans out first.", translation: "Voyons d'abord comment ça se déroule." },
            { term: "I'm your new resident advisor.", translation: "Je suis votre nouveau conseiller résident." },
            { term: "I liked it better when I was feared and hated.", translation: "Je préférais quand j'étais crainte et détestée." },
            { term: "Our little angel of death.", translation: "Notre petit ange de la mort." },
            { term: "He pulled his son and his endowment from Nevermore.", translation: "Il a retiré son fils et sa dotation de Nevermore." },
            { term: "The gardener's cottage was reserved for faculty.", translation: "Le cottage du jardinier était réservé au corps enseignant." },
            { term: "A homicidal maniac.", translation: "Un maniaque homicide." },
            { term: "You shouldn't cast aspersions, dear.", translation: "Tu ne devrais pas jeter le discrédit, ma chère." },
            { term: "Such ghastly taste.", translation: "Un goût si affreux." },
            { term: "Nothing your putrefying touch couldn't fix.", translation: "Rien que ta touche putréfiante ne puisse réparer." },
            { term: "Pugsley shot up like poison oak.", translation: "Pugsley a grandi comme du sumac vénéneux." },
            { term: "A sense of dread I usually reserve for costumed mascots.", translation: "Un sentiment d'effroi que je réserve habituellement aux mascottes costumées." },
            { term: "You've never been very forthcoming about Aunt Ophelia.", translation: "Tu n'as jamais été très loquace au sujet de tante Ophélie." },
            { term: "Real sleaze.", translation: "Une vraie ordure." },
            { term: "Catching our supermarket sweethearts in a tryst.", translation: "Surprendre nos amoureux de supermarché en plein rendez-vous secret." },
            { term: "A squad of kamikaze crows.", translation: "Une escouade de corbeaux kamikazes." },
            { term: "Get the body to the morgue before he gets ripe.", translation: "Amenez le corps à la morgue avant qu'il ne commence à sentir." },
            { term: "I promise to be better than my predecessor.", translation: "Je promets d'être meilleur que mon prédécesseur." },
            { term: "That's an exceptionally low bar.", translation: "La barre est exceptionnellement basse." },
            { term: "A broken, deluded man.", translation: "Un homme brisé et plein d'illusions." },
            { term: "You were drawn to it like a moth to a flame.", translation: "Tu as été attirée par ça comme un papillon par la flamme." },
            { term: "Psychological insight.", translation: "Perspicacité psychologique." },
            { term: "His body could keep up with his dazzling mind.", translation: "Son corps pouvait suivre son esprit éblouissant." },
            { term: "Our most famous alumni would be proud.", translation: "Nos anciens élèves les plus célèbres seraient fiers." },
            { term: "You positively ooze it.", translation: "Tu en débordes positivement (charisme)." },
            { term: "Political aspirations.", translation: "Aspirations politiques." },
            { term: "The fundraising gala's student liaison.", translation: "La liaison étudiante pour le gala de collecte de fonds." },
            { term: "You have the sizzle factor I've been looking for.", translation: "Tu as le facteur 'pétillant' que je cherchais." },
            { term: "The school is in a financial pickle.", translation: "L'école est dans le pétrin financier." },
            { term: "Using your powers of persuasion for a good cause should be applauded.", translation: "Utiliser vos pouvoirs de persuasion pour une bonne cause devrait être applaudi." },
            { term: "Unrelenting quest for excellence.", translation: "Quête incessante de l'excellence." },
            { term: "Teaching was the purview for those who can't.", translation: "L'enseignement était le domaine de ceux qui ne peuvent pas faire." },
            { term: "In the face of overwhelming odds.", translation: "Face à des obstacles insurmontables." },
            { term: "We have our work cut out for us.", translation: "Nous avons du pain sur la planche." },
            { term: "Deceptively cunning.", translation: "Trompeusement rusé." },
            { term: "I play for my own fulfillment.", translation: "Je joue pour mon propre épanouissement." },
            { term: "You have to succumb to its beautiful chaos.", translation: "Tu dois succomber à son magnifique chaos." },
            { term: "It instills pride in our children.", translation: "Cela insuffle de la fierté à nos enfants." },
            { term: "That is a testament to you.", translation: "C'est un témoignage en votre honneur." },
            { term: "I'm still pondering.", translation: "Je suis encore en train de réfléchir." },
            { term: "Keep your fingers peeled.", translation: "Garde l'œil ouvert (avec les doigts)." },
            { term: "You kind of ghosted me this summer.", translation: "Tu m'as en quelque sorte 'ghosté' cet été." },
            { term: "Wolf Camp was strictly device-free.", translation: "Le Camp des Loups était strictement sans appareils électroniques." },
            { term: "I am abolishing the Nightshades.", translation: "J'abolis les Sombres Nuits (société secrète)." },
            { term: "The protection of our school falls upon all of us.", translation: "La protection de notre école nous incombe à tous." },
            { term: "I had this commissioned to commemorate.", translation: "J'ai fait faire ceci pour commémorer." },
            { term: "Your ragtag group of Nevermore buddies.", translation: "Ton groupe hétéroclite de copains de Nevermore." },
            { term: "Our banquet of discontent.", translation: "Notre banquet du mécontentement." },
            { term: "On those who would subdue us.", translation: "Sur ceux qui voudraient nous soumettre." },
            { term: "Our enemies have been vanquished!", translation: "Nos ennemis ont été vaincus !" },
            { term: "Any imbecile stupid enough to cheer on some shallow, rabble-rousing diatribe like that.", translation: "Tout imbécile assez stupide pour applaudir une diatribe creuse et populiste comme celle-là." },
            { term: "Do not put me on a pedestal.", translation: "Ne me mettez pas sur un piédestal." },
            { term: "So you're going to torpedo it for us?", translation: "Alors tu vas le saborder (le torpiller) pour nous ?" }
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