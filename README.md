# TP4 : Buffet à volonté

# Prérequis

* [GNS3](../../memo/setup_gns3.md)
* [iOS Cisco routeur](https://drive.google.com/drive/folders/1hnOwFTcEYeznsBwjFCzKbDripnCLOJSQ)
* [IOU L2 Cisco](https://www.gns3.com/marketplace/appliance/iou-l2)
* CentOS7 prêt à être cloné

## Topographie

### Tableau des réseaux

| Name     | Address        | VLAN |
|----------|----------------|------|
| `admins` | `10.5.10.0/24` | 10   |
| `guests` | `10.5.20.0/24` | 20   |
| `infra`  | `10.5.30.0/24` | 30   |

### Tableau d'adressage

| Machine  | `admins`      | `guests`      | `infra`       |
|----------|---------------|---------------|---------------|
| `r1`     | `10.5.10.254` | `10.5.20.254` | `10.5.30.254` |
| `admin1` | `10.5.10.11`  | x             | x             |
| `admin2` | `10.5.10.12`  | x             | x             |
| `admin3` | `10.5.10.13`  | x             | x             |
| `guest1` | x             | `10.5.20.11`  | x             |
| `guest2` | x             | `10.5.20.12`  | x             |
| `guest3` | x             | `10.5.20.13`  | x             |
| `dhcp`   | x             | `10.5.20.253` | x             |
| `dns`    | x             | x             | `10.5.30.11`  |
| `web`    | x             | x             | `10.5.30.12`  |


### A faire :

**1.** Setup la topographie dans GNS  
**2.** Mettre en place les VLANs  
**3.** Définir les IPs statiques des admins, guests et routeurs  
**4.** Mettre en place le serveur DHCP  
**5.** Mettre en place le serveur DNS  
**6.** Mettre en place le serveur Web  

#### DHCP

* serveur CentOS7
* installer le paquet `dhcp`
* configurer `/etc/dhcp/dhcpd.conf`
  * [exemple de conf](./dhcp/dhcpd.conf)
* démarrer le service `dhcpd`

Tester avec un client la récupération d'un IP et l'addresse de la passerelle, (et adresse de serveur DNS).

#### DNS

* Installer `bind` & `bind-utils`
* Configuration du DNS :
  * Modifier : [`/etc/named.conf`](./dns/etc/named.conf)
  * Modifier : [`/var/named/tp4.b2.db`](./dns/var/named/tp4.b2.db)
  * Modifier : [`/var/named/20.5.10.db`](./dns/var/named/20.5.10.db)
* Ouvrir les ports du firewall
* Lancer!


#### Serveur web

* Installer `epel-release` & `nginx`
* Lancer `nginx`

## Sujet 5 : Anonymat en ligne

**Comment garantir ou renforcer son anonymat en ligne ?**

### Le proxy

On parle souvent au sein d'un réseau informatique de proxy mais au final qu'en est t'il ? A quoi ça sert ? Tout d'abord il ne faut pas confondre cela avec un firewall (pare-feu) bien que le couplage des deux matériels en un soit très courant ! Un serveur proxy est une machine qui sert d’intermédiaire entre les machines d'un réseau et un autre réseau. Le proxy participe plus ou moins à la sécurité du réseau. Ils permettent de sécuriser et d'améliorer l'accès à certaines pages Web en les stockant en cache (ou copie).  
Ainsi, lorsqu’un navigateur envoie une requête sur la demande d'une page Web qui a été précédemment stockée, la réponse et le temps d'affichage en sont améliorés. L'utilisateur accède plus rapidement au site et ne sature pas le proxy pour sortir. Les serveurs proxy renforcent également la sécurité en filtrant certains contenus Web et les logiciels malveillants du moins pour le proxy de confiance.

### Le VPN

Le VPN, pour Virtual Private Network, est un outil qui vous permet de naviguer de manière totalement anonyme... enfin presque malheureusement. Tout d'abords pour mieux comprendre le fonctionnement d'un VPN, nous pouvons nous poser la question suivante : Dans quels cas doit-on utiliser un VPN ? En premier lieu, l’utilisation d’un VPN peut servir à protéger son réseau d’une menace extérieure. Prenant un exemple : Vous naviguer sur un site quelconque que l'on va appeler www.jetehack.com et ce dernier récupère votre adresse IP et la stock quelque part. Seul problème vous n'avez utiliser de VPN pour accèder à ce site. A partir de là, la personne qui à récupérer votre adresse IP, peut très bien décider d'utiliser une attaque "DDoS" sur vous.  
Dans un deuxième temps, le VPN peut aussi se révéler utile quand vous vous connecter depuis un endroit qui n’est pas sûr comme la Wifi Publique du McDonald où vous allez manger tous les mercredis. Mais si je devais garder en tête un seul atout du VPN, je dirais qu'il peut servir à regarder des programmes TV, à accéder à des sites ou des services qui sont prohibés dans certaines régions du globe.  
Concrètement comment ça fonctionne ? Un VPN crée une liaison directe entre deux machines, tout en isolant le trafic généré par l’utilisateur et l’ordinateur distant. Cela vous permet de contourner les analyses indiscrètes de votre trafic, tout en évitant les potentielles attaques des hackers, puisqu’il n’y a aucune interaction entre votre connexion et le trafic extérieur. Toute fois méfiez vous des VPN gratuit qui pourraient vendre vos informations !

### Client Tor

TOR est l’acronyme de   ```"The Onion Router"```. A l’origine, ce logiciel fut développé pour être utilisé par l’armée américaine et plus particulièrement la Navy. Les militaires s’en servaient pour masquer leurs adresses IP, afin d’éviter tout risque de vol des données sensibles collectées lors de missions. Cependant, lorsque l’armée a commencé à utiliser son propre système VPN, TOR a été relaxé sous forme de logiciel gratuit open-source. TOR est un navigateur web permettant de naviguer de façon plus ou moins anonyme sur le web, grâce à un réseau constitué par les utilisateurs du monde entier.


![Tor](https://www.hotspotshield.com/imgs/resources/tor-vs-vpn/how-tor-works.png)

Contrairement à ce que beaucoup de monde pourrait penser **il n’est pas illégal d’utiliser TOR** mais l'utilisation qu'on peut en faire oui. Toute fois attention, il est possible que certaines personnes s'amusent à créé des points d'accès Tor afin de récupérer vos données sensibles !

### Hidden Service Tor

Maintenant que nous avons vu comment marche Tor et ce que c'est, il me sera facile de vous expliquer le principe d'un ```"Hidden Service Tor"```. Comme nous l'avons vu le réseau Tor est un réseau créé par plusieurs utilisateurs qui peuvent héberger eux même des sites sur le réseau Tor mais qui ne seront ni référencer ni accèssible par un navigateur classique comme Chrome ou Firefox, c'est ce qu'on appel ```"Hidden Service Tor"```.

### DoH/DoT

Le DoH, pour DNS-over-HTTPS, est une fonction qui n'est pas activée par défaut pour les utilisateurs de Firefox. Ils devront modifier plusieurs paramètres avant de pouvoir mettre le DoH en marche.Le protocole DNS-over-HTTPS fonctionne en prenant le nom de domaine tapé dans par l'utilisateur dans son navigateur. Cela envoie une requête à un serveur DNS pour connaître l'adresse IP du serveur web qui héberge ce site spécifique.
C'est ainsi que le DNS normal fonctionne aussi. Cependant, DoH prend la requête DNS et l'envoie à un serveur DNS compatible DoH (résolveur) via une connexion HTTPS chiffrée sur le port 443, plutôt qu'en texte clair sur le port 53.  
De cette façon, le DoH cache les requêtes DNS dans le trafic HTTPS régulier, de sorte que les observateurs tiers (dont les FAI) ne pourront pas suivre le trafic et dire quelles requêtes DNS les utilisateurs ont exécuté et déduire quels sites Web ils sont sur le point d'accéder.

### Conclusion

Comment garantir ou renforcer son anonymat en ligne ?  

Réponse : Il n'y a pas vraiment de meilleure façon de rendre son anonyme sur Internet toute fois si vous souhaitez quand même être le plus anonyme possible je vous conseil de faire une combinaison d'un VPN avec le réseau Tor. Si vous faîtes ceci, assurez-vous que de lancer votre VPN avant de vous connecter au réseau Tor et n'oubliez pas, ne jamais prendre un VPN gratuit.
