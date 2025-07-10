# JD-Flashcard- 
<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Nigerian History Flashcard Game</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #0a3d2c;
      color: #fff;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .container {
      width: 90%;
      max-width: 700px;
      background: #114c3f;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 0 20px rgba(0,0,0,0.3);
    }
    h1 {
      text-align: center;
      color: #fdd835;
    }
    .mode-switch {
      text-align: center;
      margin-bottom: 20px;
    }
    .question {
      margin: 20px 0;
    }
    .options button {
      display: block;
      width: 100%;
      padding: 10px;
      margin: 5px 0;
      background-color: #2ecc71;
      color: white;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      transition: background-color 0.3s;
      cursor: pointer;
    }
    .options button:hover {
      background-color: #27ae60;
    }
    .feedback {
      font-weight: bold;
      margin: 10px 0;
    }
    .controls {
      display: flex;
      justify-content: space-between;
      margin-top: 20px;
    }
    #timer {
      font-weight: bold;
      color: #f39c12;
    }
    #jd {
      text-align: center;
      margin-top: 30px;
      color: #ccc;
      font-size: 12px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Nigerian History Flashcards</h1>
    <div class="mode-switch">
      <button onclick="startGame('practice')">Practice Mode</button>
      <button onclick="startGame('exam')">Exam Mode</button>
    </div>
    <div id="game" style="display:none;">
      <div id="timer"></div>
      <div class="question" id="question"></div>
      <div class="options" id="options"></div>
      <div class="feedback" id="feedback"></div>
      <div class="controls">
        <button onclick="nextQuestion()">Next</button>
        <div id="score"></div>
      </div>
    </div>
    <div id="jd">&copy; JD CREATIONS</div>
  </div>  <script>
    const flashcards = [
  {q: "Who was known as the 'Father of Nigerian Nationalism'?", options: ["A) Nnamdi Azikiwe", "B) Ahmadu Bello", "C) Herbert Macaulay", "D) Tafawa Balewa"], answer: "C", explanation: "Herbert Macaulay led early nationalist efforts and was a vocal critic of colonial rule."},
  {q: "When was Nigeria granted independence?", options: ["A) 1954", "B) 1960", "C) 1966", "D) 1999"], answer: "B", explanation: "Nigeria gained independence on October 1, 1960."},
  {q: "Who founded the Action Group political party?", options: ["A) Tafawa Balewa", "B) Obafemi Awolowo", "C) Ahmadu Bello", "D) Nnamdi Azikiwe"], answer: "B", explanation: "Obafemi Awolowo formed the Action Group in 1951."},
  {q: "What major political reform did the Clifford Constitution introduce?", options: ["A) Federalism", "B) Adult suffrage", "C) Elected African representation", "D) Self-government"], answer: "C", explanation: "It allowed limited African representation in the legislative council."},
  {q: "What year did the first military coup occur in Nigeria?", options: ["A) 1960", "B) 1963", "C) 1966", "D) 1975"], answer: "C", explanation: "The first coup happened on January 15, 1966."},
  {q: "Who succeeded General Aguiyi Ironsi as Head of State?", options: ["A) Murtala Mohammed", "B) Yakubu Gowon", "C) Olusegun Obasanjo", "D) Ibrahim Babangida"], answer: "B", explanation: "Gowon became Head of State after the July 1966 coup."},
  {q: "Who moved the motion for Nigerian independence?", options: ["A) Herbert Macaulay", "B) Anthony Enahoro", "C) Tafawa Balewa", "D) Nnamdi Azikiwe"], answer: "B", explanation: "Anthony Enahoro moved the motion for independence in 1953."},
  {q: "The slogan \"No Victor, No Vanquished\" was declared after what event?", options: ["A) First coup", "B) Nigeria’s independence", "C) Nigerian Civil War", "D) 1979 elections"], answer: "C", explanation: "Declared by Yakubu Gowon after the Nigerian Civil War in 1970."},
  {q: "What caused the Nigerian Civil War?", options: ["A) Oil dispute", "B) Ethnic violence", "C) Refusal to implement the Aburi Accord", "D) Elections"], answer: "C", explanation: "The failure of Aburi Accord implementation led to the war."},
  {q: "Which country supported Biafra during the civil war?", options: ["A) France", "B) USA", "C) Ghana", "D) UK"], answer: "A", explanation: "France provided limited support to Biafra during the war."},
  {q: "What year did the civil war end?", options: ["A) 1967", "B) 1970", "C) 1973", "D) 1980"], answer: "B", explanation: "The civil war ended in January 1970."},
  {q: "What system of government was introduced by the Macpherson Constitution?", options: ["A) Military rule", "B) Federal system", "C) Centralized government", "D) Monarchy"], answer: "B", explanation: "It introduced a federal system in Nigeria."},
  {q: "What was the main goal of the nationalist movements in Nigeria?", options: ["A) Trade", "B) Education", "C) Self-government", "D) Religion"], answer: "C", explanation: "Their primary goal was to achieve self-rule."},
  {q: "Who edited the West African Pilot?", options: ["A) Ernest Ikoli", "B) Nnamdi Azikiwe", "C) Ahmadu Bello", "D) Obafemi Awolowo"], answer: "B", explanation: "Azikiwe was the editor of the West African Pilot."},
  {q: "Which region did the NPC dominate?", options: ["A) East", "B) West", "C) North", "D) Midwest"], answer: "C", explanation: "The Northern People's Congress was dominant in the North."},
  {q: "The 1914 Amalgamation united which two regions?", options: ["A) East & West", "B) North & South", "C) North & East", "D) West & Midwest"], answer: "B", explanation: "It united Northern and Southern protectorates."},
  {q: "Who was the first president of Nigeria?", options: ["A) Tafawa Balewa", "B) Yakubu Gowon", "C) Nnamdi Azikiwe", "D) Murtala Mohammed"], answer: "C", explanation: "Azikiwe became Nigeria’s first President in 1963."},
  {q: "Which constitution gave Nigeria full independence?", options: ["A) Clifford", "B) Lyttleton", "C) 1960 Independence Constitution", "D) 1979 Constitution"], answer: "C", explanation: "It was enacted when Nigeria became independent."},
  {q: "The military government ruled Nigeria for how many cumulative years?", options: ["A) 10", "B) 14", "C) 29", "D) 34"], answer: "C", explanation: "From 1966–1999, except 1979–1983: 29 years total."},
  {q: "Which leader created 12 states in 1967?", options: ["A) Gowon", "B) Ironsi", "C) Obasanjo", "D) Shagari"], answer: "A", explanation: "Yakubu Gowon created 12 states to weaken Biafra’s cause."},
  {q: "When did Nigeria become a republic?", options: ["A) 1960", "B) 1963", "C) 1979", "D) 1999"], answer: "B", explanation: "Nigeria became a republic on October 1, 1963."},
  {q: "The Aburi Accord was signed in which country?", options: ["A) Nigeria", "B) Ghana", "C) Cameroon", "D) South Africa"], answer: "B", explanation: "It was signed in Ghana to prevent civil war."},
  {q: "Who was the military ruler before Shehu Shagari?", options: ["A) Murtala Mohammed", "B) Obasanjo", "C) Babangida", "D) Buhari"], answer: "B", explanation: "Obasanjo handed power to Shagari in 1979."},
  {q: "The Zikist Movement was inspired by which leader?", options: ["A) Nkrumah", "B) Azikiwe", "C) Macaulay", "D) Awolowo"], answer: "B", explanation: "Named after Azikiwe, the Zikist Movement was radical."},
  {q: "What was the motto during the civil war?", options: ["A) One Nigeria", "B) Unity is strength", "C) No victor, no vanquished", "D) To keep Nigeria one is a task that must be done"], answer: "D", explanation: "Used by federal forces to justify the war effort."},
  {q: "What political party did Nnamdi Azikiwe lead?", options: ["A) AG", "B) NPC", "C) NCNC", "D) PDP"], answer: "C", explanation: "He was a leading figure in the National Council of Nigeria and the Cameroons."},
  {q: "Which constitution first introduced regional assemblies?", options: ["A) Clifford", "B) Macpherson", "C) Richards", "D) Lyttleton"], answer: "C", explanation: "Richards Constitution of 1946 introduced regions."},
  {q: "Who was the first Nigerian Governor-General?", options: ["A) Azikiwe", "B) Macaulay", "C) Awolowo", "D) Ironsi"], answer: "A", explanation: "Azikiwe held this title in 1960 before becoming president."},
  {q: "When did the Second Republic begin?", options: ["A) 1966", "B) 1979", "C) 1983", "D) 1993"], answer: "B", explanation: "The Second Republic started with Shagari’s presidency in 1979."},
  {q: "Who founded the Nigerian Youth Movement?", options: ["A) Obafemi Awolowo", "B) Ernest Ikoli", "C) Azikiwe", "D) Macaulay"], answer: "B", explanation: "Ernest Ikoli was one of its founders."}
];

    let mode = 'practice';
    let index = 0;
    let score = 0;
    let timer;
    let timeLeft = 900;

    function startGame(selectedMode) {
      mode = selectedMode;
      index = 0;
      score = 0;
      document.getElementById('game').style.display = 'block';
      document.querySelector('.mode-switch').style.display = 'none';
      if (mode === 'exam') {
        timeLeft = 900;
        timer = setInterval(updateTimer, 1000);
      }
      loadQuestion();
    }

    function updateTimer() {
      if (timeLeft <= 0) {
        clearInterval(timer);
        alert("Time's up! Your final score is: " + score);
        return;
      }
      const mins = Math.floor(timeLeft / 60);
      const secs = timeLeft % 60;
      document.getElementById('timer').textContent = `Time left: ${mins}:${secs.toString().padStart(2, '0')}`;
      timeLeft--;
    }

    function loadQuestion() {
      const card = flashcards[index];
      document.getElementById('question').textContent = `Q${index + 1}: ${card.q}`;
      const opts = document.getElementById('options');
      opts.innerHTML = '';
      card.options.forEach(opt => {
        const btn = document.createElement('button');
        btn.textContent = opt;
        btn.onclick = () => checkAnswer(opt.charAt(0));
        opts.appendChild(btn);
      });
      document.getElementById('feedback').textContent = '';
    }

    function checkAnswer(choice) {
      const correct = flashcards[index].answer;
      const feedback = document.getElementById('feedback');
      if (choice === correct) {
        feedback.textContent = "✅ Correct!";
        feedback.style.color = 'lightgreen';
        score++;
      } else {
        feedback.textContent = `❌ Wrong! Correct answer: ${correct}. ${flashcards[index].explanation}`;
        feedback.style.color = 'red';
      }
      document.getElementById('score').textContent = `Score: ${score}/${flashcards.length}`;
    }

    function nextQuestion() {
      index++;
      if (index >= flashcards.length) {
        clearInterval(timer);
        document.getElementById('question').textContent = '🎉 Quiz Complete!';
        document.getElementById('options').innerHTML = '';
        document.getElementById('feedback').textContent = `Final Score: ${score}/${flashcards.length}`;
        return;
      }
      loadQuestion();
    }
  </script></body>
</html>
