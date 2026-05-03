# 📷 Projet Vision — Reconstruction 3D par Stéréovision

Reconstruction d'un nuage de points 3D à partir de deux photos prises avec la même caméra déplacée latéralement (stéréovision passive).

## Pipeline

```
Images damier  →  Calibration caméra  →  Détection SIFT  →  Matching + RANSAC  →  Triangulation 3D  →  Nuage de points
```

| Étape | Méthode |
|-------|---------|
| Calibration | Damier 7×9, `cv2.calibrateCamera` |
| Détection | SIFT (6000 keypoints) |
| Matching | BFMatcher + ratio test de Lowe (0.8) |
| Filtrage géométrique | Matrice fondamentale + RANSAC |
| Triangulation | `cv2.triangulatePoints`, filtrage IQR |
| Visualisation | Matplotlib 3D + Plotly interactif |

## Structure du projet

```
ProjetVision/
├── ProjetVision.ipynb      # Notebook principal
├── chess/                  # Images de calibration (damier) — non incluses
│   └── *.jpg
├── 7.jpg                   # Photo stéréo gauche — non incluse
├── 8.jpg                   # Photo stéréo droite — non incluse
├── camera_params/          # Généré automatiquement à l'exécution
│   ├── K.npy
│   ├── dist.npy
│   └── points3D.npy
├── requirements.txt
└── README.md
```

> **Note :** Les images (`chess/`, `7.jpg`, `8.jpg`) ne sont pas incluses dans le repo car elles peuvent contenir des informations personnelles ou être trop lourdes. Remplace-les par tes propres photos.

## Installation

```bash
git clone https://github.com/<ton-username>/ProjetVision.git
cd ProjetVision
pip install -r requirements.txt
```

## Utilisation

1. Place tes images de calibration dans `chess/` (format `.jpg`, damier 7×9)
2. Place tes deux photos stéréo à la racine : `7.jpg` et `8.jpg`
3. Lance le notebook :

```bash
jupyter notebook ProjetVision.ipynb
```

4. Exécute les cellules dans l'ordre

## Paramètres configurables

En haut du notebook (Cellule 1) :

| Paramètre | Valeur par défaut | Description |
|-----------|------------------|-------------|
| `BASELINE` | 15.0 cm | Distance entre les deux positions de caméra |
| `TAILLE_CASE` | 2.0 cm | Taille d'une case du damier |
| `NB_COINS_X` | 7 | Coins intérieurs en X |
| `NB_COINS_Y` | 9 | Coins intérieurs en Y |
| `SHRINK_CALIB` | 0.3 | Facteur de resize pour la calibration |
| `SHRINK_STEREO` | 0.8 | Facteur de resize pour les photos stéréo |

## Résultats générés

- `output_1_sift_keypoints.jpg` — Keypoints SIFT détectés
- `output_2_sift_matches.jpg` — Correspondances après RANSAC
- `output_3_nuage_points_3D.png` — Nuage de points 3D (vue statique)
- `output_4_vue_cote.png` — Vue de côté (X-Z)
- Visualisation Plotly interactive dans le notebook

## Dépendances

Python 3.8+ — voir `requirements.txt`
