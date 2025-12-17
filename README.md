# wasgood1234.github.io
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>三角比 選択式テスト</title>
<style>
  body {
    font-family: system-ui, -apple-system, sans-serif;
    background: #f5f5f5;
    padding: 20px;
  }
  .card {
    background: white;
    padding: 20px;
    border-radius: 12px;
    max-width: 420px;
    margin: auto;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  }
  h1 { text-align: center; }
  .choice {
    margin: 10px 0;
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 8px;
    background: #fafafa;
    cursor: pointer;
    font-size: 18px;
  }
  .choice:hover { background: #eaeaea; }
  .correct { background: #c8f7c5; }
  .wrong { background: #f7c5c5; }
  button {
    width: 100%;
    padding: 12px;
    font-size: 18px;
    margin-top: 15px;
  }
</style>
</head>
<body>

<div class="card">
  <h1>三角比テスト</h1>
  <div id="question"></div>
  <div id="choices"></div>
  <button onclick="nextQuestion()">次の問題</button>
  <p id="result"></p>
</div>

<script>
const angles = [
  ["π/6", 30], ["π/4", 45], ["π/3", 60], ["π/2", 90],
  ["2π/3", 120], ["3π/4", 135], ["5π/6", 150], ["π", 180]
];

const values = {
  "sin30":"1/2","cos30":"√3/2","tan30":"1/√3",
  "sin45":"1/√2","cos45":"1/√2","tan45":"1",
  "sin60":"√3/2","cos60":"1/2","tan60":"√3",
  "sin90":"1","cos90":"0","tan90":"定義されない",
  "sin120":"√3/2","cos120":"-1/2","tan120":"-√3",
  "sin135":"1/√2","cos135":"-1/√2","tan135":"-1",
  "sin150":"1/2","cos150":"-√3/2","tan150":"-1/√3",
  "sin180":"0","cos180":"-1","tan180":"0"
};

const funcs = ["sin","cos","tan"];
let score = 0;
let count = 0;
const TOTAL = 10;
let correctAnswer = "";

function nextQuestion() {
  if (count >= TOTAL) {
    document.getElementById("question").innerHTML =
      `<h2>結果：${score} / ${TOTAL}</h2>`;
    document.getElementById("choices").innerHTML = "";
    document.getElementById("result").innerText =
      score >= 7 ? "🎉 合格！" : "❌ もう一回！";
    return;
  }

  count++;
  document.getElementById("result").innerText = "";

  const [rad, deg] = angles[Math.floor(Math.random()*angles.length)];
  const func = funcs[Math.floor(Math.random()*funcs.length)];
  correctAnswer = values[func + deg];

  let choices = Object.values(values)
    .sort(() => 0.5 - Math.random())
    .slice(0,3);

  if (!choices.includes(correctAnswer)) choices.push(correctAnswer);
  choices = choices.sort(() => 0.5 - Math.random());

  document.getElementById("question").innerHTML =
    `<h2>第${count}問：${func}(${rad} ラジアン)</h2>`;

  const choicesDiv = document.getElementById("choices");
  choicesDiv.innerHTML = "";

  choices.forEach(c => {
    const div = document.createElement("div");
    div.className = "choice";
    div.innerText = c;
    div.onclick = () => checkAnswer(div, c);
    choicesDiv.appendChild(div);
  });
}

function checkAnswer(div, answer) {
  const all = document.querySelectorAll(".choice");
  all.forEach(c => c.onclick = null);

  if (answer === correctAnswer) {
    div.classList.add("correct");
    score++;
    document.getElementById("result").innerText = "○ 正解！";
  } else {
    div.classList.add("wrong");
    document.getElementById("result").innerText =
      `× 不正解（正解：${correctAnswer}）`;
  }
}

nextQuestion();
</script>

</body>
</html>
