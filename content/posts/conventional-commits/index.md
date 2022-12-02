---
title: "Conventional Commits"
date: 2022-12-02T23:01:29+01:00
draft: false
tags: ["git"]
categories: ["reference"]
author: "Claude Gamache"
cover:
    image: "images/cover.jpg"
    alt: "Conventional Commits"
    relative: false # To use relative path for cover image, used in hugo Page-bundles
---

## Introduction

```text
[JIRA-5478][TARTANPION][SUJET1][COUCOU] Changement d'url
```

... Moche pas vrai ?

Comment rédiger des commentaires de commits concis mais suffisamment explicite pour expliquer aux copains ce qu'on a
tenté de résoudre sans être noyé dans des tags sans intérêt ?

## Qu'est ce que les Conventional Commits ?

Si vous trainez un peu sur les github open source, ou si vous avez des collègues un peu curieux, vous avez certainement
déjà vu ce genre de message :

```text
feat: allow provided config object to extend other configs
fix(build): change ci url
docs: fix versions in changelog
```

Il s'agit d'une convention, permettant de formater les messages de commits pour les rendres lisibles tout en proposant
plusieurs niveau de lecture au développeur qui consulte l'historique.

Voici la convention en question :

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

- Sur la première ligne, le type d'action, un scope optionnel, ainsi qu'une courte description.

- A la ligne, un peu plus de détails, ou des précisions sur les breaking changes apportés par le commit.

- Dans le footer, éventuellement, les références externes comme la jira ou le nom du reviewer.

### Types de commits

Pour référence, voici les types de commits nommés dans la convention :

- `fix:`
- `feat:`
- `build:`
- `chore:`
- `ci:`
- `docs:`
- `style:`
- `refactor:`
- `perf:`
- `test:`

Même si cette liste n'est qu'une convention, l'idéal est de s'y tenir pour rentrer dans les clous de certains outils,
comme le
[commitlinter](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional). Ce
linter
permet d'ailleurs, si vous le souhaitez, d'ajouter un peu d'automatisme pour s'assurer que tout chaque commit respecte
la convention
avant l'intégration au repository.

### Commit Footer

Il est également d'usage de respecter le template du [git-trailers](https://git-scm.com/docs/git-interpret-trailers),
cela permet de s'assurer
qu'il est bien parsé par les éventuels outils de lecture des commits.

En gros, cela peut se résumer en une liste de `clé: valeur`, un par ligne.

Par exemple :

```text
fix(mailer): set a timeout on smtp client

Ref: JIRA-2546
Reviewer: Claude Gamache <test@test.com>
```

En général, les intégrations gitlab/github vous permettent, en outre, de faire de jolies lien directement vers votre
outils
de ticketing, de cette façon vous avez un jolie message de commit bien propre, affiché correctement peu importe
l'outil utilisé.

# Sources

Un peu de lecture pour aller plus loin !

- [conventionalcommits.org](https://www.conventionalcommits.org/)
- [github.com/conventional-changelog/commitlint](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional)