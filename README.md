mkdir discord-dashboard
cd discord-dashboard
npm init -y
npm install express ejs
const express = require('express');
const path = require('path');
const app = express();
const port = 3000;

// Configura o EJS como o template engine
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

// Middleware para servir arquivos estáticos (como CSS ou imagens)
app.use(express.static(path.join(__dirname, 'public'))); // Crie uma pasta 'public' se precisar de arquivos estáticos

// Rota principal (página do dashboard)
app.get('/', (req, res) => {
    // Exemplo de dados que você pode passar para o template EJS
    // No seu projeto real, esses dados viriam de uma API ou banco de dados
    const dashboardData = {
        serverName: "Nomwendwha",
        memberCount: 150,
        onlineMembers: 85,
        messagesSent: 12450,
        botStatus: "Online",
        recentActivities: [
            "J doat",
            "?? ",
            "?"
        ]
    };
    res.render('index', { data: dashboardData });
});

// load
app.post('/api/update', express.json(), (req, res) => {
    // receiver bot data
    const newData = req.body;
    console.log('Dados recebidos do bot:', newData);

    // 
    // Exemplo: saveToDatabase(newData);

    res.status(200).send({ message: 'Dados recebidos com sucesso!' });
});

app.listen(port, () => {
    console.log(`Dashboard load on http://localhost:${port}`);
});
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= data.serverName %> - Dashboard do Bot</title>
    <style>
        body { font-family: Arial, sans-serif; background-color: #f0f4f8; color: #333; margin: 0; padding: 20px; }
        .container { max-width: 900px; margin: auto; background: #fff; padding: 20px; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        h1 { color: #5865F2; }
        .card-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; margin-top: 20px; }
        .card { background-color: #f8f9fa; padding: 15px; border-radius: 6px; text-align: center; }
        .card h3 { margin: 0 0 10px 0; color: #6e7a88; }
        .card p { font-size: 2em; margin: 0; font-weight: bold; }
        .status { margin-top: 20px; }
        .status h3 { margin-bottom: 10px; }
        .status-online { color: green; font-weight: bold; }
        .activities-list { list-style: none; padding: 0; }
        .activities-list li { padding: 8px 0; border-bottom: 1px solid #eee; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Dashboard do <%= data.serverName %></h1>

        <div class="card-grid">
            <div class="card">
                <h3>Membros Totais</h3>
                <p><%= data.memberCount %></p>
            </div>
            <div class="card">
                <h3>Membros Online</h3>
                <p><%= data.onlineMembers %></p>
            </div>
            <div class="card">
                <h3>Mensagens Enviadas</h3>
                <p><%= data.messagesSent %></p>
            </div>
            <div class="card">
                <h3>Status do Bot</h3>
                <p class="status-online"><%= data.botStatus %></p>
            </div>
        </div>

        <div class="status">
            <h3>Atividades Recentes</h3>
            <ul class="activities-list">
                <% data.recentActivities.forEach(function(activity) { %>
                    <li><%= activity %></li>
                <% }); %>
            </ul>
        </div>
    </div>
</body>
</html>
