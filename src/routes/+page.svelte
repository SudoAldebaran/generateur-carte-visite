<script>
    let titre = ""; // Nouvelle variable pour le titre
    let nom = "";
    let email = "";
    let telephone = "";
    let profession = "";
    let selectedStyle = "classique"; // Style par défaut

    const styles = {
        classique: {
            name: "Classique",
            headerBg: "#ffffff",
            headerColor: "#000000",
            bodyBg: "#f0f0f0",
            bodyColor: "#000000",
            borderRadius: "0px",
            separator: true,
            boxShadow: "none",
            border: "1px solid #000000",
            pattern: "none",
            headerFont: "'Times New Roman', serif",
            bodyFont: "Arial, sans-serif",
            nameSize: "2rem",
            professionSize: "1.3rem",
            nameWeight: "bold",
            professionWeight: "normal",
            iconStyle: "classic",
        },
        moderne: {
            name: "Moderne",
            headerBg: "#ffffff",
            headerColor: "#000000",
            bodyBg: "#ffffff",
            bodyColor: "#000000",
            borderRadius: "0px",
            separator: false,
            boxShadow: "0 4px 12px rgba(0, 0, 0, 0.1)",
            border: "none",
            pattern: "geometric",
            headerFont: "'Montserrat', sans-serif",
            bodyFont: "'Open Sans', sans-serif",
            nameSize: "2.2rem",
            professionSize: "1.4rem",
            nameWeight: "900",
            professionWeight: "normal",
            iconStyle: "modern",
        },
        professionnel: {
            name: "Professionnel",
            headerBg: "#2c3e50",
            headerColor: "#03224c",
            bodyBg: "#ecf0f1",
            bodyColor: "#03224c",
            borderRadius: "6px",
            separator: false,
            boxShadow: "0 4px 12px rgba(0, 0, 0, 0.3)",
            border: "none",
            pattern: "none",
            headerFont: "'Helvetica Neue', sans-serif",
            bodyFont: "'Roboto', sans-serif",
            nameSize: "1.8rem",
            professionSize: "1.2rem",
            nameWeight: "600",
            professionWeight: "500",
            iconStyle: "professional",
        },
    };

    $: currentStyle = styles[selectedStyle];

    function getIcons(style) {
        return {}; // Plus d'icônes
    }

    $: icons = getIcons(currentStyle);
</script>

<main>
    <h1>Générateur de Carte de Visite</h1>
    <p class="subtitle">Créez une carte élégante en quelques secondes</p>

    <div class="container">
        <div class="formulaire">
            <div class="input-group">
                <label for="titre">Titre</label>
                <select id="titre" bind:value={titre}>
                    <option value="">Aucun</option>
                    <option value="M.">M.</option>
                    <option value="Mme.">Mme.</option>
                    <option value="Me">Me</option>
                    <option value="Dr">Dr</option>
                </select>
            </div>
            <div class="input-group">
                <label for="nom">Nom</label>
                <input id="nom" type="text" bind:value={nom} placeholder="Sabrina Smith" />
            </div>
            <div class="input-group">
                <label for="profession">Profession</label>
                <input id="profession" type="text" bind:value={profession} placeholder="Nutricioniste" />
            </div>
            <div class="input-group">
                <label for="email">Email</label>
                <input id="email" type="email" bind:value={email} placeholder="sabrina@the-company.com" />
            </div>
            <div class="input-group">
                <label for="telephone">Téléphone</label>
                <input id="telephone" type="tel" bind:value={telephone} placeholder="022 - 786 127 890" />
            </div>
            <div class="input-group">
                <label for="style">Choisissez un style</label>
                <select id="style" bind:value={selectedStyle}>
                    <option value="classique">Classique</option>
                    <option value="moderne">Moderne</option>
                    <option value="professionnel">Professionnel</option>
                </select>
            </div>
        </div>

        <div
            class="carte"
            style="
                --header-bg: {currentStyle.headerBg};
                --header-color: {currentStyle.headerColor};
                --body-bg: {currentStyle.bodyBg};
                --body-color: {currentStyle.bodyColor};
                --border-radius: {currentStyle.borderRadius};
                --box-shadow: {currentStyle.boxShadow};
                --border: {currentStyle.border};
                --header-font: {currentStyle.headerFont};
                --body-font: {currentStyle.bodyFont};
                --name-size: {currentStyle.nameSize};
                --profession-size: {currentStyle.professionSize};
                --name-weight: {currentStyle.nameWeight};
                --profession-weight: {currentStyle.professionWeight};
            "
            class:carte-classique={selectedStyle === "classique"}
            class:carte-moderne={selectedStyle === "moderne"}
            class:carte-professionnel={selectedStyle === "professionnel"}
        >
            {#if currentStyle.pattern === "geometric"}
                <div class="pattern-container">
                    <div class="top-left-curve"></div>
                    <div class="top-right-curve"></div>
                    <div class="center-square"></div>
                    <div class="center-bordeaux-circle"></div>
                    <div class="plume-bleu"></div>
                    <div class="plume-orange"></div>
                    <div class="plume-bas"></div>
                    <div class="quart-cercle"></div>
                    <div class="quart-cercle-droit"></div>
                    <div class="demi-cercle"></div>
                    <div class="bottom-right-curve"></div>
                    <div class="bottom-green-shape"></div>
                </div>
            {/if}

            <div class="carte-content">
                <div class="carte-header">
                    <h2>{titre ? titre + " " : ""}{nom || "Sabrina Smith"}</h2>
                    <h3>{profession || "Nutricioniste"}</h3>
                </div>

                {#if currentStyle.separator}
                    <hr
                        class:separator-classique={selectedStyle === "classique"}
                        class:separator-professionnel={selectedStyle === "professionnel"}
                    />
                {/if}

                <div class="carte-body">
                    <div class="contact-info">
                        <p>{email || "sabrina@the-company.com"}</p>
                        <p>{telephone || "022 - 786 127 890"}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</main>

<style>
    :global(body) {
        font-family: "Arial", sans-serif;
        background-color: #1a1a1a;
        color: #ffffff;
        margin: 0;
        padding: 0;
    }

    main {
        width: 900px; /* Taille fixe au lieu de max-width */
        margin: 40px auto;
        text-align: center;
    }

    h1 {
        font-size: 2.5rem;
        color: #ecf0f1;
        margin-bottom: 10px;
    }

    .subtitle {
        font-size: 1.1rem;
        color: #bdc3c7;
        margin-bottom: 30px;
    }

    .container {
        display: flex;
        justify-content: space-between;
        gap: 40px;
        width: 900px; /* Taille fixe */
    }

    .formulaire {
        width: 400px; /* Taille fixe au lieu de flex */
        background: #2c3e50;
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
    }

    .input-group {
        margin-bottom: 20px;
        text-align: left;
    }

    label {
        display: block;
        font-size: 0.9rem;
        color: #ecf0f1;
        margin-bottom: 5px;
    }

    input,
    select {
        width: 90%; /* Réduit la largeur à 90% du conteneur */
        max-width: 350px; /* Limite la largeur maximale */
        padding: 10px;
        font-size: 1rem;
        border: 1px solid #7f8c8d;
        border-radius: 8px;
        outline: none;
        transition: border-color 0.3s;
        background-color: #34495e;
        color: #ecf0f1;
        margin: 0 auto; /* Centre les champs si nécessaire */
    }

    input:focus,
    select:focus {
        border-color: #3498db;
    }

    /* Carte avec taille fixe */
    .carte {
        position: relative;
        width: 450px; /* Taille fixe */
        height: 250px; /* Taille fixe */
        background: var(--body-bg);
        border-radius: var(--border-radius);
        box-shadow: var(--box-shadow);
        border: var(--border);
        overflow: hidden;
        transform-origin: top left; /* Point de départ pour la réduction */
        transition: transform 0.3s ease;
    }

    .carte:hover {
        transform: translateY(-5px);
    }

    .carte-content {
        position: relative;
        z-index: 2;
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
    }

    /* Style Classique */
    .carte-classique .carte-header {
        position: absolute;
        top: 15px;
        left: 20px;
        text-align: left;
    }

    .carte-classique .carte-header h2 {
        margin: 0;
        font-size: var(--name-size);
        font-weight: var(--name-weight);
        font-family: var(--header-font);
        color: var(--header-color);
    }

    .carte-classique .carte-header h3 {
        margin: 5px 0 0 0;
        font-size: var(--profession-size);
        font-weight: var(--profession-weight);
        font-family: var(--header-font);
        color: var(--header-color);
    }

    .carte-classique .carte-body {
        position: absolute;
        top: 130px;
        left: 20px;
        font-family: var(--body-font);
        text-align: left;
    }

    .carte-classique .contact-info p {
        margin: 8px 0;
        font-size: 1rem;
        color: var(--body-color);
    }

    .carte-classique .separator-classique {
        position: absolute;
        top: 110px;
        left: 20px;
        right: 20px;
        border-top: 1px solid #000000;
        margin: 0;
    }

    /* Style Moderne */
    .carte-moderne .carte-header {
        position: absolute;
        bottom: 20px;
        right: 20px;
        text-align: right;
    }

    .carte-moderne .carte-header h2 {
        margin: 0;
        font-size: var(--name-size);
        font-weight: var(--name-weight);
        font-family: var(--header-font);
        color: var(--header-color);
    }

    .carte-moderne .carte-header h3 {
        margin: 5px 0 0 0;
        font-size: var(--profession-size);
        font-weight: var(--profession-weight);
        font-family: var(--header-font);
        color: var(--header-color);
    }

    .carte-moderne .carte-body {
        position: absolute;
        top: 20px;
        left: 20px;
        font-family: var(--body-font);
        text-align: left;
    }

    .carte-moderne .contact-info p {
        margin: 8px 0;
        font-size: 1rem;
        color: var(--body-color);
    }

    .pattern-container {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: 1;
        overflow: hidden;
    }

    .top-left-curve {
        position: absolute;
        top: -80px;
        left: -80px;
        width: 160px;
        height: 160px;
        border-radius: 50%;
        background-color: #f4e04d;
    }

    .top-right-curve {
        position: absolute;
        top: -70px;
        right: -70px;
        width: 140px;
        height: 140px;
        border-radius: 50%;
        background-color: #93a8ac;
    }

    .plume-orange {
        position: absolute;
        width: 70px;
        height: 50px;
        background-color: #f2ed6f;
        border-radius: 100px 0px 100px 0;
        top: 0;
        right: 0px;
    }

    .plume-bleu {
        position: absolute;
        width: 80px;
        height: 60px;
        background-color: #14342b;
        border-radius: 100px 0px 100px 0;
        top: 0;
        right: 0px;
    }

    .demi-cercle {
        position: absolute;
        width: 70px;
        height: 35px;
        background-color: #14342b;
        border-radius: 0 0 100px 100px;
        top: 60px;
        right: 80px;
    }

    .quart-cercle {
        position: absolute;
        width: 35px;
        height: 40px;
        background-color: #93a8ac;
        border-radius: 0px 80px 0 0;
        bottom: 0;
        right: 130px;
    }

    .plume-bas {
        position: absolute;
        width: 30px;
        height: 40px;
        background-color: #add8c7;
        border-radius: 100px 0px 100px 0;
        bottom: 0px;
        right: 165px;
    }

    .center-square {
        position: absolute;
        top: 25px;
        right: 115px;
        width: 35px;
        height: 35px;
        background-color: #add8c7;
    }

    .quart-cercle-droit {
        position: absolute;
        width: 35px;
        height: 35px;
        background-color: #f4e04d;
        border-radius: 80px 0 0 0;
        top: 25px;
        right: 80px;
    }

    /* Style Professionnel */
    .carte-professionnel .carte-header {
        position: absolute;
        top: 50px;
        left: 0;
        right: 0;
        text-align: center;
    }

    .carte-professionnel .carte-header h2 {
        margin: 0;
        font-size: var(--name-size);
        font-weight: var(--name-weight);
        font-family: var(--header-font);
        color: var(--header-color);
    }

    .carte-professionnel .carte-header h3 {
        margin: 5px 0 0 0;
        font-size: var(--profession-size);
        font-weight: var(--profession-weight);
        font-family: var(--header-font);
        color: var(--header-color);
    }

    .carte-professionnel .carte-body {
        position: absolute;
        bottom: 40px;
        left: 0;
        right: 0;
        font-family: var(--body-font);
        text-align: center;
    }

    .carte-professionnel .contact-info p {
        margin: 5px 0;
        font-size: 1rem;
        color: var(--body-color);
    }

    .carte-professionnel .separator-professionnel {
        position: absolute;
        top: 100px;
        left: 20px;
        right: 20px;
        border-top: 1px solid #ecf0f1;
        margin: 0;
    }
</style>