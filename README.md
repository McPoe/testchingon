<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Test de Chingonas</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f8d7da;
            flex-direction: column;
            text-align: center;
        }
        #quiz-container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 4px 8px rgba(0,0,0,0.2);
            max-width: 500px;
            font-size: 1.2em;
        }
        button {
            padding: 10px;
            margin: 10px;
            border: none;
            background-color: #ff4081;
            color: white;
            cursor: pointer;
            border-radius: 5px;
            font-size: 1em;
        }
        button:hover {
            background-color: #e91e63;
        }
        .chingonas-button {
            background-color: #6200ea;
            color: white;
            font-weight: bold;
            padding: 12px;
            border-radius: 5px;
            font-size: 1em;
        }
        .chingonas-button:hover {
            background-color: #3700b3;
        }
        #progress {
            margin-top: 10px;
            font-weight: bold;
        }
        .cta-text {
            color: #e91e63;
            font-weight: bold;
            font-size: 1.2em;
        }
        hr {
            border: none;
            height: 2px;
            background-color: #ff4081;
            margin: 15px 0;
        }
        .chingonas-image {
            width: 100%;
            max-width: 300px;
            margin-top: 15px;
        }
         .confetti {
            position: fixed;
            width: 10px;
            height: 10px;
            background-color: rgba(255, 0, 150, 0.7);
            opacity: 0.7;
            top: -10px;
            animation: fall 3s linear infinite;
        }
        @keyframes fall {
            0% { transform: translateY(0) rotate(0deg); }
            100% { transform: translateY(100vh) rotate(720deg); }
        }
    </style>
</head>
<body>
    <div id="quiz-container">
        <h2 id="question"></h2>
        <div id="answers"></div>
        <p id="progress"></p>
    </div>
    <script>
        const quiz = [
            { question: "Cuando te enfrentas a un problema en tu vida o negocio, tu reacción es:", answers: ["A) Lo analizo, busco soluciones y tomo acción de inmediato.", "B) Me mantengo firme, no dejo que nada me derrumbe, pero cuido mi imagen en el proceso.", "C) Me río, le echo desmadre y sigo adelante porque nada me derrumba."] },
            { question: "Cuando alguien te critica o duda de ti, tú…", answers: ["A) Ignoro los comentarios y sigo trabajando en mis metas.", "B) Me molesta, pero me aseguro de demostrar con hechos que estoy por encima de eso.", "C) ¡Les doy un show! Me burlo de la situación y sigo siendo yo misma."] },
            { question: "¿Cómo te describes en tres palabras?", answers: ["A) Fuerte, estratégica, determinada.", "B) Disciplinada, elegante, persistente.", "C) Divertida, auténtica, sin filtros."] },
            { question: "En una reunión social, tú eres la que…", answers: ["A) Habla de negocios, crecimiento y proyectos de éxito.", "B) Llega impactando, bien vestida, con porte y seguridad.", "C) Pone el ambiente, se ríe de todo y se roba la atención sin esfuerzo."] },
            { question: "¿Qué es lo más importante en tu camino al éxito?", answers: ["A) La estrategia y la acción constante.", "B) La constancia y la disciplina para mantenerme en la cima.", "C) La autenticidad y el carisma para conectar con la gente."] },
            { question: "Cuando te miras al espejo, piensas…", answers: ["A) “Vamos por más, todavía hay mucho por hacer.”", "B) “Reina, te ves espectacular, que el mundo se prepare.”", "C) “Ay, mana, pero qué preciosa estás hoy.”"] },
            { question: "¿Si tu vida fuera una película, ¿cómo sería?", answers: ["A) Un documental de superación y éxito.", "B) Un drama de telenovela con final épico.", "C) Una comedia con momentos icónicos."] },
            { question: "¿Qué frase te define mejor?", answers: ["A) “Si quieres resultados, deja de poner excusas y ponte a trabajar.”", "B) “Nunca dejes que nadie apague tu brillo, tú naciste para brillar.”", "C) “Yo no tengo competencia, yo soy un espectáculo aparte.”"] },
            { question: "¿Cómo manejas el estrés o la presión?", answers: ["A) Me enfoco en soluciones y ejecuto un plan.", "B) Lo disimulo con elegancia, pero en privado me enfoco en recuperarme.", "C) Me río, hago un chiste de la situación y sigo adelante."] },
            { question: "¿Qué consejo le darías a otra mujer que quiere lograr sus sueños?", answers: ["A) “Deja de esperar el momento perfecto y empieza a actuar ya.”", "B) “No permitas que nada ni nadie te haga dudar de tu valor.”", "C) “Que se aviente sin miedo, con todo y el chisme.”"] },
        ];
        
        const results = [
            { title: "🔥 LA MÁS CHINGONA 🔥", description: "Tú eres una mujer con mentalidad de empresaria, enfocada en el éxito y el empoderamiento. No estás aquí para que te aplaudan por intentarlo, estás aquí para lograrlo.<br><br>No necesitas discursos motivacionales sin acción, necesitas estrategias que te hagan avanzar de verdad. Eres directa, dices las cosas como son y no te andas con rodeos, porque sabes que la verdad duele, pero las excusas duelen más cuando te estancan.<br><br>Entiendes los miedos y las dudas, claro que sí, pero no los usas como pretexto. Porque tú no viniste a este mundo a quedarte con las ganas, viniste a transformar tu vida y a demostrarle al mundo —y a ti misma— de qué estás hecha." },
            { title: "✨ LA CHINGONA BOMBÓN ✨", description: "Tú eres fuerza y sensualidad en una sola persona. Caminas con confianza, proyectas feminidad y poder sin tener que pedir permiso. Sabes lo que vales y no necesitas que nadie te lo confirme.<br><br>Te ha tocado enfrentar críticas, obstáculos y momentos difíciles, pero aquí sigues, más fuerte y espectacular que nunca. Porque si algo te define es la perseverancia: cuando el mundo te reta, tú respondes con más ganas y más brillo. La disciplina y la constancia son tu sello, porque entiendes que el éxito no es cuestión de suerte, sino de esfuerzo y preparación. <br><br>Eres elegante, pero con carácter; sabes moverte con gracia, pero también sabes defender tu lugar. Y no cualquiera puede decir eso. Tu presencia deja huella, porque tú no eres una más: eres un símbolo de poder, determinación y belleza." },
            { title: "👑 LA CHINGONA NO-TAN PERDIDA 👑", description: "Tú eres auténtica sin filtros, dices lo que piensas y te expresas como te da la gana, sin pedir permiso ni tratar de encajar en moldes que no fueron hechos para ti. Tu carisma es innato, no necesitas forzar nada porque la gente se siente atraída por tu esencia, por tu humor y por esa manera tan única de ser. <br><br>Has pasado por momentos duros, sí, pero en lugar de dejar que te definan, los has usado como trampolín para brillar más fuerte. Te conectas con la gente de una forma que pocas pueden, porque hablas como amiga, sin poses ni disfraces. Y lo mejor de todo: eres una prueba viviente de que ser tú misma es tu mayor superpoder. <br><br>Porque viniste a este mundo a marcar tendencia, a romper esquemas y a demostrar que el éxito se construye con valentía, descaro y mucho amor propio." }
        ];
        
        let currentQuestion = 0;
        let score = [0, 0, 0];
        
        function loadQuestion() {
            if (currentQuestion < quiz.length) {
                document.getElementById("question").innerText = quiz[currentQuestion].question;
                document.getElementById("answers").innerHTML = "";
                document.getElementById("progress").innerText = `Pregunta ${currentQuestion + 1} de ${quiz.length}`;
                
                quiz[currentQuestion].answers.forEach((answer, index) => {
                    const btn = document.createElement("button");
                    btn.innerText = answer;
                    btn.onclick = () => {
                        score[index]++;
                        currentQuestion++;
                        loadQuestion();
                    };
                    document.getElementById("answers").appendChild(btn);
                });
            } else {
                showResult();
            }
        }
        
        function showResult() {
            const maxScore = Math.max(...score);
            const resultIndex = score.indexOf(maxScore);
            document.getElementById("quiz-container").innerHTML = `
                <h2>${results[resultIndex].title}</h2>
                <p class="result-text">${results[resultIndex].description}</p>
                <hr>
                <p class="cta-text">¿Lista para llevar tu chingonería al siguiente nivel? No te pierdas <strong>ChingonasCon</strong>, el evento que cambiará tu vida.</p>
                <a href="https://chingonascon.com" target="_blank">
                    <img src="https://irp.cdn-website.com/15798f70/dms3rep/multi/CHINGONASCON+LANDING+CLIC+FUNNERS+BANNER+PRINCIPAL.png?dm-skip-opt=true" alt="ChingonasCon" class="chingonas-image">
                    <button class="chingonas-button">IR A CHINGONASCON</button>

                </a>
            `;
            launchConfetti();
        }
        
        function launchConfetti() {
            for (let i = 0; i < 100; i++) {
                let confetti = document.createElement("div");
                confetti.classList.add("confetti");
                confetti.style.left = Math.random() * 100 + "vw";
                confetti.style.backgroundColor = `hsl(${Math.random() * 360}, 100%, 70%)`;
                confetti.style.animationDuration = (Math.random() * 2 + 2) + "s";
                document.body.appendChild(confetti);
                setTimeout(() => { confetti.remove(); }, 3000);
            }
        }
        
        loadQuestion();
    </script>
</body>
</html>
