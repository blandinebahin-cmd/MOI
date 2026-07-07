# Patrons de profils primes et fonctions calcul (code de production GDLP + standard)

**Statut : OBSERVÉ** — code de production transmis par Blandine le 2026-07-07, conservé intact dans `sources/raw/`. À distinguer des profils standards éditeur (`SILAE_PROFILS_STANDARDS_CATALOGUE.md`).

## Sources déposées

| Fichier (`sources/raw/`) | Contenu | Patron illustré |
|---|---|---|
| `2026-07-07_STD_MAJCPN-1_code-profil.txt` | **Code du profil standard `MAJCPN-1`** | `saisie("NbjCPN-1")` → `AffecteCPAcquisRef(Nb + Bul.CPReportJours)` + `AjouteProvCpRef(Nb × Prov/Nbcp)` ; correctif éditeur #137665 du 05/03/2026 (report). ⚠ Voir cas `cases/2026-07-06_B065_CP-CLOTURE_…` |
| `2026-07-07_GDLP_MAJCPN_ajout-acquis-provision-N.txt` | Profil `MAJCPN` (côté CP N / anticipé) | `AjouteCpAcquisAnt(Nb)` + `AjouteProvCpAnt(Nb × Tx)`, taux par défaut = `BUL.CpProvAcquiseAnt / BUL.CPNBJACQUISAnt` |
| `2026-07-07_GDLP_COMPTEUR-JREPOS_jour-repos-compensateur.txt` | Compteur custom « J.Repos » sur les rails CPSup2 | EV `Saisie(nom + ".Acquis+-")`, report manuel en janvier via `CumulStockvar`, paiement du solde en STC (`lprime_I04`, taux `SdB/21.67`), garde **`If Bul.Fonction = Fonction.CALCULNORMAL`** avec commentaire d'origine : « *sinon, la saisie faite en EV est doublée* » |
| `2026-07-07_GDLP_CPT_SILAE_stockage-compteurs.txt` | FC `CPT_SILAE` | Pont stockvar : `GDLP_RTTACQUIS/PRIS`, `GDLP_CPSUPACQUIS/PRIS`, `GDLP_CPSUP2ACQUIS/PRIS` |
| `2026-07-07_GDLP_P-FIXE_primes-fixes-EV-M1.txt` | Profil `P.FIXE` (~25 primes fixes) | `SaisieM1` (EV reportée M+1), `methodeCalcul = 2`, rattachement AED sur exercice fiscal via `marquedtdeb/fin` + `MtPart.EXERCICEFISCALSPE`, bornage entrée/sortie |
| `2026-07-07_GDLP_PROVISIONECRCPT1_provisions-HS.txt` | `PROVISIONECRCPT1` (provisions HS) | Provision d'écriture comptable : comptes 6/4 provision + charges, `BrutProvision = stockvar("NG.HS")`, `AvecExtourne = true` ; `PourcentageChargeProvision` commenté (À CONFIRMER : taux appliqué par défaut) |
| `2026-07-07_dossier-a-confirmer_PPRIME-CHAINE_execution-profils.txt` | Chaîne d'orchestration `PPrime_*` | Ordre d'exécution des profils (haut de bulletin → bas → standards → `PostPPrime2_*`), blocs « à supprimer au démarrage du dossier » |

## Patrons transverses (OBSERVÉ dans ces codes)

1. **Anti-double comptage** : les affectations de compteurs (`Affecte*`) ne doivent s'exécuter que sous `If Bul.Fonction = Fonction.CALCULNORMAL`, sinon la valeur d'EV est appliquée à chaque passe de calcul (« doublée » — commentaire GDLP).
2. **`Affecte*` vs `Ajoute*`** : `Affecte*` **remplace** la valeur du champ « Éléments calculés » du bulletin (d'où la ré-inclusion explicite de `Bul.CPReportJours` dans MAJCPN-1) ; `Ajoute*` incrémente.
3. **Report de clôture** : `Bul.CPReportJours` (jours reportés de N-1 à la clôture, cf. `SAL_ClotureCPReport`) est intégré par le MAJCPN-1 standard depuis le correctif #137665 — impact direct sur toute correction posée le mois suivant la clôture.
4. **Point de vigilance nommage stockvar** : COMPTEUR-JREPOS lit `GDLP_CPSUP2.ACQUIS` (avec point) alors que CPT_SILAE stocke `GDLP_CPSUP2ACQUIS` (sans point) → le report de janvier lirait un stockvar inexistant (À CONFIRMER : autre bloc stockant la variante avec point ?).
5. **Clients cités dans P.FIXE** (VINDIMA, OMAG, CAP CORSE, RACINE, PERRET SA, JBF) : code multi-clients de la bibliothèque GDLP — ne pas transposer tel quel sans purge des spécifiques.
