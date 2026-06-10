# 📊 Signal Marchés — Dashboard Multi-Marchés Temps Réel

> **Signal Intelligence v2.7** — Outil d'analyse de marchés financiers en temps réel, hébergé sur GitHub Pages. Fichier HTML unique, zéro dépendance, zéro serveur.

---

## 🚀 Démo

Ouvrir directement dans le navigateur ou héberger sur GitHub Pages.  
Cliquer sur **Analyser maintenant** pour lancer une analyse complète.

---

## 📋 Marchés couverts

| Icône | Marché | Actifs suivis |
|-------|--------|---------------|
| 🔬 | Semi-conducteurs | Nvidia, AMD, TSMC, Broadcom, ASML |
| 📈 | Nasdaq 100 | QQQ, Microsoft, Apple, Meta, Alphabet |
| 🇪🇺 | Bourse Européenne | TotalEnergies, Airbus, LVMH, Sanofi, BNP, L'Oréal |
| 🛢️ | Pétrole & Énergie | USO/WTI, ExxonMobil, Chevron, Occidental |
| 🥇 | Or & Métaux | GLD/Or, Argent (SLV), Minières (GDX) |
| ₿ | Crypto | Bitcoin, Ethereum, Solana, XRP |
| 🚀 | Thématiques | IA/Robotique (BOTZ), ARK Innov. (ARKK), Défense (ITA), Énergie propre (ICLN), Biotech (XBI), Cybersécurité (HACK) |

---

## 🧠 Logique des signaux

### Score par actif (0 → 100)
Chaque actif est noté sur 5 critères cumulatifs :

| Critère | Poids | Description |
|---------|-------|-------------|
| Variation du jour | Moyen | +3% → +4 pts bull / -3% → +4 pts bear |
| Variation 5 jours | Fort | Tendance court terme, seuils ×1.5 |
| Variation 20 jours | Moyen | Tendance moyen terme |
| RSI 14 | Fort | Survendu (<30) ou surachat (>70) |
| SMA5 vs SMA20 | Faible | Croisement de moyennes mobiles |

### Signal global d'un marché
Le score moyen des actifs du groupe détermine le signal :

| Score | Signal |
|-------|--------|
| > 60 | 🟢 **ACHAT** |
| 43 – 60 | 🟡 **NEUTRE** |
| < 43 | 🔴 **VENTE** |
| ≥ 2 alertes critiques | 🟠 **DANGER** |

---

## 🔒 Filtres de sécurité

Le signal **ACHAT est automatiquement bloqué** si l'un de ces critères est détecté :

| Condition | Effet |
|-----------|-------|
| RSI > 75 | Score plafonné à 48 (zone neutre) |
| Chute 5j > 8% | Score plafonné à 38 (zone vente) |
| Volatilité > 4%/j | Signal neutralisé |
| ≥ 2 alertes critiques dans le groupe | Signal → **DANGER** |
| ≥ 50% des actifs bloqués | Score du groupe plafonné à 50 |
| ≥ 75% des actifs bloqués | Score du groupe plafonné à 42 |

> La volatilité affichée (`v2.3%`) est l'**écart-type des rendements journaliers sur 10 jours**. Elle mesure l'agitation quotidienne moyenne d'un actif.

---

## ⭐ Indice vedette

À chaque analyse, le dashboard sélectionne automatiquement **l'indice qui sort le plus positivement du lot** :

- Signal ACHAT confirmé (ou meilleur NEUTRE si aucun ACHAT disponible)
- Aucun filtre de sécurité actif
- Score le plus élevé du moment

Les raisons sont affichées : RSI sain, tendance 5j haussière, faible volatilité, etc.

---

## 📡 Sources de données

| Source | Données |
|--------|---------|
| **Finnhub** | Quotes temps réel + candles 1 mois + actualités |
| **Binance** | Prix crypto en temps réel (BTC, ETH, SOL, XRP) |
| **CoinGecko** | Fallback crypto si Binance indisponible |
| **MyMemory** | Traduction automatique des actualités (EN → FR) |

---

## 🗂️ Structure du fichier

```
signal-marches.html
├── CSS (variables, composants, responsive)
├── JavaScript
│   ├── Utilitaires         — cl(), fmtPct(), fmtPrice(), fmtScore()
│   ├── fetchStocks()       — Quotes + candles via Finnhub
│   ├── fetchCrypto()       — Binance + fallback CoinGecko
│   ├── fetchNews()         — Actualités Finnhub (general + crypto + sociétés)
│   ├── traduireNews()      — Traduction batch MyMemory
│   ├── rsi14()             — RSI 14 périodes
│   ├── volatilite()        — Écart-type rendements 10j
│   ├── scoreAsset()        — Score individuel + filtres de sécurité
│   ├── computeSignal()     — Signal global d'un marché
│   ├── choisirVedette()    — Sélection de l'indice vedette
│   ├── analyserTout()      — Orchestration complète
│   └── renderAll()         — Rendu HTML dynamique
└── HTML (structure, tabs, footer)
```

---

## 📦 Déploiement GitHub Pages

1. Forker ou uploader `signal-marches.html` dans un dépôt GitHub
2. Aller dans **Settings → Pages**
3. Source : `Deploy from a branch` → branche `main`, dossier `/root`
4. Renommer le fichier en `index.html` si besoin
5. L'URL sera : `https://<username>.github.io/<repo>/`

---

## 📌 Versioning

| Version | Type | Changement |
|---------|------|------------|
| v1.x | — | Versions initiales |
| v2.0 | Majeure | Refonte sécurité — RSI/volatilité/chute bloquants, signal DANGER |
| v2.1 | Mineure | Indice vedette + versioning formalisé |
| v2.2 | Mineure | Barres jour vs 5j séparées, DANGER orange, scores entiers |
| v2.3 | Mineure | Titres des marchés plus visibles |
| v2.4 | Mineure | Finnhub direct (quotes temps réel + candles) |
| v2.5 | Mineure | Titres poids 600, couleur tx2 |
| v2.6 | Mineure | Titres moins gras |
| v2.7 | Mineure | Forex remplacé par ETFs Thématiques (IA, Défense, Biotech, Propre, Cyber) |

---

## ⚠️ Avertissement

> Ce dashboard est **informatif uniquement** et ne constitue pas un conseil en investissement.  
> Les signaux sont des indicateurs probabilistes — ils ne garantissent aucun résultat.  
> Les produits à levier (ex : 3SEM ×3) comportent un **risque de perte totale**.  
> Toujours utiliser un stop-loss. Ne jamais investir plus que ce que vous pouvez vous permettre de perdre.
