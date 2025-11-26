---
title: Conception - Présentation générale
---

# Conception

Cette section présente les grandes lignes de l’architecture et des principaux choix de conception réalisés pour le projet.

## Approche utilisée

L’application repose sur plusieurs principes d’architecture qui visent à la rendre modulaire, maintenable et évolutive :

- **Microservices** : séparation des responsabilités en plusieurs services indépendants, ce qui facilite l’évolution, le déploiement et les tests de chaque partie sans impacter tout le système.
- **Client-side rendering (CSR)** : la logique d’affichage est principalement exécutée côté client, ce qui permet une interface plus réactive et une meilleure expérience utilisateur.
- **Découpage en couches** : l’application est structurée en plusieurs couches logiques (UI, contrôleurs, services, accès aux données), afin de limiter le couplage et de clarifier les responsabilités.

## Contraintes prises en compte

Plusieurs contraintes non fonctionnelles ont orienté les choix de conception :

- **Base de données au format fichier** : utilisation d’un stockage basé sur des fichiers pour faciliter le transport et le partage d’une base de données semi-statique (ex. échange facile entre membres de l’équipe ou enseignants/correcteurs).
- **Manque de main-d’œuvre** : l’équipe étant restreinte, il a fallu privilégier des solutions simples, bien documentées et rapides à mettre en place.
- **Manque de temps** : les choix technologiques et architecturaux ont été orientés vers des approches limitant la complexité (configuration minimale, peu de dépendances, automatisation quand possible).

## Découpage en couches

Pour garder une architecture claire et évolutive, l’application est organisée en plusieurs couches :

- **Couche UI (interface utilisateur)**  
  Gère l’affichage et les interactions avec l’utilisateur (formulaires, pages, composants visuels). Elle ne contient pas de logique métier complexe et délègue les opérations aux couches inférieures.

- **Couche contrôleurs**  
  Sert de point d’entrée entre l’UI et la logique métier. Les contrôleurs reçoivent les actions de l’utilisateur, valident les données et appellent les services appropriés.

- **Couche services (logique métier)**  
  Contient les règles métier et les scénarios d’utilisation (création, modification, validation des données, etc.). Elle orchestre les appels à la couche d’accès aux données sans connaître les détails techniques de stockage.

- **Couche d’accès aux données**  
  Gère la lecture et l’écriture dans la base de données au format fichier. Les détails d’implémentation (chemins, structure interne, sérialisation) sont entièrement encapsulés dans cette couche.

Ce découpage réduit le **couplage** entre les parties du système et augmente la **cohésion** de chaque module : chaque couche a un rôle précis et peut évoluer indépendamment (par exemple, changer le mode de stockage sans modifier l’UI).

## Encapsulation et gestion des données

L’un des objectifs principaux a été de respecter l’**encapsulation** :

- Les détails internes (structure des fichiers, format exact des données, appels internes) sont cachés derrière des **méthodes publiques simples** exposées par les services et la couche d’accès aux données.
- L’UI et les contrôleurs manipulent principalement des **objets métiers** ou des DTO (Data Transfer Objects), sans dépendre des structures techniques utilisées pour le stockage.

## Choix du format JSON

Le projet utilise **JSON** comme format de stockage et d’échange de données :

- Format **simple et léger**, facile à lire et à comprendre même pour les membres de l’équipe non spécialisés.
- **Flexibilité** : il est possible d’ajouter des champs ou de modifier légèrement la structure sans casser tout le système, tant que les services gèrent ces évolutions.
- **Bonne intégration** avec les technologies modernes côté client comme côté serveur (parsers natifs, sérialisation/désérialisation automatique).
- Adapté à une base **semi-statique** : les fichiers peuvent être versionnés, copiés ou transportés facilement (par exemple, via un dépôt Git ou par simple partage de fichiers).

Ces choix contribuent à rendre le système plus robuste face aux changements, tout en respectant les contraintes de temps, de ressources humaines et de simplicité technique.

## Sacrifice

- Temps de chargement des données lors de la visite /grid.
