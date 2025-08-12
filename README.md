this is funny test of your inteligent scale, for testing a few iq charasteristics
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Test de IQ Dinámico</title>
    <!-- Simple, clean styles for readability -->
    <style>
        body {
            font-family: Arial, Helvetica, sans-serif;
            background: #f2f4f7;
            margin: 0;
            padding: 0;
            color: #333;
        }
        .container {
            max-width: 800px;
            margin: 40px auto;
            background: #fff;
            padding: 20px 30px 40px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            margin-bottom: 10px;
        }
        p {
            margin: 0 0 15px;
        }
        .hidden {
            display: none;
        }
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        .btn-primary {
            background-color: #007bff;
            color: white;
        }
        .btn-secondary {
            background-color: #6c757d;
            color: white;
        }
        .btn:hover {
            opacity: 0.9;
        }
        .slider-container {
            display: flex;
            align-items: center;
            margin: 20px 0;
        }
        .slider-container input[type=range] {
            flex: 1;
            margin: 0 10px;
        }
        .question {
            margin: 20px 0;
        }
        .options label {
            display: block;
            margin: 8px 0;
        }
        .progress-bar-wrapper {
            width: 100%;
            background: #e9ecef;
            border-radius: 8px;
            margin-bottom: 20px;
            overflow: hidden;
        }
        .progress-bar {
            height: 14px;
            background: #28a745;
            width: 0%;
            transition: width 0.3s ease;
        }
        .results {
            text-align: center;
        }
        .results h2 {
            margin-top: 0;
        }
        .classification {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container" id="start-container">
        <h1>Test de Coeficiente Intelectual (IQ)</h1>
        <p>Selecciona el número de preguntas que deseas responder (entre 10 y 50) y luego presiona <strong>Iniciar</strong>.</p>
        <div class="slider-container">
            <span>10</span>
            <input type="range" min="10" max="50" value="30" id="questionCount" oninput="updateCountLabel()" />
            <span>50</span>
        </div>
        <p>Número de preguntas seleccionadas: <span id="count-label">30</span></p>
        <button class="btn btn-primary" onclick="startTest()">Iniciar Test</button>
    </div>

    <div class="container hidden" id="quiz-container">
        <div class="progress-bar-wrapper">
            <div class="progress-bar" id="progress-bar"></div>
        </div>
        <p id="question-number">Pregunta 1</p>
        <div class="question" id="question-text">Texto de la pregunta aparecerá aquí</div>
        <div class="options" id="options-container"></div>
        <div style="margin-top:20px; display:flex; justify-content: space-between;">
            <button class="btn btn-secondary" id="stop-button" onclick="stopTest()">Terminar</button>
            <button class="btn btn-primary" id="next-button" onclick="nextQuestion()">Siguiente</button>
        </div>
    </div>

    <div class="container hidden" id="results-container">
        <div class="results">
            <h2>Resultados del Test</h2>
            <p id="result-score"></p>
            <p class="classification" id="result-classification"></p>
            <button class="btn btn-primary" onclick="restartTest()">Volver a empezar</button>
        </div>
    </div>

    <script>
        // Banco de preguntas: cada objeto contiene la pregunta, opciones y respuesta correcta (índice)
        const questionBank = [
            {
                pregunta: "Completa la serie: 2, 4, 8, 16, ?",
                opciones: ["32", "30", "24", "36"],
                correcta: 0
            },
            {
                pregunta: "¿Cuál palabra es la intrusa? manzana, naranja, pera, zanahoria",
                opciones: ["zanahoria", "manzana", "pera", "naranja"],
                correcta: 0
            },
            {
                pregunta: "Si todos los gatos son animales y algunos animales son negros, ¿se deduce que algunos gatos son negros?",
                opciones: ["Sí", "No", "No se puede determinar", "Ninguna de las anteriores"],
                correcta: 2
            },
            {
                pregunta: "¿Cuál número falta? 5, 10, 20, __, 80",
                opciones: ["25", "30", "35", "40"],
                correcta: 3
            },
            {
                pregunta: "Si x + 3 = 7, ¿cuánto es x?",
                opciones: ["4", "5", "3", "1"],
                correcta: 0
            },
            {
                pregunta: "Si un reloj marca las 3:00, ¿qué ángulo hay entre las agujas?",
                opciones: ["90°", "45°", "180°", "30°"],
                correcta: 0
            },
            {
                pregunta: "Cuchillo es a cortar como lápiz es a ______.",
                opciones: ["escribir", "comer", "beber", "dormir"],
                correcta: 0
            },
            {
                pregunta: "¿Qué letra sigue en la secuencia? A, C, F, J, O, ?",
                opciones: ["T", "S", "U", "R"],
                correcta: 0
            },
            {
                pregunta: "¿Cuál figura no pertenece al grupo? Triángulo, Círculo, Cuadrado, Pirámide",
                opciones: ["Triángulo", "Círculo", "Cuadrado", "Pirámide"],
                correcta: 3
            },
            {
                pregunta: "Agua es a sed como comida es a ______.",
                opciones: ["hambre", "bebida", "sueño", "cansancio"],
                correcta: 0
            },
            {
                pregunta: "Si 3x = 12, entonces x es:",
                opciones: ["3", "4", "5", "6"],
                correcta: 1
            },
            {
                pregunta: "Encuentra la próxima fracción en la serie: 1/2, 2/3, 3/4, ?",
                opciones: ["4/5", "4/6", "5/6", "5/7"],
                correcta: 0
            },
            {
                pregunta: "Si CAMARÓN se relaciona con MARISCO, entonces POLLO se relaciona con:",
                opciones: ["ave", "pescado", "carne", "mamífero"],
                correcta: 2
            },
            {
                pregunta: "¿Qué número sigue? 7, 14, 28, 56, ?",
                opciones: ["70", "112", "84", "72"],
                correcta: 2
            },
            {
                pregunta: "El contrario de ‘descender’ es:",
                opciones: ["bajar", "subir", "saltarse", "caer"],
                correcta: 1
            },
            {
                pregunta: "¿Cuál número no pertenece a la serie? 16, 25, 36, 49, 50",
                opciones: ["25", "36", "49", "50"],
                correcta: 3
            },
            {
                pregunta: "ANA es a CANOA como OTO es a:",
                opciones: ["OTOÑO", "MOTO", "OTOÑO", "OTOÑO"],
                correcta: 1
            },
            {
                pregunta: "Si hoy es martes, ¿qué día será dentro de 15 días?",
                opciones: ["jueves", "viernes", "miércoles", "martes"],
                correcta: 0
            },
            {
                pregunta: "¿Cuál palabra no pertenece al grupo? azul, rojo, pastel, verde",
                opciones: ["pastel", "azul", "rojo", "verde"],
                correcta: 0
            },
            {
                pregunta: "Si 5 + 5 + 5 = 5510, ¿cómo se completa?",
                opciones: ["5+5+5=510", "5+5+5=555", "5+5+5=1515", "5+5+5=5550"],
                correcta: 0
            },
            {
                pregunta: "¿Cuál número sigue? 1, 1, 2, 3, 5, 8, ?",
                opciones: ["13", "10", "11", "14"],
                correcta: 0
            },
            {
                pregunta: "Un tren viaja a 60 km/h. ¿Cuántos kilómetros recorrerá en 2,5 horas?",
                opciones: ["120", "130", "140", "150"],
                correcta: 0
            },
            {
                pregunta: "Encuentra la relación: CARPINTERO : MADERA :: SASTRE : ______",
                opciones: ["tela", "clavo", "hierro", "papel"],
                correcta: 0
            },
            {
                pregunta: "Si 8 cajas pesan 64 kg, ¿cuánto pesan 2 cajas?",
                opciones: ["8", "16", "32", "24"],
                correcta: 1
            },
            {
                pregunta: "Sigue la secuencia: 2, 3, 5, 8, 13, ?",
                opciones: ["18", "20", "21", "22"],
                correcta: 2
            },
            {
                pregunta: "¿Qué palabra se forma reorganizando las letras: 'ATCIOPN'?",
                opciones: ["CAPTION", "ACTION", "CAPITON", "CATION"],
                correcta: 1
            },
            {
                pregunta: "Si 12 es 3×4, entonces 64 es:",
                opciones: ["8×8", "4×16", "6×10", "5×13"],
                correcta: 1
            },
            {
                pregunta: "Si todos los plátanos son amarillos y todos los limones son amarillos, entonces los plátanos son limones?",
                opciones: ["Sí", "No", "Algunos", "Imposible saber"],
                correcta: 1
            },
            {
                pregunta: "En un grupo de 15 personas, ¿cuántos apretones de manos se realizan si cada persona saluda a todas las demás una vez?",
                opciones: ["210", "105", "110", "120"],
                correcta: 1
            },
            {
                pregunta: "¿Qué número falta? 81, 27, 9, 3, ?",
                opciones: ["1", "2", "0", "9"],
                correcta: 0
            },
            {
                pregunta: "¿Cuál palabra no pertenece? lápiz, libro, papel, cama",
                opciones: ["lápiz", "libro", "papel", "cama"],
                correcta: 3
            },
            {
                pregunta: "Elegir la opción que continúa la secuencia: JFMAMJJASON?",
                opciones: ["D", "J", "N", "S"],
                correcta: 0
            },
            {
                pregunta: "Completa la serie: 3, 9, 27, __, 243",
                opciones: ["54", "81", "72", "108"],
                correcta: 1
            },
            {
                pregunta: "Si 2 gatos atrapan 2 ratones en 2 minutos, ¿cuánto tiempo tardarán 4 gatos en atrapar 4 ratones?",
                opciones: ["2", "4", "3", "1"],
                correcta: 0
            },
            {
                pregunta: "Cuenta las letras en la palabra 'EXTRAORDINARIO'",
                opciones: ["13", "14", "15", "16"],
                correcta: 2
            },
            {
                pregunta: "Si ABC=6 y BCD=12, entonces CDE= ? (Considere A=1, B=2, C=3…)",
                opciones: ["18", "20", "24", "30"],
                correcta: 2
            },
            {
                pregunta: "Selecciona la figura que completa la serie: ▲, ▼, ▲, ▼, ?",
                opciones: ["▲", "▼", "○", "□"],
                correcta: 0
            },
            {
                pregunta: "Un pastel se corta en 8 partes iguales. ¿Cuántos cortes mínimos se necesitan?",
                opciones: ["3", "4", "5", "2"],
                correcta: 0
            },
            {
                pregunta: "¿Cuál es la sombra de la palabra 'REFLEJO'?",
                opciones: ["OJELFER", "OJELFRE", "FRELEJO", "OJELFER"],
                correcta: 0
            },
            {
                pregunta: "En una carrera, adelantas al segundo corredor. ¿En qué posición quedas?",
                opciones: ["primero", "segundo", "tercero", "cuarto"],
                correcta: 1
            },
            {
                pregunta: "Si giras 90° a la derecha desde el norte, ¿hacia dónde miras?",
                opciones: ["este", "oeste", "sur", "norte"],
                correcta: 0
            },
            {
                pregunta: "Si 1 = 5, 2 = 25, 3 = 125, entonces 4 = ?",
                opciones: ["625", "325", "6250", "3125"],
                correcta: 3
            },
            {
                pregunta: "¿Cuál es la mediana de los números 3, 5, 7, 9, 11?",
                opciones: ["7", "5", "9", "8"],
                correcta: 0
            },
            {
                pregunta: "Identifica la próxima forma: círculo, triángulo, cuadrado, pentágono, ?",
                opciones: ["hexágono", "heptágono", "octágono", "dodecágono"],
                correcta: 0
            },
            {
                pregunta: "¿Qué número tiene tantas letras como su valor?",
                opciones: ["uno", "tres", "dos", "cuatro"],
                correcta: 1
            },
            {
                pregunta: "Selecciona la opción que completa la analogía: Ojo : Ver :: Oreja : ?",
                opciones: ["Oír", "Comer", "Saborear", "Oler"],
                correcta: 0
            },
            {
                pregunta: "¿Cuál número sigue? 6, 11, 21, 41, ?",
                opciones: ["81", "80", "61", "82"],
                correcta: 0
            },
            {
                pregunta: "Si 0 es a 1 como 1 es a 3 y 2 es a 5, ¿3 es a?",
                opciones: ["7", "8", "6", "9"],
                correcta: 0
            },
            {
                pregunta: "Convierte 45° a radianes (aprox.)",
                opciones: ["0.785", "1.570", "0.523", "0.392"],
                correcta: 0
            },
            {
                pregunta: "Si duplicas un número y le sumas 10 obtienes 26. ¿Cuál es el número?",
                opciones: ["6", "8", "9", "10"],
                correcta: 1
            },
            {
                pregunta: "Encuentra el número que falta: 24, 20, 16, 12, ?",
                opciones: ["8", "10", "6", "14"],
                correcta: 0
            },
            {
                pregunta: "¿Cuál letra falta? B, E, H, K, ?",
                opciones: ["N", "O", "M", "L"],
                correcta: 0
            },
            {
                pregunta: "Completa la analogía: Noche : Luna :: Día : ?",
                opciones: ["Sol", "Estrella", "Nube", "Oscuridad"],
                correcta: 0
            },
            {
                pregunta: "¿Qué número falta en la serie? 4, 9, 16, 25, ?",
                opciones: ["30", "35", "36", "32"],
                correcta: 2
            },
            {
                pregunta: "De los siguientes, elige el intruso: perro, gato, vaca, león",
                opciones: ["perro", "gato", "vaca", "león"],
                correcta: 2
            },
            {
                pregunta: "¿Qué número completa la serie? 2, 5, 10, 17, ?",
                opciones: ["26", "24", "25", "23"],
                correcta: 2
            },
            {
                pregunta: "Un automóvil recorre 150 km en 3 horas. ¿A qué velocidad media viaja?",
                opciones: ["50 km/h", "55 km/h", "60 km/h", "65 km/h"],
                correcta: 0
            },
            {
                pregunta: "Si al duplicar un número le restas 4 obtienes 10. ¿Cuál es el número?",
                opciones: ["8", "7", "6", "5"],
                correcta: 1
            },
            {
                pregunta: "¿Qué número es la mitad de un cuarto de 100?",
                opciones: ["12.5", "25", "50", "75"],
                correcta: 0
            }
        ];

        let selectedQuestions = [];
        let currentIndex = 0;
        let score = 0;
        let totalQuestions = 30;

        function updateCountLabel() {
            const count = document.getElementById('questionCount').value;
            document.getElementById('count-label').textContent = count;
        }

        // Fisher-Yates shuffle
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        function startTest() {
            // Reset variables
            score = 0;
            currentIndex = 0;
            totalQuestions = parseInt(document.getElementById('questionCount').value);
            // Shuffle and select the desired number of questions
            const shuffled = shuffleArray([...questionBank]);
            selectedQuestions = shuffled.slice(0, totalQuestions);
            // Hide start, show quiz
            document.getElementById('start-container').classList.add('hidden');
            document.getElementById('quiz-container').classList.remove('hidden');
            document.getElementById('results-container').classList.add('hidden');
            // Initialize first question
            showQuestion();
        }

        function showQuestion() {
            const q = selectedQuestions[currentIndex];
            document.getElementById('question-number').textContent = `Pregunta ${currentIndex + 1} de ${totalQuestions}`;
            document.getElementById('question-text').textContent = q.pregunta;
            const optionsDiv = document.getElementById('options-container');
            optionsDiv.innerHTML = '';
            q.opciones.forEach((opcion, idx) => {
                const optionId = `option${currentIndex}_${idx}`;
                const label = document.createElement('label');
                const radio = document.createElement('input');
                radio.type = 'radio';
                radio.name = 'question_' + currentIndex;
                radio.value = idx;
                radio.id = optionId;
                label.appendChild(radio);
                label.appendChild(document.createTextNode(' ' + opcion));
                optionsDiv.appendChild(label);
            });
            // Update progress bar
            const progress = ((currentIndex) / totalQuestions) * 100;
            document.getElementById('progress-bar').style.width = progress + '%';
            // Change next button text on last question
            if (currentIndex === totalQuestions - 1) {
                document.getElementById('next-button').textContent = 'Finalizar';
            } else {
                document.getElementById('next-button').textContent = 'Siguiente';
            }
        }

        function nextQuestion() {
            // Check answer
            const options = document.getElementsByName('question_' + currentIndex);
            let selected = -1;
            options.forEach((opt) => {
                if (opt.checked) selected = parseInt(opt.value);
            });
            if (selected === selectedQuestions[currentIndex].correcta) {
                score++;
            }
            currentIndex++;
            if (currentIndex < totalQuestions) {
                showQuestion();
            } else {
                finishTest();
            }
        }

        function finishTest() {
            // Fill progress bar
            document.getElementById('progress-bar').style.width = '100%';
            // Hide quiz, show results
            document.getElementById('quiz-container').classList.add('hidden');
            document.getElementById('results-container').classList.remove('hidden');
            // Compute percentage
            const percentage = Math.round((score / totalQuestions) * 100);
            document.getElementById('result-score').textContent = `Aciertos: ${score} de ${totalQuestions} (${percentage}%)`;
            // Determine classification
            let classification = '';
            if (percentage < 40) {
                classification = 'Nivel: Bajo';
            } else if (percentage < 60) {
                classification = 'Nivel: Promedio';
            } else if (percentage < 80) {
                classification = 'Nivel: Superior';
            } else if (percentage < 100) {
                classification = 'Nivel: Excelente';
            } else {
                classification = 'Nivel: Perfecto';
            }
            document.getElementById('result-classification').textContent = classification;
        }

        function stopTest() {
            // Finishes the test early; compute results on answered questions so far
            finishTest();
        }

        function restartTest() {
            // Show start container
            document.getElementById('start-container').classList.remove('hidden');
            document.getElementById('quiz-container').classList.add('hidden');
            document.getElementById('results-container').classList.add('hidden');
            // Reset slider label
            updateCountLabel();
        }

        // Initialize label on load
        document.addEventListener('DOMContentLoaded', () => {
            updateCountLabel();
        });
    </script>
</body>
</html>
