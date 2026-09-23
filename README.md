# M7-B2 — Évolution du prédicteur MediVox

Ce dossier compare trois architectures pour le prédicteur de séjour prolongé.
L'objectif est de choisir une évolution maîtrisée, sans transformer le modèle
en décision autonome : la validation humaine reste requise avant toute action
patient.

## Options

- [Option A — ML classique modernisé](schemas/option_a_ML_classique_modernisé.md) : pipeline tabulaire versionné et régression logistique calibrée.
- [Option B — Hybride LLM vers ML](schemas/option_b_Hybride_LLM_ML.md) : extraction contrôlée via LLM de variables dans les comptes-rendus, puis prédiction ML.
- [Option C — Multi-agents orchestrés](schemas/option_c_Multi_agents.md) : orchestration LangGraph, avec le LLM limité à l'explication.

## Comparaison

- [Comparatif des quatre dimensions](comparatif.md) : conformité et évolutivité qualifiées ; performance et sobriété chiffrées par ordres de grandeur.
- [Note de comparaison](note_comparaison.md) : recommandation, fallbacks, procédure HITL et plan de migration.

## Décision

L'option A est retenue comme cible immédiate : elle fournit une baseline
explicable, calibrable et sobre, estimée à environ **50 €/mois**. L'option B
reste une évolution conditionnelle : elle ne sera retenue que si les variables
extraites par le LLM démontrent un gain reproductible sur le même jeu de test,
avec un coût, une latence et un niveau de conformité acceptables. L'option C
n'est pas retenue pour une prédiction tabulaire mono-étape, en raison de sa
complexité et de son coût supplémentaires.

## Principes de maîtrise

- Toute entrée invalide, incertaine ou hors distribution déclenche une abstention ou une revue humaine.
- Le fallback est distinct de la dérive : la dérive déclenche une investigation et une réévaluation contrôlée du modèle.
- Les données de santé, les versions de modèles, les extractions et les décisions sont tracées avec accès contrôlé.
- Aucun appel LLM n'est effectué si l'information est déjà disponible dans les variables structurées.

Les ressources pédagogiques utilisées sont disponibles dans [ressources](ressources/).
