# [CCN] [CODE] — [Sujet]

> Gabarit d'analyse d'un code Silae — suivre la méthode définie dans `AGENTS.md` §5 et §6.
> Renommer le fichier selon la convention `YYYY-MM-DD_CCN_CODE_SUJET.md` avant dépôt dans `knowledge/cases/`.

- **Date d'analyse :** YYYY-MM-DD
- **Client / dossier :** (anonymisé si nécessaire)
- **Version Silae / date du paramétrage :** (si connue, sinon « À confirmer »)
- **Sources brutes :** `sources/raw/...`
- **Statut :** brouillon | validé
- **Niveau de confiance :** Confirmé par le code | Confirmé par test | Probable | À confirmer | Non démontré

---

## Conclusion

Une phrase claire.

## Point d'entrée

| Élément | Valeur |
|---|---|
| Code rubrique / cotisation / fonction | |
| CCN | |
| Date de bulletin | |
| Période de taux | |
| Établissement | |
| Salarié / population | |
| Organisme | |
| Environnement (test / production) | |

## Lecture du code

Bloc ou logique exacte (extrait minimal nécessaire — le code complet reste dans `sources/raw/`).

```
[extrait de code OBSERVÉ, conservé intact]
```

Contexte d'exécution reconstitué : initialisations, `Include(...)`, `Exec(...)`, `Call ...`, fonctions appelées, conditions de date / effectif / salarié / établissement, régularisations, bulletins post-emploi.

## Cartographie des variables

| Variable | Valeur / source | Rôle | Niveau | Statut |
|---|---|---|---|---|
| | | | bulletin / salarié / société / organisme | observé / déduit / à confirmer |

## Règle fonctionnelle

> La ligne se déclenche si A, B et C.
> Elle est neutralisée si D.
> La base est E.
> Le taux est F.
> Le résultat attendu est G.

## Population concernée

Inclus / exclus / non déterminé. Exclusions testées : apprenti, stagiaire, mandataire, salarié sans contrat de travail, cadre / non-cadre, temps partiel, entrée / sortie en cours de mois, bulletin post-emploi, participation / intéressement, multi-contrats, autre établissement, Mayotte / DOM.

## Conditions de déclenchement

Dates, effectif, statut, organisme, option dossier, avenant, méthode.

## Base et taux

Distinguer ce qui vient du code (OBSERVÉ) de ce qui vient du paramétrage de taux (À CONFIRMER dans le dossier).

## Matrice de test

| # | Cas | Données d'entrée | Résultat attendu | Contrôle | Résultat obtenu | Statut |
|---|---|---|---|---|---|---|
| 1 | Cas nominal | | | bulletin | | |
| 2 | Seuil juste en dessous | | | bulletin | | |
| 3 | Seuil exact | | | bulletin | | |
| 4 | Seuil au-dessus | | | bulletin | | |
| 5 | Catégorie particulière | | | bulletin | | |
| 6 | Date avant effet | | | bulletin | | |
| 7 | Date après effet | | | bulletin | | |
| 8 | Régularisation | | | bulletin + cumul | | |
| 9 | Contrôle bulletin | | | bulletin | | |
| 10 | Contrôle DSN | | | DSN (bloc / rubrique) | | |

## Où tester

Chemin fonctionnel, salarié test, période, résultat attendu.

## Contrôles

Bulletin, états, DSN, CRM, cumul annuel, organisme.

## Risques / points ouverts

Uniquement les vrais points non démontrés.

---

## Révisions

| Date | Changement | Raison | Auteur |
|---|---|---|---|
| YYYY-MM-DD | Création | — | |
