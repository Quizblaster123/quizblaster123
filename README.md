<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Quiz Blast! — Fun Cartoon Quiz</title>

<style>
:root{--bg:#f7fbff;--card:#fff;--accent:#ffcc33;--accent2:#66ccff;--text:#123;}
*{box-sizing:border-box}
body{
  font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,'Helvetica Neue',Arial;
  background:linear-gradient(180deg,var(--bg),#e8f6ff);
  margin:0; min-height:100vh;
  display:flex; align-items:center; justify-content:center;
  padding:24px;
}

/* ★ WIDER PANEL */
.wrapper{
  width:100%;
  max-width:1200px;
}

.card{
  background:var(--card); border-radius:18px;
  padding:20px;
  box-shadow:0 8px 30px rgba(3,20,40,0.08)
}

/* ★ Glow effect for your logo */
.logo img{
  border-radius:14px;
  width:74px;
  height:74px;
  object-fit:cover;
  box-shadow:0 0 14px rgba(0,200,255,0.55);
  animation: logoGlow 3s ease-in-out infinite alternate;
}

@keyframes logoGlow {
  from { box-shadow:0 0 10px rgba(0,180,255,0.4); }
  to   { box-shadow:0 0 22px rgba(255,130,255,0.55); }
}

header{display:flex; gap:16px; align-items:center}
.logo{display:flex; gap:12px; align-items:center}
.title{font-size:22px; font-weight:700; color:var(--text)}
.subtitle{font-size:12px; color:#346; opacity:0.85}

/* ★ WIDER QUIZ AREA */
main{
  display:grid;
  grid-template-columns:1.5fr 320px;
  gap:18px;
  margin-top:16px;
}

@media(max-width:880px){
  main{grid-template-columns:1fr}
  .side{order:2}
}

.quiz-area{padding:18px}
.question{font-size:18px;margin-bottom:12px}

.answers{display:grid;gap:10px}
.ans{
  background:#f4fbff;border:2px solid rgba(102,204,255,0.15);
  padding:12px;border-radius:10px;cursor:pointer;
  transition:transform .12s ease, box-shadow .12s;
}
.ans:hover{transform:translateY(-4px); box-shadow:0 8px 20px rgba(10,50,80,0.06)}
.ans.correct{
  background:linear-gradient(90deg,#d1ffc7,#b8ffd0);
  border-color:#75d44b
}
.ans.wrong{
  background:linear-gradient(90deg,#ffd6d6,#ffcfcf);
  border-color:#ff6b6b
}

.meta{
  display:flex;justify-content:space-between;
  align-items:center;margin-top:12px;
  font-size:13px;color:#334
}

.side{
  padding:18px;background:linear-gradient(180deg,#f8ffff,#ffffff);
  border-radius:12px;border:1px solid rgba(15,50,80,0.03)
}

.score{font-size:40px;font-weight:800;color:#0b6e4f}
.small{font-size:12px;color:#456;margin-top:6px}

.btn{
  display:inline-block;padding:10px 14px;
  border-radius:12px;background:linear-gradient(90deg,var(--accent2),#9be6ff);
  cursor:pointer;border:none;font-weight:700
}

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

<header>
  <div class="logo">

    <!-- ★ Your Logo Here -->
    <img src="C:\Users\IT\Downloads/quizblaster logo.jpg" alt="Quiz Blaster Logo">

    <div>
      <div class="title">Quiz Blast!</div>
      <div class="subtitle">fast, fun & cartoon — code included 🎮</div>
    </div>

  </div>
</header>

<main>

<section class="quiz-area cardless">
  <div id="questionBox">
    <div class="question" id="question">Loading question...</div>
    <div class="answers" id="answers"></div>
    <div class="meta">
      <div id="progress">Question 0 / 0</div>
      <div id="timer">--:--</div>
    </div>
  </div>

  <div style="margin-top:12px;display:flex;gap:8px;justify-content:flex-end">
    <button class="btn" id="nextBtn" disabled>Next</button>
    <button class="btn" id="restartBtn" style="background:linear-gradient(90deg,#ffd36b,#ffcc99)">Restart</button>
  </div>
</section>

<aside class="side">
  <div style="display:flex;align-items:center;justify-content:space-between">
    <div>
      <div style="font-size:13px;color:#345">Score</div>
      <div class="score" id="score">0</div>
    </div>
    <div style="text-align:right">
      <div style="font-size:13px;color:#345">Lives</div>
      <div style="font-weight:800;color:#cc4b4b" id="lives">3</div>
    </div>
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
/* ⭐ All your JavaScript stays the same — unchanged */
</script>

</body>
</html>
