# Catalogue des profils standards Silae (KB officielle)

**Source :** centre d'aide Silae `support.silae.fr/hc/fr/sections/16162913557522-Profils`, section **Paramétrages > Primes et indemnités > Profils**, lue le **2026-07-06** via la session authentifiée de Blandine (Claude-in-Chrome ; le site renvoie 403 en accès anonyme). **Statut : OBSERVÉ** (titres/index de la KB — corps des articles à ingérer à la demande, non encore lus sauf mention).

> Ce sont les **profils standards éditeur** (à distinguer des profils du dossier GDLP dans `PATRONS_PROFILS_PRIMES_ET_FC.md`, qui sont du code client). ~85 profils, 3 pages.

## ⭐ Congés payés / compteurs / provisions (notre sujet)

### `MAJCPN-1` — Ajouter des jours au compteur CP N-1 acquis (OBSERVÉ, art. Silae MAJ 10/07/2025)
**Objectif :** ajoute des jours au compteur des **jours acquis N-1** ; ils entrent aussi dans la **provision CP N-1**.
**Saisie :** bulletin → **Saisie des éléments variables** → volet droit **« Ajouter un profil »** → filtrer **`MAJCPN-1`** → une colonne **`NbjCPN-1`** apparaît → saisir le **nombre de jours** à ajouter (triangle vert = enregistré).
**Effet :** CP N-1 acquis +N (« Compteurs CP ») ; provision recalculée (« Solde de repos ») : jours ajoutés en « Jours acquis N-1 Report » + « Jours acquis N-1 », nouveau solde provision = provision avant report + provision du report (ex. donné : 1208,57 + 185,93 = 1394,50).
**⚠ Priorité :** entre un forçage « Éléments calculés » et le profil, **la valeur saisie dans la colonne EV du profil prime** (dans les deux sens).

### Levier révélé par l'article : forçage direct « Éléments calculés »
On peut **forcer les jours acquis (report) N-1 directement dans le bulletin via « Éléments calculés »** (sans profil) → piste pour forcer aussi les **jours pris** (champ exact **à confirmer**).

### `PROVCPN` / `PROVCPN-1` — Mise à jour de la provision CP (article à lire).

### Application correction CP 2026 (cf. `cases/2026-07-06_B065_CP-CLOTURE_…`)
Défaut = N jour(s) **faussement compté(s) en "Pris"** sur le nouveau N-1 → solde court de N.
- **Exact** : forcer **Jours pris N-1 −N** (« Éléments calculés ») → **29/0/29**, sans toucher acquis ni provision. *(champ à confirmer)*
- **Documenté (`MAJCPN-1`)** : ajouter **+N en acquis** → restaure le **solde** mais **gonfle l'acquis (30) et la provision** → à éviter si on veut l'exactitude comptable.

## 🔧 Forçage / Annulation / Régularisation (famille « correction »)
| Profil(s) | Objet |
|---|---|
| `FORC-FNAL7` / `FORC-FNAL8` | Forcer le calcul du FNAL |
| `FORC-EXO26` | Forcer l'exonération cotisations chômage < 26 ans |
| `FORCE-FS8` / `ANNULE-FS8` | Calcul / annulation forfait social 8 % |
| `ANNULE-FS` · `ANNUL-FS20` · `ANNUL-FS16` | Annulation forfait social (intéressement/participation/PEE ; 20 % ; 16 %) |
| `FOR-TXAPPR` / `ANN-TXAPPR` | Forcer / annuler la taxe d'apprentissage |
| `FOR-TAXSAL` / `ANN-TAXSAL` · `REG-TAXSAL` · `CUM-TC005/A` | Forcer / annuler / régulariser la taxe sur les salaires |
| `FORC-TC002` / `AN-TC002` | Forcer / annuler la participation à l'effort construction |
| `AN-SS011.4` / `AN-AA311.4` | Annuler les réductions générales de cotisations patronales |
| `ANN-CIFCDD` · `ANN-DI005` | Annuler contribution CPF-CDD / FAFSEA CDD |
| `EXOHSEXCLU` | Exclure un salarié de l'exonération sociale/fiscale HS |
| `RGCPROSMIC` | Forcer le prorata SMIC RGC/RGDU |
| `SS061` · `CFP` | Forcer/annuler/régulariser CFP (artisan / cotisation) |
| `REG-TX-AA` | Régularisation des taux de retraite |
| `REGULRS` · `REGULRS2` · `REGULRF` · `RFS` | Régularisation réintégration sociale/fiscale, forfait social |
| `REGRETSRC` | Régularisation de la retenue à la source |
| `REGULFISC` · `NIZERO` · `MASQUE_NI` · `NETSOUHAIT` | Annuler / mettre à zéro / masquer le net imposable ; définir un net souhaité |
| `MAJSDBASE` / `MAJSCONV` | Régulariser un salaire |
| `TRS` / `TRSAD` | Calcul / annulation / régularisation versement transport (+ additionnel) |
| cotisations `MSA` | Annulation rétroactive de cotisations MSA |

## Frais, mobilité, avantages
`IKVELO` (forfait mobilités durables) · `MOBILITE1` · `FRAISFORF` · `FPDIRECT` (frais pris en charge directement) · `FORMHTT` (allocation formation HTT) · allocation forfaitaire **télétravail** (AIDEDIRECT = services à la personne) · `BONSKDO` (bons cadeaux) · `CESU`/`CESU2` · `GDPLCTOM` / `GDPCTOMXc` (grands déplacements OM / étranger) · `HDEL` (heures de délégation en annexe).

## Primes / dispositifs sectoriels
`BONUSDOM` (vie chère OM) · `Laforcade` (médico-social) · `SOCIOEDU` · `PRMEDECIN` · `MEDAILLE` (médaille du travail) · `DOMMAGES` (dommages-intérêts) · `ESSAIPRO` (essai professionnel) · `REDESPOPRO` (sportifs) · `ROYALT` (artistes/mannequins) · `VIECHEREDO` · `BSPCE` · `SKOPTIONS`/`SKOPTION2`/`SKOPTIONL` (stock-options) · `SOUSCAPIT1/2` (SCOP) · `LOCAGERANC`/`LOCAGERA` (location gérance) · `CRPNMIXTE` (navigants) · `INTOCATODE` (TODE intermittents) · `JRSFILLON` (Fillon sans horaire) · `IJPREV`/`IJPREV2`/`IJPREVLM…` / `IJPREV.SD` (IJ prévoyance) · `INDCOMPCSG` · `TAXEMEDIC` · `SDBFORMAT` (formateurs FFP/PRAA) · `PRMEDECIN` · majoration dimanche (accord Saint-Malo).

> **À faire :** ingérer le corps des articles prioritaires (`MAJCPN-1`, `PROVCPN-1`, la famille forçage) via la session Chrome — les titres seuls ne suffisent pas pour la saisie exacte (§4.1).
