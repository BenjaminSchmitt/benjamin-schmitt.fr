---
title: "Hello World!"
date: 2021-12-19
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

Non sans émotions, j'annonce au monde l'ouverture de mon site [benjamin-schmitt.fr](https://www.benjamin-schmitt.fr). :confetti_ball:

# Pourquoi ouvrir un site ?

La première raison est évidemment pour avoir un **portfolio** et une **présence en ligne**, afin de mettre en avant ma modeste personne à de potentiels recruteurs. Je suis pour le moment comblé par mon travail, mais ça ne durera peut être pas des années! Ainsi, avoir une petite vitrine à travers laquelle exposer mes compétences et mes projets personnels est toujours un plus, surtout quand on travaille dans le domaine de l'IT.

Vient ensuite la partie *blog*, et finalement, pourquoi pas ? Pas question de faire de ce site un journal intime, ça ne m'intéressera pas plus que vous. Il sera question uniquement de courts articles autour de sujets IT. Je ne m'attends évidemment pas à avoir beaucoup de lecteurs, puisqu'il existe sur le net des milliers de personnes plus compétentes que moi, qui créent du contenu en ligne hyper intéressant et qualitatif.

Cependant, je pense que cela peut se transformer en aventure personnelle sympa: 
- écrire du contenu force à être précis, car si l'on veut être compris, il faut d'abord comprendre. Cela peut être une motivation supplémentaire pour creuser un sujet particulier
- c'est une façon attrayante de documenter mon *lab* et son évolution
- l'historique des articles pourra servir à retracer mon évolution et apprendre de mes erreurs

Évidemment, aucune pression, aucun calendrier, écrire sera un plaisir, et non une contrainte.

# Comment ?
 
Puisque ce blog va être technique, je ne peux pas me permettre de ne pas présenter les technologies, et pourquoi elles.

Les maîtres mots sont: **simple** et **efficace**! Encore une fois, ce site doit être un plaisir, et non une contrainte. Exit donc les frontend React, les backends C#, les pipelines compliqués de déploiements, les kubernetes HA: **use the right tool for the job**.

Ce site est **statique**, généré avec l'excellent outil [Hugo](https://github.com/gohugoio/hugo). Comme je suis médiocre dans tout ce qui touche au design et au CSS par manque d'appétence, j'utilise un thème open source, [Toha](https://github.com/hugo-toha/toha.git), que je trouve très bien fait et beau.

La construction du blog devient alors un jeu d'enfant: il suffit d'éditer des fichiers textes au format `.yaml`! De cette manière, je me concentre sur le fond, et non la forme. 

L'exploitation via [docker-compose](https://docs.docker.com/compose/) est également simplissime, [Caddy](https://caddyserver.com/) s'occupe quant à lui de l'exposition HTTPS, et de la gestion automatique des certificats TLS.

Par ailleurs, les sources de ce site sont hébergées publiquement sur un [repo Github](https://github.com/BenjaminSchmitt/benjamin-schmitt.fr).

# Où ?

Dans un premier temps, le site sera hébergé sur un VPS [DigitalOcean](https://www.digitalocean.com/). Je réfléchis cependant à l'héberger sur mon propre matériel, car je suis très attaché à la philosophie du [self hosting](https://en.wikipedia.org/wiki/Self-hosting_(web_services)).

Image par [ktphotography](https://pixabay.com/fr/users/ktphotography-5847971/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2527495) de [Pixabay](https://pixabay.com/fr/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2527495).
