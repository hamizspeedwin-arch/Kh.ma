# Kh.ma
<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة الدودة للموبايل 🐍</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #1a1a2e;
            color: white;
            font-family: sans-serif;
            touch-action: manipulation; /* يمنع التكبير عند اللمس السريع */
        }
        canvas {
            border: 4px solid #16a085;
            background-color: #000;
            max-width: 90vw;
            max-height: 50vh;
        }
        .controls {
            display: grid;
            grid-template-columns: repeat(3, 70px);
            grid-template-rows: repeat(2, 70px);
            gap: 10px;
            margin-top: 20px;
        }
        button {
            width: 70px;
            height: 70px;
            background-color: #16a085;
            border: none;
            color: white;
            font-size: 24px;
            border-radius: 10px;
            font-weight: bold;
        }
        .up { grid-column: 2; }
        .left { grid-column: 1; grid-row: 2; }
        .down { grid-column: 2; grid-row: 2; }
        .right { grid-column: 3; grid-row: 2; }
    </style>
</head>
<body>

    <h2>النقاط: <span id="score">0</span></h2>
    <canvas id="snakeGame" width="400" height="400"></canvas>

    <div class="controls">
        <button class="up" onclick="changeDir('UP')">↑</button>
        <button class="left" onclick="changeDir('LEFT')">←</button>
        <button class="down" onclick="changeDir('DOWN')">↓</button>
        <button class="right" onclick="changeDir('RIGHT')">→</button>
    </div>

    <script>
        const canvas = document.getElementById("snakeGame");
        const ctx = canvas.getContext("2d");
        const scoreElement = document.getElementById("score");

        let box = 20;
        let snake = [{ x: 10 * box, y: 10 * box }];
        let food = {
            x: Math.floor(Math.random() * 19 + 1) * box,
            y: Math.floor(Math.random() * 19 + 1) * box
        };
        let score = 0;
        let d = "RIGHT"; // جعلناها تبدأ بالتحرك لليمين تلقائياً

        function changeDir(newDir) {
            if(newDir == "LEFT" && d != "RIGHT") d = "LEFT";
            else if(newDir == "UP" && d != "DOWN") d = "UP";
            else if(newDir == "RIGHT" && d != "LEFT") d = "RIGHT";
            else if(newDir == "DOWN" && d != "UP") d = "DOWN";
        }

        function draw() {
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            for(let i = 0; i < snake.length; i++) {
                ctx.fillStyle = (i == 0) ? "#2ecc71" : "#27ae60";
                ctx.fillRect(snake[i].x, snake[i].y, box, box);
            }

            ctx.fillStyle = "#e74c3c";
            ctx.fillRect(food.x, food.y, box, box);

            let snakeX = snake[0].x;
            let snakeY = snake[0].y;

            if( d == "LEFT") snakeX -= box;
            if( d == "UP") snakeY -= box;
            if( d == "RIGHT") snakeX += box;
            if( d == "DOWN") snakeY += box;

            if(snakeX == food.x && snakeY == food.y) {
                score++;
                scoreElement.innerHTML = score;
                food = {
                    x: Math.floor(Math.random() * 19 + 1) * box,
                    y: Math.floor(Math.random() * 19 + 1) * box
                };
            } else {
                snake.pop();
            }

            let newHead = { x: snakeX, y: snakeY };

            if(snakeX < 0 || snakeX >= canvas.width || snakeY < 0 || snakeY >= canvas.height || collision(newHead, snake)) {
                clearInterval(game);
                alert("انتهت اللعبة! نقاطك: " + score);
                location.reload();
            }

            snake.unshift(newHead);
        }

        function collision(head, array) {
            for(let i = 0; i < array.length; i++) {
                if(head.x == array[i].x && head.y == array[i].y) return true;
            }
            return false;
        }

        let game = setInterval(draw, 150); // سرعة متوسطة تناسب الهاتف
    </script>
</body>
</html>
