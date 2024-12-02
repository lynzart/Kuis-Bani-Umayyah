<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Bani Umayyah Interaktif</title>
    <style>
        body { 
            font-family: 'Arial', sans-serif; 
            background-image: url('https://upload.wikimedia.org/wikipedia/commons/e/e7/Great_Mosque_of_Damascus.jpg'); /* Ganti URL dengan gambar bertema Bani Umayyah */
            background-size: cover; 
            background-attachment: fixed; 
            color: #fff; 
            text-align: center; 
            padding: 20px; 
            overflow-x: hidden; 
        }

        .container { 
            background-color: rgba(0, 0, 0, 0.8); 
            padding: 20px; 
            border-radius: 10px; 
            display: inline-block; 
            width: 80%; 
            max-width: 600px; 
            animation: fadeIn 1.5s ease-in-out; 
        }

        h1 { 
            font-size: 2em; 
            margin-bottom: 20px; 
            color: #FFD700; 
            text-shadow: 2px 2px 5px #000; 
        }

        .question { 
            margin-bottom: 15px; 
            font-weight: bold; 
            font-size: 1.5em; 
        }

        .answer { 
            margin: 10px 0; 
            padding: 10px 20px; 
            background-color: #007BFF; 
            color: white; 
            border: none; 
            border-radius: 5px; 
            cursor: pointer; 
            font-size: 1em; 
            transition: all 0.3s; 
        }

        .answer:hover { 
            background-color: #0056b3; 
            transform: scale(1.05); 
        }

        #result, #feedback { 
            margin-top: 20px; 
            font-size: 1.2em; 
            background-color: #222; 
            padding: 10px; 
            border-radius: 5px; 
        }

        #timer { 
            font-size: 1.2em; 
            color: #FF4500; 
            margin-top: 10px; 
        }

        .icon { 
            width: 50px; 
            height: 50px; 
            display: inline-block; 
            margin-top: 10px; 
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .hidden { 
            display: none; 
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Kuis Sejarah Bani Umayyah</h1>
        <div id="quiz-container"></div>
        <div id="feedback"></div>
        <div id="timer" class="hidden"></div>
        <div id="result"></div>
    </div>

    <audio id="correctSound" src="https://www.soundjay.com/button/beep-07.wav"></audio>
    <audio id="wrongSound" src="https://www.soundjay.com/button/beep-10.wav"></audio>

    <script>
        const questions = [
            {
                question: "Siapakah pendiri Dinasti Bani Umayyah?",
                answers: { a: "Muawiyah bin Abu Sufyan", b: "Umar bin Abdul Aziz", c: "Abdul Malik bin Marwan", d: "Yazid bin Muawiyah" },
                correct: "a",
                explanation: "Muawiyah bin Abu Sufyan adalah pendiri Dinasti Bani Umayyah setelah Perang Saudara Islam pertama."
            },
            {
                question: "Di mana ibu kota Bani Umayyah pertama kali didirikan?",
                answers: { a: "Madinah", b: "Damaskus", c: "Kufa", d: "Baghdad" },
                correct: "b",
                explanation: "Damaskus menjadi ibu kota Bani Umayyah di bawah pemerintahan Muawiyah bin Abu Sufyan."
            },
            {
                question: "Siapakah khalifah yang dikenal sebagai 'khalifah yang adil'?",
                answers: { a: "Al-Walid bin Abdul Malik", b: "Umar bin Abdul Aziz", c: "Marwan bin Hakam", d: "Yazid bin Abdul Malik" },
                correct: "b",
                explanation: "Umar bin Abdul Aziz terkenal karena keadilannya dan berbagai reformasi sosial selama masa pemerintahannya."
            },
            {
                question: "Siapakah khalifah Bani Umayyah yang memulai pembangunan Kubah Batu (Dome of the Rock)?",
                answers: { a: "Muawiyah bin Abu Sufyan", b: "Abdul Malik bin Marwan", c: "Al-Walid bin Abdul Malik", d: "Umar bin Abdul Aziz" },
                correct: "b",
                explanation: "Abdul Malik bin Marwan memulai pembangunan Dome of the Rock sebagai simbol kekuasaan Bani Umayyah."
            },
            {
                question: "Dinasti Bani Umayyah jatuh pada tahun berapa?",
                answers: { a: "750 M", b: "1258 M", c: "1492 M", d: "1517 M" },
                correct: "a",
                explanation: "Dinasti Bani Umayyah runtuh pada tahun 750 M setelah dikalahkan oleh Dinasti Abbasiyah."
            }
        ];

        let currentQuestion = 0;
        let score = 0;
        let timer;

        function startTimer() {
            let timeLeft = 15;
            const timerDisplay = document.getElementById('timer');
            timerDisplay.classList.remove('hidden');
            timerDisplay.innerText = `Waktu tersisa: ${timeLeft} detik`;

            timer = setInterval(() => {
                timeLeft--;
                timerDisplay.innerText = `Waktu tersisa: ${timeLeft} detik`;
                if (timeLeft <= 0) {
                    clearInterval(timer);
                    showFeedback(false);
                    loadNextQuestion();
                }
            }, 1000);
        }

        function loadQuestion() {
            const container = document.getElementById('quiz-container');
            container.innerHTML = '';
            document.getElementById('feedback').innerText = '';
            if (currentQuestion < questions.length) {
                const q = questions[currentQuestion];
                container.innerHTML = `<div class="question">${q.question}</div>`;
                for (let key in q.answers) {
                    container.innerHTML += `
                        <button class="answer" onclick="checkAnswer('${key}')">${q.answers[key]}</button>
                    `;
                }
                startTimer();
            } else {
                document.getElementById('result').innerHTML = `<h2>Kuis selesai! Skor kamu: ${score}/${questions.length}</h2>`;
            }
        }

        function checkAnswer(answer) {
            clearInterval(timer);
            const correctSound = document.getElementById('correctSound');
            const wrongSound = document.getElementById('wrongSound');
            const isCorrect = answer === questions[currentQuestion].correct;
            showFeedback(isCorrect);
            if (isCorrect) {
                correctSound.play();
                score++;
            } else {
                wrongSound.play();
            }
            loadNextQuestion();
        }

        function showFeedback(isCorrect) {
            const feedback = document.getElementById('feedback');
            const explanation = questions[currentQuestion].explanation;
            feedback.innerHTML = isCorrect ? 
                `<span style="color: green;">Benar!</span> ${explanation} <img src="https://img.icons8.com/emoji/48/000000/check-mark-emoji.png" class="icon">` : 
                `<span style="color: red;">Salah!</span> ${explanation} <img src="https://img.icons8.com/emoji/48/000000/cross-mark-emoji.png" class="icon">`;
        }

        function loadNextQuestion() {
            currentQuestion++;
            setTimeout(loadQuestion, 3000);
        }

        loadQuestion();
    </script>
</body>
</html>
