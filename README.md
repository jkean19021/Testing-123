# Testing-123
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }

        .calculator {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            width: 320px;
        }

        .display {
            width: 100%;
            padding: 20px;
            margin-bottom: 20px;
            font-size: 28px;
            text-align: right;
            background: #f0f0f0;
            border: 2px solid #ddd;
            border-radius: 5px;
            color: #333;
            word-wrap: break-word;
            word-break: break-all;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            padding: 20px;
            font-size: 18px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.2s;
        }

        button:hover {
            opacity: 0.8;
        }

        button:active {
            transform: scale(0.95);
        }

        .number {
            background: #e0e0e0;
            color: #333;
        }

        .operator {
            background: #667eea;
            color: white;
        }

        .equals {
            background: #4caf50;
            color: white;
            grid-column: span 2;
        }

        .clear {
            background: #f44336;
            color: white;
            grid-column: span 2;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="operator" onclick="setOperation('/')">/</button>
            <button class="operator" onclick="setOperation('*')">*</button>

            <button class="number" onclick="appendToDisplay('7')">7</button>
            <button class="number" onclick="appendToDisplay('8')">8</button>
            <button class="number" onclick="appendToDisplay('9')">9</button>
            <button class="operator" onclick="setOperation('-')">-</button>

            <button class="number" onclick="appendToDisplay('4')">4</button>
            <button class="number" onclick="appendToDisplay('5')">5</button>
            <button class="number" onclick="appendToDisplay('6')">6</button>
            <button class="operator" onclick="setOperation('+')">+</button>

            <button class="number" onclick="appendToDisplay('1')">1</button>
            <button class="number" onclick="appendToDisplay('2')">2</button>
            <button class="number" onclick="appendToDisplay('3')">3</button>
            <button class="number" onclick="appendToDisplay('.')">.</button>

            <button class="number" onclick="appendToDisplay('0')">0</button>
            <button class="equals" onclick="calculate()">=</button>
        </div>
    </div>

    <script>
        let currentInput = '0';
        let previousInput = '';
        let operator = '';
        let shouldResetDisplay = false;

        function updateDisplay() {
            document.getElementById('display').textContent = currentInput;
        }

        function appendToDisplay(value) {
            // Reset display after calculation
            if (shouldResetDisplay) {
                currentInput = '0';
                shouldResetDisplay = false;
            }

            // Prevent multiple decimal points
            if (value === '.' && currentInput.includes('.')) {
                return;
            }

            // Replace leading 0 with new number (unless adding decimal)
            if (currentInput === '0' && value !== '.') {
                currentInput = value;
            } else {
                currentInput += value;
            }

            updateDisplay();
        }

        function setOperation(op) {
            if (currentInput === '' || currentInput === '-') return;

            // If there's already an operation pending, calculate it first
            if (previousInput !== '' && !shouldResetDisplay) {
                calculate();
                return;
            }

            previousInput = currentInput;
            operator = op;
            shouldResetDisplay = true;
        }

        function calculate() {
            if (operator === '' || previousInput === '' || currentInput === '') {
                return;
            }

            let result;
            const prev = parseFloat(previousInput);
            const curr = parseFloat(currentInput);

            switch (operator) {
                case '+':
                    result = prev + curr;
                    break;
                case '-':
                    result = prev - curr;
                    break;
                case '*':
                    result = prev * curr;
                    break;
                case '/':
                    result = curr === 0 ? 'Error' : prev / curr;
                    break;
                default:
                    return;
            }

            currentInput = result.toString();
            operator = '';
            previousInput = '';
            shouldResetDisplay = true;
            updateDisplay();
        }

        function clearDisplay() {
            currentInput = '0';
            previousInput = '';
            operator = '';
            shouldResetDisplay = false;
            updateDisplay();
        }

        // Initialize display
        updateDisplay();
    </script>
</body>
</html>
