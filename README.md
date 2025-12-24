#CALCULATOR





<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neon Glow Calculator</title>
    <style>
        /* --- CSS STYLES --- */
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Poppins:wght@300;500&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #121212; /* Dark background to make glow pop */
        }

        /* Calculator Container */
        .calculator {
            position: relative;
            background: #1e1e1e;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            width: 360px;
            border: 1px solid #333;
        }

        /* The Display Screen */
        .display-screen {
            width: 100%;
            height: 80px;
            background: #000;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: right;
            padding: 10px 20px;
            font-family: 'Orbitron', sans-serif; /* Digital clock font */
            font-size: 2.5rem;
            color: #fff;
            border: none;
            outline: none;
            box-shadow: inset 0 0 10px rgba(255, 255, 255, 0.1);
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }

        /* Button Grid Layout */
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
        }

        /* General Button Styling */
        .btn {
            padding: 20px;
            border-radius: 50%; /* Circular buttons */
            border: none;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            background: #2d2d2d;
            color: #fff;
            box-shadow: 5px 5px 10px rgba(0,0,0,0.5), 
                        -2px -2px 10px rgba(255,255,255,0.05);
            transition: all 0.2s ease-in-out;
            user-select: none;
            width: 65px;
            height: 65px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* --- GLOWING EFFECTS & COLORS --- */

        /* Normal Number Buttons */
        .btn:hover {
            transform: scale(1.1);
            background: #3a3a3a;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.2);
        }

        /* Operator Buttons (Cyan Glow) */
        .operator {
            color: #00ffff;
        }
        .operator:hover {
            background: #00ffff;
            color: #000;
            box-shadow: 0 0 20px #00ffff, 0 0 40px #00ffff;
        }

        /* Clear & Delete (Red/Pink Glow) */
        .action-btn {
            color: #ff0055;
        }
        .action-btn:hover {
            background: #ff0055;
            color: #fff;
            box-shadow: 0 0 20px #ff0055, 0 0 40px #ff0055;
        }

        /* Equals Button (Green Glow) - Spans 2 columns optionally, or stays distinct */
        .equal {
            background: #00ff88;
            color: #000;
            box-shadow: 0 0 10px #00ff88;
        }
        .equal:hover {
            background: #00ff88;
            box-shadow: 0 0 20px #00ff88, 0 0 40px #00ff88;
            transform: scale(1.1);
        }

        /* Active Click Effect (Pressing down) */
        .btn:active {
            transform: scale(0.9);
            box-shadow: inset 5px 5px 10px rgba(0,0,0,0.5);
        }

    </style>
</head>
<body>

    <div class="calculator">
        <input type="text" class="display-screen" id="display" readonly placeholder="0">
        
        <div class="buttons">
            <div class="btn action-btn" onclick="clearDisplay()">AC</div>
            <div class="btn action-btn" onclick="deleteLast()">DEL</div>
            <div class="btn operator" onclick="appendOperator('%')">%</div>
            <div class="btn operator" onclick="appendOperator('/')">/</div>

            <div class="btn" onclick="appendNumber('7')">7</div>
            <div class="btn" onclick="appendNumber('8')">8</div>
            <div class="btn" onclick="appendNumber('9')">9</div>
            <div class="btn operator" onclick="appendOperator('*')">×</div>

            <div class="btn" onclick="appendNumber('4')">4</div>
            <div class="btn" onclick="appendNumber('5')">5</div>
            <div class="btn" onclick="appendNumber('6')">6</div>
            <div class="btn operator" onclick="appendOperator('-')">-</div>

            <div class="btn" onclick="appendNumber('1')">1</div>
            <div class="btn" onclick="appendNumber('2')">2</div>
            <div class="btn" onclick="appendNumber('3')">3</div>
            <div class="btn operator" onclick="appendOperator('+')">+</div>

            <div class="btn" onclick="appendNumber('0')">0</div>
            <div class="btn" onclick="appendNumber('00')">00</div>
            <div class="btn" onclick="appendNumber('.')">.</div>
            <div class="btn equal" onclick="calculate()">=</div>
        </div>
    </div>

    <script>
        /* --- JAVASCRIPT LOGIC --- */
        const display = document.getElementById('display');

        // Add number to display
        function appendNumber(num) {
            if(display.value === 'Error') display.value = '';
            display.value += num;
        }

        // Add operator to display
        function appendOperator(operator) {
            const lastChar = display.value.slice(-1);
            // Prevent empty input operators (except minus)
            if (display.value === '' && operator !== '-') return;
            
            // Prevent consecutive operators
            if (['+', '-', '*', '/', '%'].includes(lastChar)) {
                // Replace the last operator if user changes their mind
                display.value = display.value.slice(0, -1) + operator;
            } else {
                display.value += operator;
            }
        }

        // Clear All
        function clearDisplay() {
            display.value = '';
        }

        // Delete last character
        function deleteLast() {
            display.value = display.value.toString().slice(0, -1);
        }

        // Calculate Result
        function calculate() {
            try {
                // Prevent empty calculation
                if(display.value === '') return;
                
                // Eval is simple for this, but 'new Function' is slightly safer practice
                // We perform the math
                let result = eval(display.value); 
                
                // Handle dividing by zero or other weirdness
                if (!isFinite(result) || isNaN(result)) {
                    display.value = "Error";
                } else {
                    display.value = result;
                }
            } catch (error) {
                display.value = "Error";
            }
        }
    </script>
</body>
</html>
