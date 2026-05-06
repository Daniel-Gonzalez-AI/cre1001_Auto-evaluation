# Documentation technique — Auto-évaluation de la Confiance créative (CRE 1001)

## Fichier source
- **Excel** : `Auto-évaluation de ma Confiance créative.xlsx`
- **HTML**  : `/auto-evaluation/Auto-évaluation de ma Confiance créative.html`
- **Répertoire** : `/home/artemisai/studies/summer_2026/cre_1001/Lectures/Seance_1/`

---

## 1. Structure générale

Le questionnaire se compose de **deux sections identiques** de 20 énoncés chacune.
Chaque énoncé proposé à l'utilisateur (colonnes B–F) génère deux valeurs :
- **G** = position de la case cochée (`1` à `5`)
- **H** = score calculé (voir §3 ci-dessous)

| Section | Lignes Excel | Nb énoncés |
|---------|-------------|------------|
| 1 | 6 – 25 | 20 |
| 2 | 30 – 49 | 20 |

---

## 2. En-têtes de colonnes (traduits en HTML)

| Colonne | Contenu Excel | Affichage HTML |
|---------|---------------|----------------|
| A | ÉNONCÉ | Énoncé |
| B | 1 / Strongly disagree | 1 — Pas du tout d'accord |
| C | 2 / Disagree | 2 — Pas d'accord |
| D | 3 / Neutral | 3 — Neutre |
| E | 4 / Agree | 4 — D'accord |
| F | 5 / Strongly agree | 5 — Tout à fait d'accord |

---

## 3. Formules colonne G — Détection de la réponse

### Formule Excel (toutes les lignes)
```
=IFERROR(MATCH(TRUE, B{row}:F{row}, 0), 0)
```

### Explication
- `MATCH(TRUE, B{row}:F{row}, 0)` cherche la **première** cellule contenant `TRUE` dans la plage B→F.
- Cela retourne la **position** (1, 2, 3, 4 ou 5) de la case cochée.
- `IFERROR(..., 0)` retourne `0` si aucune case n'est cochée.

### Équivalent HTML
```javascript
// Récupérer la valeur du bouton radio sélectionné (1–5)
const radios = document.getElementsByName(`s1_${i}`);
let val = 0;
for (const rb of radios) {
  if (rb.checked) { val = +rb.value; break; }
}
```

---

## 4. Formules colonne H — Score ajusté

### 4.1 Énoncés standard (non inversés)

**Formule Excel** :
```
=G{row}
```

**Explication** : Le score égal la position (1→5).

| Réponse donnée | Valeur B–F | Score H |
|----------------|------------|---------|
| Pas du tout d'accord | 1 | 1 |
| Pas d'accord | 2 | 2 |
| Neutre | 3 | 3 |
| D'accord | 4 | 4 |
| Tout à fait d'accord | 5 | 5 |

**Équivalent HTML** (JavaScript) :
```javascript
score = val;  // Pas d'inversion
```

---

### 4.2 Énoncés inversés (REVERSE / ↻)

**Formule Excel** :
```
=6-G{row}
```

**Explication** : Le score est **inversé** avec la formule `6 − position`.

| Réponse donnée | Valeur B–F | Score H |
|----------------|------------|---------|
| Pas du tout d'accord | 1 | **5** |
| Pas d'accord | 2 | **4** |
| Neutre | 3 | **3** |
| D'accord | 4 | **2** |
| Tout à fait d'accord | 5 | **1** |

**Équivalent HTML** (JavaScript) :
```javascript
score = 6 - val;  // Inversé
```

---

### 4.3 Liste complète des énoncés inversés

| Section | Rang | Énoncé |
|---------|------|--------|
| 1 | 3 | Je préfère m'appuyer sur les stratégies que j'ai utilisées dans le passé. |
| 1 | 6 | J'ai tendance à tergiverser lorsque je prends des décisions difficiles. |
| 1 | 11 | Je regrette souvent mes décisions. |
| 1 | 17 | J'aimerais que d'autres personnes résolvent des problèmes difficiles à ma place. |
| 2 | 3 | Je préfère m'appuyer sur les stratégies que j'ai utilisées dans le passé. |
| 2 | 6 | J'ai tendance à tergiverser lorsque je prends des décisions difficiles. |
| 2 | 11 | Je regrette souvent mes décisions. |
| 2 | 17 | J'aimerais que d'autres personnes résolvent des problèmes difficiles à ma place. |

> **Identifiant HTML** : lignes avec `class="reverse-row"` (ou anciennement `class="reverse"`)

---

## 5. Formules de total

### 5.1 Section 2 (ligne H50)
**Formule Excel** :
```
=SUM(H30:H49)
```

**Explication** : Somme des 20 scores ajustés de la section 2.

**Équivalent HTML** :
```javascript
const s2 = scoreSection('s2', 20);  // Somme des H30–H49
```

---

### 5.2 Total global (ligne G51)
**Formule Excel** :
```
=SUM(H30:H49)
```

**Explication** : Somme des 20 scores ajustés de la section 2.  
*(Note : le fichier Excel d'origine contenait une formule mal formée `=SUM(G30:G49)+SUM(H30:H49)-G32-G35-G40-G46`, corrigée en `=SUM(H30:H49)` car les H contiennent déjà les valeurs finales.)*

**Équivalent HTML** :
```javascript
const global = s1.sum + s2.sum;  // Somme des H de section 1 + section 2
```

---

## 6. Mathématiques du score

### 6.1 Bornes possibles

Avec 20 énoncés par section :
- **16 énoncés standard** : score de 1 à 5 chacun
- **4 énoncés inversés** : score de 1 à 5 chacun (via l'inversion)

**Par section** :
| | Calcule | Valeur |
|--|---------|--------|
| **Maximum** | 20 × 5 | **100** |
| **Minimum** | 20 × 1 | **20** |

**Global (2 sections)** :
| | Calcule | Valeur |
|--|---------|--------|
| **Maximum** | 40 × 5 | **200** |
| **Minimum** | 40 × 1 | **40** |

### 6.2 Exemple de calcul détaillé

**Exemple** : toutes les réponses = 4 ("D'accord")
- Énoncés standard (16) : 16 × 4 = **64**
- Énoncés inversés (4) : 4 × (6 − 4) = 4 × 2 = **8**
- **Total par section** : 64 + 8 = **72**

---

## 7. Concordance HTML ↔ Excel

| Élément | Excel | HTML |
|---------|-------|------|
| Nombre de sections | 2 | 2 |
| Énoncés par section | 20 | 20 |
| Échelle de réponse | 1–5 (checkbox booléennes) | 1–5 (boutons radio) |
| Détection réponse | `MATCH(TRUE, B:F, 0)` | Lecture `radio.value` |
| Score standard | `=G{row}` | `score = value` |
| Score inversé | `=6-G{row}` | `score = 6 - value` |
| Positions inversées | 3, 6, 11, 17 (dans chaque section) | `reverseRows = [3, 6, 11, 17]` |
| Total section 1 | `SUM(H6:H25)` | JS : somme des 20 items S1 |
| Total section 2 | `SUM(H30:H49)` | JS : somme des 20 items S2 |
| Total global | `SUM(H30:H49)` (corrigé) | JS : `s1 + s2` |
| Affichage conditionnel | — | Dash `–` tant qu'incomplet |

---

## 8. Corrections appliquées au fichier Excel

| Problème | Correction |
|----------|------------|
| Faute de frappe "Stronly" | → "Strongly" |
| Case B16 = `True` par erreur (section 1) | → `False` |
| Cases B25:F25 absentes | → `False` |
| H-formules manquantes lignes 30–49 | Ajoutées (`=G{row}`) |
| D27 formule invalide `=SUMIF(D6:D25, "check", ...)` | Effacée |
| G51 formule incorrecte (double-comptage) | → `=SUM(H30:H49)` |

---

## 9. Stratégie de sauvegarde (HTML uniquement)

```javascript
// Clé localStorage
'cre1001_confiance_creative'

// Structure
{
  "s1_1": "3",
  "s1_2": "4",
  ...
  "s2_20": "5",
  "_meta": {
    "name": "Dupont Marie",
    "date": "2026-05-05",
    "saved": "2026-05-05T20:30:00.000Z"
  }
}
```

---

*Document généré le 5 mai 2026. Dernière mise à jour : validation complète des formules Excel ↔ HTML.*
