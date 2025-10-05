---
title: Analyse des besoins - Cas d'utilisation
---

# Cas d'utilisation

## Vue d’ensemble

Ce document décrit les cas d’utilisation principaux du système, leur portée fonctionnelle, les acteurs impliqués, ainsi que les scénarios de réussite et d’échec. Il sert de base pour l’analyse, le design, les tests d’acceptation et la planification.

## Liste des cas d’utilisation

| ID   | Nom                        | Acteurs principaux | Description                                          |
| ---- | -------------------------- | ------------------ | ---------------------------------------------------- |
| CU01 | Connexion                  | Utilisateur        | L’utilisateur s’authentifie pour accéder au système. |
| CU02 | Inscription                | Visiteur           | Création d’un compte utilisateur.                    |
| CU03 | Réinitialiser mot de passe | Utilisateur        | Récupération d’accès via question unique.            |
| CU04 | Déconnexion                | Utilisateur        | Terminer la session en cours.                        |
| CU04 | Consulter tableau de bord  | Utilisateur        | Accéder aux fonctionnalités de base après connexion. |
| CU04 | Rechercher & filtrer       | Utilisateur        | Rechercher des éléments avec critères et pagination. |

## Détail

### CU01 - Connexion

**Acteurs** : Utilisateur (principal)
**Préconditions** : Le compte existe et n’est pas supprimé.
**PostConditions** : Une session valide (token/JWT/cookie) est émise et associée à l’utilisateur.
**Déclencheur** : L’utilisateur souhaite accéder à une fonctionnalité nécessitant une authentification (clic sur « Se connecter » ou accès à une ressource protégée).
**Dépendances** : Service d’authentification (vérification des identifiants, hachage). Base de données des utilisateurs et des rôles.
**But** : Vérifier l’identité de l’utilisateur et établir une session sécurisée.

### CU02 - Inscription

**Acteurs** : Visiteur (principal)
**Préconditions** : Le visiteur n’a pas encore de compte actif avec le même identifiant (e‑mail/numéro)
**PostConditions** : Un compte est créé.
**Déclencheur** : Le visiteur clique sur « S’inscrire » ou tente d’accéder à une fonctionnalité nécessitant un compte et choisit de créer un compte.
**Dépendances** : Service d’identité
**But** : Permettre à un nouveau visiteur de créer de manière sécurisée un compte utilisateur utilisable par le système.

### CU03 - Réinitialiser mot de passe

**Acteurs** : Utilisateur (principal)
**Préconditions** : Le compte existe
**PostConditions** : Mot de passe mis à jour
**Déclencheur** : L’utilisateur choisit « Mot de passe oublié ? »
**Dépendances** : Service d’identité
**But** : Restaurer l’accès de manière sécurisée sans divulguer d’informations sensibles.

### CU04 - Déconnexion

**Acteurs** : Utilisateur (principal)
**Préconditions** : Session active
**PostConditions** : Jetons révoqués côté serveur
**Déclencheur** : Clic sur « Se déconnecter »
**Dépendances** : Gestionnaire de session/token
**But** : Mettre fin à la session de manière sécurisée

### CU05 - Consulter tableau de bord

**Acteurs** : Utilisateur (principal)
**Préconditions** : Authentification réussie
**PostConditions** : Widgets/données rendus selon permissions ; préférences utilisateur
**Déclencheur** : Redirection après connexion ou navigation explicite
**Dépendances** : API de données
**But** : Offrir une vue synthèse personnalisée des informations et actions clés.

### CU06 - Rechercher & filtrer

**Acteurs** : Utilisateur (principal)
**Préconditions** : Données disponibles et indexées, ou API de recherche prête
**PostConditions** : Résultats paginés/triés
**Déclencheur** : L’utilisateur saisit des termes, applique des filtres, change le tri/pagination.
**Dépendances** : API de listing/recherche
**But** : Permettre de localiser rapidement des éléments selon des critères combinés
