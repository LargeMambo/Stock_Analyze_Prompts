# Stock Analyze Prompts

<div align="center">

[简体中文](README.md) | [English](README_en.md) | [Français](README_fr.md)

</div>

---

Une collection de prompts IA pour l'analyse boursière, proposant deux cadres d'investissement de valeur systématiques conçus pour Claude et d'autres LLM. Concepts stratégiques inspirés du créateur Bilibili « 史诗级韭菜 ».

## Cadre 1 : Prompt Mégots de Cigares (v1.8)

Un cadre d'analyse quantitative pour les actions sous-évaluées cotées à Hong Kong et sur les marchés A. Construit sur la logique de décote sur actif net à la Graham, il identifie systématiquement trois sous-types de « mégots de cigares », exécute 22 vérifications factuelles et fournit une note de marge de sécurité par paliers avec un dimensionnement de position à demi-Kelly.

**Trois Sous-types Couverts :**

| Sous-type | Logique Principale |
|---|---|
| A — Décote sur Actifs | Décote sur trésorerie nette / actifs ; capitalisation inférieure à la valeur de liquidation |
| B — Déverrouillage de Filiale | La capitalisation de la filiale cotée dépasse celle de la société mère |
| C — Rachat / Privatisation | Rachats en cours ou anticipation de privatisation avec un chemin de libération de valeur clair |

**Mode d'Emploi :**

1. Collez le contenu intégral de [`cigbutt/烟蒂股prompt_v1.8.md`](cigbutt/烟蒂股prompt_v1.8.md) comme System Prompt dans Claude (ou un autre LLM)
2. Fournissez la cible : code boursier ou nom de la société, ainsi qu'au moins 2 périodes de rapports financiers (upload PDF ou tableau collé)
3. L'IA récupère en temps réel la capitalisation, le cours, le rendement du dividende, etc. via yfinance MCP / WebSearch et génère un rapport d'analyse complet en Markdown

**Exemple d'Entrée :**

```
Analyser 3328.HK Bank of Communications. J'ai joint les PDF du rapport annuel 2023 et du rapport semestriel 2024.
```

**Livrables :**
- Calcul du coussin d'actifs (marge de sécurité de niveau T)
- Détermination du sous-type avec vérification de chaque condition
- 22 vérifications factuelles (classification des alertes : type Données vs type Risque)
- Note globale (A / B / C / Rejeté)
- Recommandation de dimensionnement de position à demi-Kelly

**Normes Comptables Supportées :** HKFRS / IFRS, CN-GAAP

---

## Cadre 2 : Stratégie Tortue (v0.15)

Un cadre d'analyse d'investissement multi-agents systématique couvrant les marchés de Hong Kong, des actions A et américains. Utilise une architecture Coordinateur + trois phases de sous-agents pour générer des rapports d'analyse approfondis pour toute société cotée.

**Pipeline d'Analyse :**

```
Entrée Utilisateur (code boursier + PDF rapport annuel optionnel)
        ↓
  Coordinateur — analyse l'entrée, planifie toutes les phases
        ↓
  Phase 1 — Collecte de Données
  (yfinance MCP : données de marché, états financiers, historique des dividendes)
        ↓
  Phase 2 — Analyse PDF
  (extraction structurée des rapports annuels / semestriels)
        ↓
  Phase 3 — Analyse et Rapport
  (valorisation, avantage concurrentiel, évaluation des risques, dimensionnement demi-Kelly)
```

**Mode d'Emploi (Claude Projects / API) :**

1. Configurer yfinance MCP (ou un outil MCP équivalent pour données boursières)
2. Définir [`coordinator.md`](turtle_framework/乌龟战法_v0.15/coordinator.md) comme System Prompt de l'agent principal
3. Configurer les Phases 1–3 comme System Prompts des sous-agents respectifs
4. Saisir un code boursier dans la conversation principale (joindre optionnellement un PDF de rapport annuel)

**Exemple d'Entrée :**

```
Analyser 00700.HK Tencent Holdings.
```

**Livrables :**
- Pack de données complet (données de marché + compte de résultat + bilan + flux de trésorerie)
- Analyse du paysage concurrentiel et des avantages concurrentiels
- Valorisation multi-méthodes (DCF / relative / réévaluation d'actifs)
- Liste des risques
- Dimensionnement de position à demi-Kelly et fourchette de prix d'achat

**Normes Comptables Supportées :** HKFRS / IFRS, CN-GAAP, US-GAAP

---

## Rapports d'Exemple

Le répertoire [`reports/`](reports/) contient plus de 40 rapports d'analyse boursière individuels couvrant les marchés de Hong Kong, des actions A et américains, disponibles à titre de référence.

- **Rapports Mégots de Cigares (20)** : China Dongxiang, Pou Sheng International, Financial Street Property, HK Ferry, China Education Group, etc.
- **Rapports Stratégie Tortue (20)** : Bull Group, NetEase, Gree Electric, Wuliangye, Feihe, Trip.com (TCOM), Duolingo (DUOL), etc.

---

## Structure du Répertoire

```
Stock_Analyze_Prompts/
├── cigbutt/
│   ├── index.md                      — Présentation du cadre
│   └── 烟蒂股prompt_v1.8.md          — Prompt version actuelle
├── turtle_framework/
│   ├── index.md                      — Présentation du cadre
│   └── 乌龟战法_v0.15/
│       ├── coordinator.md            — Coordinateur (agent principal)
│       ├── phase1_数据采集.md         — Sous-agent 1 : Collecte de données
│       ├── phase2_PDF解析.md          — Sous-agent 2 : Analyse PDF
│       └── phase3_分析与报告.md       — Sous-agent 3 : Analyse et rapport
└── reports/
    ├── cigbutt_reports/              — Rapports mégots de cigares (20)
    └── turtle_reports/               — Rapports Stratégie Tortue (20)
```
