# Happyning ! 📅

## What is this? 🤔

Cette application est une application de gestion d'évènements.
Elle permet notamment la création des évents, et d'en afficher la liste.

Il s'agit d'un test technique pour la société [Kumojin](https://kumojin.com/).

## Fonctionnalités ✨

Quoi de mieux pour comprendre les fonctionnalités d'une application que quelques user story ?
Et pourquoi pas même un peu de gherkin ?!

### US1-1 : Créer un évènement 🔖

```gherkin
Feature: Créer un évènement
  En tant qu'utilisateur,
  Je veux pouvoir créer un évènement,
  Afin de pouvoir le partager.

  Scenario Outline: Créer un évènement
    Given je créé un évènement <evenement> du <date_debut> au <date_fin>, à propos de <description>
    When je valide la création
    Then l'évènement <evenement> est créé
    Examples:
      | evenement                    | date_debut | date_fin   | description                                     |
      | Fête de la soupe de Wazemmes | 2024-02-01 | 2024-02-02 | La légende raconte qu'on y boit peu de soupe... |
      | Igloofest Festival           | 2024-02-01 | 2024-03-02 | Un incontournable ! Venez nombreux <3           |
      | Fête de la bière             | 2024-02-01 | 2024-04-02 | Le nord de la France, dans toute sa splendeur ! |

  Scenario Outline: Créer un évènement avec une date de fin antérieure à la date de début
    Given je créé un évènement <evenement> du <date_debut> au <date_fin>, avec <description>
    When je valide la création
    Then l'évènement <evenement> n'est pas créé
    And je suis notifié que la date de fin est antérieure à la date de début
    Examples:
      | evenement                    | date_debut | date_fin   | description                                     |
      | Fête de la soupe de Wazemmes | 2024-02-01 | 2024-01-02 | La légende raconte qu'on y boit peu de soupe... |
      | Igloofest Festival           | 2024-03-01 | 2024-02-02 | Un incontournable ! Venez nombreux <3           |
      | Fête de la bière             | 2024-04-01 | 2024-03-02 | Le nord de la France, dans toute sa splendeur ! |

```

### US1-2 : Afficher la liste des évènements 📋

En tant qu'utilisateur,
Je veux pouvoir afficher la liste des évènements,
Afin de pouvoir voir les évènements créés.

> Pas besoin de gherkin ici, vous avez compris le principe !

### US1-3 : L'heure des évènements 🕙

En tant qu'utilisateur,
Je veux pouvoir afficher l'heure des évènements,
Afin de pouvoir savoir à quelle heure se déroule l'évènement.

```gherkin
Feature: Heure des évènements
  En tant qu'utilisateur,
  Je veux pouvoir afficher l'heure locale des évènements,
  Afin de pouvoir savoir à quelle heure se déroule l'évènement sur le territoire où il se déroule.
```

## Conception 🤓

### Architecture 🔨

L'application est composée de 3 couches :
- Un front en React
- Un back Spring Boot
- Une base de données MySQL

![HG](assets/hg.png)

### Front ⭐

#### Technologie

Le front est une application React, créée avec `Create React App`.

La technologie a été sélectionnée selon les critères suivants : 
- Facilité de mise en place
- Performance générales
- Pas de besoin explicite en SEO (et conversion possible avec Next JS si besoin)
- Compétences de l'équipe

Les composants seront créés en utilisant des fonctions, et non des classes.

Le langage TypeScript sera utilisé afin de faciliter la maintenance et la lisibilité du code.

#### Dépendances

La dépendance 'Prime React' sera également utilisée afin de faciliter la mise en place de l'interface.

Il a été défini qu'il n'était pour le moment pas nécessaire d'utiliser un gestionnaire d'état (Redux, Context API, ...),
compte tenu du faible nombre de composants et de la simplicité de l'application.

Des modèles de données seront toutefois utilisés afin de faciliter la communication entre les composants.

#### Architecture 

Les services d'accès aux données seront séparés en deux catégories :
- Repositories : Accès aux données (en charge des requêtes HTTP)
- Services : Logique, conversion ...

L'utilisation de custom hooks sera également privilégiée afin de faciliter la réutilisation de code.

#### Nomenclature

- La norme Angular sera utilisée pour nommer les différents composants
- Cette norme ajoute un suffixe au nom des composants, afin de faciliter leur identification :
  - `.page` : Page
  - `.component` : Composant
  - `.service` : Service
  - `.model` : Modèle de données
  - `.hook` : Hook
  - etc.

> Ex : `Homepage.tsx` deviendra `Homepage.page.tsx`

#### Tests 🧪

Les tests seront réalisés avec `Jest` et `React Testing Library`.
Les composants seront essentiellement testés sur leur rendu, s'agissant de composants importés d'une librairie externe.
La couche service sera également testée, afin de s'assurer du bon fonctionnement de la logique s'il y a lieu.

Il n'a pas été jugé utile ou pertinent de développer l'application en TDD.

### Back 👽

#### Technologie 🛠

Le back est une application Spring Boot, créée avec `Spring Initializr`.

La technologie a été sélectionnée selon les critères suivants :
- Facilité de mise en place
- Temps de développement fortement réduit grâce à l'auto-configuration/ORM
- Architecture n-tiers (et non micro-services) pour une application de cette taille
- Compétences de l'équipe

> L'application n'est pas déployée sur le cloud

#### Dépendances 📦

Les dépendances suivantes seront utilisées :
- Spring Web : Pour la création d'une API REST
- Spring Data JPA : Pour la création d'un ORM
- Spring Validation : Pour la validation des données
- Lombok : Pour la génération de code
- MySQL Driver : Pour la connexion à la base de données
- Spring Boot DevTools : Pour le hot-reload

#### Tests 🧪

La couche service uniquement sera testée.
La couche repository ne sera pas testée, car elle est générée par l'ORM.
Le contrôleur ne sera pas testé, car il s'agit d'une simple couche de communication.

Il n'a pas été jugé utile ou pertinent de développer l'application en TDD,
car le temps de développement est court et l'application contient peu, voire pas de logique métier.

#### Base de données 💾

La base de données est une base de données MySQL, hébergée sur un serveur local.

La technologie a été sélectionnée selon les critères suivants :
- Facilité de mise en place
- Compétences de l'équipe
- Pas de besoin explicite en scalabilité

> Les compétences attendues dans l'offre ! :D

Si le besoin évolue, il sera possible de migrer.
Il ne sera pas utilisé de triggers, ou procédures stockées.

### Modèle de données 📟

Le modèle de données est le suivant :

![Model](assets/model.png)

---

## Gestion de projet 📈

- Le projet sera géré en utilisant github projects
- Des tâches seront créées pour chaque fonctionnalité

### Organisation des branches 🌳

Les branches seront organisées en utilisant le modèle GitHubFlow :
- Une branche `master` principale
- Des releases tirées de master
- Des branches `feature/<nom>` tirées de `master`

### Organisation des commits 📝

Les commits suivront un agglomérat de deux conventions :
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [Gitmoji](https://gitmoji.dev/)

Le conventional commit sera placé en préfixe du message de commit, et le gitmoji en suffixe.
L'utilisation de conventional commit permettra de gérer facilement les changelogs via `standard-version`.

### Organisation des PR 📝

- Bien qu'il n'y ait qu'un seul développeur, les PR seront tout de même utilisées
- Elles seront utilisées afin d'assurer une auto relecture ainsi qu'une meilleure traçabilité
- Elles permettront aussi de déclencher les pipelines

## Automatisations 🤖

Plusieurs automatisations sont prévues : 

- Automatisation des tests pour le front et le back
- Automatisation du déploiement du front et du back (Dockerhub)

## Organisation des répository

- Un répository sera créé pour le front
- Un répository sera créé pour le back
- Un répository sera créé pour regrouper les deux autres, en utilisant git submodules (mode dev)
- Un répository seré créé pour le déploiement (mode prod)

## Déploiement 🚀

- Le répository `Happyning` sera utilisé pour déployer l'application
- Il contiendra un docker-compose, qui permettra de déployer l'application en local
- Il sera basé sur les images dockerhub des deux autres répository
- Il sera également possible de lancer la stack en mode dev depuis ce même répository

----

### Temps de développement ⌛

Le temps recommandé pour l'exercice est de 4 à 5h environ.
Dans les faits :

- Conception, recherches : 1h environ
- Back : environ 2h
- Front : environ 3h
- Gateway : environ 45mn
- CI/CD : environ 1h
- Rédaction : environ 1h

----

## Etapes suivantes 🚀

- Augmentation du coverage du front (70% actuellement, CF pipelines)
- Intégration de SonarCloud pour l'analyse du back et du front
- Style sur le front
- Déploiement automatique sur dockerhub + rework du docker-compose de prod
- Amélioration des messages d'erreur renvoyés par le back
- Amélioration des messages d'erreur sur le front
