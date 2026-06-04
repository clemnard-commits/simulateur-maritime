<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulateur de Divisions Maritimes & Électriques</title>
    <style>
        :root {
            --primary: #1e3a8a;
            --primary-light: #eff6ff;
            --accent: #0ea5e9;
            --text: #1e293b;
            --bg: #f8fafc;
            --warning-bg: #fffbeb;
            --warning-border: #d97706;
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
            margin-bottom: 8px;
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
    <p>Configurez les caractéristiques du navire pour identifier les règles de sécurité, les obligations d'estrin et les normes électriques.</p>
    
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

        <div class="form-group">
            <label for="propulsion">Type de motorisation</label>
            <select id="propulsion" onchange="calculerDivision()">
                <option value="thermique">Thermique Classique (Diesel / Essence)</option>
                <option value="electrique">100% Électrique ou Hybride (Batteries Lithium)</option>
            </select>
        </div>
    </div>

    <div class="result-box">
        <div class="result-title" id="div-titre">Division --</div>
        <p class="result-text" id="div-desc">Sélectionnez les options pour analyser le navire.</p>
    </div>

    <div class="electric-specs" id="bloc-electrique">
        <h3>⚡ Prescriptions Techniques Critiques (Divisions 223b, 219-6 &amp; 322)</h3>
        
        <h4>1. Certification Obligatoire des Batteries</h4>
        <ul>
            <li><strong>Marine Type Approval :</strong> Le système global (cellules + BMS + coffret de protection) doit être certifié par un organisme notifié (Bureau Veritas, DNV, RINA).</li>
            <li><strong>Normes d'origine obligatoires :</strong> Conformité absolue aux normes <span class="badge">CEI 62619</span> (sécurité des batteries au lithium industrielles) et <span class="badge">CEI 62281</span> (sécurité lors du transport).</li>
        </ul>

        <h4>2. Risque Incendie &amp; Extinction (Division 322 &amp; 223b)</h4>
        <ul>
            <li><strong>Isolation Structurelle :</strong> Le local abritant les batteries doit obligatoirement former un caisson étanche coupe-feu de classe <span class="badge">A-60</span> (résistance au feu testée pendant 60 minutes).</li>
            <li><strong>Système d'extinction fixe :</strong> Obligation d'installer un dispositif fixe d'extinction automatique à déclenchement à distance (ex: brouillard d'eau haute pression ou agent inhibiteur gazeux) certifié spécifiquement pour étouffer l'emballement thermique du lithium. Les extincteurs à eau douce classiques ou CO2 standards sont interdits pour cet usage en local fermé.</li>
            <li><strong>Dégazage &amp; Ventilation :</strong> Système de ventilation mécanique indépendant forçant l'extraction des gaz inflammables (hydrogène/monoxyde de carbone issus du dégazage) directement vers l'extérieur en zone saine.</li>
        </ul>

        <h4>3. Contraintes d'Estrin (Échouage &amp; Mise au sec mécanique)</h4>
        <ul>
            <li><strong>Renfort structurel de quille :</strong> Si le navire utilise un estrin ou est destiné à s'échouer régulièrement à la marée, les liaisons de coque et les carlingues de fond doivent être échantillonnées pour supporter le poids accru des parcs de batteries sans déformation.</li>
            <li><strong>Isolation des chocs mécaniques :</strong> L'intégration mécanique des berceaux de batteries doit amortir les vibrations et les chocs verticaux violents liés à la mise sur l'estrin, afin de prévenir les courts-circuits internes des cellules lithium (exigence de l'inspection de sécurité).</li>
        </ul>

        <h4>4. Motorisation &amp; Sécurité Électrique</h4>
        <ul>
            <li><strong>Norme Moteur :</strong> Moteurs de propulsion certifiés selon la série <span class="badge">CEI 60034</span> (machines électriques tournantes).</li>
            <li><strong>Contrôleur Permanent d'Isolement (CPI) :</strong> Obligation d'un réseau à neutre isolé de la coque avec surveillance continue. Toute perte d'isolement doit immédiatement lever une alarme visuelle et sonore en passerelle pour prévenir le risque d'électrolyse destructrice sur les coques alu.</li>
        </ul>
    </div>

    <div class="links-box">
        <h4>📂 Liens officiels vers la réglementation d'origine (Gouvernement Français)</h4>
        <ul>
            <li><span class="badge-blue">Légifrance</span> : <a class="btn-link" href="https://www.legifrance.gouv.fr/loda/id/JORFTEXT000000313605/" target="_blank">Arrêté du 23 novembre 1987 (Règlement général de sécurité)</a></li>
            <li><span class="badge-blue">Ministère de la Mer</span> : <a class="btn-link" href="https://www.mer.gouv.fr/sites/default/files/2020-11/Division_223b_0.pdf" target="_blank">Télécharger le texte d'origine de la Division 223b (PDF)</a></li>
            <li><span class="badge-blue">Ministère de la Mer</span> : <a class="btn-link" href="https://www.mer.gouv.fr/sites/default/files/2020-11/Division_219.pdf" target="_blank">Télécharger le texte d'origine de la Division 219 - Systèmes Électriques (PDF)</a></li>
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

        const titre = document.getElementById('div-titre');
        const desc = document.getElementById('div-desc');
        const groupPassagers = document.getElementById('group-passagers');
        const blocElectrique = document.getElementById('bloc-electrique');

        if (usage === 'passagers') {
            groupPassagers.style.opacity = "1";
            groupPassagers.style.pointerEvents = "auto";
        } else {
            groupPassagers.style.opacity = "0.4";
            groupPassagers.style.pointerEvents = "none";
        }

        if (propulsion === 'electrique') {
            blocElectrique.style.display = "block";
        } else {
            blocElectrique.style.display = "none";
        }

        // Logique simplifiée arbre
        if (usage === 'peche') {
            titre.innerText = longueur < 12 ? "Division 227" : "Division 226/228";
            desc.innerText = "Réglementation relative aux navires de pêche professionnelle.";
        } else if (usage === 'plaisance_perso') {
            titre.innerText = "Division 240";
            desc.innerText = "Règlement de la plaisance de loisir.";
        } else if (usage === 'plaisance_pro') {
            titre.innerText = "Division 241 (NUC)";
            desc.innerText = "Plaisance commerciale (Skipper / Charter), limitée à 12 passagers maximum.";
        } else if (usage === 'travail') {
            titre.innerText = "Division 230 / 222";
            desc.innerText = "Navires aquacoles ou navires de charge/travaux maritimes.";
        } else if (usage === 'passagers') {
            if (passagers <= 12) {
                titre.innerText = "Erreur (Max 12 passagers)";
                desc.innerText = "Pour 12 passagers ou moins, l'homologation requise est la Division 241 (NUC) et non Navire à Passagers.";
                blocElectrique.style.display = "none";
                return;
            }
            if (zone === 'internationale') {
                titre.innerText = "Division 221 (SOLAS)";
                desc.innerText = "Navire à passagers international.";
            } else {
                if (longueur >= 24) {
                    titre.innerText = "Division 223";
                    desc.innerText = "Grand navire à passagers national.";
                } else {
                    if (coque === 'acier') {
                        titre.innerText = "Division 223a";
                        desc.innerText = "Navire à passagers national de moins de 24 mètres en acier.";
                    } else {
                        if (propulsion === 'electrique') {
                            titre.innerText = "Division 223b, 219-6 & 322";
                            desc.innerText = "Navire à passagers national (< 24m, coque alu/composite/bois). Soumis de plein droit aux contraintes croisées de la Division 223b (structure), la Division 219 (stockage d'énergie lithium) et la Division 322 (systèmes d'extinction d'incendie fixes).";
                        } else {
                            titre.innerText = "Division 223b";
                            desc.innerText = "Navire à passagers national de moins de 24 mètres en matériaux autres que l'acier.";
                        }
                    }
                }
            }
        }
    }

    calculerDivision();
</script>

</body>
</html>
