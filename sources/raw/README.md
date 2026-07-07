# sources/raw — Sources brutes

Dépôt des pièces exactes ayant servi aux analyses : code Silae (standard ou spécifique), captures d'écran, extraits de bulletins, extraits DSN, cahiers des charges, documentation officielle.

## Convention de nommage

```
YYYY-MM-DD_CLIENT-ou-STD_description-courte.ext
```

Exemples :

- `2026-06-24_STD_TC120_code-standard-silae.txt`
- `2026-07-01_clientA_bulletin-cadre-juin-anonymise.pdf`

## Règles

1. Chaque source est datée (date de capture/export, pas date de dépôt si différente).
2. Le contenu est conservé **intact** : pas de reformatage, pas de correction.
3. Les données individuelles (noms, NIR, RIB, salaires nominatifs) sont anonymisées avant dépôt.
4. Jamais de mot de passe, jeton, identifiant secret ou donnée bancaire.
5. Un sous-dossier par client dès que plusieurs clients coexistent — jamais de mélange.
6. Chaque source est référencée depuis au moins un cas dans `knowledge/cases/`.
