---
title: Analyse des besoins - Présentation générale
---

# Présentation du projet

Il s’agit d’une plateforme web (avec une API REST) pensée pour les étudiants de l’UdeM, en priorité ceux du DIRO. L’idée est d’offrir un outil pratique pour mieux choisir leurs cours.
La plateforme rassemble à la fois des données officielles (Planifium, résultats agrégés) et des avis d’étudiants (via Discord). L’étudiant peut ainsi consulter des fiches de cours détaillées, comparer plusieurs options, et recevoir des recommandations personnalisées en fonction de son profil et de ses intérêts.

## Méthodologie pour la cueillette des données

    Les besoins ont surtout été identifiés à partir de mon expérience en développement logiciel.

## Description du domaine

### Fonctionnement

    Découverte : recherche d’un cours par code, titre ou mots-clés.

    Fiche de cours : affichage des informations principales (crédits, cycle, horaires, préalables/co-requis, etc.), des résultats agrégés (moyennes, taux d’échec) et, si disponibles, des avis d’étudiants.

    Personnalisation : chaque étudiant peut créer un profil (ex. préférence théorie/pratique, charge de travail visée, domaines d’intérêt). Ce profil influence l’ordre de tri et les étiquettes affichées.

    Comparaison : possibilité de sélectionner plusieurs cours pour voir la charge de travail totale estimée, les conflits d’horaires et la compatibilité avec le profil.

    Collecte d’avis : un bot Discord permet de recueillir des avis structurés (format JSON). Ces avis sont anonymisés, modérés et intégrés à la base pour enrichir les statistiques.

### Acteurs

    Étudiant : consulte, recherche, compare, crée son profil, partage ses avis.

    Bot Discord : récupère les avis et les transmet à l’API.

    Planifium : fournit les informations officielles (programmes, horaires, prérequis).

    Service résultats : donne accès à des fichiers CSV agrégés (moyennes, nombre d’inscrits, taux d’échec).

    Administrateur : valide les flux, gère les signalements et ajuste les paramètres (seuils, filtres, etc.).

### Dépendances

    API Planifium (lecture)

    Discord (webhook/bot)

    Stockage (DB locals)

    CI/CD (tests, déploiement)

## Hypothèses et contraintes

    Hypothèses :

    1. Les avis et résultats sont transmis respectivement en format JSON et CSV.

    2. Les avis ne sont affichés que si un minimum de 5 est atteint (pour préserver l’anonymat et la représentativité).

    3. L’API Planifium est utilisée uniquement en lecture.

    4. Une base de données locale est utilisée pour sécuriser l’information.

    Contrainte :

    1. La performance dépend des temps de réponse de l’API Planifium.

    2. Conformité à la loi 25 : la base est réinitialisée après une session pour respecter le consentement et la protection des données.

    3. Les formats imposés (JSON, CSV) doivent être respectés; s’ils changent, cela pose un risque d’incompatibilité.

    4. Ressources humaines limitées (projet mené principalement en solo).

    5. Contraintes de temps liées à la double réalité d’étudiant à temps plein et d’employé à temps plein.
