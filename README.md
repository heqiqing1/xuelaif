<!DOCTYPE html>
<html>
<head>
    <title>学雷锋贪吃蛇</title>
    <meta charset="UTF-8">
    <meta name="keywords" content="贪吃蛇, 学雷锋">
    <meta name="Description" content="这是一个以学雷锋为主题的贪吃蛇小游戏">
    <style type="text/css">
        * { margin: 0; }
        .map {
            margin: 50px auto;
            height: 600px;
            width: 900px;
            background: #E8F5E9; /* 浅绿色背景 */
            border: 10px solid #4CAF50; /* 绿色边框 */
            border-radius: 8px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
        }
        h1 {
            text-align: center;
            color: #4CAF50;
            margin-top: 20px;
            font-family: '微软雅黑', sans-serif;
        }
    </style>
</head>
<body>
    <h1>学雷锋贪吃蛇</h1>
    <div class="map">
        <canvas id="canvas" height="600" width="900"></canvas>
    </div>

    <script type="text/javascript">
        // 获取画布和上下文
        var c = document.getElementById("canvas");
        var ctx = c.getContext("2d");

        // 定义蛇和食物
        var snake = []; // 蛇的身体
        var snakeCount = 6; // 初始长度
        var foodx = 0; // 食物的x坐标
        var foody = 0; // 食物的y坐标
        var togo = 0; // 蛇的移动方向
        var words = ["学", "雷", "锋"]; // 蛇身体的文字

        // 画地图
        function drawtable() {
            ctx.clearRect(0, 0, 900, 600); // 清空画布

            // 画蛇的身体
            for (var k = 0; k < snakeCount; k++) {
                ctx.fillStyle = k === snakeCount - 1 ? "#FF5722" : "#4CAF50"; // 蛇头为橙色，身体为绿色
                ctx.fillRect(snake[k].x, snake[k].y, 15, 15);
                ctx.fillStyle = "#FFF"; // 文字颜色为白色
                ctx.font = "12px Arial";
                ctx.fillText(words[k % 3], snake[k].x + 3, snake[k].y + 12); // 在方块中显示文字
            }

            // 画食物
            ctx.fillStyle = "#FF0000"; // 食物为红色
            ctx.font = "15px Arial";
            ctx.fillText("❤", foodx + 3, foody + 12); // 食物用“❤”表示
        }

        // 初始化蛇的位置
        function start() {
            for (var k = 0; k < snakeCount; k++) {
                snake[k] = { x: k * 15, y: 0 };
            }
            drawtable();
            addfood(); // 添加食物
        }

        // 随机生成食物
        function addfood() {
            foodx = Math.floor(Math.random() * 60) * 15;
            foody = Math.floor(Math.random() * 40) * 15;

            // 防止食物生成在蛇身上
            for (var k = 0; k < snakeCount; k++) {
                if (foodx === snake[k].x && foody === snake[k].y) {
                    addfood();
                    return;
                }
            }
        }

        // 蛇的移动
        function move() {
            var head = { x: snake[snakeCount - 1].x, y: snake[snakeCount - 1].y };

            switch (togo) {
                case 1: head.x -= 15; break; // 左
                case 2: head.y -= 15; break; // 上
                case 3: head.x += 15; break; // 右
                case 4: head.y += 15; break; // 下
                default: head.x += 15; // 默认向右
            }

            snake.push(head); // 添加新头部
            snake.shift(); // 删除尾部

            // 检查是否吃到食物
            if (head.x === foodx && head.y === foody) {
                addfood();
                snakeCount++;
                snake.unshift({ x: -15, y: -15 }); // 增加身体长度
            }

            // 检查是否撞墙或撞到自己
            if (head.x < 0 || head.x >= 900 || head.y < 0 || head.y >= 600 || checkCollision()) {
                alert("游戏结束！你的分数是：" + (snakeCount - 6));
                window.location.reload(); // 重新开始游戏
            }

            drawtable(); // 重新绘制
        }

        // 检查是否撞到自己
        function checkCollision() {
            var head = snake[snakeCount - 1];
            for (var k = 0; k < snakeCount - 1; k++) {
                if (head.x === snake[k].x && head.y === snake[k].y) {
                    return true;
                }
            }
            return false;
        }

        // 键盘控制
        function keydown(e) {
            switch (e.keyCode) {
                case 37: togo = 1; break; // 左
                case 38: togo = 2; break; // 上
                case 39: togo = 3; break; // 右
                case 40: togo = 4; break; // 下
            }
        }

        // 初始化游戏
        document.onkeydown = keydown;
        window.onload = function () {
            start();
            setInterval(move, 150); // 每150ms移动一次
        };
    </script>
</body>
</html>
