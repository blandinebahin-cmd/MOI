# Gestion manuelle des compteurs (CP N/N-1, CP Sup, RTT, RCR/RCO/RCC) — solder, modifier, payer

**Sources (KB officielle, lues le 2026-07-06) :** *« Solder et modifier des compteurs de congés / repos »* (MAJ **01/07/2026**) → `sources/raw/kb/Gestion_courante_paies/Gestion_des_compteurs/33255274312466_*` · *« Paramétrer un compteur en jours / heures (CP Sup, RCR, RCO, RCC) »* (MAJ 03/07/2026) → `17605495651730_*`. **Statut : OBSERVÉ** (éditeur).

## ⭐ Règle officielle — ajuster les compteurs CP N / N-1
> **« L'ajustement du nombre de jours de CP N passe uniquement par le module "Éléments calculés" du bulletin de paie. »**

Écran : bulletin → volet droit **Éléments calculés** → bloc **« Jours de congés acquis/pris sur le bulletin »** → colonnes **Période de référence (N-1)** / **Période anticipée (N)** × lignes **Acquis** / **Pris** (éditables). C'est la voie standard pour forcer un compteur CP (cf. cas `2026-07-06_B065_CP-CLOTURE_…` : Pris N-1 6→0).

## Profils d'ajustement compteurs & provisions CP (mutation / reprise / réembauche / correction)
| Profil | Effet |
|---|---|
| `MAJCPN-1` | MAJ compteur CP **et provision N-1** (colonne EV `NbjCPN-1` ; prime sur le forçage Élém. calculés — voir catalogue) |
| `PROVCPN` / `PROVCPN-1` | MAJ **provision** CP N / N-1 |
| `PROVCONSN` / `PROVCONSN1` | MAJ **provision consommée** N / N-1 |

💡 **Astuce officielle :** pour solder/modifier un compteur **sans paiement**, utiliser les profils *acquis*/*pris* **avec des quantités négatives ou positives**.

## Payer des jours/heures acquis (profils de solde)
`RCOPRIS` (heures RCO) · `RCRPRIS` (RCR, indemnité = majoration HS) · `RCRPRIS2` (RCR sans majoration) · `RCCPRIS` (RCC) · `RTTPRIS` (jours RTT) · `CPSUPPRISP` (CP supplémentaires).
Saisie : EV → « Ajouter un profil » → nb de jours/heures + montant unitaire. Les jours payés passent en **« pris »** au compteur.

## Alimenter / consommer les compteurs standards
- **Jours :** `CPSUP` / `CPSUP2` (acquis) · prise via **Activité** (motif CP Sup) **ou** profils `CPSUPPRIS` / `CPSUP2PRIS`.
- **Heures :** `RCRACQUIS` · `RCOACQUIS` · `RCCACQUIS` (acquis) · prise via **Activité** (motifs RCO/RCR/RCC, préciser les heures sinon = heures travaillées du jour) **ou** profils `RCRPRIS3` / `RCOPRIS3` / `RCCPRIS3`. Heures RCR majorées : profils `HSxxRR` (menu Heures).
- ⚠ **Affichage bulletin :** via **Activité**, le pris **apparaît** sur le bulletin ; via **profil**, il **n'apparaît pas** (compteur seul).
- 📌 Préco éditeur : personnaliser plutôt **RCC ou RCR** ; **RCO réservé** au retraitement des HS (contingent annuel).

## Notes
- CP ancienneté & fractionnement : rattachés par défaut au compteur légal CPN/CPN-1 ; gérables dans un compteur à part (service Onboarding Produit Paie / paramétrage avancé).
- Libellés des compteurs de repos non personnalisables en standard (sauf forçage du masque BP) ; masques dispo pour masquer des compteurs.
- Création/paramétrage de compteurs (acquisition auto) : voir `SILAE_COMPTEURS_JOURS.md`.
