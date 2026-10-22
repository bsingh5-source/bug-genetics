<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bug Genetics Recombination</title>
    <style>
        :root {
            --mother-bg: #d1a3b9;
            --father-bg: #b3d1ff;
            --mother-border: #8da399;
            --father-border: #0000ff;
        }

        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: #f0f2f5;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        .btn-container { margin-bottom: 30px; }

        button {
            padding: 15px 30px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            border: none;
            border-radius: 50px;
            transition: all 0.2s;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        .btn-sort { background-color: #28a745; color: white; margin-right: 10px; }
        .btn-sort:hover { background-color: #218838; transform: translateY(-2px); }
        .btn-reset { background-color: #dc3545; color: white; }
        .btn-reset:hover { background-color: #c82333; }

        .parents-grid {
            display: flex;
            gap: 40px;
            margin-bottom: 40px;
        }

        .parent-box {
            background: white;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            text-align: center;
            min-width: 200px;
        }

        .header-m { background-color: var(--mother-bg); padding: 10px; border-radius: 8px; margin-bottom: 15px; }
        .header-f { background-color: var(--father-bg); padding: 10px; border-radius: 8px; margin-bottom: 15px; }

        .pool {
            display: flex;
            justify-content: center;
            gap: 20px;
            min-height: 250px;
        }

        .legend-box {
            background: #ffffff;
            border: 2px solid #d1d5db;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            margin-bottom: 30px;
            padding: 16px 20px;
            width: min(1000px, 90vw);
        }

        .legend-box h3 {
            margin: 0 0 12px;
            text-align: center;
        }

        .legend-layout {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .trait-table {
            flex: 1;
            border-collapse: collapse;
            background: #f8fafc;
            border: 1px solid #dbe3ee;
            border-radius: 10px;
            overflow: hidden;
        }

        .trait-table th,
        .trait-table td {
            border: 1px solid #dbe3ee;
            padding: 10px 12px;
            text-align: left;
            vertical-align: middle;
        }

        .trait-table th {
            background: #e2e8f0;
            font-weight: bold;
            text-align: center;
        }

        .gene-label {
            display: inline-block;
            background: #e2e8f0;
            border-radius: 6px;
            padding: 4px 8px;
            font-weight: bold;
            text-align: center;
        }

        .ladybug-panel {
            width: 220px;
            min-width: 220px;
            height: 220px;
            background: linear-gradient(135deg, #fffaf2, #f3f4f6);
            border: 2px solid #dbe3ee;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: inset 0 0 0 1px rgba(148, 163, 184, 0.15);
        }

        .ladybug-svg {
            width: 170px;
            height: 170px;
        }

        .legend-note {
            margin-top: 12px;
            text-align: center;
            font-size: 0.95rem;
            color: #334155;
        }

        /* Chromosome Styling */
        .chromosome {
            width: 60px;
            border: 4px solid;
            border-radius: 10px;
            background: white;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            transition: all 0.5s ease-in-out;
        }

        .m-type { border-color: var(--mother-border); }
        .f-type { border-color: var(--father-border); }

        .gene {
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-bottom: 1px solid #eee;
            font-weight: bold;
            font-size: 1.2rem;
        }
        .gene:last-child { border-bottom: none; }

        /* Recombination Area */
        .recombination-zone {
            background: #e2e8f0;
            border: 4px dashed #94a3b8;
            border-radius: 20px;
            padding: 30px;
            width: 500px;
            text-align: center;
        }

        .slots {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-top: 20px;
            min-height: 260px;
        }

        .slot {
            width: 70px;
            border: 3px solid #cbd5e1;
            border-radius: 12px;
            background: #f8fafc;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    </style>
</head>
<body>

    <h1>Bug Chromosome Lab</h1>

    <div class="legend-box">
        <h3>Traits & Genes</h3>
        <div class="legend-layout">
            <table class="trait-table">
                <thead>
                    <tr>
                        <th>Trait</th>
                        <th>Dominant Gene</th>
                        <th>Recessive Gene</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Body segments</td>
                        <td><span class="gene-label">B = 3 segments</span></td>
                        <td><span class="gene-label">b = 2 segments</span></td>
                    </tr>
                    <tr>
                        <td>Pairs of wings</td>
                        <td><span class="gene-label">T = 2 pairs</span></td>
                        <td><span class="gene-label">t = 1 pair</span></td>
                    </tr>
                    <tr>
                        <td>Antenna style</td>
                        <td><span class="gene-label">R = straight</span></td>
                        <td><span class="gene-label">r = curly</span></td>
                    </tr>
                    <tr>
                        <td>Pairs of legs</td>
                        <td><span class="gene-label">L = 3 pairs</span></td>
                        <td><span class="gene-label">l = 2 pairs</span></td>
                    </tr>
                    <tr>
                        <td>Color of eyes</td>
                        <td><span class="gene-label">E = Black</span></td>
                        <td><span class="gene-label">e = Green</span></td>
                    </tr>
                    <tr>
                        <td>Body Color</td>
                        <td><span class="gene-label">G = Gray</span></td>
                        <td><span class="gene-label">g = Purple</span></td>
                    </tr>
                </tbody>
            </table>

            <div class="ladybug-panel" aria-label="Ladybug illustration">
                <svg class="ladybug-svg" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Ladybug illustration">
                    <ellipse cx="100" cy="105" rx="72" ry="60" fill="#e53935" stroke="#b71c1c" stroke-width="5" />
                    <ellipse cx="58" cy="74" rx="18" ry="16" fill="#111827" />
                    <ellipse cx="142" cy="74" rx="18" ry="16" fill="#111827" />
                    <ellipse cx="58" cy="136" rx="18" ry="16" fill="#111827" />
                    <ellipse cx="142" cy="136" rx="18" ry="16" fill="#111827" />
                    <circle cx="100" cy="105" r="25" fill="#f7f3d0" />
                    <line x1="100" y1="52" x2="100" y2="18" stroke="#2d3748" stroke-width="4" stroke-linecap="round" />
                    <line x1="70" y1="68" x2="38" y2="50" stroke="#2d3748" stroke-width="4" stroke-linecap="round" />
                    <line x1="130" y1="68" x2="162" y2="50" stroke="#2d3748" stroke-width="4" stroke-linecap="round" />
                    <line x1="70" y1="140" x2="42" y2="162" stroke="#2d3748" stroke-width="4" stroke-linecap="round" />
                    <line x1="130" y1="140" x2="158" y2="162" stroke="#2d3748" stroke-width="4" stroke-linecap="round" />
                    <circle cx="82" cy="103" r="6" fill="#111827" />
                    <circle cx="118" cy="103" r="6" fill="#111827" />
                    <path d="M90 118 Q100 128 110 118" fill="none" stroke="#111827" stroke-width="4" stroke-linecap="round" />
                    <circle cx="100" cy="100" r="4" fill="#111827" />
                    <path d="M100 70 L100 95" stroke="#111827" stroke-width="4" stroke-linecap="round" />
                    <path d="M85 75 L72 60" stroke="#111827" stroke-width="4" stroke-linecap="round" />
                    <path d="M115 75 L128 60" stroke="#111827" stroke-width="4" stroke-linecap="round" />
                </svg>
            </div>
        </div>

        <p class="legend-note"><strong>Tip:</strong> Capital letters are dominant traits and lowercase letters are recessive traits.</p>
    </div>

    <div class="btn-container">
        <button class="btn-sort" onclick="recombine()">Sort & Recombine Genes</button>
        <button class="btn-reset" onclick="resetAll()">Reset</button>
    </div>

    <div class="parents-grid">
        <div class="parent-box">
            <div class="header-m">Mother Bug Genes</div>
            <div class="pool" id="mother-pool">
                <div class="chromosome m-type" id="m1">
                    <div class="gene">B</div><div class="gene">T</div><div class="gene">R</div><div class="gene">L</div><div class="gene">E</div><div class="gene">X</div>
                </div>
                <div class="chromosome m-type" id="m2">
                    <div class="gene">b</div><div class="gene">t</div><div class="gene">r</div><div class="gene">l</div><div class="gene">e</div><div class="gene">X</div>
                </div>
            </div>
        </div>

        <div class="parent-box">
            <div class="header-f">Father Bug Genes</div>
            <div class="pool" id="father-pool">
                <div class="chromosome f-type" id="f1">
                    <div class="gene">b</div><div class="gene">t</div><div class="gene">r</div><div class="gene">L</div><div class="gene">e</div><div class="gene">X</div>
                </div>
                <div class="chromosome f-type" id="f2">
                    <div class="gene">b</div><div class="gene">t</div><div class="gene">r</div><div class="gene">l</div><div class="gene">e</div><div class="gene">Y</div>
                </div>
            </div>
        </div>
    </div>

    <div class="recombination-zone">
        <h3>Recombination Area</h3>
        <p>Offspring Chromosome Pair</p>
        <div class="slots">
            <div class="slot" id="slot-m"></div>
            <div class="slot" id="slot-f"></div>
        </div>
    </div>

    <script>
        function resetAll() {
            // Move chromosomes back to their original pools
            document.getElementById('mother-pool').appendChild(document.getElementById('m1'));
            document.getElementById('mother-pool').appendChild(document.getElementById('m2'));
            document.getElementById('father-pool').appendChild(document.getElementById('f1'));
            document.getElementById('father-pool').appendChild(document.getElementById('f2'));
        }

        function recombine() {
            // Step 1: Always reset first so we choose from all 4 options
            resetAll();

            // Step 2: Randomly pick Mother's chromosome
            const motherOptions = ["m1", "m2"];
            const randomM = motherOptions[Math.floor(Math.random() * motherOptions.length)];
            const chosenM = document.getElementById(randomM);

            // Step 3: Randomly pick Father's chromosome
            const fatherOptions = ["f1", "f2"];
            const randomF = fatherOptions[Math.floor(Math.random() * fatherOptions.length)];
            const chosenF = document.getElementById(randomF);

            // Step 4: Move them to the recombination area
            document.getElementById('slot-m').appendChild(chosenM);
            document.getElementById('slot-f').appendChild(chosenF);
        }
    </script>
</body>
</html>
