<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>For Eyza 💖</title>

<style>
body{
    margin:0;
    font-family: 'Poppins', sans-serif;
    text-align:center;
    background: linear-gradient(135deg,#ff9ec4,#ffcce6);
    overflow:hidden;
}

.screen{
    display:none;
    padding-top:120px;
    color:white;
}

button{
    padding:15px 30px;
    font-size:22px;
    border:none;
    border-radius:30px;
    cursor:pointer;
    margin:20px;
    transition:.3s;
}

#intro{ display:block; }

#no{
    position:absolute;
    background:gray;
    color:white;
}

#yes{
    background:#ff4d88;
    color:white;
}

.heart{
    position:absolute;
    animation: float 5s linear infinite;
}

@keyframes float{
    0%{transform:translateY(100vh);opacity:1;}
    100%{transform:translateY(-10vh);opacity:0;}
}
</style>
</head>

<body>

<!-- Intro -->
<div id="intro" class="screen">
    <h1>Hey Eyza 💌</h1>
    <p>I have something special for you...</p>
    <button onclick="start()">Click Here 💖</button>
</div>

<!-- Loading -->
<div id="loading" class="screen">
    <h1>Checking Our Love Compatibility... 💞</h1>
    <h2 id="percent">0%</h2>
</div>

<!-- Question -->
<div id="question" class="screen">
    <h1>Eyza, Will you be my Valentine? 💖</h1>
    <button id="yes">Yes 💘</button>
    <button id="no">No 😜</button>
</div>

<!-- Final -->
<div id="final" class="screen">
    <h1>Yea! Good choice Eyza 😍💞</h1>
    <img src="https://media.giphy.com/media/l0MYt5jPR6QX5pnqM/giphy.gif" width="250">
</div>

<script>

function start(){
    document.getElementById("intro").style.display="none";
    document.getElementById("loading").style.display="block";

    let percent = 0;
    let interval = setInterval(()=>{
        percent++;
        document.getElementById("percent").innerText = percent + "%";

        if(percent >= 100){
            clearInterval(interval);
            showQuestion();
        }
    },30);
}

function showQuestion(){
    document.getElementById("loading").style.display="none";
    document.getElementById("question").style.display="block";
}

let noBtn = document.getElementById("no");
let yesBtn = document.getElementById("yes");
let size = 22;

noBtn.addEventListener("mouseover", function(){
    let x = Math.random()*(window.innerWidth-100);
    let y = Math.random()*(window.innerHeight-50);
    noBtn.style.left = x+"px";
    noBtn.style.top = y+"px";

    size += 5;
    yesBtn.style.fontSize = size+"px";
});

yesBtn.addEventListener("click", function(){
    document.getElementById("question").style.display="none";
    document.getElementById("final").style.display="block";
    launchHearts();
});

function launchHearts(){
    for(let i=0;i<50;i++){
        let heart=document.createElement("div");
        heart.classList.add("heart");
        heart.innerHTML="💖";
        heart.style.left=Math.random()*100+"vw";
        heart.style.fontSize=(20+Math.random()*30)+"px";
        document.body.appendChild(heart);
        setTimeout(()=>heart.remove(),5000);
    }
}

</script>

</body>
</html>
