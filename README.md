<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Age, Months, Days Calculator with Digital Clock</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: black; /* Black sky background */
            color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            flex-direction: column;
            overflow: hidden;
        }

        .container {
            background-color: rgba(0, 0, 0, 0.7);
            padding: 40px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 400px;
            text-align: center;
            margin-top: 30px;
        }

        h1 {
            margin-bottom: 20px;
            font-size: 24px;
        }

        input {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
        }

        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }

        .result {
            margin-top: 20px;
            font-size: 20px;
            font-weight: bold;
        }

        /* Moon Styling */
        .moon {
            position: absolute;
            top: 50px;
            right: 50px;
            width: 120px;
            height: 120px;
            background-color: #f1f1f1;
            border-radius: 50%;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.5);
            animation: float 10s ease-in-out infinite;
        }

        /* Digital Clock Styling (Upper Centered) */
        .clock {
            font-size: 48px; /* Increased size for visibility */
            position: absolute;
            top: 10px; /* Position at the top */
            left: 50%; /* Center horizontally */
            transform: translateX(-50%); /* Adjust for true centering */
            background-color: rgba(0, 0, 0, 0.7);
            padding: 15px;
            border-radius: 8px;
            color: #fff;
            font-family: 'Courier New', Courier, monospace;
            text-align: center;
        }

        /* Twinkling Stars */
        .star {
            position: absolute;
            background-color: #fff;
            border-radius: 50%;
            opacity: 0;
            animation: twinkle 2s infinite ease-in-out;
        }

        /* Random Size and Position for Stars */
        .star:nth-child(odd) {
            animation-duration: 2.5s;
        }

        .star:nth-child(even) {
            animation-duration: 3s;
        }

        /* Keyframes for Twinkling Stars */
        @keyframes twinkle {
            0% {
                opacity: 0;
            }
            50% {
                opacity: 1;
            }
            100% {
                opacity: 0;
            }
        }

        /* Keyframes for Moon Animation */
        @keyframes float {
            0% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-20px);
            }
            100% {
                transform: translateY(0);
            }
        }

    </style>
</head>
<body>
    <!-- Digital Clock -->
    <div id="clock" class="clock"></div>

    <div class="moon"></div>

    <!-- Twinkling Stars -->
    <div class="star" style="top: 30px; left: 40%; width: 3px; height: 3px;"></div>
    <div class="star" style="top: 70px; left: 10%; width: 2px; height: 2px;"></div>
    <div class="star" style="top: 120px; left: 60%; width: 2px; height: 2px;"></div>
    <div class="star" style="top: 200px; left: 75%; width: 4px; height: 4px;"></div>
    <div class="star" style="top: 150px; left: 85%; width: 2px; height: 2px;"></div>
    <div class="star" style="top: 300px; left: 40%; width: 3px; height: 3px;"></div>
    <div class="star" style="top: 400px; left: 30%; width: 2px; height: 2px;"></div>

    <div class="container">
        <h1>Age, Months, and Days Calculator</h1>
        <label for="birthdate">Enter your birthdate:</label>
        <input type="date" id="birthdate" required>
        <button onclick="calculateAge()">Calculate</button>
        <div id="result" class="result"></div>
    </div>

    <script>
        // Function to update the digital clock every second
        function updateClock() {
            const clockDiv = document.getElementById('clock');
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');
            clockDiv.textContent = `${hours}:${minutes}:${seconds}`;
        }

        // Update the clock every second
        setInterval(updateClock, 1000);

        // Initial call to display the clock immediately
        updateClock();

        // Function to calculate the age when the "Calculate" button is clicked
        function calculateAge() {
            const birthdate = document.getElementById('birthdate').value;
            const resultDiv = document.getElementById('result');
            
            if (!birthdate) {
                resultDiv.textContent = 'Please enter a valid birthdate.';
                return;
            }
            
            const birthDateObj = new Date(birthdate);
            const currentDate = new Date();
            
            let years = currentDate.getFullYear() - birthDateObj.getFullYear();
            const months = currentDate.getMonth() - birthDateObj.getMonth();
            const days = currentDate.getDate() - birthDateObj.getDate();

            // Adjust years and months if the birthday hasn't occurred this year yet
            if (months < 0 || (months === 0 && days < 0)) {
                years--;
            }

            // Adjust months and days if the birthday hasn't occurred yet this month
            let fullMonths = months < 0 ? months + 12 : months;
            let fullDays = days < 0 ? new Date(currentDate.getFullYear(), currentDate.getMonth(), 0).getDate() + days : days;

            // If the birthday is later in the current month, subtract the months
            let totalMonths = years * 12 + fullMonths;

            resultDiv.innerHTML = `
                You are <strong>${years}</strong> years, <strong>${fullMonths}</strong> months, and <strong>${fullDays}</strong> days old.<br>
                Total months lived: <strong>${totalMonths}</strong> months.<br>
                Total days lived: <strong>${Math.floor((currentDate - birthDateObj) / (1000 * 60 * 60 * 24))}</strong> days.
            `;
        }

        // Add event listener for "Enter" key press
        document.getElementById('birthdate').addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                calculateAge(); // Call the calculateAge function when Enter is pressed
            }
        });
    </script>
</body>
</html>
