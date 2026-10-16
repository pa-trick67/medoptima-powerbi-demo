# MedOptima – Démo Power BI (PBIP)

Projet de démonstration de la conférence **« De l'Excel au Power BI : transformer ses données en un véritable outil de pilotage »**.

MedOptima Distribution est un distributeur de parapharmacie **fictif** (4 régions, 12 commerciaux, 120 pharmacies, 23 produits). Toutes les données et adresses e-mail sont fictives.

## Contenu

- `Pilotage commercial MedOptima.pbip` – point d'entrée à ouvrir dans Power BI Desktop
- `MedOptima.SemanticModel/` – modèle sémantique au format TMDL
- `Pilotage commercial MedOptima.Report/` – définition du rapport (PBIR)

## Ouvrir le projet

1. Cloner le repo puis ouvrir `Pilotage commercial MedOptima.pbip` dans Power BI Desktop.
2. Dans **Transformer les données > Gérer les paramètres**, renseigner `CheminFichier` avec le chemin local du classeur `MedOptima_Distribution_Donnees.xlsx` (non inclus dans le repo).
3. Actualiser.

## Modèle sémantique

Modèle en étoile : 3 tables de faits, 5 dimensions et une table de mesures.

| Table | Rôle |
|---|---|
| `Fait_Ventes` | Lignes de commande (quantité, remise, CA HT, coût HT) |
| `Fait_Objectifs` | Objectifs mensuels de CA par commercial (2025-2026) |
| `Fait_Stocks` | Stock de fin de mois par produit et région |
| `Dim_Produit`, `Dim_Pharmacie`, `Dim_Commercial`, `Dim_Region` | Dimensions métier |
| `Dim_Date` | Table de dates calculée en DAX, bornée sur les dates des ventes et des stocks |
| `_Mesures` | Table dédiée aux mesures |

Relations :

- `Dim_Date` filtre `Fait_Ventes[Date]`, `Fait_Stocks[Date_Stock]` et `Fait_Objectifs[Période]` (grain mois, via `Dim_Date[AnnéeMoisNum]`)
- `Dim_Commercial` filtre `Fait_Ventes` et `Fait_Objectifs` ; `Dim_Region` filtre `Dim_Commercial` et `Fait_Stocks`
- `Dim_Produit` filtre `Fait_Ventes` et `Fait_Stocks` ; `Dim_Pharmacie` filtre `Fait_Ventes`

## Mesures (35)

| Dossier | Mesures |
|---|---|
| **01 Ventes** | CA HT, CA brut HT, Remise accordée, Taux de remise %, Coût HT, Marge HT, Taux de marge %, Quantité vendue, Nb commandes, Nb lignes de vente, Panier moyen, Prix de vente moyen, Nb pharmacies actives, Nb produits vendus |
| **02 Temps** | CA HT N-1, Écart CA vs N-1, Écart % CA vs N-1, CA HT YTD, CA HT YTD N-1, Écart % YTD vs N-1, CA HT M-1, CA HT 12 mois glissants, Marge HT YTD |
| **03 Objectifs** | Objectif, Objectif YTD, Écart vs objectif, Taux atteinte %, Taux atteinte YTD %, Statut objectif |
| **04 Stocks** | Stock unités, Valeur stock HT, Date du stock, Nb jours écoulés, Ventes moyennes par jour, Couverture stock (jours) |

## Rapport

3 pages :

1. **Pilotage commercial**
2. **Produits & stocks**
3. **Performance commerciale**

## Points démontrés

- Transformation Power Query d'un tableau Excel « mis en forme pour un humain » (objectifs)
- Fonction Power Query réutilisable (`fx_loadData`) et paramètre de chemin (`CheminFichier`)
- Modèle en étoile avec table de dates et table de mesures organisée en dossiers
- Time intelligence (N-1, YTD, 12 mois glissants) et suivi des objectifs
- Intégration Git avec le format PBIP / TMDL
