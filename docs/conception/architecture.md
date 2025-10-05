---
title: Conception - Architecture
---

# Architecture du système

## Vue d’ensemble

- **Type** : Monolithique REST (Backend Java Spring Boot + Frontend web)
- **Raisons du choix** :
  - Équipe petite → moins de complexité opérationnelle
  - Mise en place rapide (un seul déploiement)
  - Cohérence des données (une base)
  - Facile à tester et à maintenir au début

## Composants principaux

- **Frontend (Interface)**
  - Web (ex. React/Vite) – pages, formulaires, tableau de bord
  - Appelle l’API backend via HTTP(S)
- **API Backend**
  - Java Spring Boot (REST)
  - Modules internes :
    - Authentification (JWT), Autorisations (rôles)
    - Gestion des utilisateurs (CRUD, rôles, profils)
  - Services techniques :
    - Validation, Logs, Audit, Tâches planifiées
- **Base de données**
  - PostgreSQL
  - ORM: Spring Data/JPA,

## Communication entre composants

- **Frontend ↔ Backend** : HTTP(S) REST
- **Backend ↔ BD** : JDBC via JPA
- **Format des données** : JSON (principal), CSV pour export
- **Sécurité** :
  - Auth: JWT (Bearer)
  - Chiffrement: HTTPS (TLS)
  - CORS activé pour le domaine du frontend
  - Rôles: Admin, User (minimum)

## Flux techniques clés

- **Login** : Frontend → /auth/login → JWT → stockage sécurisé (memory/httponly)
- **Gestion des erreurs** : codes HTTP clairs (400, 401, 403, 404, 409, 500) + message JSON
- **Pagination/tri** : paramètres `page`, `size`, `sort`
- **Validation** : côté frontend + backend (Bean Validation)

## Déploiement

- **Cible** : Ubuntu 22.04
- **Empaquetage** : Docker (1 conteneur backend, 1 frontend, 1 PostgreSQL ou DB managée)
- **Réseau** : Backend (HTTP interne) → PostgreSQL
- **CI/CD** : build, tests, scan, déploiement auto (option)
- **Sauvegardes** : dumps réguliers PostgreSQL, versionner les migrations

## Performance & Scalabilité

- Cache léger (Spring Cache) pour listes stables
- Index BD sur colonnes de filtre/recherche

## Diagramme d’architecture (Modèle C4)
