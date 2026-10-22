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

        .comparison-box {
            display: grid;
            grid-template-columns: repeat(2, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 18px;
        }

        .bug-card {
            background: #fffaf0;
            border: 2px solid #e7dcc4;
            border-radius: 18px;
            min-height: 420px;
            padding: 14px 12px 18px;
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
        }

        .bug-card.female { background: linear-gradient(180deg, #fffaf5, #f7efe8); }
        .bug-card.male { background: linear-gradient(180deg, #f7fbff, #edf4ff); }

        .bug-title {
            font-size: 1.15rem;
            font-weight: 700;
            margin: 0 0 10px;
            letter-spacing: 0.04em;
        }

        .bug-card.female .bug-title { color: var(--annotation-female); }
        .bug-card.male .bug-title { color: var(--annotation-male); }

        .bug-figure {
            width: 100%;
            height: 300px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .bug-figure svg {
            width: 100%;
            max-width: 360px;
            height: auto;
        }

        .gene-chip {
            display: inline-block;
            font-weight: 700;
            font-size: 0.8rem;
            background: rgba(255,255,255,0.7);
            border-radius: 999px;
            padding: 4px 8px;
            border: 1px solid rgba(0,0,0,0.08);
        }

        .tagline {
            margin-top: 6px;
            text-align: center;
            color: var(--muted-text);
            font-size: 0.9rem;
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

        <div class="comparison-box">
            <div class="bug-card female">
                <h4 class="bug-title">Female Bug (Red)</h4>
                <div class="bug-figure">
                    <svg viewBox="0 0 360 300" xmlns="http://www.w3.org/2000/svg" aria-label="Female bug cartoon illustration">
                        <g stroke-linecap="round" stroke-linejoin="round" fill="none">
                            <path d="M92 175 C 36 128, 56 52, 108 52 C 130 52, 158 68, 180 110 C 202 68, 230 52, 252 52 C 304 52, 324 128, 268 175 C 250 196, 224 214, 180 220 C 136 214, 110 196, 92 175 Z" fill="#e95050" stroke="#ad3434" stroke-width="4"/>
                            <path d="M103 109 C 88 150, 96 170, 124 190" stroke="#ad3434" stroke-width="4"/>
                            <path d="M257 109 C 272 150, 264 170, 236 190" stroke="#ad3434" stroke-width="4"/>
                            <path d="M105 127 C 78 112, 70 77, 101 66" stroke="#ad3434" stroke-width="4"/>
                            <path d="M255 127 C 282 112, 290 77, 259 66" stroke="#ad3434" stroke-width="4"/>
                            <g fill="#f7f1d2">
                                <circle cx="102" cy="116" r="17"/>
                                <circle cx="258" cy="116" r="17"/>
                            </g>
                            <circle cx="96" cy="116" r="5" fill="#2f2d2b"/>
                            <circle cx="264" cy="116" r="5" fill="#2f2d2b"/>
                            <path d="M140 180 Q180 200 220 180" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M176 82 L176 52" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M144 98 L120 70" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M216 98 L240 70" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M170 70 L148 44" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M190 70 L212 44" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M90 176 L76 220" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M270 176 L284 220" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M120 208 L103 245" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M240 208 L257 245" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M166 206 L151 244" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M194 206 L209 244" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M178 92 L178 102" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M98 92 Q88 84 80 84" stroke="#2f2d2b" stroke-width="3"/>
                            <path d="M262 92 Q272 84 280 84" stroke="#2f2d2b" stroke-width="3"/>
                        </g>
                        <g font-family="Segoe UI, Arial, sans-serif" font-size="18" font-weight="700" fill="#b62c2c">
                            <text x="42" y="92">colored eyes</text>
                            <text x="42" y="152">3 pairs of legs</text>
                            <text x="125" y="262">XX-female</text>
                            <text x="122" y="38">round antennae</text>
                            <text x="228" y="146">Two pairs of wings</text>
                            <text x="238" y="192">3 body segments</text>
                        </g>
                        <g font-family="Segoe UI, Arial, sans-serif" font-size="16" font-weight="700" fill="#b62c2c">
                            <text x="52" y="120">(EE)</text>
                            <text x="52" y="180">(LL)</text>
                            <text x="178" y="28">(RR)</text>
                            <text x="247" y="175">(TT)</text>
                            <text x="262" y="222">(BB)</text>
                        </g>
                    </svg>
                </div>
                <div class="tagline"><span class="gene-chip">EE</span> · <span class="gene-chip">LL</span> · <span class="gene-chip">RR</span> · <span class="gene-chip">TT</span> · <span class="gene-chip">BB</span></div>
            </div>

            <div class="bug-card male">
                <h4 class="bug-title">Male Bug (Blue)</h4>
                <div class="bug-figure">
                    <svg viewBox="0 0 360 300" xmlns="http://www.w3.org/2000/svg" aria-label="Male bug cartoon illustration">
                        <g stroke-linecap="round" stroke-linejoin="round" fill="none">
                            <path d="M92 175 C 36 128, 56 52, 108 52 C 130 52, 158 68, 180 110 C 202 68, 230 52, 252 52 C 304 52, 324 128, 268 175 C 250 196, 224 214, 180 220 C 136 214, 110 196, 92 175 Z" fill="#4c83d8" stroke="#2b5da6" stroke-width="4"/>
                            <path d="M103 109 C 88 150, 96 170, 124 190" stroke="#2b5da6" stroke-width="4"/>
                            <path d="M257 109 C 272 150, 264 170, 236 190" stroke="#2b5da6" stroke-width="4"/>
                            <path d="M105 127 C 78 112, 70 77, 101 66" stroke="#2b5da6" stroke-width="4"/>
                            <path d="M255 127 C 282 112, 290 77, 259 66" stroke="#2b5da6" stroke-width="4"/>
                            <g fill="#f7f7f7">
                                <circle cx="102" cy="116" r="17"/>
                                <circle cx="258" cy="116" r="17"/>
                            </g>
                            <circle cx="96" cy="116" r="5" fill="#2f2d2b"/>
                            <circle cx="264" cy="116" r="5" fill="#2f2d2b"/>
                            <path d="M140 180 Q180 200 220 180" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M176 82 L176 52" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M144 98 L120 70" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M216 98 L240 70" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M170 70 L148 44" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M190 70 L212 44" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M90 176 L76 220" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M270 176 L284 220" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M120 208 L103 245" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M240 208 L257 245" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M166 206 L151 244" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M194 206 L209 244" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M178 92 L178 102" stroke="#2f2d2b" stroke-width="4"/>
                            <path d="M98 92 Q88 84 80 84" stroke="#2f2d2b" stroke-width="3"/>
                            <path d="M262 92 Q272 84 280 84" stroke="#2f2d2b" stroke-width="3"/>
                        </g>
                        <g font-family="Segoe UI, Arial, sans-serif" font-size="18" font-weight="700" fill="#244f9c">
                            <text x="42" y="96">clear eyes</text>
                            <text x="42" y="146">3 pairs of legs</text>
                            <text x="125" y="262">XY-male</text>
                            <text x="122" y="38">flat antennae</text>
                            <text x="225" y="140">one pair of wings</text>
                            <text x="238" y="192">2 body segments</text>
                        </g>
                        <g font-family="Segoe UI, Arial, sans-serif" font-size="16" font-weight="700" fill="#244f9c">
                            <text x="52" y="120">(ee)</text>
                            <text x="52" y="176">(ll)</text>
                            <text x="178" y="28">(rr)</text>
                            <text x="248" y="174">(tt)</text>
                            <text x="262" y="220">(bb)</text>
                        </g>
                    </svg>
                </div>
                <div class="tagline"><span class="gene-chip">ee</span> · <span class="gene-chip">ll</span> · <span class="gene-chip">rr</span> · <span class="gene-chip">tt</span> · <span class="gene-chip">bb</span></div>
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
