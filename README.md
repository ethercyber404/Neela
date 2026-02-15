<html>
<head>
<meta charset="UTF-8">
<title>tHeEtHeR</title>

<style>
body{
    margin:0;
    overflow:hidden;
    background:#70c5ce;
}
canvas{
    display:block;
    margin:auto;
}
</style>
</head>
<body>

<canvas id="gameCanvas"></canvas>

<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

canvas.width = 400;
canvas.height = 600;

// ===== IMAGE =====
const birdImg = new Image();
birdImg.src = "Alamin.png";

// ===== SOUNDS =====
const tapSound = new Audio("tap.mp3");
const gameOverSound = new Audio("gameover.mp3");

// ===== BIRD =====
let bird = {
    x: 80,
    y: 200,
    width: 60,
    height: 60,
    gravity: 0.5,
    lift: -9,
    velocity: 0
};

// ===== PIPES =====
let pipes = [];
let pipeWidth = 60;
let gap = 160;
let frame = 0;
let score = 0;
let gameOver = false;

// ===== CONTROLS =====
document.addEventListener("click", flap);
document.addEventListener("touchstart", flap);

function flap(){
    if(gameOver){
        location.reload();
    } else {
        bird.velocity = bird.lift;
        tapSound.currentTime = 0;
        tapSound.play();
    }
}

// ===== DRAW BIRD =====
function drawBird(){
    if(!birdImg.complete) return;

    ctx.save();
    ctx.beginPath();
    ctx.arc(bird.x + bird.width/2, bird.y + bird.height/2, 30, 0, Math.PI*2);
    ctx.closePath();
    ctx.clip();
    ctx.drawImage(birdImg, bird.x, bird.y, bird.width, bird.height);
    ctx.restore();
}

// ===== DRAW PIPES =====
function drawPipes(){
    pipes.forEach(pipe => {
        ctx.fillStyle = "#2ecc71";
        ctx.fillRect(pipe.x, 0, pipeWidth, pipe.top);
        ctx.fillRect(pipe.x, pipe.bottom, pipeWidth, canvas.height - pipe.bottom);
    });
}

// ===== UPDATE =====
function update(){
    if(gameOver) return;

    frame++;

    if(frame % 100 === 0){
        let top = Math.random() * 300 + 50;
        pipes.push({
            x: canvas.width,
            top: top,
            bottom: top + gap
        });
    }

    bird.velocity += bird.gravity;
    bird.y += bird.velocity;

    pipes.forEach(pipe => {
        pipe.x -= 2;

        if(
            bird.x < pipe.x + pipeWidth &&
            bird.x + bird.width > pipe.x &&
            (bird.y < pipe.top || bird.y + bird.height > pipe.bottom)
        ){
            endGame();
        }
    });

    pipes = pipes.filter(pipe => pipe.x + pipeWidth > 0);

    if(bird.y + bird.height >= canvas.height || bird.y <= 0){
        endGame();
    }

    score++;
}

// ===== END GAME =====
function endGame(){
    if(!gameOver){
        gameOver = true;
        gameOverSound.play();
    }
}

// ===== DRAW =====
function draw(){
    ctx.clearRect(0,0,canvas.width,canvas.height);
    drawBird();
    drawPipes();

    ctx.fillStyle="white";
    ctx.font="25px Arial";
    ctx.fillText("Score: " + Math.floor(score/10), 10, 40);

    if(gameOver){
        ctx.font="35px Arial";
        ctx.fillText("GAME OVER", 90, 300);
        ctx.font="20px Arial";
        ctx.fillText("Tap to Restart", 130, 340);
    }
}

// ===== GAME LOOP =====
function gameLoop(){
    update();
    draw();
    requestAnimationFrame(gameLoop);
}

// Image load হলে game start হবে
birdImg.onload = function(){
    gameLoop();
};
</script>

</body>
</html>



