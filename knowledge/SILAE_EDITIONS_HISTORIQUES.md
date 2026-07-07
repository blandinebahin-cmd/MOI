# Éditions Historiques (EH) — langage de reporting / export

**Sources :** articles d'aide officiels Silae (MAJ 2024) → `sources/raw/formation/` fiches `1.` à `7.` + listes de fonctions `3.1` (mode Salariés, 38 K) et `3.2` (mode Bulletins, 54 K). **Statut : OBSERVÉ** (éditeur).
L'EH **réutilise la grammaire des fonctions calcul** (`If`, `Select Case`, `Mid`, fonctions de date, marques, zones, famille `CumulLignes*`) mais avec son **propre modèle de colonnes** et ses **préfixes de champs**.

## À quoi ça sert
Générer/exporter des états de données (salariés, emplois, bulletins) sur mesure. Deux niveaux de création :
- **Dossier** : `Paramétrage > Éditions historiques`.
- **Domaine (cabinet)** : `Paramétrage paie > Éditions historiques`, rattachable à un **groupe de dossiers**.
Accès réservé (droit complet / responsables sociaux). Transférable entre dossiers (Importer depuis / Exporter vers).

## Structure d'une édition : 2 sections (chacune `Begin … End`)
1. **En-tête** (facultatif) — titres de colonnes + options : `colonne010.titre="Date de naissance"`.
2. **Lignes** (fondamental) — les données : `colonne010 = INT_DateNaissance`.
> Numéroter `colonne010, colonne020, colonne030…` (pas `colonne1, colonne2` : sinon `colonne11` sort avant `colonne2`). Les n° des Lignes doivent matcher ceux de l'En-tête.

## Deux modes de lecture (les fonctions diffèrent selon le mode)
- **Bulletins** (défaut) : données des bulletins **calculés** de la période → fonctions de la fiche `3.2`.
- **Salariés** : données des salariés ayant ≥ 1 emploi sur la période (accessible même **sans bulletin**) → fonctions de la fiche `3.1`.

## Noms du champ (préfixes)
| Préfixe | Portée | Exemples |
|---|---|---|
| `INT_` | Entier / admin fiche salarié | `INT_DateNaissance`, `INT_NumeroSS` |
| `SAL_` | Salarié (admin) | |
| `SEM_` | Emploi | `SEM_CLM_Code` (classification), `SEM_S41_G01_00_012_001` (type contrat DSN), `SEM_CDDMotif` |
| `BUL_` | Bulletin | `BUL_PERIODE` |
| `EH.` | Variables système | `EH.NumeroLigne`, `EH.DATEDEBUT`, `EH.DATEFIN` |

## Fonctions (section Lignes)
Pour des données complexes (cumuls). Respecter le **nombre de variables** entre parenthèses. Ex. à 5 variables :

```
CumulLignesBasePSelonZone("Code libellé","Marque interne","Marque1","Marque2",Zone)
colonne0010 = CumulLignesResultatS("D06","","","")
```

- **Code libellé** : codes du bulletin (colonne « Code libellé »).
- **Marque1 / Marque2** : colonnes Marque1/Marque2 des lignes du bulletin.
- **Marques internes** : `$IPF`, `$IC2`, `$IJB`… (mêmes codes que `REFERENTIELS_ABSENCES_ET_MARQUES.md`).
- **Zones** : `2` = entre salaire de base et brut · `3` = cotisations · `4` = entre net imposable et net à payer. **La zone 1 n'existe pas** (lignes « salaire de base » non appelables).
- **Dates** : `Date(j,m,a)`, `DateDay/Month/Year`, `DateAddDays/Months/Years`, bornes `EH.DATEDEBUT` / `EH.DATEFIN`.
> Listes exhaustives de fonctions par mode : `sources/raw/formation/3_1_…Salariés` et `3_2_…Bulletins`.

## Conditions & instructions (mêmes mots-clés que les FC)
- **If** : `If <cond> Then <inst>` · multi-lignes `… EndIf` · `… Else … EndIf`.
- **Exclure une ligne/salarié** : `if <cond> then LigneExclue = true` (ex. ne garder que le personnel externe).
- **Select Case** : `Select Case <Variable> … Case "xxx": … Default: … Endselect` ; multi-valeurs `Case "04","05":`.
- **Mid** (extraire une sous-chaîne — 3 args : champ, position début, longueur) :

```
If Mid(INT_NumeroSS,1,1) = "1" then    // 1er chiffre du NIR : 1=H, 2=F
   colonne005 = "H"
else
   colonne005 = "F"
Endif
```

Exemple métier (prime selon classification) :

```
if Mid(SEM_CLM_Code,1,7) = "B065.01" then
   colonne0010 = CumulLignesResultatS("D06","","","")
else
   colonne0010 = 0.00
endif
```

Décodage de codes DSN en libellés (type contrat) :

```
Select Case SEM_S41_G01_00_012_001
   Case "01": colonne001 = "CDI"
   Case "02": colonne001 = "CDD"
   Case "04","05": colonne001 = "Apprenti"
   Case "90": colonne001 = "Sans contrat de travail"
   Default:   colonne001 = "Autre"
Endselect
```

## Fonctions compteurs CP / provisions (OBSERVÉ — fiche officielle « 7. Fonctions les plus demandées »)

```
colonne0010 = CompteurCP("CPN1ACQUIS") - CompteurCP("CPN1PRIS")      // solde CP N-1 (mode Bulletins)
colonne0020 = CompteurCP("CPNACQUIS")  - CompteurCP("CPNPRIS")       // solde CP N
colonne0030 = ProvisionsCP("CPN1ACQUIS")                              // provision N-1
colonne0070 = CompteurCP("RTTACQUIS") - CompteurCP("RTTPRIS")        // RTT ; aussi RCRACQUIS/RCRPRIS, CETACQUIS/CETPRIS
// mode Salariés (à une date) :
colonne0010 = CompteurCPPeriode("CPN1PRIS", EH.DATEFIN)
colonne0030 = ProvisionsCP(EH.DATEFIN, "CPN1ACQUIS")
```

Clés observées : `CPN1ACQUIS` · `CPN1PRIS` · `CPNACQUIS` · `CPNPRIS` · `RTTACQUIS/PRIS` · `RCRACQUIS/PRIS` · `CETACQUIS/PRIS`.

## Champs bulletin CP (OBSERVÉ — fiche « 2. Mots-clés », section CP)
`BUL_CPJoursAcquis` · `BUL_CPJoursAcquisAnciennete` · `BUL_CPJoursAcquisFractionnement` · `BUL_CPJoursAcquisEpsilon` (epsilon de clôture) · **`BUL_CPJoursPrisRef`** (jours pris **période de référence** enregistrés sur le bulletin) · `BUL_CPJoursPrisAnt` (pris par anticipation) · **`BUL_CPMtRef`** / `BUL_CPMtAnt` (montants correspondants) · `SAL_ClotureCPReport` (report du solde après clôture) · `SEM_CPAcqMois` · `BUL_CPSupJoursAcquis/Pris`.

## Génération
État d'avancement → clic droit sur la bulle **Bulletins** (période calculée) ou sur un **salarié/groupe** → « Autres éditions » > libellé de l'édition. Colonnes **Matricule** et **salarié** présentes par défaut.

> À approfondir si besoin : `5.` options de présentation/lancement · `7.` fonctions les plus demandées + export hors EH · decks `[12]`/`MAJ 032020`. Tout est dans `sources/raw/formation/`.
