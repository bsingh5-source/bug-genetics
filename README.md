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
            padding: 12px 24px;
            font-size: 1rem;
            cursor: pointer;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 8px;
            margin: 5px;
            font-weight: bold;
            transition: 0.2s;
        }

        button:hover { background-color: #45a049; transform: scale(1.05); }
        .btn-reset { background-color: #f44336; }
        .btn-reset:hover { background-color: #d32f2f; }

        .container {
            display: flex;
            gap: 40px;
            margin-bottom: 30px;
            justify-content: center;
        }

        .parent-section {
            background: white;
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            text-align: center;
            min-width: 180px;
        }

        .chromosome-pool {
            display: flex;
            gap: 15px;
            min-height: 250px;
            justify-content: center;
            padding: 10px;
            border: 2px inset #eee;
            border-radius: 8px;
        }

        .chromosome {
            width: 55px;
            border: 3px solid;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            background: white;
            transition: all 0.3s ease;
        }

        .gene-box {
            height: 35px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-bottom: 1px solid #ddd;
            font-weight: bold;
        }

        .gene-box:last-child { border-bottom: none; }

        .mother-chrom { border-color: #8da399; }
        .mother-header { background: var(--mother-color); padding: 8px; margin-bottom: 10px; border-radius: 5px; font-weight: bold;}

        .father-chrom { border-color: blue; }
        .father-header { background: var(--father-color); padding: 8px; margin-bottom: 10px; border-radius: 5px; font-weight: bold;}

        #recombination-area {
            width: 100%;
            max-width: 500px;
            background: #e9ecef;
            border: 3px dashed #6c757d;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
        }

        .drop-slots {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 15px;
            min-height: 260px;
        }

        .slot {
            width: 65px;
            border: 2px solid #adb5bd;
            border-radius: 10px;
            background: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .slot::before {
            content: "Empty";
            color: #dee2e6;
            position: absolute;
            font-size: 0.8rem;
        }
    </style>
</head>
<body>

    <h1>Bug Genetics Lab</h1>

    <div class="controls">
        <button onclick="autoRecombine()">Random Gene Sort & Move</button>
        <button class="btn-reset" onclick="resetPools()">Reset Lab</button>
    </div>

    <div class="container">
