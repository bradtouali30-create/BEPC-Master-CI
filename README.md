# 📚 BEPC MASTER CI — Guide Complet du Développeur

> Application PWA de préparation au BEPC pour les élèves de 3e en Côte d'Ivoire
> Conforme au programme DECO (Direction des Examens et Concours)

---

## 🏗️ Architecture du Projet

```
bepc-master-ci/
├── index.html        ← Application complète (HTML + CSS + JS tout-en-un)
├── manifest.json     ← Configuration PWA (installation sur téléphone)
├── sw.js             ← Service Worker (fonctionnement hors ligne)
├── icon-192.png      ← Icône app (à créer)
├── icon-512.png      ← Icône app grande (à créer)
└── README.md         ← Ce fichier
```

---

## 🚀 Technologie Choisie : PWA (Progressive Web App)

### Pourquoi PWA et non Sketchware ou Flutter ?

| Critère | PWA (HTML/CSS/JS) | Sketchware | Flutter |
|---------|-------------------|------------|---------|
| Développement sur téléphone | ✅ Facile | ✅ Moyen | ❌ Difficile |
| Aucune installation requise | ✅ Oui | ❌ Non | ❌ Non |
| Fonctionne hors ligne | ✅ Service Worker | ✅ Oui | ✅ Oui |
| Évolutivité | ✅ Excellente | ⚠️ Limitée | ✅ Excellente |
| Publication nationale | ✅ URL + Play Store | ⚠️ APK manuel | ✅ Play Store |
| Compétences requises | HTML/CSS/JS | Visuel | Dart |

**✅ Verdict : PWA est le meilleur choix pour démarrer depuis un téléphone Android.**

---

## 📱 Comment tester depuis un téléphone Android

### Méthode 1 — Serveur local (recommandée)
1. Télécharger **Termux** (F-Droid ou Play Store)
2. Dans Termux :
```bash
pkg install nodejs
npx serve .
```
3. Ouvrir Chrome → `http://localhost:3000`
4. Menu Chrome → "Ajouter à l'écran d'accueil"

### Méthode 2 — GitHub Pages (gratuit)
1. Créer un compte GitHub
2. Nouveau repository → Uploader les fichiers
3. Settings → Pages → Source: main
4. Lien: `https://ton-username.github.io/bepc-master-ci`

### Méthode 3 — Netlify Drop (le plus simple)
1. Aller sur **netlify.com/drop**
2. Glisser-déposer le dossier
3. L'app est en ligne en 30 secondes !

---

## 🎨 Design System

### Palette de couleurs
```css
--bg: #060D1A          /* Fond principal - bleu nuit profond */
--surface: #0D1B2E     /* Cartes et composants */
--surface2: #122238    /* Éléments secondaires */
--border: #1E3352      /* Bordures subtiles */
--accent: #F5A623      /* Orange doré - couleur principale */
--accent2: #00D4AA     /* Vert turquoise - succès */
--accent3: #FF6B6B     /* Rouge corail - erreur/urgent */
--blue: #3B8BEB        /* Bleu vif - informations */
--text: #E8F0FE        /* Texte principal */
--text2: #8BA4C0       /* Texte secondaire */
```

### Couleurs par matière
```css
--math: #F5A623        /* Mathématiques - Orange */
--phys: #3B8BEB        /* Physique-Chimie - Bleu */
--svt: #00D4AA         /* SVT - Vert */
--fr: #FF6B6B          /* Français - Rouge */
--en: #A78BFA          /* Anglais - Violet */
--hg: #FB923C          /* Histoire-Géo - Orange vif */
--edhc: #34D399        /* EDHC - Vert émeraude */
```

### Typographie
- **Sora** : Titres, interface, texte principal
- **Space Mono** : Scores, compteurs, données chiffrées

---

## 🧩 Structure du Code (index.html)

### 1. Variables CSS + Design System (lignes 1-120)
Toutes les variables de design centralisées.

### 2. Composants UI (lignes 121-600)
- Loading screen
- Bottom navigation
- Topbar
- Cards et listes
- Boutons et formulaires

### 3. Screens (lignes 601-900)
| Screen ID | Description |
|-----------|-------------|
| `screen-home` | Accueil avec XP, stats, modules |
| `screen-quiz-subjects` | Choix de la matière |
| `screen-quiz-question` | Question en cours |
| `screen-quiz-results` | Résultats du quiz |
| `screen-redaction` | Éditeur de rédaction |
| `screen-saved-essays` | Rédactions sauvegardées |
| `screen-exam-config` | Configuration examen blanc |
| `screen-exam-running` | Examen en cours |
| `screen-exam-results` | Rapport d'examen |
| `screen-stats` | Statistiques complètes |

### 4. Base de données Questions (JS)
```javascript
const QUESTIONS_DB = {
  math: [ /* 12 questions */ ],
  physique: [ /* 10 questions */ ],
  svt: [ /* 10 questions */ ],
  francais: [ /* 10 questions */ ],
  anglais: [ /* 10 questions */ ],
  histoire_geo: [ /* 10 questions */ ],
  edhc: [ /* 10 questions */ ],
};
```

**Format d'une question :**
```javascript
{
  q: "Texte de la question",
  options: ["Option A", "Option B", "Option C", "Option D"],
  answer: 1,  // Index de la bonne réponse (0-3)
  expl: "Explication détaillée de la réponse correcte"
}
```

### 5. Système d'état (State Management)
```javascript
let state = {
  xp: 0,                    // Points d'expérience
  streak: 0,                // Jours consécutifs actifs
  lastActiveDay: '',        // Dernière date d'activité
  quizHistory: [],          // Historique de tous les quiz
  essays: [],               // Rédactions sauvegardées
  quiz: { ... },            // État du quiz actif
  exam: { ... },            // État de l'examen actif
};
```
**Persistance :** `localStorage` → fonctionne 100% hors ligne.

---

## ⚙️ Fonctionnalités Implémentées

### ✅ Module Quiz
- [x] 7 matières avec questions
- [x] Questions aléatoires (Fisher-Yates shuffle)
- [x] Chronomètre 2 min par question
- [x] Correction détaillée affichée après réponse
- [x] Note sur 20 calculée
- [x] Système XP (10 XP/bonne réponse + bonus)
- [x] 5 niveaux : Débutant → Maître académique
- [x] Barre de progression XP

### ✅ Module Rédaction
- [x] Zone de texte complète
- [x] 6 types de rédaction (libre, narration, description, argumentation, lettre formelle, résumé)
- [x] Compteur de mots en temps réel
- [x] Analyse : mots, phrases, paragraphes
- [x] Retours qualitatifs automatiques
- [x] Sauvegarde locale dans localStorage
- [x] Bibliothèque des rédactions sauvegardées
- [x] Chargement d'une rédaction pour modification

### ✅ Mode Examen Blanc
- [x] Simulation BEPC : toutes les matières
- [x] Chronomètre global 45 minutes
- [x] Questions mélangées de toutes les matières
- [x] Sélection personnalisée des matières
- [x] Rapport détaillé par matière
- [x] Score global sur 20

### ✅ Statistiques
- [x] Moyenne générale
- [x] Stats par matière avec barre de progression
- [x] Meilleur score par matière
- [x] Graphique de progression (Canvas 2D)
- [x] Historique complet des quiz et examens
- [x] Compteur de jours actifs (streak)

### ✅ Fonctionnalités Techniques
- [x] PWA installable (manifest.json)
- [x] Fonctionnement hors ligne (Service Worker)
- [x] Persistance des données (localStorage)
- [x] Design responsive mobile-first
- [x] Animations fluides
- [x] Support iOS et Android

---

## 🔧 Comment Ajouter des Questions

Dans `index.html`, chercher `const QUESTIONS_DB` et ajouter :

```javascript
// Exemple : Ajouter une question de maths
{ 
  q: "Quel est le résultat de 15% de 200 ?", 
  options: ["20","25","30","35"], 
  answer: 2,    // "30" est à l'index 2
  expl: "15% de 200 = (15/100) × 200 = 0,15 × 200 = 30." 
}
```

---

## 🚀 Évolutions Futures (Roadmap)

### Version 2.0
- [ ] Correction automatique des rédactions (via API IA)
- [ ] Questions dynamiques depuis une API/Firebase
- [ ] Classement national des élèves
- [ ] Partage des scores sur WhatsApp
- [ ] Mode hors ligne avancé avec IndexedDB

### Version 3.0
- [ ] Cours et fiches de révision par matière
- [ ] Vidéos explicatives intégrées
- [ ] Système d'abonnement (version premium)
- [ ] Application Android native (React Native ou Flutter)
- [ ] Tableau de bord professeur

---

## 📊 Barème de Notation

| Note /20 | Mention | Message |
|----------|---------|---------|
| 18-20 | ⭐ Excellent | Performance exceptionnelle |
| 14-17 | ✅ Bien | Très bon travail |
| 10-13 | 🟡 Passable | Révisions recommandées |
| 5-9 | ⚠️ Insuffisant | Revoir le chapitre |
| 0-4 | ❌ Très insuffisant | Révision complète requise |

---

## 💡 Conseils pour Développer sur Téléphone

1. **Éditeur recommandé :** Acode (Play Store) — éditeur de code gratuit
2. **Navigateur test :** Chrome (inspect via chrome://inspect sur PC)
3. **Débogage :** Utiliser `console.log()` + `alert()` pour déboguer
4. **Hébergement :** Netlify Drop pour partager rapidement

---

## 👨‍💻 Informations Techniques

- **Taille totale :** ~40 Ko (ultra léger)
- **Dépendances externes :** Google Fonts uniquement
- **Compatibilité :** Chrome 80+, Safari 13+, Firefox 75+
- **Storage :** localStorage (jusqu'à 5-10 Mo selon navigateur)
- **Framework :** Vanilla JS (pas de dépendances)

---

*BEPC MASTER CI — Vers l'excellence académique en Côte d'Ivoire 🇨🇮*
