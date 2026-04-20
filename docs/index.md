# Documentation de Riff

[Riff](https://www.riff-radio.org) est une webradio associative axée rock international et genres voisins. Elle diffuse de la musique 24h/24 via un ensemble de playlists autogénérées et des décrochages réguliers pour des émissions en direct depuis les home studio de ses différents animateurs.

Elle fonctionne à l'aide de divers scripts et logiciels faits maison, aussi bien en backend qu'en frontend, pour la plupart open source. Elle dispose d'une API permettant de l'intégrer sur des sites web, logiciels et autres espaces.

La diffusion se fait via Shoutcast et Icecast.

Les scripts et sources de logiciels de Riff sont disponibles sur [la page github de la radio.](https://github.com/riffradio)

---

## API de Riff

### Cas d'usage

L’API de Riff permet notamment de :  
  
- afficher le titre en cours de diffusion (« Now Playing ») sur un site web ;
- afficher le statut de la radio (live, playlist, émission, etc.) dans un client ;  
- suivre le nombre d’auditeurs en temps réel ;
- intégrer des informations de la radio dans une interface web ou un outil de streaming (ex. OBS) ;

Elle s'adresse à ceux qui souhaitent intégrer Riff à leurs propres projets.
  
### Documentation de l'API  
  
Cette documentation couvre :  
  
- la **référence de l’API**, détaillant les différents points de terminaison et leurs formats de réponse ;
- des **guides pratiques** pour intégrer les données de Riff dans différents contextes (site web, outils de streaming, etc.) ;
  
L’API est volontairement simple : elle est accessible sans authentification et repose sur des requêtes HTTP en lecture seule.

---

## Scripts et logiciels open source

### Cas d'usage

Cette section est plutôt consacrée aux personnes souhaitant réutiliser les logiciels initialement créés par et pour Riff, et aujourd'hui distribués en open source, pour leur propre webradio ou projet de flux (audio et/ou vidéo).

Elle permet de s'approprier les outils en en comprenant le fonctionnement, les prérequis et les limitations et en voyant, le cas échéant, comment les adapter aux besoins spécifiques d'un autre projet.

### Documentation des scripts et logiciels

Pour le moment, la documentation couvre :

- Une **présentation** des logiciels Windows open source créés et distribués par Riff (fonctionnalités, limitations, informations pour développeurs envisageant de forker le projet, etc.)
- À terme, vous trouverez également les guides d'utilisation des logiciels proposés sur le Github, à l'attention des développeurs et utilisateurs souhaitant adapter ces outils pour leurs propres besoins (autres webradios).
