<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe</title>
    <style>
        .game-container {
            width: 300px;
            margin: 40px auto;
            text-align: center;
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-gap: 10px;
        }

        .cell {
            background-color: #eee;
            padding: 20px;
            border: 1px solid #ddd;
            cursor: pointer;
        }

        .cell:hover {
            background-color: #ccc;
        }

        .game-info {
            margin-top: 20px;
        }

        #game-status {
            font-size: 18px;
            font-weight: bold;
        }

        #reset-button {
            background-color: #4CAF50;
            color: #fff;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        #reset-button:hover {
            background-color: #3e8e41;
        }
    </style>
</head>
<body>
    <h1>Tic-Tac-Toe</h1>
    <div class="game-container">
        <div class="game-board">
            <div class="row">
                <div class="cell" id="cell-0"></div>
                <div class="cell" id="cell-1"></div>
                <div class="cell" id="cell-2"></div>
            </div>
            <div class="row">
                <div class="cell" id="cell-3"></div>
                <div class="cell" id="cell-4"></div>
                <div class="cell" id="cell-5"></div>
            </div>
            <div class="row">
                <div class="cell" id="cell-6"></div>
                <div class="cell" id="cell-7"></div>
                <div class="cell" id="cell-8"></div>
            </div>
        </div>
        <div class="game-info">
            <p id="game-status">Game in progress...</p>
            <button id="reset-button">Reset Game</button>
        </div>
    </div>

    <script>
        let gameBoard = [];
        let currentPlayer = 'X';
        let gameOver = false;

        // Initialize game board
        for (let i = 0; i < 9; i++) {
            gameBoard.push('');
            document.getElementById(`cell-${i}`).addEventListener('click', handleCellClick);
        }

        // Handle cell click
        function handleCellClick(event) {
            if (gameOver) return;
            const cellIndex = event.target.id.split('-')[1];
            if (gameBoard[cellIndex] === '') {
                gameBoard[cellIndex] = currentPlayer;
                updateCell(event.target, currentPlayer);
                checkWinningCondition();
                switchPlayer();
            }
        }

        // Update cell with current player's mark
        function updateCell(cell, player) {
            cell.textContent = player;
        }

        // Switch current player
        function switchPlayer() {
            currentPlayer = currentPlayer === 'X'? 'O' : 'X';
        }

        // Check winning condition
        function checkWinningCondition() {
            const winningCombinations = [
                [0, 1, 2],
                [3, 4, 5],
                [6, 7, 8],
                [0, 3, 6],
                [1, 4, 7],
                [2, 5, 8],
                [0, 4, 8],
                [2, 4, 6]
            ];

            for (let i = 0; i < winningCombinations.length; i++) {
                const combination = winningCombinations[i];
                if (gameBoard[combination[0]] === gameBoard[combination[1]] && gameBoard[combination[1]] === gameBoard[combination[2]] && gameBoard[combination[0]]!== '') {
                    gameOver = true;
                    updateGameStatus(`Player ${gameBoard[combination[0]]} wins!`);
                    return;
                }
            }

            if (!gameBoard.includes('')) {
                gameOver = true;
                updateGameStatus('It\'s a draw!');
            }
        }

        // Update game status
        function updateGameStatus(status) {
            document.getElementById('game-status').textContent = status;
        }

        // Reset game
        document.getElementById('reset-button').addEventListener('click', function() {
            gameBoard
