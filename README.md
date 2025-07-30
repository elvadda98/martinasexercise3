<!DOCTYPE html>
<html>
<head>
    <title>Vocabulary Typing Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 700px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f9fc;
            line-height: 1.6;
        }
        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 10px;
        }
        .word-bank {
            background-color: #e8f4f8;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            text-align: center;
        }
        .word-bank span {
            display: inline-block;
            background-color: #3498db;
            color: white;
            padding: 5px 10px;
            margin: 5px;
            border-radius: 4px;
            font-weight: bold;
        }
        .sentence {
            background-color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 15px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            position: relative;
        }
        input {
            padding: 8px;
            border: 2px solid #3498db;
            border-radius: 4px;
            width: 120px;
            font-size: 16px;
        }
        .check-btn {
            background-color: #2ecc71;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            margin-left: 10px;
        }
        .check-btn:hover {
            background-color: #27ae60;
        }
        .hint-btn {
            background-color: #f39c12;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            margin-left: 10px;
        }
        .hint-btn:hover {
            background-color: #e67e22;
        }
        .feedback {
            margin-top: 8px;
            font-style: italic;
            display: none;
        }
        .correct {
            color: #27ae60;
        }
        .incorrect {
            color: #e74c3c;
        }
        #score {
            text-align: center;
            font-size: 18px;
            margin: 20px 0;
            font-weight: bold;
        }
        #restart {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        #restart:hover {
            background-color: #2980b9;
        }
        .hint-text {
            color: #7f8c8d;
            font-size: 14px;
            margin-top: 5px;
            display: none;
        }
        .correct-answer {
            font-weight: bold;
            color: #27ae60;
        }
    </style>
</head>
<body>
    <h1>✍️ Vocabulary Typing Challenge</h1>
    
    <div class="word-bank">
        <h3>Word Bank:</h3>
        <span>depending on</span>
        <span>headquarters</span>
        <span>overcome</span>
        <span>main</span>
        <span>convenient</span>
        <span>write down</span>
        <span>meet up</span>
        <span>topic</span>
        <span>come up with</span>
        <span>kick off</span>
    </div>
    
    <div id="score">Score: 0/20</div>
    
    <div id="sentences-container">
        <!-- Sentences will be added by JavaScript -->
    </div>
    
    <button id="restart">Restart Game</button>

    <script>
        // Vocabulary words and sentences
        const wordBank = ["depending on", "headquarters", "overcome", "main", "convenient", "write down", "meet up", "topic", "come up with", "kick off"];
        
        const sentences = [
            { text: "We'll ______ the meeting with introductions at 9 AM.", answer: "kick off", hint: "Means to start something" },
            { text: "The company's ______ is in downtown Chicago.", answer: "headquarters", hint: "Main office of a company" },
            { text: "Please ______ your ideas during the brainstorming session.", answer: "write down", hint: "Put thoughts on paper" },
            { text: "Let's ______ for coffee to discuss the project.", answer: "meet up", hint: "Get together with someone" },
            { text: "The ______ reason for our success is customer satisfaction.", answer: "main", hint: "Most important" },
            { text: "We must ______ these technical challenges quickly.", answer: "overcome", hint: "Successfully deal with problems" },
            { text: "Is this location ______ for all team members?", answer: "convenient", hint: "Suitable or easy to reach" },
            { text: "The main ______ of today's meeting is budget planning.", answer: "topic", hint: "Subject being discussed" },
            { text: "Can you ______ a better solution to this problem?", answer: "come up with", hint: "Think of and suggest" },
            { text: "Our schedule will change ______ the weather conditions.", answer: "depending on", hint: "Determined by something else" },
            { text: "The team worked together to ______ all obstacles.", answer: "overcome", hint: "Deal with successfully" },
            { text: "The ______ entrance is on the west side of the building.", answer: "main", hint: "Primary or most important" },
            { text: "We should ______ before the conference to prepare.", answer: "meet up", hint: "Gather together" },
            { text: "The workshop will ______ with an icebreaker activity.", answer: "kick off", hint: "Begin officially" },
            { text: "This meeting time isn't ______ for our overseas colleagues.", answer: "convenient", hint: "Suitable or appropriate" },
            { text: "The discussion moved to a new ______ after lunch.", answer: "topic", hint: "Subject of conversation" },
            { text: "We need to ______ a new marketing strategy.", answer: "come up with", hint: "Create or invent" },
            { text: "Our success will be ______ how well we collaborate.", answer: "depending on", hint: "Determined by" },
            { text: "Don't forget to ______ important action items.", answer: "write down", hint: "Make a note of" },
            { text: "The CEO visited from the ______ to meet the team.", answer: "headquarters", hint: "Company's main office" }
        ];

        let score = 0;
        
        // Initialize the game
        function initGame() {
            score = 0;
            document.getElementById("score").textContent = "Score: 0/20";
            const container = document.getElementById("sentences-container");
            container.innerHTML = "";
            
            // Create sentence elements
            sentences.forEach((sentence, index) => {
                const sentenceDiv = document.createElement("div");
                sentenceDiv.className = "sentence";
                
                const parts = sentence.text.split("______");
                
                sentenceDiv.innerHTML = `
                    <p>${index + 1}. ${parts[0]}<input type="text" id="answer-${index}" data-answer="${sentence.answer.toLowerCase()}">${parts[1]}</p>
                    <button class="check-btn" onclick="checkAnswer(${index})">Check</button>
                    <button class="hint-btn" onclick="showHint(${index})">Hint</button>
                    <div class="hint-text" id="hint-${index}">${sentence.hint}</div>
                    <div class="feedback" id="feedback-${index}"></div>
                `;
                
                container.appendChild(sentenceDiv);
            });
        }
        
        // Show hint
        function showHint(index) {
            const hint = document.getElementById(`hint-${index}`);
            hint.style.display = "block";
        }
        
        // Check answer
        function checkAnswer(index) {
            const input = document.getElementById(`answer-${index}`);
            const feedback = document.getElementById(`feedback-${index}`);
            const userAnswer = input.value.trim().toLowerCase();
            const correctAnswer = input.dataset.answer;
            
            if (userAnswer === correctAnswer) {
                feedback.innerHTML = "✓ <span class='correct-answer'>Correct!</span>";
                feedback.className = "feedback correct";
                feedback.style.display = "block";
                input.style.borderColor = "#27ae60";
                score++;
                document.getElementById("score").textContent = `Score: ${score}/20`;
            } else {
                feedback.innerHTML = `✗ Incorrect. The correct answer is: <span class='correct-answer'>${correctAnswer}</span>`;
                feedback.className = "feedback incorrect";
                feedback.style.display = "block";
                input.style.borderColor = "#e74c3c";
            }
        }
        
        // Restart game
        document.getElementById("restart").onclick = initGame;
        
        // Start the game
        initGame();
    </script>
</body>
</html>
