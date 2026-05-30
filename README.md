<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Multiplication Adventure</title>

<style>

body{
    margin:0;
    font-family:Arial,sans-serif;
    background:linear-gradient(135deg,#ffb3ec,#a0e7ff,#fff5ba);
    text-align:center;
    padding:20px;
}

.container{
    max-width:800px;
    margin:auto;
}

.card{
    background:white;
    border-radius:25px;
    padding:30px;
    margin-top:20px;
    box-shadow:0 8px 25px rgba(0,0,0,0.2);
}

h1{
    color:#ff006e;
}

button{
    padding:15px 28px;
    font-size:1.2rem;
    border:none;
    border-radius:15px;
    background:#ff006e;
    color:white;
    cursor:pointer;
    margin:5px;
}

button:hover{
    transform:scale(1.05);
}

.hidden{
    display:none;
}

#question{
    font-size:3rem;
    margin:20px;
    color:#118ab2;
}

input{
    padding:12px;
    font-size:1.4rem;
    border-radius:10px;
    border:2px solid #8338ec;
}

#answer{
    width:120px;
    font-size:2rem;
}

#message{
    margin-top:20px;
    font-size:1.5rem;
    font-weight:bold;
    white-space:pre-line;
}

#reward{
    font-size:5rem;
    margin-top:15px;
}

#score{
    font-size:1.4rem;
    font-weight:bold;
    margin-top:15px;
}

#progressContainer{
    width:100%;
    height:28px;
    background:#eee;
    border-radius:20px;
    overflow:hidden;
    margin:15px 0;
}

#progressBar{
    width:0%;
    height:100%;
    background:linear-gradient(90deg,#06d6a0,#118ab2);
    transition:width .4s;
}

.example{
    font-size:2rem;
    margin:10px;
    font-weight:bold;
}

</style>
</head>

<body>

<div class="container">

<input
    type="text"
    id="playerName"
    placeholder="Enter your name"
    maxlength="20"
>

<h1 id="gameTitle">🌈 Multiplication Adventure 🌈</h1>

<div class="card" id="practiceScreen">

    <h2>Learn These Examples</h2>

    <div id="examplesArea"></div>

    <button id="startBtn">Start Quiz 🎉</button>

</div>

<div class="card hidden" id="quizScreen">

    <div id="roundInfo"></div>

    <div id="progressContainer">
        <div id="progressBar"></div>
    </div>

    <div id="question"></div>

    <input type="number" id="answer">

    <br><br>

    <button id="checkBtn">Check Answer</button>

    <div id="message"></div>

    <div id="reward"></div>

    <button id="nextBtn" class="hidden">
        Next Question ➡️
    </button>

    <div id="score">⭐ Score: 0</div>

</div>

</div>

<script>

let examples = [];
let currentQuiz = 0;
let score = 0;

const rewards = [
    "🐵","🦄","🐸","🐼","🍌",
    "🐧","🌟","🎈","🚀","🏆"
];

const praiseMessages = [
    "Mrs. Krista will be very proud of you.",
    "Lama thinks you're a genius.",
    "You are the new star of LIS.",
    "This makes Miss Jasmine smile.",
    "This makes Mr. Bollie dance.",
    "Miss Brenda will give you a hug.",
    "Multiplication can be fun.",
    "Multiplication is for cool kids.",
    "Be a cool kid — keep learning!",
    "Being smart is the new cool.",
    "Being clever is the new cool.",
    "MrBeast likes multiplication.",
    "Math champions never give up!",
    "Every multiplication master started as a beginner.",
    "You're getting smarter with every answer.",
    "Your brain is growing stronger!",
    "Keep going — you're doing great!",
    "Future genius at work!",
    "Multiplication powers activated!",
    "Math superheroes practice every day.",
    "You've got this!",
    "Learning is your superpower."
];

document.getElementById("startBtn")
    .addEventListener("click", startQuiz);

document.getElementById("checkBtn")
    .addEventListener("click", checkAnswer);

document.getElementById("nextBtn")
    .addEventListener("click", nextQuestion);

document.getElementById("answer")
    .addEventListener("keypress", function(e){
        if(e.key === "Enter"){
            checkAnswer();
        }
    });

createExamples();

function createExamples(){

    examples = [];

    for(let i=0;i<12;i++){

        let a = Math.floor(Math.random()*12)+1;
        let b = Math.floor(Math.random()*12)+1;

        examples.push({
            a:a,
            b:b,
            answer:a*b
        });
    }

    showExamples();
}

function showExamples(){

    const area = document.getElementById("examplesArea");

    area.innerHTML = "";

    examples.slice(0,4).forEach(item=>{

        area.innerHTML +=
            `<div class="example">
                ${item.a} × ${item.b} = ${item.answer}
            </div>`;
    });
}

function startQuiz(){

    const playerName =
        document.getElementById("playerName").value.trim();

    if(playerName){

        document.getElementById("gameTitle").innerText =
            "🌈 Multiplication Adventure for " +
            playerName +
            " 🌈";
    }

    document.getElementById("practiceScreen")
        .classList.add("hidden");

    document.getElementById("quizScreen")
        .classList.remove("hidden");

    currentQuiz = 0;

    loadQuestion();
}

function updateProgress(){

    const percent =
        (currentQuiz / 12) * 100;

    document.getElementById("progressBar").style.width =
        percent + "%";
}

function loadQuestion(){

    const q = examples[currentQuiz];

    document.getElementById("question").innerText =
        q.a + " × " + q.b + " = ?";

    document.getElementById("answer").value = "";

    document.getElementById("message").innerText = "";

    document.getElementById("reward").innerText = "";

    document.getElementById("nextBtn")
        .classList.add("hidden");

    document.getElementById("roundInfo").innerText =
        "Question " +
        (currentQuiz + 1) +
        " of 12";

    updateProgress();

    document.getElementById("answer").focus();
}

function checkAnswer(){

    const userAnswer =
        Number(document.getElementById("answer").value);

    const correct =
        examples[currentQuiz].answer;

    if(userAnswer === correct){

        score++;

        const praise =
            praiseMessages[Math.floor(
                Math.random()*praiseMessages.length)];

        const reward =
            rewards[Math.floor(
                Math.random()*rewards.length)];

        const playerName =
            document.getElementById("playerName").value.trim();

        if(playerName){

            document.getElementById("message").innerText =
                "Correct, " +
                playerName +
                "! 🎉\n" +
                praise;

        }else{

            document.getElementById("message").innerText =
                "Correct! 🎉\n" +
                praise;
        }

        document.getElementById("message").style.color =
            "green";

        document.getElementById("reward").innerText =
            reward;

        document.getElementById("score").innerText =
            "⭐ Score: " + score;

        document.getElementById("nextBtn")
            .classList.remove("hidden");

    }else{

        document.getElementById("message").innerText =
            "Oops! Try again 😊";

        document.getElementById("message").style.color =
            "red";
    }
}

function nextQuestion(){

    currentQuiz++;

    if(currentQuiz < 12){

        loadQuestion();

    }else{

        document.getElementById("progressBar").style.width =
            "100%";

        alert(
            "🏆 Amazing! You completed all 12 questions!\n\nScore: " +
            score + "/12"
        );

        createExamples();

        document.getElementById("quizScreen")
            .classList.add("hidden");

        document.getElementById("practiceScreen")
            .classList.remove("hidden");
    }
}

</script>

</body>
</html>
