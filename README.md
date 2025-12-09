<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Quiz Blast! — Fun Cartoon Quiz</title>
<style>
:root{--bg:#f7fbff;--card:#fff;--accent:#ffcc33;--accent2:#66ccff;--text:#123;}
*{box-sizing:border-box}
body{font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,'Helvetica Neue',Arial; background:linear-gradient(180deg,var(--bg),#e8f6ff); margin:0; min-height:100vh; display:flex; align-items:center; justify-content:center; padding:24px}
.wrapper{width:100%;max-width:940px}
.card{background:var(--card); border-radius:18px; padding:20px; box-shadow: 0 8px 30px rgba(3,20,40,0.08)}
header{display:flex; gap:16px; align-items:center}
.logo{display:flex; gap:12px; align-items:center}
.title{font-size:22px; font-weight:700; color:var(--text)}
.subtitle{font-size:12px; color:#346; opacity:0.85}
.svg-wrap{width:74px;height:74px;padding:8px;background:linear-gradient(135deg,var(--accent),#ff9966);border-radius:14px;display:flex;align-items:center;justify-content:center;box-shadow:0 6px 18px rgba(255,170,50,0.12)}
main{display:grid;grid-template-columns:1fr 320px;gap:18px;margin-top:16px}
@media(max-width:880px){main{grid-template-columns:1fr} .side{order:2}}
.quiz-area{padding:18px}
.question{font-size:18px;margin-bottom:12px}
.answers{display:grid;gap:10px}
.ans{background:#f4fbff;border:2px solid rgba(102,204,255,0.15);padding:12px;border-radius:10px;cursor:pointer;transition:transform .12s ease, box-shadow .12s;}
.ans:hover{transform:translateY(-4px); box-shadow:0 8px 20px rgba(10,50,80,0.06)}
.ans.correct{background:linear-gradient(90deg,#d1ffc7,#b8ffd0);border-color:#75d44b}
.ans.wrong{background:linear-gradient(90deg,#ffd6d6,#ffcfcf);border-color:#ff6b6b}
.meta{display:flex;justify-content:space-between;align-items:center;margin-top:12px;font-size:13px;color:#334}
.side{padding:18px;background:linear-gradient(180deg,#f8ffff,#ffffff);border-radius:12px;border:1px solid rgba(15,50,80,0.03)}
.score{font-size:40px;font-weight:800;color:#0b6e4f}
.small{font-size:12px;color:#456;margin-top:6px}
.btn{display:inline-block;padding:10px 14px;border-radius:12px;background:linear-gradient(90deg,var(--accent2),#9be6ff);cursor:pointer;border:none;font-weight:700}
.controls{display:flex;gap:10px;flex-wrap:wrap;margin-top:12px}
footer{margin-top:14px;text-align:center;font-size:12px;color:#567}
.confetti{position:fixed;left:0;right:0;top:0;pointer-events:none}
@keyframes pop {0%{transform:scale(1)}50%{transform:scale(1.15)}100%{transform:scale(1)}}
.pop {animation: pop 0.3s ease;}
</style>
</head>
<body>
<div class="wrapper">
<div class="card">
<header> <div class="logo"> <div class="svg-wrap" aria-hidden> </div>
<div> <div class="title">Quiz Blast!</div> <div class="subtitle">fast, fun & cartoon — code included 🎮</div> </div>
</div>
</header>
<main>
<section class="quiz-area cardless">
<div id="questionBox">
<div class="question" id="question">Loading question...</div>
<div class="answers" id="answers"></div>
<div class="meta"><div id="progress">Question 0 / 0</div><div id="timer">--:--</div></div>
</div>
<div style="margin-top:12px;display:flex;gap:8px;justify-content:flex-end">
<button class="btn" id="nextBtn" disabled>Next</button>
<button class="btn" id="restartBtn" style="background:linear-gradient(90deg,#ffd36b,#ffcc99)">Restart</button>
</div>
</section>
<aside class="side">
<div style="display:flex;align-items:center;justify-content:space-between">
<div><div style="font-size:13px;color:#345">Score</div><div class="score" id="score">0</div></div>
<div style="text-align:right"><div style="font-size:13px;color:#345">Lives</div><div style="font-weight:800;color:#cc4b4b" id="lives">3</div></div>
</div>
<div class="small">Tip: click an answer. Correct answers glow green. Wrong answers glow red. Try to keep lives!</div>
<div class="controls">
<button class="btn" id="bgmToggle">Music On</button>
<button class="btn" id="shuffle">Shuffle Qs</button>
<button class="btn" id="toggleTimer">Timer On</button>
</div>
</aside>
</main>
<footer>Made for learning & fun — edit <code>questions</code> in the script to add your own.</footer>
</div>
</div>
<canvas id="confetti" class="confetti"></canvas>
<script>
const questions=[
{q:"Which planet is known as the Red Planet?",choices:["Earth","Mars","Jupiter","Venus"],answer:1,pts:10},
{q:"Which language runs in a web browser?",choices:["Python","C++","JavaScript","Swift"],answer:2,pts:10},
{q:"2 + 2 × 3 = ?",choices:["8","10","12","6"],answer:0,pts:10},
{q:"Which animal is the largest mammal?",choices:["Elephant","Blue whale","Giraffe","Rhino"],answer:1,pts:10},
{q:"What color do you get mixing red and white?",choices:["Pink","Orange","Brown","Purple"],answer:0,pts:5},
{q:"HTML is used to...",choices:["Style a page","Structure content","Program logic","Store data"],answer:1,pts:10},
{q:"Which one is a prime number?",choices:["9","15","17","21"],answer:2,pts:10},
{q:"Which instrument has keys and produces sound when hammers strike strings?",choices:["Violin","Piano","Flute","Drum"],answer:1,pts:10},
{q:"What do bees produce?",choices:["Milk","Honey","Oil","Water"],answer:1,pts:10},
{q:"Which shape has 3 sides?",choices:["Circle","Triangle","Square","Star"],answer:1,pts:10}
];
const backgroundMusic=new Audio('https://cdn.pixabay.com/download/audio/2021/09/06/audio_calm_relax_loop.mp3?filename=calm-relax-loop.mp3');
backgroundMusic.loop=true;let musicOn=true;
const correctSound=new Audio('https://actions.google.com/sounds/v1/voice/english_male_congratulations.ogg');
const wrongSound=new Audio('https://actions.google.com/sounds/v1/cartoon/pop.ogg');
let pool=[],current=0,score=0,lives=3,timerOn=true,timeLeft=20,tickInterval=null;
const questionEl=document.getElementById('question');
const answersEl=document.getElementById('answers');
const progressEl=document.getElementById('progress');
const scoreEl=document.getElementById('score');
const livesEl=document.getElementById('lives');
const nextBtn=document.getElementById('nextBtn');
const restartBtn=document.getElementById('restartBtn');
const shuffleBtn=document.getElementById('shuffle');
const toggleTimerBtn=document.getElementById('toggleTimer');
function updateHUD(){scoreEl.textContent=score;livesEl.textContent=lives;progressEl.textContent=`Question ${Math.min(current+1,pool.length)} / ${pool.length}`;updateTimerDisplay();}
function startGame(){pool=[...questions];shuffle(pool);current=0;score=0;lives=3;updateHUD();showRandomQuestion();if(musicOn)backgroundMusic.play().catch(()=>{});}
function showRandomQuestion(){clearInterval(tickInterval);if(current>=pool.length||lives<=0){endGame();return;}const item=pool[current];questionEl.textContent=item.q;answersEl.innerHTML='';item.choices.forEach((c,i)=>{const btn=document.createElement('button');btn.className='ans';btn.textContent=c;btn.onclick=()=>selectAnswer(i,btn,item);answersEl.appendChild(btn);});updateHUD();nextBtn.disabled=true;timeLeft=20;updateTimerDisplay();if(timerOn)tickInterval=setInterval(tick,1000);}
function selectAnswer(i,el,item){const buttons=answersEl.querySelectorAll('button');buttons.forEach(b=>b.onclick=null);clearInterval(tickInterval);if(i===item.answer){correctSound.currentTime=0;correctSound.play().catch(()=>{});el.classList.add('pop');el.classList.add('correct');score+=item.pts;scoreEl.textContent=score;questionEl.textContent='🎉 Congratulations!';burstConfetti();}else{wrongSound.currentTime=0;wrongSound.play().catch(()=>{});el.classList.add('wrong');lives-=1;livesEl.textContent=lives;if(buttons[item.answer])buttons[item.answer].classList.add('correct');}nextBtn.disabled=false;}
nextBtn.addEventListener('click',()=>{current++;showRandomQuestion();});
restartBtn.addEventListener('click',startGame);
shuffleBtn.addEventListener('click',()=>{shuffle(pool);current=0;showRandomQuestion();});
toggleTimerBtn.addEventListener('click',()=>{timerOn=!timerOn;toggleTimerBtn.textContent=timerOn?'Timer On':'Timer Off';if(!timerOn)clearInterval(tickInterval);else showRandomQuestion();});
document.getElementById('bgmToggle').addEventListener('click',()=>{musicOn=!musicOn;document.getElementById('bgmToggle').textContent=musicOn?'Music On':'Music Off';if(musicOn)backgroundMusic.play().catch(()=>{});else backgroundMusic.pause();});
function tick(){timeLeft-=1;updateTimerDisplay();if(timeLeft<=0){clearInterval(tickInterval);const buttons=answersEl.querySelectorAll('button');buttons.forEach(b=>b.onclick=null);if(buttons[pool[current].answer])buttons[pool[current].answer].classList.add('correct');lives-=1;livesEl.textContent=lives;nextBtn.disabled=false;}}
function updateTimerDisplay(){const m=Math.floor(timeLeft/60).toString().padStart(2,'0');const s=(timeLeft%60).toString().padStart(2,'0');timerEl.textContent=`${m}:${s}`;}
function endGame(){questionEl.textContent=`Game over! Final score: ${score}`;answersEl.innerHTML='<div style="padding:12px;color:#445">Thanks for playing — click Restart to play again.</div>';progressEl.textContent=`Questions played: ${Math.min(pool.length,current)}`;nextBtn.disabled=true;clearInterval(tickInterval);}
function shuffle(a){for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];}}
function burstConfetti(){const canvas=document.getElementById('confetti');const ctx=canvas.getContext('2d');const ratio=window.devicePixelRatio||1;canvas.width=window.innerWidth*ratio;canvas.height=200*ratio;canvas.style.height='200px';canvas.style.top='0';ctx.scale(ratio,ratio);let pieces=[];for(let i=0;i<40;i++){pieces.push({x:Math.random()*window.innerWidth,y:0,vx:(Math.random()-0.5)*6,vy:2+Math.random()*5,rot:Math.random()*360,size:6+Math.random()*6,color:`hsl(${Math.random()*360},80%,60%)`});}let t=0;function frame(){ctx.clearRect(0,0,window.innerWidth,200);t++;pieces.forEach(p=>{p.x+=p.vx;p.y+=p.vy;p.vy+=0.2;ctx.save();ctx.translate(p.x,p.y);ctx.rotate(p.rot*(Math.PI/180));ctx.fillStyle=p.color;ctx.fillRect(-p.size/2,-p.size/2,p.size,p.size);ctx.restore();});if(t<90)requestAnimationFrame(frame);else ctx.clearRect(0,0,window.innerWidth,200);}requestAnimationFrame(frame);}
startGame();
</script>
</body>
</html>
