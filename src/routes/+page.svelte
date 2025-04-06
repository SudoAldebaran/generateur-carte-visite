<script>
    let nom = "";
    let email = "";
    let telephone = "";
    let profession = "";
    let facebook = "";
    let twitter = "";
    let instagram = "";
    let selectedStyle = "classique"; // Style par défaut

    // Définition des styles prédéfinis
    const styles = {
        classique: {
            headerBg: "#ffffff",
            headerColor: "#000000",
            bodyBg: "#f0f0f0",
            bodyColor: "#000000",
            borderRadius: "0px",
            separator: true,
            boxShadow: "none",
            border: "1px solid #000000",
            pattern: "none",
        },
        moderne: {
            headerBg: "#ffffff",
            headerColor: "#000000",
            bodyBg: "#ffffff",
            bodyColor: "#000000",
            borderRadius: "0px",
            separator: false,
            boxShadow: "0 4px 12px rgba(0, 0, 0, 0.1)",
            border: "none",
            pattern: "geometric",
        },
        professionnel: {
            headerBg: "#2c3e50",
            headerColor: "#ecf0f1",
            bodyBg: "#ecf0f1",
            bodyColor: "#2c3e50",
            borderRadius: "6px",
            separator: true,
            boxShadow: "0 4px 12px rgba(0, 0, 0, 0.3)",
            border: "none",
            pattern: "none",
        },
    };

    // Mise à jour du style courant en fonction de la sélection
    $: currentStyle = styles[selectedStyle];
</script>

<main>
    <h1>Générateur de Carte de Visite</h1>
    <p class="subtitle">Créez une carte élégante en quelques secondes</p>

    <div class="container">
        <!-- Formulaire pour modifier les informations et choisir le style -->
        <div class="formulaire">
            <div class="input-group">
                <label for="nom">Nom</label>
                <input
                    id="nom"
                    type="text"
                    bind:value={nom}
                    placeholder="Sabrina Smith"
                />
            </div>
            <div class="input-group">
                <label for="profession">Profession</label>
                <input
                    id="profession"
                    type="text"
                    bind:value={profession}
                    placeholder="Nutricioniste"
                />
            </div>
            <div class="input-group">
                <label for="email">Email</label>
                <input
                    id="email"
                    type="email"
                    bind:value={email}
                    placeholder="sabrina@the-company.com"
                />
            </div>
            <div class="input-group">
                <label for="telephone">Téléphone</label>
                <input
                    id="telephone"
                    type="tel"
                    bind:value={telephone}
                    placeholder="022 - 786 127 890"
                />
            </div>

            <div class="input-group">
                <label for="facebook">Facebook</label>
                <input
                    id="facebook"
                    type="text"
                    bind:value={facebook}
                    placeholder="/fb_fanpage"
                />
            </div>
            <div class="input-group">
                <label for="twitter">Twitter</label>
                <input
                    id="twitter"
                    type="text"
                    bind:value={twitter}
                    placeholder="@tw_account"
                />
            </div>
            <div class="input-group">
                <label for="instagram">Instagram</label>
                <input
                    id="instagram"
                    type="text"
                    bind:value={instagram}
                    placeholder="@ig_account"
                />
            </div>

            <!-- Sélection du style -->
            <div class="input-group">
                <label for="style">Choisissez un style</label>
                <select id="style" bind:value={selectedStyle}>
                    <option value="classique">Classique</option>
                    <option value="moderne">Moderne</option>
                    <option value="professionnel">Professionnel</option>
                </select>
            </div>
        </div>

        <!-- Carte de visite qui affiche le style sélectionné -->
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
      "
        >
            {#if currentStyle.pattern === "geometric"}
                <div class="pattern-container">
                    <!-- Partie supérieure -->
                    <div class="top-left-curve"></div>
                    <div class="top-right-curve"></div>

                    <!-- Partie centrale -->
                    <div class="center-square"></div>
                    <div class="center-bordeaux-circle"></div>
                    <div class="plume-bleu"></div>
                    <div class="quart-cercle"></div>
                    <div class="demi-cercle"></div>

                    <!-- Partie inférieure -->
                    <div class="bottom-right-curve"></div>
                    <div class="bottom-green-shape"></div>
                </div>
            {/if}

            <div class="carte-content">
                <div class="carte-header">
                    <h2>{nom || "Sabrina Smith"}</h2>
                    <h3>{profession || "Nutricioniste"}</h3>
                </div>

                {#if currentStyle.separator}
                    <hr />
                {/if}

                <div class="carte-body">
                    <div class="contact-info">
                        <p>
                            <span class="icon">✉️</span>
                            {email || "sabrina@the-company.com"}
                        </p>
                        <p>
                            <span class="icon">📞</span>
                            {telephone || "022 - 786 127 890"}
                        </p>
                    </div>

                    {#if currentStyle.pattern === "geometric"}
                        <div class="social-links">
                            <p>
                                <span class="social-icon fb-icon">📘</span>
                                {facebook || "/fb_fanpage"}
                            </p>
                            <p>
                                <span class="social-icon tw-icon">🐦</span>
                                {twitter || "@tw_account"}
                            </p>
                            {#if instagram}
                                <p>
                                    <span class="social-icon ig-icon">📷</span>
                                    {instagram}
                                </p>
                            {/if}
                        </div>
                    {/if}
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
        max-width: 900px;
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
        flex-wrap: wrap;
    }

    .formulaire {
        flex: 1;
        min-width: 300px;
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
        width: 100%;
        padding: 10px;
        font-size: 1rem;
        border: 1px solid #7f8c8d;
        border-radius: 8px;
        outline: none;
        transition: border-color 0.3s;
        background-color: #34495e;
        color: #ecf0f1;
    }

    input:focus,
    select:focus {
        border-color: #3498db;
    }

    .carte:hover {
        transform: translateY(-5px);
    }

    .carte-content {
        position: relative;
        z-index: 2;
        height: 100%;
        display: flex;
        flex-direction: column;
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

    /* Formes géométriques pour le style moderne */
    .top-left-curve {
        position: absolute;
        top: -80px;
        left: -80px;
        width: 160px;
        height: 160px;
        border-radius: 50%;
        background-color: #93a8ac; /* violet */
    }

    .top-right-curve {
        position: absolute;
        top: -70px;
        right: -70px;
        width: 140px;
        height: 140px;
        border-radius: 50%;
        background-color: #93a8ac; /* violet */
    }

    .plume-bleu {
        position: absolute;
        width: 120px;
        height: 50px;
        background-color: #d7ceb2;
        border-radius: 100px 0px 100px 0;
        top: 0;
        right: 0px;
    }

    .demi-cercle {
        position: absolute;
        width: 70px;
        height: 40px;
        background-color: #d49e21;
        border-radius: 0 0 100px 100px;
        top: 100px;
        right: 40px;
    }

    .quart-cercle {
        position: absolute;
        width: 80px;
        height: 80px;
        background-color: #ff0000;
        border-radius: 80px 0 0 0;
        bottom: 0;
        left: 10px;
    }

    .center-square {
        position: absolute;
        top: 66px;
        right: 18%;
        width: 35px;
        height: 35px;
        background-color: #add8c7; /* vert clair */
    }

    .carte-header h2 {
        margin: 0;
        font-size: 2.2rem;
        font-weight: 900;
        color: #000;
    }

    .carte-header h3 {
        margin: 0;
        font-size: 1.4rem;
        font-weight: normal;
        color: #000;
    }

    .carte {
        position: relative; /* Référence pour les éléments absolus */
        flex: 1;
        min-width: 300px;
        width: 450px;
        height: 250px;
        background: var(--body-bg);
        border-radius: var(--border-radius);
        box-shadow: var(--box-shadow);
        border: var(--border);
        overflow: hidden;
        transition: transform 0.3s;
    }

    .carte-header {
        margin: 100px 0 10px 30px; /* Déplace avec margin */
        text-align: left;
    }

    .carte-body {
        padding: 20px 0 15px 38px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        height: 100%;
    }

    .social-links {
        position: absolute;
        right: 40px;
        bottom: 20px; /* Ou top: 20px si tu préfères en haut */
        text-align: right;
    }

    .contact-info {
        margin-top: 10px;
    }

    .carte-body p {
        margin: 8px 0;
        font-size: 1rem;
        color: #000;
    }

    .icon {
        margin-right: 15px;
        color: #333;
        font-size: 1.2rem;
        display: inline-block;
        width: 20px;
        vertical-align: middle;
    }

    .social-icon {
        margin-right: 12px;
        font-size: 1rem;
        display: inline-block;
        width: 20px;
        vertical-align: middle;
    }

    hr {
        border: none;
        border-top: 1px solid #bdc3c7;
        margin: 10px 0;
    }

    @media (max-width: 600px) {
        .container {
            flex-direction: column;
            align-items: center;
        }
    }
</style>
