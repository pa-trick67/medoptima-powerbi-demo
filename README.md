# MedOptima – Démo Power BI (PBIP)

Projet de démonstration de la conférence **« De l'Excel au Power BI : transformer ses données en un véritable outil de pilotage »**.

MedOptima Distribution est un distributeur de parapharmacie **fictif** (4 régions, 12 commerciaux, 120 pharmacies, 23 produits). Toutes les données, adresses e-mail et codes postaux sont fictifs.

## Contenu

- `Rapport MedOptima.pbip` – point d'entrée à ouvrir dans Power BI Desktop
- `Rapport MedOptima.SemanticModel/` – modèle sémantique au format TMDL
- `Rapport MedOptima.Report/` – définition du rapport (PBIR)

Le classeur source `MedOptima_Distribution_Donnees.xlsx` **n'est pas inclus** dans le dépôt.

## Ouvrir le projet

1. Cloner le dépôt, puis ouvrir `Rapport MedOptima.pbip` dans Power BI Desktop.
2. Dans **Accueil > Transformer les données > Gérer les paramètres**, renseigner `CheminFichier`
   avec le chemin complet du classeur `MedOptima_Distribution_Donnees.xlsx` sur votre poste.
3. Actualiser. `Dim_Date`, `_Mesures` et `Dim_Pharmacie[Ville_Geo]` sont calculées : elles ne
   se remplissent qu'après cette actualisation.

## Le modèle

Modèle en étoile, 9 tables.

| Table | Rôle |
|---|---|
| `Fait_Ventes` | lignes de commande (date, pharmacie, produit, commercial, CA, coût) |
| `Fait_Objectifs` | objectifs mensuels par commercial, au grain `yyyyMM` |
| `Fait_Stocks` | photo de stock par produit et région |
| `Dim_Produit`, `Dim_Pharmacie`, `Dim_Commercial`, `Dim_Region` | dimensions |
| `Dim_Date` | calendrier DAX, plage déduite des données, marquée comme table de dates |
| `_Mesures` | 35 mesures DAX, rangées en 4 dossiers |

Deux points méritent un mot, parce qu'ils ne se devinent pas à la lecture du modèle :

- **`Fait_Objectifs` est relié à `Dim_Date[AnnéeMoisNum]` en plusieurs-à-plusieurs**, en sens
  unique. Aucune des deux colonnes n'est unique, une relation un-à-plusieurs est donc
  impossible. Le sens unique évite un chemin de filtre parasite
  `Dim_Commercial → Fait_Objectifs → Dim_Date → Fait_Ventes`.
  Conséquence : filtrer une partie de mois renvoie l'objectif du **mois entier**.
- **Les mesures de stock sont semi-additives.** `Stock unités` et `Valeur stock HT` utilisent
  `LASTNONBLANK` pour prendre la dernière photo de la période, pas la somme des photos.

### Géocodage

Les codes postaux du classeur sont fictifs : seul le préfixe départemental est exploitable.
La colonne calculée `Dim_Pharmacie[Ville_Geo]` reconstruit une adresse non ambiguë
(`Ville, Département, France`) et porte la catégorie `Address`. Sans elle, Bing plaçait
Saint-Denis (93) à La Réunion.

## Le rapport

Trois pages, thème clair personnalisé `MedOptima_Clair`.

| Page | Contenu |
|---|---|
| Pilotage commercial | KPI, CA réalisé vs objectif par mois, classement des commerciaux, détail par commercial |
| Produits & stocks | CA et marge par catégorie et produit, volume × marge, couverture de stock |
| Performance commerciale | suivi individuel, CA vs N-1, taux d'atteinte, territoire couvert |

La palette est validée pour fond clair : bande de clarté, plancher de chroma, séparation en
vision deutan/protan et contraste WCAG. Les couleurs de statut sont toujours accompagnées du
libellé `Statut objectif`, jamais seules.

## Points démontrés

- Transformation Power Query d'un tableau Excel « mis en forme pour un humain » (objectifs)
- Modèle en étoile, table de dates, time intelligence
- Intégration Git avec le format PBIP / TMDL : un `git diff` lisible sur une mesure DAX
