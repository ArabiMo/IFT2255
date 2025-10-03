---
title: Analyse des besoins - Exigences
---

# Exigences

## Exigences fonctionnelles

- [ ] EF1 : L’étudiant peut rechercher un cours par code, titre ou mot-clé.
- [ ] EF2 : Le système affiche une fiche de cours (infos générales, prérequis, horaires, résultats agrégés, avis).
- [ ] EF3 : L’étudiant peut comparer plusieurs cours entre eux (charge totale, conflits d’horaires, compatibilité avec son profil).
- [ ] EF4 : L’étudiant peut créer et modifier un profil (ex. charge de travail souhaitée, préférences théorie/pratique).
- [ ] EF5 : Les avis étudiants collectés via Discord sont intégrés et anonymisés avant publication.
- [ ] EF6 : Le système ingère automatiquement les données provenant de Planifium (API) et des fichiers de résultats agrégés (CSV).

## Exigences non fonctionnelles

- [ ] ENF1 : Les temps de réponse doivent être raisonnables (idéalement < 2 secondes pour une recherche).
- [ ] ENF2 : L’application doit fonctionner sur les principaux navigateurs (Chrome, Firefox, Edge) et resolution (ecran et cellulaire).
- [ ] ENF3 : La base de données locale doit être supprimée et recréée après chaque session, afin de respecter la confidentialité et la loi 25.
- [ ] ENF4 : Les données sensibles (avis, profils) doivent être anonymisées et stockées uniquement de façon temporaire.
- [ ] ENF5 : L’application doit être robuste face à des formats de données incorrects (JSON/CSV invalides).

## Priorisation

Critiques : EF1, EF2, EF3, EF5, ENF2, ENF3.

Importantes : EF4, EF6, ENF2.

## Types d'utilisateurs

| Type d’utilisateur   | Description                            | Exemples de fonctionnalités accessibles                   |
| -------------------- | -------------------------------------- | --------------------------------------------------------- |
| Étudiant invité      | Accès limité, pas de profil enregistré | Recherche simple, fiche de cours basique                  |
| Étudiant authentifié | A un profil stocké temporairement      | Recherche, comparaison, avis, recommandations adaptées    |
| Administrateur       | Supervise et modère le système         | Validation des flux, gestion des signalements, paramètres |

## Infrastructures

> Informations sur l’environnement d’exécution cible, les outils ou plateformes nécessaires.

- Le système sera hébergé sur un serveur Ubuntu 22.04.
- Base de données : PostgreSQL version 15.
- Serveur Web : Nginx + Gunicorn (pour une app Python, par exemple).
- Framework principal : [À spécifier selon le projet].

<!-- TODO: Compléter selon le stack technique prévu. -->
