<!DOCTYPE html>
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
    <p>Configurez les caractéristiques du navire pour identifier les règles de sécurité, les obligations d'estrin et l'impact du poids des batteries.</p>
    
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
            <label for="longueur">Longueur du navire : <span id="longueur-val">15</span> m</label>
            <div class="slider-container">
                <input type="range" id="longueur" min="5" max="50" value="15" oninput="updateLongueur(this.value)">
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

        <div class="sub-grid" id="bloc-calculateur-batterie" style="display: none;">
            <h4>📐 Dimensionnement Électrique Estimatif (Densité moyenne LFP Marine : 100 Wh/kg)</h4>
            <div class="form-group">
                <label for="puissance-moteur">Puissance du moteur (kW)</label>
                <input type="number" id="puissance-moteur" value="50" min="1" oninput="calculerDivision()">
            </div>
            <div class="form-group">
                <label for="autonomie">Autonomie souhaitée en croisière (Heures)</label>
                <input type="number" id="autonomie" value="4" min="0.5" step="0.5" oninput="calculerDivision()">
            </div>
        </div>
    </div>

    <div class="alert-stability" id="alerte-stabilite">
        ⚠️ Alerte Stabilité Critique !
    </div>

    <div class="result-box">
        <div class="result-title" id="div-titre">Division --</div>
        <p class="result-text" id="div-desc">Sélectionnez les options pour analyser le navire.</p>
    </div>

    <div class="electric-specs" id="bloc-electrique">
        <h3>⚡ Prescriptions Techniques Critiques (Divisions 223b, 219-6 &amp; 322)</h3>
        
        <h4>1. Bilan Énergétique Estimé</h4>
        <ul>
            <li><strong>Capacité Utile Requise :</strong> <span class="badge-green" id="res-capacite">0 kWh</span> (avec marge technique de sécurité de 20%).</li>
            <li><strong>Poids Estimé du Parc :</strong> <span class="badge-green" id="res-poids">0 kg</span> (Berceaux métalliques et BMS inclus).</li>
        </ul>

        <h4>2. Certification Obligatoire des Batteries</h4>
        <ul>
            <li><strong>Marine Type Approval :</strong> Le système global (cellules + BMS + coffret de protection) doit être certifié par un organisme notifié (Bureau Veritas, DNV, RINA).</li>
            <li><strong>Normes d'origine obligatoires :</strong> Conformité absolue aux normes <span class="badge">CEI 62619</span> (sécurité des batteries au lithium industrielles) et <span class="badge">CEI 62281</span> (sécurité lors du transport).</li>
        </ul>

        <h4>3. Risque Incendie &amp; Extinction (Division 322 &amp; 223b)</h4>
        <ul>
            <li><strong>Isolation Structurelle :</strong> Le local abritant les batteries doit obligatoirement former un caisson étanche coupe-feu de classe <span class="badge">A-60</span> (résistance au feu testée pendant 60 minutes).</li>
            <li><strong>Système d'extinction fixe :</strong> Dispositif automatique à déclenchement à distance (brouillard d'eau ou agent gazeux) certifié pour stopper l'emballement thermique. Extincteurs classiques interdits.</li>
            <li><strong>Dégazage :</strong> Extraction mécanique indépendante forçant l'évacuation des gaz vers l'extérieur.</li>
        </ul>

        <h4>4. Contraintes d'Estrin (Échouage &amp; Mise au sec mécanique)</h4>
        <ul>
            <li><strong>Renfort structurel de quille :</strong> Les liaisons de coque et les varangues de fond doivent être échantillonnées pour supporter la charge ponctuelle lourde des batteries lors de la mise sur estrin.</li>
            <li><strong>Amortissement mécanique :</strong> L'intégration des berceaux de batteries doit isoler les cellules des chocs verticaux secs subis lors de l'échouage régulier.</li>
        </ul>

        <h4>5. Motorisation &amp; Sécurité Électrique</h4>
        <ul>
            <li><strong>Norme Moteur :</strong> Conforme à la série <span class="badge">CEI 60034</span>.</li>
            <li><strong>Contrôleur Permanent d'Isolement (CPI) :</strong> Obligatoire. Le réseau ne doit pas être mis à la masse de la coque pour supprimer tout risque d'électrolyse (corrosion accélérée de l'aluminium).</li>
        </ul>
    </div>

    <div class="links-box">
        <h4>📂 Liens officiels vers la réglementation d'origine (Gouvernement Français)</h4>
        <ul>
            <li><span class="badge-blue">Légifrance</span> : <a class="btn-link" href="https://www.legifrance.gouv.fr/loda/id/JORFTEXT000000313605/" target="_blank">Arrêté du 23 novembre 1987 (Règlement général)</a></li>
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
        
        // Nouveaux paramètres électriques
        const puissanceMoteur = parseFloat(document.getElementById('puissance-moteur').value) || 0;
        const autonomie = parseFloat(document.getElementById('autonomie').value) || 0;

        const titre = document.getElementById('div-titre');
        const desc = document.getElementById('div-desc');
        const groupPassagers = document.getElementById('group-passagers');
        const blocElectrique = document.getElementById('bloc-electrique');
        const blocCalculateur = document.getElementById('bloc-calculateur-batterie');
        const alerteStabilite = document.getElementById('alerte-stabilite');

        // Afficher/Masquer le champ passager selon l'activité
        if (usage === 'passagers') {
            groupPassagers.style.opacity = "1";
            groupPassagers.style.pointerEvents = "auto";
        } else {
            groupPassagers.style.opacity = "0.4";
            groupPassagers.style.pointerEvents = "none";
        }

        // Gestion de l'affichage des blocs électriques
        if (propulsion === 'electrique') {
            blocElectrique.style.display = "block";
            blocCalculateur.style.display = "grid";
            
            // --- LOGIQUE DE CALCUL DU POIDS ET DE LA CAPACITÉ ---
            // On calcule l'énergie nette consommée (Puissance x Temps)
            // On ajoute une marge de décharge/sécurité de 20% (multiplié par 1.2)
            const capaciteKwh = Math.round((puissanceMoteur * autonomie) * 1.2);
            // Densité énergétique LFP marine moyenne de sécurité = 100 Wh/kg (soit 10 kg par kWh)
            const poidsKg = capaciteKwh * 10;

            document.getElementById('res-capacite').innerText = capaciteKwh + " kWh";
            document.getElementById('res-poids').innerText = poidsKg.toLocaleString() + " kg (" + (poidsKg/1000).toFixed(2) + " t)";

            // --- ANALYSE DE LA STABILITÉ (CRITÈRES ALERTE) ---
            // Seuil empirique de sécurité : Le poids de la batterie ne doit pas excéder un certain ratio de la taille du navire
            // Exemple : Pour 15m, si batterie > 2.5 tonnes -> Alerte Division 211
            let seuilCritiqueTonnes = 1.0; 
            if (longueur <= 12) seuilCritiqueTonnes = 1.2;
            else if (longueur <= 16) seuilCritiqueTonnes = 2.5;
            else if (longueur <= 20) seuilCritiqueTonnes = 4.5;
            else if (longueur <= 24) seuilCritiqueTonnes = 7.0;
            else seuilCritiqueTonnes = 12.0;

            const poidsTonnes = poidsKg / 1000;

            if (poidsTonnes > seuilCritiqueTonnes && usage === 'passagers' && coque === 'autre') {
                alerteStabilite.style.display = "block";
                alerteStabilite.innerHTML = "<strong>⚠️ Alerte Division 211 (Stabilité critique) :</strong> Le poids estimé du parc de batteries (" + poidsTonnes.toFixed(2) + " t) est trop élevé pour un navire en " + (longueur) + "m de type composite/alu. Une étude de stabilité approfondie (test d'inclinaison, pesée de coque) menée par un cabinet d'architecture navale agréé sera exigée par le CSN pour obtenir le permis de navigation.";
            } else {
                alerteStabilite.style.display = "none";
            }

        } else {
            blocElectrique.style.display = "none";
            blocCalculateur.style.display = "none";
            alerteStabilite.style.display = "none";
        }

        // Logique de calcul des divisions
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
                blocCalculateur.style.display = "none";
                alerteStabilite.style.display = "none";
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
                            desc.innerText = "Navire à passagers national (< 24m, coque alu/composite/bois). Soumis aux contraintes de la Division 223b (structure), la Division 219-6 (Lithium) et la Division 322 (incendie).";
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
