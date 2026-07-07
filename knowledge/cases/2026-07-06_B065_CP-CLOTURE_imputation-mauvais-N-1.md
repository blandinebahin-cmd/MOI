> **Note de dépôt** — Fiche alignée le 2026-07-08 sur la version maître consolidée de Blandine (fichier local `2026-07-06_B065_CP-CLOTURE_cp-pris-mai-saisis-juin.md`), contenu conservé intact. Les versions antérieures de la fiche (structure de ce dépôt) restent consultables dans l'historique git. Statut : **correction VALIDÉE sur salarié test (29/0/29, brut intact) — import de masse imminent**.
> Sources brutes (vidéos, captures, exports CSV/XLSX) volontairement **hors base** : données nominatives (§4.4), conservées dans `Downloads\` côté poste de travail. Documents référencés : `SILAE_METHODES_CP.md` (emplacement réservé), [`../SILAE_COMPTEURS_GESTION_MANUELLE.md`](../SILAE_COMPTEURS_GESTION_MANUELLE.md), [`../PATRONS_PROFILS_PRIMES_ET_FC.md`](../PATRONS_PROFILS_PRIMES_ET_FC.md), [`../../sources/raw/REF_Langage_Silae_Syntaxes_et_variables.txt`](../../sources/raw/REF_Langage_Silae_Syntaxes_et_variables.txt).

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
- **Test 1 salarié (2026-07-07) — import OK, branchement KO :** colonne `NbjCPN-1` alimentée à 3.00 sur juillet ✔ (mécanique IMPORTSILAE validée), MAIS bulletin de juillet : ① **ligne parasite `B01 Commissions` base 3.00 → +3,00 € de brut** (la valeur EV a déclenché une *prime* : une « prime » a été créée dans le conteneur au lieu d'un renvoi vers le *profil* standard) ; ② compteur N-1 = **35/3/32** au lieu de 32/3/29 attendu (+6 = **double exécution** probable de MAJCPN-1 — ajouté deux fois / deux canaux). → Rollback : `Saisie des EV juillet > Réinitialiser les saisies` + refaire bulletin (retour attendu 29/3/26, brut 6 326.67). Construction correcte : dans le **PCCN01 existant** (un seul par dossier — deck formation EV/profils), « + » → **« Ajouter un profil »** (≠ « Ajouter une prime ») → sélectionner **MAJCPN-1** (liste des profils pré-paramétrés) → Sauver ; supprimer tout profil `MAJCPN-1` créé à vide au niveau dossier (risque de masquage du standard) et toute « prime » au libellé de colonne NbjCPN-1. Le profil compteur ne doit générer **aucune ligne** de rémunération.
- **Re-test après branchement PCCN01 (vidéo 2026-07-07 14h12)** : PCCN01 « Variables » du dossier contient bien un onglet-**profil** `MAJCPN-1` (via « Sélection ») ✔ ; le 2ᵉ écran « Profils primes » = création vierge non sauvée (pas de masquage) ✔ ; compteur juillet = **32/3/29 = CIBLE ATTEINTE** ✔ ; le +6 initial a disparu (doublon purgé au réinit/re-import) ; Élém. calculés juillet sains (Pris N-1 0, l'ajout MAJCPN-1 passe en report compteur, pas en « acquis du bulletin ») ✔ ; historique mai intact (Pris N-1 4.0000) ✔. **Reste UNE anomalie : ligne `B01 Commissions` base 3.00 → +3,00 € de brut** — une *prime* du PCCN01 (probable résidu du 1er essai « Ajouter une prime », code modèle B01, libellé de colonne homonyme `NbjCPN-1`) consomme la même saisie. → passer en revue TOUS les onglets du PCCN01 (flèches ◄►) + « Ouvrir dans tableur » la saisie EV pour repérer la colonne/prime en doublon → **Enlever la prime**, réinitialiser les saisies juillet, re-importer le TEST, refaire : attendu **32/3/29, brut 6 326.67 strictement, zéro ligne B01**. Alors seulement → import des 86.

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

## Solution finale retenue (2026-07-07 soir) — profil spécifique `CP.REGULPRIS` (V1 à tester sur copie)
Le script du standard `MAJCPN-1` (fourni par Blandine) + la bibliothèque GDLP (`CPPRIS_REG`, `CPSUP_GDLP`) débloquent tout :
- **+6 expliqué** : le standard `MAJCPN-1` écrit les compteurs **sans le garde** `Bul.Fonction = Fonction.CALCULNORMAL` → écriture rejouée sur les passes de calcul (« la saisie EV est doublée » — commentaire GDLP). Défaut du profil **standard** → à signaler à l'éditeur (réf. patch #137665).
- **Écriture du PRIS possible** : `AjouteCPPris(-x)` / `AjouteCPPris2(-x, -mt)` (GDLP `CPPRIS_REG`) → correction **exacte** (29/0/29), massifiable par import EV, sans MAJCPN-1.

**Profil V1.1** (mis à jour 2026-07-07 après dump technique — `AjouteCPPrisRef` confirmé = cible période de référence explicite) :

```silae
Begin
// BB le 07/07/2026 : Régul CP pris mai saisis sur juin (clôture 31/05) - retire x jours du pris N-1
x = Saisie("CP.RegulPris",0)
If x <> 0.0 Then
	Prov = Bul.CpProvAcquiseRef - Bul.CpProvAcquiseRefParReport
	Nbcp = Bul.CpNbjAcquisRef - Bul.CpNbjAcquisRefParRepport
	Tx = 0
	If Nbcp <> 0 Then Tx = Prov / Nbcp
	If Bul.Fonction = Fonction.CALCULNORMAL Then      // garde anti-doublement (19 passes de calcul)
		x = Min(x, Bul.CpNbjPrisRef)                  // borne : jamais de pris négatif
		Call AjouteCPPrisRef ( -x )                   // décrémente le PRIS période de référence (N-1)
		// V2 si la provision consommée ne suit pas au test :
		//Call AjouteCPPris2 ( -x, -(x * Tx) )
	EndIf
EndIf
End
```

**Protocole de test (copie de dossier, salarié test 3 j)** : ① sauvegarde du profil (valide l'existence des fonctions) ; ② saisie 3 → calcul → compteur attendu **29/0/29** (vérifier que c'est bien le pris **N-1/Ref** qui bouge, pas le N) ; ③ **F5 ×2** → compteur stable (idempotence) ; ④ solde de repos : provision consommée — si elle ne se recale pas, V2 avec `AjouteCPPris2(-x, -(x×Tx))` ; ⑤ brut strictement inchangé (aucune ligne générée). Puis 1 salarié divergent (pris 11≠4) : cible 7. Ensuite import de masse (fichier à régénérer : Code `EV-CP.RegulPris`, mêmes 86 lignes, valeurs positives).

**Lecture du Solde de repos post-V1.1 (détail « Utilisation des CP », salarié test)** — colonnes JA/JP/SJ/PA/PC × N-1/N :
- Construction du 29 confirmée : solde initial 14.56 + 2.08×5 (janv-mai) + **4.0 ancienneté (col. Anc, mai)** + **0.0268 (col. Delta = epsilon de clôture)**.
- 06/2026 : `JA N-1 Rep +3.0000` (**report fantôme MAJCPN-1, colonne Rep**) · `JP N-1 +3.0000` (imputation fautive d'origine) · `PC N-1 +1 212.43` (= 3 × 404.14).
- 07/2026 : **`JP N-1 -3.0000` = notre `AjouteCPPrisRef(-3)` — visible et traçable dans l'historique** ✔.
- Solde final : JA 32 (29+3 rep) / JP 0 ✔ / SJ 32 · **PC N-1 = 1 212.43 INCHANGÉE → la provision consommée n'est PAS re-créditée par AjouteCPPrisRef → V2 NÉCESSAIRE.**
- **Taux moteur identifié** : « Provision Journalière » = PA/JA **report inclus** (12 932.58/32 = 404.14) → la formule Tx de la V2 doit être `Bul.CpProvAcquiseRef / Bul.CpNbjAcquisRef` (sans exclure le report — correction vs V1.1 qui copiait la formule « hors report » de MAJCPN-1, adaptée au report, pas à la conso).
**V2 (à tester)** : remplacer l'appel par **`Call AjouteCPPris2(-x, -(x*Tx))`** — le dump GDLP documente : « AjouteCPPris2 mouvemente CP Pris **et** la provision ; AjouteCPPris mouvemente que les CP Pris ». Vérifs au test : période visée (JP **N-1**), pas de double retrait de jours, PC re-créditée de 3×404.14. Fallback V2b si KO : `AjouteCPPrisRef(-x)` + profil standard `PROVCONSN1` pour la provision. Préalable : **purge du report fantôme** (MAJCPN-1 avec saisie **-3** → `Affecte(-3+3)=0`) et test de **réversibilité** (retirer la saisie → F5 → le pris doit revenir à 3 = compteur reconstruit à chaque calcul).

**REVIREMENT V2 → V1.1 (2026-07-08, preuve par les soldes de repos de MAI)** : les écrans de mai démontrent que **la provision du solde de repos = SOLDE DE JOURS × taux journalier courant** (ou × maintien journalier si supérieur), PAS « PA − PC » :
- Mai, N-1 : solde 3 j × 274.93 (=PA 8 248.04/JA 30) = **824.79** affiché ✔ (alors que PA−PC = 8 248.04−8 907.86 = −659.82) ; synthétique 875.88 = 3 × 291.96 (maintien > 10ème) ✔.
- Juillet, N-1 : 32 × 404.14 = **12 932.58** affiché ✔.
→ **Dès que le solde de JOURS est corrigé, la provision comptable est juste par construction : la V1.1 (jours seuls) SUFFIT.** La PC est une colonne d'historique interne, non transférée en compta.
→ De plus, un re-crédit PC en masse serait **valorisé au taux de juillet** alors que le débit l'a été au taux de juin → PC résiduelles fausses voire **négatives** (ex. test : débit 3×404.14=1 212.43, re-crédit post-purge 3×417.51=1 252.53 → PC −40.10). **Décision : V1.1 en masse ; V2 abandonnée** (PROVCONSN1 en réserve si un contrôle interne exigeait PC=0).
- Bonus lecture mai : ancienne période 2024-25 du salarié test = **30 acquis (25+4 anc+1 fractionnement, col. Frac)/27 pris/solde 3** → les 3 j de mai la soldaient à 30/30 ✔ (diagnostic bouclé) ; taux journalier moteur confirmé = PA/JA sur deux instantanés (274.93 mai, 404.14 juillet).
- **Fichier d'import final généré** : `Downloads\import_REGULCPN1_complet.csv` (86 lignes, 508 j, code `EV-CP.RegulPris`).

**Validation finale salarié test (2026-07-08)** : saisies `CP.RegulPris = 3` **+** `NbjCPN-1 = -3` (purge fantôme) → **compteur 29/0/29 ✔**, brut intact ✔, traçabilité parfaite (juin figé : Rep +3/JP +3/PC +1 212.43 ; juillet : Rep **-3**/JP **-3**). Confirmations : ① `REGULCPN1` ne touche pas la PA (le mouvement PA de juillet vient de MAJCPN-1) ; ② la ligne PA 07/2026 = **-1 252.53** = `AjouteProvCpRef(-3 × 417.51)` de MAJCPN-1(-3) → **résidu de labo chez le salarié test : PA sous-évaluée de 427.73 €** (+824.80 posé au taux de juin − 1 252.53 retiré au taux de juillet) — correctif optionnel : profil standard `PROVCPN-1` (+427.73, saisie unique), sinon documenter.
**⚠ DANGER identifié** : la saisie `NbjCPN-1 = -3` laissée en place = **bombe au recalcul** — au prochain F5, `Affecte(-3 + report 0) = -3` → JA 26 et PA −1 252.53 **à chaque calcul**. → **Vider la saisie NbjCPN-1 + retirer MAJCPN-1 du PCCN01 AVANT tout F5.** (Énième preuve de la non-idempotence de MAJCPN-1 standard.)
**Réponse à la question « 2 colonnes par salarié ? » : NON — les 86 de prod n'ont jamais eu MAJCPN-1 (pas de report fantôme) → une seule colonne `CP.RegulPris` (le fichier d'import est bon tel quel).**

**Résultat test V1.1 (2026-07-07, salarié test, dossier prod/juillet non validé)** : profil `REGULCPN1` créé + branché PCCN01, colonne `CP.RegulPris` = 3 → calcul → **pris N-1 : 3 → 0 ✔ · aucune ligne générée ✔ · brut intact (6 326,67) ✔ → `AjouteCPPrisRef` VALIDÉ.** ⚠ Compteur = **32/0/32** au lieu de 29/0/29 : **+3 d'acquis = report fantôme résiduel des tests `MAJCPN-1`** sur ce même salarié — **l'écriture `AffecteCPAcquisRef` persiste au compteur même après retrait de la saisie et du profil** (leçon durable : les écritures de report survivent aux recalculs ; les 86 salariés de prod ne sont pas concernés, MAJCPN-1 n'a jamais tourné sur eux). Purge : re-brancher MAJCPN-1 temporairement et saisir **-3** (`AffecteCPAcquisRef(-3 + report 3) = 0`) — à vérifier au solde de repos (ligne « Jours acquis N-1 Report »). Restent : purge → 29/0/29, F5 ×2, solde de repos/provision conso.

## Vérification croisée du dépôt (2026-07-08, sur écrans juillet + juin + mai — assistant)

**Conformes (OBSERVÉ, recalculé indépendamment) :**
- Bulletin juillet : **CP N-1 = 29/0/29** ✔ · **Brut = 6 326,67 = SdB strictement** ✔ · **aucune ligne B01** ✔ (anomalie prime résiduelle soldée) · CP N 4.17/0/4.17 ✔ · RTT 9/2/7 ✔.
- Solde de repos, cohérence arithmétique complète : JA N-1 = 14.56 + 2.08 + 3×2.0833 + 6.1101 (mai : 2.0833+4 anc+0.0268 delta) = 29.0000 puis +3−3 ✔ · JP 3−3 = 0 ✔ · PA = 12 107.78 (propre) + 824.80 − 1 252.53 = 11 680.05 ✔.
- **Règle de valorisation triple-confirmée** (3 instantanés) : Solde de Provision = solde de jours × Provision Journalière (= PA/JA) — mai : 3 × 274.93 = 824.79 ✔ · juin : 29 × 404.14 = **11 720.06** ✔ · juillet : 29 × 402.76 = 11 680.05 ✔.
- **Résidu PA −427.73 € confirmé par l'écran** : PA propre attendue 12 107.78 vs 11 680.05 (écart = +824.80 posé au taux 274.93 − 1 252.53 retiré au taux 417.51). Salarié test uniquement ; correctif optionnel `PROVCPN-1` +427.73, sinon documenter.
- « Une seule colonne pour les 86 » ✔ ; bonus : le salarié test présent dans le fichier est inoffensif (`Min(3, pris 0) = 0` → no-op).

**Non conformes / à faire immédiatement :**
1. ⚠ **CONFIRMÉ À L'ÉCRAN EV (juillet)** : la saisie **`NbjCPN-1 = −3.00` est toujours en place** et la colonne existe toujours (MAJCPN-1 toujours branché au PCCN01) → **bombe au recalcul armée**. Vider la saisie + retirer MAJCPN-1 du PCCN01 **avant tout F5 / calcul de masse**.
2. Après purge : **re-contrôler 29/0/29 par un F5** — la persistance des écritures de report joue dans les deux sens (le +3 avait survécu au retrait de sa saisie ; vérifier que le −3 tient aussi). Si le compteur revient à 32/0/32, re-poser la purge autrement avant l'import.
3. Trace à documenter : les écritures de labo sont datées **06/2026** (Rep +3, PA +824.80, PC +1 212.43), mois payé/DSN transmise — sans impact DSN (compteurs internes) mais l'historique du mois validé porte ces traces ; à mentionner dans la note de synthèse.

**GO import des 86** conditionné à : purge faite + F5 stable à 29/0/29 + EV de juillet du salarié test propre (seul `CP.RegulPris` restant, ou rien).

## Runbook d'exécution final (2026-07-08, profil nettoyé)

Séquence exécutée/à exécuter — détail dans la réponse assistant du 2026-07-08 : **A.** désamorçage (vider `NbjCPN-1`, retirer MAJCPN-1 du PCCN01) → **B.** revalidation salarié test (F5, cible 29/0/29, brut 6 326,67 strict, F5 ×2 figé, solde de repos) → **C.** import `import_REGULCPN1_complet.csv` sur bulles rouges + calcul de juillet → **D.** EH sans filtre sur juillet (pris 0 / solde = acquis partout sauf 2 sorties), 3 oranges en unitaire, 2 témoins, spot-check solde de repos → **E.** ⚠ **REGULCPN1 et les saisies restent en place jusqu'à la validation de juillet** (la correction est recalculée à chaque calcul : retirer le profil ou réinitialiser les EV avant validation = tout annuler) ; nettoyage du PCCN01 en août (saisie non reportée, profil inerte). Rollback à tout moment avant validation : `Réinitialiser les saisies` + recalcul.

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
| 2026-07-07 | Test import MAJCPN-1 non conforme (35/3/32), cause identifiée dans le code du profil, bascule `REGULCPN1` V1.1/V2, lecture solde de repos, V2 écartée sur preuve chiffrée | Sessions de test Blandine | Assistant (session Claude) |
| 2026-07-08 | **Alignement complet sur la fiche maître locale** (version consolidée : B01 expliqué = prime résiduelle du PCCN01, re-test 32/3/29, solution finale `CP.REGULPRIS`, revirement V2→V1.1, validation finale 29/0/29, danger saisie `NbjCPN-1 = -3`) — structure antérieure du dépôt conservée dans l'historique git | Fiche maître transmise par Blandine (« / vérifie / ») | Assistant (session Claude) |
| 2026-07-08 | Vérification croisée écrans juillet/mai : 29/0/29 ✔, brut 6 326.67 strict ✔, B01 absent ✔, arithmétique PA (−427.73) confirmée ✔ ; ⚠ saisies `NbjCPN-1 = -3` et colonne MAJCPN-1 **encore en place à l'écran EV** → purge avant tout F5 | Vérification demandée par Blandine | Assistant (session Claude) |
