    body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background-color: #f4f7f6;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px;
    }

    h1 { color: #333; }

    .container {
        display: flex;
        gap: 50px;
        margin-bottom: 40px;
        flex-wrap: wrap;
        justify-content: center;
    }

    .parent-section {
        background: white;
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        text-align: center;
    }

    .chromosome-pool {
        display: flex;
        gap: 15px;
        min-height: 200px;
    }

    /* Chromosome Styling */
    .chromosome {
        width: 60px;
        border: 3px solid;
        border-radius: 8px;
        cursor: grab;
        display: flex;
        flex-direction: column;
        background: white;
        transition: transform 0.2s;
    }

    .chromosome:active { cursor: grabbing; }

    .gene-box {
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-bottom: 1px solid #ddd;
        font-weight: bold;
        font-size: 1.2rem;
    }

    .gene-box:last-child { border-bottom: none; }

    /* Mother specific */
    .mother-chrom { border-color: #8da399; } /* Greenish border like your image */
    .mother-header { background: var(--mother-color); padding: 5px; margin-bottom: 10px; border-radius: 4px; }

    /* Father specific */
    .father-chrom { border-color: blue; }
    .father-header { background: var(--father-color); padding: 5px; margin-bottom: 10px; border-radius: 4px; }

    /* Recombination Zone */
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
        min-height: 260px;
    }

    .slot {
        width: 80px;
        border: 2px solid #ccc;
        border-radius: 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        background: #fff;
        position: relative;
    }

    .slot::after {
        content: "Drop Here";
        color: #ccc;
        font-size: 0.8rem;
        position: absolute;
    }

    .slot.occupied::after { content: ""; }
</style>
