# Auto-évaluation de la Confiance Créative — CRE 1001

**Cours :** CRE 1001 — Fondements de la créativité  
**Séance :** Séance 1 (5 mai 2026)  
**Enseignant :** Lynne Lamarche — Direction de l'apprentissage continu, UdeM  
**Adaptation HTML :** Daniel Gonzalez, Cert. en IA, UdeM — 05 mai 2026

---

## 📋 Vue d'ensemble

Ce dépôt contient un **questionnaire interactif d'auto-évaluation de la confiance créative**, développé à partir du fichier Excel original du cours CRE 1001. L'outil permet à chaque étudiant d'évaluer son niveau de confiance créative sur 40 énoncés répartis en deux sections identiques de 20 items, avec calcul automatique des scores et interprétation par paliers.

---

## 🗂️ Structure du dépôt

```
auto-evaluation/
├── index.html                           ← Version interactive principale
├── Auto-évaluation de ma Confiance créative.html   ← Copy (même contenu)
├── Auto-evaluation_Confiance_creative.xlsx         ← Fichier Excel original du cours
├── package.json                         ← Config npm (serveur local)
├── vercel.json                          ← Config Vercel (déploiement)
├── .gitignore                           ← Règles d'ignorance Git
├── README.md                            ← Ce fichier
├── docs/                                ← Documentation technique
│   ├── FORMULES.md                     ← Concordance Excel ↔ HTML
│   ├── SOURCES_ANALYSIS.md             ← Analyse critique des sources
│   ├── Kelley_Kelley_Creative_Confidence_Preface_Intro_FR.md
│   └── Torrance_Tests_of_Creative_Thinking.md
├── references/                           ← Sources académiques (PDF)
│   ├── IsaksenVIEWandCPSKJTPS07.pdf     ← Isaksen & Geuens (2007)
│   ├── Torrance_Tests_of_Creative_Thinking.pdf
│   └── TTCT_InterpMOD.2018.pdf          ← Manuel Torrance TTCT
└── scripts/
    └── generate_excel.py                 ← Script de génération Excel
```

---

## 🚀 Utilisation

### Option 1 — Ouvrir directement dans le navigateur
Double-cliquez sur `index.html` (ou `Auto-évaluation de ma Confiance créative.html`).

### Option 2 — Serveur local (recommandé)
```bash
npm install -g serve
serve .
```
Puis ouvrez http://localhost:3000

### Option 3 — Déploiement Vercel
Le projet est prêt pour Vercel (`package.json` + `vercel.json` configurés).

---

## 📐 Fonctionnalités

| Fonction | Description |
|----------|-------------|
| **🎓 Two sections** | 2 × 20 énoncés = 40 items au total |
| **🔄 Items inversés** | 8 items (4 par section) avec inversion automatique du score |
| **📊 Scoring live** | Totaux section (/100) + global (/200) se mettent à jour en temps réel |
| **🌙 Mode sombre / clair** | Basculable via 🌙/☀️, persistance via `localStorage` |
| **💾 Sauvegarde auto** | Chaque clic est enregistré dans le navigateur |
| **🖨️ Impression / PDF** | Bouton dédié avec CSS optimisée pour l'impression |
| **🗑️ Réinitialisation** | Effacement complet avec confirmation |
| **👤 Champs élève** | Nom/Prénom + Date (pré-remplie) |
| **📈 Barre de progrès** | Suvisuel du pourcentage complété |

---

## 🔢 Logique de scoring

### Échelle
| Valeur | Intitulé |
|--------|----------|
| 1 | Pas du tout d'accord |
| 2 | Pas d'accord |
| 3 | Neutre |
| 4 | D'accord |
| 5 | Tout à fait d'accord |

### Items inversés (↻)
| Position | Énoncé |
|----------|--------|
| 3 | Je préfère m'appuyer sur les stratégies que j'ai utilisées dans le passé. |
| 6 | J'ai tendance à tergiverser lorsque je prends des décisions difficiles. |
| 11 | Je regrette souvent mes décisions. |
| 17 | J'aimerais que d'autres personnes résolvent des problèmes difficiles à ma place. |

**Scoring :**
- Standard : `score = valeur choisie` (1→5)
- Inversé : `score = 6 − valeur choisie` (5→1, 1→5)

### Bornes
| | Par section | Global |
|---|---|---|
| **Minimum** | 20 | 40 |
| **Maximum** | 100 | 200 |

### Paliers d'interprétation
| Niveau | Section | Global |
|---|---|---|
| 🛠️ Potentiel de développement | 20–45 | 40–89 |
| 🌱 Confiance modérée | 46–72 | 90–144 |
| ✨ Confiance élevée | 73–100 | 145–200 |

---

## 📚 Sources et références

### Source conceptuelle
- **Kelley, T. & Kelley, D. (2013).** *Creative Confidence: Unleashing the Creative Potential Within Us All.* New York: Crown Business.

### Références consultées
- **Isaksen, S. G. & Geuens, D. (2007).** VIEW et résolution créative de problèmes.
- **Torrance, E. P. (1966→2018).** *Tests of Creative Thinking (TTCT).* Scholastic Testing Service.

### Analyse critique
> Voir `docs/SOURCES_ANALYSIS.md` pour une analyse détaillée : le questionnaire de CRE 1001 est un **outil pédagogique original** inspiré du *concept* de Kelley & Kelley, mais **pas un instrument validé** issu de la littérature psychométrique. Les trois sources ci-dessus ne contiennent pas la grille de 40 énoncés Likert utilisée ici.

---

## 🛠️ Technologies

| Technologie | Utilisation |
|-------------|-------------|
| HTML5 | Structure sémantique |
| CSS3 (variables + `oklab`) | Thème clair/sombre, responsive |
| Vanilla JavaScript | Logique de scoring, `localStorage` |
| Python + openpyxl | Réparation et vérification Excel |
| IBM Docling | Conversion PDF→Markdown (GPU CUDA) |

---

## 📝 Notes importantes

1. **Cet outil est pédagogique.** Il favorise la réflexion personnelle et la discussion en classe, mais n'a pas subi de validation psychométrique standardisée (alpha de Cronbach, test-retest, normes populationnelles).

2. **Ne pas interpréter le score comme un diagnostic clinique.** Il s'agit d'un point de départ pour la métacognition, pas d'une mesure normative.

3. **Les deux sections sont identiques.** Leur redondance vise un usage didactique (pré/post, discussion de classe) plutôt qu'une fiabilité interne psychométrique.

4. **Données stockées localement uniquement.** Aucune donnée n'est envoyée sur un serveur. Les réponses résident dans le `localStorage` du navigateur.

---

## 🔧 Réparations appliquées au fichier Excel d'origine

| Problème | Correction |
|----------|------------|
| Faute de frappe « Stronly » | → « Strongly » |
| Case B16 = `True` par erreur | → `False` |
| Cases B25:F25 absentes | → Remplies avec `False` |
| Formules H manquantes section 2 | → Complétées selon la logique de la section 1 |
| D27 formule invalide | → Effacée |
| G51 formule double-comptage | → Corrigée pour éviter duplication |

---

## 📄 Licences et crédits

- **Questionnaire original :** Lynne Lamarche, CRE 1001, UdeM
- **Adaptation HTML et traduction :** Daniel Gonzalez, Cert. en IA, UdeM
- **Conversion PDF :** IBM Docling (GPU NVIDIA RTX 3060)
- **Design et développement HTML :** Libre de réutilisation académique

---

*Dernière mise à jour : 6 mai 2026*
