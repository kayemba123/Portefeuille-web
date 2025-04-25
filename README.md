<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ma Caisse Électronique</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-100 text-gray-800">
  <div class="max-w-xl mx-auto p-6">
    <h1 class="text-3xl font-bold mb-6 text-center">Portefeuille en ligne</h1>

    <div id="user-auth" class="mb-6">
      <input id="username" type="text" placeholder="Nom d'utilisateur" class="border p-2 w-full mb-2">
      <button onclick="login()" class="bg-blue-600 text-white px-4 py-2 w-full">Connexion</button>
    </div>

    <div id="dashboard" class="hidden">
      <h2 class="text-xl mb-2">Bienvenue, <span id="user-name"></span></h2>
      <p>Solde : <span id="user-balance">0</span> FC</p>

      <div class="mt-4">
        <input id="amount" type="number" placeholder="Montant (FC)" class="border p-2 w-full mb-2">
        <button onclick="deposit()" class="bg-green-600 text-white px-4 py-2 w-full mb-2">Dépôt</button>
        <button onclick="withdraw()" class="bg-red-600 text-white px-4 py-2 w-full">Retrait</button>
      </div>

      <h3 class="text-lg mt-6 mb-2">Historique</h3>
      <ul id="history" class="bg-white border rounded p-4 h-40 overflow-y-auto text-sm">
        <!-- Historique des transactions -->
      </ul>
    </div>
  </div>

  <script>
    let user = null;

    function login() {
      const name = document.getElementById('username').value;
      if (!name) return alert('Veuillez entrer un nom d\\'utilisateur.');

      user = {
        name: name,
        balance: 0,
        history: []
      };

      document.getElementById('user-name').innerText = user.name;
      document.getElementById('user-balance').innerText = user.balance;
      document.getElementById('user-auth').classList.add('hidden');
      document.getElementById('dashboard').classList.remove('hidden');
    }

    // Simuler une transaction mobile money via une API
    function mobileMoneyTransaction(amount, type) {
      // Simulations de services Mobile Money
      const services = ["Airtel", "Orange Money", "M-Pesa"];
      const service = services[Math.floor(Math.random() * services.length)]; // Choisir un service aléatoirement
      console.log(`${type} de ${amount} FC via ${service}`);

      // Retour de l'API simulée
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          // Simulation de réussite
          resolve(true);
        }, 2000);
      });
    }

    function deposit() {
      const amount = parseInt(document.getElementById('amount').value);
      if (isNaN(amount) || amount <= 0) return alert('Montant invalide.');

      // Appel à l'API Mobile Money pour simuler un dépôt
      mobileMoneyTransaction(amount, 'Dépôt').then(() => {
        user.balance += amount;
        user.history.push(`+ ${amount} FC`);
        updateUI();
      }).catch(err => alert('Erreur lors du dépôt.'));
    }

    function withdraw() {
      const amount = parseInt(document.getElementById('amount').value);
      if (isNaN(amount) || amount <= 0) return alert('Montant invalide.');
      if (user.balance < amount) return alert('Solde insuffisant.');

      // Appel à l'API Mobile Money pour simuler un retrait
      mobileMoneyTransaction(amount, 'Retrait').then(() => {
        user.balance -= amount;
        user.history.push(`- ${amount} FC`);
        updateUI();
      }).catch(err => alert('Erreur lors du retrait.'));
    }

    function updateUI() {
      document.getElementById('user-balance').innerText = user.balance;
      const historyList = document.getElementById('history');
      historyList.innerHTML = '';
      user.history.slice().reverse().forEach(item => {
        const li = document.createElement('li');
        li.innerText = item;
        historyList.appendChild(li);
      });
    }
  </script>
</body>
</html>
