# Mots-fl-ch-s-Elise
Mots fléchés du matou
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mots croisés</title>
  <style>
    body {
      font-family: 'Helvetica Neue', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      background: #fafafa;
      color: #333;
      margin: 0;
      padding: 2rem;
    }

    h1 {
      font-weight: 500;
      letter-spacing: 1px;
      margin-bottom: 1rem;
    }

    .container {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 2rem;
      width: 90%;
      max-width: 900px;
    }

    canvas {
      background: #fff;
      border: 1px solid #ccc;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }

    .definitions {
      font-size: 0.95rem;
      line-height: 1.6;
    }

    .definitions strong {
      display: inline-block;
      width: 2em;
    }

    @media (max-width: 700px) {
      .container {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>
  <h1>🧩 Mots croisés</h1>
  <div class="container">
    <canvas id="grid" width="500" height="500"></canvas>
    <div class="definitions" id="clues"></div>
  </div>

  <script>
    // Les mots et définitions
    const entries = [
      {word: "FRIEDRICH", clue: "Copain de Karl"},
      {word: "FANFARE", clue: "Cuivres mobiles"},
      {word: "CONSTITUTION", clue: "L'état la respecte"},
      {word: "HAMAC", clue: "Offre un repos suspendu"},
      {word: "OVERTON", clue: "Fenêtre qui s'ouvre à droite"},
      {word: "REGALIEN", clue: "Appartient au roi"},
      {word: "SYNCLINAL", clue: "Peut être perché"},
      {word: "LA", clue: "Pour s'accorder"},
      {word: "WOKE", clue: "Trop éveillé"},
      {word: "SANTE", clue: "Demeure de président"},
      {word: "MIE", clue: "Cœur mou"},
      {word: "PLOUC", clue: "Schlag"},
      {word: "CHARLES", clue: "Aime Emma"},
      {word: "IF", clue: "Château insulaire"},
      {word: "RE", clue: "Île de riches"}
    ];

    // --- Génération de la grille ---
    const gridSize = 15;
    const cellSize = 30;
    const canvas = document.getElementById("grid");
    const ctx = canvas.getContext("2d");
    const grid = Array.from({length: gridSize}, () => Array(gridSize).fill(""));

    // Placement simplifié en diagonale pour la version minimaliste :
    entries.forEach((e, i) => {
      let r = i;
      let c = 0;
      for (let j = 0; j < e.word.length && c + j < gridSize; j++) {
        grid[r][c + j] = e.word[j];
      }
    });

    // Dessin de la grille vide
    function drawGrid() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.font = "16px monospace";
      ctx.textAlign = "center";
      ctx.textBaseline = "middle";
      for (let r = 0; r < gridSize; r++) {
        for (let c = 0; c < gridSize; c++) {
          const x = c * cellSize;
          const y = r * cellSize;
          ctx.strokeStyle = "#ccc";
          ctx.strokeRect(x, y, cellSize, cellSize);
        }
      }
    }
    drawGrid();

    // Interaction clavier
    let cursor = {r: 0, c: 0};
    const inputGrid = Array.from({length: gridSize}, () => Array(gridSize).fill(""));

    canvas.addEventListener("click", (e) => {
      const rect = canvas.getBoundingClientRect();
      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;
      cursor.c = Math.floor(x / cellSize);
      cursor.r = Math.floor(y / cellSize);
      draw();
    });

    document.addEventListener("keydown", (e) => {
      const key = e.key.toUpperCase();
      if (key.length === 1 && key >= "A" && key <= "Z") {
        inputGrid[cursor.r][cursor.c] = key;
        moveCursor();
      } else if (key === "BACKSPACE") {
        inputGrid[cursor.r][cursor.c] = "";
        moveCursor(-1);
      }
      draw();
    });

    function moveCursor(dir = 1) {
      cursor.c += dir;
      if (cursor.c >= gridSize) cursor.c = 0;
      if (cursor.c < 0) cursor.c = gridSize - 1;
    }

    function draw() {
      drawGrid();
      for (let r = 0; r < gridSize; r++) {
        for (let c = 0; c < gridSize; c++) {
          const x = c * cellSize + cellSize/2;
          const y = r * cellSize + cellSize/2;
          if (inputGrid[r][c]) {
            ctx.fillStyle = "#333";
            ctx.fillText(inputGrid[r][c], x, y);
          }
        }
      }
      ctx.strokeStyle = "#666";
      ctx.lineWidth = 2;
      ctx.strokeRect(cursor.c * cellSize, cursor.r * cellSize, cellSize, cellSize);
    }

    // Affichage des définitions
    const clues = document.getElementById("clues");
    clues.innerHTML = entries.map((e, i) => `<p><strong>${i+1}.</strong> ${e.clue}</p>`).join("");
  </script>
</body>
</html>
