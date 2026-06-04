<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulateur de Divisions Maritimes</title>
    <style>
        :root {
            --primary: #1e3a8a;
            --primary-light: #eff6ff;
            --accent: #0ea5e9;
            --text: #1e293b;
            --bg: #f8fafc;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
        }

        .container {
            width: 100%;
            max-width: 800px;
            background: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
        }

        h1 {
            color: var(--primary);
            margin-top: 0;
            border-bottom: 2px solid var(--primary-light);
            padding-bottom: 10px;
        }

        .grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 30px;
        }

        @media (max-width: 600px) {
            .grid { grid-template-columns: 1fr; }
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        label {
            font-weight: 600;
            font-size: 0.95rem;
        }

        select, input[type="number"] {
            padding: 10px;
            border: 1px solid #cbd5e1;
            border-radius: 6px;
            font-size: 1rem;
            outline: none;
            transition: border-color 0.2s;
        }

        select:focus, input[type="number"]:focus {
            border-color: var(--accent);
        }

        .slider-container {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        input[type="range"] {
            flex: 1;
        }

        .result-box {
            background-color: var(--primary-light);
            border-left: 6px solid var(--primary);
            padding: 20px;
            border-radius: 0 8px 8px 0;
        }

        .result-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--primary);
            margin: 0 0 10px 0;
        }

        .result-text {
            margin: 0;
            line-height: 1.5;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Quel est le statut réglementaire de votre navire ?</h1>
    <p>Modifiez les paramètres ci-dessous pour trouver instantanément la Division de sécurité applicable.</p>
    
    <div class="grid">
        <div class="form-group">
            <label for="usage">Usage / Activité principale</label>
            <select id="usage" onchange="calculerDivision()">
                <option value="plaisance_perso">Plaisance (Usage personnel / Loisir)</option>
                <option value="plaisance_pro">Plaisance Commerciale (NUC - Skipper / Charter)</option>
                <option value="passagers" selected>Transport de Passagers (Commercial)</option>
                <option value="peche">Pêche Professionnelle</option>
                <option value="travail">Aquaculture / Navire de Travail</option>
            </select>
        </div>

        <div class="form-group">
            <label for="zone">Zone de navigation</label>
            <select id="zone" onchange="calculerDivision()">
                <option value="nationale" selected>Nationale (Eaux françaises)</option>
                <option value="internationale">Internationale (Voyages à l'étranger)</option>
            </select>
        </div>

        <div class="form-group">
            <label for="longueur">Longueur du navire : <span id="longueur-val">20</span> m</label>
            <div class="slider-container">
                <input type="range" id="longueur" min="5" max="50" value="20" oninput="updateLongueur(this.value)">
            </div>
        </div>

        <div class="form-group" id="group-passagers">
            <label for="passagers">Nombre de passagers à bord</label>
            <input type="number" id="passagers" value="25" min="0" oninput="calculerDivision()">
        </div>

        <div class="form-group">
            <label for="coque">Matériau de la coque</label>
            <select id="coque" onchange="calculerDivision()">
                <option value="acier">Acier</option>
                <option value="autre" selected>Autre (Alu, Composite, Bois, etc.)</option>
            </select>
        </div>
    </div>

    <div class="result-box">
        <div class="result-title" id="div-titre">Division --</div>
        <p class="result-text" id="div-desc">Sélectionnez les options pour analyser le navire.</p>
    </div>
</div>

<script>
    // Met à jour l'affichage de la longueur en temps réel et relance le calcul
    function updateLongueur(val) {
        document.getElementById('longueur-val').innerText = val;
        calculerDivision();
    }

    // Logique de l'arbre de décision
    function calculerDivision() {
        // Récupération des valeurs du formulaire
        const usage = document.getElementById('usage').value;
        const zone = document.getElementById('zone').value;
        const longueur = parseFloat(document.getElementById('longueur').value);
        const passagers = parseInt(document.getElementById('passagers').value) || 0;
        const coque = document.getElementById('coque').value;

        // Éléments HTML à modifier
        const titre = document.getElementById('div-titre');
        const desc = document.getElementById('div-desc');
        const groupPassagers = document.getElementById('group-passagers');

        // Afficher/Masquer le champ passager selon l'activité pour clarté
        if (usage === 'passagers') {
            groupPassagers.style.opacity = "1";
            groupPassagers.style.pointerEvents = "auto";
        } else {
            groupPassagers.style.opacity = "0.4";
            groupPassagers.style.pointerEvents = "none";
        }

        // --- ARBRE DE DÉCISION ---

        // 1. Branche Pêche
        if (usage === 'peche') {
            if (longueur < 12) {
                titre.innerText = "Division 227";
                desc.innerText = "Navires de pêche de petite taille (moins de 12 mètres). Normes simplifiées adaptées à la pêche côtière artisanale.";
            } else if (longueur >= 12 && longueur < 24) {
                titre.innerText = "Division 226";
                desc.innerText = "Navires de pêche de taille intermédiaire (12 à 24 mètres). Exigences renforcées sur la stabilité et la sécurité de l'équipage.";
            } else {
                titre.innerText = "Division 228";
                desc.innerText = "Navires de pêche de grande taille (24 mètres et plus). Réglementation lourde pour la pêche industrielle et hauturière.";
            }
        }
        
        // 2. Branche Plaisance Personnelle
        else if (usage === 'plaisance_perso') {
            if (longueur < 24) {
                titre.innerText = "Division 240";
                desc.innerText = "Règlement phare de la plaisance de loisir. Définit le matériel de sécurité embarqué requis selon l'éloignement d'un abri (Basique, Côtier, Semi-hauturier, Hauturier).";
            } else {
                titre.innerText = "Division 242 / Grande Plaisance";
                desc.innerText = "Règlement applicable aux grands yachts de loisir de plus de 24 mètres (Mega-yachts privés).";
            }
        }

        // 3. Branche Plaisance Commerciale (NUC)
        else if (usage === 'plaisance_pro') {
            if (longueur < 24) {
                titre.innerText = "Division 241 (NUC)";
                desc.innerText = "Navires d'Utilisation Commerciale de moins de 24 mètres. S'applique aux bateaux de location avec skipper professionnel, écoles de voile ou charters légers (limité à 12 passagers).";
            } else {
                titre.innerText = "Division 242 (Yacht Commercial)";
                desc.innerText = "Règlement pour les grands yachts de pavillon français exploités commercialement (plus de 24 mètres).";
            }
        }

        // 4. Branche Aquaculture / Travail
        else if (usage === 'travail') {
            if (longueur < 24) {
                titre.innerText = "Division 230 / 238";
                desc.innerText = "S'applique aux navires conchylicoles, d'aquaculture ou aux petites embarcations de servitude et travaux maritimes côtiers.";
            } else {
                titre.innerText = "Division 222 / 235";
                desc.innerText = "Navires de charge ou de services industriels (Offshore, remorquage lourd, grands navires de travaux).";
            }
        }

        // 5. Branche Navires à Passagers (Votre cas d'origine)
        else if (usage === 'passagers') {
            // Si moins de 12 passagers, ce n'est techniquement pas un "navire à passagers" réglementaire
            if (passagers <= 12) {
                titre.innerText = "Erreur de catégorie (Max 12 passagers)";
                desc.innerText = "Un navire transportant 12 passagers ou moins ne rentre pas dans la catégorie des 'Navires à passagers'. Il doit plutôt être homologué en Plaisance Commerciale / NUC (Division 241).";
                return;
            }

            // Trajets Internationaux
            if (zone === 'internationale') {
                titre.innerText = "Division 221 (Normes SOLAS)";
                desc.innerText = "Navires à passagers effectuant des trajets internationaux. Soumis aux conventions internationales de sécurité en mer (SOLAS), très exigeantes.";
            } 
            // Trajets Nationaux
            else {
                if (longueur >= 24) {
                    titre.innerText = "Division 223";
                    desc.innerText = "Grands navires à passagers en navigation nationale (Ferries, grands navires de lignes intérieures).";
                } else {
                    // Moins de 24 mètres : Le matériau de la coque est le filtre final
                    if (coque === 'acier') {
                        titre.innerText = "Division 223a";
                        desc.innerText = "Navires à passagers de moins de 24 mètres construits en acier. Navigation nationale.";
                    } else {
                        titre.innerText = "Division 223b";
                        desc.innerText = "Navires à passagers de moins de 24 mètres construits en matériaux autres que l'acier (Alu, Fibre de verre, Composite, Bois). Très stricte sur le risque incendie (notamment les batteries électriques !).";
                    }
                }
            }
        }
    }

    // Lancement du calcul au premier chargement de la page
    calculerDivision();
</script>

</body>
</html>
