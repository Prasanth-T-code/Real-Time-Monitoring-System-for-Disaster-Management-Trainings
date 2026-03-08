# Real-Time-Monitoring-System-for-Disaster-Management-Trainings
<!DOCTYPE html>
<html>

<head>
    <title>Disaster Training Monitoring System</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            height: 100vh;
            background: linear-gradient(135deg, #2980b9, #6dd5fa, #ffffff);
        }
        /* HEADER */
        
        header {
            text-align: center;
            padding: 25px;
            color: white;
            font-size: 28px;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        /* CENTER LOGIN */
        
        #loginPage {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 80vh;
        }
        
        .loginBox {
            background: white;
            padding: 40px 35px;
            width: 350px;
            border-radius: 15px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
            text-align: center;
            animation: fade 1s;
        }
        
        @keyframes fade {
            from {
                opacity: 0;
                transform: translateY(-30px);
            }
            to {
                opacity: 1;
            }
        }
        
        .loginBox h2 {
            margin-bottom: 25px;
            color: #2980b9;
        }
        
        input {
            width: 85%;
            padding: 12px;
            margin: 12px 0;
            border-radius: 8px;
            border: 1px solid #ccc;
            font-size: 14px;
            transition: 0.3s;
        }
        
        input:focus {
            border-color: #2980b9;
            box-shadow: 0 0 8px rgba(41, 128, 185, 0.5);
            outline: none;
        }
        
        button {
            padding: 12px 30px;
            background: #2980b9;
            border: none;
            color: white;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            margin-top: 15px;
            transition: 0.3s;
        }
        
        button:hover {
            background: #1f6391;
            transform: scale(1.05);
        }
        /* DASHBOARD */
        
        #dashboard {
            display: none;
            padding: 30px 50px;
        }
        
        .card {
            background: white;
            padding: 25px;
            margin: 20px 0;
            border-radius: 15px;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
        }
        
        h2,
        h3 {
            text-align: center;
            color: #2980b9;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }
        
        th,
        td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: center;
            font-size: 14px;
        }
        
        th {
            background: #34495e;
            color: white;
        }
        
        #monitor,
        #report {
            font-size: 16px;
            font-weight: bold;
            color: #2980b9;
            margin-top: 15px;
        }
    </style>
</head>

<body>

    <header>
        Real-Time Monitoring System for Disaster Management Trainings
    </header>

    <!-- LOGIN PAGE -->
    <div id="loginPage">
        <div class="loginBox">
            <h2>Admin Login</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <br>
            <button onclick="login()">Login</button>
        </div>
    </div>

    <!-- DASHBOARD -->
    <div id="dashboard">

        <div class="card">
            <h2>Training Dashboard</h2>
            <p style="text-align:center;">Welcome to Disaster Training Monitoring System</p>
        </div>

        <div class="card">
            <h3>Add Training</h3>
            <input type="text" id="name" placeholder="Training Name">
            <input type="text" id="status" placeholder="Status (Active/Completed)">
            <input type="number" id="people" placeholder="Number of Trainees">
            <br>
            <button onclick="addTraining()">Add Training</button>
        </div>

        <div class="card">
            <h3>Training List</h3>
            <table id="trainingTable">
                <tr>
                    <th>Training</th>
                    <th>Status</th>
                    <th>Trainees</th>
                </tr>
            </table>
        </div>

        <div class="card">
            <h3>Real-Time Monitoring</h3>
            <p id="monitor" style="text-align:center;">No Active Training</p>
        </div>

        <div class="card">
            <h3>Training Report</h3>
            <button onclick="generateReport()">Generate Report</button>
            <p id="report"></p>
        </div>

        <div class="card">
            <h3>User Login Records</h3>
            <p id="userRecords">No users logged in yet</p>
        </div>

    </div>

    <script>
        let trainings = [];
        let userLogins = []; // Store unique user login details

        function login() {
            let user = document.getElementById("username").value;
            let pass = document.getElementById("password").value;

            if ((user === "admin" && pass === "admin") || (user === "prasanth" && pass === "123")) {

                // Record unique login
                let exists = userLogins.find(u => u.username === user);
                if (!exists) {
                    userLogins.push({
                        username: user,
                        password: pass
                    });
                    updateUserRecords();
                }

                document.getElementById("loginPage").style.display = "none";
                document.getElementById("dashboard").style.display = "block";

            } else {
                alert("Invalid Login");
            }
        }

        // Update the user login records on dashboard
        function updateUserRecords() {
            let recordElement = document.getElementById("userRecords");
            if (userLogins.length === 0) {
                recordElement.innerHTML = "No users logged in yet";
            } else {
                recordElement.innerHTML = userLogins.map(u => `Username: ${u.username} | Password: ${u.password}`).join("<br>");
            }
        }

        function addTraining() {
            let name = document.getElementById("name").value;
            let status = document.getElementById("status").value;
            let people = document.getElementById("people").value;

            if (!name || !status || !people) {
                alert("Please fill all fields");
                return;
            }

            let table = document.getElementById("trainingTable");
            let row = table.insertRow();
            row.insertCell(0).innerHTML = name;
            row.insertCell(1).innerHTML = status;
            row.insertCell(2).innerHTML = people;

            trainings.push({
                name,
                status,
                people
            });
            updateMonitor();

            // Clear input fields
            document.getElementById("name").value = "";
            document.getElementById("status").value = "";
            document.getElementById("people").value = "";
        }

        function updateMonitor() {
            let active = trainings.filter(t => t.status.toLowerCase() == "active");
            document.getElementById("monitor").innerHTML =
                active.length > 0 ? "Active Trainings Running : " + active.length : "No Active Training";
        }

        function generateReport() {
            let total = trainings.length;
            let active = trainings.filter(t => t.status.toLowerCase() == "active").length;
            let completed = trainings.filter(t => t.status.toLowerCase() == "completed").length;

            document.getElementById("report").innerHTML =
                "Total Trainings: " + total +
                "<br>Active Trainings: " + active +
                "<br>Completed Trainings: " + completed;
        }
    </script>

</body>

</html>
