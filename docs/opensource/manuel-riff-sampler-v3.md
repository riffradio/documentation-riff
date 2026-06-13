---
title: "Manuel de référence du Riff Sampler v3.2"
description: Cet article présente une description exhaustive de l'interface du Riff Sampler v3 et de son implémentation.
category: reference
date: 2026-06-11
version: 3.2.0
---

# Manuel de référence du Riff Sampler v3.2

## Présentation

Le **Riff Sampler v3** est un logiciel autonome pour toutes les configurations et toutes les versions de Windows (Vista et supérieur recommandé), qui permet de déclencher des sons soit en cliquant sur un bouton, soit directement via une touche du clavier.
Il permet de déclencher les sons même si la fenêtre se trouve en arrière-plan ou minimisée, d'une pression sur les touches F6 à F12 et et 0 à 9 du clavier, soit 17 sons personnalisables.

Afin de dépasser la limite des 17 sons, ceux-ci peuvent être organisés en « pages », ou banques de sons. Il est possible de configurer jusqu'à 999 pages. Chacune peut disposer de ses propres sons, et il est possible de passer d'une page à l'autre au clavier ou à la souris.

Il est conçu pour un usage en réalisation temps réel, en particulier dans le cadre d'une webradio ou pour le streaming vidéo, notamment en l'absence de Streamdeck.

Les formats audio pris en charge sont le **MP3**, l'**OGG Vorbis** et le **WAV**.

Le logiciel supporte la lecture multi-canal. Vous pouvez jouer les 17 sons d'une même page en même temps. Il est également possible de jouer deux sons affectés à des touches différentes sur plusieurs pages différentes. En revanche, il n'est pas possible de jouer simultanément deux sons affectés à une même touche et provenant de deux pages de son différentes.

### Licence

Le Riff Sampler est un logiciel open source placé sous licence [Mozilla Public License 2.0](https://www.mozilla.org/en-US/MPL/2.0/).

Le code source est accessible sur [Github](https://github.com/riffradio/riffsampler).

## Installation / Configuration

Le sampler est constitué :

- D'un exécutable
- D'une image servant de logo
- D'un ou plusieurs répertoires numérotés 1 à 999

Il est fourni sous forme d'archive zip contenant l'exécutable, le logo, et un premier répertoire "1" déjà créé. En revanche, il n'est accompagné d'aucun fichier audio.

Chaque répertoire est constitué des fichiers suivants :

- **name.txt** : Le nom donné à cette banque de sons
- **reminder1.txt** : Les noms des sons déclenchés via les touches F6 à F12
- **reminder2.txt** : Les noms des sons déclenchés via les touches 0 à 9

Toutes ces informations s'afficheront dans le sampler lorsque vous chargerez la banque correspondante.

Il contient également les fichiers audio destinés à être lus.

Pour créer plusieurs banques de son, vous devrez créer d'autres répertoires, nommés 2 à 999, et y inclure les fichiers mentionnés ci-dessus.

> **Remarque** : Les noms indiqués dans les fichiers `reminder` ne sont pas des références à des noms de fichiers, mais des posts-it à usage humain. Les noms des fichiers sont fixes ; ces fichiers texte sont le moyen pour vous de savoir quel son produit chaque touche depuis l'interface du logiciel.

### Chargement des sons

Chaque banque (chaque répertoire) peut contenir jusqu'à 17 fichiers sonores. Ceux-ci doivent être nommés de la façon suivante :

- `sound0` à `sound9` pour les sons à déclencher via les touches numériques
- `soundF6` à `soundF12` pour les sons à déclencher via les touches fonction.

L'extension des fichiers peut être `.wav`, `.ogg` ou `.mp3`. Toutefois, si vous placez plusieurs fichiers du même nom avec une extension différente, un seul fichier sera joué.

> **Remarque** : Les noms de fichiers ne sont pas sensibles à la casse.

## Utilisation du sampler

### Utilisation à la souris

De haut en bas et de gauche à droite, les contrôles sont les suivants :

- **Lock** : Permet de désactiver le changement de banque au clavier, afin d'éviter un changement accidentel.
- **Mapper aussi les touches alphanumériques** : Si décochée, seules les touches 0 à 9 du pavé numérique permettent de lancer des sons. Si cochée, la rangée haute (touches numériques) de la section alphanumérique du clavier permet également de lancer les sons 0 à 9.
- **Boîte de nombre avec sélecteur** : Permet de sélectionner la page de sons active, entre 1 et 999.
- **Boîte de texte** : Contient le nom de la page sélectionnée (contenu du fichier `reminder.txt`).
- **Boutons F6 à F12** : Permettent de lancer les sons F6 à F12 à la souris. S'allument en rouge pendant qu'un son est en cours de lecture.
- **Boutons  9 à 0** : Permettent de lancer les sons 0 à 9 à la souris. S'allument en rouge pendant qu'un son est en cours de lecture.
- **Boutons radio Fichiers / AUTO** : Permettent de sélectionner la lecture de fichiers `.wav`, `.mp3` ou `.ogg`. Le bouton radio `AUTO` permet de lire les fichiers supportés quelle que soit l'extension. Si plusieurs fichiers portent le même nom et une extension différente sur la même page, lira le .wav s'il existe, sinon le .mp3, sinon le .ogg.
- **Utiliser une police étroite** : Affiche les reminders en police **Arial Narrow** plutôt que **Arial**. Cette police plus étroite permet d'assigner des textes plus longs pour chaque touche.
- **Stop son** : Permet de couper prématurément un son en cours de lecture.
- **Sélecteur de volume** : Permet de régler le volume de lecture des sons (modifie directement le volume de l'application dans le mixer Windows).
- **Éditer texte** : Ouvre les fichiers `reminder1` et `reminder2` de la page de sons sélectionnée dans le Bloc-notes Windows (**notepad.exe**) afin de les modifier.
- **Recharger** : Permet d'actualiser les contenus des fichiers `reminder1` et `reminder2` après modification, sans avoir à redémarrer l'application.
- **Aide** : Ouvre un onglet de navigateur vers la présente page d'aide.
- **Texte inférieur droit** : Cliquez dessus pour afficher la boîte "À propos de ce logiciel" qui contient les mentions légales.

Veuillez noter que les éventuels réglages modifiés via cette interface ne sont pas sauvegardés d'une session à l'autre. Vous devrez refaire ces réglages à chaque lancement.

> L'interface ne donne aucune indication quant à l'existence effective des fichiers audio dans le répertoire approprié. Si un fichier audio est manquant, le bouton pourra être cliqué de la même manière, mais aucun son ne sortira.

### Utilisation au clavier

- **Touches 0 à 9** : Lance les sons 0 à 9. Si la case **Mapper aussi les touches alphanumériques** est décochée, seules les touches 0 à 9 du pavé numérique fonctionneront. Sinon, la rangée de touches numériques de la section alphanumérique fonctionnera également.
- **Touches F6 à F12** : Lance les sons F6 à F12
- **Touches + et ²** : Permet de couper immédiatement le son en cours de lecture.
- **Touche Maj** : Si celle-ci est enfoncée au moment où l'utilisateur coupe un son (que ce soit au clavier ou à la souris), alors le son se coupera progressivement avec un fondu de 2 secondes.
- **Touche / (pavé numérique) ou F4** : Permet de sélectionner la page de sample précédente (par exemple, passer de la page 2 à la page 1). Désactivé si la case **Lock** est cochée.
- **Touche * (pavé numérique) ou F5** : Permet de sélectionner la page de sample suivante (par exemple, passer de la page 2 à la page 3). Désactivé si la case **Lock** est cochée.

## Cas particuliers et problèmes connus

### Redimensionnement

La fenêtre peut être redimensionnée horizontalement et verticalement.

- Le redimensionnement horizontal permet de gagner de la place pour afficher des textes plus longs dans les reminders, notamment pour les touches 1 à 9 qui disposent d'un espace limité.
- Le redimensionnement vertical permet d'agrandir la police d'affichage des reminders pour une meilleure lisibilité.
- Il est possible de redimensionner les deux pour gagner sur les deux tableaux. Cela permet également d'agrandir les boutons pour les rendre plus faciles à viser à la souris.

> ⚠️ Attention, un redimensionnement mal proportionné peut provoquer une perte d'alignement entre les boutons et les textes situés en face.

### Tabulation dans reminder2.txt

Le fichier `reminder2.txt` contient les noms des sons pour les touches 0 à 9. Or, pour les touches 1 à 9, trois noms de sons sont affichés par ligne. Pour des raisons de lisibilité, il est recommandé de les aligner correctement en utilisant des tabulations.

Cependant, le Bloc-notes Windows utilise une police à largeur fixe tandis que le logiciel utilise une police à largeur variable, ce qui peut entraîner un décalage. Il peut donc arriver que les tabulations fassent apparaître un alignement correct dans le Bloc-notes mais pas dans le logiciel, et vice-versa.

Par exemple, vous pouvez observer l'affichage suivant dans le Bloc-notes :

```text
7: Son n° 77ZZ   8: Son n° 88YY   9: Son n° 99XX
4: Son n° 44AA   5: Son n° 55BB   6: Son n° 66CC
1: Son n° 11II   2: Son n° 22KK   3: Son n° 33MM
```

Et l'affichage suivant dans le logiciel :

7: Son n° 77ZZ&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8: Son n° 88YY&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9: Son n° 99XX  
4: Son n° 44AA&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5: Son n° 55BB&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6: Son n° 66CC  
1: Son n° 11II &nbsp; 2: Son n° 22KK&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3: Son n° 33MM

Dans ce cas, il faudra rajouter une tabulation supplémentaire après 11II. Les textes paraîtront alors non-alignés dans le Bloc-notes, mais s'afficheront correctement dans le logiciel.

### Fonction AUTO

La fonction AUTO permet en principe de lancer des fichiers audio indépendamment de leur extension, selon l'ordre de priorité indiqué ci-dessus. Toutefois, il peut arriver que le logiciel échoue à lire les fichiers même s'ils existent. Choisir une seule extension et sélectionner le bouton radio correspondant est donc plus fiable.
