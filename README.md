# Welcome! 🎉

Thank you for using my custom calculator! If you encounter any issues or notice a malfunctioning function, please don't hesitate to send me a bug report. 

### Quick Setup Guide:
1. **Font Installation**: 
   - The fonts need to be installed for all users. A dedicated folder is provided for this purpose.
   
2. **Installation Steps**: 
   - Simply select all files in the folder, right-click, and choose **Install for all users**.
   - Confirm the installation when prompted, then wait for it to complete.

3. Once done, just open the website and you’re all set to go!

code(with comments, so you can understand it):
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>iPhone Calculator</title>
    <style>
        /* Basic styling for body */
        body {
            background: black;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        /* Calculator container styling */
        .calculator {
            width: 300px;
            padding: 15px;
            border-radius: 20px;
            background: black;
            box-shadow: 0px 4px 10px rgba(255, 255, 255, 0.1);
            font-family: "SF Pro Display", sans-serif;
        }

        /* Display screen styling */
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

        /* Button grid styling */
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            margin-top: 10px;
        }

        /* General button styling */
        button {
            height: 60px;
            border-radius: 50%;
            font-size: 1.5rem;
            border: none;
            cursor: pointer;
            transition: 0.2s;
        }

        /* Button colors */
        .gray { background: #a5a5a5; color: black; }
        .darkgray { background: #333; color: white; }
        .orange { background: #ff9500; color: white; }

        /* Zero button styling */
        .zero { grid-column: span 2; border-radius: 30px; }

        /* Button active state */
        button:active { opacity: 0.7; }
    </style>
</head>
<body>
    <div class="calculator">
        <!-- Display screen where numbers are shown -->
        <div class="display" id="display">0</div>
        
        <!-- Buttons grid for calculator -->
        <div class="buttons">
            <button class="gray" onclick="clearDisplay()">C</button>
            <button class="gray" onclick="toggleSign()">+/-</button>
            <button class="gray" onclick="percent()">%</button>
            <button class="orange" onclick="appendOperator('/')">÷</button>

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
        /* Variable declarations for calculator functionality */
        let display = document.getElementById("display");  // The calculator display element
        let currentInput = "0";  // Stores the current input as a string
        let operator = null;  // Stores the current operator for calculation
        let firstValue = null;  // Stores the first number before an operator
        let waitingForSecondValue = false;  // Flag to check if waiting for second input after an operator

        /* Function to update the calculator display with the current input */
        function updateDisplay() {
            display.textContent = currentInput;
        }

        /* Function to append a number to the current input */
        function appendNumber(num) {
            // If waiting for the second value after an operator
            if (waitingForSecondValue) {
                currentInput = String(num);  // Start a new number
                waitingForSecondValue = false;  // Reset flag
            } else {
                currentInput = currentInput === "0" ? String(num) : currentInput + num;  // Append number to input
            }
            updateDisplay();  // Update the display
        }

        /* Function to append an operator to the calculation */
        function appendOperator(op) {
            if (!waitingForSecondValue) {
                firstValue = currentInput;  // Store the first number
                operator = op;  // Store the operator
                waitingForSecondValue = true;  // Set flag to wait for second value
            }
        }

        /* Function to append a decimal point to the current input */
        function appendDot() {
            if (!currentInput.includes(".")) {  // Check if decimal already exists
                currentInput += ".";  // Add decimal point
                updateDisplay();  // Update the display
            }
        }

        /* Function to clear the display */
        function clearDisplay() {
            currentInput = "0";  // Reset current input
            operator = null;  // Reset operator
            firstValue = null;  // Reset first value
            waitingForSecondValue = false;  // Reset flag
            updateDisplay();  // Update the display
        }

        /* Function to toggle the sign of the current input (positive/negative) */
        function toggleSign() {
            currentInput = String(parseFloat(currentInput) * -1);  // Multiply by -1 to toggle sign
            updateDisplay();  // Update the display
        }

        /* Function to calculate the percentage of the current input */
        function percent() {
            currentInput = String(parseFloat(currentInput) / 100);  // Divide input by 100
            updateDisplay();  // Update the display
        }

        /* Function to calculate the result of the operation */
        function calculateResult() {
            if (operator && firstValue !== null) {
                currentInput = String(eval(firstValue + operator + currentInput));  // Perform the calculation
                operator = null;  // Reset operator
                firstValue = null;  // Reset first value
                waitingForSecondValue = false;  // Reset flag
                updateDisplay();  // Update the display
            }
        }

        /* Add event listener for keyboard input */
        document.addEventListener("keydown", function(event) {
            const key = event.key;
            // Handle number keys
            if (!isNaN(key)) {
                appendNumber(parseInt(key));
            } 
            // Handle operator keys
            else if (key === "+" || key === "-" || key === "*" || key === "/") {
                appendOperator(key);
            } 
            // Handle equal sign or Enter key
            else if (key === "Enter" || key === "=") {
                calculateResult();
            } 
            // Handle backspace for deleting last input
            else if (key === "Backspace") {
                currentInput = currentInput.slice(0, -1) || "0";  // Remove last character
                updateDisplay();  // Update the display
            } 
            // Handle Escape key to clear the display
            else if (key === "Escape") {
                clearDisplay();
            } 
            // Handle decimal point key
            else if (key === ".") {
                appendDot();
            }
        });
    </script>
</body>
</html>
```
