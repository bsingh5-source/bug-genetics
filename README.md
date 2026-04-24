<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bug Gene Recombinator</title>
    <style>
        :root {
            --mother-color: #d1a3b9;
            --father-color: #b3d1ff;
        }

        body {
            font-family: 'Segoe UI', sans-serif;
            background-color: #f4f7f6;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        .controls { margin-bottom: 20px; }
        
        button {
            padding: 10px 20px;
            font-size: 1rem;
            cursor: pointer;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            margin: 5px;
        }

        button:hover { background-color: #45a049; }

        .container {
            display: flex;
            gap: 50px;
            margin-bottom: 40px;
            justify-content: center;
        }

        .parent-section {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            text-align: center;
            min-width: 200px;
        }

        .chromosome-pool {
            display: flex;
            gap: 15px;
            min-height: 260px;
            justify-content: center;
        }

        .chromosome {
            width: 60px;
            border: 3px solid;
            border-radius: 8px;
            cursor: grab;
            display: flex;
            flex-direction: column;
            background: white;
        }

        .gene-box {
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-bottom: 1px solid #ddd;
            font-weight: bold;
        }

        .mother-chrom { border-color: #8da399; }
        .mother-header { background: var(--mother-color); padding: 5px; margin-bottom: 10px; }

        .father-chrom { border-color: blue; }
        .father-header { background: var(--father-color); padding: 5px; margin-bottom: 10px; }

        #recombination-area {
            width: 100%;
            max-width: 600px;
            background: #eee;
            border: 3px dashed #999;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
        }

        .drop-slots {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-top: 20px;
            min-height: 270px;
        }

        .slot {
            width: 70px;
            border: 2px solid #ccc;
            border-radius: 10px;
            background: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    </style>
</head>
<body>

    <h1>Bug Gene Recombinator</h1>

    <div class="controls">
        <button onclick="sortChromosomes()">Shuffle/Sort Chromosomes</button>
        <button style="background-color: #f44336;" onclick="reset()">Reset All</button>
    </div>

    <div class="container">
        <div class="parent-section">
            <div class="mother-header">Mother Bug Genes</div>
            <div class="chromosome-pool" id="mother-pool">
                <div class="chromosome mother-chrom" draggable="true" id="m1">
                    <div class="gene-box">B</div><div class="gene-box">T</div><div class="gene-box">R</div><div class="gene-box">L</div><div class="gene-box">E</div><div class="gene-box">X</div>
                </div>
                <div class="chromosome mother-chrom" draggable="true" id="m2">
                    <div class="gene-box">b</div><div class="gene-box">t</div><div class="gene-box">r</div><div class="gene-box">l</div><div class="gene-box">e</div><div class="gene-box">X</div>
                </div>
            </div>
        </div>

        <div class="parent-section">
            <div class="father-header">Father Bug Genes</div>
            <div class="chromosome-pool" id="father-pool">
                <div class="chromosome father-chrom" draggable="true" id="f1">
                    <div class="gene-box">b</div><div class="gene-box">t</div><div class="gene-box">r</div><div class="gene-box">L</div><div class="gene-box">e</div><div class="gene-box">X</div>
                </div>
                <div class="chromosome father-chrom" draggable="true" id="f2">
                    <div class="gene-box">b</div><div class="gene-box">t</div><div class="gene-box">r</div><div class="gene-box">l</div><div class="gene-box">e</div><div class="gene-box">Y</div>
                </div>
            </div>
        </div>
    </div>

    <div id="recombination-area">
        <h3>Offspring Recombination Zone</h3>
        <div class="drop-slots">
            <div class="slot" id="slot-mother" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
            <div class="slot" id="slot-father" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
        </div>
    </div>

    <script>
        // Randomization Logic
        function sortChromosomes() {
            shuffleContainer('mother-pool');
            shuffleContainer('father-pool');
        }

        function shuffleContainer(containerId) {
            const container = document.getElementById(containerId);
            const items = Array.from(container.children);
            
            // Fisher-Yates shuffle algorithm
            for (let i = items.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                container.appendChild(items[j]); // Moving the element shuffles the order
            }
        }

        // Drag and Drop Logic
        function allowDrop(ev) { ev.preventDefault(); }

        function drag(ev) { ev.dataTransfer.setData("text", ev.target.id); }

        document.querySelectorAll('.chromosome').forEach(chrom => {
            chrom.addEventListener('dragstart', drag);
        });

        function drop(ev) {
            ev.preventDefault();
            const data = ev.dataTransfer.getData("text");
            const draggedElement = document.getElementById(data);
            const dropTarget = ev.target.closest('.slot');

            if (dropTarget && dropTarget.children.length === 0) {
                if (dropTarget.id === 'slot-mother' && !draggedElement.classList.contains('mother-chrom')) return;
                if (dropTarget.id === 'slot-father' && !draggedElement.classList.contains('father-chrom')) return;

                dropTarget.appendChild(draggedElement);
            }
        }

        function reset() {
            location.reload(); // Simplest way to reset the whole state
        }
    </script>
</body>
</html>
