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

## Test import `IMPORTSILAE` + `MAJCPN-1` du 2026-07-07 — ⚠ RÉSULTAT NON CONFORME (+3 j en trop)

**OBSERVÉ (captures écran, salarié test matricule 1003082, entré 03/04/2023 — le cas « 3 j pris 11-13/05 » revalidé en vidéo) :**
- Mécanique d'import OK : profil `MAJCPN-1` rattaché en **colonne pérenne** via le profil utilisateur `PCCN01` (« Variables ») ✓ ; EV de **juillet 2026** : colonne `NbjCPN-1` = **3.00**, une seule ligne, totaux 3.00, triangle vert ✓.
- Bulletin de juillet : bandeau **CP N-1 = 35.00 / 3.00 / 32.00** (CP N = 4.17/0/4.17 ✓ = 2 × 2.0833 ; RTT 9/2/7).
- Ligne **`B01 Commissions` = 3.00** en zone brut (brut 6 329.67 = SdB 6 326.67 + 3.00) — valeur **exactement égale** au nombre de jours importé.

**Attendu (voie A, trade-off assumé) : 32 / 3 / 29** (acquis 29+3, pris 3 inchangé, solde 29). **Observé : 35 / 3 / 32 → l'acquis N-1 a pris +6 au lieu de +3** (DÉDUIT : baseline juillet = juin = 29/3/26, le N-1 n'acquiert plus). Un excédent de **+3 j** sur acquis et solde, alors que l'EV de juillet n'en porte que 3.
**Baseline confirmée (2026-07-07)** : les 3 salariés corrigés manuellement avant l'EH étaient les cas à **10, 8 et 1 j** — le salarié test (3 j) n'en faisait pas partie ; aucun forçage antérieur ne peut expliquer le +3. L'hypothèse « correction manuelle antérieure » est **écartée** ; restent le résidu de saisie sur un autre mois et la double colonne EV (dont la saisie historique via « Ajouter un profil », la voie KB d'origine de `MAJCPN-1`, non pérenne — faite avant le rattachement PCCN01 ?).

**Check discriminant n°1 (à faire en premier)** : bulletin de **juillet** → Éléments calculés → bloc « Jours de congés acquis/pris sur le bulletin » → ligne **Acquis**, colonne **Période de référence (N-1)** :
- **= 3.0000** → le +3 excédentaire vient d'un **autre bulletin** → contrôler la grille EV de **juin** (colonne `NbjCPN-1` : résidu du test unitaire antérieur ?) et les Élém. calculés de juin (Acquis N-1 forcé ?).
- **= 6.0000** → le doublement a lieu **sur juillet même** → défiler la grille EV de juillet vers la droite : **deux colonnes `NbjCPN-1`** (une ajoutée jadis via « Ajouter un profil » dans les EV — la voie dont la fiche dit que l'import ne fonctionne pas — plus la colonne pérenne PCCN01) ; sinon profil compté deux fois (remonter à l'assistance).

**Checks complémentaires :**
- Popup « Compteurs CP » + « Solde de repos » (juillet) : décomposition de l'acquis N-1 — le +3 apparaît-il en double (« Jours acquis N-1 » + « Jours acquis N-1 Report ») ? Provision N-1 gonflée de 2 × 3 j ?
- Fiche société (point ouvert) : **report auto du solde à la clôture** — un report des 3 j non consommés de l'ancien N-1 au 31/05 expliquerait aussi +3, et changerait **tout le chiffrage de masse** (les salariés n'auraient alors rien perdu). Contre-indice : juin observé = 29/3/26 sans report.
- **Ligne `B01 Commissions` 3.00** : coïncidence suspecte avec la valeur importée → vérifier la provenance (colonne EV Commissions ? code EV du CSV apparié à deux colonnes ?). Si l'import alimente B01, il y a un **impact en euros dans le brut** — bloquant absolu pour la masse. Si ce sont de vraies commissions de 3.00 €, lever le doute et tracer.

### ✔ Cause identifiée (2026-07-07) — code du profil `MAJCPN-1` (OBSERVÉ, déposé dans `sources/raw/2026-07-07_STD_MAJCPN-1_code-profil.txt`)

```silae
NB = saisie("NbjCPN-1",0)
Prov = BUL.CpProvAcquiseRef - Bul.CpProvAcquiseRefParReport
Nbcp = BUL.CPNBJACQUISREF - Bul.CpNbjAcquisRefParRepport
...
//Call AffecteCPAcquisRef ( Nb )
Call AffecteCPAcquisRef ( Nb + Bul.CPReportJours )   //Ni 05032026 #137665 - Tenir compte des CP acquis par report lors de l'ajout sur mois suivant mois de cloture
call AjouteProvCpRef  (Nb * (Prov / Nbcp))
```

- Le profil **force** (`Affecte`, pas `Ajoute`) le champ Élém. calculés « Acquis / Période de référence » du bulletin à **`NB + Bul.CPReportJours`** — correctif éditeur **#137665 du 05/03/2026**.
- **DÉDUIT (arithmétique)** : 35 = 29 + 3 (NB) + 3 → **`Bul.CPReportJours` = 3** pour le salarié test, soit **exactement son solde ancien N-1 au 31/05**. Le dossier **reporte donc le solde non consommé à la clôture** (`SAL_ClotureCPReport` / option société — le point ouvert « report auto » est en réalité ACTIF, au moins pour lui).
- **Conséquence majeure (À CONFIRMER en priorité)** : si le report est natif, le compteur **s'auto-corrige à partir de juillet sans aucun import** (juillet natif attendu : 29+3 report / 3 / **29** = la cible). L'import des 86 serait alors **inutile et sur-créditerait tout le monde** de la valeur importée — exactement ce qu'on observe sur le test. Et les **3 corrigés manuels (10/8/1 j)** seraient à re-contrôler : leur correction de juin + le report de juillet = **sur-crédit du même montant**.

**Checks de confirmation (rapides) :**
1. **Salarié témoin NON importé**, bulletin de juillet en brouillon : bandeau CP N-1 attendu `(acquis+report)/pris/solde-cible` (ex. 32/3/29) et Élém. calculés « Acquis / réf » montrant le report → confirme l'auto-correction.
2. **Salarié test** : Élém. calculés juillet « Acquis / réf » attendu **6.0000** (= Affecte 3+3) → confirme la lecture du code.
3. Les **3 corrigés manuels** : bandeau juillet — si solde > cible du montant corrigé, le report double leur correction de juin.
4. `B01 Commissions 3.00` : **non expliqué par ce code** (le profil ne génère aucune ligne) → vérification distincte maintenue.

**Si confirmé** : annuler la saisie importée du salarié test (Saisie des EV juillet → **Réinitialiser les saisies** → F5), **abandonner l'import de masse**, laisser le report de clôture faire la correction des compteurs, re-contrôler l'EH sur juillet, et re-chiffrer le seul sujet restant : l'écart d'**ICP** (10ème nouvelle vs ancienne période) payé en juin, qui n'est pas corrigé par le report.

**Décision : import complet (86 lignes) SUSPENDU** tant que l'excédent +3 et la ligne B01 ne sont pas expliqués. La mécanique d'import elle-même est validée (colonne pérenne + CSV + « OUI » fonctionnent).
**Correction selon cause** : résidu EV sur juin → effacer la saisie de juin (modif compteur sans impact DSN, mais bulletin de juin réédité — mois payé/DSN transmise, décision à tracer) ; doublement structurel du profil → abandonner la voie A, basculer voie B (Élém. calculés) ou voie C (ticket éditeur).
**Rappel** : même corrigée, la voie A affichera 32/3/29 (trade-off assumé) — l'affichage exact 29/0/29 n'existe qu'en voie B/C.

## Correctif V2 retenu (2026-07-07) — profil `REGULCPN1` (retrait du pris + provision consommée)

> **⚠ RÉVISÉ le 2026-07-07 (même jour, écrans de mai)** : la V2 est finalement **INUTILE et écartée** — voir la section « Décision finale : V1.1 seule » plus bas. Conclusion conservée ici pour traçabilité (§7 AGENTS.md).

**Lecture du solde de repos (OBSERVÉ, salarié test)** : 07/2026 `JP N-1 = -3.0000` (correction V1, tracée) · 06/2026 `JA N-1 Rep = +3.0000` (report — le « +3 » d'acquis), `JP +3`, `PC +1 212,43` (provision consommée de l'imputation fautive) · mai : `Anc = 4.0`, `Delta = 0.0268` (epsilon de clôture). **Taux moteur confirmé : Provision journalière = PA ÷ JA report inclus** (12 932,58 / 32 = **404,14**).
**Verdict** : `AjouteCPPrisRef` ne touche que les jours — `PC N-1` reste chargée (3 × 404,14) → **V2 obligatoire** pour re-créditer la provision consommée.

**Code V2 (remplace `REGULCPN1`) :**

```silae
Begin
// BB le 07/07/2026 : Régul CP pris mai saisis sur juin - retire x jours du pris N-1 + provision consommée
x = Saisie("CP.RegulPris",0)
If x <> 0.0 Then
	Tx = 0
	If Bul.CpNbjAcquisRef <> 0 Then Tx = Bul.CpProvAcquiseRef / Bul.CpNbjAcquisRef
	If Bul.Fonction = Fonction.CALCULNORMAL Then
		x = Min(x, Bul.CpNbjPrisRef)
		Call AjouteCPPris2 ( -x, -(x * Tx) )
	EndIf
EndIf
End
```

(Réf. doc éditeur : « `AjouteCPPris2` mouvemente CP Pris **et** la provision ; `AjouteCPPris` que les CP Pris ».)

**Ordre opératoire (salarié test)** : ① purge du report : `MAJCPN-1` avec `NbjCPN-1 = -3`, F5 → cible **JA N-1 = 29** au solde de repos, puis retrait définitif de MAJCPN-1 du PCCN01 ; ② test de réversibilité (vider `CP.RegulPris`, F5 → pris doit revenir à 3) ; ③ V2 + `CP.RegulPris = 3`, F5 → cible **29/0/29** + `PC N-1` re-créditée (1 212,43 → 0), PA intacte — fallback si dérive : `AjouteCPPrisRef(-x)` + profil standard `PROVCONSN1` ; ④ F5 ×2 → stabilité. Quatre feux verts → fichier d'import des 86 (`EV-CP.RegulPris`).

### Test V1.1 du 2026-07-07 (copie de dossier, salarié test) — cœur validé, artefact résiduel

La version **V1.1** (`AjouteCPPrisRef(-x)` seul, V2 `AjouteCPPris2` en commentaire) a été testée d'abord. Le dump technique (`REF_Langage_Silae_Syntaxes_et_variables.txt`) documente `AjouteCPPrisRef` (pris **période de référence** explicitement) et la liste des passes de calcul (`CALCULNORMAL`, `CALCULPRIMES`, `CALCULVIRTUELBRUT`, `DETERMINEPRIMESMAJORATIONHEURESSUP`, …) — le garde `Fonction.CALCULNORMAL` est la protection contre l'exécution multi-passes.

**Résultats (OBSERVÉ)** : colonne `CP.RegulPris` créée via `REGULCPN1` ajouté au conteneur `PCCN01` ✔ · saisie 3 → **Pris N-1 : 3 → 0** ✔ · aucune ligne bulletin ✔ · brut strictement intact (6 326,67) ✔ · CP N / RTT intouchés ✔.
→ **Point de vigilance n°1 (Min) levé par l'observation** : le retrait a bien opéré sur juillet, donc `Bul.CpNbjPrisRef` se lit en cumul de la période de référence, pas champ du seul bulletin courant.

**Compteur = 32/0/32** au lieu de 29/0/29 : le +3 d'acquis est un **résidu des essais MAJCPN-1 sur ce même salarié** (l'écriture de report `AffecteCPAcquisRef` persiste après retrait de la saisie et du profil — les écritures de report **survivent aux recalculs**). Preuve attendue au solde de repos : « Jours acquis N-1 Report : 3 ». Purge : re-brancher `MAJCPN-1`, `NbjCPN-1 = -3`, recalcul (`Affecte(-3 + report 3) = 0`), puis retrait définitif du PCCN01.
**Conséquence** : les **86 de prod ne sont pas concernés** par ce résidu (import complet jamais lancé ; les 3 corrections manuelles étaient en Éléments calculés) → **point n°2 quasi levé** ; le contrôle d'un témoin non importé reste une assurance à 2 minutes avant l'émission du fichier.
**Deux explications concurrentes du « +6 » initial restent en lice** (À CONFIRMER, tranchées par la purge) : (a) exécution multi-passes du MAJCPN-1 standard, qui n'a **pas** de garde `Fonction.CALCULNORMAL` ; (b) `Bul.CPReportJours` = 3 porté par les données du salarié (report de clôture). Le résultat de la purge et le témoin diront laquelle tient.

**Choix d'objet (capitalisé)** : correction en **profil de prime** (pas en fonction calcul) — seule voie ouvrant une colonne EV importable en masse ; exécution par salarié dans la chaîne des primes ; conforme au choix éditeur (MAJCPN-1 est un profil). Branchement via le conteneur `PCCN01` existant (PCCN02 possible pour isoler, mais non éprouvé sur ce dossier — prod sur PCCN01). Après campagne : retirer du PCCN01 les onglets `REGULCPN1` et `MAJCPN-1`.

### ⚠ Points de vigilance relevés avant l'import de masse (revue du 2026-07-07)

1. **`Min(x, Bul.CpNbjPrisRef)`** : si `Bul.CpNbjPrisRef` est un champ **du bulletin courant** (juillet = 0, le pris étant enregistré sur juin — même sémantique que `BUL_CPJoursPrisRef` en EH), le Min écrase x à 0 et **la V2 ne fait rien** alors que la V1 fonctionnait. Symptôme au test ③ : aucun `JP -3` au solde de repos. Correctif : retirer le Min (les valeurs des 86 sont déjà validées par l'EH + lignes de bulletins) ou le baser sur le cumul. À CONFIRMER au test ③ avant toute généralisation.
2. **Salarié témoin avant d'émettre le fichier** : contrôler au solde de repos d'un **non-importé** (brouillon juillet) s'il porte lui aussi une ligne `Rep +N` native. Si oui (report de clôture natif pour tous) : la seule régul du pris amènerait les 85 à `(29+N)/0/(29+N)` — il faudrait alors **deux colonnes** par salarié (purge report `NbjCPN-1 = -N` + `CP.RegulPris = N`). Si non : plan actuel OK.
3. **Les 3 corrigés manuels (10/8/1 j)** : pris déjà à 0 — si le report natif existe pour eux, leur besoin est **la purge seule** (pas de CP.RegulPris), sinon rien. À trancher avec le même contrôle qu'au point 2.
4. Rappel prérequis import : la colonne `CP.RegulPris` doit être **pérenne** (profil de prime utilisateur type PCCN01), pas ajoutée via « Ajouter un profil » dans les EV.
5. Au test ① : selon que `Bul.CPReportJours` est porté par juin ou juillet, la purge peut se matérialiser soit par la disparition de la ligne `Rep`, soit par une ligne `-3` compensatrice sur juillet — le critère de réussite est **JA N-1 = 29 en net**, pas la forme de l'affichage.

## Décision finale (2026-07-07) — V1.1 seule, V2 écartée (preuve chiffrée)

**Écrans de mai (OBSERVÉ, salarié test)** — deux enseignements :

1. **Validation chiffrée finale du diagnostic initial** : ancienne période 2024-25 = **30 acquis** (25 + 4 ancienneté + **1 fractionnement** — colonne Frac découverte chez lui) / 27 pris / solde 3. Les 3 j pris 11-13/05 la soldaient à 30/30, au jour près.
2. **Mécanisme de valorisation de la provision (OBSERVÉ)** : `Solde de Provision = solde de JOURS × taux journalier courant` (ou × maintien journalier si plus favorable) — **pas** « provision acquise − provision consommée » :
   - Mai N-1 : 3 j × 274,93 = **824,79** affiché ✔ (PA−PC donnerait −659,82) ; vue synthétique 875,88 = 3 × 291,96 (maintien > 10ème) ✔
   - Juillet N-1 : 32 j × 404,14 = **12 932,58** affiché ✔

**Conséquences :**
- Dès que le **solde de jours** est corrigé, la provision comptable est juste automatiquement. La « provision consommée » restée chargée est une **colonne d'historique interne** — elle ne part pas en compta.
- Argument supplémentaire contre la V2 en masse : le re-crédit se ferait au **taux de juillet** alors que le débit a eu lieu au **taux de juin** → provisions consommées résiduelles fausses, voire négatives (salarié test : **−40,10 €**). Inacceptable sur 86 salariés.

**Décision : `REGULCPN1` reste en V1.1** (`AjouteCPPrisRef(-x)` seul). Si passé en V2 : revenir à la V1.1.

### Plan final (3 gestes)

1. **Purge du report fantôme** (salarié test uniquement) : `MAJCPN-1` rebranché 5 min, `NbjCPN-1 = -3`, F5 → **29/0/29** → retirer MAJCPN-1 du PCCN01.
2. **F5 ×2** → compteur figé (la borne `Min` neutralise le profil : pris déjà 0 → retrait 0).
3. **Import de masse** : `import_REGULCPN1_complet.csv` (86 lignes, 508 jours, code `EV-CP.RegulPris` — PII, hors base) → `Traitement mois > Import de données variables` → `IMPORTSILAE` → calcul des bulletins de juillet.

**Contrôle final** : EH « Contrôle compteurs CP clôture » **sans filtre** sur juillet → solde N-1 = acquis partout (29/29 ; 27/27 pour le cas à 27) ; les 3 « oranges » vérifiés individuellement ; puis **nettoyage** (retirer `REGULCPN1` et `MAJCPN-1` du PCCN01). Clôture de la fiche sur l'EH post-import + note de synthèse client (préventif mai 2027 : CP de mai saisis sur mai, ou méthode 147 re-testée à froid).

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
| 2026-07-07 | Ajout section « Test import IMPORTSILAE + MAJCPN-1 » : résultat non conforme (35/3/32 au lieu de 32/3/29, +3 j en trop), import de masse suspendu, checklist de discrimination | Résultat de test observé (captures Blandine) | Assistant (session Claude) |
| 2026-07-07 | **Cause identifiée** : code MAJCPN-1 observé — `AffecteCPAcquisRef(Nb + Bul.CPReportJours)` (#137665) → CPReportJours=3 déduit → report de clôture actif, auto-correction probable en juillet, import de masse remis en cause | Code du profil transmis par Blandine | Assistant (session Claude) |
| 2026-07-07 | **Changement de voie** : abandon MAJCPN-1, profil custom `REGULCPN1` (V1.1 `AjouteCPPrisRef`, V2 `AjouteCPPris2` si PC ne suit pas) + test V1.1 validé (pris 3→0, brut intact), artefact 32/0/32 = résidu report MAJCPN-1 à purger ; lecture solde de repos ; points de vigilance pré-import | Tests Blandine + dump technique | Assistant (session Claude) |
| 2026-07-07 | **V2 écartée** (provision = solde jours × taux courant, PC = historique interne ; re-crédit au taux de juillet ⇒ résidus faux, ex. −40,10 €) → **décision finale V1.1 seule** + plan final 3 gestes (purge, F5×2, import 86) et contrôle EH sans filtre | Écrans de mai (Blandine) | Assistant (session Claude) |
