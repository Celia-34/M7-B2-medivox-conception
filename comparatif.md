# Comparatif des 3 options — 4 dimensions

| Dimension | A — ML classique | B — Hybride LLM → ML | C — Multi-agents |
|---|---|---|---|
| **Conformité** *(qualifiée)* | **Intermédiaire** : minimisation, traçabilité et supervision humaine prévues. Risques : usage clinique et base légale non qualifiés ; conservation et accès à cadrer. Maîtrise : registre, DPIA si requise, matrice d'accès, revue métier/juridique. | **Intermédiaire** : mêmes garanties, mais transfert et traitement supplémentaires des comptes-rendus de santé par le LLM. Risques : fournisseur/localisation et réutilisation des données. Maîtrise : hébergement approuvé ou auto-hébergement, chiffrement, habilitations, traçabilité et DPIA si requise. | **Intermédiaire** : traçabilité par agent et supervision prévues. Risques : surface élargie, LLM et orchestration plus difficiles à prouver ; injection de prompt. Maîtrise : passerelle sans outil ni accès données, liste blanche, journaux par étape, DPIA si requise et revue juridique. |
| **Performance** *(chiffrée)* | F1 de la classe « prolongé » : **à mesurer** ; abstention si entrée invalide, hors distribution ou incertaine ; p95 **≤ 200 ms**, débit **≥ 10 req/s**, erreur **< 1 %**. Risque : relations non linéaires ou dérive dégradant le rappel. | F1 avec variables extraites : **à mesurer par comparaison contrôlée** ; extraction p95 **≤ 3 s/document**, ML p95 **≤ 200 ms**, erreur technique **< 1 %**, champs acceptés **≥ 95 %** (cible). Risque : extraction erronée ou omission. | p95 **≤ 1,5 s/décision** avec LLM, **≤ 400 ms** en mode dégradé, débit **≥ 3 req/s**, erreur **< 1 %** ; abstention si confiance insuffisante ou budget dépassé. Risque : latence cumulée et boucles. |
| **Sobriété** *(chiffrée)* | **~50 €/mois**, **~0,0003 €/prédiction**, **1 vCPU / 512 MiB**, 0 appel LLM. Risque : coûts cachés de revue, stockage et réentraînement. | **~125 / ~420 / ~1 150 €/mois** selon palier LLM petit/intermédiaire/grand, soit environ **0,001 à 0,008 €/document** ; 1 appel LLM/document, sortie plafonnée à 300 tokens. Risque : tarif, tokens, réessais et relecture humaine. | **~200 à ~950 €/mois**, soit **~0,0015 à 0,006 €/décision** ; 2 vCPU et 1,5 GiB par instance, 1 appel LLM/décision. Risque : orchestration et observabilité fixes, même en mode dégradé. |
| **Évolutivité** *(qualifiée)* | **Intermédiaire** : contrat, API et pipeline versionnés facilitent sources, versions et volumes. Points de rupture : partitionnement, multi-sites et qualité des nouvelles données. Maîtrise : tests de charge, contrats et déploiement progressif. | **Intermédiaire** : schéma et contrats facilitent l'ajout de champs et sources. Points de rupture : débit/tarif LLM, formats, dérive des formulations et capacité de relecture. Maîtrise : file d'attente, limitation de débit, versionnement et retour au ML tabulaire. | **Forte pour ajouter des capacités, intermédiaire pour le volume** : agents remplaçables via graphe et état partagé. Points de rupture : latence, coût cumulés et complexité. Maîtrise : contrats d'interface, budgets d'itérations/coût, tests de charge et déploiement progressif. |

> **Évolutivité** = capacité à intégrer de **nouvelles sources de données**, à
> faire **évoluer le modèle** et à **augmenter le volume** sans refonte majeure.
> Même définition pour les 3 options — sinon vous comparez des choses différentes.

> **Chiffré vs qualifié** : seules **sobriété** et **performance** se chiffrent.
> Conformité et évolutivité se **qualifient** — niveau *faible / intermédiaire /
> fort* + justification + mesures de maîtrise. Une conformité « 7/10 » est une
> fausse précision.

> Chaque cellule : ordre de grandeur **chiffré** (ou niveau **qualifié**) +
> **risque principal**.

## Hypothèses (obligatoire)

Tout chiffre du tableau renvoie à une hypothèse listée ici. Ce sont des
**ordres de grandeur fondés sur des hypothèses explicites, pas des mesures de
production** : le but est de rendre les options comparables, pas de prédire le
coût réel.

| # | Hypothèse | Valeur retenue | Source / date |
|---|---|---|---|
| H1 | Volume (dossiers/mois) | 5 000 séjours/jour, 1 décision par séjour, soit ~150 000/mois | ressource, 23/09/26 |
| H2a | Tokens B, extraction par compte-rendu (in / out) | 2 000 / 150 tokens JSON | option B, 23/09/26 |
| H2b | Tokens C, explication par décision (in / out) | 800 / 250 | option C, 23/09/26 |
| H3 | Coût de revue manuelle | 5 % de 5 000 dossiers/jour et ~2 min par cas, ≈ 1 ETP, soit **~4 000 €/mois** | estimé, 23/09/26 |
| H4 | Tarifs LLM publics retenus (entrée / sortie) | petit : ~0,20 / ~0,60 €/M tokens ; intermédiaire : ~1 / ~3 ; grand : ~3 / ~9 | ordre de grandeur, 23/09/26 |
| H5 | Coût ML et ressources A/B | ~50 €/mois ; A : 1 vCPU / 512 MiB | options A/B, 23/09/26 |

> ⚠️ **Performance de l'option B** : aucun gain annoncé sans le **protocole
> d'ablation** qui le prouverait (même modèle avec / sans les variables
> extraites, même jeu de test, même métrique) — cf. `ressources/02`.
