# Langage Silae — Syntaxe et instructions

**Sources :**
- Doc éditeur officielle *« Langage SILAE – Syntaxes et variables »* → extrait dans `sources/raw/REF_Langage_Silae_Syntaxes_et_variables.txt`
- Recoupée avec du code de production GDLP : `CP010` (cotisation syndicale BTP), `CP002` (intempéries).

**Statut global : OBSERVÉ** (source éditeur officielle + code réel). Les rares points incertains sont marqués *À CONFIRMER*.

---

## 1. Structure d'un profil prime / d'une fonction calcul

| Mot-clé | Rôle |
|---|---|
| `Begin` | Ouvre systématiquement tout profil prime ou fonction calcul. |
| `End` | Ferme systématiquement le bloc. |
| `Return` | Sort immédiatement du profil/FC ; le reste du code **n'est pas exécuté**. |
| `// ...` | Commentaire de ligne (observé dans tout le code de prod). |

Exemple réel (CP002) : sortie anticipée pour les expatriés.

```silae
If Expat = true then
    Bases = 0 : Tauxs = 0 : Basep = 0 : Tauxp = 0
    return
Endif
```

## 2. Conditions `If`

Sur une ligne : `If Bul.SortiCeMois = true Then MaVariable = 1`

Multi-lignes :

```silae
If Bul.SortiCeMois = true Then
    Mavariable1 = 10
Else
    Mavariable1 = 5
EndIf
```

Opérateurs observés : `=`, `<>`, `<`, `>`, `>=`, `<=`, combinés par `And` / `Or`.

## 3. `Select Case`

```silae
Select Case Bul.Mois
    Case 1 :  MaVariable1 = bul.salairedebase * 0.10
    Case "01","02" :  // multi-valeurs possibles (observé CP010 par département)
    Default : MaVariable1 = 0
EndSelect
```

## 4. Boucle `Do … Loop`

```silae
i = 0
Do
    i = i + 1
    If i = 10 Then exit      // sortie de boucle
    MaChaine = MaChaine + i
Loop
```

Pattern d'itération standard sur les tableaux (heures, maintiens, IJSS) : voir `SILAE_FONCTIONS_CALCUL_CATALOGUE.md`.

## 5. Saisie d'un élément variable (EV)

| Fonction | Effet |
|---|---|
| `SAISIE("Nom", défaut)` | Ouvre un EV numérique ; renvoie la valeur saisie (ou le défaut). |
| `SAISIEM1("Nom")` | Idem mais valeur par défaut = celle saisie le mois précédent. |
| `VariableSaisie("Nom")` | N'ouvre **pas** d'EV ; lit la valeur d'un EV déjà ouvert. |
| `SaisieChaine("Nom", défaut)` | Ouvre un EV de type texte (`""` = pas de défaut). |

## 6. Génération d'une ligne — primes

```silae
Exec("Lprime_C01")        // appel du modèle de libellé
methodeCalcul = 3
BaseS = 10
Tauxs = bul.tauxhoraire   // seulement si methodeCalcul = 1 ou 3
Exec("GenereLprime")      // déclenche la ligne sur le bulletin
```

**`methodeCalcul` :** `1` = BaseS × TauxS / 100 · `2` = BaseS · `3` = BaseS × TauxS · `99` = base seule (ligne « neutre »).

Options à poser entre `Exec("Lprime_xx")` et `Exec("GenereLprime")` — ⚠ **elles persistent d'une prime à l'autre, à repositionner** :
`LigneNeutre = True` (hors brut/NAP) · `SupplementCoutGlobal = False` · `PrimeNette = True` (montant rebrutalisé) · `Liblong = "..."`.

Pour exécuter un profil entier : `Exec("PPrime_NOMPROFIL")`.

## 7. Génération d'une ligne — cotisations (observé CP010/CP002)

```silae
CodeDucs = "7001210"
Exec("TAUX_CP010")              // applique le taux daté
Exec("TAUX_CP010." + Eta.Departement)   // taux variabilisé par département
Liblong = "Cotisation syndicale CAPEB"
Exec("GenereLcotis")           // génère la ligne de cotisation
```

Variables de pilotage d'une cotisation : `BaseS`/`BaseP`, `TauxS`/`TauxP`, `CodeDucs`, `Liblong`, `Marque1`, `marquedtdeb`/`marquedtfin`, `InhibeRegularisations`.

## 8. `Include`
`Include("NOMFONCTIONCALCUL")` injecte du code dans un profil **sans figer le paramétrage légal/conventionnel**. Utilisé dans la quasi-totalité des profils standards Silae (ex. observés : `INIT-CP010`, `INIT-CP002`, `PLFSSREFORME2019`, `TRT-INTEMP`).

## 9. Commentaires sur le bulletin
- `call AjouteLigneNeutre(zone, "texte")` — zone `2` = brut, `3` = cotisations, `4` = bas de bulletin.
- `Call Rem("texte")` — note de calcul (observé massivement en prod, ex. trace d'un plafonnement de base).

## 10. StockVar (mémoire inter-bulletins)

| Instruction | Effet |
|---|---|
| `Call Stockevar("Nom", valeur)` | Stocke une valeur (numérique ou texte). |
| `Stockvar("Nom")` | Relit la valeur sur le bulletin courant. |
| `StockVarExists("Nom")` / `StockVarDef("Nom", défaut)` | Test d'existence / lecture avec défaut (observé prod). |
| `StockvarAbs("CALABSXXX")` | Lecture des stockvar d'absence. |
| `CumulStockvar(d1, d2, "Nom")` | Cumul d'un stockvar sur une période. |
| `CumulStockvarEmp(d1, d2, "Nom")` | Idem, restreint à l'emploi en cours. |

## 11. Dates
`Date(j,m,a)` · `DateDay / DateMonth / DateYear(d)` · `DateAddDays / DateAddMonths / DateAddYears(d,n)` · `DiffDays / DiffMonths / DiffYears(d1,d2)` · `DateToday()`.
Repères bulletin : `Bul.Periode` (1er jour du mois) · `Bul.Date` (dernier jour) · `Bul.Mois` (1-12) · `Bul.Annee`.

## 12. Texte
`Right(s,n)` · `Left(s,n)` · `Mid(s,pos,n)` · `StrLen(s)` · `StrReplace(s,a,b)` · `StrTrim(s)`.

## 13. Numérique & conversions
`Trunc(x)` · `Round(x, 2)` · `ToDouble(s)` · `ToInt(s)` · `ToString(x)`.

## 14 bis. Profil de prime vs fonction calcul (OBSERVÉ, 2026-07-07)

| | Profil de prime | Fonction calcul |
|---|---|---|
| Où | `Paramétrage > Primes > Profils` | `Paramétrage > Fonctions calculs` |
| Exécution | Calcul du bulletin, salarié par salarié, dans la chaîne des primes | Moteur, à des points précis : noms réservés appelés automatiquement (`CP-ANCIENNETE`, `M-MALADIE`, `PROVISIONECRCPT`, `IMPORT*`…) ou injection via `Include` |
| Colonne EV | ✅ `Saisie("X",0)` ouvre une colonne EV → importable en masse | ❌ pas de colonne EV de masse |
| Usage type | primes, compteurs, saisies mensuelles | maintiens, INIT/FIN de rubriques, exports compta, imports, personnalisation conventionnelle |
| Pour tous les salariés | via un conteneur type `PCCN01` (« Ajouter un profil ») | automatique selon nom / point d'accrochage |

**Passes de calcul** : le moteur exécute le bulletin en de multiples passes (`CALCULNORMAL`, `CALCULPRIMES`, `CALCULVIRTUELBRUT`, `CALCULVIRTUELNET`, `DETERMINEPRIMESMAJORATIONHEURESSUP`, `DETERMINEBRUTAPARTIRDUNET`, … — liste complète dans `REF_Langage_Silae_Syntaxes_et_variables.txt`). Un profil qui mouvemente des compteurs **sans garde** `If Bul.Fonction = Fonction.CALCULNORMAL` peut s'exécuter plusieurs fois → double comptage. `AjouteCPPrisRef` / `AjouteCPPrisAnt` / `AffecteCpAcquisRef` ciblent explicitement la période (réf = N-1, ant = N). ⚠ Les écritures de report (`AffecteCPAcquisRef`) **persistent** dans les données du compteur après retrait de la saisie/du profil (observé cas CP-CLOTURE) — purge par contre-saisie négative.

## 14 ter. Divers utiles (OBSERVÉ dans le dump)

- `Call ChangementMoisClotureCP(période_bulletin, ancien_mois, 2)` dans une FC `SALMINCONVPRECALC` : changer le mois de clôture CP en cours d'année.
- Import XLS des EV : colonnes directes `cpn-1acquis` / `cpn-1pris` / `cpnacquis` / `cpnpris` / `rttacquis` / `rttpris` (point d'entrée à confirmer) ; programmes « à la demande » éditeur (`CORRIGEINITCP`, `IMPORTRECAPPAIE`, …).

## 14. Objets de données
Préfixes : `SAL.` (salarié), `EMP.` (emploi/contrat), `BUL.` (bulletin), `ETA.` (établissement), `STE.` (société), `CUM.` / `CumP_` (cumuls), `MtPart.` (montants particuliers).
→ Catalogue détaillé des variables dans **`SILAE_CODE_GLOSSARY.md`**, fonctions dans **`SILAE_FONCTIONS_CALCUL_CATALOGUE.md`**.
