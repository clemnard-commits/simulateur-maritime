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

    <div class="alert-stability" id="alerte-stabilite">
        ⚠️ Alerte Stabilité Critique !
    </div>

    <div class="result-box">
        <div class="result-title" id="div-titre">Division --</div>
        <p class="result-text" id="div-desc">Sélectionnez les options pour analyser le navire.</p>
    </div>

    <div class="electric-specs" id="bloc-electrique">
        </div>

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

        // Visibilité du bloc passager
        if (usage === 'passagers') {
            groupPassagers.style.opacity = "1";
            groupPassagers.style.pointerEvents = "auto";
        } else {
            groupPassagers.style.opacity = "0.4";
            groupPassagers.style.pointerEvents = "none";
        }

        // Visibilité du calculateur de batterie
        if (propulsion === 'electrique') {
            blocElectrique.style.display = "block";
            blocCalculateur.style.display = "grid";
        } else {
            blocElectrique.style.display = "none";
            blocCalculateur.style.display = "none";
            alerteStabilite.style.display = "none";
        }

        // Calculs de base de l'énergie et du poids
        const capaciteKwh = Math.round((puissanceMoteur * autonomie) * 1.2);
        const poidsKg = capaciteKwh * 10;
        const poidsTonnes = poidsKg / 1000;

        // Seuil générique pour l'alerte stabilité (Division 211)
        let seuilCritiqueTonnes = 2.5;
        if (longueur <= 12) seuilCritiqueTonnes = 1.2;
        else if (longueur <= 16) seuilCritiqueTonnes = 2.5;
        else if (longueur <= 20) seuilCritiqueTonnes = 4.5;
        else if (longueur <= 24) seuilCritiqueTonnes = 7.0;

        // --- CODE DE GÉNÉRATION DYNAMIQUE SELON L'USAGE ---
        let htmlContent = "";

        // CAS 1 : USAGE PROFESSIONNEL / TRANSPORT DE PASSAGERS
        if (usage === 'passagers') {
            if (passagers <= 12) {
                titre.innerText = "Erreur (Max 12 passagers)";
                desc.innerText = "Pour 12 passagers ou moins, l'homologation requise est la Division 241 (NUC) et non Navire à Passagers.";
                blocElectrique.style.style.display = "none";
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
                        titre.innerText = "Division 223b, 219-6 & 322";
                        desc.innerText = "Navire à passagers national (< 24m, coque alu/composite/bois). Soumis aux contraintes de la Division 223b.";
                        
                        // Injection des specs Passagers 223b
                        htmlContent = `
                            <h3>⚡ Prescriptions Techniques Critiques (Divisions 223b, 219-6 &amp; 322)</h3>
                            <h4>1. Bilan Énergétique Estimé</h4>
                            <ul>
                                <li><strong>Capacité Utile Minimale :</strong> <span class="badge-green">${capaciteKwh} kWh</span> (avec 20% de réserve technique).</li>
                                <li><strong>Poids Net Estimé :</strong> <span class="badge-green">${poidsKg.toLocaleString()} kg (${poidsTonnes.toFixed(2)} t)</span>.</li>
                            </ul>
                            <h4>2. Certification et Sécurité Batteries (Division 219-6)</h4>
                            <ul>
                                <li><strong>Marine Type Approval :</strong> Agrément complet du bloc par un organisme notifié (Bureau Veritas, DNV ou RINA) obligatoire pour le transport public. Normes <span class="badge">CEI 62619</span> et <span class="badge">CEI 62281</span> requises.</li>
                                <li><strong>BMS &amp; Alarme Passerelle :</strong> Le système de gestion de batterie doit envoyer un signal d'alerte sonore et visuel au pilote <em>avant</em> de couper l'alimentation pour surchauffe.</li>
                            </ul>
                            <h4>3. Cloisonnement et Extinction Incendie (Division 322)</h4>
                            <ul>
                                <li><strong>Local Batterie Dédié :</strong> Isolement coupe-feu de classe <span class="badge">A-60</span> obligatoire (60 min) par rapport aux zones passagers.</li>
                                <li><strong>Extinction Fixe :</strong> Obligation d'un système automatique fixe (brouillard d'eau ou gaz) commandé depuis la passerelle. Extincteurs manuels refusés pour la validation du local.</li>
                                <li><strong>Ventilation :</strong> Extraction mécanique indépendante vers l'extérieur pour évacuer les gaz de dégazage.</li>
                            </ul>
                            <h4>4. Contraintes d'Estrin &amp; Échouage</h4>
                            <ul>
                                <li><strong>Renfort Structurel :</strong> Les varangues et structures de fond doivent être échantillonnées pour supporter la charge lourde localisée des batteries lors de la mise sur estrin.</li>
                            </ul>
                        `;

                        // Alerte Stabilité spécifique 223b
                        if (poidsTonnes > seuilCritiqueTonnes && coque === 'autre') {
                            alerteStabilite.style.display = "block";
                            alerteStabilite.innerHTML = `<strong>⚠️ Alerte Division 211 (Stabilité critique - Passagers) :</strong> Le poids de vos batteries (${poidsTonnes.toFixed(2)} t) est trop élevé pour une coque légère de ${longueur}m. Une étude de stabilité avec essai de pesée et d'inclinaison sera exigée par le Centre de Sécurité des Navires (CSN).`;
                        } else {
                            alerteStabilite.style.display = "none";
                        }
                    }
                }
            }
        }

        // CAS 2 : USAGE PLAISANCE PERSONNELLE (DIVISION 240)
        else if (usage === 'plaisance_perso') {
            titre.innerText = longueur < 24 ? "Division 240" : "Division 242";
            desc.innerText = "Règlement général de la plaisance de loisir.";
            alerteStabilite.style.style.display = "none";

            htmlContent = `
                <h3>⛵ Prescriptions Électriques Plaisance Privée (Division 240)</h3>
                <h4>1. Bilan Énergétique Estimé</h4>
                <ul>
                    <li><strong>Capacité de stockage :</strong> <span class="badge-green">${capaciteKwh} kWh</span> / <strong>Poids estimé :</strong> <span class="badge-green">${poidsKg.toLocaleString()} kg</span>.</li>
                </ul>
                <h4>2. Exigences Électriques de Bord</h4>
                <ul>
                    <li><strong>Marquage CE :</strong> Contrairement aux navires de commerce, les batteries de plaisance ne nécessitent pas de certification lourde par Bureau Veritas, mais doivent obligatoirement posséder le marquage européen <span class="badge">CE</span> et respecter les normes de sécurité de construction de la directive plaisance.</li>
                    <li><strong>Protection des circuits :</strong> Coupe-circuit bipolaire général obligatoire accessible rapidement. Fusibles ou disjoncteurs adaptés à chaque départ de ligne de puissance.</li>
                </ul>
                <h4>3. Ventilation et Compartimentage</h4>
                <ul>
                    <li><strong>Aération naturelle ou forcée :</strong> Les compartiments batteries doivent être ventilés de manière à éviter toute accumulation d'hydrogène ou de gaz de décharge, mais sans obligation de cloisons coupe-feu certifiées A-60 (sauf si la taille dépasse 24 mètres).</li>
                </ul>
            `;
        }

        // CAS 3 : USAGE PLAISANCE COMMERCIALE / CHARTER (DIVISION 241 - NUC)
        else if (usage === 'plaisance_pro') {
            titre.innerText = "Division 241 (Navire d'Utilisation Commerciale - NUC)";
            desc.innerText = "Règlement applicable aux navires de plaisance loués avec skipper (limité à 12 passagers).";
            alerteStabilite.style.style.display = "none";

            htmlContent = `
                <h3>💼 Obligations Techniques Plaisance Commerciale (Division 241 / NUC)</h3>
                <h4>1. Stockage &amp; Sécurité Électrique</h4>
                <ul>
                    <li><strong>BMS de Sécurité Répertorié :</strong> Les parcs de batteries de propulsion au Lithium doivent disposer d'un BMS fiable coupant automatiquement la charge et la décharge en cas d'anomalie, avec indicateur de charge visible au poste de pilotage.</li>
                    <li><strong>Coupure d'urgence :</strong> Un arrêt d'urgence "coup de poing" doit être installé à la barre pour isoler instantanément le moteur électrique et le parc de traction en cas d'avarie.</li>
                </ul>
                <h4>2. Sécurité Incendie</h4>
                <ul>
                    <li><strong>Extinction en cale :</strong> Le compartiment moteur/batterie doit pouvoir être fermé hermétiquement et disposer d'un orifice de projection pour y injecter un agent extincteur depuis l'extérieur sans ouvrir les panneaux de cale.</li>
                </ul>
            `;
        }

        // CAS 4 : USAGE PÊCHE PROFESSIONNELLE (DIVISION 226 / 227)
        else if (usage === 'peche') {
            const divPeche = longueur < 12 ? "Division 227" : "Division 226";
            titre.innerText = divPeche;
            desc.innerText = `Règlement de sécurité des navires de pêche professionnelle de taille inférieure à 24 mètres.`;
            alerteStabilite.style.style.display = "none";

            htmlContent = `
                <h3>🐟 Normes Électriques Navire de Pêche (Division 226 / 227)</h3>
                <h4>1. Robustesse et Étanchéité</h4>
                <ul>
                    <li><strong>Indice IP Élevé :</strong> Le milieu de la pêche artisanale étant très exposé, les moteurs et les coffrets de batteries doivent afficher un indice de protection d'origine minimal <span class="badge">IP56</span> ou <span class="badge">IP65</span> (résistance aux paquets de mer et nettoyages haute pression).</li>
                    <li><strong>Contrôleur d'Isolement (CPI) :</strong> Obligatoire pour éviter toute fuite de courant dans la coque, protégeant l'équipage contre les risques d'électrisation lors des manœuvres des engins de pêche (treuils, apparaux).</li>
                </ul>
                <h4>2. Autonomie et Continuité</h4>
                <ul>
                    <li><strong>Alimentation de secours :</strong> Le parc de batterie de propulsion ne doit pas être partagé avec les équipements de navigation obligatoires (VHF, GPS, feux de navigation), qui doivent conserver leur propre batterie de secours indépendante.</li>
                </ul>
            `;
        }

        // CAS 5 : AQUACULTURE / NAVIRES DE TRAVAIL (DIVISION 230)
        else if (usage === 'travail') {
            titre.innerText = "Division 230 / 238";
            desc.innerText = "Règlement applicable aux navires de travaux maritimes côtiers et barges aquacoles.";
            alerteStabilite.style.style.display = "none";

            htmlContent = `
                <h3>🏗️ Spécifications Barges de Travail &amp; Aquaculture (Division 230)</h3>
                <h4>1. Fixation Mécanique et Chocs</h4>
                <ul>
                    <li><strong>Résistance aux impacts :</strong> Les structures de réception des batteries doivent être surdimensionnées pour résister aux chocs répétés contre les installations conchylicoles, les quais ou lors de manutentions lourdes à la grue à bord.</li>
                </ul>
                <h4>2. Sécurité Électrique de Puissance</h4>
                <ul>
                    <li><strong>Protection Electrolyse :</strong> Exigence absolue de surveillance de l'isolement du réseau électrique de propulsion. Les fuites de courant de traction sont la cause principale de la corrosion perforante ultra-rapide sur les coques alu des barges de travail.</li>
                </ul>
            `;
        }

        // Injection finale du contenu réécrit dans le bloc jaune
        blocElectrique.innerHTML = htmlContent;
    }

    // Lancement au chargement de la page
    calculerDivision();
</script>

</body>
</html>



