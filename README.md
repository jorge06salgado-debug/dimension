<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>Quiz interactivo: Genes, Psiconeuroinmunología, Epigenética y Neuropsicología</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root{
      --bg: #0e0f13;
      --card: #171923;
      --accent: #6ee7b7;   /* verde menta */
      --accent2: #60a5fa;  /* azul suave */
      --warn: #f59e0b;     /* ámbar */
      --danger: #ef4444;   /* rojo */
      --text: #e5e7eb;     /* gris claro */
      --muted: #9ca3af;    /* gris medio */
      --ok: #22c55e;       /* verde ok */
      --shadow: 0 10px 30px rgba(0,0,0,.35);
      --radius: 16px;
    }
    * { box-sizing: border-box; }
    body{
      margin:0;
      font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, "Helvetica Neue", Arial, "Noto Sans", "Apple Color Emoji", "Segoe UI Emoji";
      background:
        radial-gradient(1200px 800px at 20% -10%, #1f2937 0%, transparent 60%),
        radial-gradient(900px 600px at 120% 0%, #111827 0%, transparent 60%),
        var(--bg);
      color: var(--text);
    }
    header{
      padding: 32px 20px 10px;
      text-align: center;
    }
    .title{
      font-weight: 800;
      font-size: clamp(26px, 3.5vw, 42px);
      letter-spacing: .5px;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      margin: 0 0 8px;
    }
    .subtitle{
      margin: 0 auto;
      color: var(--muted);
      max-width: 900px;
      font-size: clamp(14px, 1.8vw, 17px);
    }
    .container{
      max-width: 1100px;
      margin: 24px auto 40px;
      padding: 0 16px;
      display: grid;
      grid-template-columns: 1fr;
      gap: 22px;
    }
    .toolbar{
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
      justify-content: center;
      margin-bottom: 8px;
    }
    .btn{
      border: none;
      padding: 12px 16px;
      border-radius: 12px;
      background: linear-gradient(180deg, #22273a 0%, #1b2033 100%);
      color: var(--text);
      cursor: pointer;
      transition: transform .08s ease, box-shadow .2s ease, background .2s ease;
      box-shadow: var(--shadow);
      font-weight: 600;
      font-size: 14px;
    }
    .btn:hover{ transform: translateY(-1px); }
    .btn:active{ transform: translateY(1px) scale(.98); }
    .btn.primary{
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      color: #0a0a0a;
    }
    .btn.warning{
      background: linear-gradient(90deg, #fbbf24, #f59e0b);
      color: #111827;
    }
    .btn.success{
      background: linear-gradient(90deg, #34d399, #22c55e);
      color: #0b1020;
    }
    .card{
      background: linear-gradient(180deg, rgba(255,255,255,.05) 0%, rgba(255,255,255,.02) 100%), var(--card);
      border: 1px solid rgba(255,255,255,.08);
      box-shadow: var(--shadow);
      border-radius: var(--radius);
      overflow: hidden;
    }
    .cardHeader{
      padding: 14px 18px;
      display: flex;
      align-items: center;
      gap: 10px;
      border-bottom: 1px solid rgba(255,255,255,.08);
      background:
        linear-gradient(180deg, rgba(96,165,250,.12) 0%, rgba(110,231,183,.08) 100%);
    }
    .chip{
      padding: 6px 10px;
      border-radius: 999px;
      font-size: 12px;
      font-weight: 700;
      color: #0b1020;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
    }
    .qindex{
      font-weight: 800;
      color: var(--muted);
      font-size: 13px;
    }
    .question{
      padding: 16px 18px 8px;
      font-weight: 700;
      font-size: clamp(15px, 2vw, 18px);
    }
    .options{
      padding: 6px 18px 18px;
      display: grid;
      gap: 10px;
    }
    .opt{
      display: flex;
      align-items: flex-start;
      gap: 10px;
      padding: 12px;
      border-radius: 12px;
      background: rgba(255,255,255,.03);
      border: 1px solid rgba(255,255,255,.08);
      cursor: pointer;
      transition: background .2s ease, border-color .2s ease, transform .05s ease;
    }
    .opt:hover{
      background: rgba(96,165,250,.10);
      border-color: rgba(96,165,250,.35);
      transform: translateY(-1px);
    }
    .opt input{ margin-top: 3px; }
    .opt.correct{
      background: rgba(34,197,94,.12);
      border-color: rgba(34,197,94,.6);
    }
    .opt.incorrect{
      background: rgba(239,68,68,.12);
      border-color: rgba(239,68,68,.6);
    }
    .feedback{
      padding: 4px 18px 16px;
      font-size: 13px;
      color: var(--muted);
    }
    .scorebar{
      width: 100%;
      background: rgba(255,255,255,.08);
      height: 10px;
      border-radius: 999px;
      overflow: hidden;
      margin: 8px 0 0;
    }
    .scorefill{
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      transition: width .3s ease;
    }
    .footer{
      text-align: center;
      color: var(--muted);
      padding: 18px;
      font-size: 13px;
    }
    .grid{
      display: grid;
      gap: 18px;
    }
    @media(min-width: 860px){
      .grid{
        grid-template-columns: 1fr 1fr;
      }
    }

    /* Modal (segunda ventana de lectura) */
    .modalBackdrop{
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,.55);
      display: none;
      align-items: center;
      justify-content: center;
      padding: 16px;
      z-index: 99;
    }
    .modal{
      max-width: 900px;
      width: 100%;
      background: linear-gradient(180deg, rgba(255,255,255,.05) 0%, rgba(255,255,255,.02) 100%), var(--card);
      border: 1px solid rgba(255,255,255,.1);
      border-radius: 16px;
      box-shadow: var(--shadow);
      overflow: hidden;
    }
    .modalHeader{
      padding: 14px 16px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid rgba(255,255,255,.08);
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      color: #0b1020;
      font-weight: 800;
    }
    .modalBody{
      padding: 16px;
      max-height: 70vh;
      overflow: auto;
      color: var(--text);
    }
    .modalBody h3{
      margin: 12px 0 6px;
      font-size: 18px;
      color: var(--accent2);
    }
    .modalBody p, .modalBody li{
      color: var(--text);
      line-height: 1.55;
    }
    .modalFooter{
      padding: 12px 16px;
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      border-top: 1px solid rgba(255,255,255,.08);
    }
    .kbd{
      padding: 2px 6px;
      border-radius: 6px;
      background: rgba(255,255,255,.12);
      font-size: 12px;
    }
  </style>
</head>
<body>

  <header>
    <h1 class="title">Quiz interactivo de Psicología Biológica y Neuropsicología</h1>
    <p class="subtitle">
      Responde las preguntas. Al seleccionar, verás en colores si tu opción es correcta o incorrecta. 
      Puedes abrir la segunda ventana para leer el material resumido y volver al quiz.
    </p>
  </header>

  <div class="container">
    <div class="toolbar">
      <button class="btn primary" id="openInfo">Abrir ventana de lectura</button>
      <button class="btn success" id="checkAll">Calificar todo</button>
      <button class="btn warning" id="resetAll">Reiniciar</button>
    </div>

    <div class="card" style="padding:16px 18px;">
      <div style="display:flex; align-items:center; gap:12px; flex-wrap:wrap;">
        <div class="chip">Tu progreso</div>
        <div style="flex:1;">
          <div class="scorebar"><div class="scorefill" id="scoreFill"></div></div>
        </div>
        <div id="scoreText" style="font-weight:700; color:var(--muted);">0/20 correctas</div>
      </div>
    </div>

    <div id="quiz" class="grid"><!-- Questions will be injected here --></div>

    <div class="footer">
      Sugerencia: Puedes estudiar primero en la ventana de lectura y luego intentar el quiz. 
      Consejo: usa la tecla <span class="kbd">R</span> para reiniciar rápido.
    </div>
  </div>

  <!-- Segunda ventana: Modal de lectura -->
  <div class="modalBackdrop" id="modalBackdrop" role="dialog" aria-modal="true" aria-labelledby="infoTitle">
    <div class="modal">
      <div class="modalHeader">
        <div id="infoTitle">Lectura guiada: conceptos clave</div>
        <button class="btn" id="closeInfo">Cerrar</button>
      </div>
      <div class="modalBody">
        <h3>Genes, evolución y conducta</h3>
        <ul>
          <li><strong>Genes:</strong> Unidades de información hereditaria que codifican proteínas y regulan funciones biológicas.</li>
          <li><strong>Evolución y conducta:</strong> La selección natural mantiene rasgos conductuales con valor adaptativo.</li>
          <li><strong>Naturaleza vs. crianza:</strong> La conducta emerge de la interacción entre genética y ambiente; estudios en gemelos lo evidencian.</li>
          <li><strong>Herencia poligénica:</strong> Rasgos complejos (p. ej., inteligencia) están influidos por múltiples genes.</li>
        </ul>

        <h3>Psiconeuroinmunología: condicionamiento de la respuesta inmune</h3>
        <ul>
          <li><strong>Interdisciplina:</strong> Relación entre conducta, sistemas nervioso, endocrino e inmune.</li>
          <li><strong>Condicionamiento clásico:</strong> EC (sabor/olor/estímulo visual-auditivo) apareado con EI (fármaco inmunomodulador o estrés) modifica la respuesta inmune.</li>
          <li><strong>Bases:</strong> Eje HPA, sistema límbico y simpático; inervación de órganos inmunes.</li>
          <li><strong>Aplicaciones:</strong> Inmunosupresión condicionada (trasplantes, autoinmunidad) e inmunofacilitación (NK e interferones, progresión tumoral).</li>
        </ul>

        <h3>Epigenética: un nuevo lenguaje</h3>
        <ul>
          <li><strong>Definición:</strong> Cambios heredables en la expresión génica sin alterar la secuencia del ADN.</li>
          <li><strong>Mecanismos:</strong> Metilación del ADN, modificaciones de histonas (acetilación, fosforilación, etc.), microARNs.</li>
          <li><strong>Ambiente:</strong> Nutrición, estrés, adicciones, ejercicio y aprendizaje pueden marcar la cromatina.</li>
          <li><strong>Transgeneracional:</strong> Algunas marcas epigenéticas alcanzan células germinales y se heredan.</li>
        </ul>

        <h3>Evaluación neuropsicológica: introducción</h3>
        <ul>
          <li><strong>Propósito:</strong> Relacionar alteraciones cognitivas con funciones/lesiones cerebrales.</li>
          <li><strong>Áreas:</strong> Memoria, atención, lenguaje, funciones ejecutivas, percepción, habilidades motoras.</li>
          <li><strong>Métodos:</strong> Entrevista, pruebas estandarizadas (Stroop, WCST, WAIS), observación.</li>
          <li><strong>Utilidad:</strong> Diagnóstico diferencial, rehabilitación, seguimiento clínico.</li>
        </ul>

        <h3>Guía general de evaluación neuropsicológica</h3>
        <ul>
          <li><strong>Pasos:</strong> Definir objetivos; seleccionar pruebas; considerar variables culturales-educativas-emocionales; interpretar en contexto; redactar informe claro.</li>
          <li><strong>Ética:</strong> Confidencialidad, respeto, comunicación comprensible para pacientes y equipos clínicos.</li>
          <li><strong>Clave:</strong> Es ciencia y arte: rigor técnico y sensibilidad clínica.</li>
        </ul>
      </div>
      <div class="modalFooter">
        <button class="btn success" id="goBackToQuiz">Volver al quiz</button>
      </div>
    </div>
  </div>

  <script>
    // Banco de preguntas (20) con opciones y índice de la correcta
    const questions = [
      {
        topic: "Genes y evolución",
        q: "¿Qué son los genes en términos funcionales?",
        options: [
          "Secuencias aleatorias sin función definida",
          "Unidades de información que codifican proteínas y regulan funciones",
          "Solo regiones no codificantes del genoma",
          "Estructuras celulares que degradan proteínas"
        ],
        correct: 1,
        feedback: "Los genes contienen información para sintetizar proteínas y regular procesos biológicos."
      },
      {
        topic: "Genes y evolución",
        q: "La teoría evolutiva explica que ciertos rasgos conductuales persisten porque:",
        options: [
          "Son culturalmente aceptados",
          "Tienen valor adaptativo y favorecen la supervivencia",
          "Aumentan el tamaño cerebral",
          "Reducen la variabilidad genética"
        ],
        correct: 1,
        feedback: "La selección natural mantiene rasgos con ventaja adaptativa."
      },
      {
        topic: "Genes y evolución",
        q: "Los estudios con gemelos permiten observar:",
        options: [
          "La influencia exclusiva del ambiente",
          "La interacción entre genética y ambiente en la conducta",
          "La ausencia de diferencias individuales",
          "La imposibilidad de separar factores biológicos"
        ],
        correct: 1,
        feedback: "Comparar gemelos monocigóticos y dicigóticos evidencia naturaleza y crianza."
      },
      {
        topic: "Genes y evolución",
        q: "La herencia poligénica implica:",
        options: [
          "Un rasgo determinado por un único gen",
          "Rasgos complejos influidos por múltiples genes",
          "La no heredabilidad de la inteligencia",
          "Que el ambiente no influye"
        ],
        correct: 1,
        feedback: "Características como inteligencia o personalidad son poligénicas."
      },
      {
        topic: "Psiconeuroinmunología",
        q: "La psiconeuroinmunología estudia la relación entre:",
        options: [
          "Conducta y sistemas nervioso, endocrino e inmune",
          "Solo el sistema inmune",
          "Cultura y aprendizaje",
          "Genética y sociología"
        ],
        correct: 0,
        feedback: "Integra mente, cerebro, hormonas y defensas inmunes."
      },
      {
        topic: "Psiconeuroinmunología",
        q: "El paradigma usado para modificar respuestas inmunes es:",
        options: [
          "Refuerzo negativo",
          "Condicionamiento clásico (Pavlov)",
          "Moldeamiento operante",
          "Exposición sistemática"
        ],
        correct: 1,
        feedback: "EC apareado con EI inmunomodulador o estrés altera la respuesta inmune."
      },
      {
        topic: "Psiconeuroinmunología",
        q: "¿Qué eje hormonal modula fuertemente la inmunidad?",
        options: [
          "Eje tiroideo",
          "Eje gonadal",
          "Eje hipotálamo-hipófisis-suprarrenales (HPA)",
          "Eje renina-angiotensina"
        ],
        correct: 2,
        feedback: "El eje HPA y los glucocorticoides influyen en la función inmune."
      },
      {
        topic: "Psiconeuroinmunología",
        q: "La inmunosupresión condicionada puede ser útil para:",
        options: [
          "Aumentar respuesta a vacunas",
          "Disminuir rechazo a trasplantes",
          "Incrementar inflamación articular",
          "Generar alergias"
        ],
        correct: 1,
        feedback: "Ha mostrado reducir rechazo y apoyar tratamiento de autoinmunidad."
      },
      {
        topic: "Psiconeuroinmunología",
        q: "La inmunofacilitación condicionada destaca por:",
        options: [
          "Reducir actividad de células NK",
          "Aumentar progresión tumoral",
          "Incrementar actividad de células NK e interferones",
          "Suprimir memoria inmunológica"
        ],
        correct: 2,
        feedback: "Puede elevar NK y afectar favorablemente progresión tumoral en modelos animales."
      },
      {
        topic: "Epigenética",
        q: "La epigenética se define como:",
        options: [
          "Cambios en secuencia de ADN heredables",
          "Cambios en expresión génica sin alterar la secuencia de ADN",
          "Mutaciones puntuales por ambiente",
          "Solo regulación por proteínas ribosomales"
        ],
        correct: 1,
        feedback: "Marcas y estructura de cromatina regulan qué genes se expresan y cuándo."
      },
      {
        topic: "Epigenética",
        q: "La metilación del ADN tiende a:",
        options: [
          "Aumentar transcripción de todos los genes",
          "Silenciar genes o generar inestabilidad si es aberrante",
          "Eliminar histonas del nucleosoma",
          "Romper la doble hélice"
        ],
        correct: 1,
        feedback: "Metilación en promotores suele reducir expresión; patrones anómalos se asocian a patología."
      },
      {
        topic: "Epigenética",
        q: "¿Cuál NO es una modificación epigenética de histonas?",
        options: [
          "Acetilación",
          "Fosforilación",
          "Ubiquitinación",
          "Replicación"
        ],
        correct: 3,
        feedback: "Replicación no es una marca; es proceso de copia del ADN."
      },
      {
        topic: "Epigenética",
        q: "Los microARNs regulan:",
        options: [
          "La secuencia del ADN directamente",
          "La expresión post-transcripcional y dialogan con la maquinaria epigenética",
          "Solo la traducción de histonas",
          "El número de cromosomas"
        ],
        correct: 1,
        feedback: "Modulan RNAs mensajeros y también influyen/son influidos por marcas epigenéticas."
      },
      {
        topic: "Epigenética",
        q: "Un ejemplo de factor ambiental que modifica el epigenoma es:",
        options: [
          "Número de galaxias",
          "Nutrición, estrés, adicciones, ejercicio, aprendizaje",
          "Edad del universo",
          "Latitud geográfica únicamente"
        ],
        correct: 1,
        feedback: "Diversos factores cotidianos pueden marcar cromatina y alterar expresión génica."
      },
      {
        topic: "Epigenética",
        q: "Los efectos epigenéticos transgeneracionales implican que:",
        options: [
          "No hay herencia de marcas epigenéticas",
          "Solo las mutaciones se heredan",
          "Algunas marcas alcanzan células germinales y se heredan",
          "Todo cambio epigenético es temporal"
        ],
        correct: 2,
        feedback: "Exposiciones tempranas pueden dejar marcas que afectan a descendientes."
      },
      {
        topic: "Neuropsicología",
        q: "La evaluación neuropsicológica busca:",
        options: [
          "Medir nivel de estrés únicamente",
          "Relacionar alteraciones cognitivas con funciones/lesiones cerebrales",
          "Sustituir a la neuroimagen",
          "Diagnosticar solo trastornos del estado de ánimo"
        ],
        correct: 1,
        feedback: "Integra pruebas y clínica para entender el vínculo cerebro-conducta."
      },
      {
        topic: "Neuropsicología",
        q: "Una batería típica evalúa:",
        options: [
          "Memoria, atención, lenguaje, funciones ejecutivas, percepción, habilidades motoras",
          "Solo velocidad de reacción",
          "Únicamente lenguaje",
          "Exclusivamente habilidades visomotoras"
        ],
        correct: 0,
        feedback: "Cobertura multidimensional para perfil cognitivo amplio."
      },
      {
        topic: "Neuropsicología",
        q: "Utilidad clínica clave de la evaluación neuropsicológica:",
        options: [
          "Diagnóstico diferencial y planificación de rehabilitación",
          "Eliminar la necesidad de terapia",
          "Medir masa encefálica",
          "Detectar enfermedades infecciosas"
        ],
        correct: 0,
        feedback: "Ayuda a distinguir cuadros (p. ej., demencia vs. depresión) y guiar intervención."
      },
      {
        topic: "Guía de evaluación",
        q: "Primer paso al planear una evaluación neuropsicológica:",
        options: [
          "Aplicar todas las pruebas disponibles",
          "Definir objetivos de la evaluación",
          "Ignorar variables culturales",
          "Redactar el informe antes de evaluar"
        ],
        correct: 1,
        feedback: "Los objetivos guían selección de instrumentos y profundidad."
      },
      {
        topic: "Guía de evaluación",
        q: "Principio ético fundamental durante la evaluación:",
        options: [
          "Divulgar resultados públicamente",
          "Confidencialidad y comunicación clara y respetuosa",
          "Presionar respuestas rápidas",
          "Priorizar puntuación sobre contexto"
        ],
        correct: 1,
        feedback: "La ética sostiene la práctica clínica responsable y humana."
      }
    ];

    // Render del quiz
    const quizEl = document.getElementById('quiz');
    const scoreFill = document.getElementById('scoreFill');
    const scoreText = document.getElementById('scoreText');

    function renderQuiz(){
      quizEl.innerHTML = '';
      questions.forEach((item, i) => {
        const card = document.createElement('div');
        card.className = 'card';
        card.dataset.index = i;

        const header = document.createElement('div');
        header.className = 'cardHeader';
        header.innerHTML = `
          <div class="chip">${item.topic}</div>
          <div class="qindex">Pregunta ${i+1} de ${questions.length}</div>
        `;

        const q = document.createElement('div');
        q.className = 'question';
        q.textContent = item.q;

        const opts = document.createElement('div');
        opts.className = 'options';

        item.options.forEach((optText, idx) => {
          const opt = document.createElement('label');
          opt.className = 'opt';
          opt.innerHTML = `
            <input type="radio" name="q${i}" value="${idx}" />
            <div>${optText}</div>
          `;
          // Al hacer clic, marcar correcta/incorrecta
          opt.addEventListener('click', (e) => {
            const inputs = opts.querySelectorAll('input');
            inputs.forEach(inp => {
              const parent = inp.parentElement;
              parent.classList.remove('correct', 'incorrect');
            });
            const selected = opt.querySelector('input');
            selected.checked = true;
            const isCorrect = idx === item.correct;
            opt.classList.add(isCorrect ? 'correct' : 'incorrect');
            // también marca la correcta si falló (para feedback inmediato)
            if(!isCorrect){
              const correctLabel = opts.children[item.correct];
              correctLabel.classList.add('correct');
            }
            updateScore();
            setFeedback(card, isCorrect, item.feedback);
          });

          opts.appendChild(opt);
        });

        const feedback = document.createElement('div');
        feedback.className = 'feedback';
        feedback.textContent = 'Selecciona una opción para ver retroalimentación.';

        card.appendChild(header);
        card.appendChild(q);
        card.appendChild(opts);
        card.appendChild(feedback);
        quizEl.appendChild(card);
      });
      updateScore();
    }

    function setFeedback(card, isCorrect, text){
      const fb = card.querySelector('.feedback');
      fb.innerHTML = isCorrect
        ? `Correcto. ${text}`
        : `Incorrecto. ${text}`;
    }

    function updateScore(){
      let correctCount = 0;
      questions.forEach((item, i) => {
        const opts = quizEl.querySelectorAll('.card')[i].querySelector('.options').children;
        const selected = Array.from(opts).find(opt => opt.querySelector('input').checked);
        if(selected){
          const val = parseInt(selected.querySelector('input').value, 10);
          if(val === item.correct) correctCount++;
        }
      });
      const total = questions.length;
      const pct = Math.round((correctCount / total) * 100);
      scoreFill.style.width = pct + '%';
      scoreText.textContent = `${correctCount}/${total} correctas`;
    }

    function checkAll(){
      // Marca explícitamente correctas/incorrectas según selección actual
      const cards = quizEl.querySelectorAll('.card');
      cards.forEach((card, i) => {
        const opts = card.querySelector('.options').children;
        const selected = Array.from(opts).find(opt => opt.querySelector('input').checked);
        Array.from(opts).forEach(o => o.classList.remove('correct','incorrect'));
        if(selected){
          const val = parseInt(selected.querySelector('input').value, 10);
          if(val === questions[i].correct){
            selected.classList.add('correct');
            setFeedback(card, true, questions[i].feedback);
          } else {
            selected.classList.add('incorrect');
            const correctLabel = opts[questions[i].correct];
            correctLabel.classList.add('correct');
            setFeedback(card, false, questions[i].feedback);
          }
        } else {
          // si no hay selección, solo resalta la correcta como guía
          const correctLabel = opts[questions[i].correct];
          correctLabel.classList.add('correct');
          setFeedback(card, true, "La opción correcta está resaltada en verde.");
        }
      });
      updateScore();
    }

    function resetAll(){
      renderQuiz();
    }

    // Modal (segunda ventana)
    const backdrop = document.getElementById('modalBackdrop');
    const openInfoBtn = document.getElementById('openInfo');
    const closeInfoBtn = document.getElementById('closeInfo');
    const goBackBtn = document.getElementById('goBackToQuiz');

    openInfoBtn.addEventListener('click', () => {
      backdrop.style.display = 'flex';
    });
    closeInfoBtn.addEventListener('click', () => {
      backdrop.style.display = 'none';
    });
    goBackBtn.addEventListener('click', () => {
      backdrop.style.display = 'none';
    });

    // Toolbar
    document.getElementById('checkAll').addEventListener('click', checkAll);
    document.getElementById('resetAll').addEventListener('click', resetAll);

    // Tecla R para reiniciar
    document.addEventListener('keydown', (e) => {
      if(e.key.toLowerCase() === 'r'){
        resetAll();
      }
      // ESC cierra modal
      if(e.key === 'Escape'){
        backdrop.style.display = 'none';
      }
    });

    // Inicializar
    renderQuiz();
  </script>
</body>
</html>
