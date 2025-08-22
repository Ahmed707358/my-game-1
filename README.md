<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة العائلة - Family Feud</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            color: white;
        }

        .game-container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .game-title {
            font-size: 3rem;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 10px;
        }

        .teams-container {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
            gap: 20px;
        }

        .team {
            background: rgba(255,255,255,0.1);
            padding: 20px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
            flex: 1;
            text-align: center;
        }

        .team h3 {
            font-size: 1.5rem;
            margin-bottom: 10px;
        }

        .score {
            font-size: 2rem;
            font-weight: bold;
            color: #ffd700;
        }

        .question-board {
            background: rgba(255,255,255,0.95);
            color: #333;
            padding: 30px;
            border-radius: 20px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }

        .question {
            font-size: 1.8rem;
            text-align: center;
            margin-bottom: 30px;
            font-weight: bold;
            color: #2c3e50;
        }

        .answers-grid {
            display: grid;
            gap: 15px;
        }

        .answer-row {
            display: flex;
            align-items: center;
            background: #34495e;
            border-radius: 10px;
            padding: 15px 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .answer-row:hover {
            background: #2c3e50;
            transform: translateY(-2px);
        }

        .answer-number {
            background: #e74c3c;
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-left: 15px;
        }

        .answer-text {
            flex: 1;
            font-size: 1.2rem;
            color: white;
            text-align: right;
        }

        .answer-points {
            background: #f39c12;
            color: white;
            padding: 5px 15px;
            border-radius: 20px;
            font-weight: bold;
        }

        .answer-row.revealed {
            background: #27ae60;
        }

        .answer-row.revealed .answer-number {
            background: #2ecc71;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 20px;
        }

        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 25px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: #3498db;
            color: white;
        }

        .btn-primary:hover {
            background: #2980b9;
            transform: translateY(-2px);
        }

        .btn-danger {
            background: #e74c3c;
            color: white;
        }

        .btn-danger:hover {
            background: #c0392b;
            transform: translateY(-2px);
        }

        .input-section {
            background: rgba(255,255,255,0.1);
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 20px;
            backdrop-filter: blur(10px);
        }

        .input-group {
            display: flex;
            gap: 10px;
            align-items: center;
            justify-content: center;
        }

        .answer-input {
            padding: 12px 20px;
            border: none;
            border-radius: 25px;
            font-size: 1rem;
            width: 300px;
            text-align: right;
        }

        .strikes {
            text-align: center;
            margin: 20px 0;
        }

        .strike {
            display: inline-block;
            font-size: 3rem;
            margin: 0 10px;
            color: #e74c3c;
        }

        .feedback {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 4rem;
            font-weight: bold;
            z-index: 1000;
            opacity: 0;
            transition: all 0.5s ease;
        }

        .feedback.show {
            opacity: 1;
            animation: bounce 0.6s ease;
        }

        .feedback.correct {
            color: #27ae60;
        }

        .feedback.wrong {
            color: #e74c3c;
        }

        @keyframes bounce {
            0%, 20%, 60%, 100% { transform: translate(-50%, -50%) scale(1); }
            40% { transform: translate(-50%, -50%) scale(1.2); }
            80% { transform: translate(-50%, -50%) scale(1.1); }
        }

        .current-turn {
            text-align: center;
            font-size: 1.3rem;
            margin-bottom: 20px;
            padding: 10px;
            background: rgba(255,255,255,0.1);
            border-radius: 10px;
        }

        .points-control {
            background: rgba(255,255,255,0.1);
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 20px;
            backdrop-filter: blur(10px);
            border: 2px solid rgba(255,255,255,0.2);
        }

        @media (max-width: 768px) {
            .teams-container {
                flex-direction: column;
            }
            
            .game-title {
                font-size: 2rem;
            }
            
            .answer-input {
                width: 250px;
            }
            
            .input-group {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <div class="header">
            <h1 class="game-title">🎯 لعبة العائلة 🎯</h1>
        </div>

        <div class="teams-container">
            <div class="team">
                <div style="margin-bottom: 10px;">
                    <input type="text" id="team1Name" value="الفريق الأول" style="background: rgba(255,255,255,0.2); border: 2px solid rgba(255,255,255,0.3); color: white; padding: 8px 15px; border-radius: 10px; font-size: 1.2rem; font-weight: bold; text-align: center; width: 100%;" onchange="updateTeamName(1, this.value)">
                </div>
                <div class="score" id="team1Score">0</div>
                <div style="margin-top: 15px; display: flex; justify-content: center; gap: 10px;">
                    <button class="btn btn-primary" style="padding: 8px 12px; font-size: 0.9rem;" onclick="adjustScore(1, -10)">-10</button>
                    <button class="btn btn-primary" style="padding: 8px 12px; font-size: 0.9rem;" onclick="adjustScore(1, -5)">-5</button>
                    <button class="btn btn-primary" style="padding: 8px 12px; font-size: 0.9rem;" onclick="adjustScore(1, 5)">+5</button>
                    <button class="btn btn-primary" style="padding: 8px 12px; font-size: 0.9rem;" onclick="adjustScore(1, 10)">+10</button>
                </div>
            </div>
            <div class="team">
                <div style="margin-bottom: 10px;">
                    <input type="text" id="team2Name" value="الفريق الثاني" style="background: rgba(255,255,255,0.2); border: 2px solid rgba(255,255,255,0.3); color: white; padding: 8px 15px; border-radius: 10px; font-size: 1.2rem; font-weight: bold; text-align: center; width: 100%;" onchange="updateTeamName(2, this.value)">
                </div>
                <div class="score" id="team2Score">0</div>
                <div style="margin-top: 15px; display: flex; justify-content: center; gap: 10px;">
                    <button class="btn btn-primary" style="padding: 8px 12px; font-size: 0.9rem;" onclick="adjustScore(2, -10)">-10</button>
                    <button class="btn btn-primary" style="padding: 8px 12px; font-size: 0.9rem;" onclick="adjustScore(2, -5)">-5</button>
                    <button class="btn btn-primary" style="padding: 8px 12px; font-size: 0.9rem;" onclick="adjustScore(2, 5)">+5</button>
                    <button class="btn btn-primary" style="padding: 8px 12px; font-size: 0.9rem;" onclick="adjustScore(2, 10)">+10</button>
                </div>
            </div>
        </div>

        <div class="current-turn" id="currentTurn">
            دور الفريق الأول
        </div>

        <div class="question-board">
            <div class="question" id="questionText">
                ما هي الأشياء التي نجدها في المطبخ؟
            </div>
            
            <div class="answers-grid" id="answersGrid">
                <!-- الإجابات ستظهر هنا -->
            </div>
        </div>

        <div class="strikes" id="strikes">
            <span class="strike">❌</span>
            <span class="strike">❌</span>
        </div>

        <div class="input-section">
            <div class="input-group">
                <input type="text" class="answer-input" id="answerInput" placeholder="اكتب إجابتك هنا...">
                <button class="btn btn-primary" onclick="submitAnswer()">إرسال</button>
            </div>
        </div>

        <div class="points-control" id="pointsControl" style="display: none;">
            <div style="text-align: center; margin-bottom: 15px;">
                <h3 style="color: white; margin-bottom: 10px;">من يأخذ النقاط؟</h3>
                <div id="foundAnswerText" style="background: rgba(255,255,255,0.2); padding: 10px; border-radius: 10px; margin-bottom: 15px;"></div>
            </div>
            <div style="display: flex; justify-content: center; gap: 15px;">
                <button class="btn btn-primary" onclick="awardPoints(1)" id="awardTeam1Btn">الفريق الأول</button>
                <button class="btn btn-primary" onclick="awardPoints(2)" id="awardTeam2Btn">الفريق الثاني</button>
                <button class="btn btn-danger" onclick="markWrong()">إجابة خاطئة</button>
            </div>
        </div>

        <div class="controls">
            <button class="btn btn-primary" onclick="nextQuestion()">السؤال التالي</button>
            <button class="btn btn-danger" onclick="resetGame()">بداية جديدة</button>
        </div>
    </div>

    <div class="feedback" id="feedback"></div>

    <script>
        let currentQuestionIndex = 0;
        let currentTeam = 1;
        let team1Score = 0;
        let team2Score = 0;
        let strikes = 0;
        let revealedAnswers = [];
        let team1Name = "الفريق الأول";
        let team2Name = "الفريق الثاني";

        const questions = [
            {
                question: "ما هي الأشياء التي نجدها في المطبخ؟",
                answers: [
                    { text: "الثلاجة", points: 40 },
                    { text: "الفرن", points: 30 },
                    { text: "الموقد", points: 20 },
                    { text: "الحوض", points: 15 },
                    { text: "الخلاط", points: 10 }
                ]
            },
            {
                question: "أشياء نحتاجها عند السفر؟",
                answers: [
                    { text: "جواز السفر", points: 35 },
                    { text: "الحقيبة", points: 30 },
                    { text: "التذكرة", points: 25 },
                    { text: "النقود", points: 20 },
                    { text: "الهاتف", points: 15 }
                ]
            },
            {
                question: "ما هي وسائل المواصلات؟",
                answers: [
                    { text: "السيارة", points: 40 },
                    { text: "الحافلة", points: 25 },
                    { text: "الطائرة", points: 20 },
                    { text: "القطار", points: 15 },
                    { text: "الدراجة", points: 10 }
                ]
            },
            {
                question: "أشياء نراها في السماء؟",
                answers: [
                    { text: "الشمس", points: 35 },
                    { text: "القمر", points: 30 },
                    { text: "النجوم", points: 25 },
                    { text: "الغيوم", points: 20 },
                    { text: "الطائرات", points: 15 }
                ]
            },
            {
                question: "ما هي الألوان الأساسية؟",
                answers: [
                    { text: "الأحمر", points: 35 },
                    { text: "الأزرق", points: 30 },
                    { text: "الأصفر", points: 25 },
                    { text: "الأخضر", points: 20 },
                    { text: "الأبيض", points: 15 }
                ]
            },
            {
                question: "أشياء نأكلها في الإفطار؟",
                answers: [
                    { text: "البيض", points: 40 },
                    { text: "الخبز", points: 30 },
                    { text: "الجبن", points: 25 },
                    { text: "الحليب", points: 20 },
                    { text: "العسل", points: 15 }
                ]
            },
            {
                question: "ما هي الرياضات الشعبية؟",
                answers: [
                    { text: "كرة القدم", points: 45 },
                    { text: "كرة السلة", points: 25 },
                    { text: "التنس", points: 20 },
                    { text: "السباحة", points: 15 },
                    { text: "الجري", points: 10 }
                ]
            },
            {
                question: "أشياء نجدها في الحديقة؟",
                answers: [
                    { text: "الأشجار", points: 35 },
                    { text: "الورود", points: 30 },
                    { text: "العشب", points: 25 },
                    { text: "المقاعد", points: 20 },
                    { text: "النافورة", points: 15 }
                ]
            },
            {
                question: "ما هي المهن الشائعة؟",
                answers: [
                    { text: "الطبيب", points: 40 },
                    { text: "المعلم", points: 30 },
                    { text: "المهندس", points: 25 },
                    { text: "الممرض", points: 20 },
                    { text: "الطباخ", points: 15 }
                ]
            },
            {
                question: "أشياء نستخدمها للنظافة؟",
                answers: [
                    { text: "الصابون", points: 35 },
                    { text: "الشامبو", points: 30 },
                    { text: "فرشاة الأسنان", points: 25 },
                    { text: "المنشفة", points: 20 },
                    { text: "معجون الأسنان", points: 15 }
                ]
            },
            {
                question: "ما هي الحيوانات الأليفة؟",
                answers: [
                    { text: "القطة", points: 40 },
                    { text: "الكلب", points: 35 },
                    { text: "العصفور", points: 20 },
                    { text: "السمك", points: 15 },
                    { text: "الأرنب", points: 10 }
                ]
            },
            {
                question: "أشياء نجدها في المدرسة؟",
                answers: [
                    { text: "الكتب", points: 35 },
                    { text: "السبورة", points: 30 },
                    { text: "المكاتب", points: 25 },
                    { text: "القلم", points: 20 },
                    { text: "الحقيبة", points: 15 }
                ]
            },
            {
                question: "ما هي الفواكه المشهورة؟",
                answers: [
                    { text: "التفاح", points: 40 },
                    { text: "الموز", points: 35 },
                    { text: "البرتقال", points: 25 },
                    { text: "العنب", points: 20 },
                    { text: "الفراولة", points: 15 }
                ]
            }
        ];

        function initializeGame() {
            loadQuestion();
            updateScores();
            updateTurn();
            updateStrikes();
            updateAwardButtons();
        }

        function loadQuestion() {
            const question = questions[currentQuestionIndex];
            document.getElementById('questionText').textContent = question.question;
            
            const answersGrid = document.getElementById('answersGrid');
            answersGrid.innerHTML = '';
            revealedAnswers = [];

            question.answers.forEach((answer, index) => {
                const answerRow = document.createElement('div');
                answerRow.className = 'answer-row';
                answerRow.innerHTML = `
                    <div class="answer-number">${index + 1}</div>
                    <div class="answer-text">???</div>
                    <div class="answer-points">${answer.points}</div>
                `;
                answersGrid.appendChild(answerRow);
            });
        }

        let currentFoundAnswer = null;

        function submitAnswer() {
            const input = document.getElementById('answerInput');
            const userAnswer = input.value.trim();
            
            if (!userAnswer) return;

            const question = questions[currentQuestionIndex];
            let found = false;
            let foundIndex = -1;

            question.answers.forEach((answer, index) => {
                if (answer.text.includes(userAnswer) || userAnswer.includes(answer.text)) {
                    if (!revealedAnswers.includes(index)) {
                        found = true;
                        foundIndex = index;
                        currentFoundAnswer = { index: index, answer: answer };
                    }
                }
            });

            if (found) {
                // إظهار أزرار التحكم بالنقاط
                document.getElementById('foundAnswerText').textContent = `الإجابة: ${question.answers[foundIndex].text} (${question.answers[foundIndex].points} نقطة)`;
                document.getElementById('pointsControl').style.display = 'block';
            } else {
                // إجابة خاطئة مباشرة
                strikes++;
                showFeedback('✗', 'wrong');
                updateStrikes();
                
                if (strikes >= 2) {
                    switchTeam();
                    strikes = 0;
                    updateStrikes();
                }
            }

            input.value = '';
        }

        function awardPoints(teamNumber) {
            if (currentFoundAnswer) {
                // كشف الإجابة
                revealAnswer(currentFoundAnswer.index);
                
                // إضافة النقاط للفريق المحدد
                if (teamNumber === 1) {
                    team1Score += currentFoundAnswer.answer.points;
                } else {
                    team2Score += currentFoundAnswer.answer.points;
                }
                
                updateScores();
                showFeedback('✓', 'correct');
                
                // إخفاء أزرار التحكم
                document.getElementById('pointsControl').style.display = 'none';
                currentFoundAnswer = null;
            }
        }

        function markWrong() {
            // اعتبار الإجابة خاطئة
            strikes++;
            showFeedback('✗', 'wrong');
            updateStrikes();
            
            if (strikes >= 2) {
                switchTeam();
                strikes = 0;
                updateStrikes();
            }
            
            // إخفاء أزرار التحكم
            document.getElementById('pointsControl').style.display = 'none';
            currentFoundAnswer = null;
        }

        function revealAnswer(index) {
            const question = questions[currentQuestionIndex];
            const answerRows = document.querySelectorAll('.answer-row');
            const answerRow = answerRows[index];
            
            answerRow.classList.add('revealed');
            answerRow.querySelector('.answer-text').textContent = question.answers[index].text;
            revealedAnswers.push(index);
        }

        function showFeedback(symbol, type) {
            const feedback = document.getElementById('feedback');
            feedback.textContent = symbol;
            feedback.className = `feedback ${type} show`;
            
            setTimeout(() => {
                feedback.classList.remove('show');
            }, 1000);
        }

        function switchTeam() {
            currentTeam = currentTeam === 1 ? 2 : 1;
            updateTurn();
        }

        function updateTurn() {
            const turnElement = document.getElementById('currentTurn');
            turnElement.textContent = `دور ${currentTeam === 1 ? team1Name : team2Name}`;
        }

        function updateTeamName(teamNumber, newName) {
            if (newName && newName.trim() !== '') {
                if (teamNumber === 1) {
                    team1Name = newName.trim();
                } else {
                    team2Name = newName.trim();
                }
                // تحديث أزرار إعطاء النقاط
                updateAwardButtons();
                // تحديث مؤشر الدور
                updateTurn();
            }
        }

        function updateAwardButtons() {
            document.getElementById('awardTeam1Btn').textContent = team1Name;
            document.getElementById('awardTeam2Btn').textContent = team2Name;
        }

        function updateScores() {
            document.getElementById('team1Score').textContent = team1Score;
            document.getElementById('team2Score').textContent = team2Score;
        }

        function adjustScore(teamNumber, points) {
            if (teamNumber === 1) {
                team1Score = Math.max(0, team1Score + points);
            } else {
                team2Score = Math.max(0, team2Score + points);
            }
            updateScores();
        }

        function updateStrikes() {
            const strikesElement = document.getElementById('strikes');
            const strikeElements = strikesElement.querySelectorAll('.strike');
            
            strikeElements.forEach((strike, index) => {
                if (index < strikes) {
                    strike.style.opacity = '1';
                } else {
                    strike.style.opacity = '0.3';
                }
            });
        }

        function nextQuestion() {
            currentQuestionIndex = (currentQuestionIndex + 1) % questions.length;
            strikes = 0;
            loadQuestion();
            updateStrikes();
        }

        function resetGame() {
            currentQuestionIndex = 0;
            currentTeam = 1;
            team1Score = 0;
            team2Score = 0;
            strikes = 0;
            team1Name = "الفريق الأول";
            team2Name = "الفريق الثاني";
            document.getElementById('team1Name').value = team1Name;
            document.getElementById('team2Name').value = team2Name;
            document.getElementById('awardTeam1Btn').textContent = team1Name;
            document.getElementById('awardTeam2Btn').textContent = team2Name;
            initializeGame();
        }

        // إضافة مستمع للضغط على Enter
        document.getElementById('answerInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                submitAnswer();
            }
        });

        // تشغيل اللعبة عند تحميل الصفحة
        initializeGame();
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'972dfd7515577917',t:'MTc1NTgxOTc3OS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
