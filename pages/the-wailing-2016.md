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
            { term: "It's still dawn.", translation: "C'est encore l'aube." },
            { term: "Somebody died.", translation: "Quelqu'un est mort." },
            { term: "Who?", translation: "Qui ?" },
            { term: "Yeah.", translation: "Ouais." },
            { term: "It's pouring out there.", translation: "Il pleut des cordes dehors." },
            { term: "Where are you off to?", translation: "Où vas-tu ?" },
            { term: "That's all I know.", translation: "C'est tout ce que je sais." },
            { term: "She's so young.", translation: "Elle est si jeune." },
            { term: "I gotta hurry.", translation: "Je dois me dépêcher." },
            { term: "Just a bite!", translation: "Juste une bouchée !" },
            { term: "I need to go now.", translation: "Je dois y aller maintenant." },
            { term: "Come on.", translation: "Allez." },
            { term: "What happened?", translation: "Que s'est-il passé ?" },
            { term: "Not sure.", translation: "Pas sûr." },
            { term: "Bye, dad.", translation: "Au revoir, papa." },
            { term: "What took so long?!", translation: "Qu'est-ce qui a pris si longtemps ?!" },
            { term: "Hurry up!", translation: "Dépêche-toi !" },
            { term: "I'm coming.", translation: "J'arrive." },
            { term: "Let go of me!", translation: "Lâchez-moi !" },
            { term: "I'm his uncle!", translation: "Je suis son oncle !" },
            { term: "Holy shit.", translation: "Putain de merde." },
            { term: "What's that?", translation: "Qu'est-ce que c'est ?" },
            { term: "What a mess.", translation: "Quel bazar." },
            { term: "Looks that way.", translation: "On dirait bien." },
            { term: "I'll be damned.", translation: "Je n'en reviens pas." },
            { term: "And the kid?", translation: "Et le gamin ?" },
            { term: "Why'd he do it?", translation: "Pourquoi a-t-il fait ça ?" },
            { term: "What the hell happened?", translation: "Qu'est-ce qui s'est passé, bordel ?" },
            { term: "What's that stink?", translation: "C'est quoi cette puanteur ?" },
            { term: "Get over here!", translation: "Viens ici !" },
            { term: "Oh god...", translation: "Oh mon dieu..." },
            { term: "Scary, my ass.", translation: "Effrayant, mon cul." },
            { term: "Anyway...", translation: "Bref..." },
            { term: "Like what?", translation: "Comme quoi ?" },
            { term: "So?", translation: "Et alors ?" },
            { term: "What?", translation: "Quoi ?" },
            { term: "Hell no.", translation: "Sûrement pas." },
            { term: "What the hell?", translation: "C'est quoi ce bordel ?" },
            { term: "Fuck.", translation: "Merde." },
            { term: "What's wrong?", translation: "Qu'est-ce qui ne va pas ?" },
            { term: "What's your problem?", translation: "C'est quoi ton problème ?" },
            { term: "You check it out.", translation: "Va voir." },
            { term: "Okay, I'll go.", translation: "D'accord, j'y vais." },
            { term: "Who the fuck are you?", translation: "T'es qui putain ?" },
            { term: "Stop, goddamn it!", translation: "Arrête, bon sang !" },
            { term: "Damn, you're heavy.", translation: "Putain, t'es lourd." },
            { term: "Don't be ridiculous.", translation: "Ne sois pas ridicule." },
            { term: "No big deal.", translation: "Ce n'est pas grave." },
            { term: "It's okay.", translation: "C'est bon." },
            { term: "Son of a bitch!", translation: "Fils de pute !" },
            { term: "No, I wasn't. I swear.", translation: "Non, je ne l'étais pas. Je le jure." },
            { term: "Get to work.", translation: "Au travail." },
            { term: "Excuse me, ma'am?", translation: "Excusez-moi, madame ?" },
            { term: "Yes, sir.", translation: "Oui, monsieur." },
            { term: "Get off me!", translation: "Lâchez-moi !" },
            { term: "Back off, people.", translation: "Reculez, les gens." },
            { term: "Please, calm down.", translation: "S'il vous plaît, calmez-vous." },
            { term: "It's not your fault.", translation: "Ce n'est pas ta faute." },
            { term: "Hello!", translation: "Bonjour !" },
            { term: "How are you?", translation: "Comment ça va ?" },
            { term: "Good.", translation: "Bien." },
            { term: "Yeah.", translation: "Ouais." },
            { term: "Thanks, darling.", translation: "Merci, chérie." },
            { term: "Take a shower.", translation: "Prends une douche." },
            { term: "I will. Go home.", translation: "Je le ferai. Rentre chez toi." },
            { term: "See you.", translation: "À plus." },
            { term: "Bye.", translation: "Au revoir." },
            { term: "Beats me.", translation: "Aucune idée." },
            { term: "Over here.", translation: "Par ici." },
            { term: "In here.", translation: "Ici." },
            { term: "Give us a hand.", translation: "Donne-nous un coup de main." },
            { term: "Found it.", translation: "Trouvé." },
            { term: "That's evidence.", translation: "C'est une preuve." },
            { term: "I was just messing with you.", translation: "Je te taquinais juste." },
            { term: "Gimme a break.", translation: "Laisse-moi tranquille." },
            { term: "What are you looking at?", translation: "Qu'est-ce que tu regardes ?" },
            { term: "Get outta here!", translation: "Fous le camp !" },
            { term: "What a nut job.", translation: "Quelle folle." },
            { term: "Come on, hurry.", translation: "Allez, dépêche-toi." },
            { term: "Are you from this village?", translation: "Êtes-vous de ce village ?" },
            { term: "Don't come near me!", translation: "Ne m'approche pas !" },
            { term: "Dammit!", translation: "Bon sang !" },
            { term: "No, I'm not.", translation: "Non, je ne le suis pas." },
            { term: "Then who are you?", translation: "Alors qui êtes-vous ?" },
            { term: "Come with me.", translation: "Viens avec moi." },
            { term: "You saw with your own eyes?", translation: "Tu as vu de tes propres yeux ?" },
            { term: "Of course I did.", translation: "Bien sûr que je l'ai fait." },
            { term: "How?", translation: "Comment ?" },
            { term: "Be careful.", translation: "Fais attention." },
            { term: "Why?", translation: "Pourquoi ?" },
            { term: "Wait here for a second.", translation: "Attends ici une seconde." },
            { term: "Okay.", translation: "D'accord." },
            { term: "Where'd she go?", translation: "Où est-elle allée ?" },
            { term: "Are you feeling sick?", translation: "Tu te sens malade ?" },
            { term: "Yes, sir.", translation: "Oui, monsieur." },
            { term: "Shut your hole.", translation: "Ferme ta gueule." },
            { term: "You got proof?", translation: "Tu as des preuves ?" },
            { term: "Hell yes, proof!", translation: "Putain ouais, des preuves !" },
            { term: "That's not the point.", translation: "Là n'est pas la question." },
            { term: "I'm telling you.", translation: "Je te le dis." },
            { term: "Anyway,", translation: "Bref," },
            { term: "Where does the guy live?", translation: "Où habite ce type ?" },
            { term: "I think I should.", translation: "Je pense que je devrais." },
            { term: "Don't. You'll regret it.", translation: "Ne le fais pas. Tu le regretteras." },
            { term: "He's not human.", translation: "Il n'est pas humain." },
            { term: "Give me his address.", translation: "Donne-moi son adresse." },
            { term: "You can't miss it.", translation: "Tu ne peux pas le rater." },
            { term: "Don't touch me, I gotta go!", translation: "Ne me touche pas, je dois y aller !" },
            { term: "I said let go!", translation: "J'ai dit lâche-moi !" },
            { term: "We're sorry.", translation: "Nous sommes désolés." },
            { term: "Fuck off.", translation: "Va te faire foutre." },
            { term: "Fuck you.", translation: "Va te faire foutre." },
            { term: "What's the point of taking...", translation: "À quoi bon prendre..." },
            { term: "How can something like this happen?", translation: "Comment une chose pareille peut-elle arriver ?" },
            { term: "What are the chances?", translation: "Quelles sont les chances ?" },
            { term: "What's going on?", translation: "Que se passe-t-il ?" },
            { term: "Let's go back.", translation: "Retournons-y." },
            { term: "Daddy's right here.", translation: "Papa est là." },
            { term: "Look at me.", translation: "Regarde-moi." },
            { term: "It's okay.", translation: "Ce n'est rien." },
            { term: "You're all right?", translation: "Tu vas bien ?" },
            { term: "Morning, Dad.", translation: "Bonjour, papa." },
            { term: "Something's wrong with her.", translation: "Quelque chose ne va pas avec elle." },
            { term: "Got that?", translation: "Compris ?" },
            { term: "Good.", translation: "Bien." },
            { term: "Say hello.", translation: "Dis bonjour." },
            { term: "What, a priest?", translation: "Quoi, un prêtre ?" },
            { term: "You speak Japanese?", translation: "Tu parles japonais ?" },
            { term: "Just a little.", translation: "Juste un peu." },
            { term: "Let's go.", translation: "Allons-y." },
            { term: "Get in.", translation: "Monte." },
            { term: "Anybody home?", translation: "Y'a quelqu'un ?" },
            { term: "What is it?", translation: "Qu'est-ce que c'est ?" },
            { term: "I know.", translation: "Je sais." },
            { term: "What are you staring at?", translation: "Qu'est-ce que tu regardes fixement ?" },
            { term: "Get over here!", translation: "Viens ici !" },
            { term: "Shit!", translation: "Merde !" },
            { term: "Where is it?", translation: "Où est-ce ?" },
            { term: "Over there.", translation: "Là-bas." },
            { term: "Are you all right?", translation: "Tu vas bien ?" },
            { term: "Yeah.", translation: "Ouais." },
            { term: "Did you get bit?", translation: "Tu t'es fait mordre ?" },
            { term: "Say something, dammit.", translation: "Dis quelque chose, bon sang." },
            { term: "He's the criminal.", translation: "C'est lui le criminel." },
            { term: "I'm sure of it.", translation: "J'en suis sûr." },
            { term: "What did you see?", translation: "Qu'as-tu vu ?" },
            { term: "Tell me!", translation: "Dis-moi !" },
            { term: "You're getting all wet!", translation: "Tu es tout trempé !" },
            { term: "Answer me.", translation: "Réponds-moi." },
            { term: "What the hell...", translation: "Mais putain..." },
            { term: "Stop it!", translation: "Arrête ça !" },
            { term: "I'm gonna kill all of you.", translation: "Je vais tous vous tuer." },
            { term: "How much?", translation: "Combien ?" },
            { term: "I'll have it ready.", translation: "Ce sera prêt." },
            { term: "You can go.", translation: "Tu peux partir." },
            { term: "Can I ask you something?", translation: "Je peux te demander quelque chose ?" },
            { term: "No.", translation: "Non." },
            { term: "Yes.", translation: "Oui." },
            { term: "Stop!", translation: "Arrête !" },
            { term: "Get out.", translation: "Sors." },
            { term: "Now!", translation: "Maintenant !" },
            { term: "Sorry?", translation: "Pardon ?" },
            { term: "Go!", translation: "Vas-y !" },
            { term: "Is everyone here?", translation: "Tout le monde est là ?" },
            { term: "This ain't a joke?", translation: "Ce n'est pas une blague ?" },
            { term: "Back off.", translation: "Recule." },
            { term: "He's not dead.", translation: "Il n'est pas mort." },
            { term: "It was no dream.", translation: "Ce n'était pas un rêve." },
            { term: "Let me ask you one thing.", translation: "Laisse-moi te demander une chose." },
            { term: "That's not true.", translation: "Ce n'est pas vrai." },
            { term: "Look at me.", translation: "Regarde-moi." },
            { term: "Please.", translation: "S'il te plaît." },
            { term: "Oh Lord...", translation: "Oh Seigneur..." },
            { term: "It's okay.", translation: "Ça va." },
            { term: "My baby.", translation: "Mon bébé." }
          ],
          hard: [
            { term: "A ghost does not have flesh and bones.", translation: "Un fantôme n'a ni chair ni os." },
            { term: "Crime of passion.", translation: "Crime passionnel." },
            { term: "He's high on something.", translation: "Il est défoncé à quelque chose." },
            { term: "He's spreading those stories.", translation: "Il répand ces histoires." },
            { term: "There's definitely something off with that guy.", translation: "Il y a vraiment quelque chose qui cloche avec ce type." },
            { term: "All this happened after that Japanese man arrived.", translation: "Tout ça est arrivé après l'arrivée de ce Japonais." },
            { term: "Quit talking out of your ass.", translation: "Arrête de raconter des conneries." },
            { term: "The ones with the drugs that make you go crazy.", translation: "Ceux avec les drogues qui te rendent fou." },
            { term: "Mushrooms would never make you that way.", translation: "Les champignons ne te mettraient jamais dans cet état." },
            { term: "For crying out loud...", translation: "Bon sang de bonsoir..." },
            { term: "You dirty slut! You whore!", translation: "Sale traînée ! Salope !" },
            { term: "Men can still get it up after 70.", translation: "Les hommes peuvent encore avoir une érection après 70 ans." },
            { term: "I keep having fucked-up nightmares.", translation: "Je n'arrête pas de faire des cauchemars de merde." },
            { term: "Prime suspect is the missus who hung herself.", translation: "Le suspect principal est la bonne femme qui s'est pendue." },
            { term: "The Jap raped a woman.", translation: "Le Japonais a violé une femme." },
            { term: "She was totally covered in rashes and boils.", translation: "Elle était totalement couverte d'éruptions cutanées et de furoncles." },
            { term: "Mumbling gibberish the whole time.", translation: "Marmonnant du charabia tout le temps." },
            { term: "You worthless bastard.", translation: "Espèce de bâtard inutile." },
            { term: "The rash is the link here.", translation: "L'éruption cutanée est le lien ici." },
            { term: "You could hurt somebody!", translation: "Tu pourrais blesser quelqu'un !" },
            { term: "Her head was smashed open like a watermelon.", translation: "Sa tête a été éclatée comme une pastèque." },
            { term: "The Jap is a ghost.", translation: "Le Japonais est un fantôme." },
            { term: "He was gonna suck her blood dry.", translation: "Il allait la saigner à blanc." },
      { term: "He's stalking you.", translation: "Il te traque." },
      { term: "What terrible sin did you commit?", translation: "Quel terrible péché as-tu commis ?" },
      { term: "Keep having weird dreams.", translation: "Je continue de faire des rêves bizarres." },
      { term: "Driving me up the wall.", translation: "Ça me rend fou." },
      { term: "You think this case is going to be your big break?", translation: "Tu penses que cette affaire va être ta grande chance ?" },
      { term: "Stark naked except for a diaper.", translation: "Totalement nu à l'exception d'une couche." },
      { term: "Bladder control problems.", translation: "Problèmes de contrôle de la vessie." },
      { term: "He had his face buried in the carcass.", translation: "Il avait le visage enfoui dans la carcasse." },
      { term: "His eyes all bloodshot.", translation: "Ses yeux tout injectés de sang." },
      { term: "You saw him eating raw flesh?", translation: "Tu l'as vu manger de la chair crue ?" },
      { term: "Bastard even bit me.", translation: "Le salaud m'a même mordu." },
      { term: "It's out in the middle of nowhere.", translation: "C'est au milieu de nulle part." },
      { term: "I'm gonna get your asses sacked.", translation: "Je vais vous faire virer." },
      { term: "You deserve to get scorched by lightning.", translation: "Tu mérites de te faire foudroyer." },
      { term: "He survived.", translation: "Il a survécu." },
      { term: "Can anybody make sense of what's happening here?", translation: "Quelqu'un peut-il comprendre ce qui se passe ici ?" },
      { term: "Someone keeps banging on the door.", translation: "Quelqu'un n'arrête pas de frapper à la porte." },
      { term: "Stop grilling me, goddamn it!", translation: "Arrête de me cuisiner, bon sang !" },
      { term: "Yanking up your daughter's skirt.", translation: "Remontant la jupe de ta fille." },
      { term: "Tell me, you fucking shithead!", translation: "Dis-moi, espèce de connard de merde !" },
      { term: "Something's possessed Hyo-jin.", translation: "Quelque chose a possédé Hyo-jin." },
      { term: "If we don't act, there'll be dead bodies.", translation: "Si nous n'agissons pas, il y aura des cadavres." },
      { term: "He's supposed to be the best.", translation: "Il est censé être le meilleur." },
      { term: "Ask him where he stashed the stuff.", translation: "Demande-lui où il a planqué les trucs." },
      { term: "You loose-assed, dog-fucking son of a whore!", translation: "Toi, fils de pute au cul lâche qui baise des chiens !" },
      { term: "I'll give you three days.", translation: "Je te donne trois jours." },
      { term: "Get out, or you'll end up like your goddamned dog.", translation: "Dégage, ou tu finiras comme ton putain de chien." },
      { term: "There's gotta be an explanation for what she has.", translation: "Il doit y avoir une explication à ce qu'elle a." },
      { term: "My body's been in pain.", translation: "Mon corps me fait mal." },
      { term: "A man's face comes out of the wall.", translation: "Le visage d'un homme sort du mur." },
      { term: "It's a real wicked spirit we've got here.", translation: "C'est un véritable esprit maléfique que nous avons ici." },
      { term: "Of all the evil I've seen, this is the strongest.", translation: "De tout le mal que j'ai vu, c'est le plus puissant." },
      { term: "You disturbed it.", translation: "Tu l'as dérangé." },
      { term: "That's no man. That's a ghost.", translation: "Ce n'est pas un homme. C'est un fantôme." },
      { term: "Everything that walks on two feet will perish.", translation: "Tout ce qui marche sur deux pattes périra." },
      { term: "Either banish it, or kill it.", translation: "Soit le bannir, soit le tuer." },
      { term: "I'm casting a deadly hex tomorrow.", translation: "Je lance un sort mortel demain." },
      { term: "It's incredibly dangerous.", translation: "C'est incroyablement dangereux." },
      { term: "Or the spell will backfire.", translation: "Ou le sort se retournera contre toi." },
      { term: "He died a long time ago.", translation: "Il est mort il y a longtemps." },
      { term: "That demon will destroy this village.", translation: "Ce démon détruira ce village." },
      { term: "Even among other demons, he's a master of evil.", translation: "Même parmi les autres démons, c'est un maître du mal." },
      { term: "He just threw out the bait, and your daughter took it.", translation: "Il a juste jeté l'appât, et ta fille l'a mordu." },
      { term: "Rumor has it he's a renowned professor.", translation: "La rumeur dit que c'est un professeur de renom." },
      { term: "And there are darker, more disturbing rumors.", translation: "Et il y a des rumeurs plus sombres, plus dérangeantes." },
      { term: "Cross your heart, or your mom's a whore?", translation: "Croix de bois, croix de fer, si je mens, ma mère est une pute ?" },
      { term: "The rat fell into the trap.", translation: "Le rat est tombé dans le piège." },
      { term: "Severe mental derangement.", translation: "Grave dérangement mental." },
      { term: "I misread the divination.", translation: "J'ai mal interprété la divination." },
      { term: "I cast the hex on the wrong ghost.", translation: "J'ai jeté le sort sur le mauvais fantôme." },
      { term: "I made a grave mistake.", translation: "J'ai fait une grave erreur." },
      { term: "He was trying to kill that woman.", translation: "Il essayait de tuer cette femme." },
      { term: "Be our safeguard against the wickedness and snares of the devil.", translation: "Sois notre protection contre la méchanceté et les pièges du diable." },
      { term: "She's possessed by an evil spirit.", translation: "Elle est possédée par un esprit malin." },
      { term: "I laid a trap for it.", translation: "Je lui ai tendu un piège." },
      { term: "Death cannot touch him.", translation: "La mort ne peut pas le toucher." },
      { term: "The demon will soon enter your home.", translation: "Le démon entrera bientôt dans ta maison." },
      { term: "When the demon is snared, the rooster will cry three times.", translation: "Quand le démon sera piégé, le coq chantera trois fois." },
      { term: "Do not waver.", translation: "Ne fléchis pas." },
      { term: "You came here to confirm your suspicions about me.", translation: "Tu es venu ici pour confirmer tes soupçons à mon sujet." },
      { term: "Why in God's name is he doing this?", translation: "Pourquoi diable fait-il ça ?" },
      { term: "Her father suspected another.", translation: "Son père en a suspecté un autre." },
      { term: "Why is there doubt in your heart?", translation: "Pourquoi y a-t-il du doute dans ton cœur ?" }
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
