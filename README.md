Hello! Thank you for using my own calculator. If you run in a struggle/a function not works make sure to send me a bug report.
Just to clarify:
-Install fonts to all users(it has a folder for it)
-If you don't know how to, you select all files in it, right click -> install for all users -> yes and wait till it installs, open website and you're good to go!

code(with comments, so you can understand it):
```
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Character encoding for the webpage -->
    <meta charset="UTF-8">
    <!-- Viewport settings for responsiveness -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- Title of the calculator page -->
    <title>iPhone Calculator</title>
    <style>
        /* General styles for the body to center the calculator */
        body {
            background: black;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        /* Style for the calculator container */
        .calculator {
            width: 300px;
            padding: 15px;
            border-radius: 20px;
            background: black;
            box-shadow: 0px 4px 10px rgba(255, 255, 255, 0.1);
            font-family: "SF Pro Display", sans-serif;
        }
        /* Style for the display area */
        .display {
            color: white;
            font-size: 2rem;
            text-align: right;
            padding: 10px;
            background: black;
            border-radius: 10px;
            height: 60px;
            overflow: hidden;
        }
        /* Button grid styles */
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            margin-top: 10px;
        }
        /* Styles for individual buttons */
        button {
            height: 60px;
            border-radius: 50%;
            font-size: 1.5rem;
            border: none;
            cursor: pointer;
            transition: 0.2s;
        }
        /* Color styles for different button types */
        .gray { background: #a5a5a5; color: black; }
        .darkgray { background: #333; color: white; }
        .orange { background: #ff9500; color: white; }
        .zero { grid-column: span 2; border-radius: 30px; }
        /* Button click effect */
        button:active { opacity: 0.7; }
    </style>
</head>
<body>
    <div class="calculator">
        <!-- Display for showing current input or result -->
        <div class="display" id="display">0</div>
        <!-- Buttons for the calculator -->
        <div class="buttons">
            <!-- Buttons for clearing, toggling sign, percentage, and operators -->
            <button class="gray" onclick="clearDisplay()">C</button>
            <button class="gray" onclick="toggleSign()">+/-</button>
            <button class="gray" onclick="percent()">%</button>
            <button class="orange" onclick="appendOperator('/')">÷</button>

            <!-- Number buttons -->
            <button class="darkgray" onclick="appendNumber(7)">7</button>
            <button class="darkgray" onclick="appendNumber(8)">8</button>
            <button class="darkgray" onclick="appendNumber(9)">9</button>
            <button class="orange" onclick="appendOperator('*')">×</button>

            <button class="darkgray" onclick="appendNumber(4)">4</button>
            <button class="darkgray" onclick="appendNumber(5)">5</button>
            <button class="darkgray" onclick="appendNumber(6)">6</button>
            <button class="orange" onclick="appendOperator('-')">−</button>

            <button class="darkgray" onclick="appendNumber(1)">1</button>
            <button class="darkgray" onclick="appendNumber(2)">2</button>
            <button class="darkgray" onclick="appendNumber(3)">3</button>
            <button class="orange" onclick="appendOperator('+')">+</button>

            <button class="darkgray zero" onclick="appendNumber(0)">0</button>
            <button class="darkgray" onclick="appendDot()">.</button>
            <button class="orange" onclick="calculateResult()">=</button>
        </div>
    </div>

    <script>
        // Initial variables for the display and calculation logic
        let display = document.getElementById("display");
        let currentInput = "0"; // Current input on display
        let operator = null; // Current operator (+, -, *, /)
        let firstValue = null; // First value for the operation
        let waitingForSecondValue = false; // Flag to check if we are waiting for second operand

        // Update the display to show the current input
        function updateDisplay() {
            display.textContent = currentInput;
        }

        // Append a number to the current input
        function appendNumber(num) {
            // If waiting for the second value, start a new number
            if (waitingForSecondValue) {
                currentInput = String(num);
                waitingForSecondValue = false;
            } else {
                // If the current input is "0", replace it with the number
                // Otherwise, append the number to the current input
                currentInput = currentInput === "0" ? String(num) : currentInput + num;
            }
            updateDisplay();
        }

        // Append an operator (+, -, *, /) to the calculation
        function appendOperator(op) {
            // Only proceed if not waiting for second value
            if (!waitingForSecondValue) {
                firstValue = currentInput;
                operator = op;
                waitingForSecondValue = true;
            }
        }

        // Append a decimal point to the current input
        function appendDot() {
            // Add a dot only if there isn't one already
            if (!currentInput.includes(".")) {
                currentInput += ".";
                updateDisplay();
            }
        }

        // Clear the display and reset all variables
        function clearDisplay() {
            currentInput = "0";
            operator = null;
            firstValue = null;
            waitingForSecondValue = false;
            updateDisplay();
        }

        // Toggle the sign of the current input (positive/negative)
        function toggleSign() {
            currentInput = String(parseFloat(currentInput) * -1);
            updateDisplay();
        }

        // Convert the current input to a percentage
        function percent() {
            currentInput = String(parseFloat(currentInput) / 100);
            updateDisplay();
        }

        // Perform the calculation based on the operator and values
        function calculateResult() {
            if (operator && firstValue !== null) {
                // Use eval to evaluate the expression (e.g., 5 + 3)
                currentInput = String(eval(firstValue + operator + currentInput));
                operator = null; // Reset the operator
                firstValue = null; // Reset the first value
                waitingForSecondValue = false; // Reset the flag
                updateDisplay();
            }
        }

        // Add keyboard support for the calculator
        document.addEventListener("keydown", function(event) {
            const key = event.key;
            if (!isNaN(key)) {
                // If the key is a number, append it to the display
                appendNumber(parseInt(key));
            } else if (key === "+" || key === "-" || key === "*" || key === "/") {
                // If the key is an operator, append it
                appendOperator(key);
            } else if (key === "Enter" || key === "=") {
                // If Enter is pressed, calculate the result
                calculateResult();
            } else if (key === "Backspace") {
                // Backspace will delete the last character
                currentInput = currentInput.slice(0, -1) || "0";
                updateDisplay();
            } else if (key === "Escape") {
                // Escape will clear the display
                clearDisplay();
            } else if (key === ".") {
                // Dot key will append a dot
                appendDot();
            }
        });
    </script>
</body>
</html>
```
