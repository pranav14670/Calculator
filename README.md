<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Calculator</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        * {
            font-family: 'Inter', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            min-height: 100vh;
        }
        
        .calculator {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 
                0 25px 50px -12px rgba(0, 0, 0, 0.5),
                0 0 100px rgba(99, 102, 241, 0.1);
        }
        
        .display-container {
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        
        .btn {
            transition: all 0.15s ease;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        
        .btn:hover {
            background: rgba(255, 255, 255, 0.15);
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }
        
        .btn:active {
            transform: translateY(0);
            background: rgba(255, 255, 255, 0.2);
        }
        
        .btn-number {
            color: #ffffff;
        }
        
        .btn-operator {
            background: rgba(99, 102, 241, 0.3);
            color: #a5b4fc;
        }
        
        .btn-operator:hover {
            background: rgba(99, 102, 241, 0.5);
        }
        
        .btn-equals {
            background: linear-gradient(135deg, #6366f1 0%, #8b5cf6 100%);
            color: white;
            box-shadow: 0 10px 30px rgba(99, 102, 241, 0.4);
        }
        
        .btn-equals:hover {
            background: linear-gradient(135deg, #818cf8 0%, #a78bfa 100%);
            box-shadow: 0 15px 40px rgba(99, 102, 241, 0.5);
        }
        
        .btn-clear {
            background: rgba(239, 68, 68, 0.2);
            color: #fca5a5;
        }
        
        .btn-clear:hover {
            background: rgba(239, 68, 68, 0.4);
        }
        
        .btn-function {
            background: rgba(16, 185, 129, 0.2);
            color: #6ee7b7;
        }
        
        .btn-function:hover {
            background: rgba(16, 185, 129, 0.4);
        }
        
        .history-panel {
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        
        .history-item {
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        }
        
        .history-item:hover {
            background: rgba(255, 255, 255, 0.05);
        }
        
        @keyframes pulse-glow {
            0%, 100% { box-shadow: 0 0 20px rgba(99, 102, 241, 0.3); }
            50% { box-shadow: 0 0 40px rgba(99, 102, 241, 0.5); }
        }
        
        .result-glow {
            animation: pulse-glow 2s infinite;
        }
        
        ::-webkit-scrollbar {
            width: 4px;
        }
        
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        
        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 2px;
        }
    </style>
</head>
<body class="flex items-center justify-center p-4">
    <div class="flex flex-col lg:flex-row gap-6 items-start">
        <!-- Calculator -->
        <div class="calculator rounded-3xl p-6 w-80">
            <!-- Display -->
            <div class="display-container rounded-2xl p-5 mb-6">
                <div id="expression" class="text-gray-400 text-right text-sm h-6 overflow-hidden font-light tracking-wide"></div>
                <div id="display" class="text-white text-right text-5xl font-light mt-2 overflow-hidden tracking-tight">0</div>
            </div>
            
            <!-- Buttons Grid -->
            <div class="grid grid-cols-4 gap-3">
                <!-- Row 1 -->
                <button onclick="clearAll()" class="btn btn-clear rounded-2xl h-16 text-lg font-medium">AC</button>
                <button onclick="backspace()" class="btn btn-function rounded-2xl h-16 text-lg font-medium">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2M3 12l6.414 6.414a2 2 0 001.414.586H19a2 2 0 002-2V7a2 2 0 00-2-2h-8.172a2 2 0 00-1.414.586L3 12z" />
                    </svg>
                </button>
                <button onclick="inputOperator('%')" class="btn btn-function rounded-2xl h-16 text-lg font-medium">%</button>
                <button onclick="inputOperator('/')" class="btn btn-operator rounded-2xl h-16 text-xl font-medium">÷</button>
                
                <!-- Row 2 -->
                <button onclick="inputNumber('7')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">7</button>
                <button onclick="inputNumber('8')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">8</button>
                <button onclick="inputNumber('9')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">9</button>
                <button onclick="inputOperator('*')" class="btn btn-operator rounded-2xl h-16 text-xl font-medium">×</button>
                
                <!-- Row 3 -->
                <button onclick="inputNumber('4')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">4</button>
                <button onclick="inputNumber('5')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">5</button>
                <button onclick="inputNumber('6')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">6</button>
                <button onclick="inputOperator('-')" class="btn btn-operator rounded-2xl h-16 text-xl font-medium">−</button>
                
                <!-- Row 4 -->
                <button onclick="inputNumber('1')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">1</button>
                <button onclick="inputNumber('2')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">2</button>
                <button onclick="inputNumber('3')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">3</button>
                <button onclick="inputOperator('+')" class="btn btn-operator rounded-2xl h-16 text-xl font-medium">+</button>
                
                <!-- Row 5 -->
                <button onclick="toggleSign()" class="btn btn-number rounded-2xl h-16 text-xl font-medium">±</button>
                <button onclick="inputNumber('0')" class="btn btn-number rounded-2xl h-16 text-xl font-medium">0</button>
                <button onclick="inputDecimal()" class="btn btn-number rounded-2xl h-16 text-xl font-medium">.</button>
                <button onclick="calculate()" class="btn btn-equals rounded-2xl h-16 text-xl font-medium">=</button>
            </div>
        </div>
        
        <!-- History Panel -->
        <div class="history-panel rounded-3xl p-5 w-72 hidden lg:block">
            <div class="flex items-center justify-between mb-4">
                <h3 class="text-gray-400 font-medium">History</h3>
                <button onclick="clearHistory()" class="text-gray-500 hover:text-gray-300 text-sm transition">Clear</button>
            </div>
            <div id="history" class="space-y-2 max-h-96 overflow-y-auto">
                <p class="text-gray-600 text-sm text-center py-8">No calculations yet</p>
            </div>
        </div>
    </div>

    <script>
        let currentNumber = '0';
        let previousNumber = '';
        let operator = null;
        let shouldResetDisplay = false;
        let history = [];

        const display = document.getElementById('display');
        const expression = document.getElementById('expression');
        const historyContainer = document.getElementById('history');

        function updateDisplay() {
            // Format number with commas
            let displayValue = currentNumber;
            if (!isNaN(parseFloat(displayValue))) {
                const parts = displayValue.split('.');
                parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ',');
                displayValue = parts.join('.');
            }
            
            // Adjust font size for long numbers
            if (displayValue.length > 10) {
                display.classList.remove('text-5xl');
                display.classList.add('text-3xl');
            } else {
                display.classList.remove('text-3xl');
                display.classList.add('text-5xl');
            }
            
            display.textContent = displayValue;
        }

        function updateExpression() {
            if (previousNumber && operator) {
                const op = operator === '*' ? '×' : operator === '/' ? '÷' : operator === '-' ? '−' : operator;
                expression.textContent = `${formatNumber(previousNumber)} ${op}`;
            } else {
                expression.textContent = '';
            }
        }

        function formatNumber(num) {
            const n = parseFloat(num);
            if (isNaN(n)) return num;
            return n.toLocaleString('en-US', { maximumFractionDigits: 10 });
        }

        function inputNumber(num) {
            if (shouldResetDisplay) {
                currentNumber = num;
                shouldResetDisplay = false;
            } else {
                if (currentNumber === '0' && num !== '0') {
                    currentNumber = num;
                } else if (currentNumber !== '0') {
                    currentNumber += num;
                }
            }
            updateDisplay();
        }

        function inputDecimal() {
            if (shouldResetDisplay) {
                currentNumber = '0.';
                shouldResetDisplay = false;
            } else if (!currentNumber.includes('.')) {
                currentNumber += '.';
            }
            updateDisplay();
        }

        function inputOperator(op) {
            if (operator && !shouldResetDisplay) {
                calculate(false);
            }
            previousNumber = currentNumber;
            operator = op;
            shouldResetDisplay = true;
            updateExpression();
        }

        function calculate(addToHistory = true) {
            if (!operator || !previousNumber) return;
            
            const prev = parseFloat(previousNumber);
            const current = parseFloat(currentNumber);
            let result;

            switch (operator) {
                case '+':
                    result = prev + current;
                    break;
                case '-':
                    result = prev - current;
                    break;
                case '*':
                    result = prev * current;
                    break;
                case '/':
                    result = current === 0 ? 'Error' : prev / current;
                    break;
                case '%':
                    result = prev % current;
                    break;
            }

            if (result !== 'Error') {
                // Round to avoid floating point issues
                result = Math.round(result * 1000000000) / 1000000000;
            }

            const historyEntry = {
                expression: `${formatNumber(previousNumber)} ${operator === '*' ? '×' : operator === '/' ? '÷' : operator === '-' ? '−' : operator} ${formatNumber(currentNumber)}`,
                result: result.toString()
            };

            if (addToHistory && result !== 'Error') {
                addHistoryEntry(historyEntry);
            }

            currentNumber = result.toString();
            previousNumber = '';
            operator = null;
            shouldResetDisplay = true;
            
            updateDisplay();
            updateExpression();
            
            // Add glow effect
            display.parentElement.classList.add('result-glow');
            setTimeout(() => {
                display.parentElement.classList.remove('result-glow');
            }, 1000);
        }

        function clearAll() {
            currentNumber = '0';
            previousNumber = '';
            operator = null;
            shouldResetDisplay = false;
            updateDisplay();
            updateExpression();
        }

        function backspace() {
            if (currentNumber.length > 1) {
                currentNumber = currentNumber.slice(0, -1);
            } else {
                currentNumber = '0';
            }
            updateDisplay();
        }

        function toggleSign() {
            if (currentNumber !== '0') {
                currentNumber = currentNumber.startsWith('-') 
                    ? currentNumber.slice(1) 
                    : '-' + currentNumber;
            }
            updateDisplay();
        }

        function addHistoryEntry(entry) {
            history.unshift(entry);
            if (history.length > 10) history.pop();
            renderHistory();
        }

        function renderHistory() {
            if (history.length === 0) {
                historyContainer.innerHTML = '<p class="text-gray-600 text-sm text-center py-8">No calculations yet</p>';
                return;
            }

            historyContainer.innerHTML = history.map((item, index) => `
                <div class="history-item p-3 rounded-xl cursor-pointer transition" onclick="loadFromHistory(${index})">
                    <div class="text-gray-500 text-sm">${item.expression}</div>
                    <div class="text-white text-lg font-medium">= ${formatNumber(item.result)}</div>
                </div>
            `).join('');
        }

        function loadFromHistory(index) {
            currentNumber = history[index].result;
            updateDisplay();
        }

        function clearHistory() {
            history = [];
            renderHistory();
        }

        // Keyboard support
        document.addEventListener('keydown', (e) => {
            if (e.key >= '0' && e.key <= '9') {
                inputNumber(e.key);
            } else if (e.key === '.') {
                inputDecimal();
            } else if (e.key === '+') {
                inputOperator('+');
            } else if (e.key === '-') {
                inputOperator('-');
            } else if (e.key === '*') {
                inputOperator('*');
            } else if (e.key === '/') {
                e.preventDefault();
                inputOperator('/');
            } else if (e.key === '%') {
                inputOperator('%');
            } else if (e.key === 'Enter' || e.key === '=') {
                calculate();
            } else if (e.key === 'Backspace') {
                backspace();
            } else if (e.key === 'Escape' || e.key === 'c' || e.key === 'C') {
                clearAll();
            }
        });

        // Initialize
        updateDisplay();
    </script>
</body>
</html>

