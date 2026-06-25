DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Norval Business - Inscription</title>
    <style>
        body{font-family:Arial,sans-serif;background:#f0f2f5;padding:20px;margin:0}
      .container{max-width:400px;margin:0 auto;background:white;padding:30px;border-radius:10px;box-shadow:0 2px 10px rgba(0,0,0,0.1)}
        h1{color:#1877f2;text-align:center;margin-bottom:20px}
        input{width:100%;padding:12px;margin:8px 0;border:1px solid #ddd;border-radius:6px;box-sizing:border-box}
        button{width:100%;background:#1877f2;color:white;padding:14px;border:none;border-radius:6px;font-size:16px;font-weight:bold;cursor:pointer;margin-top:10px}
      .error{color:red;font-size:14px;margin-top:5px}
      .info{background:#e7f3ff;padding:10px;border-radius:6px;margin:10px 0;font-size:14px}
    </style>
</head>
<body>
    <div class="container">
        <h1>Rejoignez NORVAL BUSINESS</h1>
        <div class="info">🎁 Gagnez 24F/jour + 100F par filleul</div>
        <input type="tel" id="numero" placeholder="Votre numéro WhatsApp (228XXXXXXXX)" required>
        <input type="tel" id="parrain" placeholder="Numéro du parrain" required>
        <div class="error" id="error"></div>
        <button onclick="inscrire()">S'inscrire - Activation 500F</button>
    </div>

    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <script>
        const firebaseConfig = {
            apiKey: "AIzaSyAnSyDW11gaKXUktRjd4W5lJoe5LbMT-ZY",
            authDomain: "norval-buisiness.firebaseapp.com",
            projectId: "norval-buisiness",
            storageBucket: "norval-buisiness.firebasestorage.app",
            messagingSenderId: "307580475568",
            appId: "1:307580475568:web:5fe0eeb5afdd441e914375"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        const urlParams = new URLSearchParams(window.location.search);
        const parrainLink = urlParams.get('parrain');
        if(parrainLink) document.getElementById('parrain').value = parrainLink;

        async function inscrire(){
            const numero = document.getElementById('numero').value.trim();
            const parrain = document.getElementById('parrain').value.trim();
            const error = document.getElementById('error');
            
            if(!numero ||!parrain){
                error.textContent = "Remplis tous les champs";
                return;
            }
            if(numero === parrain){
                error.textContent = "Tu ne peux pas être ton propre parrain";
                return;
            }

            const checkUser = await db.collection('users').doc(numero).get();
            if(checkUser.exists){
                error.textContent = "Ce numéro est déjà inscrit";
                return;
            }

            localStorage.setItem('norval_temp_user', JSON.stringify({numero, parrain}));
            window.location.href = 'credit.html';
        }
    </script>
</body>
</html>