---
title: Manhattan (1979)
layout: default
---
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Manhattan (1979)</title>
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
        <h1>Manhattan (1979)</h1>
        
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
            { term: "He adored", translation: "Il adorait" },
            { term: "No matter what", translation: "Peu importe ce que" },
            { term: "Let me start this over", translation: "Laissez-moi recommencer" },
            { term: "Too corny", translation: "Trop banal / ringard" },
            { term: "Face it", translation: "Rends-toi à l'évidence" },
            { term: "I wanna sell some books", translation: "Je veux vendre des livres" },
            { term: "Too angry", translation: "Trop en colère" },
            { term: "I love this", translation: "J'adore ça" },
            { term: "It always would be", translation: "Ça le serait toujours" },
            { term: "Talent is luck", translation: "Le talent, c'est de la chance" },
            { term: "The important thing", translation: "La chose importante" },
            { term: "To dive into", translation: "Plonger dans" },
            { term: "A key question", translation: "Une question clé" },
            { term: "I can't swim", translation: "Je ne sais pas nager" },
            { term: "Oh, God", translation: "Oh, mon Dieu" },
            { term: "You don't smoke", translation: "Tu ne fumes pas" },
            { term: "It gives you cancer", translation: "Ça donne le cancer" },
            { term: "You like the way I look?", translation: "Tu aimes mon apparence ?" },
            { term: "Excuse me a sec", translation: "Excusez-moi une seconde" },
            { term: "She's gorgeous", translation: "Elle est magnifique" },
            { term: "I'm dating a girl", translation: "Je sors avec une fille" },
            { term: "Can you believe that?", translation: "Tu peux croire ça ?" },
            { term: "My ex-wife", translation: "Mon ex-femme" },
            { term: "That's really tacky", translation: "C'est vraiment de mauvais goût" },
            { term: "It's really depressing", translation: "C'est vraiment déprimant" },
            { term: "I have anything to hide", translation: "J'ai quelque chose à cacher" },
            { term: "You should never drink", translation: "Tu ne devrais jamais boire" },
            { term: "I know", translation: "Je sais" },
            { term: "I've got an exam tomorrow", translation: "J'ai un examen demain" },
            { term: "She's got homework", translation: "Elle a des devoirs" },
            { term: "What's the matter with you?", translation: "Qu'est-ce qui ne va pas avec toi ?" },
            { term: "A million miles away", translation: "À des millions de kilomètres" },
            { term: "I have something I wanna say", translation: "J'ai quelque chose que je veux dire" },
            { term: "I met a woman there", translation: "J'y ai rencontré une femme" },
            { term: "You're kidding?", translation: "Tu plaisantes ?" },
            { term: "It started out very casually", translation: "Ça a commencé de manière très décontractée" },
            { term: "It's getting out of hand", translation: "Ça devient hors de contrôle" },
            { term: "I don't know what to do", translation: "Je ne sais pas quoi faire" },
            { term: "It's scary", translation: "Ça fait peur" },
            { term: "Who is she?", translation: "Qui est-ce ?" },
            { term: "What are the details?", translation: "Quels sont les détails ?" },
            { term: "She's very beautiful", translation: "Elle est très belle" },
            { term: "It sounds wonderful", translation: "Ça a l'air merveilleux" },
            { term: "She's on my mind", translation: "Je pense à elle" },
            { term: "How serious is it?", translation: "À quel point est-ce sérieux ?" },
            { term: "It's pretty serious", translation: "C'est assez sérieux" },
            { term: "God, no", translation: "Mon Dieu, non" },
            { term: "I love her", translation: "Je l'aime" },
            { term: "I hate myself", translation: "Je me déteste" },
            { term: "This is not like that", translation: "Ce n'est pas comme ça" },
            { term: "You shouldn't ask me for advice", translation: "Tu ne devrais pas me demander conseil" },
            { term: "I think she's terrific", translation: "Je pense qu'elle est géniale" },
            { term: "He could do a lot worse", translation: "Il pourrait faire bien pire" },
            { term: "He has done a lot worse", translation: "Il a fait bien pire" },
            { term: "Wasting his life", translation: "Gâcher sa vie" },
            { term: "That crap for television", translation: "Ces merdes pour la télévision" },
            { term: "Having kids", translation: "Avoir des enfants" },
            { term: "Oh, my God", translation: "Oh, mon Dieu" },
            { term: "It's not practical", translation: "Ce n'est pas pratique" },
            { term: "All my stuff's here", translation: "Toutes mes affaires sont ici" },
            { term: "It's just the wrong time", translation: "Ce n'est juste pas le bon moment" },
            { term: "Leave me alone", translation: "Laisse-moi tranquille" },
            { term: "I'm free to do as I please", translation: "Je suis libre de faire ce qui me plaît" },
            { term: "This affects me", translation: "Ça me concerne" },
            { term: "Do you spy on me?", translation: "Est-ce que tu m'espionnes ?" },
            { term: "I don't care to discuss it", translation: "Ça ne m'intéresse pas d'en discuter" },
            { term: "How's Willie?", translation: "Comment va Willie ?" },
            { term: "It's an honest account", translation: "C'est un récit honnête" },
            { term: "You're so threatened", translation: "Tu te sens tellement menacé" },
            { term: "It's mind-boggling", translation: "C'est ahurissant" },
            { term: "Don't get carried away", translation: "Ne t'emballe pas" },
            { term: "We're having a great time", translation: "On passe un super moment" },
            { term: "You're a kid", translation: "Tu es un enfant" },
            { term: "You've got your whole life ahead of you", translation: "Tu as toute la vie devant toi" },
            { term: "How can you ask that?", translation: "Comment peux-tu demander ça ?" },
            { term: "Get dressed", translation: "Habille-toi" },
            { term: "You gotta go", translation: "Tu dois partir" },
            { term: "Don't you want me to stay over?", translation: "Tu ne veux pas que je reste dormir ?" },
            { term: "It's not a great idea", translation: "Ce n'est pas une bonne idée" },
            { term: "Believe me", translation: "Crois-moi" },
            { term: "Are you joking with me?", translation: "Tu te moques de moi ?" },
            { term: "Of course I'm joking!", translation: "Bien sûr que je plaisante !" },
            { term: "It's fun", translation: "C'est amusant" },
            { term: "Are you kidding me?", translation: "Tu te moques de moi ?" },
            { term: "You should talk!", translation: "C'est l'hôpital qui se moque de la charité !" },
            { term: "Thanks a lot", translation: "Merci beaucoup" },
            { term: "How long have you been here?", translation: "Depuis quand es-tu là ?" },
            { term: "We were talking about you", translation: "On parlait de toi" },
            { term: "That's hilarious", translation: "C'est hilarant" },
            { term: "How are you?", translation: "Comment vas-tu ?" },
            { term: "Nice to meet you", translation: "Ravi de vous rencontrer" },
            { term: "It's really good", translation: "C'est vraiment bien" },
            { term: "You liked the Plexiglas?", translation: "Tu as aimé le Plexiglas ?" },
            { term: "That was the worst", translation: "C'était le pire" },
            { term: "You know what I mean?", translation: "Tu vois ce que je veux dire ?" },
            { term: "The rest of the stuff was bullshit", translation: "Le reste, c'était de la merde" },
            { term: "That'd be fun", translation: "Ce serait amusant" },
            { term: "I go to high school", translation: "Je vais au lycée" },
            { term: "Oh, really", translation: "Oh, vraiment" },
            { term: "Get her away from me", translation: "Éloignez-la de moi" },
            { term: "It was very nice meeting you", translation: "C'était très agréable de vous rencontrer" },
            { term: "We have to go", translation: "Nous devons partir" },
            { term: "What a creep", translation: "Quel sale type" },
            { term: "She seemed nervous", translation: "Elle avait l'air nerveuse" },
            { term: "Why are you getting so mad?", translation: "Pourquoi t'énerves-tu autant ?" },
            { term: "I don't like that", translation: "Je n'aime pas ça" },
            { term: "A sucker for those kind of women", translation: "Un faible pour ce genre de femmes" },
            { term: "I don't believe in", translation: "Je ne crois pas en" },
            { term: "People should mate for life", translation: "Les gens devraient s'accoupler pour la vie" },
            { term: "Get the groceries", translation: "Va chercher les courses" },
            { term: "This is the worst", translation: "C'est le pire" },
            { term: "It's not funny", translation: "Ce n'est pas drôle" },
            { term: "That's funny", translation: "C'est drôle" },
            { term: "Take a lude", translation: "Prends un calmant" },
            { term: "I quit", translation: "J'arrête / Je démissionne" },
            { term: "You're being silly", translation: "Tu es stupide" },
            { term: "I made a terrible mistake", translation: "J'ai fait une terrible erreur" },
            { term: "The first smart thing you've done", translation: "La première chose intelligente que tu aies faite" },
            { term: "I've screwed myself up", translation: "Je me suis fichu en l'air" },
            { term: "If you need money", translation: "Si tu as besoin d'argent" },
            { term: "That's not the point", translation: "Là n'est pas la question" },
            { term: "I gotta cut down", translation: "Je dois réduire (mes dépenses)" },
            { term: "Give up my apartment", translation: "Abandonner mon appartement" },
            { term: "It'll kill my father", translation: "Ça va tuer mon père" },
            { term: "What am I...?", translation: "Qu'est-ce que je...?" },
            { term: "It's ridiculous", translation: "C'est ridicule" },
            { term: "Your book is gonna be wonderful", translation: "Ton livre va être merveilleux" },
            { term: "I'm proud of you", translation: "Je suis fier de toi" },
            { term: "This is a good move", translation: "C'est une bonne décision" },
            { term: "Congratulations on your book", translation: "Félicitations pour ton livre" },
            { term: "It was terrific", translation: "C'était génial" },
            { term: "What are you doing here?", translation: "Qu'est-ce que tu fais ici ?" },
            { term: "I'm sorry", translation: "Je suis désolé" },
            { term: "It's all right", translation: "Ce n'est rien / Tout va bien" },
            { term: "I heard you quit your job", translation: "J'ai entendu dire que tu avais quitté ton travail" },
            { term: "Get right to the point", translation: "Aller droit au but" },
            { term: "You have to forgive Dennis", translation: "Vous devez pardonner à Dennis" },
            { term: "Good night", translation: "Bonne nuit" },
            { term: "Same here", translation: "Moi aussi / Pareil" },
            { term: "Bye-bye", translation: "Au revoir" },
            { term: "She's a brilliant woman", translation: "C'est une femme brillante" },
            { term: "She's a genius", translation: "C'est un génie" },
            { term: "How come you guys got divorced?", translation: "Comment se fait-il que vous ayez divorcé ?" },
            { term: "I hardly know you", translation: "Je vous connais à peine" },
            { term: "We fought a lot", translation: "On se disputait beaucoup" },
            { term: "What kind of dog you got?", translation: "Quelle sorte de chien as-tu ?" },
            { term: "The worst", translation: "Le pire" },
            { term: "Are you in a rush?", translation: "Es-tu pressé ?" },
            { term: "What do you mean?", translation: "Que veux-tu dire ?" },
            { term: "I'd like to hear about your book", translation: "J'aimerais que tu me parles de ton livre" },
            { term: "Yeah?", translation: "Ouais ?" },
            { term: "My book is about...", translation: "Mon livre parle de..." },
            { term: "Isn't it beautiful out?", translation: "N'est-ce pas qu'il fait beau dehors ?" },
            { term: "I know. I love it.", translation: "Je sais. J'adore ça." },
            { term: "This is really a great city", translation: "C'est vraiment une ville géniale" },
            { term: "I don't care what anybody says", translation: "Je me fiche de ce que les gens disent" },
            { term: "It's a knockout", translation: "C'est une bombe / C'est canon" },
            { term: "I better head back", translation: "Je ferais mieux de rentrer" },
            { term: "I'm awake", translation: "Je suis réveillé" },
            { term: "What are you doing?", translation: "Qu'est-ce que tu fais ?" },
            { term: "Oh, yeah", translation: "Oh, ouais" },
            { term: "You did?", translation: "Vraiment ?" },
            { term: "You still feel the same way?", translation: "Tu ressens toujours la même chose ?" },
            { term: "I gotta go", translation: "Je dois y aller" },
            { term: "Come on", translation: "Allez" },
            { term: "I missed you so much", translation: "Tu m'as tellement manqué" },
            { term: "You're married", translation: "Tu es marié" },
            { term: "It sounds terrible", translation: "Ça a l'air terrible" },
            { term: "I hate it", translation: "Je déteste ça" },
            { term: "I don't wanna break up a marriage", translation: "Je ne veux pas briser un mariage" },
            { term: "It's crazy", translation: "C'est fou" },
            { term: "What do you want me to do?", translation: "Qu'est-ce que tu veux que je fasse ?" },
            { term: "Nothing", translation: "Rien" },
            { term: "I don't know", translation: "Je ne sais pas" },
            { term: "Please stop it", translation: "S'il te plaît, arrête" },
            { term: "Someone's gonna see us", translation: "Quelqu'un va nous voir" },
            { term: "Not now!", translation: "Pas maintenant !" },
            { term: "I'm a pushover!", translation: "Je suis une bonne poire !" },
            { term: "Hi, Isaac", translation: "Salut, Isaac" },
            { term: "Come on in", translation: "Entre" },
            { term: "How you been?", translation: "Comment vas-tu ?" },
            { term: "Good", translation: "Bien" },
            { term: "I've been terrific", translation: "J'ai été super" },
            { term: "Things are going really well", translation: "Les choses se passent vraiment bien" },
            { term: "Want some coffee?", translation: "Tu veux du café ?" },
            { term: "Excuse me", translation: "Excusez-moi" },
            { term: "Can I talk to you a minute?", translation: "Puis-je te parler une minute ?" },
            { term: "You can't understand?", translation: "Tu ne peux pas comprendre ?" },
            { term: "You knew my history!", translation: "Tu connaissais mon passé !" },
            { term: "You look funny", translation: "Tu as l'air bizarre" },
            { term: "Do you miss me?", translation: "Est-ce que je te manque ?" },
            { term: "Of course I miss you", translation: "Bien sûr que tu me manques" },
            { term: "I love you", translation: "Je t'aime" },
            { term: "That's why...", translation: "C'est pourquoi..." },
            { term: "I'm serious", translation: "Je suis sérieux" },
            { term: "I think I should...", translation: "Je pense que je devrais..." },
            { term: "Nothing's wrong", translation: "Rien ne va mal" },
            { term: "It was just a shot", translation: "C'était juste une tentative" },
            { term: "I won't keep you", translation: "Je ne te retiendrai pas" },
            { term: "OK. Bye-bye.", translation: "OK. Au revoir." },
            { term: "Not at all", translation: "Pas du tout" },
            { term: "How you doin'?", translation: "Comment ça va ?" },
            { term: "You wanna go for a walk?", translation: "Tu veux aller te promener ?" },
            { term: "It's such a beautiful Sunday", translation: "C'est un si beau dimanche" },
            { term: "It's an electrical storm", translation: "C'est un orage électrique" },
            { term: "I'm soaking wet", translation: "Je suis trempé" },
            { term: "This is awful!", translation: "C'est horrible !" },
            { term: "You look ridiculous!", translation: "Tu as l'air ridicule !" },
            { term: "Next time", translation: "La prochaine fois" },
            { term: "I can't see", translation: "Je ne vois pas" },
            { term: "You're sort of pretty", translation: "Tu es plutôt jolie" },
            { term: "I'm really annoyed with", translation: "Je suis vraiment agacé par" },
            { term: "Why?", translation: "Pourquoi ?" },
            { term: "That's what happens when...", translation: "C'est ce qui arrive quand..." },
            { term: "You're having an affair", translation: "Tu as une liaison" },
            { term: "Hey, I didn't put it that way", translation: "Hé, ce n'est pas moi qui l'ai dit comme ça" },
            { term: "I don't agree at all", translation: "Je ne suis pas du tout d'accord" },
            { term: "You're fine", translation: "Tu vas bien" },
            { term: "Are you kidding?", translation: "Tu plaisantes ?" },
            { term: "I think you're terrific", translation: "Je pense que tu es formidable" },
            { term: "You're very insecure", translation: "Tu manques beaucoup d'assurance" },
            { term: "I think you're wonderful, really", translation: "Je pense que tu es merveilleuse, vraiment" },
            { term: "Grab a bite", translation: "Manger un morceau" },
            { term: "OK. OK.", translation: "D'accord. D'accord." },
            { term: "Are you OK?", translation: "Est-ce que ça va ?" },
            { term: "Yeah, I'm fine", translation: "Oui, je vais bien" },
            { term: "I feel good", translation: "Je me sens bien" },
            { term: "Come on", translation: "Allez" },
            { term: "It's not too crowded", translation: "Il n'y a pas trop de monde" },
            { term: "Not bad for Sunday", translation: "Pas mal pour un dimanche" },
            { term: "I thought it'd be jammed", translation: "Je pensais que ce serait bondé" },
            { term: "You look adorable", translation: "Tu es adorable" },
            { term: "I have a chance to go to London", translation: "J'ai l'occasion d'aller à Londres" },
            { term: "When did this happen?", translation: "Quand est-ce que c'est arrivé ?" },
            { term: "The other day", translation: "L'autre jour" },
            { term: "That's great. That's terrific.", translation: "C'est super. C'est formidable." },
            { term: "I don't wanna go without you", translation: "Je ne veux pas y aller sans toi" },
            { term: "I can't go", translation: "Je ne peux pas y aller" },
            { term: "Of course you should go", translation: "Bien sûr que tu devrais y aller" },
            { term: "You'll have a great time", translation: "Tu vas passer un super moment" },
            { term: "So what happens to us?", translation: "Alors, qu'est-ce qui nous arrive ?" },
            { term: "I'm kidding", translation: "Je plaisante" },
            { term: "What kind of question is that?", translation: "Quelle sorte de question est-ce ?" },
            { term: "Thank you", translation: "Merci" },
            { term: "It's absurd", translation: "C'est absurde" },
            { term: "Anything?", translation: "N'importe quoi ?" },
            { term: "Absolutely anything", translation: "Absolument n'importe quoi" },
            { term: "OK, I know what we can do", translation: "OK, je sais ce qu'on peut faire" },
            { term: "Shut up", translation: "Tais-toi" },
            { term: "This is so corny", translation: "C'est tellement ringard" },
            { term: "I can't believe this", translation: "Je ne peux pas croire ça" },
            { term: "I think it's fun!", translation: "Je trouve ça amusant !" },
            { term: "I think it's great", translation: "Je trouve ça super" },
            { term: "Quit fighting it", translation: "Arrête de te battre contre ça" }
          ],
          hard: [
            { term: "He idolised it all out of proportion", translation: "Il l'idolâtrait de manière démesurée" },
            { term: "He romanticised it", translation: "Il l'a romancé" },
            { term: "Pulsated to the great tunes", translation: "Pulsait au rythme des grands airs" },
            { term: "The hustle, bustle of the crowds", translation: "L'agitation, le remue-ménage de la foule" },
            { term: "Street-smart guys", translation: "Des gars débrouillards / qui connaissent la rue" },
            { term: "Who seemed to know all the angles", translation: "Qui semblaient connaître toutes les combines" },
            { term: "Make it more profound", translation: "Le rendre plus profond" },
            { term: "A metaphor for the decay", translation: "Une métaphore de la décadence" },
            { term: "The same lack of integrity", translation: "Le même manque d'intégrité" },
            { term: "Take the easy way out", translation: "Choisir la facilité" },
            { term: "A society desensitised by", translation: "Une société désensibilisée par" },
            { term: "The coiled sexual power of a jungle cat", translation: "La puissance sexuelle enroulée d'un félin" },
            { term: "A working-through situation", translation: "Une situation de catharsis / d'élaboration psychique" },
            { term: "To get in touch with feelings", translation: "Entrer en contact avec des sentiments" },
            { term: "Have the nerve to do something", translation: "Avoir le cran de faire quelque chose" },
            { term: "I don't inhale", translation: "Je n'inhale pas" },
            { term: "Provocative", translation: "Provocateur" },
            { term: "I'm getting through to you?", translation: "Est-ce que je t'atteins ? / Mon message passe ?" },
            { term: "I'm older than her father", translation: "Je suis plus âgé que son père" },
            { term: "Wherein I can beat up her father", translation: "Situation où je peux battre son père" },
            { term: "Writing a book about our marriage", translation: "Écrire un livre sur notre mariage" },
            { term: "My little idiosyncrasies", translation: "Mes petites particularités" },
            { term: "My quirks and mannerisms", translation: "Mes bizarreries et mes manies" },
            { term: "Gossip is the new pornography", translation: "Les commérages sont la nouvelle pornographie" },
            { term: "I just didn't know how to get into it", translation: "Je ne savais juste pas comment aborder le sujet" },
            { term: "I've got kind of involved with her", translation: "Je me suis un peu engagé avec elle" },
            { term: "Nervous, high-strung, illusive", translation: "Nerveuse, tendue, insaisissable" },
            { term: "One of the best marriages", translation: "L'un des meilleurs mariages" },
            { term: "Very minor things with other women", translation: "Des choses très mineures avec d'autres femmes" },
            { term: "The winner of the August Strindberg Award", translation: "Le lauréat du prix August Strindberg (ironique)" },
            { term: "He writes that crap for television", translation: "Il écrit cette daube pour la télévision" },
            { term: "We can't abandon him", translation: "On ne peut pas l'abandonner" },
            { term: "Very Freudian", translation: "Très freudien" },
            { term: "An advance chapter", translation: "Un chapitre en avant-première" },
            { term: "It was hot stuff", translation: "C'était du lourd / croustillant" },
            { term: "I spilled wine on my pants", translation: "J'ai renversé du vin sur mon pantalon" },
            { term: "The immoral, psychotic, promiscuous one", translation: "Celui qui est immoral, psychotique, et a une vie sexuelle débridée" },
            { term: "I hope I didn't leave out anything", translation: "J'espère que je n'ai rien oublié" },
            { term: "I was still being tucked in", translation: "On me bordait encore au lit" },
            { term: "My wry sense of humour", translation: "Mon sens de l'humour pince-sans-rire" },
            { term: "Astonishing sexual technique", translation: "Technique sexuelle étonnante" },
            { term: "Don't wanna get hung up with one person", translation: "Je ne veux pas être obsédé par une seule personne" },
            { term: "A detour on the highway of life", translation: "Un détour sur l'autoroute de la vie" },
            { term: "The mouse in Tom And Jerry", translation: "La souris dans Tom et Jerry" },
            { term: "You've a whiny voice", translation: "Tu as une voix geignarde" },
            { term: "It was very derivative", translation: "C'était très peu original" },
            { term: "It had none of the wit", translation: "Ça n'avait aucune finesse d'esprit" },
            { term: "The Plexiglas sculpture", translation: "La sculpture en plexiglas" },
            { term: "It was very textural", translation: "C'était très texturé" },
            { term: "A marvellous kind of negative capability", translation: "Une merveilleuse sorte de capacité négative" },
            { term: "Mired in Thirties radicalism", translation: "Empêtré dans le radicalisme des années 30" },
            { term: "The Academy of the Overrated", translation: "L'Académie des Surcotés" },
            { term: "Adolescent, fashionable pessimism", translation: "Pessimisme adolescent et à la mode" },
            { term: "God's silence", translation: "Le silence de Dieu" },
            { term: "I loved it when I was at Radcliffe", translation: "J'adorais ça quand j'étais à Radcliffe" },
            { term: "You outgrow it", translation: "On finit par s'en lasser / le dépasser" },
            { term: "The dignifying of one's psychological and sexual hang-ups", translation: "La dignification de ses propres blocages psychologiques et sexuels" },
            { term: "Grandiose, philosophical issues", translation: "Des questions philosophiques grandioses" },
            { term: "I'm just from Philadelphia", translation: "Je suis juste de Philadelphie" },
            { term: "She was all cerebral", translation: "Elle était purement cérébrale" },
            { term: "Pseudo-intellectual garbage", translation: "Des déchets pseudo-intellectuels" },
      	    { term: "Discussions of existential reality", translation: "Discussions sur la réalité existentielle" },
      	    { term: "Mispronounce 'allegorical' and 'didacticism'", translation: "Mal prononcer 'allégorique' et 'didactisme'" },
      	    { term: "I was World War II", translation: "J'étais la Seconde Guerre mondiale" },
      	    { term: "I was in the trenches", translation: "J'étais dans les tranchées" },
      	    { term: "Antiseptic", translation: "Aseptisé" },
      	    { term: "Chancy material", translation: "Un sujet risqué / hasardeux" },
      	    { term: "Gamma rays eat the white cells of their brains out", translation: "Les rayons gamma dévorent les globules blancs de leur cerveau" },
      	    { term: "Open a pharmaceutical house", translation: "Ouvrir une société pharmaceutique" },
      	    { term: "I live like Mahatma Gandhi", translation: "Je vis comme le Mahatma Gandhi" },
      	    { term: "I'm cash poor", translation: "Je suis à court de liquidités" },
      	    { term: "I got no cash flow", translation: "Je n'ai pas de flux de trésorerie" },
      	    { term: "I'm not liquid", translation: "Je ne suis pas liquide" },
      	    { term: "Two alimonies and child support", translation: "Deux pensions alimentaires et une pension pour enfant" },
      	    { term: "Far from the action", translation: "Loin de l'action" },
      	    { term: "A self-destructive impulse", translation: "Une impulsion autodestructrice" },
      	    { term: "A devastating satirical piece", translation: "Un article satirique dévastateur" },
      	    { term: "Biting satire", translation: "Une satire mordante" },
      	    { term: "It's hard to satirise a guy with shiny boots", translation: "C'est difficile de faire la satire d'un gars avec des bottes brillantes" },
      	    { term: "It's aggressive-homicidal", translation: "C'est agressif-homicidaire" },
      	    { term: "Theodor Reik with a touch of Charles Manson", translation: "Theodor Reik avec une touche de Charles Manson" },
      	    { term: "I finally had an orgasm and my doctor told me it was the wrong kind", translation: "J'ai enfin eu un orgasme et mon médecin m'a dit que ce n'était pas le bon" },
      	    { term: "My worst one was right on the money", translation: "Mon pire était pile poil parfait" },
      	    { term: "Like the cast of a Fellini movie", translation: "Comme le casting d'un film de Fellini" },
      	    { term: "Submerging my identity", translation: "Submerger mon identité" },
      	    { term: "That must have been demoralising", translation: "Ça a dû être démoralisant" },
      	    { term: "Incredible sexual humiliation", translation: "Humiliation sexuelle incroyable" },
      	    { term: "No possible threat at all", translation: "Absolument aucune menace possible" },
      	    { term: "You have a losing personality", translation: "Tu as une personnalité de perdant" },
      	    { term: "I say what's on my mind", translation: "Je dis ce que je pense" },
      	    { term: "If you can't take it, then fuck off", translation: "Si tu ne peux pas le supporter, alors va te faire foutre" },
      	    { term: "Pithy, yet degenerate", translation: "Laconique, mais dégénéré" },
      	    { term: "It's all so subjective anyway", translation: "C'est tellement subjectif de toute façon" },
      	    { term: "When you climb into the sack", translation: "Quand tu montes dans le sac (au lit)" },
      	    { term: "A psychoanalytic quarterly", translation: "Une revue trimestrielle de psychanalyse" },
      	    { term: "Few people survive one mother", translation: "Peu de gens survivent à une seule mère" },
      	    { term: "It's a penis substitute for me", translation: "C'est un substitut de pénis pour moi" },
      	    { term: "You call your analyst Donny?", translation: "Tu appelles ton analyste Donny ?" },
      	    { term: "He hits me with a ruler", translation: "Il me frappe avec une règle" },
      	    { term: "I was his student", translation: "J'étais son étudiante" },
      	    { term: "He failed me and I fell in love with him", translation: "Il m'a fait échouer et je suis tombée amoureuse de lui" },
      	    { term: "Not even an Incomplete, right? Just a straight F?", translation: "Même pas un 'Incomplet', n'est-ce pas ? Juste un F direct ?" },
      	    { term: "Decaying values", translation: "Les valeurs en décomposition" },
      	    { term: "The Castrating Zionist", translation: "La Sioniste Castratrice" },
      	    { term: "You'll wind up in an ashtray", translation: "Tu vas finir dans un cendrier" },
      	    { term: "He was just a louse", translation: "C'était juste un pou / un salaud" },
      	    { term: "He really opened me up sexually", translation: "Il m'a vraiment ouverte sexuellement" },
      	    { term: "Nothing worth knowing is understood with the mind", translation: "Rien qui vaille la peine d'être su n'est compris avec l'esprit" },
      	    { term: "You rely too much on your brain", translation: "Tu te reposes trop sur ton cerveau" },
      	    { term: "The brain is the most overrated organ", translation: "Le cerveau est l'organe le plus surcoté" },
      	    { term: "I'm not gonna have any free time", translation: "Je n'aurai pas de temps libre" },
      	    { term: "I'm working on this book", translation: "Je travaille sur ce livre" },
      	    { term: "A new book on Virginia Woolf", translation: "Un nouveau livre sur Virginia Woolf" },
      	    { term: "I like it when you get an uncontrollable urge", translation: "J'aime quand tu as une envie incontrôlable" },
      	    { term: "My boyish impetuosity", translation: "Mon impétuosité juvénile" },
      	    { term: "We'll always have Paris", translation: "Nous aurons toujours Paris" },
      	    { term: "You'll be at the height of your sexual powers", translation: "Tu seras à l'apogée de ta puissance sexuelle" },
      	    { term: "I'm a late starter", translation: "Je suis un débutant tardif" },
      	    { term: "You're God's answer to Job", translation: "Tu es la réponse de Dieu à Job" },
      	    { term: "It's a no-win situation", translation: "C'est une situation sans issue" },
      	    { term: "I'm beautiful and I'm bright and I deserve better!", translation: "Je suis belle et je suis intelligente et je mérite mieux !" },
      	    { term: "I'm not a home wrecker", translation: "Je ne suis pas une briseuse de ménage" },
      	    { term: "My family's never had affairs", translation: "Ma famille n'a jamais eu de liaisons" },
      	    { term: "A man sawing a trumpet in half", translation: "Un homme sciant une trompette en deux" },
      	    { term: "I'll get my scuba-diving equipment", translation: "Je vais chercher mon équipement de plongée" },
      	    { term: "I got rats with bongos and a frog", translation: "J'ai des rats avec des bongos et une grenouille" },
      	    { term: "You're not happy the way things are going", translation: "Tu n'es pas content de la tournure des choses" },
      	    { term: "The computer in 2001", translation: "L'ordinateur dans 2001 (l'Odyssée de l'espace)" },
      	    { term: "I've gotta start thinking about Emily", translation: "Je dois commencer à penser à Emily" },
      	    { term: "You love Rampal", translation: "Tu adores Rampal" },
      	    { term: "Fuck off, Yale!", translation: "Va te faire foutre, Yale !" },
      	    { term: "It causes abdominal cancer, I think", translation: "Ça cause le cancer de l'abdomen, je pense" },
      	    { term: "You pick a married guy", translation: "Tu choisis un homme marié" },
      	    { term: "Don't psychoanalyse me", translation: "Ne me psychanalyse pas" },
      	    { term: "Your self-esteem is like a notch below Kafka's", translation: "Ton estime de soi est un cran en dessous de celle de Kafka" },
      	    { term: "He's up there strangling a parrot", translation: "Il est là-haut en train d'étrangler un perroquet" },
      	    { term: "I'm starting to sound like Rabbi Blitzstein", translation: "Je commence à parler comme le rabbin Blitzstein" },
      	    { term: "That guy's toupee", translation: "La perruque de ce type" },
      	    { term: "An inch of cheesecloth", translation: "Un pouce de gaze" },
      	    { term: "Her face has been lifted about 8,000 times", translation: "Son visage a été lifté environ 8000 fois" },
      	    { term: "I got black bean sauce in the bed", translation: "J'ai mis de la sauce aux haricots noirs dans le lit" },
      	    { term: "A WC Fields film", translation: "Un film de WC Fields" },
      	    { term: "I'm not the type for affairs", translation: "Je ne suis pas du genre à avoir des liaisons" },
      	    { term: "She deserves more than a fling", translation: "Elle mérite plus qu'une amourette" },
      	    { term: "She's screwed up, but great", translation: "Elle est paumée, mais géniale" },
      	    { term: "Right up your alley", translation: "Pile dans tes cordes" },
      	    { term: "Under my personal vibrations", translation: "Sous mes vibrations personnelles" },
      	    { term: "She went from bisexuality to homosexuality", translation: "Elle est passée de la bisexualité à l'homosexualité" },
      	    { term: "I gave it the old college try", translation: "J'ai fait de mon mieux" },
      	    { term: "Corned beef should not be blue", translation: "Le corned-beef ne devrait pas être bleu" },
      	    { term: "I cannot get my life in any kind of order", translation: "Je n'arrive pas à mettre de l'ordre dans ma vie" },
      	    { term: "I was kissing you flush on the mouth", translation: "Je t'embrassais en plein sur la bouche" },
      	    { term: "I had a mad impulse to throw you down on the lunar surface", translation: "J'ai eu une impulsion folle de te jeter sur la surface lunaire" },
      	    { term: "Commit interstellar perversion with you", translation: "Commettre une perversion interstellaire avec toi" },
      	    { term: "I can't go from relationship to relationship", translation: "Je ne peux pas passer d'une relation à l'autre" },
      	    { term: "Trouble is my middle name", translation: "Mon deuxième prénom est 'Problème'" },
      	    { term: "My middle name is Mortimer", translation: "Mon deuxième prénom est Mortimer" },
      	    { term: "I'm both attracted and repelled by the male organ", translation: "Je suis à la fois attirée et repoussée par l'organe masculin" },
      	    { term: "She got into drugs, became a Moonie", translation: "Elle a commencé à se droguer, est devenue une Moonie" },
      	    { term: "She's with the William Morris Agency now", translation: "Elle est maintenant avec l'agence William Morris" },
      	    { term: "A kind of wonderful otherness to it", translation: "Une sorte de merveilleuse altérité" },
      	    { term: "I can hardly keep my eyes on the meter", translation: "J'ai du mal à garder les yeux sur le compteur" },
      	    { term: "The only time in my life I ever had Chianti from Warsaw", translation: "La seule fois de ma vie où j'ai bu du Chianti de Varsovie" },
      	    { term: "The one between Hitler and Eva Braun", translation: "Celle entre Hitler et Eva Braun" },
      	    { term: "You're throwing away a enormous amount of real affection", translation: "Tu gaspilles une énorme quantité d'affection réelle" },
      	    { term: "You're getting too hung up on me", translation: "Tu deviens trop obsédé par moi" },
      	    { term: "Don't be so precocious", translation: "Ne sois pas si précoce" },
      	    { term: "My hair's falling out", translation: "Je perds mes cheveux" },
      	    { term: "Barefoot kids from Bolivia who needs foster parents", translation: "Enfants aux pieds nus de Bolivie qui ont besoin de parents d'accueil" },
      	    { term: "The mosquitoes have sucked all the blood out of my left leg", translation: "Les moustiques ont aspiré tout le sang de ma jambe gauche" },
      	    { term: "I felt for about two seconds you were faking", translation: "J'ai senti pendant environ deux secondes que tu simulais" },
      	    { term: "When you dug your nails into my neck", translation: "Quand tu as planté tes ongles dans mon cou" },
      	    { term: "An oversexed, brilliant kind of animal", translation: "Une sorte d'animal sur-sexué et brillant" },
      	    { term: "What am I? Grandma Moses?", translation: "Je suis quoi ? Grand-mère Moïse ?" },
      	    { term: "We'll trade fours", translation: "On va échanger des improvisations (terme de jazz)" },
      	    { term: "This is my friend Isaac Davis", translation: "Voici mon ami Isaac Davis" },
      	    { term: "A symposium on semantics", translation: "Un symposium sur la sémantique" },
      	    { term: "I was a sucker for Germanic theatre", translation: "J'ai toujours eu un faible pour le théâtre germanique" },
      	    { term: "This little homunculus", translation: "Ce petit homoncule" },
      	    { term: "He's quite devastating", translation: "Il est assez dévastateur" },
      	    { term: "I'm on that novelisation", translation: "Je suis sur cette novélisation" },
      	    { term: "Another contemporary American phenomenon that's truly moronic", translation: "Un autre phénomène américain contemporain qui est vraiment crétin" },
      	    { term: "I was hoping you'd pick up", translation: "J'espérais que tu décrocherais" },
      	    { term: "Viking loved my book", translation: "Viking a adoré mon livre" },
      	    { term: "Viking will shell out the money", translation: "Viking va débourser l'argent" },
      	    { term: "A meaningless extravagance", translation: "Une extravagance dénuée de sens" },
      	    { term: "A bizarre charade sex with my husband was", translation: "Une mascarade bizarre qu'était le sexe avec mon mari" },
      	    { term: "I didn't wanna be a bad sport", translation: "Je ne voulais pas être mauvais joueur" },
      	    { term: "The car lurched", translation: "La voiture a fait une embardée" },
      	    { term: "Jewish, liberal paranoia", translation: "Paranoïa juive et libérale" },
      	    { term: "Male chauvinism, self-righteous misanthropy", translation: "Chauvinisme masculin, misanthropie moralisatrice" },
      	    { term: "Nihilistic moods of despair", translation: "Humeurs nihilistes de désespoir" },
      	    { term: "He longed to be an artist, but balked at the necessary sacrifices", translation: "Il aspirait à être un artiste, mais reculait devant les sacrifices nécessaires" },
      	    { term: "His fear of death which he elevated to tragic heights", translation: "Sa peur de la mort qu'il élevait à des hauteurs tragiques" },
      	    { term: "It was mere narcissism", translation: "Ce n'était que du narcissisme pur" },
      	    { term: "You make me out to be Lee Harvey Oswald!", translation: "Tu me fais passer pour Lee Harvey Oswald !" },
      	    { term: "What would Freud say?", translation: "Que dirait Freud ?" },
      	    { term: "There's something I wanna tell you", translation: "Il y a quelque chose que je veux te dire" },
      	    { term: "Somebody should throw a blanket over me", translation: "Quelqu'un devrait me jeter une couverture dessus (pour le choc)" },
      	    { term: "This is shaping up like a Noel Coward play", translation: "Ça prend la tournure d'une pièce de Noel Coward" },
      	    { term: "I tend to internalise", translation: "J'ai tendance à intérioriser" },
      	    { term: "I grow a tumour instead", translation: "Je développe une tumeur à la place" },
      	    { term: "Donny's in a coma. He had a very bad acid experience.", translation: "Donny est dans le coma. Il a eu une très mauvaise expérience à l'acide." },
      	    { term: "The Zelda Fitzgerald Emotional Maturity Award", translation: "Le Prix Zelda Fitzgerald de la Maturité Émotionnelle" },
      	    { term: "You rationalise everything", translation: "Tu rationalises tout" },
      	    { term: "You're in front of a Senate committee naming names", translation: "Tu te retrouves devant un comité du Sénat à donner des noms" },
      	    { term: "You are so self-righteous", translation: "Tu es tellement moralisateur" },
      	    { term: "I gotta model myself after someone", translation: "Je dois prendre modèle sur quelqu'un" },
      	    { term: "When I thin out, I wanna make sure I'm well thought of", translation: "Quand je disparaîtrai, je veux m'assurer qu'on a une bonne opinion de moi" },
      	    { term: "Marriage requires some minor compromises", translation: "Le mariage exige quelques compromis mineurs" },
      	    { term: "I'm just a non-compromiser", translation: "Je suis simplement quelqu'un qui ne fait pas de compromis" },
      	    { term: "I think I really missed a good bet", translation: "Je pense que j'ai vraiment raté une bonne occasion" },
      	    { term: "I kept her at a distance", translation: "Je l'ai gardée à distance" },
      	    { term: "I didn't wanna lead her on", translation: "Je ne voulais pas lui donner de faux espoirs" },
      	    { term: "Creating these real, unnecessary, neurotic problems", translation: "Créer ces problèmes réels, inutiles et névrotiques" },
      	    { term: "More unsolvable, terrifying problems about the universe", translation: "Des problèmes plus insolubles et terrifiants concernant l'univers" },
      	    { term: "Why is life worth living?", translation: "Pourquoi la vie vaut-elle la peine d'être vécue ?" },
      	    { term: "The second movement of the Jupiter Symphony", translation: "Le deuxième mouvement de la Symphonie Jupiter" },
      	    { term: "Sentimental Education by Flaubert", translation: "L'Éducation Sentimentale de Flaubert" },
      	    { term: "Those incredible apples and pears by Cézanne", translation: "Ces incroyables pommes et poires de Cézanne" },
      	    { term: "The crabs at Sam Wo's", translation: "Les crabes chez Sam Wo" },
      	    { term: "I couldn't get a taxi cab, so I ran", translation: "Je n'ai pas pu trouver de taxi, alors j'ai couru" },
      	    { term: "Let me get right to the point", translation: "Laissez-moi aller droit au but" },
      	    { term: "I made a big mistake", translation: "J'ai fait une grosse erreur" },
      	    { term: "You'll be in the theatre with actors and directors", translation: "Tu seras dans le théâtre avec des acteurs et des metteurs en scène" },
      	    { term: "Attachments form", translation: "Des liens se créent" },
      	    { term: "Don't be so mature", translation: "Ne sois pas si mûr(e)" },
      	    { term: "You made such a convincing case", translation: "Tu as présenté des arguments si convaincants" },
      	    { term: "Not everybody gets corrupted", translation: "Tout le monde n'est pas corrompu" },
      	    { term: "You have to have a little faith in people", translation: "Il faut avoir un peu foi en les gens" }
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