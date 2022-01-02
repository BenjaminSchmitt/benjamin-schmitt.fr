---
title: "Hello World!"
date: 2022-01-02
description: "Annonce de l'ouverture du site"
menu:
  sidebar:
    name: Ouverture du site
    identifier: ouverture-site
    parent: annonces
    weight: 1
hero: fete.jpg
tags: ["annonce"]
categories: ["annonce"]
---

Non sans émotions, j'annonce au monde l'ouverture de mon site. :confetti_ball:

# Pourquoi ?

La première motivation est évidemment pour avoir un **portfolio** et une **présence en ligne**, afin de mettre en avant ma modeste personne à de potentiels recruteurs. Je suis pour le moment comblé par mon travail, mais ça ne durera peut être pas des années! Ainsi, avoir une petite vitrine à travers laquelle exposer mes compétences et mes projets personnels est un plus, surtout quand on travaille dans le domaine de l'IT.

Vient ensuite la partie **blog**. J'ai longtemps hésité, et finalement, pourquoi pas ? Je n'ai aucunement la prétention de pouvoir produire du contenu hors du commun, mais je pense que cela peut se transformer en aventure personnelle sympathique: 

- écrire du contenu force à être précis, car si l'on veut être compris, il faut d'abord comprendre. Cela peut être une motivation supplémentaire pour creuser un sujet particulier

- c'est une façon attrayante de documenter mon **lab** et son évolution
 
- l'historique des articles pourra servir à retracer mon **évolution**

Evidemment, pas question de faire de ce site un journal intime, ça ne m'intéressera pas plus que vous. Il sera question uniquement de courts articles autour de sujets IT, pas forcément techniques.

Aucun objectif, aucun calendrier, écrire sera un plaisir, et non une contrainte.

# Parlons peu, parlons technique

Puisque ce blog va être technique, je vais commencer dès maintenant et présenter les technologies utilisées, et pourquoi elles.

Les maîtres mots sont: **simple**, **efficace** et **sécurisé**! Encore une fois, ce site doit être un plaisir, et non une contrainte. Exit donc (pour l'instant ?) les frontend React, les backends C#, les pipelines compliqués de déploiements, les Kubernetes HA: 
> use the right tool for the job

Mon blog est créé avec l'excellent générateur de sites statiques [Hugo](https://github.com/gohugoio/hugo).

**Note**: les sources de ce site sont hébergées publiquement sur un [repo GitHub](https://github.com/BenjaminSchmitt/benjamin-schmitt.fr).

## Création

[Hugo](https://github.com/gohugoio/hugo) est suffisamment **modulaire** pour pouvoir être utilisé avec des centaines de thèmes différents (l'apparence qu'aura le site). Cela rend la création vraiment agréable: plus besoin d'implémenter un design en CSS, il suffit de se concentrer sur le fond, et d'éditer des **fichiers textes**! Hugo s'occupe ensuite de tout, en fusionnant le fond (les fichiers textes) et la forme (le thème soigneusement choisi), puis en exposant le site généré via un serveur web intégré.

Autre avantage indéniable, Hugo et le [thème](https://github.com/hugo-toha/toha.git) sont des [FOSS](https://en.wikipedia.org/wiki/Free_and_open-source_software).

## Exploitation

### Fichiers

L'exploitation est simple: l'artefact généré est un ensemble de **fichiers statiques** HTML, CSS et javascript, sans aucune autre dépendance (pas de base de données!). N'importe quel serveur web sera capable de les exposer. Mieux: le serveur web **intégré** à Hugo est amplement suffisant pour une petite production.

On pourrait tout à fait s'arrêter là, mais je suis allé un peu plus loin en **conteneurisant** le site. Cela apporte une petite couche de sécurité en plus, mais cela me rend surtout indépendant de l'infrastructure sous jacente. Peu importe la plateforme (bare-metal On Prem, machine virtuelle On Prem, VPS, Kubernetes …), tant que je peux installer un *container runtime*, je peux exposer mon site.

Parmi toutes les plateformes d'exécution de conteneurs, j'ai choisi le très classique **docker-compose**, qui me fournit les services d'un orchestrateur de manière très simple, rapide, et légère.

Dans un premier temps, le site sera hébergé sur un minuscule **VPS** [DigitalOcean](https://www.digitalocean.com/) (1 CPU, 1 GB RAM). Je réfléchis cependant à l'héberger sur mon propre matériel, car je suis très attaché à la philosophie du [self hosting](https://en.wikipedia.org/wiki/Self-hosting_(web_services)), pour des raisons de sécurité principalement. J'aurais très certainement l'occasion d'y revenir dans un futur article. 

### Reverse proxy

Je n'expose pas directement à internet le conteneur Hugo car:
- Hugo ne gère pas la création et le renouvellement des certificats TLS
- J'expose plusieurs services conteneurisés sur mon VPS. Or, plusieurs conteneurs ne peuvent pas écouter en même temps sur un même port

C'est tout à fait le use case d'un [reverse proxy](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/).  Parmi tous ceux disponibles, j'ai choisi le très pratique [Caddy](https://caddyserver.com/), car il gère automatiquement les certificats TLS, via l'autorité de certification gratuite [Let's Encrypt](https://letsencrypt.org/). Sa conteneurisation est [enfantine](https://hub.docker.com/_/caddy).

### Protection DDoS

J'étais un peu mal à l'aise d'exposer publiquement l'IP de mon VPS, car cela m'expose à des attaques  [DDoS](https://www.cisco.com/c/fr_fr/products/security/what-is-a-ddos-attack.html). Ce site n'est évidemment pas critique, mais j'héberge d'autres services qui nécessitent un minimum de disponibilité. 

[Cloudflare](https://www.cloudflare.com/fr-fr/) peut tout à fait répondre à ce besoin, car il propose [gratuitement](https://www.cloudflare.com/fr-fr/plans/#overview) des services de:
- **Serveur DNS**, pour gérer les différents noms de domaines sous sa responsabilité
- **Protection des attaques DDoS**, ce qui m'intéresse ici
- **Réseau [CDN](https://azure.microsoft.com/fr-fr/services/cdn/#overview)**, pour améliorer les performances du site, via de la mise en cache des données
- **Analytics** basique, pour suivre l'audience

Pour ce faire, Cloudflare va se mettre en relais entre le client et le site, comme le montre cette image (source Cloudflare):

![proxy cloudflare](/posts/annonces/ouverture-site/cloudflare.png)

Les entrée DNS `*.benjamin-schmitt.fr` pointent donc vers les serveurs de Cloudflare, masquant l'IP réelle de mon VPS. Tous les flux passent pas eux, ce qui explique qu'ils sont en capacité de les bloquer.

La configuration complète de Cloudflare et du serveur cible fera peut être l'objet d'un article futur, car la configuration de Caddy avec le proxy Cloudflare pour chiffrer de bout en bout était intéressante à mettre en place.

### Analytics

Cloudflare propose de l'Analytics, mais très basique dans sa version gratuite. Fort heureusement le thème que j'utilise intègre [Google Analytics](https://analytics.withgoogle.com/), qui propose un suivi beaucoup plus complet.

# Conclusion

Je pense que le contrat de départ (simple, efficace et sécurisé) est respecté. 

J'apprécie **énormément** les outils utilisés, qui permettent de mettre en place un site complet sans connaissances approfondies, en investissant seulement quelques heures et quelques euros par mois!

J'espère vous retrouver rapidement pour un autre article, et vous souhaite une excellente année 2022.

# Crédits

Image de fond par [ktphotography](https://pixabay.com/fr/users/ktphotography-5847971/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2527495) de [Pixabay](https://pixabay.com/fr/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2527495).