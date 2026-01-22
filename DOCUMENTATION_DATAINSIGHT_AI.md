# Documentation Complète - DataInsight AI

## Analyse Intelligente de Données 100% Local

---

## Table des matières

1. [Présentation du projet](#1-présentation-du-projet)
2. [Architecture technique](#2-architecture-technique)
3. [Structure du fichier HTML](#3-structure-du-fichier-html)
4. [Les variables CSS (Thèmes)](#4-les-variables-css-thèmes)
5. [Les composants visuels](#5-les-composants-visuels)
6. [Le JavaScript - Variables globales](#6-le-javascript---variables-globales)
7. [Fonctionnalités détaillées](#7-fonctionnalités-détaillées)
8. [Les algorithmes de prédiction](#8-les-algorithmes-de-prédiction)
9. [La détection d'anomalies](#9-la-détection-danomalies)
10. [L'assistant IA (NLP)](#10-lassistant-ia-nlp)
11. [Système de traduction FR/EN](#11-système-de-traduction-fren)
12. [Guide de reproduction pas à pas](#12-guide-de-reproduction-pas-à-pas)
13. [Améliorations possibles](#13-améliorations-possibles)

---

## 1. Présentation du projet

### Objectif
DataInsight AI est une application web d'analyse de données qui fonctionne **entièrement dans le navigateur**, sans serveur backend ni API externe. Elle permet d'analyser des fichiers CSV avec :

- Statistiques descriptives automatiques
- Détection d'anomalies (3 méthodes)
- Prédictions (3 algorithmes)
- Visualisations interactives (6 types de graphiques)
- Assistant IA en langage naturel

### Technologies utilisées
- **HTML5** : Structure de la page
- **CSS3** : Styles avec variables CSS pour les thèmes
- **JavaScript ES6** : Logique applicative
- **Chart.js 4.4.0** : Bibliothèque de graphiques (CDN)

### Avantages
- ✅ Aucune installation requise
- ✅ Fonctionne hors ligne (après chargement initial)
- ✅ Données confidentielles (rien n'est envoyé sur internet)
- ✅ Gratuit et open source

---

## 2. Architecture technique

```
┌─────────────────────────────────────────────────────────────┐
│                      index.html                              │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │   <style>   │  │   <body>    │  │     <script>        │ │
│  │             │  │             │  │                     │ │
│  │ - Variables │  │ - Header    │  │ - Variables         │ │
│  │   CSS       │  │ - Upload    │  │ - Event Listeners   │ │
│  │ - Thèmes    │  │ - Results   │  │ - Parsing CSV       │ │
│  │ - Classes   │  │ - Tabs      │  │ - Statistiques      │ │
│  │             │  │ - Footer    │  │ - Prédictions       │ │
│  │             │  │             │  │ - Anomalies         │ │
│  │             │  │             │  │ - NLP               │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │   Chart.js CDN  │
                    │   (externe)     │
                    └─────────────────┘
```

---

## 3. Structure du fichier HTML

### 3.1 En-tête (Head)
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DataInsight AI - Analyse Intelligente de Données</title>
    <style>
        /* CSS ici */
    </style>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
</head>
```

**Explications :**
- `charset="UTF-8"` : Supporte les caractères français (accents)
- `viewport` : Rend la page responsive (mobile-friendly)
- Chart.js : Chargé depuis un CDN (Content Delivery Network)

### 3.2 Structure du Body
```html
<body>
    <!-- 1. Bouton de thème (fixe en haut à droite) -->
    <div class="theme-toggle" id="themeToggle">...</div>

    <!-- 2. En-tête avec titre -->
    <header class="header">...</header>

    <!-- 3. Contenu principal -->
    <main class="container">
        <!-- Section d'upload -->
        <section id="uploadSection">...</section>

        <!-- Section de chargement -->
        <section id="loadingSection">...</section>

        <!-- Section des résultats (4 onglets) -->
        <section id="resultsSection">...</section>
    </main>

    <!-- 4. Pied de page -->
    <footer class="footer">...</footer>

    <!-- 5. JavaScript -->
    <script>
        // Code JS ici
    </script>
</body>
```

---

## 4. Les variables CSS (Thèmes)

### 4.1 Définition des variables
```css
:root {
    /* Couleurs principales */
    --primary: #667eea;      /* Violet/bleu - couleur principale */
    --secondary: #764ba2;    /* Violet foncé - couleur secondaire */
    --success: #48bb78;      /* Vert - succès/positif */
    --warning: #ed8936;      /* Orange - avertissement */
    --danger: #f56565;       /* Rouge - erreur/danger */

    /* Couleurs de fond et texte (mode clair) */
    --bg-main: #f7fafc;      /* Fond principal */
    --bg-card: #ffffff;      /* Fond des cartes */
    --text-main: #2d3748;    /* Texte principal */
    --text-muted: #a0aec0;   /* Texte secondaire */
    --border-color: #e2e8f0; /* Bordures */
}
```

### 4.2 Thème sombre
```css
[data-theme="dark"] {
    --bg-main: #1a202c;      /* Fond sombre */
    --bg-card: #2d3748;      /* Cartes sombres */
    --text-main: #f7fafc;    /* Texte clair */
    --text-muted: #a0aec0;   /* Texte secondaire */
    --border-color: #4a5568; /* Bordures sombres */
}
```

### 4.3 Comment fonctionne le changement de thème
```javascript
// Quand on clique sur le toggle
themeToggle.addEventListener('click', () => {
    const currentTheme = document.documentElement.getAttribute('data-theme');
    if (currentTheme === 'dark') {
        // Passer en mode clair
        document.documentElement.removeAttribute('data-theme');
        localStorage.setItem('theme', 'light');
    } else {
        // Passer en mode sombre
        document.documentElement.setAttribute('data-theme', 'dark');
        localStorage.setItem('theme', 'dark');
    }
});
```

**Principe :**
1. On ajoute `data-theme="dark"` à `<html>`
2. Le CSS avec `[data-theme="dark"]` s'applique
3. Toutes les variables sont redéfinies
4. Les éléments utilisant `var(--variable)` changent automatiquement

---

## 5. Les composants visuels

### 5.1 Les cartes (Cards)
```css
.card {
    background: var(--bg-card);      /* Utilise la variable */
    border-radius: 12px;              /* Coins arrondis */
    padding: 2rem;                    /* Espacement interne */
    box-shadow: 0 2px 8px rgba(0,0,0,0.1); /* Ombre légère */
    margin-bottom: 1.5rem;
    transition: background 0.3s;      /* Animation douce */
}
```

### 5.2 Zone d'upload (Drag & Drop)
```css
.upload-area {
    border: 3px dashed var(--primary); /* Bordure en pointillés */
    border-radius: 12px;
    padding: 3rem;
    text-align: center;
    cursor: pointer;
    transition: all 0.3s;
}

.upload-area:hover {
    background: rgba(102, 126, 234, 0.05); /* Fond légèrement coloré */
    border-color: var(--secondary);
}
```

### 5.3 Les onglets (Tabs)
```css
.tabs {
    display: flex;
    gap: 0.5rem;
    border-bottom: 2px solid var(--border-color);
}

.tab {
    background: none;
    border: none;
    padding: 1rem 1.5rem;
    cursor: pointer;
    border-bottom: 3px solid transparent; /* Ligne invisible */
}

.tab.active {
    color: var(--primary);
    border-bottom-color: var(--primary); /* Ligne visible */
}
```

### 5.4 Les boutons
```css
.btn {
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.3s;
}

.btn-primary {
    /* Dégradé de couleurs */
    background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
    color: white;
}

.btn-primary:hover {
    transform: translateY(-2px);  /* Monte légèrement */
    box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4); /* Ombre */
}
```

---

## 6. Le JavaScript - Variables globales

```javascript
// Données du fichier CSV
let csvData = null;        // Tableau d'objets [{col1: val1, col2: val2}, ...]
let headers = [];          // Noms des colonnes ["col1", "col2", ...]
let numericColumns = [];   // Colonnes numériques seulement

// Graphiques
let currentChart = null;       // Instance Chart.js actuelle
let currentChartType = 'line'; // Type de graphique sélectionné
let currentChartColumn = null; // Colonne affichée dans le graphique

// Statistiques calculées
let allStats = {};  // {colonne: {mean, median, std, min, max, ...}}
```

---

## 7. Fonctionnalités détaillées

### 7.1 Parsing du fichier CSV

```javascript
function parseCSV(text) {
    // 1. Découper en lignes
    const lines = text.trim().split('\n');

    // 2. Extraire les en-têtes (première ligne)
    // Supporte virgule (,) ou point-virgule (;) comme séparateur
    headers = lines[0].split(/[,;]/).map(h => h.trim().replace(/"/g, ''));

    // 3. Initialiser les tableaux
    csvData = [];
    numericColumns = [];

    // 4. Parcourir chaque ligne de données
    for (let i = 1; i < lines.length; i++) {
        const values = lines[i].split(/[,;]/).map(v => v.trim().replace(/"/g, ''));

        // Vérifier que la ligne a le bon nombre de colonnes
        if (values.length === headers.length) {
            const row = {};
            headers.forEach((h, idx) => {
                const val = values[idx];
                // Convertir en nombre si possible
                row[h] = isNaN(parseFloat(val)) ? val : parseFloat(val);
            });
            csvData.push(row);
        }
    }

    // 5. Identifier les colonnes numériques
    // Une colonne est numérique si >50% de ses valeurs sont des nombres
    headers.forEach(h => {
        const values = csvData.map(row => row[h])
                              .filter(v => typeof v === 'number' && !isNaN(v));
        if (values.length > csvData.length * 0.5) {
            numericColumns.push(h);
        }
    });
}
```

**Exemple :**
```
Entrée CSV:
nom,age,salaire
Alice,25,3000
Bob,30,3500

Résultat:
headers = ["nom", "age", "salaire"]
csvData = [
    {nom: "Alice", age: 25, salaire: 3000},
    {nom: "Bob", age: 30, salaire: 3500}
]
numericColumns = ["age", "salaire"]
```

### 7.2 Calcul des statistiques

```javascript
// Pour chaque colonne numérique
numericColumns.forEach(col => {
    // Extraire les valeurs numériques valides
    const values = csvData.map(r => r[col])
                          .filter(v => typeof v === 'number' && !isNaN(v));

    if (values.length > 0) {
        // MOYENNE : somme / nombre d'éléments
        const mean = values.reduce((a, b) => a + b, 0) / values.length;

        // MÉDIANE : valeur du milieu quand triées
        const sorted = [...values].sort((a, b) => a - b);
        const median = sorted[Math.floor(sorted.length / 2)];

        // ÉCART-TYPE : mesure de dispersion
        const std = Math.sqrt(
            values.reduce((sum, v) => sum + Math.pow(v - mean, 2), 0) / values.length
        );

        // MIN et MAX
        const min = Math.min(...values);
        const max = Math.max(...values);

        // QUARTILES (pour IQR)
        const q1 = sorted[Math.floor(sorted.length * 0.25)]; // 25%
        const q3 = sorted[Math.floor(sorted.length * 0.75)]; // 75%
        const iqr = q3 - q1; // Écart interquartile

        // Stocker les stats
        allStats[col] = { mean, median, std, min, max, q1, q3, iqr, values };
    }
});
```

### 7.3 Calcul des corrélations

```javascript
function calculateCorrelation(col1, col2) {
    // 1. Récupérer les paires de valeurs valides
    const pairs = csvData.filter(r =>
        typeof r[col1] === 'number' && typeof r[col2] === 'number'
    );

    if (pairs.length < 3) return 0; // Pas assez de données

    const x = pairs.map(r => r[col1]);
    const y = pairs.map(r => r[col2]);
    const n = x.length;

    // 2. Calculer les sommes nécessaires
    const sumX = x.reduce((a, b) => a + b, 0);
    const sumY = y.reduce((a, b) => a + b, 0);
    const sumXY = x.reduce((sum, xi, i) => sum + xi * y[i], 0);
    const sumX2 = x.reduce((sum, xi) => sum + xi * xi, 0);
    const sumY2 = y.reduce((sum, yi) => sum + yi * yi, 0);

    // 3. Formule de corrélation de Pearson
    const numerateur = n * sumXY - sumX * sumY;
    const denominateur = Math.sqrt(
        (n * sumX2 - sumX * sumX) * (n * sumY2 - sumY * sumY)
    );

    return denominateur === 0 ? 0 : numerateur / denominateur;
}
```

**Interprétation de la corrélation :**
- **+1** : Corrélation positive parfaite (quand X augmente, Y augmente)
- **0** : Aucune corrélation linéaire
- **-1** : Corrélation négative parfaite (quand X augmente, Y diminue)
- **|r| > 0.7** : Corrélation forte
- **|r| > 0.5** : Corrélation modérée
- **|r| > 0.3** : Corrélation faible

---

## 8. Les algorithmes de prédiction

### 8.1 Régression linéaire

**Principe :** Trouver la droite Y = aX + b qui passe au mieux par les points.

```javascript
function predictLinearRegression(values, steps) {
    const n = values.length;
    let sumX = 0, sumY = 0, sumXY = 0, sumX2 = 0;

    // X = index (0, 1, 2, ...), Y = valeur
    for (let i = 0; i < n; i++) {
        sumX += i;
        sumY += values[i];
        sumXY += i * values[i];
        sumX2 += i * i;
    }

    // Formule des moindres carrés
    // pente (a) = (n*ΣXY - ΣX*ΣY) / (n*ΣX² - (ΣX)²)
    const slope = (n * sumXY - sumX * sumY) / (n * sumX2 - sumX * sumX);

    // ordonnée à l'origine (b) = (ΣY - a*ΣX) / n
    const intercept = (sumY - slope * sumX) / n;

    // Générer les prédictions futures
    const predictions = [];
    for (let i = 0; i < steps; i++) {
        // Y = a * X + b, où X = n + i (point futur)
        predictions.push(slope * (n + i) + intercept);
    }

    return {
        predictions,
        slope,      // Pente de la droite
        intercept,  // Ordonnée à l'origine
        trend: slope > 0 ? 'hausse' : slope < 0 ? 'baisse' : 'stable'
    };
}
```

**Exemple visuel :**
```
Valeurs : [10, 12, 15, 18, 20]

20 |           *
18 |         *   ----→ Prédictions
15 |       *
12 |     *
10 |   *
   +---------------
     0  1  2  3  4  5  6  7
```

### 8.2 Moyenne mobile simple

**Principe :** Utiliser la moyenne des dernières valeurs pour prédire.

```javascript
function predictMovingAverage(values, steps, windowSize = 5) {
    // Ajuster la fenêtre si pas assez de données
    windowSize = Math.min(windowSize, Math.floor(values.length / 2));

    // Prendre les dernières valeurs
    const lastValues = values.slice(-windowSize);

    // Calculer leur moyenne
    const avg = lastValues.reduce((a, b) => a + b, 0) / lastValues.length;

    // Calculer la tendance récente
    const recentTrend = (values[values.length - 1] - values[values.length - windowSize]) / windowSize;

    // Générer les prédictions
    const predictions = [];
    for (let i = 0; i < steps; i++) {
        predictions.push(avg + recentTrend * i);
    }

    return { predictions, average: avg, trend: ... };
}
```

**Exemple :**
```
Valeurs : [10, 12, 15, 18, 20]
Fenêtre = 3 dernières valeurs : [15, 18, 20]
Moyenne = (15 + 18 + 20) / 3 = 17.67
Tendance = (20 - 15) / 3 = 1.67 par point

Prédictions : 17.67, 19.34, 21.01, ...
```

### 8.3 Moyenne mobile exponentielle (EMA)

**Principe :** Donner plus de poids aux valeurs récentes.

```javascript
function predictEMA(values, steps, alpha = 0.3) {
    // Initialiser l'EMA avec la première valeur
    let ema = values[0];

    // Calculer l'EMA progressivement
    // EMA = α * valeur_actuelle + (1 - α) * EMA_précédent
    for (let i = 1; i < values.length; i++) {
        ema = alpha * values[i] + (1 - alpha) * ema;
    }

    // Calculer la tendance sur les 5 dernières valeurs
    const recentValues = values.slice(-5);
    const trend = (recentValues[recentValues.length - 1] - recentValues[0]) / recentValues.length;

    // Générer les prédictions
    const predictions = [];
    let currentEma = ema;
    for (let i = 0; i < steps; i++) {
        currentEma = currentEma + trend;
        predictions.push(currentEma);
    }

    return { predictions, lastEma: ema, trend: ... };
}
```

**Paramètre α (alpha) :**
- α proche de 1 : réagit vite aux changements récents
- α proche de 0 : lisse davantage, moins sensible aux fluctuations

---

## 9. La détection d'anomalies

### 9.1 Méthode Z-Score

**Principe :** Une valeur est anormale si elle est à plus de 3 écarts-types de la moyenne.

```javascript
// Calculer le Z-Score pour chaque valeur
const zscoreAnomalies = values.map((v, i) => ({
    value: v,
    index: i,
    zscore: (v - mean) / std  // Z = (valeur - moyenne) / écart-type
})).filter(item => Math.abs(item.zscore) > 3);  // |Z| > 3 = anomalie
```

**Interprétation :**
```
Distribution normale :
                    │
      ▄▄▄▄██████▄▄▄▄│
   ▄██              ██▄
 ▄█                    █▄
─────────────────────────────
     -3σ  -2σ  -σ  μ  +σ +2σ +3σ

- 68% des données sont entre -1σ et +1σ
- 95% des données sont entre -2σ et +2σ
- 99.7% des données sont entre -3σ et +3σ

→ Une valeur au-delà de ±3σ est très rare (0.3%)
```

### 9.2 Méthode IQR (Interquartile Range)

**Principe :** Utiliser les quartiles pour définir les bornes normales.

```javascript
// Calculer les quartiles
const q1 = sorted[Math.floor(sorted.length * 0.25)];  // 25%
const q3 = sorted[Math.floor(sorted.length * 0.75)];  // 75%
const iqr = q3 - q1;

// Définir les bornes
const lowerBound = q1 - 1.5 * iqr;  // Borne inférieure
const upperBound = q3 + 1.5 * iqr;  // Borne supérieure

// Détecter les anomalies
const iqrAnomalies = values.filter(v => v < lowerBound || v > upperBound);
```

**Représentation (Box Plot) :**
```
           Anomalie
              │
     ┌────────┼────────┐
─────┤   ▓▓▓▓▓█▓▓▓▓▓   ├─────
     └────────┼────────┘
              │
     Borne    │    Borne
     inf.     │    sup.
              │
  Q1-1.5*IQR Q1  Q3  Q3+1.5*IQR
```

### 9.3 Méthode Isolation Forest (simplifiée)

**Principe :** Les anomalies sont des points "isolés" loin de la majorité.

```javascript
// Version simplifiée : écart à la médiane
const threshold = std * 2.5;
const isolationAnomalies = values.filter(v =>
    Math.abs(v - median) > threshold
);
```

---

## 10. L'assistant IA (NLP)

### 10.1 Normalisation du texte

```javascript
function normalizeText(text) {
    return text.toLowerCase()           // minuscules
        .normalize('NFD')               // décomposer les accents
        .replace(/[\u0300-\u036f]/g, '') // supprimer les accents
        .trim();                        // supprimer espaces
}

// Exemple :
// "Créé" → "cree"
// "Prédis" → "predis"
// "Détecte" → "detecte"
```

### 10.2 Détection des intentions

```javascript
function processNLPQuery() {
    const query = normalizeText(rawQuery);

    // 1. Graphique ?
    if (query.includes('graphique') || query.includes('graph')) {
        const chartType = detectChartType(query);
        const column = detectColumn(query);
        updateChart(chartType, column);
    }

    // 2. Prédiction ?
    else if (query.includes('predi') || query.includes('prevoir')) {
        const column = detectColumn(query);
        const steps = extractNumber(query) || 10;
        generatePredictions();
    }

    // 3. Anomalies ?
    else if (query.includes('anomalie')) {
        displayAnomalies('all');
    }

    // 4. Corrélations ?
    else if (query.includes('correlation')) {
        // Afficher les corrélations
    }

    // 5. Statistiques ?
    else if (query.includes('statistique') || query.includes('analyse')) {
        // Afficher les stats
    }
}
```

### 10.3 Détection du type de graphique

```javascript
function detectChartType(query) {
    if (query.includes('barre')) return 'bar';
    if (query.includes('camembert')) return 'pie';
    if (query.includes('anneau')) return 'doughnut';
    if (query.includes('radar')) return 'radar';
    if (query.includes('polaire')) return 'polarArea';
    return 'line';  // Par défaut
}
```

### 10.4 Détection de la colonne

```javascript
function detectColumn(query) {
    // Chercher si une colonne est mentionnée
    for (const col of numericColumns) {
        if (query.includes(col.toLowerCase())) {
            return col;
        }
    }
    // Sinon, utiliser la première colonne numérique
    return numericColumns[0] || null;
}
```

### 10.5 Extraction de nombre

```javascript
function extractNumber(query) {
    const match = query.match(/(\d+)/);  // Chercher un nombre
    return match ? parseInt(match[1]) : null;
}

// Exemples :
// "pour les 5 prochains mois" → 5
// "10 points" → 10
```

---

## 11. Système de traduction FR/EN

### Présentation
L'application dispose d'un système de traduction bilingue (français/anglais) permettant de basculer l'interface complète d'une langue à l'autre.

### Bouton de langue
```html
<button class="lang-btn" onclick="toggleLanguage()">
    <span class="flag">🇬🇧</span><span id="langText">English</span>
</button>
```

Le bouton est positionné en haut à droite de l'écran (position fixed) et affiche :
- 🇬🇧 English (quand l'interface est en français)
- 🇫🇷 Français (quand l'interface est en anglais)

### Structure des traductions
```javascript
const translations = {
    fr: {
        langText: 'English',
        langFlag: '🇬🇧',
        uploadTitle: '1. Charger vos donnees',
        // ... autres clés
    },
    en: {
        langText: 'Français',
        langFlag: '🇫🇷',
        uploadTitle: '1. Load your data',
        // ... autres clés
    }
};
```

### Marquage des éléments traduisibles
Les éléments HTML utilisent des attributs `data-i18n` :
```html
<h2 data-i18n="uploadTitle">1. Charger vos donnees</h2>
<input data-i18n-placeholder="inputPlaceholder" placeholder="...">
```

### Fonction de traduction
```javascript
function applyTranslations() {
    const t = translations[currentLang];

    // Traduire les éléments avec data-i18n
    document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.getAttribute('data-i18n');
        if (t[key]) el.innerHTML = t[key];
    });

    // Traduire les placeholders
    document.querySelectorAll('[data-i18n-placeholder]').forEach(el => {
        const key = el.getAttribute('data-i18n-placeholder');
        if (t[key]) el.placeholder = t[key];
    });
}
```

### Fonction utilitaire t()
Pour les messages dynamiques générés en JavaScript :
```javascript
function t(key) {
    return translations[currentLang][key] || key;
}

// Utilisation :
respond(t('barChartResponse'));
alert(t('pleaseSelectCSV'));
```

### Persistance de la préférence
La langue choisie est sauvegardée dans `localStorage` :
```javascript
let currentLang = localStorage.getItem('datainsight-lang') || 'fr';

function toggleLanguage() {
    currentLang = currentLang === 'fr' ? 'en' : 'fr';
    localStorage.setItem('datainsight-lang', currentLang);
    applyTranslations();
}
```

### Détection bilingue des commandes
L'IA comprend les commandes en français ET en anglais :
```javascript
// Détecte "barre" (FR) ou "bar" (EN)
if (query.includes('barre') || query.includes('bar')) {
    createChart('bar');
}

// Détecte "anomalie" (FR) ou "anomaly" (EN)
if (query.includes('anomalie') || query.includes('anomaly')) {
    detectAnomalies();
}
```

### Style du bouton de langue
```css
.lang-btn {
    position: fixed;
    top: 20px;
    right: 20px;
    padding: 10px 20px;
    background: white;
    border: none;
    border-radius: 25px;
    cursor: pointer;
    font-weight: bold;
    box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    z-index: 1000;
    transition: 0.3s;
}

.lang-btn:hover {
    transform: scale(1.05);
}
```

---

## 12. Guide de reproduction pas à pas

### Étape 1 : Créer le fichier HTML de base

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mon Analyseur de Données</title>
</head>
<body>
    <h1>Analyseur de données</h1>
</body>
</html>
```

### Étape 2 : Ajouter les variables CSS

```html
<style>
    :root {
        --primary: #667eea;
        --bg-main: #f7fafc;
        --bg-card: #ffffff;
        --text-main: #2d3748;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
        font-family: 'Segoe UI', sans-serif;
        background: var(--bg-main);
        color: var(--text-main);
    }
</style>
```

### Étape 3 : Créer la zone d'upload

```html
<div class="upload-area" id="uploadArea">
    <p>Glissez votre fichier CSV ici</p>
    <input type="file" id="fileInput" accept=".csv" hidden>
</div>

<script>
    const uploadArea = document.getElementById('uploadArea');
    const fileInput = document.getElementById('fileInput');

    uploadArea.addEventListener('click', () => fileInput.click());

    fileInput.addEventListener('change', (e) => {
        if (e.target.files.length > 0) {
            handleFile(e.target.files[0]);
        }
    });
</script>
```

### Étape 4 : Lire le fichier CSV

```javascript
function handleFile(file) {
    const reader = new FileReader();
    reader.onload = function(e) {
        parseCSV(e.target.result);
    };
    reader.readAsText(file);
}

function parseCSV(text) {
    const lines = text.trim().split('\n');
    const headers = lines[0].split(',');
    // ... reste du parsing
}
```

### Étape 5 : Ajouter Chart.js

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>

<canvas id="myChart"></canvas>

<script>
    function createChart(data) {
        const ctx = document.getElementById('myChart');
        new Chart(ctx, {
            type: 'line',
            data: {
                labels: data.map((_, i) => i + 1),
                datasets: [{
                    label: 'Mes données',
                    data: data,
                    borderColor: '#667eea'
                }]
            }
        });
    }
</script>
```

### Étape 6 : Ajouter les onglets

```html
<div class="tabs">
    <button class="tab active" data-tab="overview">Vue d'ensemble</button>
    <button class="tab" data-tab="predictions">Prédictions</button>
</div>

<div id="overviewTab" class="tab-content active">...</div>
<div id="predictionsTab" class="tab-content">...</div>

<script>
    document.querySelectorAll('.tab').forEach(tab => {
        tab.addEventListener('click', () => {
            // Désactiver tous les onglets
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(tc => tc.classList.remove('active'));

            // Activer l'onglet cliqué
            tab.classList.add('active');
            document.getElementById(tab.dataset.tab + 'Tab').classList.add('active');
        });
    });
</script>
```

### Étape 7 : Ajouter le thème sombre

```css
[data-theme="dark"] {
    --bg-main: #1a202c;
    --bg-card: #2d3748;
    --text-main: #f7fafc;
}
```

```javascript
document.getElementById('themeToggle').addEventListener('click', () => {
    const html = document.documentElement;
    if (html.getAttribute('data-theme') === 'dark') {
        html.removeAttribute('data-theme');
    } else {
        html.setAttribute('data-theme', 'dark');
    }
});
```

---

## 13. Améliorations possibles

### 12.1 Fonctionnalités à ajouter
- [ ] Export des résultats en PDF
- [ ] Export des graphiques en image
- [ ] Support des fichiers Excel (.xlsx)
- [ ] Filtrage des données
- [ ] Plus de types de graphiques (histogramme, scatter plot)
- [ ] Sauvegarde des analyses en local (localStorage)

### 12.2 Algorithmes supplémentaires
- [ ] Régression polynomiale
- [ ] Clustering (K-means)
- [ ] Détection de saisonnalité
- [ ] ARIMA pour les séries temporelles

### 12.3 Améliorations UX
- [ ] Tutoriel interactif
- [ ] Tooltips d'aide
- [ ] Raccourcis clavier
- [ ] Mode plein écran pour les graphiques

---

## Glossaire

| Terme | Définition |
|-------|------------|
| **CSV** | Comma-Separated Values - format de fichier texte avec des valeurs séparées par des virgules |
| **CDN** | Content Delivery Network - serveur distant pour charger des librairies |
| **DOM** | Document Object Model - représentation de la page HTML en JavaScript |
| **EMA** | Exponential Moving Average - moyenne mobile exponentielle |
| **IQR** | Interquartile Range - écart entre le 1er et 3ème quartile |
| **NLP** | Natural Language Processing - traitement du langage naturel |
| **Régression** | Méthode statistique pour trouver une relation entre variables |
| **Z-Score** | Nombre d'écarts-types entre une valeur et la moyenne |


## Ressources

- [Chart.js Documentation](https://www.chartjs.org/docs/)
- [MDN Web Docs - JavaScript](https://developer.mozilla.org/fr/docs/Web/JavaScript)
- [CSS Variables](https://developer.mozilla.org/fr/docs/Web/CSS/Using_CSS_custom_properties)


