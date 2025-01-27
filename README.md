<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Click Fasters</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            overflow: hidden;
            position: relative;
        }
        .opening-screen {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            opacity: 0; /* Inicia transparente */
            animation: fadeIn 3s forwards; /* Animação de fadeIn em 3 segundos */
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.8);
            z-index: 1000;
        }
        #gameTitle {
            font-size: 48px;
            font-weight: bold; /* Fonte mais gordinha */
            color: #333; /* Cor escura */
            margin-bottom: 20px;
        }
        .opening-options {
            margin-top: 20px;
        }
        .opening-options button {
            padding: 10px 20px;
            margin: 0 10px;
            font-size: 18px;
            cursor: pointer;
        }
        @keyframes fadeIn {
            to {
                opacity: 1; /* Torna totalmente visível */
            }
        }
        /* Estilos para o jogo principal */
        .container {
            text-align: center;
            display: none; /* Inicia oculto */
        }
        #clickButton {
            width: 100px;
            height: 100px;
            background-image: url('https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg');
            background-size: cover;
            background-position: center;
            border: none;
            cursor: pointer;
            margin: 20px;
            border-radius: 50%;
            position: relative;
        }
        #score {
            font-size: 24px;
            margin-top: 20px;
        }
        .mini-apple {
            position: absolute;
            width: 30px;
            height: 30px;
            background-image: url('https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg');
            background-size: cover;
            background-position: center;
            border-radius: 50%;
            opacity: 0.5;
            animation: moveAndFade 2s forwards;
            pointer-events: none;
        }
        @keyframes moveAndFade {
            to {
                transform: translate(var(--moveX), var(--moveY));
                opacity: 0;
            }
        }
        /* Estilos para a loja */
        .store-icon {
            position: fixed;
            top: 20px;
            right: 20px;
            width: 40px;
            height: 40px;
            background-color: #ccc;
            border-radius: 50%;
            cursor: pointer;
            z-index: 100;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: #333;
        }
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 200;
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
        }
        .modal-back-button {
            position: absolute;
            top: 20px;
            right: 20px;
            background-color: #ccc;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
        }
        .upgrade-button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }
        .store-container.show {
            display: block;
        }
    </style>
</head>
<body>
    <!-- Tela de Abertura -->
    <div class="opening-screen">
        <div id="gameTitle">Click Fasters</div>
        <div class="opening-options">
            <button id="playButton">Play</button>
        </div>
    </div>

    <!-- Jogo Principal -->
    <div class="container" id="gameContainer">
        <div id="title">Click Fasters</div>
        <button id="clickButton"></button>
        <div id="score">Pontos: 0</div>
    </div>

    <!-- Ícone da loja -->
    <div class="store-icon" id="storeIcon">Loja</div>

    <!-- Modal da loja -->
    <div class="modal" id="storeModal">
        <div class="modal-content">
            <h2>Loja</h2>
            <p>Aqui você pode comprar upgrades!</p>
            <button class="modal-back-button" id="backButton">Voltar</button>
            <button class="upgrade-button" id="doublePointsButton">Dobrar Pontos (50 pontos)</button>
            <button class="upgrade-button" id="triplePointsButton">Triplicar Pontos (100 pontos)</button>
            <button class="upgrade-button" id="quadruplePointsButton">Quádruplo de Pontos (200 pontos)</button>
            <button class="upgrade-button" id="quintuplePointsButton">Quíntuplo de Pontos (400 pontos)</button>
            <button class="upgrade-button" id="sextuplePointsButton">Sêxtuplo de Pontos (800 pontos)</button>
            <button class="upgrade-button" id="septuplePointsButton">Sétuplo de Pontos (1600 pontos)</button>
            <button class="upgrade-button" id="octuplePointsButton">Óctuplo de Pontos (3200 pontos)</button>
            <button class="upgrade-button" id="nonuplePointsButton">Nonúplo de Pontos (6400 pontos)</button>
            <button class="upgrade-button" id="decuplePointsButton">Décuplo de Pontos (12800 pontos)</button>
            <button class="upgrade-button" id="undecuplePointsButton">Undécuplo de Pontos (25600 pontos)</button>
        </div>
    </div>

    <script>
        let score = 0;
        let pointsMultiplier = 1; // Multiplicador inicial dos pontos

        // Elementos da tela de abertura
        const openingScreen = document.querySelector('.opening-screen');
        const gameContainer = document.getElementById('gameContainer');

        // Botões da tela de abertura
        const playButton = document.getElementById('playButton');

        // Event listener para Play
        playButton.addEventListener('click', function() {
            openingScreen.style.display = 'none'; // Oculta a tela de abertura
            gameContainer.style.display = 'block'; // Exibe o jogo principal
            score = 0; // Reinicia os pontos
            pointsMultiplier = 1; // Reinicia o multiplicador de pontos
            document.getElementById('score').innerText = 'Pontos: ' + score;
        });

        // Event listener para o botão de clique no jogo principal
        document.getElementById("clickButton").addEventListener("click", function(event) {
            score += pointsMultiplier;
            document.getElementById("score").innerText = "Pontos: " + score;
            createMiniApples(event.clientX, event.clientY);
        });

        // Event listener para mostrar/ocultar a loja
        document.getElementById("storeIcon").addEventListener("click", function() {
            const modal = document.getElementById("storeModal");
            modal.style.display = "flex"; // Exibe o modal da loja ao clicar no ícone
        });

        // Event listener para o botão "Voltar" na loja
        document.getElementById("backButton").addEventListener("click", function() {
            const modal = document.getElementById("storeModal");
            modal.style.display = "none"; // Oculta o modal da loja ao clicar em "Voltar"
        });

        // Event listener para comprar upgrade de dobrar pontos
        document.getElementById("doublePointsButton").addEventListener("click", function() {
            buyUpgrade(2, 50, "Dobrar Pontos");
        });

        // Event listener para comprar upgrade de triplicar pontos
        document.getElementById("triplePointsButton").addEventListener("click", function() {
            buyUpgrade(3, 100, "Triplicar Pontos");
        });

        // Event listener para comprar upgrade de quádruplo de pontos
        document.getElementById("quadruplePointsButton").addEventListener("click", function() {
            buyUpgrade(4, 200

, "Quádruplo de Pontos");
        });

        // Event listener para comprar upgrade de quíntuplo de pontos
        document.getElementById("quintuplePointsButton").addEventListener("click", function() {
            buyUpgrade(5, 400, "Quíntuplo de Pontos");
        });

        // Event listener para comprar upgrade de sêxtuplo de pontos
        document.getElementById("sextuplePointsButton").addEventListener("click", function() {
            buyUpgrade(6, 800, "Sêxtuplo de Pontos");
        });

        // Event listener para comprar upgrade de sétuplo de pontos
        document.getElementById("septuplePointsButton").addEventListener("click", function() {
            buyUpgrade(7, 1600, "Sétuplo de Pontos");
        });

        // Event listener para comprar upgrade de óctuplo de pontos
        document.getElementById("octuplePointsButton").addEventListener("click", function() {
            buyUpgrade(8, 3200, "Óctuplo de Pontos");
        });

        // Event listener para comprar upgrade de nonúplo de pontos
        document.getElementById("nonuplePointsButton").addEventListener("click", function() {
            buyUpgrade(9, 6400, "Nonúplo de Pontos");
        });

        // Event listener para comprar upgrade de décuplo de pontos
        document.getElementById("decuplePointsButton").addEventListener("click", function() {
            buyUpgrade(10, 12800, "Décuplo de Pontos");
        });

        // Event listener para comprar upgrade de undécuplo de pontos
        document.getElementById("undecuplePointsButton").addEventListener("click", function() {
            buyUpgrade(11, 25600, "Undécuplo de Pontos");
        });

        // Função para comprar upgrade
        function buyUpgrade(multiplier, cost, label) {
            if (score >= cost) {
                pointsMultiplier *= multiplier;
                score -= cost;
                document.getElementById('score').innerText = 'Pontos: ' + score;
                alert(label + " comprado com sucesso! Agora cada clique vale " + pointsMultiplier + " pontos.");
            } else {
                alert("Pontos insuficientes para comprar " + label + ".");
            }
        }

        // Função para criar maçãs menores
        function createMiniApples(x, y) {
            const miniApple = document.createElement('div');
            miniApple.classList.add('mini-apple');
            miniApple.style.setProperty('--moveX', x + 'px');
            miniApple.style.setProperty('--moveY', y + 'px');
            document.body.appendChild(miniApple);
            setTimeout(function() {
                miniApple.remove();
            }, 2000); // Remove após 2 segundos
        }
    </script>
</body>
</html>
