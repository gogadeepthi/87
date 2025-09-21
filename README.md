const cells = document.querySelectorAll('.cell');
        const status = document.getElementById('status');
        const resetButton = document.getElementById('reset');
        let board = ['', '', '', '', '', '', '', '', ''];
        let currentPlayer = 'X';
        let gameActive = true;

        const winningCombinations = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
            [0, 4, 8], [2, 4, 6] // Diagonals
        ];

        function handleCellClick(event) {
            const index = event.target.dataset.index;
            if (board[index] !== '' || !gameActive) return;

            board[index] = currentPlayer;
            event.target.textContent = currentPlayer;

            if (checkWin()) {
                status.textContent = Player ${currentPlayer} wins!;
                gameActive = false;
                return;
            }

            if (board.every(cell => cell !== '')) {
                status.textContent = "It's a draw!";
                gameActive = false;
                return;
            }

            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            status.textContent = Player ${currentPlayer}'s turn;
        }

        function checkWin() {
            return winningCombinations.some(combination => {
                return combination.every(index => board[index] === currentPlayer);
            });
        }

        function resetGame() {
            board = ['', '', '', '', '', '', '', '', ''];
            currentPlayer = 'X';
            gameActive = true;
            status.textContent = Player ${currentPlayer}'s turn;
            cells.forEach(cell => (cell.textContent = ''));
        }

        cells.forEach(cell => cell.addEventListener('click', handleCellClick));
        resetButton.addEventListener('click', resetGame);
