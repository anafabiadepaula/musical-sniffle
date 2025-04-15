<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Rastrear Localização</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background-color: #f0f8ff;
      padding: 50px;
    }

    button {
      padding: 10px 20px;
      font-size: 16px;
      margin-top: 20px;
      cursor: pointer;
    }

    #local {
      margin-top: 20px;
      font-weight: bold;
    }

    iframe {
      margin-top: 20px;
      width: 90%;
      max-width: 600px;
      height: 400px;
      border: none;
    }
  </style>
</head>
<body>
  <h1>Bem-vindo!</h1>
  <p>Clique no botão abaixo para mostrar e salvar sua localização:</p>
  <button onclick="getLocation()">Mostrar minha localização</button>
  <p id="local"></p>
  <div id="mapa"></div>

  <!-- Firebase SDKs -->
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js"></script>

  <script>
    // Configuração do Firebase
    const firebaseConfig = {
      apiKey: "AIzaSyBtyTHSbMicNSS1tmylkGl3SYGntqaVdlg",
      authDomain: "localiza-10da3.firebaseapp.com",
      projectId: "localiza-10da3",
      storageBucket: "localiza-10da3.firebasestorage.app",
      messagingSenderId: "641360259439",
      appId: "1:641360259439:web:2ee477a67e95a9012b609a",
      measurementId: "G-FTMDR435S1"
    };

    // Inicializar Firebase
    const app = firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    function getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition, showError);
      } else {
        document.getElementById("local").innerHTML = "Geolocalização não é suportada pelo seu navegador.";
      }
    }

    function showPosition(position) {
      const lat = position.coords.latitude;
      const lon = position.coords.longitude;

      document.getElementById("local").innerHTML =
        `Latitude: ${lat.toFixed(5)}<br>Longitude: ${lon.toFixed(5)}`;

      const mapaUrl = `https://maps.google.com/maps?q=${lat},${lon}&z=15&output=embed`;
      document.getElementById("mapa").innerHTML =
        `<iframe src="${mapaUrl}"></iframe>`;

      // Salvar no Firebase
      db.collection("localizacoes").add({
        latitude: lat,
        longitude: lon,
        timestamp: new Date()
      })
      .then(() => {
        console.log("Localização salva no Firebase!");
      })
      .catch((error) => {
        console.error("Erro ao salvar no Firebase: ", error);
      });
    }

    function showError(error) {
      let mensagem = "";
      switch(error.code) {
        case error.PERMISSION_DENIED:
          mensagem = "Permissão negada.";
          break;
        case error.POSITION_UNAVAILABLE:
          mensagem = "Localização indisponível.";
          break;
        case error.TIMEOUT:
          mensagem = "Tempo de solicitação expirado.";
          break;
        default:
          mensagem = "Erro desconhecido.";
      }
      document.getElementById("local").innerHTML = mensagem;
    }
  </script>
</body>
</html>
