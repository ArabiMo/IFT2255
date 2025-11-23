---
title: Analyse des besoins - Présentation générale
---

# Présentation du projet

Il s’agit d’une plateforme web (avec une API REST) conçue pour les étudiants de l’UdeM, en priorité ceux du DIRO. L’idée est d’offrir un outil pratique pour mieux choisir leurs cours.  
La plateforme rassemble à la fois des données officielles (Planifium, résultats agrégés) et des avis d’étudiants (via Discord). L’étudiant peut ainsi consulter des fiches de cours détaillées, comparer plusieurs options et recevoir des recommandations personnalisées en fonction de son profil et de ses intérêts.

## Méthodologie pour la cueillette des données

Les besoins ont surtout été identifiés à partir de mon expérience en développement logiciel et de ma propre expérience d’étudiant au DIRO (choix de cours, consultation de Planifium, échanges sur Discord, etc.).

## Description du domaine

### Contexte actuel

Actuellement, un étudiant du DIRO qui veut choisir ses cours doit :

- Consulter Planifium et les sites officiels pour connaître l’offre de cours, les horaires, les cycles et les préalables.
- Lire la description officielle des cours, qui donne peu d’indications sur la charge de travail réelle, le style d’évaluation ou le niveau de difficulté.
- Demander l’avis d’autres étudiants, souvent de manière informelle (serveurs Discord, conversations privées, groupes Messenger, etc.).
- Faire lui-même la synthèse de toutes ces informations pour vérifier les conflits d’horaire, estimer la charge globale de la session et juger si un cours correspond à ses intérêts.

Les informations pertinentes existent donc déjà, mais elles sont fragmentées (officielles d’un côté, informelles de l’autre) et non centralisées. Il n’y a pas d’outil unique qui rassemble les données officielles et les avis étudiants de façon structurée et anonyme.

### Acteurs du domaine

- **Étudiant / Étudiante du DIRO (ou UdeM)**  
  - Cherche des informations sur les cours (contenu, charge de travail, difficulté, préalables).  
  - Consulte les ressources officielles et les avis d’autres étudiants.  
  - Partage parfois ses propres expériences (évaluations, projets, organisation du cours).

- **Étudiants ayant déjà suivi le cours (pairs)**  
  - Fournissent des retours d’expérience (positifs ou négatifs).  
  - Donnent des détails concrets sur les évaluations, la charge de travail, le style de l’enseignant, etc.  
  - Actuellement, ces avis sont partagés surtout via des canaux informels (ex. Discord).

- **Administrateur de la plateforme (dans le contexte du projet)**  
  - Personne responsable du bon fonctionnement de l’outil (souvent un étudiant responsable ou un membre de l’équipe technique).  
  - Configure les connexions aux sources de données (Planifium, fichiers de résultats).  
  - Gère la modération des avis (suppression des contenus inappropriés, respect de l’anonymat).  
  - Ajuste les paramètres fonctionnels : seuil minimal d’avis pour l’affichage, filtres, règles d’anonymisation, fréquence de mise à jour des données, etc.

### Systèmes et ressources externes

- **Planifium** : système institutionnel qui publie les informations officielles sur les cours (programmes, horaires, préalables, cycles).  
- **Serveurs Discord étudiants** : principaux lieux d’échange informel d’avis et de conseils entre étudiants (avis non structurés).  
- **Service des résultats** : service institutionnel qui génère des fichiers agrégés (moyennes, nombre d’inscrits, taux d’échec), utilisés pour analyser la réussite dans les cours.

Ces systèmes ne sont pas des acteurs, mais des sources de données importantes dans le domaine.

## Hypothèses et contraintes

**Hypothèses :**

1. Les avis et résultats sont transmis respectivement en format JSON et CSV.  
2. Les avis ne sont affichés que si un minimum de 5 est atteint (pour préserver l’anonymat et la représentativité).  
3. L’API Planifium est utilisée uniquement en lecture.  
4. Une base de données locale est utilisée pour sécuriser l’information.

**Contraintes :**

1. La performance dépend des temps de réponse de l’API Planifium.  
2. Les formats imposés (JSON, CSV) doivent être respectés ; s’ils changent, cela pose un risque d’incompatibilité.  
3. Ressources humaines limitées (projet mené principalement en solo).  
4. Contraintes de temps liées à la double réalité d’étudiant à temps plein et d’employé à temps plein.
