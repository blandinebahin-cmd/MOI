# Matrice de tests — [Client] [Sujet]

> Gabarit autonome pour préparer une campagne de tests ou un parallel run
> lorsque l'analyse ne porte pas sur un code Silae précis (reprise de dossier,
> écart de parallel run, contrôle de premier run de production).

- **Date :** YYYY-MM-DD
- **Client / dossier :** (anonymisé si nécessaire)
- **Période de paie testée :**
- **Environnement :** dossier de test | parallel run | production (contrôle uniquement)
- **Référence de comparaison :** bulletins ancien prestataire | cahier des charges | DSN antérieure

---

## Périmètre

Population, rubriques / cotisations concernées, période, établissements.

## Cas de test

| # | Cas | Salarié test (anonymisé) | Données d'entrée | Résultat attendu | Source du résultat attendu | Résultat obtenu | Écart | Statut |
|---|---|---|---|---|---|---|---|---|
| 1 | Cas nominal | | | | | | | |
| 2 | Seuil juste en dessous | | | | | | | |
| 3 | Seuil exact | | | | | | | |
| 4 | Seuil au-dessus | | | | | | | |
| 5 | Catégorie particulière (apprenti, mandataire…) | | | | | | | |
| 6 | Entrée en cours de mois | | | | | | | |
| 7 | Sortie en cours de mois | | | | | | | |
| 8 | Absence / temps partiel | | | | | | | |
| 9 | Régularisation | | | | | | | |
| 10 | Bulletin post-emploi | | | | | | | |

## Contrôles

- **Bulletin :** lignes, bases, taux, montants, cumuls.
- **États :** livre de paie, états de cotisations.
- **DSN :** blocs / rubriques à contrôler, cohérence avec la DSN antérieure.
- **Cumul annuel :** reprise des cumuls, plafonds, régularisations progressives.
- **Organisme :** montants attendus par organisme.

## Écarts constatés

| # | Écart | Cause identifiée | Statut (observé / déduit / à confirmer) | Action | Décision |
|---|---|---|---|---|---|

## Décision de passage

- [ ] Tous les écarts expliqués et acceptés ou corrigés
- [ ] Validation Blandine
- [ ] Capitalisation faite dans `knowledge/cases/` si un cas nouveau a été traité
