<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>bhot padh liya bakchodi pelo abb</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #e6f7ff; /* Light Blue Background */
            font-family: 'Arial', sans-serif;
        }

        #board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-gap: 10px;
        }

        .cell {
            width: 100px;
            height: 100px;
            font-size: 24px;
            text-align: center;
            line-height: 100px;
            cursor: pointer;
            border: 2px solid #003366; /* Dark Blue Border */
            background-color: #cce0ff; /* Light Blue Cell */
            color: #003366; /* Dark Blue Text */
        }

        .cell:hover {
            background-color: #b3ccff; /* Slightly Darker Blue on Hover */
        }

        #message {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 24px;
            text-align: center;
            display: none;
        }

        #message.winner {
            color: green; /* Green for Winner Message */
        }

        #message.loser {
            color: red; /* Red for Loser Message */
        }
    </style>
</head>
<body>

<div id="board"></div>
<div id="message"></div>

<script>
    const board = document.getElementById('board');
    const message = document.getElementById('message');
    let currentPlayer = 'X';
    let gameBoard = ['', '', '', '', '', '', '', '', ''];

    function createCell(index) {
        const cell = document.createElement('div');
        cell.className = 'cell';
        cell.dataset.index = index;
        cell.addEventListener('click', handleCellClick);
        return cell;
    }

    function renderBoard() {
        board.innerHTML = '';
        for (let i = 0; i < 9; i++) {
            const cell = createCell(i);
            cell.textContent = gameBoard[i];
            board.appendChild(cell);
        }
    }

    function handleCellClick(event) {
        const index = event.target.dataset.index;
        if (gameBoard[index] === '') {
            gameBoard[index] = currentPlayer;
            renderBoard();
            if (checkWinner()) {
                showMessage(`${currentPlayer} is good at bakchodi!`, 'winner');
                setTimeout(resetGame, 1500);
            } else if (isBoardFull()) {
                showMessage("It's a tie!", 'winner');
                setTimeout(resetGame, 1500);
            } else {
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            }
        }
    }

    function checkWinner() {
        const winConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
            [0, 4, 8], [2, 4, 6]             // Diagonals
        ];

        return winConditions.some(condition =>
            condition.every(index => gameBoard[index] === currentPlayer)
        );
    }

    function isBoardFull() {
        return gameBoard.every(cell => cell !== '');
    }

    function showMessage(msg, className) {
        message.textContent = msg;
        message.className = className;
        message.style.display = 'block';
    }

    function resetGame() {
        currentPlayer = 'X';
        gameBoard = ['', '', '', '', '', '', '', '', ''];
        renderBoard();
        message.style.display = 'none';
    }

    renderBoard();
</script>

</body>
</html>
