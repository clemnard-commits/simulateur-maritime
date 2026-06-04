<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulateur de Divisions Maritimes &amp; Électriques</title>
    <style>
        :root {
            --primary: #1e3a8a;
            --primary-light: #eff6ff;
            --accent: #0ea5e9;
            --text: #1e293b;
            --bg: #f8fafc;
            --warning-bg: #fffbeb;
            --warning-border: #d97706;
            --danger-bg: #fef2f2;
            --danger-border: #dc2626;
            --danger-text: #991b1b;
            --links-bg: #f1f5f9;
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
            max-width: 900px;
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

        .sub-grid {
            grid-column: span 2;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            background-color: #f0fdf4;
            padding: 20px;
            border-radius: 8px;
            border: 1px solid #bbf7d0;
        }

        @media (max-width: 600px) {
            .sub-grid { grid-column: span 1; grid-template-columns: 1fr; }
        }

        .sub-grid h4 {
            grid-column: span 2;
            margin: 0;
            color: #166534;
            font-size: 1.1rem;
        }

        @media (max-width: 600px) {
            .sub-grid h4 { grid-column: span 1; }
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
            padding: 25px;
            border-radius: 0 8px 8px 0;
            margin-bottom: 20px;
        }

        .result-title {
            font-size: 1.6rem;
            font-weight: 700;
            color: var(--primary);
            margin: 0 0 15px 0;
        }

        .alert-stability {
            background-color: var(--danger-bg);
            border: 1px solid var(--danger-border);
            color: var(--danger-text);
            padding: 15px;
            border-radius: 6px;
            margin-bottom: 20px;
            display: none;
            font-weight: 500;
        }

        .electric-specs {
            background-color: var(--warning-bg);
            border-left: 6px solid var(--warning-border);
            padding: 20px;
            border-radius: 0 8px 8px 0;
            margin-top: 20px;
            display: none;
        }

        .electric-specs h3 {
            margin-top: 0;
            color: #92400e;
            font-size: 1.2rem;
            border-bottom: 1px solid #fde68a;
            padding-bottom: 5px;
        }

        .links-box {
            background-color: var(--links-bg);
            padding: 20px;
            border-radius: 8px;
            margin-top: 30px;
            border: 1px solid #e2e8f0;
        }

        .links-box h4 {
            margin-top: 0;
            color: #334155;
        }

        ul {
            padding-left: 20px;
            margin: 10px 0 0 0;
        }

        li {
            margin-bottom: 12px;
            line-height: 1.4;
        }

        .badge {
            background: #d97706;
            color: white;
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 0.8rem;
            font-weight: bold;
            display: inline-block;
        }

        .badge-blue {
            background: #2563eb;
            color: white;
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 0.8rem;
            font-weight: bold;
            display: inline-block;
        }

        .badge-green {
            background: #166534;
            color: white;
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 0.8rem;
            font-weight: bold;
            display: inline-block;
        }

        a.btn-link {
            color: var(--primary);
            text-decoration: none;
            font-weight: bold;
        }

        a.btn-link:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Simulateur réglementaire Affaires Maritimes</h1>
    <p>Configurez les caractéristiques du navire pour identifier la division exacte calculée et ses prescriptions de sécurité sur-mesure.</p>
    
    <div class="grid">
        <!-- Usage -->
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

        <!-- Zone de Navigation -->
        <div class="form-group">
            <label for="zone">Zone de navigation</label>
            <select id="zone" onchange="calculerDivision()">
                <option value="nationale" selected>Nationale (Eaux françaises)</option>
                <option value="internationale">Internationale (Voyages à l'étranger)</option>
            </select>
        </div>

        <!-- Longueur -->
        <div class="form-group">
            <label for="longueur">Longueur du navire : <span id="longueur-val">15</span> m</label>
            <div class="slider-container">
                <input type="range" id="longueur" min="5" max="50" value="15" oninput="updateLongueur(this.value)">
            </div>
        </div>

        <!-- Nombre de passagers -->
        <div class="form-group" id="group-passagers">
            <label for="passagers">Nombre de passagers à bord</label>
            <input type="number" id="passagers" value="25" min="0" oninput="calculerDivision()">
        </div>

        <!-- Matériau de la coque -->
        <div class="form-group">
            <label for="coque">Matériau de la coque</label>
            <select id="coque" onchange="calculerDivision()">
                <option value="acier">Acier</option>
                <option value="autre" selected>Autre (Alu, Composite, Bois, etc.)</option>
            </select>
        </div>

        <!-- Propulsion Électrique -->
        <div class="form-group">
            <label for="propulsion">Type de motorisation</label>
            <select id="propulsion" onchange="calculerDivision()">
                <option value="thermique">Thermique Classique (Diesel / Essence)</option>
                <option value="electrique">100% Électrique ou Hybride (Batteries Lithium)</option>
            </select>
        </div>

        <!-- BLOC DE CALCUL DE CAPACITÉ -->
        <div class="sub-grid" id="bloc-calculateur-batterie" style="display: none;">
            <h4>📐 Dimensionnement Électrique Estimatif (Standard LFP Marine : 100 Wh/kg)</h4>
            <div class="form-group">
                <label for="puissance-moteur">Puissance de la motorisation (kW)</label>
                <input type="number" id="puissance-moteur" value="40" min="1" oninput="calculerDivision()">
            </div>
            <div class="form-group">
                <label for="autonomie">Autonomie cible à pleine puissance (Heures)</label>
                <input type="number" id="autonomie" value="1" min="0.5" step="0.5" oninput="calculerDivision()">
            </div>
        </div>
    </div>

    <!-- Alerte Stabilité Critique (Division 211) -->
    <div class="alert-stability" id="alerte-stabilite">
        ⚠️ Alerte Stabilité Critique !
    </div>

    <!-- Cadre de résultat principal -->
    <div class="result-box">
        <div class="result-title" id="div-titre">Division --</div>
        <p class="result-text" id="div-desc">Sélectionnez les options pour analyser le navire.</p>
    </div>

    <!-- ZONE DE PRESCRIPTIONS TECHNIQUES ADAPTÉE AUX DIVISIONS CALCULÉES -->
    <div class="electric-specs" id="bloc-electrique">
        <!-- Généré dynamiquement par JS -->
    </div>

    <!-- Liens de téléchargement officiels -->
    <div class="links-box">
        <h4>📂 Portails Officiels d'Accès à la Réglementation (Gouvernement Français)</h4>
        <p style="font-size: 0.9rem; margin-bottom: 15px; color: #475569;"><em>Note pour le client : Les URL des fichiers PDF individuels étant régulièrement modifiées par l'administration, utilisez les portails permanents ci-dessous pour télécharger les dernières versions à jour.</em></p>
        <ul>
            <li><span class="badge-blue">Légifrance</span> : <a class="btn-link" href="https://www.legifrance.gouv.fr/loda/id/JORFTEXT000000313605/" target="_blank">Arrêté du 23 novembre 1987 (Texte cadre consolidé)</a></li>
            <li><span class="badge-blue">Secrétariat à la Mer</span> : <a class="btn-link" href="https://www.mer.gouv.fr/reglementation-de-securite-des-navires-arrete-du-23-novembre-1987-modifie" target="_blank">Portail de téléchargement officiel de toutes les Divisions (223b, 219, 240, 226...)</a></li>
        </ul>
    </div>
</div>

<script>
    function updateLongueur(val) {
        document.getElementById('longueur-val').innerText = val;
        calculerDivision();
    }

    function calculerDivision() {
        const usage = document.getElementById('usage').value;
        const zone = document.getElementById('zone').value;
        const longueur = parseFloat(document.getElementById('longueur').value);
        const passagers = parseInt(document.getElementById('passagers').value) || 0;
        const coque = document.getElementById('coque').value;
        const propulsion = document.getElementById('propulsion').value;
        
        const puissanceMoteur = parseFloat(document.getElementById('puissance-moteur').value) || 0;
        const autonomie = parseFloat(document.getElementById('autonomie').value) || 0;

        const titre = document.getElementById('div-titre');
        const desc = document.getElementById('div-desc');
        const groupPassagers = document.getElementById('group-passagers');
        const blocElectrique = document.getElementById('bloc-electrique');
        const blocCalculateur = document.getElementById('bloc-calculateur-batterie');
        const alerteStabilite = document.getElementById('alerte-stabilite');

        // Visibilité des éléments
        if (usage === 'passagers') {
            groupPassagers.style.opacity = "1";
            groupPassagers.style.pointerEvents = "auto";
        } else {
            groupPassagers.style.opacity = "0.4";
            groupPassagers.style.pointerEvents = "none";
        }

        if (propulsion === 'electrique') {
            blocElectrique.style.display = "block";
            blocCalculateur.style.display = "grid";
        } else {
            blocElectrique.style.display = "none";
            blocCalculateur.style.display = "none";
            alerteStabilite.style.display = "none";
        }

        // Calculs énergétiques généraux
        const capaciteKwh = Math.round((puissanceMoteur * autonomie) * 1.2);
        const poidsKg = capaciteKwh * 10;
        const poidsTonnes = poidsKg / 1000;

        // Seuil d'alerte Division 211 (Stabilité)
        let seuilCritiqueTonnes = 2.5;
        if (longueur <= 12) seuilCritiqueTonnes = 1.0;
        else if (longueur <= 16) seuilCritiqueTonnes = 2.5;
        else if (longueur <= 20) seuilCritiqueTonnes = 4.5;
        else if (longueur <= 24) seuilCritiqueTonnes = 7.0;

        let divisionCalculee = "";
        let htmlContent = "";

        // ==========================================
        // MOTEUR LOGIQUE ET ARCHITECTURE DES PRESCRIPTIONS
        // ==========================================

        // BRANCHE 1 : TRANSPORT DE PASSAGERS
        if (usage === 'passagers') {
            if (passagers <= 12) {
                titre.innerText = "Erreur (Max 12 passagers)";
                desc.innerText = "Pour 12 passagers ou moins, l'homologation requise est la Division 241 (NUC) et non Navire à Passagers.";
                blocElectrique.style.display = "none";
                blocCalculateur.style.display = "none";
                alerteStabilite.style.display = "none";
                return;
            }

            if (zone === 'internationale') {
                divisionCalculee = "DIV_221";
                titre.innerText = "Division 221 (SOLAS)";
                desc.innerText = "Navires à passagers effectuant des voyages internationaux.";
            } else if (longueur >= 24) {
                divisionCalculee = "DIV_223_GRAND";
                titre.innerText = "Division 223";
                desc.innerText = "Grands navires à passagers en navigation nationale (longueur ≥ 24 mètres).";
            } else if (coque === 'acier') {
                divisionCalculee = "DIV_223A";
                titre.innerText = "Division 223a";
                desc.innerText = "Navires à passagers nationaux de moins de 24 mètres construits en acier.";
            } else {
                divisionCalculee = "DIV_223B";
                titre.innerText = "Division 223b, 219-6 & 322";
                desc.innerText = "Navires à passagers nationaux de moins de 24 mètres en matériaux autres que l'acier (Alu/Composite).";
            }
        }

        // BRANCHE 2 : PÊCHE PROFESSIONNELLE
        else if (usage === 'peche') {
            if (longueur < 12) {
                divisionCalculee = "DIV_227";
                titre.innerText = "Division 227";
                desc.innerText = "Navires de pêche de longueur inférieure à 12 mètres (petite pêche côtière).";
            } else if (longueur >= 12 && longueur < 24) {
                divisionCalculee = "DIV_226";
                titre.innerText = "Division 226";
                desc.innerText = "Navires de pêche de longueur comprise entre 12 et 24 mètres.";
            } else {
                divisionCalculee = "DIV_228";
                titre.innerText = "Division 228";
                desc.innerText = "Grands navires de pêche industrielle (longueur ≥ 24 mètres).";
            }
        }

        // BRANCHE 3 : PLAISANCE PERSONNELLE
        else if (usage === 'plaisance_perso') {
            if (longueur < 24) {
                divisionCalculee = "DIV_240";
                titre.innerText = "Division 240";
                desc.innerText = "Navires de plaisance à usage personnel de longueur inférieure à 24 mètres.";
            } else {
                divisionCalculee = "DIV_242_PRIVE";
                titre.innerText = "Division 242 (Privé)";
                desc.innerText = "Grands yachts de plaisance privés d'une longueur supérieure ou égale à 24 mètres.";
            }
        }

        // BRANCHE 4 : PLAISANCE COMMERCIALE (NUC)
        else if (usage === 'plaisance_pro') {
            if (longueur < 24) {
                divisionCalculee = "DIV_241";
                titre.innerText = "Division 241 (NUC)";
                desc.innerText = "Navires de plaisance à utilisation commerciale de moins de 24 mètres (Charter / Skipper).";
            } else {
                divisionCalculee = "DIV_242_PRO";
                titre.innerText = "Division 242 (Commercial)";
                desc.innerText = "Grands yachts de pavillon français exploités commercialement (longueur ≥ 24 mètres).";
            }
        }

        // BRANCHE 5 : AQUACULTURE / TRAVAIL
        else if (usage === 'travail') {
            if (longueur < 24) {
                divisionCalculee = "DIV_230";
                titre.innerText = "Division 230 / 238";
                desc.innerText = "Navires de service et barges aquacoles de longueur inférieure à 24 mètres.";
            } else {
                divisionCalculee = "DIV_222";
                titre.innerText = "Division 222";
                desc.innerText = "Navires de charge et navires industriels lourds de longueur supérieure à 24 mètres.";
            }
        }

        // ==========================================
        // TEXTES DES PRESCRIPTIONS AJUSTÉS AUX DIVISIONS CALCULÉES
        // ==========================================

        // Spécificités : DIVISION 223b (Votre cible de départ)
        if (divisionCalculee === "DIV_223B") {
            htmlContent = `
                <h3>⚡ Prescriptions Division 223b &amp; 219-6 (Passagers &lt; 24m - Alu/Composite)</h3>
                <ul>
                    <li><strong>Bilan de Stockage :</strong> Capacité requise : <span class="badge-green">${capaciteKwh} kWh</span> | Poids : <span class="badge-green">${poidsKg.toLocaleString()} kg (${poidsTonnes.toFixed(2)} t)</span>.</li>
                    <li><strong>Homologation Marine Obligatoire :</strong> Agrément d'approbation de type (*Type Approval*) par organisme notifié (Bureau Veritas, DNV ou RINA) obligatoire pour le pack complet (Cellules + BMS). Norme <span class="badge">CEI 62619</span> exigée.</li>
                    <li><strong>Protection Incendie Critique (Div. 322) :</strong> Les coques en aluminium ou en composite n'ayant pas la résistance naturelle de l'acier, le local batterie doit être certifié coupe-feu <span class="badge">A-60</span>. Système d'extinction fixe automatique (brouillard d'eau ou gaz) commandé depuis la passerelle obligatoire.</li>
                    <li><strong>Estrin &amp; Structure :</strong> Les structures de fond doivent être renforcées pour encaisser les chocs et la masse localisée des batteries lors de la mise au sec mécanique ou sur l'estrin.</li>
                </ul>
            `;
            // Activation alerte stabilité
            if (poidsTonnes > seuilCritiqueTonnes) {
                alerteStabilite.style.display = "block";
                alerteStabilite.innerHTML = `<strong>⚠️ Alerte Division 211 (Stabilité critique) :</strong> Le poids calculé (${poidsTonnes.toFixed(2)} t) excède la capacité d'emport standard pour une coque légère de ${longueur}m. Un plan de pont spécifique et un test d'inclinaison par architecte naval seront exigés par le CSN.`;
            } else { alerteStabilite.style.display = "none"; }
        }

        // Spécificités : DIVISION 223a (Passagers < 24m en Acier)
        else if (divisionCalculee === "DIV_223A") {
            htmlContent = `
                <h3>⚓ Prescriptions Division 223a (Passagers &lt; 24m - Coque Acier)</h3>
                <ul>
                    <li><strong>Tolérance Incendie Structurelle :</strong> La coque étant en acier, le risque de propagation de l'emballage thermique est structurellement mieux confiné. L'isolation coupe-feu du compartiment batterie peut être assouplie (classe A-0 à A-30 selon positionnement), mais le système d'extinction fixe automatique reste obligatoire en espace clos.</li>
                    <li><strong>Poids et Assiette :</strong> L'acier supporte mieux les charges lourdes en fond de cale, ce qui est un avantage pour les parcs lithium, mais l'étude de stabilité reste soumise à la Division 211.</li>
                </ul>
            `;
            alerteStabilite.style.display = "none";
        }

        // Spécificités : DIVISIONS 223 ou 221 (Grands navires à Passagers)
        else if (divisionCalculee === "DIV_223_GRAND" || divisionCalculee === "DIV_221") {
            htmlContent = `
                <h3>🚢 Prescriptions Lourdes Divisions 223 &amp; 221 (Grands Navires à Passagers / SOLAS)</h3>
                <ul>
                    <li><strong>Redondance de Propulsion Totale :</strong> Division de puissance obligatoire. Le parc de batteries doit être divisé en au moins deux locaux physiques totalement indépendants (généralement bâbord et tribord) séparés par des cloisons étanches A-60. La perte d'un local batterie ne doit pas priver le navire de plus de 50% de sa propulsion.</li>
                    <li><strong>Système d'extinction industrielle :</strong> Exigence de systèmes d'extinction lourds centralisés (systèmes déluge ou CO2 haute pression) interconnectés aux alarmes d'urgence du navire.</li>
                </ul>
            `;
            alerteStabilite.style.display = "none";
        }

        // Spécificités : DIVISION 227 (Petite Pêche < 12m)
        else if (divisionCalculee === "DIV_227") {
            htmlContent = `
                <h3>🐟 Prescriptions Division 227 (Petite Pêche &lt; 12 mètres)</h3>
                <ul>
                    <li><strong>Étanchéité Renforcée (Milieu Sévère) :</strong> Les équipements électriques et caissons de batteries doivent afficher un indice minimal <span class="badge">IP66</span> ou <span class="badge">IP67</span> en raison de l'omniprésence d'humidité et des lavages à grande eau.</li>
                    <li><strong>Réseau Électrique Simplifié :</strong> Le réseau de traction lithium est toléré en tension inférieure à 50V (TBT) pour limiter le besoin de certifications lourdes, mais un disjoncteur général d'urgence reste obligatoire.</li>
                </ul>
            `;
            alerteStabilite.style.display = "none";
        }

        // Spécificités : DIVISION 226 (Pêche 12m à 24m)
        else if (divisionCalculee === "DIV_226") {
            htmlContent = `
                <h3>🌊 Prescriptions Division 226 (Pêche Professionnelle 12m à 24m)</h3>
                <ul>
                    <li><strong>Contrôleur Permanent d'Isolement (CPI) :</strong> Obligatoire. Le réseau de traction haute tension ne doit absolument pas être relié à la masse de la coque. Un détecteur de défaut de terre doit alerter instantanément le patron de pêche pour bloquer l'électrolyse.</li>
                    <li><strong>Autonomie Radiocommunications :</strong> Le parc de batteries moteur ne doit pas alimenter la VHF ou les instruments de détection. Une batterie de secours séparée (Div. 226-4) est exigée.</li>
                </ul>
            `;
            alerteStabilite.style.display = "none";
        }

        // Spécificités : DIVISION 240 (Plaisance de Loisir)
        else if (divisionCalculee === "DIV_240") {
            htmlContent = `
                <h3>⛵ Prescriptions Division 240 (Plaisance Privée &lt; 24m)</h3>
                <ul>
                    <li><strong>Régime Déclaratif (Marquage CE) :</strong> Pas d'inspection requise par le Bureau Veritas pour le parc batterie. Les batteries doivent être certifiées <span class="badge">CE</span> selon la directive européenne plaisance.</li>
                    <li><strong>Sécurité de base :</strong> Fixation rigide du parc en coffret ventilé pour empêcher tout déplacement au talon ou à la gîte (résistance aux chocs de navigation). Coupe-circuit général manuel obligatoire.</li>
                </ul>
            `;
            alerteStabilite.style.display = "none";
        }

        // Spécificités : DIVISION 241 (Plaisance Commerciale / NUC)
        else if (divisionCalculee === "DIV_241") {
            htmlContent = `
                <h3>💼 Prescriptions Division 241 (Plaisance Commerciale / NUC)</h3>
                <ul>
                    <li><strong>Sécurisation du Skipper :</strong> Un interrupteur d'arrêt d'urgence de type "coup de poing" doit être positionné au poste de barre pour isoler instantanément les parcs de traction au Lithium en cas d'avarie mécanique ou électrique.</li>
                    <li><strong>Accès d'extinction :</strong> Le compartiment batterie doit posséder un opercule d'extinction pour permettre d'injecter un extincteur CO2 depuis le pont sans ouvrir la cale (évite l'apport d'oxygène).</li>
                </ul>
            `;
            alerteStabilite.style.display = "none";
        }

        // Spécificités : DIVISION 230 (Barges de Travail &amp; Aquaculture)
        else if (divisionCalculee === "DIV_230") {
            htmlContent = `
                <h3>🏗️ Prescriptions Division 230 / 238 (Navires de Travail / Aquaculture)</h3>
                <ul>
                    <li><strong>Berceaux Anti-Chocs Réhaussés :</strong> Les batteries doivent être montées sur des châssis surélevés pour éviter le contact direct avec les eaux de fond de cale. La structure doit résister aux vibrations intenses des grues et des apparaux de levage de bord.</li>
                    <li><strong>Isolation Coque Aluminium :</strong> Surveillance drastique des courants de fuite (CPI obligatoire) pour éliminer les risques de corrosion perforante galvanique sur les coques alu des parcs aquacoles.</li>
                </ul>
            `;
            alerteStabilite.style.display = "none";
        }

        // Si cas hors normes non géré explicitement
        else {
            htmlContent = `<h3>⚡ Réglementation Générale Électrique Maritime</h3><p>Veuillez vous référer aux annexes cadres de la Division 219.</p>`;
            alerteStabilite.style.display = "none";
        }

        // Injection du HTML calculé sur-mesure
        blocElectrique.innerHTML = htmlContent;
    }

    // Premier lancement au chargement
    calculerDivision();
</script>

</body>
</html>
