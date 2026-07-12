<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NAVAL_SOLVER_PRO</title>
    <style>
        :root {
            --bg-main: #050505;
            --bg-panel: #0E0E14;
            --bg-input: #13131B;
            --blue-electric: #0052FF;
            --blue-glow: #00E5FF;
            --text-main: #E5E7EB;
            --text-muted: #9CA3AF;
            --border: rgba(255, 255, 255, 0.05);
            --red-alert: #FF3333;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-main);
            overflow-x: hidden;
            padding-bottom: 60px;
        }

        header {
            background-color: var(--bg-panel);
            border-bottom: 1px solid var(--border);
            padding: 12px 16px;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 4px 20px rgba(0,0,0,0.5);
        }

        .search-container {
            position: relative;
            width: 100%;
        }

        .search-input {
            width: 100%;
            background-color: var(--bg-input);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 10px 12px 10px 36px;
            color: var(--blue-glow);
            font-size: 14px;
            outline: none;
            transition: all 0.3s ease;
        }

        .search-input:focus {
            border-color: var(--blue-electric);
            box-shadow: 0 0 10px rgba(0, 82, 255, 0.4);
        }

        .search-icon {
            position: absolute;
            left: 12px;
            top: 11px;
            font-size: 14px;
            opacity: 0.5;
        }

        .container {
            display: flex;
            flex-direction: column;
            padding: 16px;
            gap: 16px;
        }

        .module-selector-wrapper {
            width: 100%;
        }

        select.module-select {
            width: 100%;
            background-color: var(--bg-panel);
            color: var(--text-main);
            border: 1px solid var(--blue-electric);
            padding: 12px;
            border-radius: 8px;
            font-size: 14px;
            font-weight: bold;
            outline: none;
            box-shadow: 0 0 10px rgba(0, 82, 255, 0.2);
        }

        .calc-card {
            background-color: var(--bg-panel);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 16px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.4);
        }

        .card-header {
            border-bottom: 1px solid var(--border);
            padding-bottom: 8px;
            margin-bottom: 16px;
        }

        .card-title {
            font-size: 16px;
            font-weight: bold;
            color: #FFF;
        }

        .card-desc {
            font-size: 12px;
            color: var(--text-muted);
            margin-top: 4px;
        }

        .input-group {
            margin-bottom: 14px;
        }

        .input-label {
            display: block;
            font-size: 11px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            color: var(--text-muted);
            margin-bottom: 6px;
        }

        .input-wrapper {
            position: relative;
        }

        .input-field {
            width: 100%;
            background-color: var(--bg-input);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 10px 45px 10px 12px;
            color: #FFF;
            font-size: 14px;
            outline: none;
            transition: border-color 0.2s;
        }

        .input-field:focus {
            border-color: var(--blue-electric);
        }

        .input-unit {
            position: absolute;
            right: 12px;
            top: 10px;
            font-size: 12px;
            color: var(--text-muted);
            font-weight: bold;
        }

        .btn-action {
            width: 100%;
            background-color: var(--blue-electric);
            color: white;
            border: 1px solid rgba(0, 229, 255, 0.3);
            padding: 14px;
            border-radius: 8px;
            font-size: 14px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 8px;
            box-shadow: 0 0 8px rgba(0, 82, 255, 0.3);
        }

        .btn-action:active {
            box-shadow: 0 0 20px rgba(0, 229, 255, 0.6);
            transform: scale(0.98);
        }

        .results-box {
            margin-top: 20px;
            background: linear-gradient(135deg, rgba(16,16,22,1) 0%, rgba(8,8,12,1) 100%);
            border: 1px solid rgba(255,255,255,0.03);
            border-radius: 8px;
            padding: 12px;
        }

        .result-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid rgba(255,255,255,0.02);
        }

        .result-row:last-child {
            border: none;
        }

        .result-label {
            font-size: 12px;
            color: var(--text-muted);
        }

        .result-value {
            font-size: 14px;
            font-weight: bold;
            color: var(--blue-glow);
        }

        .error-message {
            color: var(--red-alert);
            font-weight: bold;
            font-size: 14px;
            text-align: center;
            padding: 10px 0;
        }

        .formula-box {
            margin-top: 12px;
            font-size: 10px;
            color: var(--text-muted);
            background-color: #050507;
            padding: 8px;
            border-radius: 4px;
            border: 1px solid var(--border);
            font-family: monospace;
        }

        @media (min-width: 768px) {
            .container {
                flex-direction: row;
                max-width: 1200px;
                margin: 0 auto;
            }
            .module-selector-wrapper {
                width: 300px;
                flex-shrink: 0;
            }
            .calc-card {
                flex-grow: 1;
            }
        }
    </style>
</head>
<body>

    <header>
        <form id="searchForm" onsubmit="handleSearch(event)">
            <div class="search-container">
                <span class="search-icon">🔍</span>
                <input type="text" id="searchInput" class="search-input" placeholder="Tapez un calcul (ex: erreur, cotes fixes, impossible...)">
            </div>
        </form>
    </header>

    <div class="container">
        <div class="module-selector-wrapper">
            <label class="input-label">Catégorie de calculs</label>
            <select id="moduleSelect" class="module-select" onchange="switchModule(this.value)">
                <option value="coque">🚢 1. Dimensions & Angles Coque</option>
                <option value="stabilite">📐 2. Stabilité & Équilibre (GM)</option>
                <option value="flottabilite">⚖ 3. Flottabilité & Charges</option>
                <option value="propulsion">⚙ 4. Moteur & Propulsion</option>
                <option value="hydrodynamique">🌊 5. Hydrodynamique & Traînée</option>
                <option value="structure">🏗 6. Échantillonnage Structure</option>
                <option value="navigation">🧭 7. Navigation & Carburant</option>
                <option value="dimensionnement">📏 8. Ratios & Pré-dimensionnement</option>
                <option value="dimensions_fixes">📐 9. Cotes Navales Fixes (Longueur Unique)</option>
            </select>
        </div>

        <main class="calc-card">
            <div class="card-header">
                <div id="cardTitle" class="card-title">Chargement...</div>
                <div id="cardDesc" class="card-desc">Description du module.</div>
            </div>

            <div id="inputsContainer"></div>

            <button class="btn-action" onclick="calculate()">📐 EXÉCUTER LE CALCUL</button>

            <div class="results-box">
                <div id="resultsContainer"></div>
                <div id="formulaBox" class="formula-box"></div>
            </div>
        </main>
    </div>

    <script>
        const CONFIG = {
            coque: {
                title: "Géométrie & Caractéristiques de Coque",
                desc: "Calcul de l'angle, du volume de carène et du tirant d'eau théorique.",
                formula: "Déplacement = Volume × 1.025 | Surface Mouillée = 2.5 × √(Déplacement × Longueur)",
                inputs: [
                    { id: "long_flot", name: "Longueur de flottaison", unit: "m", def: 45 },
                    { id: "larg_max", name: "Largeur maximale au maître-bau", unit: "m", def: 9.5 },
                    { id: "vol_coque", name: "Volume de carène immergée", unit: "m³", def: 650 },
                    { id: "ang_coque", name: "Angle de carène (Deadrise)", unit: "degrés", def: 14 }
                ],
                calc: (i) => {
                    if (i.long_flot <= 0 || i.larg_max <= 0 || i.vol_coque <= 0) return { error: "❌ Ce calcul n'est pas possible !" };
                    const depl = i.vol_coque * 1.025;
                    const sm = 2.5 * Math.sqrt(depl * i.long_flot);
                    const te = depl / (i.long_flot * i.larg_max * 0.65 * 1.025);
                    return {
                        "Déplacement global (Δ)": depl.toFixed(1) + " tonnes",
                        "Surface mouillée de la coque": sm.toFixed(1) + " m²",
                        "Tirant d'eau estimé (T)": te.toFixed(2) + " m"
                    };
                }
            },
            stabilite: {
                title: "Stabilité Hydrostatique & Équilibre",
                desc: "Hauteur métacentrique initiale et évaluation des risques de chavirement.",
                formula: "GM = KB + BM - KG",
                inputs: [
                    { id: "larg_max", name: "Largeur de flottaison", unit: "m", def: 9.5 },
                    { id: "kg", name: "Hauteur centre de gravité (KG)", unit: "m", def: 3.8 },
                    { id: "kb", name: "Hauteur centre de carène (KB)", unit: "m", def: 1.9 },
                    { id: "bm", name: "Rayon métacentrique transversal (BM)", unit: "m", def: 2.4 }
                ],
                calc: (i) => {
                    if (i.larg_max <= 0) return { error: "❌ Ce calcul n'est pas possible !" };
                    const gm = i.kb + i.bm - i.kg;
                    let alerte = "Sécurisé (Bon bras de levier)";
                    if (gm < 0.15) alerte = "CRITIQUE : Risque de chavirement !";
                    return {
                        "Hauteur Métacentrique (GM)": gm.toFixed(2) + " m",
                        "État de stabilité transversale": alerte
                    };
                }
            },
            flottabilite: {
                title: "Flottabilité & Capacités Maximales",
                desc: "Analyse des volumes de réserve et poids maximal supportable.",
                formula: "Réserve = Vol_Total - (Poids / 1.025)",
                inputs: [
                    { id: "vol_total", name: "Volume total clos du navire", unit: "m³", def: 1800 },
                    { id: "poids_total", name: "Poids lège global en charge", unit: "tonnes", def: 750 }
                ],
                calc: (i) => {
                    if (i.vol_total <= 0 || i.poids_total <= 0) return { error: "❌ Ce calcul n'est pas possible !" };
                    const res = i.vol_total - (i.poids_total / 1.025);
                    if (res < 0) return { error: "❌ Pas possible : Le bateau coule (Trop lourd) !" };
                    const charge = res * 1.025;
                    return {
                        "Réserve de flottabilité": res.toFixed(1) + " m³",
                        "Charge utile maximale admissible": charge.toFixed(1) + " tonnes"
                    };
                }
            },
            propulsion: {
                title: "Moteur & Dimensionnement Propulsion",
                desc: "Puissance requise et consommation estimée.",
                formula: "Puissance kW = (Déplacement^(2/3) × Vitesse³) / 220",
                inputs: [
                    { id: "poids_total", name: "Poids total du navire", unit: "tonnes", def: 750 },
                    { id: "vitesse_cible", name: "Vitesse maximale visée", unit: "nœuds", def: 16 },
                    { id: "nb_moteurs", name: "Nombre de moteurs installés", unit: "unités", def: 2 }
                ],
                calc: (i) => {
                    if (i.poids_total <= 0 || i.vitesse_cible <= 0) return { error: "❌ Ce calcul n'est pas possible !" };
                    const kw = (Math.pow(i.poids_total, 2/3) * Math.pow(i.vitesse_cible, 3)) / 220;
                    const ch = kw * 1.341;
                    return {
                        "Puissance requise": kw.toFixed(0) + " kW (" + ch.toFixed(0) + " ch)",
                        "Consommation approx.": (ch * 0.18 * i.nb_moteurs).toFixed(0) + " L/h"
                    };
                }
            },
            hydrodynamique: {
                title: "Hydrodynamique & Traînée",
                desc: "Analyse des forces de frottement face à l'eau.",
                formula: "Nombre de Froude Fn = Vitesse / √(g × L)",
                inputs: [
                    { id: "long_flot", name: "Longueur de flottaison", unit: "m", def: 45 },
                    { id: "poids_total", name: "Déplacement total", unit: "tonnes", def: 750 },
                    { id: "vitesse_cible", name: "Vitesse de transit", unit: "nœuds", def: 16 }
                ],
                calc: (i) => {
                    if (i.long_flot <= 0 || i.vitesse_cible <= 0) return { error: "❌ Ce calcul n'est pas possible !" };
                    const fn = (i.vitesse_cible * 0.5144) / Math.sqrt(9.81 * i.long_flot);
                    return {
                        "Nombre de Froude (Fn)": fn.toFixed(3),
                        "Effort de traînée estimé": (fn * i.poids_total * 0.15).toFixed(1) + " kN"
                    };
                }
            },
            structure: {
                title: "Structure & Matériaux",
                desc: "Calcul réglementaire de l'épaisseur de l'acier.",
                formula: "Épaisseur t = 1.2 × s × √(Pression)",
                inputs: [
                    { id: "tirant_deau", name: "Tirant d'eau cible", unit: "m", def: 2.8 },
                    { id: "esp_couple", name: "Espacement entre couples", unit: "m", def: 0.65 }
                ],
                calc: (i) => {
                    if (i.tirant_deau <= 0 || i.esp_couple <= 0) return { error: "❌ Ce calcul n'est pas possible !" };
                    const pression = (10 * i.tirant_deau) + 20;
                    const epaisseur = (1.2 * i.esp_couple * Math.sqrt(pression)) + 2;
                    return {
                        "Pression de calcul": pression.toFixed(1) + " kN/m²",
                        "Épaisseur minimale bordé": epaisseur.toFixed(1) + " mm"
                    };
                }
            },
            navigation: {
                title: "Navigation & Autonomie",
                desc: "Planification des distances d'autonomie.",
                formula: "Autonomie = Capacité / Consommation",
                inputs: [
                    { id: "capa_carb", name: "Capacité carburant", unit: "L", def: 25000 },
                    { id: "conso_horaire", name: "Consommation horaire", unit: "L/h", def: 180 },
                    { id: "vitesse_moy", name: "Vitesse moyenne", unit: "nœuds", def: 12 }
                ],
                calc: (i) => {
                    if (i.capa_carb <= 0 || i.conso_horaire <= 0) return { error: "❌ Ce calcul n'est pas possible !" };
                    const h = i.capa_carb / i.conso_horaire;
                    return {
                        "Autonomie en temps": h.toFixed(1) + " heures",
                        "Distance franchissable": (h * i.vitesse_moy).toFixed(0) + " milles"
                    };
                }
            },
            dimensionnement: {
                title: "Proportions & Dimensionnement Initial",
                desc: "Recommandations géométriques architecturales basées sur des ratios éprouvés.",
                formula: "Ratio standard L/B de stabilité optimale",
                inputs: [
                    { id: "long_souhaite", name: "Longueur hors-tout désirée", unit: "m", def: 35 }
                ],
                calc: (i) => {
                    if (i.long_souhaite <= 0) return { error: "❌ Ce calcul n'est pas possible !" };
                    if (i.long_souhaite < 2) return { error: "❌ Pas possible : Le navire est trop petit !" };
                    const b = i.long_souhaite / 5.5;
                    const creux = i.long_souhaite / 12;
                    return {
                        "Largeur recommandée optimale": b.toFixed(1) + " m",
                        "Creux sur quille théorique": creux.toFixed(1) + " m",
                        "Nombre maximum de ponts recommandés": Math.max(1, Math.floor(creux / 2.8))
                    };
                }
            },
            dimensions_fixes: {
                title: "Cotes Navales Fixes Exactes",
                desc: "Entre uniquement la longueur du bateau. Le système génère automatiquement les cotes structurelles fixes et la flottaison précise avec gestion des erreurs physiques.",
                formula: "Formules fixes : Largeur = L/5 | Hauteur Coque = L/11.5 | Sous l'eau = Hauteur × 0.60 | Au-dessus = Hauteur × 0.40",
                inputs: [
                    { id: "long_unique", name: "Longueur unique du bateau", unit: "m", def: 35 }
                ],
                calc: (i) => {
                    const l = i.long_unique;
                    
                    // Barrières de contrôle demandées en rouge si invalide
                    if (l <= 0) {
                        return { error: "❌ Ce calcul n'est pas possible !" };
                    }
                    if (l < 2) {
                        return { error: "❌ Pas possible : Le navire est trop petit !" };
                    }
                    if (l > 500) {
                        return { error: "❌ Pas possible : Hors des limites physiques navales !" };
                    }
                    
                    const largeurExacte = l / 5.0; 
                    const hauteurExacte = l / 11.5; 
                    const sousLeauExact = hauteurExacte * 0.60; 
                    const auDessusLeauExact = hauteurExacte * 0.40; 
                    
                    return {
                        "LARGEUR EXACTE DU BATEAU": largeurExacte.toFixed(2) + " m",
                        "HAUTEUR DE COQUE EXACTE (Creux)": hauteurExacte.toFixed(2) + " m",
                        "MÈTRES SOUS L'EAU (Tirant d'eau fixe)": sousLeauExact.toFixed(2) + " m",
                        "MÈTRES AU-DESSUS DE L'EAU (Franc-bord fixe)": auDessusLeauExact.toFixed(2) + " m"
                    };
                }
            }
        };

        let currentModule = "coque";

        window.onload = () => {
            switchModule("coque");
        };

        function switchModule(modId) {
            currentModule = modId;
            document.getElementById("moduleSelect").value = modId;
            
            const cfg = CONFIG[modId];
            document.getElementById("cardTitle").innerText = cfg.title;
            document.getElementById("cardDesc").innerText = cfg.desc;
            document.getElementById("formulaBox").innerText = cfg.formula;

            const container = document.getElementById("inputsContainer");
            container.innerHTML = "";

            cfg.inputs.forEach(inp => {
                container.innerHTML += `
                    <div class="input-group">
                        <label class="input-label">${inp.name}</label>
                        <div class="input-wrapper">
                            <input type="number" id="input_${inp.id}" class="input-field" value="${inp.def}" step="0.01">
                            <span class="input-unit">${inp.unit}</span>
                        </div>
                    </div>
                `;
            });
            calculate();
        }

        function calculate() {
            const cfg = CONFIG[currentModule];
            const inputsData = {};

            cfg.inputs.forEach(inp => {
                const element = document.getElementById(`input_${inp.id}`);
                inputsData[inp.id] = parseFloat(element.value) || 0;
            });

            const results = cfg.calc(inputsData);
            const resContainer = document.getElementById("resultsContainer");
            resContainer.innerHTML = "";

            if (results.error) {
                resContainer.innerHTML = `<div class="error-message">${results.error}</div>`;
                return;
            }

            Object.entries(results).forEach(([key, val]) => {
                resContainer.innerHTML += `
                    <div class="result-row">
                        <span class="result-label">${key}</span>
                        <span class="result-value">${val}</span>
                    </div>
                `;
            });
        }

        function handleSearch(event) {
            event.preventDefault();
            const text = document.getElementById("searchInput").value.toLowerCase().trim();

            if (text.includes("fixe") || text.includes("erreur") || text.includes("impossible") || text.includes("chiffre") || text.includes("petit")) {
                switchModule("dimensions_fixes");
            } else if (text.includes("dimension") || text.includes("longueur") || text.includes("largeur")) {
                switchModule("dimensionnement");
            } else if (text.includes("angle") || text.includes("coque") || text.includes("tirant")) {
                switchModule("coque");
            } else if (text.includes("stabilite") || text.includes("gm")) {
                switchModule("stabilite");
            } else if (text.includes("poids") || text.includes("flottabilité")) {
                switchModule("flottabilite");
            } else if (text.includes("moteur") || text.includes("puissance")) {
                switchModule("propulsion");
            }
            document.getElementById("searchInput").value = "";
        }
    </script>
</body>
</html>
