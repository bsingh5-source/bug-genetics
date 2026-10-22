<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Bug Genetics Recombination</title>
    <style>
        :root {
            --page-bg: #f7f3ea;
            --panel-bg: #fffdf9;
            --panel-border: #d9cfbd;
            --header-green: #5f7c68;
            --button-green: #6d8f73;
            --button-green-hover: #597961;
            --button-red: #d66a60;
            --button-red-hover: #bc5247;
            --table-header: #f1e3c7;
            --table-row: #fffaf1;
            --text: #2c2a28;
            --muted-text: #5f584f;
            --mother-bg: #f3d3d8;
            --father-bg: #dfeafc;
            --mother-border: #7a8d82;
            --father-border: #3b6bb0;
            --ink: #1f2937;
            --annotation-female: #b33d3d;
            --annotation-male: #1c4b8e;
            --female-body: #e64b4b;
            --female-body-dark: #b33232;
            --male-body: #4377d9;
            --male-body-dark: #2b5da6;
            --female-dot: #f7f1d2;
            --male-dot: #f7f7f7;
            --pencil: #1d1d1d;
        }

        * { box-sizing: border-box; }

        body {
            margin: 0;
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: var(--page-bg);
            color: var(--text);
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 24px;
        }

        h1 {
            color: var(--header-green);
            font-size: 2.2rem;
            letter-spacing: 0.5px;
            margin-bottom: 18px;
        }

        .btn-container { margin-bottom: 30px; }

        button {
            padding: 15px 30px;
            font-size: 1.05rem;
            font-weight: 700;
            cursor: pointer;
            border: none;
            border-radius: 50px;
            transition: all 0.2s ease;
            box-shadow: 0 4px 10px rgba(0,0,0,0.08);
            letter-spacing: 0.2px;
        }

        .btn-sort { background-color: var(--button-green); color: white; margin-right: 10px; }
        .btn-sort:hover { background-color: var(--button-green-hover); transform: translateY(-2px); }
        .btn-reset { background-color: var(--button-red); color: white; }
        .btn-reset:hover { background-color: var(--button-red-hover); }

        .parents-grid {
            display: flex;
            gap: 40px;
            margin-bottom: 40px;
        }

        .parent-box {
            background: var(--panel-bg);
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 4px 14px rgba(0,0,0,0.06);
            border: 1px solid var(--panel-border);
            text-align: center;
            min-width: 200px;
        }

        .header-m { background-color: var(--mother-bg); padding: 10px; border-radius: 8px; margin-bottom: 15px; color: var(--ink); }
        .header-f { background-color: var(--father-bg); padding: 10px; border-radius: 8px; margin-bottom: 15px; color: var(--ink); }

        .pool {
            display: flex;
            justify-content: center;
            gap: 20px;
            min-height: 250px;
        }

        .legend-box {
            background: var(--panel-bg);
            border: 2px solid var(--panel-border);
            border-radius: 16px;
            box-shadow: 0 4px 14px rgba(0,0,0,0.05);
            margin-bottom: 30px;
            padding: 18px 20px;
            width: min(1050px, 92vw);
        }

        .legend-box h3 {
            margin: 0 0 14px;
            text-align: center;
            color: var(--header-green);
            font-size: 1.5rem;
        }

        .legend-layout {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .trait-table {
            flex: 1;
            border-collapse: collapse;
            background: var(--table-row);
            border: 1px solid #e6dcc5;
            overflow: hidden;
        }

        .trait-table th,
        .trait-table td {
            border: 1px solid #e8ddc7;
            padding: 10px 12px;
            text-align: left;
            vertical-align: middle;
            color: var(--text);
        }

        .trait-table th {
            background: var(--table-header);
            font-weight: 700;
            text-align: center;
            color: var(--ink);
        }

        .gene-label {
            display: inline-block;
            background: #f4efe4;
            border: 1px solid #e7dcc4;
            border-radius: 6px;
            padding: 5px 8px;
            font-weight: 700;
            text-align: center;
            color: var(--ink);
        }

        .ladybug-panel {
            width: 220px;
            min-width: 220px;
            height: 220px;
            background: linear-gradient(135deg, #fffaf2, #f0f3ef);
            border: 2px solid #e5dac5;
            border-radius: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: inset 0 0 0 1px rgba(95, 124, 104, 0.08);
        }

        .ladybug-svg {
            width: 170px;
            height: 170px;
        }

        .legend-note {
            margin-top: 12px;
            text-align: center;
            font-size: 0.95rem;
            color: var(--muted-text);
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
            background: #edf2f1;
            border: 3px dashed #9bb0a7;
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
            border: 3px solid #d4dfe1;
            border-radius: 12px;
            background: #f9fbfb;
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
                    <ellipse cx="100" cy="105" rx="72" ry="60" fill="#d4544d" stroke="#b63f3a" stroke-width="5" />
                    <ellipse cx="58" cy="74" rx="18" ry="16" fill="#2f2d2b" />
                    <ellipse cx="142" cy="74" rx="18" ry="16" fill="#2f2d2b" />
                    <ellipse cx="58" cy="136" rx="18" ry="16" fill="#2f2d2b" />
                    <ellipse cx="142" cy="136" rx="18" ry="16" fill="#2f2d2b" />
                    <circle cx="100" cy="105" r="25" fill="#f7f3d0" />
                    <line x1="100" y1="52" x2="100" y2="18" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
                    <line x1="70" y1="68" x2="38" y2="50" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
                    <line x1="130" y1="68" x2="162" y2="50" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
                    <line x1="70" y1="140" x2="42" y2="162" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
                    <line x1="130" y1="140" x2="158" y2="162" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
                    <circle cx="82" cy="103" r="6" fill="#1f2937" />
                    <circle cx="118" cy="103" r="6" fill="#1f2937" />
                    <path d="M90 118 Q100 128 110 118" fill="none" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
                    <circle cx="100" cy="100" r="4" fill="#1f2937" />
                    <path d="M100 70 L100 95" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
                    <path d="M85 75 L72 60" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
                    <path d="M115 75 L128 60" stroke="#1f2937" stroke-width="4" stroke-linecap="round" />
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
            document.getElementById('mother-pool').appendChild(document.getElementById('m1'));
            document.getElementById('mother-pool').appendChild(document.getElementById('m2'));
            document.getElementById('father-pool').appendChild(document.getElementById('f1'));
            document.getElementById('father-pool').appendChild(document.getElementById('f2'));
        }

        function recombine() {
            resetAll();
            const motherOptions = ["m1", "m2"];
            const randomM = motherOptions[Math.floor(Math.random() * motherOptions.length)];
            const chosenM = document.getElementById(randomM);
            const fatherOptions = ["f1", "f2"];
            const randomF = fatherOptions[Math.floor(Math.random() * fatherOptions.length)];
            const chosenF = document.getElementById(randomF);
            document.getElementById('slot-m').appendChild(chosenM);
            document.getElementById('slot-f').appendChild(chosenF);
        }
    </script>
</body>
</html>
