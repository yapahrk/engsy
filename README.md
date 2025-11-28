
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>เกมเติมตัวอักษรที่หายไป (Missing Letters)</title>
  <style>
    :root{
      --bg:#f7fbff;
      --card:#ffffff;
      --accent:#2463ff;
      --muted:#6b7280;
      --success:#16a34a;
      --danger:#ef4444;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background:linear-gradient(180deg,#eaf2ff 0%, var(--bg) 100%);
      color:#0f172a;
      padding:24px;
      display:flex;
      align-items:center;
      justify-content:center;
      min-height:100vh;
    }
    .app{
      width:100%;
      max-width:920px;
      background:var(--card);
      border-radius:12px;
      box-shadow:0 8px 30px rgba(12, 31, 68, 0.08);
      padding:20px;
    }
    header{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      margin-bottom:12px;
    }
    header h1{
      font-size:20px;
      margin:0;
    }
    .controls{
      display:flex;
      gap:8px;
      align-items:center;
    }
    button{
      background:var(--accent);
      color:white;
      border:none;
      padding:8px 12px;
      border-radius:8px;
      cursor:pointer;
      font-weight:600;
    }
    button.secondary{
      background:#f1f5f9;
      color:#0f172a;
      border:1px solid #e6eefc;
    }
    button.ghost{
      background:transparent;
      color:var(--muted);
      border:1px dashed #e6eefc;
    }
    main{display:flex;gap:20px;flex-wrap:wrap}
    .left, .right{flex:1;min-width:260px}
    .card{
      background:linear-gradient(180deg,#ffffff,#fbfdff);
      border-radius:10px;
      padding:18px;
      box-shadow:0 4px 18px rgba(11,22,50,0.04);
    }
    .word-line{
      font-size:28px;
      font-weight:700;
      display:flex;
      gap:8px;
      align-items:center;
      flex-wrap:wrap;
    }
    .letter-box{
      min-width:36px;
      height:46px;
      border-radius:8px;
      border:2px solid #e6eefc;
      display:inline-flex;
      align-items:center;
      justify-content:center;
      font-size:20px;
      font-weight:700;
      background:#fff;
    }
    .letter-box input{
      width:100%;
      height:100%;
      border:0;
      outline:0;
      font-size:20px;
      font-weight:700;
      text-align:center;
      background:transparent;
    }
    .meta{
      margin-top:12px;
      color:var(--muted);
      font-size:14px;
    }
    .hint{
      font-size:18px;
      display:flex;
      gap:8px;
      align-items:center;
    }
    .score{
      font-weight:700;
      color:var(--accent);
      font-size:18px;
    }
    .result{
      margin-top:10px;
      font-weight:700;
    }
    .correct{color:var(--success)}
    .wrong{color:var(--danger)}
    .progress{
      height:10px;
      background:#eef2ff;
      border-radius:6px;
      overflow:hidden;
      margin-top:8px;
    }
    .progress > i{
      display:block;
      height:100%;
      background:linear-gradient(90deg,var(--accent),#5aa0ff);
      width:0%;
    }
    .toolbar{display:flex;gap:8px;margin-top:12px;flex-wrap:wrap}
    footer{margin-top:14px;font-size:13px;color:var(--muted);text-align:center}
    .emoji{
      font-size:40px;
      display:inline-block;
      width:64px;
      height:64px;
      text-align:center;
      line-height:64px;
      border-radius:10px;
      background:linear-gradient(180deg,#fff,#f7fbff);
      border:1px solid #eef6ff;
      box-shadow:0 6px 18px rgba(36,99,255,0.06);
    }
    @media (max-width:720px){
      .word-line{font-size:22px}
      .letter-box{min-width:30px;height:40px}
    }
  </style>
</head>
<body>
  <div class="app" role="application" aria-label="Missing letters English game">
    <header>
      <div>
        <h1>เติมตัวอักษรที่หายไป — Missing Letters</h1>
        <div class="meta">ระดับ: B1 • แบบทดสอบคำศัพท์ มีภาพและคำใบ้</div>
      </div>
      <div class="controls">
        <div class="score" id="score">Score: 0</div>
        <button id="show-answer" class="secondary" title="Show answer">เฉลย</button>
        <button id="next" title="Next">ข้อถัดไป</button>
      </div>
    </header>

    <main>
      <div class="left">
        <div class="card" id="card-main">
          <div style="display:flex;gap:14px;align-items:center;">
            <div class="emoji" id="emoji">🌊</div>
            <div style="flex:1">
              <div class="hint" id="hint">Hint: A natural place where water falls from a height</div>
              <div class="meta" id="category">Category: Environment</div>
              <div class="progress" aria-hidden="true" style="margin-top:10px"><i id="progress-bar"></i></div>
            </div>
          </div>

          <div style="margin-top:18px">
            <div class="word-line" id="word-line" aria-label="Word with missing letters"></div>
            <div class="toolbar">
              <button id="check">ตรวจคำ (Check)</button>
              <button id="clear" class="secondary">ล้าง (Clear)</button>
            </div>
            <div class="result" id="result"></div>
          </div>
        </div>
      </div>

      <div class="right">
        <div class="card">
          <h3 style="margin-top:0">คำศัพท์ที่ใช้ (Preview)</h3>
          <p style="color:var(--muted)">ระบบจะสุ่มแสดงคำศัพท์จากลิสต์ต่อไปนี้ทีละคำ — เมื่อกด "เฉลย" จะเห็นคำเต็ม</p>
          <ul id="word-list" style="padding-left:1rem"></ul>
          <hr />
          <div style="display:flex;gap:8px;flex-wrap:wrap;margin-top:12px">
            <button id="restart" class="ghost">เริ่มใหม่ (Restart)</button>
            <button id="shuffle" class="ghost">สลับคำ (Shuffle)</button>
          </div>
        </div>
      </div>
    </main>

    <footer>
      เคล็ดลับ: กำหนดให้ไฟล์นี้เป็นกิจกรรมออนไลน์โดยเพิ่มรูปภาพจริงแทน emoji ได้ (แก้ในตัวแปรคำศัพท์) • พัฒนาได้ตามต้องการ
    </footer>
  </div>

  <script>
    // ข้อมูลคำศัพท์: word, category, hint, emoji, maskIndices
    // maskIndices = array ของตำแหน่งตัวอักษรที่จะซ่อน (0-based)
    const words = [
      {word:'waterfall', category:'Environment', hint:'A natural place where water falls from a height', emoji:'🌊', mask:[1,2,5]},
      {word:'earthquake', category:'Environment', hint:'A sudden shaking of the ground', emoji:'🌍', mask:[2,5,8]},
      {word:'pollution', category:'Environment', hint:'Contamination of air, water, or soil', emoji:'🏭', mask:[1,4,7]},
      {word:'wildlife', category:'Environment', hint:'Animals living in natural conditions', emoji:'🦌', mask:[1,4]},
      {word:'recycle', category:'Environment', hint:'To process used items so they can be used again', emoji:'♻️', mask:[2,4]},
      {word:'pedestrian', category:'Transportation', hint:'A person walking in a town', emoji:'🚶', mask:[0,3,6]},
      {word:'departure', category:'Transportation', hint:'The time when a vehicle leaves', emoji:'🚆', mask:[1,4,7]},
      {word:'destination', category:'Transportation', hint:'The place you are going to', emoji:'📍', mask:[2,6,9]},
      {word:'schedule', category:'Transportation', hint:'A plan that shows times', emoji:'🕘', mask:[1,3]},
      {word:'highway', category:'Transportation', hint:'A main road for fast traffic', emoji:'🛣️', mask:[2,4]},
      {word:'device', category:'Technology', hint:'A machine or tool used for a purpose', emoji:'📱', mask:[1,3]},
      {word:'upload', category:'Technology', hint:'To send data to a server', emoji:'⬆️', mask:[1,4]},
      {word:'password', category:'Technology', hint:'A secret word to access accounts', emoji:'🔐', mask:[1,4,6]},
      {word:'connection', category:'Technology', hint:'The link between devices or networks', emoji:'🔗', mask:[2,5,8]},
      {word:'digital', category:'Technology', hint:'Involving computers or electronic devices', emoji:'💻', mask:[1,4]},
      {word:'flashlight', category:'Everyday', hint:'Handheld light for dark places', emoji:'🔦', mask:[2,5,8]},
      {word:'backpack', category:'Everyday', hint:'Bag carried on the back', emoji:'🎒', mask:[1,4]},
      {word:'calculator', category:'Everyday', hint:'A device for doing math', emoji:'🧮', mask:[2,5,8]},
      {word:'dictionary', category:'Everyday', hint:'A book that defines words', emoji:'📘', mask:[1,4,7]},
      {word:'microscope', category:'Everyday', hint:'Instrument to see tiny things', emoji:'🔬', mask:[2,6]}
    ];

    // app state
    let order = [...Array(words.length).keys()];
    let index = 0;
    let score = 0;
    let current = null;
    const scoreEl = document.getElementById('score');
    const hintEl = document.getElementById('hint');
    const categoryEl = document.getElementById('category');
    const emojiEl = document.getElementById('emoji');
    const wordLine = document.getElementById('word-line');
    const resultEl = document.getElementById('result');
    const progressBar = document.getElementById('progress-bar');
    const wordListEl = document.getElementById('word-list');

    function init(){
      // fill preview list
      wordListEl.innerHTML = words.map(w=>`<li>${w.word} — <small style="color:var(--muted)">${w.category}</small></li>`).join('');
      shuffleOrder();
      index = 0;
      score = 0;
      updateScore();
      loadCurrent();
    }

    function shuffleOrder(){
      // Fisher-Yates
      for(let i=order.length-1;i>0;i--){
        const j = Math.floor(Math.random()*(i+1));
        [order[i],order[j]] = [order[j],order[i]];
      }
    }

    function loadCurrent(){
      resultEl.textContent = '';
      const id = order[index % order.length];
      current = JSON.parse(JSON.stringify(words[id])); // clone
      // prepare masked representation
      current.chars = current.word.split('');
      current.masked = current.chars.map((ch,i)=> current.mask.includes(i) ? '' : ch);
      render();
      updateProgress();
    }

    function render(){
      // header parts
      emojiEl.textContent = current.emoji || '🔤';
      hintEl.textContent = 'Hint: ' + current.hint;
      categoryEl.textContent = 'Category: ' + current.category;

      // build word-line: show letter boxes; for masked indices show input
      wordLine.innerHTML = '';
      current.chars.forEach((ch,i)=>{
        const box = document.createElement('span');
        box.className = 'letter-box';
        if(current.mask.includes(i)){
          const input = document.createElement('input');
          input.setAttribute('maxlength','1');
          input.setAttribute('aria-label','Letter '+(i+1));
          input.value = current.masked[i] || '';
          input.addEventListener('input', onInput);
          input.addEventListener('keydown', onKeyDown);
          box.appendChild(input);
        } else {
          box.textContent = ch;
          box.setAttribute('aria-hidden','true');
        }
        wordLine.appendChild(box);
      });

      // focus first input
      const firstInput = wordLine.querySelector('input');
      if(firstInput) firstInput.focus();
    }

    function onInput(e){
      // uppercase to lowercase normalized
      e.target.value = e.target.value.slice(-1).toLowerCase();
    }

    function onKeyDown(e){
      // navigate inputs with arrow keys/backspace
      const inputs = Array.from(wordLine.querySelectorAll('input'));
      const idx = inputs.indexOf(e.target);
      if(e.key === 'ArrowRight' && idx < inputs.length-1) inputs[idx+1].focus();
      if(e.key === 'ArrowLeft' && idx > 0) inputs[idx-1].focus();
      if(e.key === 'Backspace' && e.target.value === '' && idx>0){
        inputs[idx-1].focus();
      }
      if(e.key === 'Enter') checkAnswer();
    }

    function getUserAnswer(){
      const inputs = Array.from(wordLine.querySelectorAll('input'));
      const userChars = current.chars.slice(); // copy
      let maskedPositions = current.mask.slice().sort((a,b)=>a-b);
      inputs.forEach((inp,i)=>{
        const pos = maskedPositions[i];
        userChars[pos] = (inp.value || '').toLowerCase();
      });
      return userChars.join('');
    }

    function checkAnswer(){
      const user = getUserAnswer();
      const correct = current.word.toLowerCase();
      if(user === correct){
        resultEl.innerHTML = '<span class="correct">ถูกต้อง! ✓</span>';
        score += 10;
        updateScore();
        // highlight boxes green briefly
        flashResult(true);
      } else {
        resultEl.innerHTML = `<span class="wrong">ยังไม่ถูกต้อง — คุณใส่: <strong>${escapeHtml(user)}</strong></span>`;
        score -= 2;
        if(score < 0) score = 0;
        updateScore();
        flashResult(false);
      }
    }

    function flashResult(isCorrect){
      wordLine.style.transition = 'box-shadow 0.15s';
      wordLine.style.boxShadow = isCorrect ? '0 6px 20px rgba(22,163,74,0.08)' : '0 6px 20px rgba(239,68,68,0.08)';
      setTimeout(()=>wordLine.style.boxShadow = 'none',600);
    }

    function showAnswer(){
      // fill inputs with correct letters
      const inputs = Array.from(wordLine.querySelectorAll('input'));
      let maskedPositions = current.mask.slice().sort((a,b)=>a-b);
      inputs.forEach((inp,i)=>{
        const pos = maskedPositions[i];
        inp.value = current.chars[pos];
      });
      resultEl.innerHTML = `<span class="muted">เฉลย: <strong>${current.word}</strong></span>`;
    }

    function next(){
      index++;
      if(index >= order.length) {
        // finished — reshuffle and continue
        shuffleOrder();
        index = 0;
      }
      loadCurrent();
    }

    function clearInputs(){
      Array.from(wordLine.querySelectorAll('input')).forEach(i=>i.value='');
      const first = wordLine.querySelector('input');
      if(first) first.focus();
      resultEl.textContent = '';
    }

    function updateScore(){
      scoreEl.textContent = 'Score: ' + score;
    }

    function updateProgress(){
      const pct = Math.round(((index % order.length) / order.length) * 100);
      progressBar.style.width = pct + '%';
    }

    function restart(){
      score = 0;
      updateScore();
      shuffleOrder();
      index = 0;
      loadCurrent();
    }

    function shuffleList(){
      shuffleOrder();
      index = 0;
      loadCurrent();
    }

    // small helper
    function escapeHtml(s){
      return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
    }

    // event bindings
    document.getElementById('check').addEventListener('click', checkAnswer);
    document.getElementById('clear').addEventListener('click', clearInputs);
    document.getElementById('next').addEventListener('click', next);
    document.getElementById('show-answer').addEventListener('click', showAnswer);
    document.getElementById('restart').addEventListener('click', restart);
    document.getElementById('shuffle').addEventListener('click', shuffleList);

    // keyboard shortcut: N = next, C = check
    document.addEventListener('keydown', (e)=>{
      if(e.key.toLowerCase() === 'n') next();
      if(e.key.toLowerCase() === 'c') checkAnswer();
    });

    // initialize app
    init();
  </script>
</body>
</html>
