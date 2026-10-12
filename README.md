# MedOptima – Démo Power BI (PBIP)

Projet de démonstration de la conférence **« De l'Excel au Power BI : transformer ses données en un véritable outil de pilotage »**.

MedOptima Distribution est un distributeur de parapharmacie **fictif** (4 régions, 12 commerciaux, 120 pharmacies, 23 produits). Toutes les données et adresses e-mail sont fictives.

## Contenu

- `Rapport MedOptima.pbip` – point d'entrée à ouvrir dans Power BI Desktop
- `Rapport MedOptima.SemanticModel/` – modèle sémantique au format TMDL
- `Rapport MedOptima.Report/` – définition du rapport (PBIR)

## Ouvrir le projet

1. Cloner le repo puis ouvrir `Rapport MedOptima.pbip` dans Power BI Desktop.
2. Dans **Transformer les données > Gérer les paramètres**, renseigner `CheminFichier` avec le chemin local du classeur `MedOptima_Distribution_Donnees.xlsx` (non inclus dans le repo).
3. Actualiser.

## Points démontrés

- Transformation Power Query d'un tableau Excel « mis en forme pour un humain » (objectifs)
- Modèle en étoile
- Intégration Git avec le format PBIP / TMDL
