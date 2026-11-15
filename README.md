# 🐾 Chtipanda CNC – Générateur de G-code de taraudage / filetage

Générateur de G-code **100 % navigateur** (un seul fichier HTML, aucune installation) pour le **taraudage** (filetage intérieur) et le **filetage extérieur** par **fraisage hélicoïdal** avec une fraise à fileter.

Version actuelle : **v0.6.0** – ISO · UN · Gaz (BSP) · Int/Ext · G28/G30

> ⚠️ **Utilisation à vos risques.** Vérifiez toujours le G-code généré (simulation, essai à vide, Z de sécurité) avant de lancer l'usinage sur une vraie machine.

---

## ✨ Fonctionnalités

- **Taraudage (intérieur) et filetage (extérieur)**
- **Normes prédéfinies** :
  - ISO métrique (M2 à M12, y compris pas fins)
  - Impérial UNC / UNF (#4-40 à 1/2-13)
  - Gaz BSP (G) 55° (G1/8 à G1)
  - ou valeurs **personnalisées** (Ø nominal + pas)
- **Pas à gauche** possible
- **Sens d'usinage** au choix : par le bas (montée) ou par le haut (descente)
- **Alésage préalable** (Outil 1) en interpolation hélicoïdale, avec calcul automatique du Ø de perçage selon le taux de filet (%)
- **Fraise à fileter** (Outil 2) avec :
  - plusieurs passes radiales
  - jeu du filet réglable (+ plus lâche / − plus serré)
  - **compensation d'avance circulaire** (l'avance est exprimée à l'arête de la fraise)
  - fraisage en avalant ou en opposition selon le sens choisi
- **Sortie en 1 fichier unique** ou **2 fichiers séparés** (OP1 : alésage / OP2 : filetage)
- **Changement d'outil** : `M6` ou pause `M0` selon le post-processeur, retour via **G28**
- **Palpeur fixe via G30** avec mesure automatique de la longueur d'outil (et fichier de **calibration du palpeur**)
- **Contrôles de sécurité** : collisions fraise/alésoir, profondeur > cou de fraise, profondeur radiale > hauteur de dent, course de palpage, etc.
- **Visualisation** en direct : vue de dessus (XY) et vue de côté (XZ)
- **Copie** dans le presse-papiers ou **téléchargement** en `.nc`

## 🛠️ Machines / post-processeurs supportés

| Machine | Changement d'outil | Fin de programme | Palpage |
|---|---|---|---|
| LinuxCNC | `M6` | `M2` | `G38.2` (variables, mesure T2−T1 automatique) |
| Mach3 / Mach4 | `M6` | `M30` | `G31` |
| FluidNC | `M6` | `M30` | `G38.2` |
| GRBL / grblHAL | pause `M0` | `M30` | `G38.2` |

## 🚀 Utilisation

1. Ouvrir `taraudage-gcode.html` dans un navigateur (ou la version en ligne, voir ci-dessous).
2. Choisir la **machine** et le mode de sortie (fichier unique ou 2 fichiers).
3. Choisir l'**opération** (intérieur / extérieur), la **norme** et les dimensions du filet.
4. Renseigner les **outils** (alésoir, fraise à fileter), avances et vitesses de broche.
5. Si besoin, activer les options **G28** (changement d'outil) et **palpeur G30**.
6. Cliquer sur **⚡ Valider & Calculer**, vérifier les vues XY / XZ et le message d'état.
7. **Copier** ou **télécharger** le fichier `.nc` et le charger dans votre contrôleur.

Les fichiers sont nommés automatiquement, par exemple : `Chtipanda_M4_Complet.nc`, `Chtipanda_M4_OP1_Alesage.nc`, `Chtipanda_M4_OP2_Filetage.nc`.

## ⚠️ Points de sécurité importants

- Le programme remonte toujours au **« Z haut »** en position **MACHINE** (`G53`) avant chaque déplacement : réglez-le dans la section 5 et **référencez la machine (homing)** avant de lancer.
- Les positions **Z haut, G28, G30 et palpeur** sont des positions **machine**, jamais des positions pièce.
- Sans palpeur, le programme marque une pause (`M0`) pour refaire le Z0 après chaque changement d'outil.
- Avec palpeur, le palpage s'arrête **5 mm avant** le palpeur : une alarme se déclenche si aucun outil n'est monté.
- Testez d'abord **à vide** (sans outil ni matière, ou en l'air) pour valider les trajectoires.

## 📐 Formules utilisées

- Ø d'alésage = Ø nominal − `1.299` × pas × taux de filet (ISO / UN)
  *(`1.2807` pour le gaz BSP)*
- Profondeur radiale du filet extérieur : `0.6134` × pas (ISO / UN) ou `0.6403` × pas (BSP)
- Profondeur de filet arrondie au nombre de tours entier : `n = ⌈profondeur / pas⌉`

## 🌐 Version en ligne (GitHub Pages)

Le générateur peut être hébergé gratuitement avec GitHub Pages :

1. Déposer le fichier dans le dépôt (le renommer `index.html` pour qu'il s'ouvre directement).
2. Dans **Settings → Pages**, choisir la branche `main` et le dossier `/ (root)`.
3. L'outil est alors accessible sur : `https://<utilisateur>.github.io/<depot>/`

## 📁 Contenu du dépôt

```
├── taraudage-gcode.html   # L'application complète (HTML + CSS + JS, autonome)
└── README.md
```

## 🤝 Contribuer

Les retours, rapports de bugs et suggestions sont les bienvenus via les **Issues** GitHub.

## 📄 Licence

À définir.

---

*Fait avec 🐾 par Chtipanda.*
