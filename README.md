# Create-A-Game-with-AI
Like many of my smart friends, I am using DuoLingo to learn Spanish
My favorite way to save my streak when I am feeling lazy is to do a mini-game from the side quests
Usually, they are easy matching games. With this weeks Innovative Tech assignment, I wanted to see if I could create my own version:


https://github.com/user-attachments/assets/72748a20-674e-4715-9321-23823652db39

Here is the code provided by Claude.ai:

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Spanish-English Matching Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        .game-container {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        .card-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }
        .card {
            border: 2px solid #3498db;
            border-radius: 5px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            background-color: #f9f9f9;
            transition: all 0.3s ease;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .card.matched {
            background-color: #2ecc71;
            color: white;
            border-color: #27ae60;
        }
        .card.selected {
            background-color: #3498db;
            color: white;
        }
        #score {
            text-align: center;
            margin-bottom: 20px;
            font-size: 1.2em;
        }
        button {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
        }
        button:hover {
            background-color: #2980b9;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Spanish-English Matching Game</h1>
        <div id="score">Matches: 0/5</div>
        <div class="card-grid" id="cardGrid"></div>
        <button onclick="resetGame()">New Game</button>
    </div>

    <script>
        const wordPairs = [
            { spanish: "gato", english: "cat" },
            { spanish: "perro", english: "dog" },
            { spanish: "casa", english: "house" },
            { spanish: "libro", english: "book" },
            { spanish: "árbol", english: "tree" }
        ];

        let firstCard = null;
        let matchedPairs = 0;
        let locked = false;

        function createCards() {
            const cards = [
                ...wordPairs.map(pair => ({ word: pair.spanish, type: 'spanish', pair: pair.english })),
                ...wordPairs.map(pair => ({ word: pair.english, type: 'english', pair: pair.spanish }))
            ].sort(() => Math.random() - 0.5);
            
            const grid = document.getElementById('cardGrid');
            grid.innerHTML = '';
            
            cards.forEach(card => {
                const cardElement = document.createElement('div');
                cardElement.classList.add('card');
                cardElement.textContent = card.word;
                cardElement.dataset.word = card.word;
                cardElement.dataset.pair = card.pair;
                cardElement.addEventListener('click', () => handleCardClick(cardElement));
                grid.appendChild(cardElement);
            });
        }

        function handleCardClick(clickedCard) {
            if (locked || 
                clickedCard.classList.contains('matched') || 
                clickedCard === firstCard) {
                return;
            }

            clickedCard.classList.add('selected');

            if (!firstCard) {
                firstCard = clickedCard;
                return;
            }

            locked = true;
            
            if (firstCard.dataset.word === clickedCard.dataset.pair || 
                firstCard.dataset.pair === clickedCard.dataset.word) {
                matchedPairs++;
                document.getElementById('score').textContent = `Matches: ${matchedPairs}/5`;
                
                firstCard.classList.add('matched');
                clickedCard.classList.add('matched');
                
                if (matchedPairs === 5) {
                    setTimeout(() => {
                        alert('¡Felicidades! You won! 🎉');
                    }, 500);
                }
                
                locked = false;
            } else {
                setTimeout(() => {
                    firstCard.classList.remove('selected');
                    clickedCard.classList.remove('selected');
                    locked = false;
                }, 1000);
            }

            firstCard = null;
        }

        function resetGame() {
            matchedPairs = 0;
            firstCard = null;
            locked = false;
            document.getElementById('score').textContent = 'Matches: 0/5';
            createCards();
        }

        // Initialize the game
        createCards();
    </script>
</body>
</html>
