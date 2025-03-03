<!DOCTYPE html>
<html>
<head>
    <title>文字俄罗斯方块 - 学雷锋版</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: #f0f0f0;
        }
        #game {
            display: inline-block;
            background: #fff;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        .cell {
            width: 30px;
            height: 30px;
            border: 1px solid #ddd;
            text-align: center;
            line-height: 30px;
            font-size: 20px;
        }
        .row {
            display: flex;
        }
    </style>
</head>
<body>

    <h1>文字俄罗斯方块 - 学雷锋版</h1>
    <div id="game"></div>
    <p>使用方向键控制：← →移动，↑旋转，↓加速</p >

    <script>
        const width = 10;
        const height = 20;
        const game = document.getElementById('game');
        let board = Array.from({ length: height }, () => Array(width).fill(''));
        const chars = ['学', '雷', '锋'];
        let currentPiece;

        function createPiece() {
            const type = Math.floor(Math.random() * 3);
            return {
                shape: [[chars[type]]],
                x: Math.floor(width / 2),
                y: 0
            };
        }

        function draw() {
            game.innerHTML = '';
            const displayBoard = JSON.parse(JSON.stringify(board));
            if (currentPiece) {
                currentPiece.shape.forEach((row, dy) => {
                    row.forEach((val, dx) => {
                        if (val) {
                            displayBoard[currentPiece.y + dy][currentPiece.x + dx] = val;
                        }
                    });
                });
            }
            displayBoard.forEach(row => {
                const rowDiv = document.createElement('div');
                rowDiv.className = 'row';
                row.forEach(cell => {
                    const cellDiv = document.createElement('div');
                    cellDiv.className = 'cell';
                    cellDiv.textContent = cell;
                    rowDiv.appendChild(cellDiv);
                });
                game.appendChild(rowDiv);
            });
        }

        function movePiece(dx, dy) {
            currentPiece.x += dx;
            currentPiece.y += dy;
            if (checkCollision()) {
                currentPiece.x -= dx;
                currentPiece.y -= dy;
                if (dy === 1) {
                    placePiece();
                }
                return false;
            }
            return true;
        }

        function checkCollision() {
            return currentPiece.shape.some((row, dy) => 
                row.some((val, dx) => 
                    val && (board[currentPiece.y + dy]?.[currentPiece.x + dx] || 
                           currentPiece.x + dx < 0 || 
                           currentPiece.x + dx >= width || 
                           currentPiece.y + dy >= height)
            );
        }

        function placePiece() {
            currentPiece.shape.forEach((row, dy) => {
                row.forEach((val, dx) => {
                    if (val) {
                        board[currentPiece.y + dy][currentPiece.x + dx] = val;
                    }
                });
            });
            clearLines();
            currentPiece = createPiece();
            if (checkCollision()) {
                alert('游戏结束！');
                board = Array.from({ length: height }, () => Array(width).fill(''));
            }
        }

        function clearLines() {
            board = board.filter(row => row.some(cell => !cell));
            while (board.length < height) {
                board.unshift(Array(width).fill(''));
            }
        }

        document.addEventListener('keydown', e => {
            if (e.key === 'ArrowLeft') movePiece(-1, 0);
            if (e.key === 'ArrowRight') movePiece(1, 0);
            if (e.key === 'ArrowDown') movePiece(0, 1);
            draw();
        });

        currentPiece = createPiece();
        setInterval(() => movePiece(0, 1) && draw(), 1000);
        draw();
    </script>

</body>
</html>
