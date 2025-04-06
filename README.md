### Rayan Magniac, Maitriya Sramaner, Fady Mikhael

# Générateur de Carte de Visite

Ce projet est une application web développée avec **Svelte** permettant de créer des cartes de visite personnalisées et élégantes. Les utilisateurs peuvent saisir leurs informations (titre, nom, profession, email, téléphone) et choisir parmi trois styles prédéfinis : Classique, Moderne et Professionnel. Une fois la carte configurée, elle peut être téléchargée au format PDF.

## Utilisation
1. Remplissez les champs du formulaire (titre, nom, profession, email, téléphone).
2. Sélectionnez un style dans la liste déroulante.
3. Visualisez l'aperçu de la carte en temps réel.
4. Cliquez sur "Télécharger en PDF" pour sauvegarder votre carte.
5. Possiblité de pré-visualiser les anciennes cartes et de les télécharger

## Prérequis
Avant de commencer, assurez-vous d'avoir les outils suivants installés sur votre machine :
- **Node.js** (version 16 ou supérieure recommandée)
- **npm** (inclus avec Node.js)

## Installation

1. **Installer les dépendances** :
   Utilisez npm pour installer les packages nécessaires :
   ```bash
   npm install
   ```

2. **Dépendances principales** :
   Les dépendances suivantes seront automatiquement installées via `npm install` : 
   - `jspdf` : Pour générer le fichier PDF.
   - `html2canvas` : Pour convertir la carte en image avant de l'intégrer dans le PDF.

   Installez-les individuellement si nécessaire :
   ```bash
   npm install jspdf html2canvas
   ```

3. **Lancer le projet en mode développement** :
   ```bash
   npm run dev
   ```
   Cela démarrera un serveur local (généralement sur `http://localhost:5173`)