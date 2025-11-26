---
title: Conception - Architecture
---

# Architecture du système

## Vue d’ensemble

- **Type** : Application web monolithique exposant une API REST (**Backend Java Javalin** + Frontend web).
- **Contexte** : Plateforme de choix de cours pour les étudiant·e·s de l’UdeM, en priorité ceux du DIRO.
- **Raisons du choix** :
  - Équipe réduite → architecture simple à développer et à comprendre.
  - Déploiement rapide : un seul backend et une logique centralisée.
  - Suffisant pour un projet académique, avec possibilité d’évolution plus tard.

## Composants principaux

- **Frontend (interface web)**

  - Application web accessible via navigateur (desktop et mobile).
  - Permet : recherche de cours, fiche détaillée, comparaison et gestion du profil.
  - Communique avec le backend via HTTP(S) et reçoit du JSON.

- **API Backend (Java / Javalin)**

  - Fournit une API REST pour :
    - Recherche de cours et consultation de fiches détaillées.
    - Récupération des préalables d’un cours.
    - Gestion des utilisateurs (inscription, connexion, modification, suppression).
    - Réception d’avis transmis par un bot Discord (fonctionnalité prévue).
  - La logique métier est dans les services, appelés par les controllers.

- **Stockage interne**

  - **Fichier JSON (`users.json`)**
  - Les utilisateurs sont persistés via `JsonUserRepository`.

- **Intégrations externes**
  - **Planifium** : source officielle des cours/horaire/préalables (API en lecture).
  - **Serveur Discord étudiant** : les avis sont écrits sur Discord puis envoyés à notre système par un bot.

## Communication entre composants

- **Frontend ↔ Backend** :

  - Protocole : HTTP(S)
  - Style : REST/JSON
  - Exemples d’endpoints :
    - `/courses`
    - `/courses/{id}`
    - `/courses/{id}/prerequisites`
    - `/users/register`, `/users/login`, `/users/{id}`

- **Backend ↔ Stockage JSON** :

  - `UserService` lit et écrit les utilisateurs via `JsonUserRepository` dans `users.json`.

- **Backend ↔ Systèmes externes** :
  - Planifium : appels HTTP en lecture via `HttpClientApi`.
  - Discord : le bot envoie une requête HTTP contenant un avis structuré (JSON).

## Flux techniques clés

- **Recherche de cours** :

  - Le frontend appelle l’API `/courses` avec des filtres (session, mots-clés, etc.).
  - `CourseService` interroge Planifium et renvoie la liste des cours.

- **Consultation d’une fiche de cours** :

  - Le frontend appelle `/courses/{id}`.
  - Le backend récupère le cours dans Planifium et le retourne.

- **Préalables d’un cours** :

  - Le frontend appelle `/courses/{id}/prerequisites`.
  - Le backend demande à Planifium la liste des préalables et la renvoie.

- **Soumission d’avis (via Discord)** :

  - L’étudiant écrit un avis sur Discord.
  - Le bot le transforme en JSON et l’envoie à l’API du backend.
  - Le backend valide et enregistre l’avis (à intégrer dans le système).

- **Gestion des erreurs** :
  - Codes HTTP explicites (400, 401, 404, 500, etc.).
  - Réponse JSON simple avec un message clair (`ResponseUtil.formatError`).

## Déploiement (version actuelle)

- Projet conçu pour fonctionner simplement :
  - Backend lancé localement (Java).
  - Frontend servi via navigateur.
  - Connexions externes via Internet (Planifium, Discord).
- Une base de données et un déploiement cloud peuvent être ajoutés plus tard si le projet évolue.

---

## Diagramme d’architecture (Modèle C4)

### Niveau 1 – Contexte du système

![C4 Niveau 1 – Contexte du système](C4_1.png)

Ce diagramme montre la plateforme au centre, entourée :

- des **acteurs humains** (Étudiant, Étudiant authentifié, Administrateur) ;
- des **systèmes externes** (API Planifium, Serveur Discord étudiant).  
  Il illustre qui interagit avec le système et pourquoi (rechercher, consulter, comparer, fournir des avis).

### Niveau 2 – Conteneurs

![C4 Niveau 2 – Conteneurs](C4_2.png)

Ce diagramme détaille les conteneurs internes actuels :

- **Frontend Web** : interface utilisée par les étudiant·e·s et l’administrateur.
- **API REST Javalin** : cœur applicatif qui reçoit les requêtes et applique la logique métier.
- **Stockage JSON (`users.json`)** : persistance des utilisateurs. (Dans l'image c'est postgreSql, mais pour des raison pratiques celui deviendra un json dans le c4_3. réalité du terrain)

Il montre aussi les liens de communication avec Planifium et Discord.

### Niveau 3 – Composants de l’API REST

![C4 Niveau 3 – Composants API](C4_3.png)

Le backend est séparé en trois couches :

- **Controllers** : reçoivent les requêtes, valident les entrées et appellent les services.
- **Services** : contiennent la logique principale (`CourseService`, `UserService`).
- **Repository JSON** : gère la sauvegarde des utilisateurs (`JsonUserRepository`).

Cette structure sépare bien les responsabilités et rend le système facile à maintenir et à faire évoluer.
