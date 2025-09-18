# dse-
關於中文DSE文言文語譯練習
<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DSE文言文語譯練習</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: "Microsoft JhengHei", "Heiti TC", sans-serif; }
        body { background-color: #f5f5f5; color: #333; line-height: 1.6; padding: 20px; }
        .container { max-width: 1000px; margin: 0 auto; }
        header { background-color: #8e44ad; color: white; padding: 20px; text-align: center; border-radius: 8px; margin-bottom: 20px; }
        h1 { font-size: 1.8rem; margin-bottom: 10px; }
        .section { background: white; border-radius: 8px; padding: 15px; margin-bottom: 20px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .section-title { color: #8e44ad; border-bottom: 2px solid #8e44ad; padding-bottom: 10px; margin-bottom: 15px; }
        .classical-text { font-size: 1.1rem; line-height: 1.8; background: #f9f9f9; padding: 15px; border-radius: 5px; margin-bottom: 15px; }
        textarea { width: 100%; height: 120px; padding: 12px; border: 1px solid #ddd; border-radius: 5px; font-size: 1rem; margin-bottom: 10px; }
        button { background: #8e44ad; color: white; border: none; padding: 12px 20px; border-radius: 5px; font-size: 1rem; cursor: pointer; }
        .difficulty-btn { background: #e0e0e0; color: #333; margin-right: 5px; }
        .difficulty-btn.active { background: #8e44ad; color: white; }
        .score-display { font-size: 1.2rem; font-weight: bold; color: #8e44ad; margin: 10px 0; }
        .comparison-box { background: #f9f9f9; padding: 15px; border-radius: 5px; margin-top: 15px; }
        footer { text-align: center; margin-top: 30px; color: #666; }
        @media (max-width: 768px) {
            h1 { font-size: 1.5rem; }
            .classical-text { font-size: 1rem; }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>DSE文言文語譯練習</h1>
            <p>提升文言文理解與翻譯能力，為DSE中文科做好準備</p>
        </header>
        
        <div class="section">
            <div style="display: flex; gap: 10px; margin-bottom: 15px;">
                <button class="difficulty-btn active" onclick="setDifficulty('easy')">初級</button>
                <button class="difficulty-btn" onclick="setDifficulty('medium')">中級</button>
                <button class="difficulty-btn" onclick="setDifficulty('hard')">高級</button>
                <button onclick="nextQuestion()" style="margin-left: auto;">下一題</button>
            </div>
            
            <h2 class="section-title">文言文篇章</h2>
            <div id="classical-text" class="classical-text">載入中...</div>
            
            <h2 class="section-title">語譯練習</h2>
            <textarea id="translation-input" placeholder="請在此輸入您的白話文翻譯..."></textarea>
            <button onclick="checkAnswer()" style="width: 100%;">檢查答案</button>
            
            <div id="score" class="score-display">得分: 0</div>
        </div>
        
        <div class="section">
            <h2 class="section-title">參考答案與解析</h2>
            <div id="reference-answer" class="comparison-box">
                <p>提交您的翻譯後，這裡會顯示參考答案和解析。</p>
            </div>
            
            <div style="display: flex; gap: 15px; margin-top: 15px;">
                <div style="flex: 1;">
                    <h3>您的翻譯</h3>
                    <div id="user-translation" class="comparison-box"></div>
                </div>
                <div style="flex: 1;">
                    <h3>參考翻譯</h3>
                    <div id="reference-translation" class="comparison-box"></div>
                </div>
            </div>
        </div>
        
        <footer>
            <p>© 2023 DSE文言文語譯練習應用 | 設計用於教育目的</p>
        </footer>
    </div>

    <script>
        // 題庫數據
        const questions = {
            easy: [
                {
                    classical: "子曰：「學而時習之，不亦說乎？有朋自遠方來，不亦樂乎？人不知而不慍，不亦君子乎？」",
                    reference: "孔子說：「學習後時常溫習，不也很愉快嗎？有朋友從遠方來，不也很快樂嗎？別人不了解我，我也不生氣，不也是君子嗎？」",
                    explanation: "這是《論語》的開篇，強調學習、交友和修養的重要性。"
                },
                {
                    classical: "晉太元中，武陵人捕魚為業。緣溪行，忘路之遠近。忽逢桃花林，夾岸數百步，中無雜樹，芳草鮮美，落英繽紛。",
                    reference: "東晉太元年間，武陵有一個以捕魚為業的人。有一天他沿著溪水划船而行，忘記了自己走了多遠。忽然遇到一片桃花林，生長在溪的兩岸，長達數百步，中間沒有別的樹，花草鮮嫩美麗，落花紛紛揚揚。",
                    explanation: "這是《桃花源記》的開頭，描述漁人無意中發現桃花林的經過。"
                }
            ],
            medium: [
                {
                    classical: "先帝創業未半而中道崩殂，今天下三分，益州疲弊，此誠危急存亡之秋也。然侍衛之臣不懈於內，忠志之士忘身於外者，蓋追先帝之殊遇，欲報之於陛下也。",
                    reference: "先帝創立帝業還沒有完成一半，就中途去世了。現在天下分裂成三個國家，我們蜀漢國力困乏，這實在是危急存亡的時刻啊。然而侍衛大臣在朝廷內毫不懈怠，忠誠有志的將士在外面捨生忘死，大概是追念先帝對他們的特別待遇，想要報答給陛下啊。",
                    explanation: "這是諸葛亮《出師表》的開頭，分析了當時的國家形勢和臣子的忠誠。"
                }
            ],
            hard: [
                {
                    classical: "嗟乎！師道之不傳也久矣！欲人之無惑也難矣！古之聖人，其出人也遠矣，猶且從師而問焉；今之眾人，其下聖人也亦遠矣，而恥學於師。是故聖益聖，愚益愚。聖人之所以為聖，愚人之所以為愚，其皆出於此乎？",
                    reference: "唉！從師學習的風尚已經失傳很久了！想要人們沒有疑惑很難啊！古代的聖人，他們超出一般人很遠，尚且跟從老師而請教；現在的一般人，他們才智低於聖人也很遠，卻以向老師學習為恥。因此聖人更加聖明，愚人更加愚昧。聖人之所以能成為聖人，愚人之所以成為愚人，大概都出於這個原因吧？",
                    explanation: "這是韓愈《師說》中的一段，批判當時社會不重視從師學習的風氣。"
                }
            ]
        };

        let currentDifficulty = 'easy';
        let currentQuestionIndex = 0;
        let score = 0;
        
        function setDifficulty(difficulty) {
            currentDifficulty = difficulty;
            document.querySelectorAll('.difficulty-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            currentQuestionIndex = 0;
            loadQuestion();
        }
        
        function loadQuestion() {
            const question = questions[currentDifficulty][currentQuestionIndex];
            document.getElementById('classical-text').textContent = question.classical;
            document.getElementById('translation-input').value = '';
            document.getElementById('reference-answer').innerHTML = '<p>提交您的翻譯後，這裡會顯示參考答案和解析。</p>';
            document.getElementById('user-translation').innerHTML = '';
            document.getElementById('reference-translation').innerHTML = '';
        }
        
        function checkAnswer() {
            const userTranslation = document.getElementById('translation-input').value.trim();
            if (!userTranslation) {
                alert('請先輸入您的翻譯');
                return;
            }
            
            const question = questions[currentDifficulty][currentQuestionIndex];
            document.getElementById('reference-answer').innerHTML = `<p><strong>解析：</strong>${question.explanation}</p>`;
            document.getElementById('user-translation').textContent = userTranslation;
            document.getElementById('reference-translation').textContent = question.reference;
            
            // 簡單評分
            score += 20;
            document.getElementById('score').textContent = `得分: ${score}`;
        }
        
        function nextQuestion() {
            currentQuestionIndex = (currentQuestionIndex + 1) % questions[currentDifficulty].length;
            loadQuestion();
        }
        
        // 初始化
        loadQuestion();
    </script>
</body>
</html>