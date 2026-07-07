# MOI — Base de travail Paie & Silae (Deel Global Payroll — France)

Base de travail de **Blandine Bahin** (BHRIS Consulting) pour la mission **Deel Global Payroll Implementation — France** : analyse de dossiers de paie repris, lecture de code Silae, spécifications de paramétrage, préparation des tests, parallel runs et premiers runs de production.

Les règles de fonctionnement de l'assistant (méthode d'analyse, niveaux de confiance, confidentialité, format des livrables) sont définies dans [`AGENTS.md`](AGENTS.md). Ce fichier fait foi.

## Structure du dépôt

```
.
├── AGENTS.md                  # Règles de mission de l'assistant (source de vérité)
├── knowledge/
│   ├── README.md              # Index de la base de connaissances (statuts des documents)
│   ├── SILAE_*.md, …          # Documents de référence Silae (syntaxe, EH, profils, compteurs…)
│   ├── cases/                 # Analyses capitalisées (1 fichier par cas)
│   └── templates/             # Gabarits : analyse de code Silae, matrice de tests
└── sources/
    └── raw/                   # Sources brutes datées (code, captures, extraits DSN, doc)
        └── A_DEPOSER.md       # Pièces référencées restant à déposer
```

## Conventions

### Capitalisation des cas (`knowledge/cases/`)

Un fichier par analyse validée, nommé :

```
YYYY-MM-DD_CCN_CODE_SUJET.md
```

Exemple : `2026-06-24_T003_TC120_paritarisme.md`

Chaque cas doit contenir : conclusion fonctionnelle, preuve (code ou données), population, date d'effet, base, taux ou sa source, matrice de test, contrôle bulletin, contrôle DSN si applicable, points restant ouverts. Une conclusion ne s'écrase jamais : tout changement est tracé dans l'historique du fichier.

### Sources brutes (`sources/raw/`)

Toute pièce ayant servi à une analyse (code Silae, capture, extrait DSN, extrait de bulletin, documentation) est déposée ici, datée, et référencée depuis le cas correspondant. Le code source y est conservé intact.

### Statuts et niveaux de confiance

Chaque affirmation est qualifiée : **OBSERVÉ**, **DÉDUIT**, **À CONFIRMER** ou **SOURCE LÉGALE REQUISE**. Chaque conclusion porte un niveau de confiance : **Confirmé par le code**, **Confirmé par test**, **Probable**, **À confirmer**, **Non démontré**.

## Confidentialité

- Données individuelles anonymisées dans tous les livrables.
- Jamais de mot de passe, jeton, identifiant secret ou donnée bancaire dans le dépôt.
- Jamais de publication des sources propriétaires Silae en dehors de ce cadre de travail.
- Jamais de mélange de données entre deux clients : un sous-dossier par client dans `knowledge/cases/` et `sources/raw/` dès que plusieurs clients coexistent.

## Règle d'or

Aucune modification en production sans test préalable dans un environnement ou dossier de test, puis validation explicite de Blandine.
