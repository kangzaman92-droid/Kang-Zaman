<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Petualangan Sifat Wajib Allah</title>
<style>
body {
    font-family: 'Comic Sans MS', sans-serif;
    background: #fffae6;
    text-align: center;
    padding: 20px;
}
button {
    padding: 10px 20px;
    margin: 10px;
    font-size: 16px;
    cursor: pointer;
    border-radius: 10px;
    transition: transform 0.2s;
}
button:hover {
    transform: scale(1.1);
}
#game, #result {
    display: none;
}
.star {
    color: gold;
    font-size: 24px;
    display: inline-block;
    animation: shine 1s infinite alternate;
}
@keyframes shine {
    0% {opacity: 0.5;}
    100% {opacity: 1;}
}
.timer {
    font-size: 24px;
    font-weight: bold;
}
</style>
</head>
<body>

<h1>🎮 Petualangan Mengenal 20 Sifat Wajib Allah</h1>
<p>Masukkan namamu untuk memulai:</p>
<input type="text" id="nameInput" placeholder="Nama Kamu">
<br>
<button onclick="startGame()">Mulai Petualangan</button>

<div id="game">
    <h2 id="levelTitle"></h2>
    <p id="question"></p>
    <div id="answers"></div>
    <p class="timer" id="timer">30</p>
    <p id="feedback"></p>
    <div id="stars"></div>
</div>

<div id="result">
    <h2>🌟 Selamat!</h2>
    <p id="finalMessage"></p>
    <button onclick="restartGame()">Main Lagi</button>
    <p style="font-size: 12px;">Created by Kang Zaman</p>
</div>

<audio id="bgMusic" loop>
    <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
</audio>
<audio id="correctSound">
    <source src="https://www.soundjay.com/buttons/sounds/button-3.mp3" type="audio/mpeg">
</audio>
<audio id="wrongSound">
    <source src="https://www.soundjay.com/buttons/sounds/button-10.mp3" type="audio/mpeg">
</audio>
<audio id="timeUpSound">
    <source src="https://www.soundjay.com/buttons/sounds/beep-07.mp3" type="audio/mpeg">
</audio>

<script>
const questions = [
    {q: "Sifat wajib Allah yang pertama adalah?", a: ["Wujud","Qidam","Baqa","Mukhalafatu lil-hawadith"], correct: 0},
    {q: "Sifat wajib Allah yang kedua adalah?", a: ["Qidam","Baqa","Wujud","Mukhalafatu lil-hawadith"], correct: 0},
    {q: "Sifat wajib Allah yang ketiga adalah?", a: ["Baqa","Qidam","Wujud","Mukhalafatu lil-hawadith"], correct: 0},
    {q: "Sifat wajib Allah yang keempat adalah?", a: ["Mukhalafatu lil-hawadith","Baqa","Wujud","Qidam"], correct: 0},
    {q: "Sifat wajib Allah yang kelima adalah?", a: ["Qiyamuhu binafsihi","Baqa","Wujud","Qidam"], correct: 0}
];

let currentLevel = 0;
let score = 0;
let timer;
let timeLeft = 30;
let studentName = "";

function startGame() {
    studentName = document.getElementById('nameInput').value || "Siswa";
    document.getElementById('bgMusic').play();
    document.querySelector('input').style.display = 'none';
    document.querySelector('button').style.display = 'none';
    document.getElementById('game').style.display = 'block';
    currentLevel = 0;
    score = 0;
    showQuestion();
}

function showQuestion() {
    const q = questions[currentLevel];
    document.getElementById('levelTitle').innerText = `Level ${currentLevel+1}`;
    document.getElementById('question').innerText = q.q;
    const answersDiv = document.getElementById('answers');
    answersDiv.innerHTML = '';
    q.a.forEach((ans,i) => {
        const btn = document.createElement('button');
        btn.innerText = ans;
        btn.onclick = () => checkAnswer(i);
        answersDiv.appendChild(btn);
    });
    startTimer();
    document.getElementById('feedback').innerText = '';
}

function checkAnswer(i) {
    stopTimer();
    const q = questions[currentLevel];
    if(i === q.correct){
        score++;
        document.getElementById('feedback').innerText = "Masya Allah. Kamu benar!";
        document.getElementById('correctSound').play();
        addStar();
    } else {
        document.getElementById('feedback').innerText = "Maaf ya, kamu belum beruntung";
        document.getElementById('wrongSound').play();
    }
    setTimeout(nextLevel,1000);
}

function nextLevel() {
    currentLevel++;
    if(currentLevel < questions.length){
        timeLeft = 30;
        showQuestion();
    } else {
        showResult();
    }
}

function addStar() {
    const starDiv = document.getElementById('stars');
    const span = document.createElement('span');
    span.className = 'star';
    span.innerText = '⭐';
    starDiv.appendChild(span);
}

function showResult() {
    document.getElementById('game').style.display = 'none';
    document.getElementById('result').style.display = 'block';
    document.getElementById('finalMessage').innerText = `MasyaAllah, ${studentName}! Kamu berhasil menyelesaikan semua level dengan skor ${score}/${questions.length}`;
}

function restartGame() {
    document.getElementById('result').style.display = 'none';
    document.querySelector('input').style.display = 'inline-block';
    document.querySelector('button').style.display = 'inline-block';
    document.querySelector('input').value = '';
    document.getElementById('stars').innerHTML = '';
}

function startTimer() {
    timeLeft = 30;
    document.getElementById('timer').innerText = timeLeft;
    timer = setInterval(() => {
        timeLeft--;
        const timerEl = document.getElementById('timer');
        timerEl.innerText = timeLeft;
        if(timeLeft <= 3){
            timerEl.style.color = timeLeft === 3 ? "yellow" : timeLeft === 2 ? "orange" : "red";
            timerEl.style.fontWeight = "bold";
            timerEl.style.transform = "scale(1.2)";
        } else {
            timerEl.style.color = "black";
            timerEl.style.transform = "scale(1)";
        }
        if(timeLeft <= 0){
            stopTimer();
            document.getElementById('feedback').innerText = "Waktu habis!";
            document.getElementById('timeUpSound').play();
            setTimeout(nextLevel,1000);
        }
    },1000);
}

function stopTimer() {
    clearInterval(timer);
}
</script>

</body>
</html>
