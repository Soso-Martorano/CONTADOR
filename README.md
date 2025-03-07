<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contador de Beijos</title>
    <link rel="icon" href="https://imgur.com/gE5nLN9.png" type="image/png">
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            background: #ffcccc;
            color: #a30000;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 350px;
            margin: 20px auto;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
        }
        .profile-img {
            width: 100px;
            height: auto;
            border-radius: 10px;
            margin-bottom: 10px;
        }
        .lobby-img {
            width: 150px;
            height: auto;
            border-radius: 10px;
            display: block;
            margin: 0 auto 10px;
        }
        button {
            padding: 10px;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            margin: 5px;
            background: #ff4d4d;
            color: white;
        }
        button:hover {
            background: #cc0000;
        }
        .counter {
            font-size: 20px;
            font-weight: bold;
        }
        .history {
            text-align: left;
            max-height: 150px;
            overflow-y: auto;
            background: white;
            padding: 10px;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <!-- Tela de Senha -->
    <div id="password-box" class="container">
        <h2>Digite a senha</h2>
        <!-- Alterado o placeholder para "Senha:" -->
        <input type="password" id="password" placeholder="Senha:">
        <button onclick="checkPassword()">Entrar</button>
    </div>

    <!-- Tela de Login -->
    <div id="login-box" class="container" style="display: none;">
        <img src="https://imgur.com/gE5nLN9.png" class="lobby-img">
        <h2>Contador de Beijos</h2>
        <input type="text" id="username" placeholder="Digite seu nome">
        <button onclick="login()">Entrar</button>
    </div>
    
    <div id="user-box" class="container" style="display: none;">
        <img id="user-img" class="profile-img">
        <h3 id="user-title"></h3>
        <input type="number" id="beijo-input" placeholder="Digite a quantidade">
        <button onclick="addBeijo()">Adicionar</button>
        <button onclick="removeBeijo()">Remover</button>
        <p class="counter">Total: <span id="user-total">0</span></p>
    </div>
    
    <div class="container">
        <h3>Histórico de Beijos</h3>
        <div class="history" id="history-box"></div>
    </div>

    <script>
        let beijos = JSON.parse(localStorage.getItem('beijos')) || { jujubinha: 0, molango: 0, history: [] };
        let currentUser = "";

        // Verifica a senha antes de exibir a tela de login
        function checkPassword() {
            let password = document.getElementById("password").value;
            if (password === "Bolo de jujuba") {
                document.getElementById("password-box").style.display = "none";
                document.getElementById("login-box").style.display = "block";
            } else {
                alert("Senha incorreta!");
            }
        }

        function login() {
            let user = document.getElementById("username").value.toLowerCase();
            if (user === "jujubinha" || user === "molango") {
                currentUser = user;
                document.getElementById("user-box").style.display = "block";
                document.getElementById("user-title").innerText = user.charAt(0).toUpperCase() + user.slice(1);
                document.getElementById("user-img").src = user === "jujubinha" ? "https://imgur.com/0nrP4oN.png" : "https://imgur.com/lVAFPOg.png";
                updateCounter();
                showHistory();
            } else {
                alert("Usuário não encontrado! Digite 'Jujubinha' ou 'Molango'.");
            }
        }

        function addBeijo() {
            let amount = parseInt(document.getElementById("beijo-input").value);
            if (!isNaN(amount) && amount > 0) {
                beijos[currentUser] += amount;
                beijos.history.push({ user: currentUser, amount: amount, date: new Date().toLocaleString() });
                saveData();
                updateCounter();
                showHistory();
            }
        }

        function removeBeijo() {
            let amount = parseInt(document.getElementById("beijo-input").value);
            if (!isNaN(amount) && amount > 0 && beijos[currentUser] >= amount) {
                beijos[currentUser] -= amount;
                beijos.history.push({ user: currentUser, amount: -amount, date: new Date().toLocaleString() });
                saveData();
                updateCounter();
                showHistory();
            }
        }

        function updateCounter() {
            document.getElementById("user-total").innerText = beijos[currentUser];
        }

        function saveData() {
            localStorage.setItem("beijos", JSON.stringify(beijos));
        }

        function showHistory() {
            let historyBox = document.getElementById("history-box");
            historyBox.innerHTML = "";
            beijos.history.slice().reverse().forEach(entry => {
                let item = document.createElement("p");
                item.innerText = `${entry.date} - ${entry.user}: ${entry.amount > 0 ? '+' : ''}${entry.amount} beijos`;
                historyBox.appendChild(item);
            });
        }
    </script>
</body>
</html>
