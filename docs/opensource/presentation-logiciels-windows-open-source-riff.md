# Présentation des logiciels Windows open source de Riff

Riff utilise depuis ses débuts plusieurs logiciels conçus sur mesure, en interne, pour les besoins de la webradio. Créés sous Multimedia Fusion / Clickteam Fusion, ils ont été conçus spécifiquement par et pour les besoins de Riff, mais peuvent s'adapter à d'autres projets similaires moyennant des modifications plus ou moins importantes.

Cet article vise à présenter ces logiciels à l'attention des autres créateurs de contenu qui souhaiteraient les adapter à leurs besoins et les réutiliser dans leurs propres flux de travail.

Ces logiciels sont aujourd'hui au nombre de trois : deux à usage interne et un à usage public.

## Logiciels à usage interne

On entend par logiciel interne un logiciel prévu pour être utilisé par l'équipe de la webradio ou du projet afin de faciliter le travail d'animation live et qui n'a pas vocation à être distribué à vos auditeurs ou spectateurs. Ceux-ci sont au nombre de deux.

### Metas Broadcaster

Le Metas Broadcaster permet d'envoyer le programme en cours de diffusion via les métadonnées ICY prises en charge par Shoutcast et Icecast, depuis n'importe quel PC Windows. Plus spécifiquement, il s'agit des métadonnées StreamTitle et StreamURL.

#### Utilisation dans le workflow

En règle générale, le Metas Broadcaster est utilisé par un animateur qui produit une émission en direct depuis son home studio. Il permet de pousser le nom de l'émission vers un serveur Shoutcast, manuellement ou semi automatiquement au lancement, ainsi que le titre en cours de diffusion (facultatif) chaque fois que celui-ci change.

Il est typiquement utilisé pour envoyer les métadonnées vers le même point d'entrée input.harbor de [liquidsoap](https://liquidsoap.info) que celui utilisé pour le décrochage live. Il est possible de le configurer pour envoyer des métadonnées directement au serveur final de diffusion, mais cela n'est pas typiquement utilisé en plein live (Riff ayant plusieurs serveurs de diffusion indépendants). Les métadonnées doivent donc ensuite être traitées par Liquidsoap, qui peut potentiellement les modifier ou ajouter du contenu.

#### Fonctionnalités spécifiques

- **Deux slots d'enregistrement de noms d'émission :** Vous pouvez enregistrer deux noms d'émissions et les rappeler d'un seul clic entre deux sessions.
- **Compte à rebours de fin de titre :** en récupérant les informations via l'API de Riff, MB est capable de donner un compte à rebours annonçant la fin du titre de la playlist 24/24 en cours de diffusion, afin que l'animateur puisse prendre l'antenne sans couper de musique en plein milieu.
- **Envoi automatique des métadonnées et lancement d'une vague de décrochage :** Optionnellement, à la fin du compte à rebours, Metas Broadcaster peut envoyer automatiquement le titre de l'émission vers le serveur sélectionné, et jouer automatiquement un son (typiquement une vague de décrochage / un jingle live), ce qui permet à l'animateur de temporiser entre le clic de décrochage et le lancement d'une première musique.
- **Compatibilité Winamp :** Metas Broadcaster est prévu pour être utilisé avec Winamp : il récupère automatiquement l'artiste et le titre en cours de lecture via le titre de la fenêtre Winamp, et peut l'ajouter dans les métadonnées envoyées, aux côtés de ou en lieu et place du titre de l'émission, selon un pattern défini dans les options. (Par défaut : `Nom de l'émission (Artiste - Titre)`).
- **Filtrage automatique :** Par défaut, la présence de certains mots-clés dans le titre et/ou l'artiste d'un fichier lu avec Winamp entraîne une absence d'envoi de métadonnées (il est aussi possible de paramétrer pour que dans un tel cas, il envoie le titre de l'émission seul). Cela évite que les metas soient polluées par des noms de fichiers tels que « JINGLE5_EMISSION ».
- **Sauvegarde de la configuration :** La liste des serveurs de connexion ainsi que l'ensemble des paramètres de configuration sont sauvegardés dans un fichier .INI

#### Prérequis et limitations

Côté client, Metas Broadcaster est en principe compatible avec n'importe quel PC sous Windows (de XP à 11 inclus). Il occupe environ 5 Mo de RAM, ce qui est négligeable dans la plupart des scénarios. Il est compatible avec les versions récentes de Wine sous Linux.

Côté serveur, Metas Broadcaster attend un point d'entrée Shoutcast V1. Les données sont envoyées sous forme de métadonnées ICY. Si vous utilisez Liquidsoap avec un input.harbor (situation recommandée), assurez-vous de l'avoir configurée en mode Shoutcast et non Icecast.

Attention, la prise en charge des caractères spéciaux est limitée. Il ne prend pas en charge les caractères Unicode. En revanche, il est compatible avec la plupart des caractères de l'alphabet latin, tels que les caractères accentués français, les lettres de l'alphabet scandinave (ø, þ, etc.), le ß allemand, etc.

#### Conseils supplémentaires

- Avant d'envoyer des métadonnées, assurez-vous d'avoir sélectionné le bon serveur dans la liste déroulante.
- Si vous envoyez automatiquement les métadonnées et le son à la fin du chrono, connectez-vous au serveur une demi-seconde avant ladite fin, afin de garantir que vous êtes en live lorsque le son part et que celui-ci est bien diffusé en entier.
- Si vous souhaitez redistribuer le logiciel après l'avoir modifié, faites attention à ne pas distribuer votre fichier .ini interne tel quel, puisqu'il contient notamment le mot de passe de connexion « animateur » à votre serveur Shoutcast.

#### Remarques pour développeurs

- Les serveurs, ports et mots de passe pour l'envoi des métadonnées sont consignés dans un fichier INI lisible et modifiable par un humain.
- En revanche, le point de terminaison racine pour l'accès à l'API pour le compte à rebours de durée de titre de playlist est actuellement codé en dur. Il devra donc être modifié sous Fusion 2.5 pour s'adapter à une autre API, même si votre API est implémentée à l'identique de celle de Riff.
- La liste des mots-clés « filtres » de la protection anti-jingle se trouve à l'événement 27 du MFA, sous forme d'expression regex à modifier selon vos besoins.

### Riff Sampler

Le Sampler permet à une personne dénuée de périphérique dédié (type streamdeck ou équivalent) d'envoyer divers sons en appuyant sur une touche du clavier (touches fonction F6 à F12 et touches 0 à 9 du pavé numérique) ou à la souris. Cela permet le déclenchement de bruitage sans achat de matériel (ou logiciel) supplémentaire.

#### Utilisation dans le workflow

Le Sampler s'utilise exclusivement dans le cadre de décrochages lives. Le son est envoyé via la carte son de l'animateur et mixé au reste des sources sonores de la machine. Une fois mixé, le son doit être envoyé à un serveur par le biais de votre choix (le type de serveur est technologiquement neutre).

Il est possible de recourir soit à une fonctionnalité de type « Stereo Mixer » pour l'envoyer au serveur parmi d'autres sources sonores, soit de l'envoyer à une table de mixage, dont le son mixé par la table est récupéré et diffusé, soit encore de capturer le son du logiciel indépendamment (par exemple sous forme de source dans OBS Studio).

#### Fonctionnalités spécifiques

- **Fonctionne même minimisé** : Le grand intérêt du Riff Sampler est que les sons peuvent être déclenchés même si la fenêtre n'a pas le focus, et même si elle est minimisée.
- **Compatibilité multiformats** : Prend en charge les fichiers ogg, mp3 et wav.
- **Stop avec fadeout** : Il est possible d'arrêter prématurément un son, avec ou sans fadeout.
- **Multiples pages de samples** : afin de dépasser la limite de 17 samples permis par les touches 0 à 9 et F6 à F12, il est possible de naviguer entre différentes « pages » de samples (jusqu'à 999). Chaque page possède sa propre liste de 17 samples. Il est possible de naviguer entre les pages au clavier et à la souris.
- **Sélecteur de volume inclus** : il est possible de régler le volume du logiciel directement depuis l'interface.
- **Possibilité de mapper la rangée de touches numériques au-dessus des lettres** : pour les claviers dénués de pavé numérique (courant sur les ordinateurs portables), il est possible de mapper les touches 0 à 9 de la section alphanumérique du clavier.

#### Prérequis et limitations

Le Riff Sampler ne nécessite rien d'autre qu'un PC sous n'importe quelle version de Windows XP ou supérieure (Vista recommandé), ou avec une version récente de Wine sous Linux. Le PC n'a pas besoin d'être connecté à Internet. Il consomme environ 6 Mo de RAM, ce qui est là encore négligeable. Il a besoin d'une sortie son fonctionnelle.

Attention, le logiciel n'enregistre pas sa configuration. Les réglages tels que le volume ou le mappage de la section alphanumérique doivent être refaits à chaque lancement.

Par ailleurs, le mode « AUTO » qui permet d'utiliser un mélange de fichiers mp3, wav et ogg est parfois capricieux. Dans la mesure du possible, il est recommandé de choisir un seul format de fichier et de sélectionner ledit format en bas à gauche de l'interface.

#### Conseils supplémentaires

- Les samples doivent être nommés `soundF6` à `soundF12` et `sound0` à `sound9` (.wav, .ogg ou .mp3), et placés dans des sous-répertoires de l'application nommés `1` à `999`, correspondant à autant de pages de samples.
- Attention, le mappage des touches de la section alphanumérique peut facilement entraîner des erreurs de manipulation (lancements de sons inopinés par l'animateur).
- Lorsque vous éditez le fichier reminder2.txt d'une page de samples, qui contient un rappel des samples bindés aux touches 0 à 9, vérifiez que l'affichage se fait correctement dans l'interface, celle-ci ne suivant pas tout à fait la même tabulation que notepad.

#### Remarques pour développeurs

Le logiciel est globalement prêt à être utilisé tel quel et ne nécessite pas d'adaptation particulière pour être utilisé sur une autre radio ou un autre stream. Cependant, plusieurs axes d'amélioration existent. Notamment :

- L'enregistrement de la configuration dans un fichier ou dans la base de registres Windows
- La correction du problème rendant le mode AUTO capricieux.
- Idéalement, l'interface pourrait être modernisée pour rendre les noms des samples plus lisibles.

## Logiciel accessible au public

On entend par logiciel accessible au public un logiciel qui est prévu pour être publié et distribué au plus grand nombre, comme moyen pratique pour le public de profiter de votre travail (en l'occurrence, votre flux).

Attention, un tel logiciel ayant par nature vocation à être redistribué, vous devrez en respecter la licence (MPL dans le cas ci-dessous), ce qui implique notamment d'en redistribuer les sources sous la même licence ou sous une licence compatible.

### Riff Player

Le Riff Player est, fondamentalement, un client Shoutcast/Icecast qui se connecte directement à l'un des flux de la radio. Il est séparé en deux applications, l'une principale et l'autre servant de menu de configuration. Il récupère et utilise diverses données de l'API de Riff pour apporter des informations supplémentaires et offre quelques fonctionnalités potentiellement utiles tout en restant très léger et rapide.

Il repose sur la bibliothèque BASS (linkée dynamiquement via sa DLL) d'[un4seen Developments](https://www.un4seen.com/bass.html). La licence de BASS est propriétaire, mais son utilisation est gratuite pour les projets non commerciaux.

#### Fonctionnalités spécifiques

- **Utilisation de l'API de Riff** : Le Riff Player exploite l'API pour afficher le type de programme diffusé (playlist, spécial, live...), la durée du morceau en cours et la durée écoulée (avec une barre de progression), le message du moment de la radio, etc.
- **Vérification des mises à jour** : Vérifie automatiquement si une mise à jour est disponible et propose de télécharger la dernière version sur le site de la radio, le cas échéant
- **Accès web** : Propose un accès rapide au chat de la radio, ainsi qu'au site web et aux réseaux sociaux.
- **Fonctionnalité réveil matin** : Il est possible de configurer le Riff Player pour que la lecture se déclenche automatiquement à une heure donnée (à condition que le player soit ouvert).
- **Fonctionnalité lecture auto sur certains types de programmes** : Il est possible de configurer le lecteur pour qu'il lance automatiquement la lecture lorsqu'un live démarre, et/ou lorsqu'un programme spécial en différé démarre.
- **Compatibilité multiflux** : Le Riff Player permet de choisir parmi plusieurs flux dans les options (avec des bitrates et des formats de fichier différents, pour s'adapter à différentes qualités de connexion).
- **Possibilité de lancer le Riff Player au démarrage de Windows** : cette fonctionnalité est facultative.

#### Prérequis et limitations

Le Riff Player est compatible avec Windows 7 ou supérieur, ou avec une version récente de Wine sous Linux. Il nécessite bien entendu une sortie audio et une connexion au débit et à la stabilité suffisante pour le flux récupéré.

Le Riff Player gère les déconnexions de façon relativement sommaire : il réinitialise totalement la communication avec la DLL s'il détecte une erreur, y compris une erreur réseau. Il refait plusieurs tentatives et, en cas d'échecs successifs, envoie un message d'erreur sous forme de boîte de dialogue, laissant à l'utilisateur la possibilité de refaire une tentative ou non. Cependant, au vu de sa légèreté, l'opération reste relativement transparente.

Attention, en l'état, le Riff Player a besoin du bon fonctionnement du flux **et** de l'API pour bien fonctionner. Si l'un des deux cesse de répondre, le Riff Player se mettra en défaut sans chercher à sauver la partie fonctionnelle.

#### Conseils supplémentaires

Le Riff Player télécharge et décode le flux en permanence même lorsque la lecture est sur « Stop ». Cela permet notamment de continuer à recevoir et décoder les métadonnées pour afficher le titre en cours de diffusion. Le téléchargement et le décodage d'un flux MP3 ou AAC étant aujourd'hui très peu gourmand en ressources, cela ne pose pas de problème la plupart du temps. Mais cela peut commencer à en devenir un sur les connexions très limitées en débit ou en forfait de données. Un flux à 320kbps consomme environ 140 Mo par heure, soit 3.36Go par jour.

#### Remarques pour développeurs

- À l'heure actuelle, les sockets pour la communication avec l'API ont été implémentés manuellement et se limitent donc au HTTP pur (pas de TLS/SSL). Un projet de conversion vers l'utilisation de GET Object, qui permettrait la prise en charge du HTTPS, est en travaux à l'heure où nous écrivons ces lignes.
- Le Riff Player comprend de nombreux easter eggs, plus ou moins cachés et plus ou moins sympathiques. Il est possible de les supprimer en tout ou partie pour gagner encore un peu d'optimisation.
- Le projet ayant démarré il y a très longtemps à l'époque de Multimedia Fusion et par un créateur peu expérimenté, il est conçu de façon peu élégante (« spaghetti code ») est il est probablement peu recommandé de le modifier substantiellement en dehors de son skin graphique et des URL pour les flux, liens web et points de terminaison de l'API.
- Notez qu'il peut encore contenir des variables devenues obsolètes du fait de l'évolution des fonctionnalités du logiciel. Si vous trouvez une variable qui ne semble utilisée nulle part, le problème ne vient donc pas nécessairement d'un manque de compréhension du logiciel de votre part.
