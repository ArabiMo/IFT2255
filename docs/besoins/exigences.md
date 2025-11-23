---
title: Analyse des besoins - Exigences
---

# Exigences

## Exigences fonctionnelles

- [ ] EF1 : L’étudiant peut rechercher un cours par code, titre ou mot-clé.
- [ ] EF2 : Le système affiche une fiche de cours (informations générales, prérequis, horaires, résultats agrégés, avis).
- [ ] EF3 : L’étudiant peut comparer plusieurs cours entre eux (charge totale estimée, conflits d’horaires, compatibilité avec son profil).
- [ ] EF4 : L’étudiant peut créer et modifier un profil (par exemple charge de travail souhaitée, préférence théorie/pratique, domaines d’intérêt).
- [ ] EF5 : Les avis étudiants collectés sont intégrés et anonymisés avant d’être affichés à l’écran.
- [ ] EF6 : Le système ingère automatiquement les données provenant des sources externes (Planifium, résultats agrégés) sans intervention manuelle.

## Exigences non fonctionnelles

- [ ] ENF1 (Performance) :  
  Pour une recherche classique (par code ou mot-clé) sur l’ensemble des cours du DIRO, 95 % des réponses doivent être retournées en moins de 2 secondes, hors indisponibilité des services externes (ex. API de Planifium).

- [ ] ENF2 (Compatibilité et adaptabilité) :  
  L’application doit être utilisable sur les principaux navigateurs de bureau (Chrome, Firefox, Edge, versions récentes) ainsi que sur mobile.  
  Sur un écran d’ordinateur portable et sur un téléphone intelligent de petite résolution, les pages principales (recherche, fiche de cours, comparaison) doivent rester lisibles sans défilement horizontal.

- [ ] ENF3 (Gestion des données personnelles et conformité – Loi 25) :  
  Les renseignements personnels éventuellement collectés (par exemple, identifiant de session ou profil associé à un étudiant) doivent être limités au strict nécessaire pour le fonctionnement de la plateforme.  
  Ces données ne sont conservées que pour la durée nécessaire aux objectifs pédagogiques du projet (par exemple, jusqu’à la fin de la session d’évaluation), puis supprimées de manière sécuritaire (purge de la base, suppression des sauvegardes associées).

- [ ] ENF4 (Confidentialité et anonymisation) :  
  Les avis et profils utilisés par le système pour l’affichage et les recommandations ne doivent contenir aucune information permettant d’identifier directement un étudiant (nom, courriel institutionnel, identifiant Discord, etc.).  
  Seules des données anonymisées (code de cours, année, session, note ou appréciation, indicateurs agrégés) sont conservées, ce qui réduit l’impact en cas de fuite de données.

- [ ] ENF5 (Robustesse et tolérance aux erreurs) :  
  En cas de données externes invalides ou incomplètes (fichier de résultats corrompu, format inattendu, réponse partielle d’une API), le système doit :  
  - journaliser l’erreur (log),  
  - informer l’administrateur ou l’utilisateur concerné via un message explicite,  
  - continuer à fonctionner en mode dégradé (par exemple en affichant les informations disponibles sans provoquer d’arrêt complet de l’application).

## Priorisation

Critiques : EF1, EF2, EF3, EF5, ENF1, ENF2, ENF3.  

Importantes : EF4, EF6, ENF4, ENF5.

## Types d'utilisateurs

| Type d’utilisateur   | Description                            | Exemples de fonctionnalités accessibles                                       |
| -------------------- | -------------------------------------- | ----------------------------------------------------------------------------- |
| Étudiant invité      | Accès limité, pas de profil enregistré | Recherche simple, consultation de fiches de cours avec informations de base   |
| Étudiant authentifié | A un profil stocké temporairement      | Recherche, comparaison, avis, recommandations adaptées à son profil           |
| Administrateur       | Supervise et modère le système         | Suivi des intégrations, gestion des signalements, ajustement des paramètres   |

## Infrastructures

- Le système sera hébergé sur un serveur Ubuntu 22.04.
- Base de données : PostgreSQL version 15 avec Spring Data JPA.
- Serveur Web : Spring Web.
- Framework principal : Spring Boot 3.
