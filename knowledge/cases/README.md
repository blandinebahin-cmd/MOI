# knowledge/cases — Analyses capitalisées

Un fichier par analyse **validée**, créé à partir du gabarit [`../templates/analyse-code-silae.md`](../templates/analyse-code-silae.md).

## Convention de nommage

```
YYYY-MM-DD_CCN_CODE_SUJET.md
```

- `YYYY-MM-DD` : date de validation de l'analyse ;
- `CCN` : identifiant de la convention collective (ex. `T003`) ou `STD` pour le standard toutes CCN ;
- `CODE` : code rubrique / cotisation / fonction analysé (ex. `TC120`) ;
- `SUJET` : mot-clé court en minuscules (ex. `paritarisme`).

Exemple : `2026-06-24_T003_TC120_paritarisme.md`

## Règles

1. La ou les sources brutes sont déposées dans `sources/raw/` et référencées depuis le cas.
2. Le code source est conservé intact (pas de reformatage).
3. Chaque cas indique la date, la version Silae si connue, le client concerné (anonymisé si nécessaire).
4. Une ancienne conclusion ne s'écrase jamais : ajouter une section « Révision » datée qui trace le changement et sa raison.
5. Données individuelles anonymisées ; jamais de mélange entre clients.
