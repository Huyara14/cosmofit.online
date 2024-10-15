<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Interativo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }
        .quiz-container {
            width: 100%;
            max-width: 700px;
            margin: 0 auto;
            background: #fff;
            padding: 0px;
            border-radius: 8px;
            box-sizing: border-box;
        }
        .quiz-title img {
            width: 100%;
            height: auto;
        }
        .pergunta {
            margin-bottom: 20px;
            text-align: center;
        }
        .pergunta img {
            width: 100%;
            height: auto;
            margin-bottom: 20px;
        }
        .pergunta h2 {
            font-size: 24px;
            text-align: center;
            font-weight: bold;
            margin-bottom: 20px;
        }
        .opcoes {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .opcoes button {
            width: 75%;
            padding: 15px;
            margin-bottom: 10px;
            background-color: #28a745;
            color: #fff;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            text-align: center;
            white-space: normal; /* Permite quebra de linha no texto */
            height: auto; /* Ajusta a altura automaticamente */
            min-height: 50px; /* Define uma altura mínima para botões pequenos */
            box-sizing: border-box; /* Inclui padding e border na altura total */
        }
        .opcoes button:hover {
            background-color: #208c39;
        }
        .ajuste-texto {
            height: auto;
            padding: 15px;
            white-space: normal;
        }
        .footer {
            text-align: center;
            margin-top: 20px;
            font-size: 14px;
            color: #666;
        }
        .footer a {
            color: #007BFF;
            text-decoration: none;
            margin: 0 5px;
        }
        .footer a:hover {
            text-decoration: underline;
        }
        #resultado {
            font-size: 22px;
            text-align: center;
            margin-top: 30px;
        }
        .mensagem-final {
            text-align: center;
            margin-top: 30px;
        }
        .mensagem-final h2 {
            color: red;
            font-size: 28px;
            font-weight: bold;
            margin-bottom: 20px;
        }
        .mensagem-final p {
            font-size: 18px;
            line-height: 1.5;
        }
        .mensagem-final .texto-preto {
            color: black;
        }
        .mensagem-final .texto-vermelho {
            color: red;
        }
        .mensagem-final button {
            padding: 22px 70px;
            background-color: #28a745;
            color: #fff;
            border: none;
            border-radius: 5px;
            font-size: 28px;
            cursor: pointer;
            margin-top: 20px;
            transition: transform 0.3s ease;
        }
        .mensagem-final button:hover {
            background-color: #218838;
            animation: pulsar 1s infinite;
        }
        @keyframes pulsar {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.05);
            }
            100% {
                transform: scale(1);
            }
        }
        .mensagem-final img {
            width: 100%;
            height: auto;
            margin-top: 20px;
        }
        .espaco-entre {
            margin-top: 40px;
        }
        @media (max-width: 600px) {
            .opcoes button {
                width: 90%;
                font-size: 20px;
                min-height: 60px; /* Aumenta a altura mínima dos botões em dispositivos móveis */
            }
            .pergunta h2 {
                font-size: 24px;
            }
            #resultado h2 {
                font-size: 20px;
            }
            .mensagem-final h2 {
                font-size: 24px;
            }
            .mensagem-final p {
                font-size: 16px;
            }
            .mensagem-final button {
                width: 90%;
                padding: 15px;
                font-size: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div id="quiz">
            <!-- As perguntas serão inseridas aqui via JavaScript -->
        </div>
        <div class="footer">
            <a href="#">Privacy</a> | <a href="#">Contact</a><br>
            &copy; 2024. All rights reserved.
        </div>
    </div>

    <script>
        // Função para capturar os parâmetros da URL
        function obterParametrosURL() {
            const params = {};
            const queryString = window.location.search.substring(1);
            const regex = /([^&=]+)=([^&]*)/g;
            let m;
            while (m = regex.exec(queryString)) {
                params[decodeURIComponent(m[1])] = decodeURIComponent(m[2]);
            }
            return params;
        }

        // Capturando os parâmetros utm e src
        const parametros = obterParametrosURL();
        const utm = parametros['utm'] || '';
        const src = parametros['src'] || '';

        // Dados do quiz
        const quizData = [
            {
                pergunta: "How old are you?",
                imagem: "https://scoreboosternow.com/qts99/assets/images/new/STAMINA-CDB-1.webp",
                opcoes: ["Under 25", "25-44", "45-64", "Over 65"]
            },
            {
                pergunta: "How Many Times Do You Want To Enjoy Sex And Satisfy Your Partner Every Week?",
                imagem: "https://scoreboosternow.com/qts99/assets/images/new/STAMINA-CDB-2.webp",
                opcoes: ["1-5 Times a Week", "5-10 Times a Week", "As Often As I Want"]
            },
            {
                pergunta: "Besides having it hard-on anytime, what's the most important issue you want to deal with?",
                imagem: "https://scoreboosternow.com/qts99/assets/images/new/STAMINA-CDB-3.webp",
                opcoes: ["<h3>Confidence</h3>“I want the kind of alpha energy that effortlessly draws women to me and makes me stand out everywhere I go.”", "<h3>Performance</h3>“I want to easily get in the mood as if on command and satisfy my partner with leg-shaking pleasure.”", "<h3>Shredded Body</h3>“I want the raw muscle strength I need to eventually outlift everyone at the gym.”", "<h3>Happiness</h3>“I want to enjoy happy, stable moods and have a positive outlook on my life.”"]
            },
            {
                mensagem: true,
                imagem: "https://scoreboosternow.com/qts99/assets/images/new/STAMINA-CDB-FINAL.webp"
            }
        ];

        let perguntaAtual = 0;

        const quizContainer = document.getElementById('quiz');

        carregarPergunta();

        function carregarPergunta() {
            if (perguntaAtual < quizData.length) {
                const perguntaObj = quizData[perguntaAtual];

                if (perguntaObj.mensagem) {
                    let linkFinal = 'https://getalphabites.com/vsl/?aff_id=1174';
                    const params = [];
                    if (utm) params.push(`utm=${encodeURIComponent(utm)}`);
                    if (src) params.push(`src=${encodeURIComponent(src)}`);
                    if (params.length) linkFinal += '?' + params.join('&');

                    quizContainer.innerHTML = `
                        <div class="mensagem-final">
                            <img src="${perguntaObj.imagem}" alt="Imagem Final">
                            <h2><strong>CONGRATULATIONS!</strong></h2>
                            <p>
                                <b><span class="texto-preto">Your answers reveal that your "tool" problems can be fixed up with this little known method.</span>
                                <span class="texto-vermelho">Click below now to watch a FREE presentation that reveals how to put you little boy up faster than ever... boost your confidence... and provide the performance of your dreams at any time.</span></b>
                            </p>
                            <button onclick="window.location.href='${linkFinal}'">Watch the free presentation</button>
                            <div class="espaco-entre">
                                <h4>Scientific References</h4>
                                <img src="https://scoreboosternow.com/qts99/assets/images/Screenshot_2023-02-22_at_17-09-26.avif" alt="Imagem 1">
                            </div>
                        </div>
                    `;
                } else {
                    quizContainer.innerHTML = `
                        <div class="pergunta">
                            <img src="${perguntaObj.imagem}" alt="Imagem da Pergunta">
                            <h2>${perguntaObj.pergunta}</h2>
                            <div class="opcoes">
                                ${perguntaObj.opcoes.map(opcao => `
                                    <button onclick="carregarPergunta()">${opcao}</button>
                                `).join('')}
                            </div>
                        </div>
                    `;
                }

                perguntaAtual++;
            }
        }
    </script>
</body>
</html>
