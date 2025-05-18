Estrutura de Pastas

jogo-preservacao-marinha/  
├── index.html  
├── styles.css  
└── app.js


---

📄 index.html

<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo de Preservação Marinha</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Jogo de Preservação Marinha</h1>
    <canvas id="gameCanvas"></canvas>
    <div class="controls">
        <button onclick="moverPeixe('cima')">↑</button>
        <button onclick="moverPeixe('baixo')">↓</button>
        <button onclick="moverPeixe('direita')">→</button>
    </div>
    <script src="app.js"></script>
</body>
</html>


---

📄 styles.css

body {
    background-color: #0077b6;
    color: #ffffff;
    font-family: Arial, sans-serif;
    text-align: center;
    margin: 0;
}

canvas {
    background-color: #90e0ef;
    display: block;
    margin: 20px auto;
    border-radius: 15px;
}

.controls button {
    font-size: 20px;
    margin: 10px;
    padding: 10px 20px;
    background-color: #00b4d8;
    color: #ffffff;
    border: none;
    border-radius: 10px;
    cursor: pointer;
}

.controls button:hover {
    background-color: #023e8a;
}


---

📄 app.js

const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");
canvas.width = 800;
canvas.height = 500;

let peixe = { x: 50, y: 250 };
let obstaculos = [
    { x: 800, y: 100 },
    { x: 1000, y: 200 },
    { x: 1200, y: 300 }
];

function desenharPeixe() {
    ctx.fillStyle = "yellow";
    ctx.beginPath();
    ctx.arc(peixe.x, peixe.y, 20, 0, Math.PI * 2);
    ctx.fill();
}

function desenharObstaculos() {
    ctx.fillStyle = "gray";
    obstaculos.forEach(obs => {
        ctx.beginPath();
        ctx.arc(obs.x, obs.y, 20, 0, Math.PI * 2);
        ctx.fill();
    });
}

function moverObstaculos() {
    obstaculos.forEach(obs => {
        obs.x -= 5;
        if (obs.x < 0) obs.x = canvas.width;
    });
}

function verificarColisao() {
    obstaculos.forEach(obs => {
        const dx = obs.x - peixe.x;
        const dy = obs.y - peixe.y;
        const distancia = Math.sqrt(dx * dx + dy * dy);
        if (distancia < 40) {
            alert("Você foi pego pelo lixo! Tente novamente.");
            resetarJogo();
        }
    });
    if (peixe.x > canvas.width - 50) {
        alert("Parabéns! Você salvou o peixe!");
        resetarJogo();
    }
}

function resetarJogo() {
    peixe = { x: 50, y: 250 };
    obstaculos = [
        { x: 800, y: 100 },
        { x: 1000, y: 200 },
        { x: 1200, y: 300 }
    ];
}

function moverPeixe(direcao) {
    if (direcao === "cima" && peixe.y > 20) peixe.y -= 40;
    if (direcao === "baixo" && peixe.y < canvas.height - 20) peixe.y += 40;
    if (direcao === "direita") peixe.x += 40;
}

function loop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    desenharPeixe();
    desenharObstaculos();
    moverObstaculos();
    verificarColisao();
    requestAnimationFrame(loop);
}

loop();
