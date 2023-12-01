+++
title = "Homelab v0 🚧"
date = "2023-01-12"
description = "Mon infra personnelle pour stocker mes données"
+++

# Qu'est ce qu'un Homelab ?
Un homelab est un "laboratoire maison", c'est à dire une infrastructure personnel et qui nous appartient physiquement. En règle générale dans un homelab on y gère le physique ainsi que l'applicatif (le plex, nextcloud, ftp ...)

# Mon infra physique

Tout d'abord je possède deux serveurs:
- Un serveur avec des ressources CPU et RAM
- Un serveur de stockage type NAS


{{<mermaid>}}
flowchart LR
    Serveur["Serveur de ressources CPU et RAM"]---|10Gb/s|NAS[NAS]
    Routeur(Routeur)---|1Gb/s|Serveur[Serveur de ressources CPU et RAM]
    Routeur(Routeur)---|1Gb/s|NAS[NAS]
    Internet(((Internet)))---|10Gb/s|Routeur(Routeur)
{{</mermaid>}}
*Schéma de mon infrastructure physique*

## Serveur de ressources CPU et RAM
Pour ce serveur j'ai décidé d'en acheter un, en effet je n'ai pas la possibilité d'en avoir un gracieusement c'est alors que j'ai cherché sur ebay. Et sur Ebay on en trouve un très grand nombre en enchère pour des prix pas trop chère. 

C'est donc avec un **DL360 Gen 9** que l'aventure commence.*(coût de 180€ livraison comprise)*
La config d'origine était la suivante: 
- 2x [Xeon E5-2650 v3](https://www.intel.fr/content/www/fr/fr/products/sku/81705/intel-xeon-processor-e52650-v3-25m-cache-2-30-ghz/specifications.html) *(10 coeur @ 2.3Ghz)*
- 4x 8Gb RAM DDR4 ECC *(32Gb au total)*
- 4x LFF 3.5 HDD avec controlleur RAID

### Quelques upgrades⬆️
![Pimpmyserver](/images/projects/pimp-my-server.png)
*[PSD PimpMyServer](/images/projects/pimp-my-server.psd)*

Sur ce serveur j'ai rajouté des composants afin de le doper un peu.🔥

Le **Pimp**:
- Carte réseau 10G **ASUS XG-C100C**
- SSD **Samsung 980 1TB**
- 3x SAS HP 7.2K 1 TB (RAID 5)


## Serveur NAS

Pour ce serveur j'ai réutilisé des pièce que j'avais déjà en stock à savoir:

- i3 6100
- 2x 8Gb RAM DDR4 *(16 au total)*


### Quelques upgrades⬆️

J'ai ajouté dans ce PC recyclé en NAS:
- Carte réseau 10G **ASUS XG-C100C**
- SSD **Samsung 970 EVO Plus 256GB**
- SSD **Samsung 980 512GB**
- 2x 4To Seagate IronWolf 


# Couche "logicielle"

## Choix des OS

Pour ce qui est de la partie processing j'ai fait le choix de **Proxmox**. Pourquoi ?
> Simple, open-source, compatible avec terraform et très bien géré par le DL360

*Il ne possède peut être pas toute les fonctionnalités avancés de d'autres hyperviseurs mais répond parfaitement à mes besoins de virtualisations.*

Coté NAS mon choix s'est porté sur **TrueNAS** *(anciennement FreeNAS)*. En effet ayant essayé un grand nombre de distributions comme Open Media Vault, je trouve que TrueNAS est de loin le système le **plus stable** que j'ai eu. Je n'ai eu aucun problème en 3 ans, sauf un disque ayant planté du jour au lendemain mais ce n'étais pas lié à TrueNas. J'ai même été étonnée par la simplicité à remplacer le disque défaillant. Aucune ligne de commande tout en GUI, et ça fonctionne du premier coup !

## Stack logicielle 

Voici la liste des différents services en production: 

| Catégorie | Conteneur    | But                                | 
|-----------|--------------|------------------------------------|
| 🔴        | traefik      | Reverse proxy                      |
| 🔴        | authelia     | Gestion de l'authentification      |
| 🔵        | nextcloud    | Stockage des données (cloud)       |
| 🔵        | redis        | nextcloud                          |
| 🔵        | mariadb      | nextcloud                          |
| 🔵        | cron         | nextcloud                          |
| 🟣        | plex         | Gestion des médias                 |
| 🟣        | flood        | Interface sympa pour les torrents  |
| 🟣        | rtorrent     | Client torrent                     |
| 🟣        | ombi         | Gestion des médias                 |
| 🟣        | sonarr       | Gestion des médias                 |
| 🟣        | radarr       | Gestion des médias                 |
| 🟣        | jackett      | Gestion des médias                 |
| 🟣        | flaresolverr | Bypass cloudflare                  |
| 🟣        | nordvpn      | VPN Nordvpn                        |
| 🟠        | grafana      | Création de magnifiques graphiques |
| 🟠        | prometheus   | Stockage des métriques             |
| 🟠        | cadvisor     | Récupération des métriques         |

