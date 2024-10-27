# Criando-um-Jogo-da-mem-ria-com-Emojis-Utilizando-Javascript
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Memória com Emojis</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Jogo da Memória com Emojis</h1>
    <div class="game-board" id="gameBoard"></div>
    <button id="resetButton">Reiniciar Jogo</button>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #f0f0f0;
}

h1 {
    margin-bottom: 20px;
}

.game-board {
    display: grid;
    grid-template-columns: repeat(4, 100px);
    grid-template-rows: repeat(4, 100px);
    gap: 10px;
}

.card {
    width: 100px;
    height: 100px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    background-color: #fff;
    border: 2px solid #ccc;
    border-radius: 10px;
    cursor: pointer;
}

.card.flipped {
    background-color: #4CAF50;
    color: white;
}
const emojis = ['😀', '😂', '🥰', '😎', '🤔', '😱', '🥳', '🙈', '😀', '😂', '🥰', '😎', '🤔', '😱', '🥳', '🙈'];
let selectedCards = [];
let matchedCards = [];
let cardElements = [];
let gameBoard = document.getElementById('gameBoard');
let resetButton = document.getElementById('resetButton');

// Função para embaralhar as cartas
function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
}

// Função para criar o tabuleiro do jogo
function createBoard() {
    const shuffledEmojis = shuffle(emojis);
    gameBoard.innerHTML = '';

    shuffledEmojis.forEach((emoji, index) => {
        const card = document.createElement('div');
        card.classList.add('card');
        card.dataset.index = index;
        card.innerText = '';
        card.addEventListener('click', () => flipCard(card, emoji));
        gameBoard.appendChild(card);
        cardElements.push(card);
    });
}

// Função para virar a carta
function flipCard(card, emoji) {
    if (selectedCards.length < 2 && !card.classList.contains('flipped') && !matchedCards.includes(card)) {
        card.innerText = emoji;
        card.classList.add('flipped');
        selectedCards.push({ card, emoji });

        if (selectedCards.length === 2) {
            checkForMatch();
        }
    }
}

// Função para verificar se as cartas combinam
function checkForMatch() {
    const [firstCard, secondCard] = selectedCards;
    
    if (firstCard.emoji === secondCard.emoji) {
        matchedCards.push(firstCard.card, secondCard.card);
        selectedCards = [];
    } else {
        setTimeout(() => {
            firstCard.card.innerText = '';
            secondCard.card.innerText = '';
            firstCard.card.classList.remove('flipped');
            secondCard.card.classList.remove('flipped');
            selectedCards = [];
        }, 1000);
    }
}

// Função para reiniciar o jogo
function resetGame() {
    selectedCards = [];
    matchedCards = [];
    cardElements = [];
    createBoard();
}

// Inicializa o jogo
resetButton.addEventListener('click', resetGame);
createBoard();
