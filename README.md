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

        .full-width {
            grid-column: span 2;
        }

        @media (max-width: 600px) {
            .full-width { grid-column: span 1; }
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
            display: none; /* Géré par JS */
        }

        .electric-specs h3 {
            margin-top: 0;
            color: #92400e;
            font-size: 1.2rem;
            border-bottom: 1px solid #fde68a;
            padding-bottom: 5px;
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
    </style>
</head>
<body>

<div class="container">
    <h1>Simulateur réglementaire Affaires Maritimes</h1>
    <p>Configurez les caractéristiques du navire pour identifier les règles de sécurité et les normes électriques obligatoires.</p>
    
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
        <h3>⚡ Exigences Critiques Propulsion Électrique &amp; Stockage (Division 219-6)</h3>
        
        <p><strong>Réglementation d'origine (Arrêté du 23 novembre 1987 modifié) :</strong> Le navire doit répondre aux critères de sécurité stricts pour éviter l'emballement thermique et garantir la sécurité des passagers.</p>
        
        <h4>1. Certification Obligatoire des Batteries</h4>
        <ul>
            <li><strong>Approbation de Type (Type Approval) :</strong> Le pack de batterie complet (Cellules + BMS + Enveloppe) doit posséder un certificat d'approbation d'une société de classification (Bureau Veritas, DNV, RINA).</li>
            <li><strong>Norme Sécurité Lithium :</strong> Conformité stricte à la norme internationale <span class="badge">CEI 62619</span> obligatoire (sécurité des accumulateurs lithium industriels).</li>
            <li><strong>Norme Transport :</strong> Certification d'origine <span class="badge">CEI 62281</span> pour la résistance au transport et aux chocs.</li>
        </ul>

        <h4>2. Architecture &amp; Système de Gestion (BMS)</h4>
        <ul>
            <li><strong>Alerte Passerelle Préventive :</strong> Le BMS doit obligatoirement renvoyer une alarme visuelle et sonore au poste de pilotage <em>avant</em> toute déconnexion automatique d'un bloc de batteries.</li>
            <li><strong>Sauvegarde de puissance :</strong> La coupure de sécurité d'un parc de batterie ne doit en aucun cas couper l'intégralité de la propulsion du navire (exigence de continuité de service).</li>
        </ul>

        <h4>3. Local Batteries &amp; Protection Incendie</h4>
        <ul>
            <li><strong>Cloisonnement Structurel :</strong> En Division 223b (coques alu/composite), le local batteries doit être isolé des zones passagers par des cloisons coupe-feu <span class="badge">A-60</span> (résistance 60 minutes).</li>
            <li><strong>Ventilation Dédiée :</strong> Extraction mécanique d'air indépendante débouchant directement à l'extérieur en zone sécurisée pour évacuer les gaz toxiques/inflammables en cas de dégazage (*off-gassing*).</li>
            <li><strong>Extinction Fixe :</strong> Le local doit intégrer un système d'extinction fixe automatique par brouillard d'eau ou agent gazeux spécifique certifié pour les feux de lithium (interdiction des systèmes à eau classiques).</li>
        </ul>

        <h4>4. Motorisation &amp; Sécurité Électrique</h4>
        <ul>
            <li><strong>Norme Moteur :</strong> Les moteurs électriques de propulsion doivent être conformes aux normes de la série <span class="badge">CEI 60034</span>.</li>
            <li><strong>Étanchéité :</strong> Indice de protection minimal <span class="badge">IP44</span> en cale fermée, fortement recommandé en <span class="badge">IP56</span> ou <span class="badge">IP68</span> selon l'exposition à l'eau de mer.</li>
            <li><strong>Contrôleur d'Isolement (CPI) :</strong> Obligation d'installer un contrôleur permanent d'isolement sur le réseau de puissance. Le réseau ne doit pas être mis à la masse de la coque pour éliminer le risque d'électrolyse (destruction rapide des coques en aluminium).</li>
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

        // Afficher/Masquer le champ passager selon l'activité
        if (usage === 'passagers') {
            groupPassagers.style.opacity = "1";
            groupPassagers.style.pointerEvents = "auto";
        } else {
            groupPassagers.style.opacity = "0.4";
            groupPassagers.style.pointerEvents = "none";
        }

        // Afficher le volet "Normes Électriques" si l'option est cochée
        if (propulsion === 'electrique') {
            blocElectrique.style.display = "block";
        } else {
            blocElectrique.style.display = "none";
        }

        // --- MOTEUR DE CALCUL DE L'ARBRE DE DÉCISION ---

        // Branche Pêche
        if (usage === 'peche') {
            if (longueur < 12) {
                titre.innerText = "Division 227";
                desc.innerText = "Navires de pêche de petite taille (moins de 12 mètres). Normes de sécurité artisanales.";
            } else if (longueur >= 12 && longueur < 24) {
                titre.innerText = "Division 226";
                desc.innerText = "Navires de pêche de taille intermédiaire (12 à 24 mètres). Exigences renforcées de stabilité.";
            } else {
                titre.innerText = "Division 228";
                desc.innerText = "Navires de pêche de grande taille (24 mètres et plus). Réglementation de pêche industrielle.";
            }
        }
        
        // Branche Plaisance Personnelle
        else if (usage === 'plaisance_perso') {
            if (longueur < 24) {
                titre.innerText = "Division 240";
                desc.innerText = "Règlement phare de la plaisance de loisir. Le matériel d'armement dépend de l'éloignement d'un abri.";
            } else {
                titre.innerText = "Division 242";
                desc.innerText = "Règlementation applicable aux grands yachts de loisir privés (plus de 24 mètres).";
            }
        }

        // Branche Plaisance Commerciale (NUC)
        else if (usage === 'plaisance_pro') {
            if (longueur < 24) {
                titre.innerText = "Division 241 (NUC)";
                desc.innerText = "Navires de plaisance à Utilisation Commerciale de moins de 24 mètres (location avec skipper, charter). Limité à un maximum de 12 passagers.";
            } else {
                titre.innerText = "Division 242 (Yacht Commercial)";
                desc.innerText = "Règlement de sécurité des grands yachts de pavillon français exploités commercialement.";
            }
        }

        // Branche Travail / Aquaculture
        else if (usage === 'travail') {
            if (longueur < 24) {
                titre.innerText = "Division 230 / 238";
                desc.innerText = "Navires aquacoles, conchylicoles ou petites embarcations de servitude et travaux maritimes côtiers.";
            } else {
                titre.innerText = "Division 222";
                desc.innerText = "Navires de charge de petite jauge brute (cargos, remorqueurs lourds).";
            }
        }

        // Branche Navires à Passagers
        else if (usage === 'passagers') {
            if (passagers <= 12) {
                titre.innerText = "Erreur de catégorie (Max 12 passagers)";
                desc.innerText = "Attention : un navire transportant 12 passagers ou moins ne peut pas être qualifié de 'Navire à passagers' au sens de la loi. Vous devez basculer l'activité sur 'Plaisance Commerciale / NUC (Division 241)'.";
                blocElectrique.style.display = "none"; // Masqué car incohérent
                return;
            }

            if (zone === 'internationale') {
                titre.innerText = "Division 221 (Sauvegarde de la vie en mer - SOLAS)";
                desc.innerText = "Navires à passagers en voyages internationaux. Soumis aux conventions internationales majeures, processus d'homologation très lourd.";
            } else {
                if (longueur >= 24) {
                    titre.innerText = "Division 223";
                    desc.innerText = "Grands navires à passagers en navigation nationale (Bacs de grande taille, Ferries nationaux).";
                } else {
                    if (coque === 'acier') {
                        titre.innerText = "Division 223a";
                        desc.innerText = "Navires à passagers de longueur inférieure à 24 mètres en navigation nationale, construits en acier.";
                    } else {
                        // LE CAS DE L'UTILISATEUR (223B + ÉLECTRIQUE)
                        if (propulsion === 'electrique') {
                            titre.innerText = "Division 223b & Division 219-6";
                            desc.innerText = "Navire à passagers (< 24m, coque alu/composite/bois). En raison de sa propulsion électrique/hybride, le navire est soumis de plein droit aux dispositions interconnectées de la Division 219 (Chapitre 6) pour la validation de ses batteries par le Centre de Sécurité des Navires.";
                        } else {
                            titre.innerText = "Division 223b";
                            desc.innerText = "Navires à passagers de longueur inférieure à 24 mètres en navigation nationale, construits en matériaux autres que l'acier (Aluminium, Composite, Bois).";
                        }
                    }
                }
            }
        }
    }

    // Premier lancement automatique
    calculerDivision();
</script>

</body>
</html>
