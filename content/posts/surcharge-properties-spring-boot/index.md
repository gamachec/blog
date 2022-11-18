---
title: "Comment surcharger les propriétés d'une application Spring Boot ?"
date: 2022-11-18T17:58:29+01:00
draft: false
tags: ["spring"]
categories: ["how-to"]
author: "Claude Gamache"
cover:
    image: "images/cover.webp"
    alt: "Comment surcharger les propriétés d'une application Spring Boot ?"
    relative: false # To use relative path for cover image, used in hugo Page-bundles
---

J’ai récemment souhaité tester, sur mon poste, l’image docker d’une application Spring Boot que j’étais en train de développer.

La première contrainte quand l’application démarre dans un conteneur, comment lui indiquer, sans avoir accès à ses properties, qu’il doit utiliser la base de données qui tourne sur mon poste ?

Une application Spring Boot est packagée avec son fichier properties, en général on y indique l’emplacement de la base de données, les URLs vers les autres services qu’il utilise, les informations d’authentification nécessaires à son fonctionnement. Nous avons plusieurs options pour packager ces fichiers properties :

- A vide, pour indiquer qu’il est nécessaire d’injecter les bonnes valeurs en fonction de l’environnement au moment où on lance l’application.
- Avec des données par défaut, que ce soit pour l’environnement de développement ou de production, afin d’être le plus autoportant possible.

Dans les deux cas, il est souvent nécessaire de surcharger certaines valeurs, soit parce que l’application ne démarre pas sans avoir ses valeurs, soit parce que certaines informations sont trop sensibles pour pouvoir les intégrer dans l’application.

Alors, comment les injecter proprement ?

Pourquoi pas en utilisant la façon la plus répandue et la plus universelle : en utilisant les variables d’environnement ?

## Pourquoi utiliser les variables d’environnement ?

Premièrement, pour une question de priorité.

Les variables d’environnement sont très hautes dans la hiérarchie des propriétés résolues par Spring Boot, elles passeront systématiquement devant les autres fichiers de propriétés chargés, peu importe le profil utilisé (cf https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config). Idéal pour une surcharge efficace.

Ensuite, les variables d’environnement peuvent être utilisées peu importe le framework et peu importe le langage. Elles sont prises en charge nativement par la totalité des outils, et tous les orchestrateurs modernes fournissent des mécanismes efficaces pour masquer les informations confidentielles comme les mots de passes ou les jetons d’accès tout en les injectant par variable d’environnement.

## Transformer une propriété en variable d’environnement

Spring Boot utilise les relaxed bindings pour tenter d’associer une variable d’environnement ou une propriété à son équivalent dans le code. Ainsi, toutes ces écritures sont équivalentes et seront appliqués à la même propriété lors du lancement de l’application :

- La propriété `project.person.first-name`
- La propriété `project.person.firstName`
- La variable d’environnement `PROJECT_PERSON_FIRSTNAME`

Voici deux méthodes pour récupérer le nom d'une variable d'environnement à partir du nom de la propriété.

### A la main

Pour trouver le nom de la variable d’environnement à utiliser, appliquer les règles suivantes :

1. Transformer les `.` en `_`
1. Retirer les `-`
1. Tout mettre en majuscule (par convention)
1. Faire apparaître les index d’une listes numériquement entre `_`

Par exemple, ce fichier Yaml :

```
project:
  person:
  - first-name: John
    last-name: Doe
```

Peut être surchargé avec ces deux variables d’environnement :
```
PROJECT_PERSON_0_FIRSTNAME=John
PROJECT_PERSON_0_LASTNAME=Doe
```

### Avec le plugin IntelliJ Yaml2env

Si vous devez surcharger plusieurs propriétés, il est possible de réaliser la transformation de façon plus industrielle, en utilisant le plugin Yaml2env sur IntelliJ IDEA :

- https://plugins.jetbrains.com/plugin/19905-yaml2env

Ce plugin vous permet de sélectionner un fichier yaml Spring Boot et de transformer toutes les propriétés qui s’y trouvent en variable d’environnement correctement nommées dans le presse-papier.

Il suffira ensuite de les coller où vous voulez les utiliser en adaptant les valeurs, on se retrouve juste après !

## Appliquer les variables d’environnement

### Dans un lanceur IntelliJ IDEA

### Dans un conteneur Docker

### Dans un déploiement Kubernetes

