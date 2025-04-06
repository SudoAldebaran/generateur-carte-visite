<script>
    import jsPDF from "jspdf";
    import html2canvas from "html2canvas";
    import '../style.css'

    let cardHistory = []; // Stocke les cartes générées
    let enlargedCard = null; // Carte actuellement agrandie

    // Charge l'historique depuis localStorage si dispo
    if (typeof window !== 'undefined') {
        const storedHistory = localStorage.getItem('cardHistory');
        if (storedHistory) {
            cardHistory = JSON.parse(storedHistory);
        }
    }

    // Sauvegarde dans localStorage à chaque changement
    $: if (typeof window !== 'undefined' && cardHistory) {
        localStorage.setItem('cardHistory', JSON.stringify(cardHistory));
    }

    // Convertit un blob en base64
    async function blobToBase64(blob) {
        return new Promise((resolve) => {
            const reader = new FileReader();
            reader.onloadend = () => resolve(reader.result.split(',')[1]);
            reader.readAsDataURL(blob);
        });
    }

    // Convertit base64 en blob pour téléchargement
    async function base64ToBlob(base64) {
        const response = await fetch(`data:application/pdf;base64,${base64}`);
        return await response.blob();
    }

    // Génère le PDF et met à jour l'historique
    async function generatePDF() {
        const carteElement = document.querySelector(".carte");
        const canvas = await html2canvas(carteElement, { scale: 2, useCORS: true });
        const imgData = canvas.toDataURL("image/png");
        const pdf = new jsPDF({ orientation: "landscape", unit: "px", format: [canvas.width, canvas.height] });
        pdf.addImage(imgData, "PNG", 0, 0, canvas.width, canvas.height);
        
        const pdfBlob = pdf.output('blob');
        const pdfBase64 = await blobToBase64(pdfBlob);

        const cardData = {
            id: Date.now(),
            titre: titre || "",
            nom: nom || "exemple",
            profession: profession || "",
            email: email || "",
            telephone: telephone || "",
            style: selectedStyle,
            pdfBlobBase64: pdfBase64,
            preview: imgData
        };

        cardHistory = [...cardHistory, cardData];
        pdf.save(`Carte-de-visite-${nom || "exemple"}.pdf`);
    }

    async function downloadExistingCard(card) {
        if (card.pdfBlobBase64) {
            const blob = await base64ToBlob(card.pdfBlobBase64);
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `Carte-de-visite-${card.nom}.pdf`;
            link.click();
        }
    }

    function enlargeCard(card) {
        enlargedCard = card;
    }

    function closeEnlargedCard() {
        enlargedCard = null;
    }

    // Variables pour le formulaire
    let titre = "";
    let nom = "";
    let email = "";
    let telephone = "";
    let profession = "";
    let selectedStyle = "classique";

    // Styles des cartes
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
</script>

<main>
    <h1>Générateur de Carte de Visite</h1>
    <p class="subtitle">Créez une carte élégante en quelques secondes</p>

    <!-- Formulaire et aperçu -->
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
                <input id="nom" type="text" bind:value={nom} placeholder="Jean Yves" />
            </div>
            <div class="input-group">
                <label for="profession">Profession</label>
                <input id="profession" type="text" bind:value={profession} placeholder="Professeur" />
            </div>
            <div class="input-group">
                <label for="email">Adresse mail</label>
                <input id="email" type="email" bind:value={email} placeholder="jean@entreprise.fr" />
            </div>
            <div class="input-group">
                <label for="telephone">Téléphone</label>
                <input id="telephone" type="tel" bind:value={telephone} placeholder="06 45 55 65 75" />
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
                    <h2>{titre ? titre + " " : ""}{nom || "Jean Yves"}</h2>
                    <h3>{profession || "Professeur"}</h3>
                </div>
                {#if currentStyle.separator}
                    <hr
                        class:separator-classique={selectedStyle === "classique"}
                        class:separator-professionnel={selectedStyle === "professionnel"}
                    />
                {/if}
                <div class="carte-body">
                    <div class="contact-info">
                        <p>{email || "jean@entreprise.fr"}</p>
                        <p>{telephone || "06 45 55 65 75"}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <button class="download-btn" on:click={generatePDF}>Télécharger en PDF</button>

    <!-- Historique des cartes -->
    {#if cardHistory.length > 0}
        <div class="history-section">
            <h2>Cartes précédemment générées</h2>
            <div class="history-grid">
                {#each cardHistory as card (card.id)}
                    <div class="history-item">
                        <img 
                            src={card.preview} 
                            alt="Aperçu de la carte" 
                            class="history-preview"
                        />
                        <div class="history-info">
                            <p><strong>{card.titre} {card.nom}</strong></p>
                            <p>{card.profession}</p>
                            <button 
                                class="history-preview-btn" 
                                on:click={() => enlargeCard(card)}
                            >
                                Prévisualiser
                            </button>
                            <button 
                                class="history-download-btn" 
                                on:click={() => downloadExistingCard(card)}
                            >
                                Télécharger à nouveau
                            </button>
                        </div>
                    </div>
                {/each}
            </div>
        </div>
    {/if}

    <!-- Modale pour aperçu agrandi -->
    {#if enlargedCard}
        <div 
            class="enlarged-modal" 
            role="dialog" 
            aria-label="Prévisualisation de la carte agrandie"
            on:click={closeEnlargedCard}
            on:keydown={(e) => { if (e.key === 'Escape') closeEnlargedCard(); }}
            tabindex="0"
        >
            <div 
                class="enlarged-card" 
                role="presentation"
                on:click|stopPropagation
                on:keydown|stopPropagation
            >
                <img 
                    src={enlargedCard.preview} 
                    alt="Carte agrandie de {enlargedCard.titre} {enlargedCard.nom}" 
                    class="enlarged-preview"
                />
                <button 
                    class="close-btn" 
                    on:click={closeEnlargedCard}
                    on:keydown={(e) => { if (e.key === 'Enter' || e.key === ' ') closeEnlargedCard(); }}
                >
                    Fermer
                </button>
            </div>
        </div>
    {/if}
</main>