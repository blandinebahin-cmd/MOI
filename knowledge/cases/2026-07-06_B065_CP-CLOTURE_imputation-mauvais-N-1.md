> **Note de dépôt (2026-07-07)** — Cas capitalisé tel que transmis, contenu conservé intact.
> Statut : **DIAGNOSTIC, en attente de validation Blandine** (§7 AGENTS.md — la validation finale sera tracée dans la section Révisions en bas de fichier).
> Documents référencés : [`../SILAE_COMPTEURS_GESTION_MANUELLE.md`](../SILAE_COMPTEURS_GESTION_MANUELLE.md) (déposé le 2026-07-07) ; `SILAE_METHODES_CP.md` (emplacement réservé dans [`../`](../), contenu à déposer).
> Sources brutes (vidéo, exports XLSX/CSV) volontairement **hors base** : données nominatives (§4.4), conservées dans `Downloads\` côté poste de travail.

---

# Cas — CP pris en mai saisis en juin : imputation post-clôture sur le mauvais N-1 (SYNTEC B065)

**Date d'analyse :** 2026-07-06 — **statut : DIAGNOSTIC, en attente de validation Blandine (§7 AGENTS.md)**
**Source :** enregistrement écran `Downloads\Enregistrement 2026-07-06 163448.mp4` (8 écrans extraits, janv→juin 2026). Frames non copiées dans la base (données nominatives, §4.4).
**Contexte :** dossier 1362463 (client V., Puteaux), salarié cadre SYNTEC **B065**, forfait **218 j**, entré 04/04/2022, CP en **jours ouvrés** (25/an + ancienneté), période de référence **01/06 → 31/05**, clôture mai. Pratique du dossier : **absences saisies en décalé** (absences de M-1 sur le bulletin de M) — observée systématiquement depuis janvier. Version Silae 1.2115.9 – BC 1137 du 02/07/2026.

## Compteurs observés (bandeau bulletin + popup « Compteurs CP »)

| Bulletin | CP N-1 (A/P/S) | CP N (A/P/S) | Lignes CP du bulletin (dates réelles) |
|---|---|---|---|
| Janv 2026 | 31.00 / 12.00 / 19.00 | 16.64 / 0 / 16.64 | — |
| Fév 2026 | 31.00 / 12.00 / 19.00 | 18.73 / 0 / 18.73 | RTT pris 02/01 (1 j) |
| Mars 2026 | 31.00 / **17.00** / 14.00 | 20.81 / 0 / 20.81 | CP pris 02-06/02 (5 j) |
| Avril 2026 | 31.00 / 17.00 / 14.00 | 22.89 / 0 / 22.89 | — |
| Mai 2026 | 31.00 / **21.00** / **10.00** | **29.00** / 0 / 29.00 | CP pris 07-10/04 (4 j) ; **D02 congés ancienneté +4.00** ; JS 25/05 |
| **Juin 2026** | **29.00 / 10.00 / 19.00** | 2.08 / 0 / 2.08 | **CP pris 07/05 (1 j) + 11-15/05 (4 j) + 25-29/05 (5 j) = 10 j** |

Décomptes de jours corrects (fériés exclus : 06/04 Pâques, 08/05, 14/05 Ascension → « 4 j » pour 11-15/05 ✓). CP N = 2.0833/mois + 4 j ancienneté en mai (FC type CP-ANCIENNETE, mai) → 29.00 à la clôture. ICP janv-mai : retenue 288.4885/j, indemnité **326.23/j** ; juin : retenue 288.3553/j, indemnité **385.67/j**.

## Conclusion (DÉDUIT des compteurs, cohérent sur 6 mois)
Les **10 CP pris en mai** (07/05, 11-15/05, 25-29/05) — qui soldaient exactement l'ancien N-1 (solde 10.00) **avant le 31/05** — ont été saisis, selon la pratique du décalage, **sur le bulletin de juin, donc après la clôture**. Silae les a imputés sur le **nouveau** N-1 (ex-CP N 2025-26 : 29 → 19) ; l'ancien solde N-1 de 10 j a disparu à la clôture sans être consommé (pas de ligne de report ni de perte visible).

**Écart : −10 jours.** Attendu au 01/06 : N-1 = 29/0/**29**. Observé : 29/10/**19**. En réalité le salarié a consommé son N-1 à 31/31 avant l'échéance.

## Impact € probable (À CONFIRMER via bulle ICP)
Les 10 j de juin ont été indemnisés à 385.67/j (10ème de la **nouvelle** période, commissions incluses) au lieu de 326.23/j (10ème de l'ancienne période, taux appliqué à toutes les prises janv-mai). Sur-indemnisation ≈ **+594.40 € brut** si l'imputation correcte est retenue.

## Correction proposée (jamais en prod sans test — §4.3)
1. Dossier de test : **régul compteur CP N-1 Pris −10** (saisie compteurs CP standard, ou profil type `CPPRIS_REG` de la bibliothèque GDLP pour régulariser pris + provision) → cible N-1 = 29/0/29. Tracer en commentaire que l'ancienne période a été soldée 31/31.
2. Décider du sort de l'écart d'ICP (~594 €) : régul ou maintien (décision client).
3. Contrôles : popup Compteurs CP, solde de repos, provision CP au recalcul suivant, bulletin de juillet.
4. **Préventif** : pour le mois de clôture (mai), saisir les absences sur le bulletin de mai — ou valider l'option dossier de décalage adaptée (fiche société, À CONFIRMER) ; balayer les **autres salariés** du dossier ayant des CP de mai saisis en juin (même mécanique ⇒ même écart).

## Correctif retenu (2026-07-06, à tester) — méthode 147
Décision : **conserver la pratique** (CP de mai traités sur le bulletin de juin) et poser la **méthode 147 « Décalage arbitrage des CP après le mois de clôture » = 1** (défaut 0 = arbitrage au mois de clôture ; description complète dans `SILAE_METHODES_CP.md`). La régul manuelle de compteur (§ ci-dessus) reste le plan B si le test 147 n'est pas concluant.
**Hypothèse à valider en test** : avec 147=1, les prises de mai saisies en juin s'imputent sur l'ancien N-1 (juin attendu : N-1 = 29/0/29, ancien N-1 soldé 31/31).
**Vérifications exigées (avant/après, 1 salarié impacté + 1 témoin)** : ① aucun changement du taux d'indemnisation 10ème sur le témoin (toutes lignes iso) ; ② taux PAS strictement identique (le montant PAS ne bouge que si le net imposable bouge) ; ③ sur le salarié impacté, l'ICP des 10 j devrait revenir au 10ème de l'ancienne période (326.23/j vs 385.67/j, ≈ −594 € brut) — changement **attendu et correct**, à faire valider ; ④ D06 prime de vacances (10 % ICP) et provision CP ; ⑤ ⚠ juin payé le 30/06 et DSN juin probablement transmise (échéance 5/07) → test sur **copie de dossier**, et décision régul si correction de juin en prod.

## Correction de masse (~90 salariés impactés) — plan validé sur sources
**Détection/chiffrage — édition historique** (mode Bulletins, juin 2026 ; fonctions officielles fiches EH 2 & 7) :
Code édition `CPCLOTURE`, libellé « Contrôle compteurs CP clôture », ordre `A0010`.

```
En-tête :
Begin
colonne010.titre="CP N-1 Acquis"
colonne020.titre="CP N-1 Pris"
colonne030.titre="CP N-1 Solde"
colonne040.titre="Jours pris réf. (juin)"
colonne050.titre="Jours pris anticipé (juin)"
colonne060.titre="Montant CP réf. (juin)"
colonne070.titre="Provision N-1"
colonne080.titre="CP N Acquis"
colonne090.titre="CP N Pris"
End

Lignes :
Begin
colonne010 = CompteurCP("CPN1ACQUIS")
colonne020 = CompteurCP("CPN1PRIS")
colonne030 = CompteurCP("CPN1ACQUIS") - CompteurCP("CPN1PRIS")
colonne040 = BUL_CPJoursPrisRef        // jours imputés période de réf. par le bulletin de juin -> correction Pris N-1
colonne050 = BUL_CPJoursPrisAnt        // jours imputés en ANTICIPÉ par juin -> correction côté N si <> 0
colonne060 = BUL_CPMtRef               // chiffrage € (montant des jours pris réf.)
colonne070 = ProvisionsCP("CPN1ACQUIS")
colonne080 = CompteurCP("CPNACQUIS")
colonne090 = CompteurCP("CPNPRIS")
If BUL_CPJoursPrisRef = 0 and BUL_CPJoursPrisAnt = 0 then LigneExclue = true
End
```

(Matricule + nom présents par défaut. Générer sur **juin 2026**, mode Bulletins.)
**Correction de masse RETENUE (2026-07-07) : import EV standard `IMPORTSILAE` + profil `MAJCPN-1`.**
Décision Blandine : pas de saisie manuelle → import. Seule voie d'import 100 % standard : les EV (le champ « Éléments calculés » ne s'importe pas ; import compteurs = mode montage uniquement).
- Fichiers générés (PII, hors base) : `Downloads\import_MAJCPN1_TEST_1salarie.csv` (1 ligne, salarié test 3 j) et `Downloads\import_MAJCPN1_complet.csv` (**86 lignes, 508 j** = 83 standards + 3 « oranges » confirmés ; 2 sorties exclues). Format officiel (fiche 16538753683730 + FAQ) : CSV `;`, décimales `,`, **ligne 1 = en-tête jamais importée**, colonnes `Matricule;Code;Valeur;Date debut;Date fin`, code EV = `EV-NbjCPN-1` (intitulé d'origine de la colonne du profil — à vérifier au test, régénérable).
- **Prérequis bloquant documenté** : la colonne doit être **pérenne** → ajouter `MAJCPN-1` via **profil de prime utilisateur / PCCN1** (`Paramétrage > Primes > Profils`) ; ⚠ « si la prime est ajoutée via *Ajouter un profil* dans les EV, l'import NE FONCTIONNERA PAS » (fiche officielle). Autre condition : **bulles bulletins rouges** (non calculés) → import sur **juillet**.
- Menu : `Traitement mois > Import de données variables` → Import 1 = `IMPORTSILAE` → fichier → « OUI ». Annulation : Saisie des EV > **Réinitialiser les saisies**.
- Protocole : ① test 1 salarié → calcul bulletin juillet → contrôles (compteur cible 32/3/29 : solde 29 ✓ ; provision N-1 ; provision consommée → si à recaler, profil `PROVCONSN1` massifiable par le même canal) ; ② import complet ; ③ contrôle EH **sans filtre** (dupliquer l'édition, retirer la ligne `If … LigneExclue`) : solde N-1 = acquis réel partout.
- Trade-off assumé : acquis/pris affichés gonflés (+N/+N) ; l'exactitude d'affichage 29/0/29 n'existe qu'en unitaire (Élém. calculés) ou via l'éditeur.

**Voies de correction :**
- **A. Masse standard (≈1 h)** : profil `MAJCPN-1` (+N acquis N-1 + provision) pour les 90 via saisie EV de masse / import EV ; contrôler `PROVCONSN1`. ⚠ bulletin affichera acquis/pris gonflés (ex. 39/10/29).
- **B. Exacte mais unitaire (2-3 h)** : Éléments calculés « Pris N-1 » → 0 par salarié (29/0/29 propre). Semi-automatisable sous supervision.
- **C. Éditeur** : ticket Assistance (« Accompagnement au déploiement ») avec le fichier EH — correction en masse côté Silae, délai inconnu.
- **Écarté (sourcé)** : import Excel des compteurs CP = menu « Initialisation des congés payés », **mode montage uniquement** (fiche 16539066395666) ; FAQ Import : compteurs auto-calculés depuis les jours pris importés, pas d'import direct des compteurs en prod.
**Contrôle post-correction** : relancer la même EH (cible : colonne020 = 0 pour les 90), solde de repos/provisions, 2 bulletins témoins.

**Résultat EH du 2026-07-07 (généré sur juin, après correction manuelle de 3 salariés — exclus par le filtre ✔) :**
- **88 salariés détectés** · **566 j** pris réf. (juin) · 3 j anticipés · montant CP réf. total **377 683 €** · provision N-1 totale **1 594 722 €**.
- **83 standards** : acquis 29 (25+4 ancienneté ; 1 cas à 27), pris compteur = pris juin → correction = champ « Pris N-1 » (Élém. calculés juin) → **0**.
- **2 sorties/STC probables (exclues de la correction)** : compteurs 0/0/0 + 29 j réf. + 1-2 j anticipés soldés sur juin (dont un montant 39,5 k€) → prises post-clôture **légitimes** en solde de tout compte, à vérifier puis ne pas toucher.
- **3 divergents à inspecter avant correction** : compteur Pris N-1 > pris du bulletin de juin (11≠4, 3≠1, 12≠8) → détail des prises à vérifier (prises réelles de juin ? reprise ?) ; correction attendue = champ juin → 0 (le compteur global absorbe).
- Décimales possibles (5.5, 7.5…) → le champ Élém. calculés accepte les demi-journées.
- ⚠ La colonne « Montant CP réf. » = valorisation brute des jours (`BUL_CPMtRef`), **pas** le trop-versé 10ème (chiffrage séparé).
- Checklist opérationnelle générée (PII, hors base) : `Downloads\correction_CP_juin2026_checklist.xlsx` (83 à faire / 3 à vérifier / 2 exclus).

**Validation croisée (2026-07-07)** avec l'export Silae « Lignes de bulletins associées » (`Downloads\VMWare CP.xlsx`, 197 lignes CP des bulletins de juin, dates réelles + taux journaliers) :
- **86/86 salariés alignés** (somme des jours des lignes = PrisRef de l'EH) → deux sources indépendantes concordent.
- Sorties confirmées : un salarié avec 13 j de lignes vs 29 PrisRef (+16 soldés à la sortie sans ligne) ; l'autre avec **0 ligne** CP et 29 PrisRef (100 % solde STC) → exclusion de la correction validée.
- 2 salariés avec prises d'**avril** saisies sur juin (1 j du 30/04 ; 5 j du 20-24/04) → antérieures au 31/05, même correction.
- Le « divergent » à 4 j : lignes = PrisRef = 4 ✓ (l'écart de compteur 11 vs 4 est antérieur à juin — vérif détail maintenue).
- Exemple re-validé en vidéo (cadre B065 entré 04/2023, 3 j pris 11-13/05) : mai = ancien N-1 30/27/**3** (3 j de solde, pris pile avant le 31/05) ; juin = nouveau N-1 29/**3**/26 + Élém. calculés « Pris N-1 = 3.0000 » → correction 3→0, cible 29/0/29, ancien N-1 réellement soldé 30/30. Pattern identique aux autres cas.

## Points ouverts
- Réglage fiche société : report auto du solde, mois de clôture, option décalage (non visibles dans la vidéo).
- Confirmation du 10ème par période via la bulle de détail ICP.
- Ligne « Journée de solidarité 25/05 » incluse dans les 5 j 25-29/05 : vérifier que la JS doit bien décompter 1 CP selon la règle du dossier (mineur).
- Pas d'impact DSN des compteurs internes ; l'ICP de juin est dans le brut déclaré.

## Test méthode 147 — résultats observés (2026-07-06)
Salarié test = cadre SYNTEC forfait 218 j à **forte part variable** (commissions ~20 k€/mois) — anonymisé (§4.4). Comparaison **AVANT** (duplicata prod, 147=0) vs **APRÈS** (brouillon, 147=1), même mois (juin 2026), 1 RTT (04/05) + 1 CP (15/05) saisis sur juin. Bulletins hors base (PII).

| Élément | Avant (147=0) | Après (147=1) | Δ |
|---|---|---|---|
| CP N-1 (Acq/Pris/Solde) | 29 / **1** / 28 | 31,50 / **0** / 31,50 | acquis +2,5 · pris −1 |
| Ligne CP 15/05 (retenue 579,93 + ICP 811,94) | présente | **absente** | arbitrage 10ème −232,01 |
| Prime vacances D06 | 2 354,64 | 2 347,92 | −6,72 |
| Brut | 35 629,60 | 35 390,87 | **−238,73** (= 232,01 + 6,72) |
| Net imposable | 29 361,62 | 29 165,45 | −196,17 |
| **Taux PAS** | **26,00 %** (perso DGFiP → 31/08/2026) | **26,00 %** | **= ✅** |
| Montant PAS | 7 634,02 | 7 583,02 | −51,00 (= 26 % × Δbase) |
| Compteur RTT | 9/1/8 | 9/1/8 | = |

**Conclusions du test :**
- **Taux PAS : confirmé inchangé** (26 %, taux personnalisé DGFiP exogène). Le montant suit mécaniquement la base. ✔
- **Taux 10ème : non "changé" mais le CP a été RETIRÉ de juin** (compteur pris 1→0, ligne + arbitrage supprimés). ⚠ **147 ne conserve pas le CP sur juin — contraire à l'objectif.**
- **Statut 147 : NON CONCLUANT en l'état.** À départager (info Blandine) : (a) artefact — EV vidée au recalcul, CP à re-saisir sur le brouillon de juin ; (b) effet réel — 147 attend le CP sur le bulletin de **mai**. Reste à réconcilier le +2,5 j acquis (29→31,5) avec le bulletin de mai.
- **Prochain test** : reposer 147=1 → **re-saisir** le CP 15/05 sur juin → recalculer → vérifier imputation sur ancien N-1 + valorisation 10ème ancienne période ; contrôler mai en parallèle.

## Forçage retenu — écran « Éléments calculés » (2026-07-06, 2e salarié test)
Champ de forçage **confirmé à l'écran** (OBSERVÉ) : bulletin → volet droit **« Éléments calculés »** → bloc **« Jours de congés acquis/pris sur le bulletin »** → colonnes **Période de référence (N-1)** / **Période anticipée (N)**, lignes **Acquis** / **Pris** (éditables).
Exemple (salarié test 2, cadre B065, forfait 218 j, anonymisé — bulletins hors base) : 6 CP (15-22/05) saisis en juin → **Élém. calculés juin : Pris N-1 = 6.0000**, compteur CP N-1 = 29/**6**/23 (le nouveau N-1 fraîchement acquis à 29 = 25 SYNTEC + 4 ancienneté D02, a été débité à tort). Mai (clôture) : Pris N-1 = 2.0000 (2 CP d'avril, imputation correcte sur l'ancien N-1 31/23/8).
**Correction :** forcer **Élém. calculés → Pris → Période de référence (N-1) : nb de jours de mai saisis en juin → 0.0000** (ici 6→0) → **F5** → cible **29/0/29**.
**À vérifier au recalcul (non garanti) :** ① la ligne « Congés payés pris » + ICP restent-elles (forcer le compteur ≠ supprimer la ligne, mais F5 peut recalculer le champ depuis la ligne) ; ② compteur 29/0/29 ; ③ taux `092 = SdB/21.67` et PAS inchangés. **Plan B si le forçage est écrasé :** profil `MAJCPN-1 +N` (ajout acquis, gonfle provision). Retenue observée = `Salaire de base / 21.67` (méthode 092 défaut N) ✔.

**✔ Confirmation officielle (KB « Solder et modifier des compteurs de congés / repos », MAJ 01/07/2026)** : « *L'ajustement du nombre de jours de CP N passe **uniquement** par le module "Éléments calculés" du bulletin de paie* » → la voie retenue est la voie standard éditeur. Profils complémentaires si l'ajustement doit toucher les **provisions** : `PROVCPN‑1` / `PROVCPN` (provision acquise) et **`PROVCONSN1`** / `PROVCONSN` (provision **consommée** — à contrôler après remise à 0 du Pris, si les jours imputés à tort ont alimenté la conso N‑1). Astuce officielle : profils acquis/pris acceptent des **quantités négatives**. Détail → `SILAE_COMPTEURS_GESTION_MANUELLE.md`.

---

## Révisions

| Date | Changement | Raison | Auteur |
|---|---|---|---|
| 2026-07-07 | Dépôt initial dans `knowledge/cases/` (statut DIAGNOSTIC) | Capitalisation §7 AGENTS.md | Assistant (session Claude) |
