# Référence de l'API de Riff

## Présentation

Cette documentation énumère les différents points de terminaison de l'API publique de **Riff**, à l'attention des développeurs et hobbyistes qui souhaitent récupérer certaines données de la webradio et du fichier en cours de diffusion.

Cette API minimaliste permet un accès simple et en (quasi-)temps réel à certaines informations concernant la webradio et les contenus en cours de diffusion ou précédemment diffusés.

### Envoi des requêtes

L'URL de base de l'API est `https://api.riff-radio.org/0/`. L'API est également accessible en HTTP simple en remplaçant `https://` par `http://`.

L'API ne nécessite aucune clé ni authentification. Vous pouvez simplement récupérer les fichiers sous forme de texte directement en appelant les points de terminaison via une requête de type `GET`. Il s'agit d'une API read-only, permettant de récupérer des données, mais pas d'interagir avec la webradio.

À l'heure actuelle, l'API n'applique pas de politique de *rate limiting*. Il est néanmoins demandé de ne pas dépasser 1 requête par seconde, par client et par point de terminaison.

Le serveur autorise explicitement les requêtes cross-origin (CORS) depuis n'importe quelle origine.

### Format des réponses

L'API respecte les standards HTTP(S). Elle envoie donc un code HTTP `200 OK` en cas de succès, et les codes d'erreur HTTP standards en cas d'échec. En cas de multiples requêtes devant renvoyer un résultat identique, si la requête de demande contient un en-tête `If-None-Match` ou `If-Modified-Since`, elle peut renvoyer un code `304 Not Modified`.

La plupart des points de terminaison de l'API renvoient des données au format texte brut. Cependant, un point de terminaison renvoie les données au format JSON, et un autre renvoie une page HTML complète.

Sauf mention contraire, les données sont encodées au standard d'encodage UTF-8.

## Liste des points de terminaison

### Texte brut

#### STATUS.txt

Renvoie le type de programme en cours de diffusion. Ce type de contenu peut être manuellement modifié pour une valeur personnalisée par l'équipe de la webradio, mais sera le plus souvent de type :

- `PLAYLIST` : La programmation 24h/24 de Riff, diffusée sous forme de playlist automatisée.
- `LIVE` : Une émission en direct est en cours de diffusion.
- `Podcast` ou `Différé`  Une émission animée mais préenregistrée et diffusée automatiquement en différé à un horaire prédéterminé.
- `Rediff` ou `Rediffusion`  Rediffusion d'une ancienne émission précédemment diffusée en direct.
- `Spécial` : Une programmation spéciale, le plus souvent sous forme de playlist générée spécifiquement pour l'occasion mais diffusée par le robot et dénuée d'animateur.
- `PARTY` : Une émission en direct exceptionnelle de type « Grand événement », telle qu'une émission anniversaire, un live 24h, ou autre surprise.

⚠️ Ces valeurs devraient être considérées comme non sensibles à la casse, et votre implémentation se doit donc d'envisager tous les scénarios (p. ex. : `LIVE` ou `Live` sont deux valeurs acceptées pour une émission en direct).

#### auditeurs

Renvoie le nombre d'auditeurs (estimé) actuellement à l'écoute de Riff, sous forme de chiffre. Par exemple `24`. Mis à jour une fois par minute.

⚠️ Pas d'extension .txt sur ce point de terminaison.

#### last.txt

Renvoie la liste des dix derniers contenus diffusés à l'antenne de Riff. On entend par « contenu » des fichiers diffusés en cas de playlist, ou tout changement de métadonnées titre/artiste causé par un décrochage live ou par un changement des métadonnées d'un animateur en cours de live.

Pour le type de format, se référer au point de terminaison [meta_last.txt](#meta_lasttxt).

#### meta_last.txt

Le contenu en cours de diffusion, tel que rapporté par l'outil d'automation de la radio.

Le format dépend du type de programme en cours de diffusion :

- **Pendant les playlists (y compris spéciales) :** Le format est de type `Artiste - Titre`.
- **Pendant les émissions en direct (y compris PARTY) :** Le format est à la discrétion de l'animateur mais généralement de type `Nom de l'émission (Artiste - Titre)` ou `Nom de l'émission`.
- **Pendant les podcasts, différés ou rediffusions :** Le format est généralement de type `Nom et numéro ou date de l'émission`.

⚠️ Dans de rares cas, la valeur rapportée par ce point de terminaison peut varier des métadonnées transmises par le serveur de diffusion (Shoutcast). Ce point de terminaison rapporte en effet le contenu en cours de traitement au niveau de **l'outil d'automation** de la radio. Il ne tient par exemple pas compte d'une modification de métadonnées effectuée manuellement au niveau du serveur de diffusion lui-même.

#### msg.txt

Renvoie un « message du jour » sous forme de chaîne d'une longueur inférieure ou égale à 100 caractères. Ce message est mis à jour manuellement, sans régularité et peut contenir une actu de la radio, une petite blague ou autre.

⚠️ Le message peut être remplacé par : `/!\Error: Outdated version!!/!\`, auquel cas cela signifie que cette version de l'API n'est plus prise en charge ou que le serveur a changé d'adresse et que les URL actuellement utilisées seront bientôt désactivées. Il est recommandé d'interpréter la présence de cette chaîne comme une erreur fatale.

#### duration.txt

Renvoie la durée en secondes du fichier ou contenu en cours de diffusion (lorsque celle ci est connue), suivi de trois sauts de ligne (soit deux lignes vides), suivis de l'heure exacte de démarrage de ce fichier (heure du serveur) sous forme de timestamp Unix. Ce timestamp peut être suivi ou non d'un retour à la ligne. Il prend typiquement la forme suivante :

```Text
128\n
\n
\n
1234567890\n
```

⚠️ Le dernier \n, après le timestamp Unix, peut être présent ou absent, il est donc recommandé de prendre en charge les deux scénarios.

⚠️ Lorsque la durée du fichier est inconnue (soit qu'il s'agisse d'une émission en direct, soit que le serveur n'ait pas été capable de calculer la durée du fichier), la première valeur est remplacée par `-1`. Le timestamp reste néanmoins généré. Il reste donc possible de savoir à quelle heure et depuis combien de temps le programme a commencé.

#### timestamp.php

Génère et renvoie l'heure exacte du serveur, au moment de la requête, sous forme de timestamp Unix. Avec [duration.txt](#durationtxt), permet de calculer le temps exact écoulé depuis le début du contenu, quel que soit le décalage entre l'horloge du serveur et celle du client.

### JSON

#### public.json.php

Génère et renvoie un document json reprenant une partie des informations énumérées précédemment, au même format et avec les mêmes particularités. Il est constitué de la manière suivante :

|Clé|Type|Valeur|
|---|----|------|
|song|object|Contient les clés `currentTime`, `timeStarted`, `duration` et `currentMeta`.|
|currentTime|integer|Timestamp Unix contenant l'heure exacte du serveur au moment de la requête.|
|timeStarted|string|Timestamp Unix contenant l'heure du serveur au moment où le contenu a démarré.|
|duration|string|Durée du contenu en secondes (retourne `-1` si la durée est inconnue)|
|currentMeta|string|Artiste - Titre|
|radio|object|Contient les clés `listeners`, `status` et `motd`.|
|listeners|string|Nombre d'auditeurs|
|status|string|Type de programme en cours de diffusion (voir [STATUS.txt](#statustxt))
|motd|string|Message du moment, longueur maximale de 100 caractères|

 La réponse ressemble à ceci :

```JSON
{
  "song": {
    "currentTime": 1234567890,
    "timeStarted": "2345678901",
    "duration": "123",
    "currentMeta": "Artiste - Titre"
  },
  "radio": {
    "listeners": "99",
    "status": "PLAYLIST",
    "motd": "Rock and much more!"
  }
}
```

⚠️ Notez que, pour des raisons historiques, le timestamp de currentTime est servi sous forme d'`integer` alors que le timestamp de timeStarted (et avec lui toutes les autres valeurs numériques) est servi sous forme de `string`.

### Page HTML

#### auto.php

Renvoie une page HTML qui se réactualise automatiquement toutes les 3 secondes et contient l'Artiste et le Titre en cours de diffusion au sein d'une balise `<div id="shoutcastmetacontent">`. Vous pouvez par exemple intégrer cette page dans une source navigateur sous OBS Studio et lui appliquer un style via CSS.

## Exemple d'utilisation

```bash
curl https://api.riff-radio.org/0/meta_last.txt
```

Récupère l'artiste et le titre du morceau en cours de diffusion, une seule fois (aucun paramètre requis).
