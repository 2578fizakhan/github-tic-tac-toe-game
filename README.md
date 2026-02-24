<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Neon Tic Tac Toe Pro</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

<div class="container">

    <h1>Neon Tic Tac Toe</h1>

    <div class="scoreboard">
        <div>X Wins: <span id="xScore">0</span></div>
        <div>O Wins: <span id="oScore">0</span></div>
        <div>Draws: <span id="drawScore">0</span></div>
    </div>

    <div id="turn">Player X Turn</div>

    <div class="board" id="board"></div>

    <div class="buttons">
        <button onclick="resetGame()">Reset Game</button>
        <button onclick="resetScore()">Reset Score</button>
    </div>

</div>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Segoe UI',sans-serif;
}

body{
min-height:100vh;
display:flex;
justify-content:center;
align-items:center;
background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
color:#fff;
padding:20px;
}

/* ================= CONTAINER ================= */

.container{
width:100%;
max-width:500px;
text-align:center;
background:rgba(255,255,255,0.05);
padding:25px;
border-radius:20px;
backdrop-filter:blur(15px);
box-shadow:0 0 30px rgba(0,255,255,0.3);
}

/* ================= HEADINGS ================= */

h1{
margin-bottom:20px;
font-size:clamp(22px,4vw,32px);
letter-spacing:2px;
}

/* ================= SCOREBOARD ================= */

.scoreboard{
display:flex;
justify-content:space-between;
flex-wrap:wrap;
gap:10px;
margin-bottom:15px;
font-size:clamp(14px,3vw,18px);
}

/* ================= TURN TEXT ================= */

#turn{
margin:15px 0;
font-size:clamp(16px,3vw,20px);
font-weight:bold;
}

/* ================= BOARD ================= */

.board{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:12px;
width:100%;
max-width:450px;
margin:20px auto;
}

/* ================= CELLS ================= */

.cell{
aspect-ratio:1/1;
background:rgba(255,255,255,0.08);
display:flex;
justify-content:center;
align-items:center;
font-size:clamp(28px,6vw,45px);
font-weight:bold;
cursor:pointer;
border-radius:15px;
transition:0.3s;
}

.cell:hover{
background:rgba(0,255,255,0.3);
transform:scale(1.05);
}

/* ================= X & O STYLE ================= */

.X{
color:#00ffff;
text-shadow:0 0 15px #00ffff;
}

.O{
color:#ff00ff;
text-shadow:0 0 15px #ff00ff;
}

.winner{
background:#00ff88 !important;
color:#000;
}

/* ================= BUTTONS ================= */

.buttons{
display:flex;
flex-wrap:wrap;
justify-content:center;
gap:10px;
}

.buttons button{
padding:10px 18px;
border:none;
border-radius:25px;
cursor:pointer;
font-size:14px;
background:linear-gradient(45deg,#00ffff,#ff00ff);
color:#000;
font-weight:bold;
transition:0.3s;
}

.buttons button:hover{
transform:scale(1.1);
}

/* ================= RESPONSIVE ================= */

/* Tablet */
@media (min-width: 768px){
.container{
max-width:600px;
}

.board{
gap:15px;
}
}

/* Large Laptop / Desktop */
@media (min-width: 1200px){
.container{
max-width:700px;
}

.board{
max-width:500px;
}
}

/* Extra Small Mobile */
@media(max-width:400px){
.container{
padding:18px;
}

.board{
gap:10px;
}
}
<const board = document.getElementById("board");
const turnText = document.getElementById("turn");
const xScoreEl = document.getElementById("xScore");
const oScoreEl = document.getElementById("oScore");
const drawScoreEl = document.getElementById("drawScore");

let cells = [];
let currentPlayer = "X";
let gameActive = true;
let xScore = 0;
let oScore = 0;
let drawScore = 0;

function createBoard(){
board.innerHTML = "";
cells = [];

for(let i=0;i<9;i++){
let cell = document.createElement("div");
cell.classList.add("cell");
cell.dataset.index = i;
cell.addEventListener("click", handleClick);
board.appendChild(cell);
cells.push(cell);
}
}

function handleClick(e){
let cell = e.target;

if(cell.textContent !== "" || !gameActive) return;

cell.textContent = currentPlayer;
cell.classList.add(currentPlayer);

if(checkWinner()){
updateScore(currentPlayer);
highlightWinner();
turnText.textContent = `Player ${currentPlayer} Wins!`;
gameActive = false;
return;
}

if(isDraw()){
drawScore++;
drawScoreEl.textContent = drawScore;
turnText.textContent = "It's a Draw!";
gameActive = false;
return;
}

currentPlayer = currentPlayer === "X" ? "O" : "X";
turnText.textContent = `Player ${currentPlayer} Turn`;
}

function checkWinner(){
const winPatterns = [
[0,1,2],[3,4,5],[6,7,8],
[0,3,6],[1,4,7],[2,5,8],
[0,4,8],[2,4,6]
];

for(let pattern of winPatterns){
let [a,b,c] = pattern;
if(
cells[a].textContent &&
cells[a].textContent === cells[b].textContent &&
cells[a].textContent === cells[c].textContent
){
cells[a].classList.add("winner");
cells[b].classList.add("winner");
cells[c].classList.add("winner");
return true;
}
}
return false;
}

function isDraw(){
return cells.every(cell => cell.textContent !== "");
}

function updateScore(player){
if(player === "X"){
xScore++;
xScoreEl.textContent = xScore;
}else{
oScore++;
oScoreEl.textContent = oScore;
}
}

function resetGame(){
currentPlayer = "X";
gameActive = true;
turnText.textContent = "Player X Turn";
createBoard();
}

function resetScore(){
xScore = 0;
oScore = 0;
drawScore = 0;
xScoreEl.textContent = 0;
oScoreEl.textContent = 0;
drawScoreEl.textContent = 0;
resetGame();
}

createBoard();script src="script.js"></script>
</body>
</html>
