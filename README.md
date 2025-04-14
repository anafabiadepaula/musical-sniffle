# musical-sniffle index.html<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Localização do Usuário</title>
</head>
<body>
    <h1>Bem-vindo!</h1>
    <p id="status">Solicitando acesso à sua localização...</p>
    <p id="localizacao"></p>

    <script>
        function sucesso(posicao) {
            const latitude = posicao.coords.latitude;
            const longitude = posicao.coords.longitude;
            document.getElementById("status").innerText = "Localização obtida com sucesso:";
            document.getElementById("localizacao").innerText = `Latitude: ${latitude}, Longitude: ${longitude}`;
        }

        function erro() {
            document.getElementById("status").innerText = "Não foi possível obter sua localização.";
        }

        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(sucesso, erro);
        } else {
            document.getElementById("status").innerText = "Geolocalização não é suportada neste navegador.";
        }
    </script>
</body>
</html>
