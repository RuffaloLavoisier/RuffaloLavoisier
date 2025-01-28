


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Obstacle Avoider</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
        }
        canvas {
            display: block;
            background: lightblue;
        }
    </style>
</head>
<body>
<canvas id="gameCanvas"></canvas>

<script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const plane = { x: 50, y: canvas.height / 2, width: 50, height: 30, speed: 7 };
    const obstacles = [];
    const obstacleWidth = 30;
    let gameOver = false;

    document.addEventListener('keydown', (e) => {
        if (e.key === 'ArrowUp' && plane.y > 0) plane.y -= plane.speed;
        if (e.key === 'ArrowDown' && plane.y + plane.height < canvas.height) plane.y += plane.speed;
    });

    function drawPlane() {
        ctx.fillStyle = 'red';
        ctx.fillRect(plane.x, plane.y, plane.width, plane.height);
    }

    function createObstacle() {
        const height = Math.random() * (canvas.height / 2) + 50;
        obstacles.push({ x: canvas.width, y: 0, width: obstacleWidth, height });
        obstacles.push({ x: canvas.width, y: height + 100, width: obstacleWidth, height: canvas.height - height - 100 });
    }

    function updateObstacles() {
        for (let i = obstacles.length - 1; i >= 0; i--) {
            obstacles[i].x -= 5;
            if (obstacles[i].x + obstacleWidth < 0) obstacles.splice(i, 1);
        }
    }

    function drawObstacles() {
        ctx.fillStyle = 'green';
        obstacles.forEach((obstacle) => {
            ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
        });
    }

    function checkCollision() {
        for (const obstacle of obstacles) {
            if (
                plane.x < obstacle.x + obstacle.width &&
                plane.x + plane.width > obstacle.x &&
                plane.y < obstacle.y + obstacle.height &&
                plane.y + plane.height > obstacle.y
            ) {
                gameOver = true;
            }
        }
    }

    function update() {
        if (gameOver) {
            ctx.fillStyle = 'black';
            ctx.font = '48px Arial';
            ctx.fillText('Game Over!', canvas.width / 2 - 100, canvas.height / 2);
            return;
        }

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawPlane();
        drawObstacles();
        updateObstacles();
        checkCollision();

        requestAnimationFrame(update);
    }

    setInterval(createObstacle, 2000);
    update();
</script>
</body>
</html>
