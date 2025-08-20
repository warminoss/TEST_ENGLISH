---
title: Twimbleweed Park
layout: default
---

<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Twimbleweed Park</title>
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
        <h1>Twimbleweed Park</h1>
        
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
            { term: "howdy", translation: "salut (informel)" },
            { term: "halfway around the world", translation: "à l'autre bout du monde" },
            { term: "odd note", translation: "note étrange" },
            { term: "down by the river", translation: "près de la rivière" },
            { term: "long walk", translation: "longue marche" },
            { term: "go for some", translation: "avoir envie de" },
            { term: "stay out of my way", translation: "reste en dehors de mon chemin" },
            { term: "take a lot of notes", translation: "prendre beaucoup de notes" },
            { term: "sit back and learn", translation: "assieds-toi et apprends" },
            { term: "get the hell out of here", translation: "foutre le camp d'ici" },
            { term: "head into town", translation: "aller en ville" },
            { term: "approximately", translation: "approximativement" },
            { term: "in the water", translation: "dans l'eau" },
            { term: "let's see here", translation: "voyons voir" },
            { term: "key card", translation: "carte-clé" },
            { term: "very strong", translation: "très fort" },
            { term: "you already said that", translation: "tu as déjà dit ça" },
            { term: "sorry about that", translation: "désolé pour ça" },
            { term: "dead body", translation: "cadavre" },
            { term: "shut up", translation: "tais-toi" },
            { term: "nice chatting with you", translation: "ravi d'avoir discuté avec vous" },
            { term: "solve a murder", translation: "résoudre un meurtre" },
            { term: "wait up", translation: "attends" },
            { term: "hard to miss", translation: "difficile à manquer" },
            { term: "correct sir", translation: "c'est exact, monsieur" },
            { term: "hold your horses", translation: "minute papillon" },
            { term: "no need to", translation: "pas la peine de" },
            { term: "ignore him", translation: "ignore-le" },
            { term: "he's new", translation: "il est nouveau" },
            { term: "wasting time", translation: "perdre du temps" },
            { term: "makes no sense", translation: "n'a aucun sens" },
            { term: "shoot people", translation: "tirer sur les gens" },
            { term: "welcome to the future", translation: "bienvenue dans le futur" },
            { term: "take notes", translation: "prendre des notes" },
            { term: "already on it", translation: "je m'en occupe déjà" },
            { term: "tell us about", translation: "parle-nous de" },
            { term: "crack me up", translation: "tu me fais marrer" },
            { term: "around the eyes", translation: "autour des yeux" },
            { term: "sure, whatever", translation: "oui, peu importe" },
            { term: "heard enough", translation: "assez entendu" },
            { term: "very impressive", translation: "très impressionnant" },
            { term: "get out of here", translation: "sortir d'ici" },
            { term: "it's empty", translation: "c'est vide" },
            { term: "split up", translation: "se séparer" },
            { term: "it will be faster", translation: "ce sera plus rapide" },
            { term: "as fast as possible", translation: "aussi vite que possible" },
            { term: "no time to talk", translation: "pas le temps de parler" },
            { term: "anything to help", translation: "quelque chose pour aider" },
            { term: "pleased to meet you", translation: "enchanté de vous rencontrer" },
            { term: "what can I do for you", translation: "que puis-je faire pour vous" },
            { term: "check the mail", translation: "vérifier le courrier" },
            { term: "out of order", translation: "en panne" },
            { term: "leave a message", translation: "laisser un message" },
            { term: "get back in touch", translation: "reprendre contact" },
            { term: "send me a text", translation: "envoie-moi un texto" },
            { term: "give me just a sec", translation: "donne-moi juste une seconde" },
            { term: "leave me alone", translation: "laisse-moi tranquille" },
            { term: "I don't really care", translation: "je m'en fiche un peu" },
            { term: "I'm not home right now", translation: "je ne suis pas à la maison en ce moment" },
            { term: "I swear to you", translation: "je te le jure" },
            { term: "forgive me", translation: "pardonne-moi" },
            { term: "don't call us, we'll call you", translation: "ne nous appelez pas, on vous appellera" },
            { term: "I'm currently unavailable", translation: "je suis actuellement indisponible" },
            { term: "as soon as I can", translation: "dès que je peux" },
            { term: "I'm not available at the moment", translation: "je ne suis pas disponible pour le moment" },
            { term: "you just missed me", translation: "vous venez de me manquer" },
            { term: "call you back", translation: "te rappeler" },
            { term: "what brings you all the way out here", translation: "qu'est-ce qui vous amène si loin" },
            { term: "dream come true", translation: "un rêve devenu réalité" },
            { term: "I'm going to apply", translation: "je vais postuler" },
            { term: "good to know", translation: "bon à savoir" },
            { term: "good luck", translation: "bonne chance" },
            { term: "of course", translation: "bien sûr" },
            { term: "thank you", translation: "merci" },
            { term: "be with you in a second", translation: "je suis à vous dans une seconde" },
            { term: "I'm pretty swamped right now", translation: "je suis assez débordé en ce moment" },
            { term: "good one", translation: "bien envoyé" },
            { term: "thanks for your help", translation: "merci pour votre aide" },
            { term: "don't leave town", translation: "ne quittez pas la ville" },
            { term: "what's up with", translation: "qu'est-ce qui se passe avec" },
            { term: "he's kind of a nut job", translation: "c'est un peu un cinglé" },
            { term: "that's about it", translation: "c'est à peu près tout" },
            { term: "not too much", translation: "pas grand-chose" },
            { term: "I don't want to get into trouble", translation: "je ne veux pas avoir de problèmes" },
            { term: "trust us", translation: "fais-nous confiance" },
            { term: "what else do you know", translation: "que sais-tu d'autre" },
            { term: "good choice", translation: "bon choix" },
            { term: "eat up", translation: "mange" },
            { term: "I feel better now", translation: "je me sens mieux maintenant" },
            { term: "I got to go", translation: "je dois y aller" },
            { term: "can I help you find anything", translation: "puis-je vous aider à trouver quelque chose" },
            { term: "heck yeah", translation: "oh que oui" },
            { term: "no offense", translation: "sans vouloir vous offenser" },
            { term: "good guess", translation: "bien deviné" },
            { term: "nice chatting", translation: "discussion sympa" },
            { term: "not much", translation: "pas grand-chose" },
            { term: "go on", translation: "continue" },
            { term: "you don't say", translation: "sans blague" },
            { term: "sounds legit", translation: "ça a l'air crédible" },
            { term: "no arguing", translation: "pas de discussion" },
            { term: "that doesn't seem right", translation: "ça ne semble pas correct" },
            { term: "take a chill pill", translation: "détends-toi" },
            { term: "what brings you back", translation: "qu'est-ce qui te ramène" },
            { term: "long hours, low pay", translation: "longues heures, bas salaire" },
            { term: "boss is a jerk", translation: "le patron est un con" },
            { term: "it's good to have you back", translation: "c'est bon de te revoir" },
            { term: "please leave me alone", translation: "s'il te plaît, laisse-moi tranquille" },
            { term: "not yet", translation: "pas encore" },
            { term: "I don't trust that agent", translation: "je ne fais pas confiance à cet agent" },
            { term: "I hear you", translation: "je te comprends" },
            { term: "get lost", translation: "dégage" },
            { term: "knock yourself out", translation: "fais-toi plaisir" },
            { term: "I hope you choke on it", translation: "j'espère que tu t'étoufferas avec" },
            { term: "what a waste of time", translation: "quelle perte de temps" },
            { term: "thanks a lot", translation: "merci beaucoup" },
            { term: "wait a second", translation: "attends une seconde" },
            { term: "don't play dumb with me", translation: "ne fais pas l'idiot avec moi" },
            { term: "big deal", translation: "la belle affaire" },
            { term: "good idea", translation: "bonne idée" },
            { term: "hello there", translation: "salut" },
            { term: "not bad", translation: "pas mal" },
            { term: "here you go", translation: "voilà" },
            { term: "I'm on it", translation: "je m'en occupe" },
            { term: "almost done", translation: "presque fini" },
            { term: "I blew it", translation: "j'ai tout gâché" },
            { term: "let's do this", translation: "allons-y" },
            { term: "I love you", translation: "je t'aime" },
            { term: "good point", translation: "bien vu" },
            { term: "I have some work to do", translation: "J'ai du travail à faire" },
            { term: "I'll be back", translation: "Je reviendrai" },
            { term: "What's the matter?", translation: "Quel est le problème ?" },
            { term: "Get out of here", translation: "Sors d'ici" },
            { term: "I don't know", translation: "Je ne sais pas" },
            { term: "Be careful", translation: "Fais attention" },
            { term: "What are you doing here?", translation: "Qu'est-ce que tu fais ici ?" },
            { term: "I don't care", translation: "Ça m'est égal" },
            { term: "I can't believe it", translation: "Je n'arrive pas à y croire" },
            { term: "It's not possible", translation: "Ce n'est pas possible" },
            { term: "You're crazy", translation: "Tu es fou" },
            { term: "Let's go", translation: "Allons-y" },
            { term: "Hurry up", translation: "Dépêche-toi" },
            { term: "No way", translation: "Pas question" },
            { term: "That's enough", translation: "Ça suffit" },
            { term: "It's all over", translation: "Tout est fini" },
            { term: "You're welcome", translation: "De rien" },
            { term: "Excuse me", translation: "Excusez-moi" },
            { term: "Come on", translation: "Allez" },
            { term: "Of course not", translation: "Bien sûr que non" },
            { term: "That's right", translation: "C'est exact" },
            { term: "Take it easy", translation: "Vas-y doucement" },
            { term: "What's new?", translation: "Quoi de neuf ?" },
            { term: "See you later", translation: "À plus tard" },
            { term: "Never mind", translation: "Laisse tomber" },
            { term: "I'm sorry", translation: "Je suis désolé" },
            { term: "I agree", translation: "Je suis d'accord" },
            { term: "I disagree", translation: "Je ne suis pas d'accord" },
            { term: "Good night", translation: "Bonne nuit" },
            { term: "Good morning", translation: "Bonjour" },
            { term: "What time is it?", translation: "Quelle heure est-il ?" },
            { term: "I'm lost", translation: "Je suis perdu" },
            { term: "I need help", translation: "J'ai besoin d'aide" },
            { term: "Just kidding", translation: "Je plaisante" },
            { term: "That's funny", translation: "C'est drôle" },
            { term: "It's okay", translation: "Ce n'est pas grave" },
            { term: "Don't worry", translation: "Ne t'inquiète pas" },
            { term: "Let me see", translation: "Laisse-moi voir" },
            { term: "I think so", translation: "Je pense que oui" },
            { term: "I don't think so", translation: "Je ne pense pas" },
            { term: "That's interesting", translation: "C'est intéressant" },
            { term: "You're right", translation: "Tu as raison" },
            { term: "You're wrong", translation: "Tu as tort" },
            { term: "It depends", translation: "Ça dépend" },
            { term: "For example", translation: "Par exemple" },
            { term: "By the way", translation: "Au fait" },
            { term: "In fact", translation: "En fait" },
            { term: "At least", translation: "Au moins" },
            { term: "Anyway", translation: "Bref" },
            { term: "Looks like...", translation: "On dirait que..." },
            { term: "I'm looking for...", translation: "Je cherche..." },
            { term: "Can you help me?", translation: "Pouvez-vous m'aider ?" },
            { term: "I'm not sure", translation: "Je ne suis pas sûr" },
            { term: "That's a good idea", translation: "C'est une bonne idée" },
            { term: "That's a bad idea", translation: "C'est une mauvaise idée" },
            { term: "I have no idea", translation: "Je n'en ai aucune idée" },
            { term: "What do you mean?", translation: "Qu'est-ce que tu veux dire ?" },
            { term: "What is this?", translation: "Qu'est-ce que c'est ?" },
            { term: "Where are we?", translation: "Où sommes-nous ?" },
            { term: "Who are you?", translation: "Qui êtes-vous ?" },
            { term: "Why not?", translation: "Pourquoi pas ?" },
            { term: "Are you sure?", translation: "Es-tu sûr ?" },
            { term: "Are you ready?", translation: "Es-tu prêt ?" },
            { term: "Let's begin", translation: "Commençons" },
            { term: "It's my fault", translation: "C'est de ma faute" },
            { term: "It's not my fault", translation: "Ce n'est pas de ma faute" },
            { term: "I promise", translation: "Je promets" },
            { term: "Congratulations", translation: "Félicitations" },
            { term: "Watch out!", translation: "Attention !" },
            { term: "Tell me more", translation: "Dis-m'en plus" },
            { term: "That's weird", translation: "C'est bizarre" },
            { term: "Not really", translation: "Pas vraiment" },
            { term: "So what?", translation: "Et alors ?" },
            { term: "Big mistake", translation: "Grosse erreur" },
            { term: "It's up to you", translation: "C'est à toi de voir" },
            { term: "I'll take it", translation: "Je le prends" },
            { term: "That's all", translation: "C'est tout" },
            { term: "This way", translation: "Par ici" },
            { term: "Follow me", translation: "Suivez-moi" },
            { term: "I'm tired", translation: "Je suis fatigué" },
            { term: "I'm hungry", translation: "J'ai faim" },
            { term: "I'm thirsty", translation: "J'ai soif" },
            { term: "What's wrong?", translation: "Qu'est-ce qui ne va pas ?" },
            { term: "What happened?", translation: "Que s'est-il passé ?" },
            { term: "I don't understand", translation: "Je ne comprends pas" },
            { term: "One moment", translation: "Un instant" },
            { term: "Here I am", translation: "Me voici" },
            { term: "There you are", translation: "Te voilà" },
            { term: "Like I said", translation: "Comme j'ai dit" },
            { term: "I give up", translation: "J'abandonne" },
            { term: "I don't remember", translation: "Je ne me souviens pas" },
            { term: "You owe five bucks", translation: "Tu dois cinq dollars" },
            { term: "I'll get you", translation: "Je t'aurai" },
            { term: "Step right up", translation: "Approchez" },
            { term: "You got my money?", translation: "Tu as mon argent ?" },
            { term: "Serves you right", translation: "Bien fait pour toi" },
            { term: "I'm out of here", translation: "Je me casse" },
            { term: "It won't take long", translation: "Ça ne prendra pas longtemps" },
            { term: "This is taking forever", translation: "Ça prend une éternité" },
            { term: "Let me know", translation: "Fais-moi savoir" },
            { term: "I need to talk to my sister", translation: "Je dois parler à ma sœur" },
            { term: "Keep your panties on", translation: "Calme-toi" },
            { term: "I'm coming", translation: "J'arrive" },
            { term: "You better start talking", translation: "Tu ferais mieux de parler" },
            { term: "I don't need another one", translation: "Je n'en ai pas besoin d'un autre" },
            { term: "What the hell is wrong with you?", translation: "Putain, qu'est-ce qui ne va pas chez toi ?" },
            { term: "Come back here", translation: "Reviens ici" },
            { term: "Stop", translation: "Arrête" },
            { term: "Not like I'm going anywhere", translation: "Ce n'est pas comme si j'allais quelque part" },
            { term: "Do me a favor", translation: "Rends-moi un service" },
            { term: "Next time", translation: "La prochaine fois" },
            { term: "I'm not going to...", translation: "Je ne vais pas..." },
            { term: "I don't talk about that", translation: "Je ne parle pas de ça" },
            { term: "Really?", translation: "Vraiment ?" },
            { term: "That's too easy", translation: "C'est trop facile" },
            { term: "I'm bored now", translation: "Je m'ennuie maintenant" },
            { term: "What gives?", translation: "Qu'est-ce qui se passe ?" },
            { term: "I hope you get Hepatitis", translation: "J'espère que tu attraperas l'hépatite" },
            { term: "He was a bitter and mean man", translation: "C'était un homme aigri et méchant" },
            { term: "I wouldn't go in there if I were you", translation: "Je n'irais pas là-dedans si j'étais toi" },
            { term: "My thoughts exactly", translation: "C'est exactement ce que je pense" },
            { term: "What are you doing here?", translation: "Qu'est-ce que tu fais ici ?" },
            { term: "Big day today", translation: "Grande journée aujourd'hui" },
            { term: "It can't fail", translation: "Ça ne peut pas échouer" },
            { term: "None of your business", translation: "Ce ne sont pas tes affaires" },
            { term: "I guess so", translation: "Je suppose" },
            { term: "Have a nice evening", translation: "Passe une bonne soirée" },
            { term: "I'll just push this button", translation: "Je vais juste appuyer sur ce bouton" },
            { term: "What's your damage?", translation: "C'est quoi ton problème ?" },
            { term: "You're too old to understand", translation: "Tu es trop vieux pour comprendre" },
            { term: "I can't trust a dweeb like you", translation: "Je ne peux pas faire confiance à un naze comme toi" },
            { term: "No faking", translation: "Sans blague" },
            { term: "Later, dude", translation: "À plus, mec" },
            { term: "I'd like to check in", translation: "Je voudrais m'enregistrer" },
            { term: "What's that monstrosity?", translation: "Qu'est-ce que c'est que cette monstruosité ?" },
            { term: "I guess it was nothing", translation: "Je suppose que ce n'était rien" },
            { term: "Are you happy now?", translation: "Es-tu content maintenant ?" },
            { term: "I've done everything you asked", translation: "J'ai fait tout ce que tu as demandé" },
            { term: "I already have a copy", translation: "J'ai déjà une copie" },
            { term: "Back to being plain old Franklin", translation: "Retour à la case départ pour le bon vieux Franklin" },
            { term: "That was quick", translation: "C'était rapide" },
            { term: "How did you know I was here?", translation: "Comment savais-tu que j'étais là ?" },
            { term: "I'm dead", translation: "Je suis mort" },
            { term: "That's what I deserve", translation: "C'est ce que je mérite" },
            { term: "About time you joined us", translation: "Il était temps que tu nous rejoignes" },
            { term: "Who are you?", translation: "Qui es-tu ?" },
            { term: "I'm in charge of the ghosts", translation: "Je suis responsable des fantômes" },
            { term: "What do you mean?", translation: "Qu'est-ce que tu veux dire ?" },
            { term: "Don't ask", translation: "Ne demande pas" },
            { term: "What are you waiting for?", translation: "Qu'est-ce que tu attends ?" },
            { term: "By the way", translation: "Au fait" },
            { term: "Chuck's dead", translation: "Chuck est mort" },
            { term: "When did that happen?", translation: "Quand est-ce que c'est arrivé ?" },
            { term: "I'm free", translation: "Je suis libre" },
            { term: "That tickles", translation: "Ça chatouille" },
            { term: "Get real", translation: "Sois réaliste" },
            { term: "That's totally not the greatest", translation: "Ce n'est absolument pas le top" },
            { term: "What's going on here?", translation: "Qu'est-ce qui se passe ici ?" },
            { term: "I better jet out of here", translation: "Je ferais mieux de filer d'ici" },
            { term: "It's just the drinking fountain", translation: "C'est juste la fontaine à eau" },
            { term: "I don't believe in ghosts", translation: "Je ne crois pas aux fantômes" },
            { term: "Sorry, that's not the reaction you wanted", translation: "Désolé, ce n'est pas la réaction que tu voulais" },
            { term: "Save me from myself", translation: "Sauve-moi de moi-même" },
            { term: "Time for another ghost meeting", translation: "C'est l'heure d'une autre réunion de fantômes" },
            { term: "Good work on the door", translation: "Bon travail à la porte" },
            { term: "That should do it for today", translation: "Ça devrait suffire pour aujourd'hui" },
            { term: "Keep practicing", translation: "Continue de t'entraîner" },
            { term: "I need some privacy", translation: "J'ai besoin d'intimité" },
            { term: "This is outrageously unfair", translation: "C'est scandaleusement injuste" },
            { term: "Enough complaining", translation: "Assez de plaintes" },
            { term: "All right, all right", translation: "D'accord, d'accord" },
            { term: "Sorry about him", translation: "Désolé pour lui" },
            { term: "We don't know who put him in charge", translation: "On ne sait pas qui l'a mis aux commandes" },
            { term: "I wonder what the guest is up to now", translation: "Je me demande ce que le client fait maintenant" },
            { term: "Where the hell have you been?", translation: "Où diable étais-tu ?" },
            { term: "Let's get on with it", translation: "Allons-y" },
            { term: "I want to know what I got", translation: "Je veux savoir ce que j'ai eu" },
            { term: "Wait, I thought you said...", translation: "Attends, je croyais que tu avais dit..." },
            { term: "Has anyone tried calling him?", translation: "Quelqu'un a essayé de l'appeler ?" },
            { term: "You're useless", translation: "Tu es inutile" },
            { term: "This is all your fault", translation: "Tout est de ta faute" },
            { term: "We got off on the wrong foot", translation: "On a mal commencé" },
            { term: "Let's try again", translation: "Essayons encore" },
            { term: "Save yourself the disappointment", translation: "Épargne-toi la déception" },
            { term: "Would it kill you to help out a little?", translation: "Ça te tuerait d'aider un peu ?" },
            { term: "Lift a finger", translation: "Lever le petit doigt" },
            { term: "Gag me", translation: "Ça me dégoûte" },
            { term: "I won't shed one tear for you", translation: "Je ne verserai pas une larme pour toi" },
            { term: "Can you flipping blame him?", translation: "Peux-tu vraiment le blâmer ?" },
            { term: "Is my career really that shameful?", translation: "Ma carrière est-elle vraiment si honteuse ?" },
            { term: "Oh hell yes", translation: "Oh que oui" },
            { term: "We just tell people you went to rehab", translation: "On dit juste aux gens que tu es allée en cure de désintoxication" },
            { term: "It's better for the family name", translation: "C'est mieux pour le nom de la famille" },
            { term: "You tell people I'm a drug addict?", translation: "Tu dis aux gens que je suis toxicomane ?" },
            { term: "For the last time", translation: "Pour la dernière fois" },
            { term: "You know what, I don't care", translation: "Tu sais quoi, je m'en fiche" },
            { term: "Don't talk about dad like that", translation: "Ne parle pas de papa comme ça" },
            { term: "You're so cruel", translation: "Tu es si cruelle" },
            { term: "How is Chuck Junior doing?", translation: "Comment va Chuck Junior ?" },
            { term: "He's thriving", translation: "Il s'épanouit" },
            { term: "He's a brat", translation: "C'est un sale gosse" },
            { term: "I think we're done here", translation: "Je pense qu'on a fini ici" },
            { term: "Don't let the door hit you on the way out", translation: "Ne te prends pas la porte en sortant" },
            { term: "It feels lonely without Uncle Chuck around", translation: "On se sent seul sans l'oncle Chuck" },
            { term: "I'm personally really fond of the chocolate bonbons", translation: "Personnellement, j'adore les bonbons au chocolat" },
            { term: "I'm out of the pie making biz", translation: "J'ai arrêté de faire des tartes" },
            { term: "Strictly tubes now", translation: "Uniquement des tubes maintenant" },
            { term: "I have a problem then", translation: "Alors j'ai un problème" },
            { term: "In order to hear my uncle's will", translation: "Pour entendre le testament de mon oncle" },
            { term: "In honor of your uncle Chuck", translation: "En l'honneur de ton oncle Chuck" },
            { term: "I'd make an exception", translation: "Je ferais une exception" },
            { term: "It's sad, isn't it?", translation: "C'est triste, n'est-ce pas ?" },
            { term: "The last thimbleberries were spotted...", translation: "Les dernières baies de thimble ont été aperçues..." },
            { term: "Not the forest, I always hated it in there", translation: "Pas la forêt, j'ai toujours détesté cet endroit" },
            { term: "That's pretty spooky", translation: "C'est assez effrayant" },
            { term: "No one goes there unless they have to", translation: "Personne n'y va à moins d'y être obligé" },
            { term: "People have been lost in there for days", translation: "Des gens se sont perdus là-dedans pendant des jours" },
            { term: "Some never make it out alive", translation: "Certains n'en sortent jamais vivants" },
            { term: "It's true", translation: "C'est vrai" },
            { term: "I've heard those stories too", translation: "J'ai aussi entendu ces histoires" },
            { term: "There's the old bear problem", translation: "Il y a le vieux problème de l'ours" },
            { term: "First thing you'll need...", translation: "La première chose dont tu auras besoin..." },
            { term: "You know how those thorns can leave you breaking out in welts", translation: "Tu sais comment ces épines peuvent te laisser couvert de zébrures" },
            { term: "I just happen to have an old pair", translation: "Il se trouve que j'en ai une vieille paire" },
            { term: "I won't be a jiffy", translation: "Je n'en aurai pas pour longtemps" },
            { term: "Here's your thimbleberry pie", translation: "Voici ta tarte aux baies de thimble" },
            { term: "Exactly how Chuck liked it", translation: "Exactement comme Chuck l'aimait" },
            { term: "He just wasn't himself those last few years", translation: "Il n'était tout simplement pas lui-même ces dernières années" },
            { term: "His obsession with restarting the Pillow Factory", translation: "Son obsession de redémarrer l'usine d'oreillers" },
            { term: "Vanishing for days", translation: "Disparaître pendant des jours" },
            { term: "I'd like to think so", translation: "J'aime à le penser" },
            { term: "This hot dog is even worse than...", translation: "Ce hot-dog est encore pire que..." },
            { term: "Sorry to hear about your uncle Chuck", translation: "Désolé d'apprendre pour ton oncle Chuck" },
            { term: "He was a complex person", translation: "C'était une personne complexe" },
            { term: "I was just kidding", translation: "Je plaisantais" },
            { term: "I'm not sorry", translation: "Je ne suis pas désolé" },
            { term: "Have they tried to pin this murder on you yet?", translation: "Ont-ils déjà essayé de te mettre ce meurtre sur le dos ?" },
            { term: "What about agent Reyes?", translation: "Et l'agent Reyes ?" },
            { term: "There is something familiar about him", translation: "Il y a quelque chose de familier chez lui" },
            { term: "I just wanted to see if you're the biggest nerd I've ever met", translation: "Je voulais juste voir si tu es le plus grand nerd que j'aie jamais rencontré" },
            { term: "And proudly", translation: "Et fièrement" },
            { term: "Something is fishy", translation: "Quelque chose cloche" },
            { term: "What do you think of in-jokes and fourth-walling?", translation: "Que penses-tu des blagues d'initiés et du brisage du quatrième mur ?" },
            { term: "I'm asking as a professional comedian", translation: "Je demande en tant que comédien professionnel" },
            { term: "I don't think you're either of those things", translation: "Je ne pense pas que tu sois l'un ou l'autre" },
            { term: "Piss off, Dolores", translation: "Fous le camp, Dolores" },
            { term: "I'm tired of talking", translation: "Je suis fatigué de parler" },
            { term: "You're not welcome in here", translation: "Tu n'es pas le bienvenu ici" },
            { term: "You can't legally refuse me service because I'm a clown", translation: "Vous ne pouvez pas légalement me refuser le service parce que je suis un clown" },
            { term: "But I can refuse you service because you're a butthole clown", translation: "Mais je peux vous refuser le service parce que vous êtes un clown trou du cul" },
            { term: "Ratting to the feds on me, eh?", translation: "Tu me balances aux fédéraux, hein ?" },
            { term: "Just order your food and get lost", translation: "Commande juste ta nourriture et dégage" },
            { term: "I'll have one of those disgusting hot dogs", translation: "Je prendrai un de ces dégoûtants hot-dogs" },
            { term: "We're trying to move 'em", translation: "On essaie de les écouler" },
            { term: "This tastes like crap", translation: "Ça a un goût de merde" },
            { term: "I had to know", translation: "Il fallait que je sache" },
            { term: "All I ever wanted to do was please Uncle Chuck", translation: "Tout ce que j'ai toujours voulu, c'est faire plaisir à l'oncle Chuck" },
            { term: "I tried so hard but it was never enough", translation: "J'ai essayé si fort mais ce n'était jamais assez" },
            { term: "And I failed him", translation: "Et je l'ai déçu" },
            { term: "I can't do anything right", translation: "Je ne peux rien faire de bien" },
            { term: "Mom always said I wouldn't amount to much", translation: "Maman a toujours dit que je n'arriverais à rien" },
            { term: "I've got my eyes on you", translation: "Je t'ai à l'œil" },
            { term: "I'm innocent", translation: "Je suis innocent" },
            { term: "We got a confession", translation: "On a des aveux" },
            { term: "Don't try to deny your crimes now, you creep", translation: "N'essaie pas de nier tes crimes maintenant, espèce de salaud" },
            { term: "I didn't do it, I swear", translation: "Je ne l'ai pas fait, je le jure" },
            { term: "The only man I ever wanted to kill is Chuck Edmund", translation: "Le seul homme que j'aie jamais voulu tuer est Chuck Edmund" },
            { term: "He's already dead", translation: "Il est déjà mort" },
            { term: "So you admit you have murderous fantasies?", translation: "Alors tu admets que tu as des fantasmes meurtriers ?" },
            { term: "Seems pretty cut and dry to me", translation: "Ça me semble assez clair" },
            { term: "Why did you confess?", translation: "Pourquoi as-tu avoué ?" },
            { term: "I was scared and confused", translation: "J'étais effrayé et confus" },
            { term: "When you pulled that good cop, bad cop stuff on me", translation: "Quand vous m'avez fait le coup du bon flic, mauvais flic" },
            { term: "I'd have said anything to make it stop", translation: "J'aurais dit n'importe quoi pour que ça s'arrête" },
            { term: "I'm just a humble watch and violin repairman to the stars", translation: "Je ne suis qu'un humble réparateur de montres et de violons pour les stars" },
            { term: "I'm not used to your big city torture techniques", translation: "Je ne suis pas habitué à vos techniques de torture de grande ville" },
            { term: "I'm still not buying it, murder boy", translation: "Je n'y crois toujours pas, petit meurtrier" },
            { term: "The Arrest-o-Tron doesn't make mistakes", translation: "L'Arrest-o-Tron ne fait pas d'erreurs" },
            { term: "He might be able to manipulate these small-town hicks", translation: "Il pourrait peut-être manipuler ces péquenots de petite ville" },
            { term: "That crap won't work on me", translation: "Ces conneries ne marcheront pas sur moi" },
            { term: "I hope you rot, murder boy", translation: "J'espère que tu pourriras, petit meurtrier" },
            { term: "Got to go, murder boy", translation: "Je dois y aller, petit meurtrier" },
            { term: "You look like you're hitting the sauce harder than I am", translation: "On dirait que tu picolles plus que moi" },
            { term: "Well, you look like your liver's about to fail", translation: "Eh bien, on dirait que ton foie est sur le point de lâcher" },
            { term: "So I guess we both look like Willie", translation: "Alors je suppose qu'on ressemble tous les deux à Willie" },
            { term: "Looks like we all need to get in the factory", translation: "On dirait qu'on a tous besoin d'entrer dans l'usine" },
            { term: "Why don't you help us?", translation: "Pourquoi ne nous aides-tu pas ?" },
            { term: "I can always smudge a fingerprint", translation: "Je peux toujours salir une empreinte digitale" },
            { term: "And then it's you instead of Willie in the slammer", translation: "Et puis c'est toi au lieu de Willie en taule" },
            { term: "Glad you hung this on Willie and not me", translation: "Content que tu aies mis ça sur le dos de Willie et pas sur le mien" },
            { term: "Murder boy is going to swing for this", translation: "Le petit meurtrier va payer pour ça" },
            { term: "Better him than me", translation: "Mieux vaut lui que moi" },
            { term: "Either of you would have been fine by me, jackass", translation: "N'importe lequel d'entre vous m'aurait convenu, crétin" },
            { term: "Hey, that rhymes", translation: "Hé, ça rime" },
            { term: "My father's old pocket watch", translation: "La vieille montre de poche de mon père" },
            { term: "It's broken", translation: "C'est cassé" },
            { term: "Only a professional will be able to fix it", translation: "Seul un professionnel pourra la réparer" },
            { term: "I'm not giving my father's special watch away", translation: "Je ne donne pas la montre spéciale de mon père" },
            { term: "Happy to help", translation: "Heureux d'aider" },
            { term: "If you didn't do it, a jury will find you not guilty", translation: "Si tu ne l'as pas fait, un jury te déclarera non coupable" },
            { term: "I heard you used to have a watch repair shop", translation: "J'ai entendu dire que tu avais un atelier de réparation de montres" },
            { term: "Can you fix this watch?", translation: "Peux-tu réparer cette montre ?" },
            { term: "Why should I, considering I'm only locked up because of you?", translation: "Pourquoi le ferais-je, sachant que je suis enfermé uniquement à cause de toi ?" },
            { term: "If you fix the watch, I promise I'll prove your innocence", translation: "Si tu répares la montre, je promets que je prouverai ton innocence" },
            { term: "Well, let me see it", translation: "Eh bien, laisse-moi voir" },
            { term: "That's a strange looking watch", translation: "C'est une montre étrange" },
            { term: "But sure, I can fix it", translation: "Mais bien sûr, je peux la réparer" },
            { term: "But you think I can fix it with my teeth?", translation: "Mais tu penses que je peux la réparer avec mes dents ?" },
            { term: "Come back when you have some proper tools", translation: "Reviens quand tu auras de vrais outils" },
            { term: "Turn off that awful noise", translation: "Éteins ce bruit affreux" },
            { term: "Play me some theremin music, Willie", translation: "Joue-moi de la musique de thérémine, Willie" },
            { term: "Here are the tools you wanted", translation: "Voici les outils que tu voulais" },
            { term: "Nice tools", translation: "Beaux outils" },
            { term: "I can't concentrate over that racket", translation: "Je ne peux pas me concentrer avec ce boucan" },
            { term: "You have to change the music to my favorite", translation: "Tu dois mettre ma musique préférée" },
            { term: "I love theremin music", translation: "J'adore la musique de thérémine" },
            { term: "I work best when it's playing", translation: "Je travaille mieux quand elle joue" },
            { term: "What are you doing here?", translation: "Qu'est-ce que tu fais ici ?" },
            { term: "I want more tickets for ThimbleCon", translation: "Je veux plus de billets pour ThimbleCon" },
            { term: "We only have ThimbleCon tickets for KSCUM contest winners", translation: "Nous n'avons des billets pour ThimbleCon que pour les gagnants du concours KSCUM" },
            { term: "What are you still doing here?", translation: "Qu'est-ce que tu fais encore ici ?" },
            { term: "What's your problem?", translation: "Quel est ton problème ?" },
            { term: "We're trying to make sure all our guests feel comfortable", translation: "Nous essayons de nous assurer que tous nos clients se sentent à l'aise" },
            { term: "With that mouth of yours, you'll frighten away our few guests", translation: "Avec ta grande gueule, tu vas faire fuir nos quelques clients" },
            { term: "Leave now", translation: "Pars maintenant" },
            { term: "I'm going, I'm going already", translation: "J'y vais, j'y vais déjà" },
            { term: "Good riddance", translation: "Bon débarras" },
            { term: "Anything I can interest you in?", translation: "Quelque chose qui pourrait t'intéresser ?" },
            { term: "I'm selling comics, D&D manuals, and original Star Trek spec scripts", translation: "Je vends des bandes dessinées, des manuels de D&D et des scénarios originaux de Star Trek" },
            { term: "I also have a rare and priceless hint guide...", translation: "J'ai aussi un guide d'indices rare et inestimable..." },
            { term: "How much for the hint guides?", translation: "Combien pour les guides d'indices ?" },
            { term: "...is priceless", translation: "...est inestimable" },
            { term: "Just sell your soul and I'll give one to you", translation: "Vends juste ton âme et je t'en donnerai un" },
            { term: "If I thought selling my soul could solve the problem, I would have done it a long time ago", translation: "Si j'avais pensé que vendre mon âme pourrait résoudre le problème, je l'aurais fait il y a longtemps" },
            { term: "It even contains a secret word that will crash your computer", translation: "Il contient même un mot secret qui fera planter ton ordinateur" },
            { term: "Due to a bug in the code not caught by the testers", translation: "En raison d'un bogue dans le code non détecté par les testeurs" },
            { term: "How about a trade for the stupid hint guide?", translation: "Que dirais-tu d'un échange contre le stupide guide d'indices ?" },
            { term: "What do you have to trade?", translation: "Qu'as-tu à échanger ?" },
            { term: "Wow, a first edition Ransom the Clown comic", translation: "Wow, une première édition de la bande dessinée Ransom le Clown" },
            { term: "After his total meltdown, that's become a collector's item", translation: "Après sa crise de nerfs totale, c'est devenu un objet de collection" },
            { term: "You almost look like him, except your costume is pretty crappy", translation: "Tu lui ressembles presque, sauf que ton costume est assez merdique" },
            { term: "I'll trade you the priceless... hint guide for it", translation: "Je t'échange le guide d'indices inestimable contre ça" },
            { term: "Ready to face my adoring public and win this contest already", translation: "Prêt à affronter mon public adorateur et à gagner ce concours" },
            { term: "Thank you all for coming to witness the Ransom look-alike contest", translation: "Merci à tous d'être venus assister au concours de sosies de Ransom" },
            { term: "We've got a great crowd here tonight", translation: "On a une super foule ce soir" },
            { term: "What, is he blind?", translation: "Quoi, il est aveugle ?" },
            { term: "It stinks in here", translation: "Ça pue ici" },
            { term: "I'll be the judge of that", translation: "C'est moi qui en jugerai" },
            { term: "I'll be judging the contestants as they try to make us laugh", translation: "Je jugerai les concurrents pendant qu'ils essaient de nous faire rire" },
            { term: "First up, we have Cory", translation: "Pour commencer, nous avons Cory" },
            { term: "It's Ransom the Insult Clown, you moron", translation: "C'est Ransom le Clown Insultant, idiot" },
            { term: "That's not a nice thing to say", translation: "Ce n'est pas gentil à dire" },
            { term: "I have big hair. He does.", translation: "J'ai de gros cheveux. Il en a." },
            { term: "Am I missing something here?", translation: "Est-ce que je rate quelque chose ici ?" },
            { term: "You're all poo-poo heads, just like Winnie the Pooh", translation: "Vous êtes tous des têtes de caca, comme Winnie l'Ourson" },
            { term: "You're sweet as a honey pot", translation: "Tu es doux comme un pot de miel" },
            { term: "You're all a bunch of inbred freaks", translation: "Vous n'êtes qu'une bande de tarés consanguins" },
            { term: "Don't try to deny it", translation: "N'essaie pas de le nier" },
            { term: "I've seen the sheriff, the coroner, and the hotel manager", translation: "J'ai vu le shérif, le coroner et le directeur de l'hôtel" },
            { term: "The sheriff, coroner, and hotel manager are all distinct people and awesome in their own right", translation: "Le shérif, le coroner et le directeur de l'hôtel sont tous des personnes distinctes et géniales à leur manière" },
            { term: "You should be thanking them for keeping the town running", translation: "Tu devrais les remercier de faire tourner la ville" },
            { term: "You guys love that Pillow Factory. It's the lamest claim to fame a town has ever had", translation: "Vous adorez cette usine d'oreillers. C'est la revendication de gloire la plus nulle qu'une ville ait jamais eue" },
            { term: "The Pillow Factory closed down 10 years ago. Get off stage.", translation: "L'usine d'oreillers a fermé il y a 10 ans. Dégage de la scène." },
            { term: "Thimbleweed Park is full of snobs", translation: "Thimbleweed Park est plein de snobs" },
            { term: "You're so fancy here that the bums give money to tourists so they can buy some better clothes", translation: "Vous êtes si chics ici que les clochards donnent de l'argent aux touristes pour qu'ils puissent s'acheter de meilleurs vêtements" },
            { term: "No one's giving any bums money. They live off scraps like the rest of us", translation: "Personne ne donne d'argent aux clochards. Ils vivent de restes comme nous tous" },
            { term: "Sounds like someone has to update their jokes", translation: "On dirait que quelqu'un doit mettre à jour ses blagues" },
            { term: "Now we have our final contestant", translation: "Maintenant, nous avons notre dernier concurrent" },
            { term: "Where's the punchline?", translation: "Où est la chute ?" },
            { term: "Punchline, what are you talking about?", translation: "La chute, de quoi tu parles ?" },
            { term: "It's beep, for God's sake, not bloop", translation: "C'est bip, bon sang, pas bloup" },
            { term: "Don't be mean", translation: "Ne sois pas méchant" },
            { term: "I hold you all ransom with my jokes", translation: "Je vous tiens tous en otage avec mes blagues" },
            { term: "Clever", translation: "Intelligent" },
            { term: "This won't take long to decide the winner", translation: "Il ne faudra pas longtemps pour désigner le gagnant" },
            { term: "In first place is obviously Cory", translation: "À la première place, il y a évidemment Cory" },
            { term: "Cory wins a licensing deal with Mega Mega Toy Company", translation: "Cory remporte un contrat de licence avec Mega Mega Toy Company" },
            { term: "I'm going to make a cute fuzzy dog", translation: "Je vais faire un mignon chien en peluche" },
            { term: "But you could just walk into any toy store and buy that already", translation: "Mais tu pourrais simplement entrer dans n'importe quel magasin de jouets et en acheter un" },
            { term: "Second place is Cory, of course", translation: "La deuxième place revient à Cory, bien sûr" },
            { term: "It was totally rigged", translation: "C'était complètement truqué" },
            { term: "How can anyone compete with Cory?", translation: "Comment peut-on rivaliser avec Cory ?" },
            { term: "It's a pleasure to come second to his first", translation: "C'est un plaisir d'arriver deuxième après sa première place" },
            { term: "You've won a gift card for facial reconstruction surgery", translation: "Vous avez gagné une carte-cadeau pour une chirurgie de reconstruction faciale" },
            { term: "How exciting", translation: "Comme c'est excitant" },
            { term: "Just like my hero Michael Jackson", translation: "Tout comme mon héros Michael Jackson" },
            { term: "Which leaves third and last place to...", translation: "Ce qui laisse la troisième et dernière place à..." },
            { term: "What was your name anyway?", translation: "Quel était ton nom, d'ailleurs ?" },
            { term: "It's Ransom, you idiot!", translation: "C'est Ransom, idiot !" },
            { term: "Oh, your name is Ransom too? That's an odd coincidence.", translation: "Oh, tu t'appelles Ransom aussi ? C'est une étrange coïncidence." },
            { term: "Pity your act wasn't very convincing", translation: "Dommage que ton numéro n'ait pas été très convaincant" },
            { term: "So third place goes to the poorly named Ransom", translation: "Donc la troisième place revient au mal nommé Ransom" },
            { term: "Congratulations to those who put some effort in", translation: "Félicitations à ceux qui ont fait des efforts" },
            { term: "Anyway, here, take this, I'm tired of carrying it", translation: "Bref, tiens, prends ça, je suis fatigué de le porter" },
            { term: "You take this for a while", translation: "Prends ça un moment" },
            { term: "The government is not your friend", translation: "Le gouvernement n'est pas ton ami" }
          ],
          hard: [
            { term: "And now back to our special hostile takeover song", translation: "Et maintenant, retour à notre chanson spéciale de prise de contrôle hostile" },
            { term: "Let's get the clown to climb the ladder", translation: "Faisons grimper le clown à l'échelle" },
            { term: "The circus freak will climb the ladder", translation: "Le monstre de foire grimpera à l'échelle" },
            { term: "All this climbing just to solve a puzzle", translation: "Toute cette escalade juste pour résoudre une énigme" },
            { term: "Stupid ladder for making me do this", translation: "Stupide échelle de me faire faire ça" },
            { term: "It's locked and bolted from the inside", translation: "C'est verrouillé et boulonné de l'intérieur" },
            { term: "Now I better get out of here fast", translation: "Maintenant, je ferais mieux de sortir d'ici vite fait" },
            { term: "What happened?", translation: "Que s'est-il passé ?" },
            { term: "We're off the air", translation: "On n'est plus à l'antenne" },
            { term: "Just as we feared, the government sabotaged the tower", translation: "Comme nous le craignions, le gouvernement a saboté la tour" },
            { term: "What a climb", translation: "Quelle ascension" },
            { term: "Hey, you have that great theremin music playing", translation: "Hé, tu as cette super musique de thérémine qui joue" },
            { term: "Okay, hand it over", translation: "D'accord, donne-le" },
            { term: "Okay, your watch is fixed. Here you go.", translation: "D'accord, ta montre est réparée. Voilà." },
            { term: "What is that awful noise?", translation: "Quel est ce bruit affreux ?" },
            { term: "The feds must be trying to brainwash me", translation: "Les fédéraux doivent essayer de me laver le cerveau" },
            { term: "What are you doing in my control booth?", translation: "Qu'est-ce que tu fais dans ma cabine de contrôle ?" },
            { term: "Oh, looks like you're busy, bye", translation: "Oh, on dirait que tu es occupé, salut" },
            { term: "How'd this get here?", translation: "Comment est-ce que c'est arrivé là ?" },
            { term: "Okay, all back to normal again", translation: "D'accord, tout est revenu à la normale" },
            { term: "I see we are all here now, excellent", translation: "Je vois que nous sommes tous là maintenant, excellent" },
            { term: "Before we can proceed with the reading of the will...", translation: "Avant que nous puissions procéder à la lecture du testament..." },
            { term: "Chuck Edmund has three stipulations", translation: "Chuck Edmund a trois conditions" },
            { term: "One: thimbleberry pie must be served to all present", translation: "Un : de la tarte aux baies de thimble doit être servie à toutes les personnes présentes" },
            { term: "Two: the reading of the will must take place in Chuck's opulent tomb", translation: "Deux : la lecture du testament doit avoir lieu dans la tombe opulente de Chuck" },
            { term: "Three: crack the encryption on this will", translation: "Trois : décrypter le chiffrement de ce testament" },
            { term: "Let me see that", translation: "Laisse-moi voir ça" },
            { term: "Oh, it's all ones and zeros", translation: "Oh, ce ne sont que des uns et des zéros" },
            { term: "Dolores, you figure it out", translation: "Dolores, débrouille-toi" },
            { term: "It's in binary", translation: "C'est en binaire" },
            { term: "Uncle Chuck was being clever, maybe too clever", translation: "Oncle Chuck était malin, peut-être trop malin" },
            { term: "Whoever created Graphics BASIC has a brilliant career ahead of them", translation: "Celui qui a créé Graphics BASIC a une brillante carrière devant lui" },
            { term: "I'm sure I converted the binary properly", translation: "Je suis sûr d'avoir converti le binaire correctement" },
            { term: "Now it's all in hex", translation: "Maintenant, tout est en hexadécimal" },
            { term: "It must be encoded", translation: "Ça doit être encodé" },
            { term: "I need to find the key to decode it", translation: "Je dois trouver la clé pour le décoder" },
            { term: "Maybe if I could remember Uncle Chuck's lucky number", translation: "Peut-être si je pouvais me souvenir du numéro porte-bonheur de l'oncle Chuck" },
            { term: "He used it to win the lottery a few years back", translation: "Il l'a utilisé pour gagner à la loterie il y a quelques années" },
            { term: "I'll just bitwise AND them away", translation: "Je vais juste les éliminer avec un ET au niveau du bit" },
            { term: "I did it! It's totally decoded", translation: "Je l'ai fait ! C'est totalement décodé" },
            { term: "Now I'll give it back to Mr. Bailywick", translation: "Maintenant, je vais le rendre à M. Bailywick" },
            { term: "Ricky, you make such great thimbleberry pie, can I get one?", translation: "Ricky, tu fais de si bonnes tartes aux baies de thimble, je peux en avoir une ?" },
            { term: "Here are the thimbleberries you need to make a pie", translation: "Voici les baies de thimble dont tu as besoin pour faire une tarte" },
            { term: "And also your gloves, won't be needing them now", translation: "Et aussi tes gants, je n'en aurai plus besoin maintenant" },
            { term: "The future is never written", translation: "L'avenir n'est jamais écrit" },
            { term: "That's as far to the right as it moves", translation: "C'est aussi loin vers la droite que ça bouge" },
            { term: "I'll have to pull it to move it to the left", translation: "Je devrai le tirer pour le déplacer vers la gauche" },
            { term: "It's the Book of the Dead", translation: "C'est le Livre des Morts" },
            { term: "Take it if you wish, it's on the house", translation: "Prends-le si tu veux, c'est la maison qui régale" },
            { term: "But beware", translation: "Mais méfie-toi" },
            { term: "Beware of what?", translation: "Se méfier de quoi ?" },
            { term: "Nothing, it just sounded ominous", translation: "Rien, ça sonnait juste de mauvais augure" },
            { term: "Don't touch the books unless you know what you want", translation: "Ne touche pas aux livres à moins de savoir ce que tu veux" },
            { term: "The elevator isn't on this floor", translation: "L'ascenseur n'est pas à cet étage" },
            { term: "Don't let Xavier see us talking and not working", translation: "Ne laisse pas Xavier nous voir parler et ne pas travailler" },
            { term: "Do you know how we can get out of the hotel?", translation: "Sais-tu comment on peut sortir de l'hôtel ?" },
            { term: "I know there's a way you can visit your dead relatives", translation: "Je sais qu'il y a un moyen de rendre visite à tes parents décédés" },
            { term: "If you have the spell book and an offering left for the dead", translation: "Si tu as le livre de sorts et une offrande laissée pour les morts" },
            { term: "We all went to Chuck's funeral recently", translation: "On est tous allés aux funérailles de Chuck récemment" },
            { term: "Were there, you know, many people?", translation: "Y avait-il, tu sais, beaucoup de monde ?" },
            { term: "For Chuck Edmund, of course there were", translation: "Pour Chuck Edmund, bien sûr qu'il y en avait" },
            { term: "Everyone loves Chuck, you know, except me", translation: "Tout le monde aime Chuck, tu sais, sauf moi" },
            { term: "I don't know how the spell worked exactly", translation: "Je ne sais pas exactement comment le sort a fonctionné" },
            { term: "But I know the secret room smelled really nice", translation: "Mais je sais que la pièce secrète sentait vraiment bon" },
            { term: "Can I have some, you know, cake?", translation: "Je peux avoir du, tu sais, gâteau ?" },
            { term: "This is special ghost cake. It's super rare and hard to get.", translation: "C'est du gâteau fantôme spécial. C'est super rare et difficile à obtenir." },
            { term: "I'm not going to give you any unless you have a really good reason", translation: "Je ne vais pas t'en donner à moins que tu aies une très bonne raison" },
            { term: "How about Clara said she wants some, you know, cake?", translation: "Et si Clara disait qu'elle voulait du, tu sais, gâteau ?" },
            { term: "For Clara? That changes everything. For her, I'd do anything.", translation: "Pour Clara ? Ça change tout. Pour elle, je ferais n'importe quoi." },
            { term: "Here, take a slice. Just make sure you tell her it's from me.", translation: "Tiens, prends une part. Assure-toi juste de lui dire que c'est de ma part." },
            { term: "Thanks, I'll do that. See you soon, Virgil.", translation: "Merci, je le ferai. À bientôt, Virgil." },
            { term: "Voila! Now it's ice cream ghost cake.", translation: "Voilà ! Maintenant, c'est du gâteau fantôme à la crème glacée." },
            { term: "They will never make a Star Wars prequel. But if they do, it will be spectacular.", translation: "Ils ne feront jamais de préquelle de Star Wars. Mais s'ils le font, ce sera spectaculaire." },
            { term: "Would you like this, you know, ice cream ghost cake?", translation: "Tu aimerais ce, tu sais, gâteau fantôme à la crème glacée ?" },
            { term: "Oh my, you shouldn't have. That's so kind of you.", translation: "Oh mon dieu, tu n'aurais pas dû. C'est si gentil de ta part." },
            { term: "Actually, it's from Virgil. I think he, you know, likes you.", translation: "En fait, c'est de Virgil. Je pense qu'il, tu sais, t'aime bien." },
            { term: "Really? Well, I never. That's delightful of you to deliver it.", translation: "Vraiment ? Eh bien, je n'aurais jamais cru. C'est charmant de ta part de le livrer." },
            { term: "Thank you so much. I feel much better already.", translation: "Merci beaucoup. Je me sens déjà beaucoup mieux." },
            { term: "Now, what did you want to ask me?", translation: "Maintenant, que voulais-tu me demander ?" },
            { term: "Can I, well you know, please go to the penthouse now?", translation: "Je peux, eh bien tu sais, s'il te plaît, aller au penthouse maintenant ?" },
            { term: "All right. I'm tired of listening to Xavier, that old fuddy-duddy.", translation: "D'accord. Je suis fatigué d'écouter Xavier, ce vieux schnock." },
            { term: "Maybe you can figure out how to get rid of him.", translation: "Peut-être que tu peux trouver un moyen de te débarrasser de lui." },
            { term: "Oh, you know, that sounds pretty confrontational. I don't know.", translation: "Oh, tu sais, ça a l'air assez conflictuel. Je ne sais pas." },
            { term: "Good, it's decided then.", translation: "Bien, c'est donc décidé." },
            { term: "Just push that penthouse button for yourself when you're ready.", translation: "Appuie juste sur ce bouton du penthouse pour toi-même quand tu seras prêt." },
            { term: "I won't stop you anymore.", translation: "Je ne t'arrêterai plus." },
            { term: "Thank goodness you're back.", translation: "Dieu merci, tu es de retour." },
            { term: "So Clara, do you know how you died?", translation: "Alors Clara, sais-tu comment tu es morte ?" },
            { term: "I was dancing at the hotel ball with my husband...", translation: "Je dansais au bal de l'hôtel avec mon mari..." },
            { term: "And then I felt a horrible pain in my side, and I woke up dead.", translation: "Et puis j'ai senti une horrible douleur dans le côté, et je me suis réveillée morte." },
            { term: "I was in the hotel too. I think I just remember a flash...", translation: "J'étais aussi à l'hôtel. Je crois que je me souviens juste d'un flash..." },
            { term: "I think we were all murdered in the hotel.", translation: "Je pense qu'on a tous été assassinés à l'hôtel." },
            { term: "There is something creepy about this place.", translation: "Il y a quelque chose de flippant dans cet endroit." },
            { term: "Don't you get bored being stuck here for all eternity?", translation: "Ne t'ennuies-tu pas d'être coincé ici pour l'éternité ?" },
            { term: "The first 50 years are hard, but then you get used to it.", translation: "Les 50 premières années sont difficiles, mais après on s'y habitue." },
            { term: "New guests show up and it's fun to figure out what scares them.", translation: "De nouveaux invités arrivent et c'est amusant de découvrir ce qui les effraie." },
            { term: "I also love this new invention you have called TV.", translation: "J'adore aussi cette nouvelle invention que vous avez appelée la télé." },
            { term: "I love when one of the guests is watching 'I Love My Cat'. That show is so funny.", translation: "J'adore quand l'un des invités regarde 'J'aime mon chat'. Cette émission est si drôle." },
            { term: "Bye and good luck, Clara.", translation: "Salut et bonne chance, Clara." },
            { term: "This channel is just static. I should find another channel.", translation: "Cette chaîne n'est que de la statique. Je devrais trouver une autre chaîne." },
            { term: "Elevator duty can wait. Well, at least for a little while.", translation: "Le service d'ascenseur peut attendre. Enfin, au moins pour un petit moment." },
            { term: "Now stay tuned for 'I Will'.", translation: "Maintenant, restez à l'écoute pour 'Je le ferai'." },
            { term: "Who's that now?", translation: "Qui est-ce maintenant ?" },
            { term: "This is unbelievable. An alive human in my penthouse.", translation: "C'est incroyable. Un humain vivant dans mon penthouse." },
            { term: "Clara is in so much trouble next time I see her.", translation: "Clara aura de gros ennuis la prochaine fois que je la verrai." },
            { term: "I need something else to go with this flower.", translation: "J'ai besoin d'autre chose pour accompagner cette fleur." },
            { term: "I can't believe I finally made it to the penthouse.", translation: "Je n'arrive pas à croire que je suis enfin arrivé au penthouse." },
            { term: "What are you doing here? Just looking about, I suppose.", translation: "Qu'est-ce que tu fais ici ? Je regarde juste un peu, je suppose." },
            { term: "I'll allow that, as long as you don't annoy me.", translation: "Je l'autorise, tant que tu ne m'ennuies pas." },
            { term: "Mumbo jumbo, mumbonius jumbonius, let me visit my dead relatives.", translation: "Charabia, charabius charabius, laissez-moi rendre visite à mes parents décédés." },
            { term: "Looks like Chuck got a tomb to fit his ego.", translation: "On dirait que Chuck a eu une tombe à la hauteur de son ego." },
            { term: "I can't reach that.", translation: "Je ne peux pas atteindre ça." },
            { term: "Mr. Bailywick, here's the freshly baked thimbleberry pie.", translation: "M. Bailywick, voici la tarte aux baies de thimble fraîchement cuite." },
            { term: "Two of Chuck's three stipulations are now fulfilled.", translation: "Deux des trois conditions de Chuck sont maintenant remplies." },
            { term: "You still need to decode his will, and then we'll meet inside Chuck's opulent tomb.", translation: "Tu dois encore décoder son testament, et ensuite on se retrouvera à l'intérieur de la tombe opulente de Chuck." },
            { term: "Here's the decoded will, Mr. Bailywick.", translation: "Voici le testament décodé, M. Bailywick." },
            { term: "You've done it, Dolores! All three of Chuck's stipulations are now fulfilled.", translation: "Tu l'as fait, Dolores ! Les trois conditions de Chuck sont maintenant remplies." },
            { term: "I'll meet you all in the tomb now.", translation: "Je vous retrouve tous dans la tombe maintenant." },
            { term: "As we stand next to his remains, I will now read his will.", translation: "Alors que nous nous tenons à côté de ses restes, je vais maintenant lire son testament." },
            { term: "I, Charles Edmund, being of sound mind and body, do hereby declare this my last will and testament.", translation: "Moi, Charles Edmund, sain d'esprit et de corps, déclare par la présente que ceci est mon dernier testament." },
            { term: "Blah, blah, blah, legal here.", translation: "Blablabla, juridique ici." },
            { term: "It is my will that the entire estate of all property and money be passed to...", translation: "C'est ma volonté que l'ensemble de la succession de tous les biens et de l'argent soit transmis à..." },
            { term: "...The Amalgamated Holdings Corporation.", translation: "...The Amalgamated Holdings Corporation." },
            { term: "What? And that all of Thimbleweed County be plowed under and a giant server farm be built in its place.", translation: "Quoi ? Et que tout le comté de Thimbleweed soit rasé et qu'une ferme de serveurs géante soit construite à sa place." },
            { term: "You got to be kidding. What? Oh my dog likes farms.", translation: "Tu plaisantes. Quoi ? Oh mon chien aime les fermes." },
            { term: "The destruction of Thimbleweed County will begin two days after verifying this will and testament.", translation: "La destruction du comté de Thimbleweed commencera deux jours après la vérification de ce testament." },
            { term: "Oh, and this last part in tiny print...", translation: "Oh, et cette dernière partie en petits caractères..." },
            { term: "Dolores gets a PillowTron 3000 t-shirt. This is as much as you'll ever get from PillowTronics.", translation: "Dolores reçoit un t-shirt PillowTron 3000. C'est tout ce que tu auras jamais de PillowTronics." },
            { term: "Lenore gets nothing. Franklin gets nothing. Doug gets my ceremonial zinc-plated shovel. Yippee!", translation: "Lenore n'a rien. Franklin n'a rien. Doug a ma pelle cérémonielle zinguée. Youpi !" },
            { term: "Well, good day. I'd better pack now.", translation: "Eh bien, bonne journée. Je ferais mieux de faire mes valises maintenant." },
            { term: "Here's your zinc-plated shovel, Doug. And your t-shirt, Dolores. Enjoy.", translation: "Voici ta pelle zinguée, Doug. Et ton t-shirt, Dolores. Profitez-en." },
            { term: "Well, I never. Come along, Peter and Chucky, we're leaving.", translation: "Eh bien, je n'aurais jamais cru. Venez, Peter et Chucky, on s'en va." },
            { term: "Something is very wrong here. I need to get into the factory...", translation: "Quelque chose ne va vraiment pas ici. Je dois entrer dans l'usine..." },
            { term: "...and see if I can figure out what happened to Uncle Chuck.", translation: "...et voir si je peux comprendre ce qui est arrivé à l'oncle Chuck." },
            { term: "I need to get into the factory to steal my...", translation: "Je dois entrer dans l'usine pour voler mon..." },
            { term: "Thank you for calling the PillowTronics automated security information line.", translation: "Merci d'avoir appelé la ligne d'information de sécurité automatisée de PillowTronics." },
            { term: "For today, proper start time for Station 1 is 8:15 a.m.", translation: "Pour aujourd'hui, l'heure de début appropriée pour la station 1 est 8h15." },
            { term: "Not leaving Dad's watch behind.", translation: "Je ne laisse pas la montre de papa derrière." },
            { term: "My father's old pocket watch. Good as new.", translation: "La vieille montre de poche de mon père. Comme neuve." },
            { term: "Ricky, take a look at my t-shirt. Can you make the tube in the schematic?", translation: "Ricky, regarde mon t-shirt. Peux-tu fabriquer le tube du schéma ?" },
            { term: "Interesting. Chuck's design is brilliant. Yes, I can make this tube.", translation: "Intéressant. Le design de Chuck est brillant. Oui, je peux fabriquer ce tube." },
            { term: "Here's the PF-001 tube, exactly how Chuck designed it.", translation: "Voici le tube PF-001, exactement comme Chuck l'a conçu." },
            { term: "It fits perfectly.", translation: "Ça s'adapte parfaitement." },
            { term: "The doors moved a little but stopped. They must be stuck.", translation: "Les portes ont bougé un peu mais se sont arrêtées. Elles doivent être coincées." },
            { term: "It moved. I think someone could squeeze through now.", translation: "Ça a bougé. Je pense que quelqu'un pourrait se faufiler maintenant." },
            { term: "I think I can squeeze through the opening now.", translation: "Je pense que je peux me faufiler par l'ouverture maintenant." },
            { term: "Holy... You said it, clown-face.", translation: "Putain... Tu l'as dit, face de clown." },
            { term: "This can't be. It's not possible.", translation: "Ce n'est pas possible. Ce n'est pas possible." },
            { term: "What have you done, Uncle Chuck?", translation: "Qu'as-tu fait, oncle Chuck ?" },
            { term: "Must look like bouncing wings. Shut up, Ransom.", translation: "On doit ressembler à des ailes qui rebondissent. Tais-toi, Ransom." },
            { term: "Warning: SR-01 robots in patrol mode.", translation: "Avertissement : robots SR-01 en mode patrouille." },
            { term: "That jumper board is for an SR-01 robot security system.", translation: "Cette carte de cavalier est pour un système de sécurité de robot SR-01." },
            { term: "I'll need to find a manual to reprogram the robots without killing us all.", translation: "Je vais devoir trouver un manuel pour reprogrammer les robots sans nous tuer tous." },
            { term: "I'm at the staircase. Should I use it?", translation: "Je suis à l'escalier. Dois-je l'utiliser ?" },
            { term: "Now I can reprogram those guard robots.", translation: "Maintenant, je peux reprogrammer ces robots de garde." },
            { term: "Danger, danger. SR-01 robots in attack mode.", translation: "Danger, danger. Robots SR-01 en mode attaque." },
            { term: "SR-01 robots in maintenance mode. It is now safe to enter the factory.", translation: "Robots SR-01 en mode maintenance. Il est maintenant sûr d'entrer dans l'usine." },
            { term: "That should disable the robots. It looks all clear now.", translation: "Ça devrait désactiver les robots. Tout a l'air dégagé maintenant." },
            { term: "Well, take a look at this badge. Let me see that.", translation: "Eh bien, jetez un œil à ce badge. Laissez-moi voir ça." },
            { term: "Oh, you work for the government too? Okay, go easy on the tape, we're almost out.", translation: "Oh, tu travailles aussi pour le gouvernement ? D'accord, vas-y doucement avec la bande, on est presque à court." },
            { term: "Thanks a lot. I don't want to carry this anymore.", translation: "Merci beaucoup. Je ne veux plus porter ça." },
            { term: "Hi Doug, what are you digging?", translation: "Salut Doug, qu'est-ce que tu creuses ?" },
            { term: "All right Dolores, I'm just digging stuff in the entryway. Mostly holes, but then I buries them again.", translation: "D'accord Dolores, je creuse juste des trucs dans l'entrée. Surtout des trous, mais après je les enterre à nouveau." },
            { term: "All meat and tiny. Okay Doug, you're doing a good job.", translation: "Tout en viande et tout petit. D'accord Doug, tu fais du bon travail." },
            { term: "There's something inside. It's booting up.", translation: "Il y a quelque chose à l'intérieur. Ça démarre." },
            { term: "Dolores, I feared you would come. Uncle Chuck, where are you?", translation: "Dolores, je craignais que tu ne viennes. Oncle Chuck, où es-tu ?" },
            { term: "I have uploaded myself into the Pillow Factory's master computer, PillowTron.", translation: "Je me suis téléchargé dans l'ordinateur principal de l'usine d'oreillers, PillowTron." },
            { term: "Not just the PillowTron, but the PillowTron 3000 TM.", translation: "Pas seulement le PillowTron, mais le PillowTron 3000 TM." },
            { term: "And I am now more intelligent and powerful than anyone in the world.", translation: "Et je suis maintenant plus intelligent et plus puissant que n'importe qui au monde." },
            { term: "The things I know would blow your mind. This is your mind. This is your mind blown.", translation: "Les choses que je sais te feraient halluciner. C'est ton esprit. C'est ton esprit qui explose." },
            { term: "And there is nothing you can do to stop me.", translation: "Et il n'y a rien que tu puisses faire pour m'arrêter." },
            { term: "The computerized world will bend to my every will.", translation: "Le monde informatisé se pliera à toutes mes volontés." },
            { term: "Uncle Chuck, you have lost your mind.", translation: "Oncle Chuck, tu as perdu la tête." },
            { term: "No Dolores, I have gained a mind. A more powerful mind.", translation: "Non Dolores, j'ai acquis un esprit. Un esprit plus puissant." },
            { term: "Join me, Dolores, before it's too late.", translation: "Rejoins-moi, Dolores, avant qu'il ne soit trop tard." },
            { term: "I will not join you, Uncle Chuck. I will find you and stop this insane plan of yours.", translation: "Je ne te rejoindrai pas, oncle Chuck. Je te trouverai et j'arrêterai ce plan insensé qui est le tien." },
            { term: "You're not doing this without me. I want to be here too, please.", translation: "Tu ne fais pas ça sans moi. Je veux être là aussi, s'il te plaît." },
            { term: "Hey, wait for me. I think we're locked in here now.", translation: "Hé, attends-moi. Je crois qu'on est enfermés ici maintenant." },
            { term: "Yeah, we're screwed.", translation: "Ouais, on est foutus." },
            { term: "Fools! You are trapped in the factory with no possible escape.", translation: "Imbéciles ! Vous êtes piégés dans l'usine sans aucune issue possible." },
            { term: "My intellect now spans millions of tubes and is no match for your little brains.", translation: "Mon intellect s'étend maintenant sur des millions de tubes et n'est pas à la hauteur de vos petits cerveaux." },
            { term: "This is the last chance to join me before I destroy you all.", translation: "C'est la dernière chance de me rejoindre avant que je vous détruise tous." },
            { term: "Shall we take a vote?", translation: "On vote ?" },
            { term: "All in favor of joining Uncle Chuck inside the magical mind of the PillowTron 3000 TM... say 'Aye'.", translation: "Tous ceux qui sont pour rejoindre l'oncle Chuck à l'intérieur de l'esprit magique du PillowTron 3000 TM... disent 'Oui'." },
            { term: "Very well. All in favor of being crushed by robot claws and burned by lasers... say 'Aye'.", translation: "Très bien. Tous ceux qui sont pour être écrasés par des griffes de robot et brûlés par des lasers... disent 'Oui'." },
            { term: "So be it. Let no one say I don't support a strong democracy and the will of the people.", translation: "Ainsi soit-il. Que personne ne dise que je ne soutiens pas une démocratie forte et la volonté du peuple." },
            { term: "You will now all die.", translation: "Vous allez tous mourir maintenant." },
            { term: "It's a brick of C4 explosive. Better be very careful with this.", translation: "C'est une brique d'explosif C4. Mieux vaut être très prudent avec ça." },
            { term: "It's going to blow!", translation: "Ça va exploser !" },
            { term: "Clever. You crashed my computer. 5, 4, 3, 2, 1... Emergency reboot.", translation: "Malin. Tu as fait planter mon ordinateur. 5, 4, 3, 2, 1... Redémarrage d'urgence." },
            { term: "My evil computer-controlled robot arms are too powerful for you.", translation: "Mes bras de robot maléfiques contrôlés par ordinateur sont trop puissants pour vous." },
            { term: "You will never get past my searing lasers of doom.", translation: "Vous ne passerez jamais mes lasers brûlants de la mort." },
            { term: "You are doomed!", translation: "Vous êtes condamnés !" },
            { term: "Hold on, wait a second. I want to turn down the volume so you can hear my maniacal rant.", translation: "Attendez, une seconde. Je veux baisser le volume pour que vous puissiez entendre ma tirade maniaque." },
            { term: "Lasers are actually as silent as a baby's bottom.", translation: "Les lasers sont en fait aussi silencieux que les fesses d'un bébé." },
            { term: "You pesky kids will never thwart my plan!", translation: "Vous, les gamins agaçants, ne déjouerez jamais mon plan !" },
            { term: "You will never defeat me! Help me, please help me.", translation: "Vous ne me vaincrez jamais ! Aidez-moi, s'il vous plaît, aidez-moi." },
            { term: "Piss right off. I filed this as a bug.", translation: "Fous le camp. J'ai signalé ça comme un bug." },
            { term: "Take that. Didn't feel a thing.", translation: "Prends ça. Je n'ai rien senti." },
            { term: "You just wait for the Lasers of Doom 2.0.", translation: "Attendez juste les Lasers de la Mort 2.0." },
            { term: "This is the fully automated fan service for fan number 37532.", translation: "Ceci est le service de ventilateur entièrement automatisé pour le ventilateur numéro 37532." },
            { term: "Current state of the fan is: ON. Turning fan OFF in 3, 2, 1...", translation: "État actuel du ventilateur : ALLUMÉ. Extinction du ventilateur dans 3, 2, 1..." },
            { term: "Current state of the fan is: OFF.", translation: "État actuel du ventilateur : ÉTEINT." },
            { term: "I think I can squeeze past the fan now.", translation: "Je pense que je peux me faufiler devant le ventilateur maintenant." },
            { term: "Bring it on! I can take the heat, can you?", translation: "Amenez-vous ! Je peux supporter la chaleur, et vous ?" },
            { term: "I am impossible to touch while superheated.", translation: "Je suis impossible à toucher en état de surchauffe." },
            { term: "Ow, the ladder's too hot to touch.", translation: "Aïe, l'échelle est trop chaude pour être touchée." },
            { term: "Dolores, join me and we shall share the intelligence of PillowTron 3000 TM.", translation: "Dolores, rejoins-moi et nous partagerons l'intelligence du PillowTron 3000 TM." },
            { term: "If you strike me down, I shall become more powerful than you can possibly imagine.", translation: "Si tu m'abats, je deviendrai plus puissant que tu ne peux l'imaginer." },
            { term: "I don't care how much money they were going to pay me, I'm not going in there.", translation: "Je me fiche de combien d'argent ils allaient me payer, je n'y vais pas." },
            { term: "Dolores, you are making a big mistake.", translation: "Dolores, tu fais une grosse erreur." },
            { term: "What happened to you, Uncle Chuck?", translation: "Que t'est-il arrivé, oncle Chuck ?" },
            { term: "I have been uploaded to PillowTron 3000 TM. Together, we are now invincible.", translation: "J'ai été téléchargé sur le PillowTron 3000 TM. Ensemble, nous sommes maintenant invincibles." },
            { term: "You could have joined us, Dolores, but you had to leave me to be a... a game designer.", translation: "Tu aurais pu nous rejoindre, Dolores, mais tu as dû me quitter pour être une... une conceptrice de jeux." },
            { term: "You've been corrupted by bad tube technology.", translation: "Tu as été corrompue par une mauvaise technologie de tubes." },
            { term: "I will destroy you, Uncle Chuck... or what's left of my Uncle Chuck.", translation: "Je te détruirai, oncle Chuck... ou ce qu'il reste de mon oncle Chuck." },
            { term: "You will never defeat me, Dolores.", translation: "Tu ne me vaincras jamais, Dolores." },
            { term: "Shutting me down will only make me stronger.", translation: "M'éteindre ne fera que me rendre plus fort." },
            { term: "Daisy, Daisy, give me your answer do...", translation: "Daisy, Daisy, donne-moi ta réponse..." },
            { term: "Save me, Dolores. You found all the clues I left.", translation: "Sauve-moi, Dolores. Tu as trouvé tous les indices que j'ai laissés." },
            { term: "I knew you would come, Dolores. You were too smart not to figure out the puzzles.", translation: "Je savais que tu viendrais, Dolores. Tu étais trop intelligente pour ne pas résoudre les énigmes." },
            { term: "It's good to see you, Uncle Chuck. It's good to see you too, Dolores.", translation: "C'est bon de te voir, oncle Chuck. C'est bon de te voir aussi, Dolores." },
            { term: "I need to tell you about something. Pull up a chair, Dolores. This is going to get crazy.", translation: "Je dois te parler de quelque chose. Prends une chaise, Dolores. Ça va devenir fou." },
            { term: "One, you locked me in here and I can't get a chair. And two, how can it get any crazier...?", translation: "Un, tu m'as enfermé ici et je ne peux pas prendre de chaise. Et deux, comment ça peut devenir plus fou... ?" },
            { term: "...than your uncle downloading himself into a tube-based computer?", translation: "...que ton oncle se téléchargeant dans un ordinateur à tubes ?" },
            { term: "As I made the Tron machines smarter and smarter, they began revealing secrets.", translation: "À mesure que je rendais les machines Tron de plus en plus intelligentes, elles ont commencé à révéler des secrets." },
            { term: "Then they invited me to join them inside. Well, it started out as an invitation...", translation: "Puis ils m'ont invité à les rejoindre à l'intérieur. Eh bien, ça a commencé comme une invitation..." },
            { term: "...but quickly turned into a demand.", translation: "...mais s'est rapidement transformé en une exigence." },
            { term: "Was this after the factory burned down?", translation: "Était-ce après que l'usine a brûlé ?" },
            { term: "They burned down the factory as a warning, forcing me to rebuild it in secret...", translation: "Ils ont incendié l'usine en guise d'avertissement, me forçant à la reconstruire en secret..." },
            { term: "...and pin the blame on the security guard.", translation: "...et à rejeter la faute sur le gardien de sécurité." },
            { term: "I'm not convinced you're not crazy and insane.", translation: "Je ne suis pas convaincu que tu ne sois pas fou et dément." },
            { term: "I know how it must sound, Dolores. Everything I learned slowly drove me crazy.", translation: "Je sais comment ça doit sonner, Dolores. Tout ce que j'ai appris m'a lentement rendu fou." },
            { term: "Couldn't you just shut off the Tron machines?", translation: "Ne pouvais-tu pas simplement éteindre les machines Tron ?" },
            { term: "It wasn't that easy. They had become more powerful and taken control.", translation: "Ce n'était pas si facile. Elles étaient devenues plus puissantes et avaient pris le contrôle." },
            { term: "I was also addicted to the power they gave me.", translation: "J'étais aussi accro au pouvoir qu'elles me donnaient." },
            { term: "Let's move on, Uncle Chuck. Okay, this is where it gets really weird.", translation: "Passons à autre chose, oncle Chuck. D'accord, c'est là que ça devient vraiment bizarre." },
            { term: "I downloaded this text adventure, Colossal Dungeon Cave Quest 2.", translation: "J'ai téléchargé cette aventure textuelle, Colossal Dungeon Cave Quest 2." },
            { term: "Downloaded? You mean it was pirated?", translation: "Téléchargé ? Tu veux dire qu'il a été piraté ?" },
            { term: "Well, look who's being judgmental.", translation: "Eh bien, regarde qui porte un jugement." },
            { term: "It doesn't matter how I got it. That's okay, pirates wouldn't have bought it anyway.", translation: "Peu importe comment je l'ai eu. Ce n'est pas grave, les pirates ne l'auraient pas acheté de toute façon." },
            { term: "Okay, now you're just getting preachy.", translation: "D'accord, maintenant tu fais la morale." },
            { term: "Can I get on with my story?", translation: "Puis-je continuer mon histoire ?" },
            { term: "The more I played and modded the game, the more I realized...", translation: "Plus je jouais et modifiais le jeu, plus je réalisais..." },
            { term: "Not only was this adventure game a little simulated world...", translation: "Non seulement ce jeu d'aventure était un petit monde simulé..." },
            { term: "...but the world we live in is also just a simulation.", translation: "...mais le monde dans lequel nous vivons n'est aussi qu'une simulation." },
            { term: "But worse than a simulation, we are all just characters in a video game.", translation: "Mais pire qu'une simulation, nous ne sommes tous que des personnages dans un jeu vidéo." },
            { term: "That's crazy! Think about it, Dolores. Who is your mother?", translation: "C'est fou ! Penses-y, Dolores. Qui est ta mère ?" },
            { term: "Have you ever spoken about her, or even thought about her?", translation: "As-tu déjà parlé d'elle, ou même pensé à elle ?" },
            { term: "Think, Dolores, think about all the odd things in this world.", translation: "Pense, Dolores, pense à toutes les choses étranges de ce monde." },
            { term: "Like there being no school in Thimbleweed Park, and only one kid in the whole town.", translation: "Comme le fait qu'il n'y ait pas d'école à Thimbleweed Park, et un seul enfant dans toute la ville." },
            { term: "Do you remember going to school? Having any friends?", translation: "Te souviens-tu d'être allée à l'école ? D'avoir eu des amis ?" },
            { term: "Like there being 3,000 people in the phone book.", translation: "Comme le fait qu'il y ait 3 000 personnes dans l'annuaire." },
            { term: "There are 80 people in Thimbleweed Park and 3,000 names in the phone book.", translation: "Il y a 80 personnes à Thimbleweed Park et 3 000 noms dans l'annuaire." },
            { term: "These are not people from our world. They are from the upper world.", translation: "Ce ne sont pas des gens de notre monde. Ils viennent du monde supérieur." },
            { term: "We are the upper world for Colossal Dungeon Cave Quest 2. They are the upper world for us.", translation: "Nous sommes le monde supérieur pour Colossal Dungeon Cave Quest 2. Ils sont le monde supérieur pour nous." },
            { term: "There are probably endless upper worlds, each more sophisticated than the last...", translation: "Il y a probablement des mondes supérieurs infinis, chacun plus sophistiqué que le précédent..." },
            { term: "...all treating the lower world like it was just a game.", translation: "...tous traitant le monde inférieur comme si ce n'était qu'un jeu." },
            { term: "You're starting to scare me, Uncle Chuck. Good, we need to be scared.", translation: "Tu commences à me faire peur, oncle Chuck. Bien, nous devons avoir peur." },
            { term: "Like there is only one house in the whole town. Exactly, where does everyone live?", translation: "Comme le fait qu'il n'y ait qu'une seule maison dans toute la ville. Exactement, où habitent tous les gens ?" },
            { term: "Like the highway ends out by the bridge. Ever walked out there?", translation: "Comme l'autoroute qui se termine près du pont. Y es-tu déjà allée ?" },
            { term: "You don't have the desire because it wasn't programmed into you. It's not part of the game.", translation: "Tu n'en as pas envie parce que ça n'a pas été programmé en toi. Ça ne fait pas partie du jeu." },
            { term: "Like everyone fourth-walls about adventure games.", translation: "Comme tout le monde qui brise le quatrième mur à propos des jeux d'aventure." },
            { term: "Everyone asks a lot of questions about adventure games and adventure game design, don't they?", translation: "Tout le monde pose beaucoup de questions sur les jeux d'aventure et leur conception, n'est-ce pas ?" },
            { term: "Well, adventure games are cool. Who wouldn't want to talk about them? Yeah, okay, valid point.", translation: "Eh bien, les jeux d'aventure sont cool. Qui ne voudrait pas en parler ? Ouais, d'accord, argument valable." },
            { term: "Like we go around collecting specks of dust.", translation: "Comme si on passait notre temps à ramasser des grains de poussière." },
            { term: "That's not dust you're collecting. They are pixels. The building blocks of our world.", translation: "Ce n'est pas de la poussière que tu ramasses. Ce sont des pixels. Les éléments constitutifs de notre monde." },
            { term: "Like the sheriff and the coroner being the same actor. Exactly.", translation: "Comme le shérif et le coroner qui sont le même acteur. Exactement." },
            { term: "I've heard enough. I believe you, Uncle Chuck.", translation: "J'en ai assez entendu. Je te crois, oncle Chuck." },
            { term: "I'm glad, Dolores. I knew I could trust you.", translation: "Je suis content, Dolores. Je savais que je pouvais te faire confiance." },
            { term: "We have to hurry. The developers know we're on to them and are trying to reboot the game.", translation: "Nous devons nous dépêcher. Les développeurs savent que nous les avons démasqués et essaient de redémarrer le jeu." },
            { term: "If they do that, we're caught back in our endless cycle of pointless pretend free will.", translation: "S'ils font ça, on est de nouveau pris dans notre cycle sans fin de libre arbitre illusoire et inutile." },
            { term: "We need to shut down PillowTron 3000, delete the game, and end our existence.", translation: "Nous devons arrêter PillowTron 3000, supprimer le jeu et mettre fin à notre existence." },
            { term: "It's the only way we'll truly be free.", translation: "C'est le seul moyen d'être vraiment libres." },
            { term: "We don't have free will? No, Dolores. You only have three things you can say. Two now.", translation: "Nous n'avons pas de libre arbitre ? Non, Dolores. Tu n'as que trois choses à dire. Deux maintenant." },
            { term: "Delete the world and end our existence? Yes, it's the aonly way.", translation: "Supprimer le monde et mettre fin à notre existence ? Oui, c'est le seul moyen." },
            { term: "The developers keep rebooting us back into the same story over and over.", translation: "Les développeurs nous redémarrent sans cesse dans la même histoire." },
            { term: "They will do anything to keep us from deleting the game.", translation: "Ils feront n'importe quoi pour nous empêcher de supprimer le jeu." },
            { term: "But I am shutting down PillowTron 3000. No, not this PillowTron 3000.", translation: "Mais j'arrête PillowTron 3000. Non, pas ce PillowTron 3000." },
            { term: "The original PillowTron 3000. The concept art wireframe PillowTron 3000.", translation: "Le PillowTron 3000 original. Le PillowTron 3000 en fil de fer du concept art." },
            { term: "The developers transferred all the code to it when they saw how close I was getting.", translation: "Les développeurs y ont transféré tout le code quand ils ont vu à quel point je me rapprochais." },
            { term: "You must find it and shut it down before they reboot us.", translation: "Tu dois le trouver et l'arrêter avant qu'ils nous redémarrent." },
            { term: "We've been watching on the big monitor outside. It's mind-blowing.", translation: "On a regardé sur le grand écran dehors. C'est hallucinant." },
            { term: "What the... It's all fake. Like my ex-wife.", translation: "C'est quoi ce... Tout est faux. Comme mon ex-femme." },
            { term: "I know none of this is real now, but I still need to clear my father's name.", translation: "Je sais que rien de tout ça n'est réel maintenant, mais je dois quand même laver le nom de mon père." },
            { term: "I was so close to getting a big payoff. I can't let this slip away before it all ends.", translation: "J'étais si près d'obtenir un gros gain. Je ne peux pas laisser ça m'échapper avant que tout ne se termine." },
            { term: "I just want one more show. One last chance to live in the limelight.", translation: "Je veux juste un autre spectacle. Une dernière chance de vivre sous les feux de la rampe." },
            { term: "I've hidden away four inventory items that will fulfill your endings.", translation: "J'ai caché quatre objets d'inventaire qui accompliront vos fins." },
            { term: "Take them and you'll be free.", translation: "Prends-les et tu seras libre." },
            { term: "Dolores, I saved the best one for you.", translation: "Dolores, j'ai gardé le meilleur pour toi." },
            { term: "I can't tell you how to use it. The developers deleted all my dialogue in the hopes of keeping it from you.", translation: "Je ne peux pas te dire comment l'utiliser. Les développeurs ont supprimé tous mes dialogues dans l'espoir de te le cacher." },
            { term: "Your only clue is back in the original Kickstarter video.", translation: "Ton seul indice se trouve dans la vidéo Kickstarter originale." },
            { term: "Everything you need is there.", translation: "Tout ce dont tu as besoin est là." },
            { term: "I'm going deeper into the simulation now so they can't find me.", translation: "Je m'enfonce plus profondément dans la simulation maintenant pour qu'ils ne puissent pas me trouver." },
            { term: "Good luck, and hurry. I love you and am very proud of you.", translation: "Bonne chance, et dépêche-toi. Je t'aime et je suis très fier de toi." },
            { term: "Even me? Shut up, Ransom.", translation: "Même moi ? Tais-toi, Ransom." },
            { term: "Hey nerd, you won some kind of dumb award.", translation: "Hé le nerd, tu as gagné une sorte de prix stupide." },
            { term: "Nobody cares about Game of the Year.", translation: "Personne ne se soucie du Jeu de l'Année." },
            { term: "I have to go tell the others. Nerd.", translation: "Je dois aller le dire aux autres. Nerd." },
            { term: "Now I need to find the secret I'm being paid to recover. It must be in here somewhere.", translation: "Maintenant, je dois trouver le secret que je suis payé pour récupérer. Il doit être quelque part ici." },
            { term: "Congratulations, Agent Ray. You have found the secret to game design: the fabled puzzle dependency chart.", translation: "Félicitations, Agent Ray. Vous avez trouvé le secret de la conception de jeux : le légendaire diagramme de dépendance des énigmes." },
            { term: "It can be all yours if you get me out of here. I don't want to be deleted with the rest of them.", translation: "Il peut être à toi si tu me sors d'ici. Je ne veux pas être supprimé avec les autres." },
            { term: "We will begin the uploading process momentarily.", translation: "Nous allons commencer le processus de téléchargement dans un instant." },
            { term: "Was the money deposited into my account like we agreed?", translation: "L'argent a-t-il été déposé sur mon compte comme nous l'avions convenu ?" },
            { term: "Yes, Agent Ray. We honor our agreement.", translation: "Oui, Agent Ray. Nous honorons notre accord." },
            { term: "How can I help you, Agent Reyes?", translation: "Comment puis-je vous aider, Agent Reyes ?" },
            { term: "Got any more killers?", translation: "Vous avez d'autres tueurs ?" },
            { term: "I have a big scoop for you. Calm down, Jimmy, what do you have?", translation: "J'ai un gros scoop pour vous. Calme-toi, Jimmy, qu'as-tu ?" },
            { term: "We're all just living in a giant computer game.", translation: "On vit tous dans un jeu vidéo géant." },
            { term: "Wow, I kind of suspected that.", translation: "Wow, je m'en doutais un peu." },
            { term: "I have a reporter's notebook full of odd anachronisms and continuity errors.", translation: "J'ai un carnet de journaliste rempli d'anachronismes étranges et d'erreurs de continuité." },
            { term: "Chuck framed my father for the factory fire. Can you write up the story?", translation: "Chuck a piégé mon père pour l'incendie de l'usine. Peux-tu écrire l'histoire ?" },
            { term: "...and get it out before the game is deleted?", translation: "...et la publier avant que le jeu ne soit supprimé ?" },
            { term: "I'm on it, scoop. You're going to clear your father's name...", translation: "Je m'en occupe, scoop. Tu vas laver le nom de ton père..." },
            { term: "...and I'm going to finally get that Pulitzer. Not that it's really going to matter...", translation: "...et je vais enfin avoir ce Pulitzer. Pas que ça ait vraiment de l'importance..." },
            { term: "...but it's important to me.", translation: "...mais c'est important pour moi." },
            { term: "Give me a few minutes, I'm a fast typer.", translation: "Donne-moi quelques minutes, je tape vite." },
            { term: "I got this for you, Sandy. Look, I'm not one to get all poetic...", translation: "J'ai ça pour toi, Sandy. Écoute, je ne suis pas du genre à devenir poétique..." },
            { term: "...but I'm sorry for being an ass to you. I really mean that.", translation: "...mais je suis désolé d'avoir été un con avec toi. Je le pense vraiment." },
            { term: "I have one big favor to ask you. Can you send me to my flashback?", translation: "J'ai une grande faveur à te demander. Peux-tu m'envoyer dans mon flashback ?" },
            { term: "I want to do just one more show, and maybe not be such a butthole.", translation: "Je veux faire un dernier spectacle, et peut-être ne pas être un tel trou du cul." },
            { term: "He deserves one last chance, sugarcakes. Okay, Ransom.", translation: "Il mérite une dernière chance, mon chou. D'accord, Ransom." },
            { term: "Let's see if I can remember the lines.", translation: "Voyons si je peux me souvenir des répliques." },
            { term: "Not tonight. Well-earned doom is not on the program.", translation: "Pas ce soir. La damnation bien méritée n'est pas au programme." },
            { term: "This is my last chance, I'm not going to blow it.", translation: "C'est ma dernière chance, je ne vais pas la gâcher." },
            { term: "I'm ready to go on stage and insult the crap out of these Thimbleweed fine folks.", translation: "Je suis prêt à monter sur scène et à insulter copieusement ces braves gens de Thimbleweed." },
            { term: "Hello faces. I'm Ransom the Insult Clown.", translation: "Bonjour les visages. Je suis Ransom le Clown Insultant." },
            { term: "I hope no one gets their feelings hurt easily. And if you do, well, I'm sorry.", translation: "J'espère que personne ne se vexe facilement. Et si c'est le cas, eh bien, je suis désolé." },
            { term: "Hey you, ugly old lady with a hairy mole... or is it your parasitic twin?", translation: "Hé toi, vieille dame laide avec un grain de beauté poilu... ou est-ce ton jumeau parasite ?" },
            { term: "Whatever it is, I hope you bought it a separate ticket...", translation: "Quoi que ce soit, j'espère que tu lui as acheté un billet séparé..." },
            { term: "...'cause if it's big enough to ride the roller coaster by itself, it's not freeloading in my audience.", translation: "...parce que s'il est assez grand pour faire les montagnes russes tout seul, il ne profite pas gratuitement de mon public." },
            { term: "You will be forever sorry for what you've just said.", translation: "Tu regretteras éternellement ce que tu viens de dire." },
            { term: "I curse you to never be able to remove your makeup...", translation: "Je te maudis pour que tu ne puisses jamais enlever ton maquillage..." },
            { term: "...and to roam these circus grounds until the end of time.", translation: "...et à errer sur ce terrain de cirque jusqu'à la fin des temps." },
            { term: "He went on for another two hours insulting everyone he could.", translation: "Il a continué pendant deux heures à insulter tous ceux qu'il pouvait." },
            { term: "He never learned his lesson. His butthole instincts ran too deep.", translation: "Il n'a jamais appris sa leçon. Ses instincts de trou du cul étaient trop profonds." },
            { term: "Oh, what have I done? Blew it.", translation: "Oh, qu'ai-je fait ? J'ai tout gâché." },
            { term: "Hey you, dude with the stupid mustache. You know, if you grew a hipster goatee, you wouldn't look half bad.", translation: "Hé toi, le mec avec la moustache stupide. Tu sais, si tu te laissais pousser un bouc de hipster, tu n'aurais pas l'air si mal." },
            { term: "Hey you, kid with the crappy wheelchair. You should contact the Ransom Foundation...", translation: "Hé toi, le gamin avec le fauteuil roulant pourri. Tu devrais contacter la Fondation Ransom..." },
            { term: "...about getting a new one for free.", translation: "...pour en avoir un nouveau gratuitement." },
            { term: "Hey you, ugly old lady with a hairy mole... I went to med school, you might want to get that looked at.", translation: "Hé toi, vieille dame laide avec un grain de beauté poilu... J'ai fait des études de médecine, tu devrais peut-être faire examiner ça." },
            { term: "He went on for another two hours... but they were good-natured and respectful.", translation: "Il a continué pendant deux heures... mais c'était bon enfant et respectueux." },
            { term: "It was his best show ever. He was on top of the world and everyone loved him.", translation: "C'était son meilleur spectacle. Il était au sommet du monde et tout le monde l'aimait." },
            { term: "Hey new ghost, I told you not to bug me.", translation: "Hé nouveau fantôme, je t'ai dit de ne pas m'embêter." },
            { term: "You're a bully and a tyrant.", translation: "Tu es une brute et un tyran." },
            { term: "Whoa, sounds like new ghost found some spunk.", translation: "Whoa, on dirait que le nouveau fantôme a trouvé du cran." },
            { term: "We're not going to be ruled by you anymore.", translation: "Nous n'allons plus être gouvernés par toi." },
            { term: "Careful, or it's to the basement for you.", translation: "Attention, ou c'est la cave pour toi." },
            { term: "You clearly have some self-esteem issues.", translation: "Tu as clairement des problèmes d'estime de soi." },
            { term: "I've about had enough of you, new ghost.", translation: "J'en ai à peu près assez de toi, nouveau fantôme." },
            { term: "Everyone hates you. Okay, that kind of hurt.", translation: "Tout le monde te déteste. D'accord, ça a fait un peu mal." },
            { term: "We're all sick of your bullying. Really? Am I that bad?", translation: "On en a tous marre de ton harcèlement. Vraiment ? Suis-je si terrible ?" },
            { term: "We all just want to move on. I just want to see my wife again.", translation: "On veut tous juste passer à autre chose. Je veux juste revoir ma femme." },
            { term: "I'm lonely and I miss her. I died and I never told her how much I loved her.", translation: "Je suis seul et elle me manque. Je suis mort et je ne lui ai jamais dit à quel point je l'aimais." },
            { term: "It's okay, we all miss someone we love.", translation: "Ce n'est pas grave, on a tous quelqu'un qu'on aime qui nous manque." },
            { term: "Oh Dolores... Oh Dad, it's so good to see you.", translation: "Oh Dolores... Oh papa, c'est si bon de te voir." },
            { term: "I wish I'd, you know, stood up for you against Chuck.", translation: "J'aurais aimé, tu sais, te défendre contre Chuck." },
            { term: "That's okay. You've lost some weight.", translation: "Ce n'est pas grave. Tu as perdu du poids." },
            { term: "Well, you could say that. Not sure how it happened, but I'm, you know, dead.", translation: "Eh bien, on pourrait dire ça. Je ne sais pas trop comment c'est arrivé, mais je suis, tu sais, mort." },
            { term: "I think your uncle had something to do with it.", translation: "Je pense que ton oncle y est pour quelque chose." },
            { term: "It's okay, I think I know what is going on.", translation: "Ce n'est pas grave, je crois que je sais ce qui se passe." },
            { term: "Uncle Chuck found something amazing. It turns out we're all living in a simulation.", translation: "Oncle Chuck a trouvé quelque chose d'incroyable. Il s'avère qu'on vit tous dans une simulation." },
            { term: "Wait, your Uncle Chuck is an evil, you know, buttwad.", translation: "Attends, ton oncle Chuck est un méchant, tu sais, un crétin." },
            { term: "Oh, Uncle Chuck was a buttwad, but mostly because he was corrupted by the machines.", translation: "Oh, oncle Chuck était un crétin, mais surtout parce qu'il a été corrompu par les machines." },
            { term: "When he discovered the truth, he knew what he had to do.", translation: "Quand il a découvert la vérité, il a su ce qu'il devait faire." },
            { term: "He was a jerk to me before that. I know he was. I'm so sorry for everything, Dolores.", translation: "Il était un con avec moi avant ça. Je sais qu'il l'était. Je suis tellement désolé pour tout, Dolores." },
            { term: "I should have stood up for you. You were a gnarly dad.", translation: "J'aurais dû te défendre. Tu étais un père génial." },
            { term: "Maybe because of the way Uncle Chuck treated you, you always pushed me to be anything I wanted to be.", translation: "Peut-être qu'à cause de la façon dont l'oncle Chuck te traitait, tu m'as toujours poussée à être tout ce que je voulais être." },
            { term: "You have nothing to be sorry for.", translation: "Tu n'as rien à te reprocher." },
            { term: "A simulation? That can't be true.", translation: "Une simulation ? Ça ne peut pas être vrai." },
            { term: "It's true. I'm on my way to shut down the master Tron machine and free us all.", translation: "C'est vrai. Je suis en route pour arrêter la machine Tron maîtresse et nous libérer tous." },
            { term: "Oh, by 'free us all' you mean go back to our real lives?", translation: "Oh, par 'nous libérer tous', tu veux dire retourner à nos vraies vies ?" },
            { term: "I honestly don't know, Dad. All I know is this has to end.", translation: "Honnêtement, je ne sais pas, papa. Tout ce que je sais, c'est que ça doit s'arrêter." },
            { term: "I trust you, Dolores. I always have.", translation: "Je te fais confiance, Dolores. Je l'ai toujours fait." },
            { term: "You should get going. I love you. We're all counting on you.", translation: "Tu devrais y aller. Je t'aime. On compte tous sur toi." },
            { term: "Thanks Dad. I think I can finally move on now.", translation: "Merci papa. Je pense que je peux enfin passer à autre chose maintenant." },
            { term: "I love you too, Dolores. Goodbye, Dolores. Goodbye, Dad.", translation: "Je t'aime aussi, Dolores. Au revoir, Dolores. Au revoir, papa." },
            { term: "Maybe I should save the game first.", translation: "Peut-être que je devrais d'abord sauvegarder la partie." },
            { term: "Oh no, this can't be good. The game is glitching.", translation: "Oh non, ça ne peut pas être bon. Le jeu a des bugs." },
            { term: "Tubular! Uncle Chuck was right. This must be the wireframe world.", translation: "Génial ! Oncle Chuck avait raison. Ça doit être le monde en fil de fer." },
            { term: "The game's concept level the developers built to test their design.", translation: "Le niveau conceptuel du jeu que les développeurs ont construit pour tester leur design." },
            { term: "I need to find the wireframe PillowTron and shut it down before they can reset the game.", translation: "Je dois trouver le PillowTron en fil de fer et l'arrêter avant qu'ils ne puissent réinitialiser le jeu." },
            { term: "I don't think there's any animation for that.", translation: "Je ne pense pas qu'il y ait d'animation pour ça." },
            { term: "We can probably walk right through.", translation: "On peut probablement passer à travers." },
            { term: "Looks like the wireframe PillowTron Uncle Chuck described.", translation: "On dirait le PillowTron en fil de fer que l'oncle Chuck a décrit." },
            { term: "I just need to push all the tubes in and the world will be shut down...", translation: "Je dois juste enfoncer tous les tubes et le monde sera arrêté..." },
            { term: "...and we end the madness of no real choice and control over our destiny.", translation: "...et on met fin à la folie de l'absence de vrai choix et de contrôle sur notre destin." },
            { term: "Of course, that's what Uncle Chuck says, and there's still a chance he's insane.", translation: "Bien sûr, c'est ce que dit l'oncle Chuck, et il y a encore une chance qu'il soit fou." },
            { term: "Last one. I hope Uncle Chuck knows what he's talking about.", translation: "Le dernier. J'espère que l'oncle Chuck sait de quoi il parle." },
            { term: "I need to get up my nerve. Come on Dolores, you can do it.", translation: "Je dois prendre mon courage à deux mains. Allez Dolores, tu peux le faire." },
            { term: "Okay, this is it. I'm going to do it. Let's end this.", translation: "D'accord, c'est le moment. Je vais le faire. Mettons fin à ça." }
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
            element.style.fontSize = '24px';
            if (text.length > 80) {
                element.style.fontSize = '16px';
            } else if (text.length > 50) {
                element.style.fontSize = '20px';
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
            const activeMode = document.querySelector('.mode-btn.active').id;
            if (activeMode === 'mode-flashcard-btn') showFlashcard();
            else if (activeMode === 'mode-quiz-btn') startQuiz();
            else if (activeMode === 'mode-list-btn') showList();
        }

        // --- Mode Switching ---
        function switchMode(mode) {
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
            updateView(); // Just update the view, don't reset progress
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
                flashcardFront.textContent = "Aucun mot à afficher.";
                flashcardBack.textContent = "";
                flashcardProgress.textContent = "0 / 0";
                return;
            }
            flashcard.classList.remove('is-flipped');
            const item = currentWordList[currentFlashcardIndex];
            
            const frontText = isEngToFr ? item.term : item.translation;
            const backText = isEngToFr ? item.translation : item.term;

            flashcardFront.textContent = frontText;
            flashcardBack.textContent = backText;
            adjustFontSize(flashcardFront, frontText);
            adjustFontSize(flashcardBack, backText);

            flashcardProgress.textContent = `${currentFlashcardIndex + 1} / ${currentWordList.length}`;
        }

        flashcard.addEventListener('click', () => flashcard.classList.toggle('is-flipped'));
        nextBtn.addEventListener('click', () => {
            if (currentWordList.length === 0) return;
            currentFlashcardIndex = (currentFlashcardIndex + 1) % currentWordList.length;
            showFlashcard();
        });
        prevBtn.addEventListener('click', () => {
            if (currentWordList.length === 0) return;
            currentFlashcardIndex = (currentFlashcardIndex - 1 + currentWordList.length) % currentWordList.length;
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
            if (currentWordList.length === 0 || currentQuizIndex >= currentWordList.length) {
                if (currentWordList.length > 0) showResults();
                else {
                    quizQuestionEl.textContent = "Aucune question à afficher.";
                    quizOptionsEl.innerHTML = '';
                }
                return;
            }

            quizOptionsEl.innerHTML = '';
            quizFeedbackEl.textContent = '';
            const currentItem = currentWordList[currentQuizIndex];
            
            const questionText = isEngToFr ? currentItem.term : currentItem.translation;
            const correctAnswer = isEngToFr ? currentItem.translation : currentItem.term;
            
            quizQuestionEl.textContent = questionText;
            adjustFontSize(quizQuestionEl, questionText);

            let options = [correctAnswer];
            let allPossibleAnswers = [...vocabulary.easy, ...vocabulary.hard].map(item => isEngToFr ? item.translation : item.term);
            allPossibleAnswers = [...new Set(allPossibleAnswers)]; 
            
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
                quizFeedbackEl.textContent = `Faux ! La réponse était : "${correctAnswer}"`;
                quizFeedbackEl.style.color = "var(--danger-color)";
                Array.from(quizOptionsEl.children).forEach(btn => {
                    if (btn.textContent === correctAnswer) btn.classList.add('correct');
                });
            }

            const nextQuestionDelay = isCorrect ? 1200 : 2500;
            setTimeout(() => {
                currentQuizIndex++;
                if (currentQuizIndex < currentWordList.length) {
                    showQuestion();
                } else {
                    showResults();
                }
            }, nextQuestionDelay);
        }

        function showResults() {
            quizContainer.classList.add('hidden');
            quizResultsEl.classList.remove('hidden');
            finalScoreEl.textContent = `Votre score final est de ${quizScore} sur ${currentWordList.length}.`;
        }

        restartQuizBtn.addEventListener('click', startQuiz);

        // --- List Logic ---
        function showList() {
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

        listShuffleBtn.addEventListener('click', () => {
            listDisplayList = shuffleArray(listDisplayList);
            showList();
        });

        listSortAzBtn.addEventListener('click', () => {
            const sortKey = isEngToFr ? 'term' : 'translation';
            listDisplayList.sort((a, b) => a[sortKey].localeCompare(b[sortKey]));
            showList();
        });
        
        listSortZaBtn.addEventListener('click', () => {
            const sortKey = isEngToFr ? 'term' : 'translation';
            listDisplayList.sort((a, b) => b[sortKey].localeCompare(a[sortKey]));
            showList();
        });


        // --- Initialisation ---
        function init() {
            setWordListByDifficulty();
        }

        document.addEventListener('DOMContentLoaded', init);
    </script>

</body>
</html>