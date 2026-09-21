# Unité Centrale de gestion de batterie et de contrôle d’alimentation BMPCU (Prototype V0)

Ce dépôt rassemble les fichiers de conception électronique d'une **BMPCU (Battery Management and Power Control Unit)** développée dans le cadre de mon stage chez OMNITECH. L'objectif de cette carte est de centraliser sur un unique PCB multicouche la supervision d'une batterie haute tension (400VDC), le contrôle d'un étage de charge solaire et l'interfaçage avec la commande moteur. 

Le système est entièrement conçu sous **Altium Designer**.

---

## 📐 Architecture du Système

Le diagramme ci-dessous illustre l'organisation des blocs de puissance, de l'alimentation logique isolée, du conditionnement des signaux et des interfaces utilisateur de la carte.

![Architecture du système BMPCU](image_Z-mTeR.png)

### Blocs matériels principaux :
*   **Contrôle de charge :** Convertisseur Buck-Boost (topologie SEPIC) dimensionné pour réguler le flux d'une source photovoltaïque.
*   **Alimentation logique isolée :** Convertisseur Flyback (400V → 12V) basé sur le contrôleur UCC28740 pour alimenter la commande basse tension à partir du pack batterie principal.
*   **Conditionnement et sécurité :** Mesures de tension et de courant isolées galvaniquement (AMC1311B et ACS37002), associées à des circuits de protection étagés (MOV, TVS, fusible DC de 25A) et un relais de déconnexion d'urgence.

---

## 🛠️ Design Review et Axes d'Amélioration Pour La Version Future du (Prototype V0)

Ce projet est une **première itération d'étude (Version 0)** orientée vers la validation théorique des schémas et le placement des composants. Le circuit n'a pas été fabriqué ni testé en laboratoire. Une analyse critique de la conception met en évidence plusieurs axes d'amélioration indispensables avant toute mise en production :

### 1. Gestion Thermique sous-dimensionnée
La dissipation thermique actuelle du prototype est insuffisante et présente un risque majeur de surchauffe :
*   **Convertisseurs LDO :** L'abaissement linéaire de la tension via des régulateurs LDO génère des pertes thermiques importantes. Pour une V1, il faudrait remplacer ces LDO par des **régulateurs à découpage (Buck DC-DC)** à haut rendement pour éviter de dissiper inutilement de l'énergie sous forme de chaleur, ou à défaut, ajouter des dissipateurs thermiques physiques dédiés sur les boîtiers LDO actuels.
*   **Convertisseur SEPIC :** Les transistors MOSFET de puissance et les diodes de redressement de l'étage de charge manquent d'éléments de refroidissement. L'intégration de **dissipateurs thermiques (radiateurs)** dédiés est indispensable pour stabiliser thermiquement le système sous forte puissance.

### 2. Erreur d'interfaçage de Commande (Vitesse de Commutation)
*   **Commande du MOSFET du SEPIC :** L'utilisation d'un relais électromécanique pour piloter la grille (Gate) du MOSFET de puissance est une erreur de conception critique. Le relais étant un composant mécanique, sa fréquence de commutation est extrêmement faible. Pour un hacheur Buck-Boost/SEPIC opérant à haute fréquence (50 kHz), le relais doit impérativement être supprimé et remplacé par un **circuit driver de MOSFET isolé** (Gate Driver) rapide et purement électronique.

Le reste de la topologie, les blocs de conditionnement analogique et la chaîne d'isolation des signaux restent valides pour l'architecture cible.

---

## 📁 Structure du Dépôt

*   `📁 altium-design/` : Fichiers sources du projet Altium (.PrjPcb, .SchDoc, .PcbDoc).
*   `📁 manufacturing/` : Fichiers d'export pour la fabrication de la carte (Gerber, nomenclature / BOM).
*   `📁 docs/` : Documentation technique et fiches techniques (datasheets) des composants majeurs.
