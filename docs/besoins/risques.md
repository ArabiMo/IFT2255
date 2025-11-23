---
title: Analyse des besoins - Risques
---

# Analyse des risques

## Identification des risques

### Risque 1 – Indisponibilité du développeur principal

- **Probabilité** : Moyenne  
- **Impact** : Élevé  
- **Justification** : Le projet est mené principalement en solo, par une personne qui est à la fois étudiant à temps plein et employé à temps plein. Une surcharge, une maladie ou une période d’examens peut entraîner un arrêt ou un ralentissement important du développement.  
- **Plan de mitigation** :
  - Découper le projet en petites fonctionnalités livrables.
  - Prioriser les fonctionnalités essentielles (MVP).
  - Documenter les décisions et l’architecture pour faciliter une reprise ultérieure.
  - Prévoir des jalons réalistes en fonction du calendrier académique.

---

### Risque 2 – Besoins mal compris ou insuffisamment validés

- **Probabilité** : Moyenne  
- **Impact** : Moyen à élevé  
- **Justification** : Les besoins sont principalement définis à partir de mon expérience personnelle (étudiant + développeur). Sans validation auprès d’autres étudiant·e·s ou parties prenantes, il existe un risque que certaines fonctionnalités importantes soient oubliées, ou que la solution ne réponde pas réellement aux usages les plus fréquents.  
- **Plan de mitigation** :
  - Discuter du projet avec quelques étudiant·e·s du DIRO pour valider les besoins (entretiens informels, petit sondage).  
  - Identifier les cas d’usage principaux (ex. choisir ses cours, éviter les conflits, estimer la charge) et les valider avec au moins 2–3 personnes.  
  - Mettre à jour l’analyse des besoins après un premier retour sur un prototype.  
  - Documenter clairement les hypothèses (pour pouvoir les réviser plus tard).

---

### Risque 3 – Technologie inadaptée nécessitant un retour en arrière

- **Probabilité** : Moyenne  
- **Impact** : Élevé  
- **Justification** : Le choix de technologies (framework web, base de données, intégration avec les API externes) peut s’avérer inadapté : difficile à maintenir, mal supporté ou trop complexe pour le temps disponible. Dans ce cas, il pourrait être nécessaire de revenir en arrière (réécriture partielle ou totale), ce qui consommerait beaucoup de temps.  
- **Plan de mitigation** :
  - Définir des critères simples de choix (stabilité, documentation, courbe d’apprentissage, support communautaire).  
  - Réaliser une petite preuve de concept avant de s’engager pleinement.  
  - Prévoir une « option B » réaliste (technologie plus simple ou déjà maîtrisée).  
  - Limiter le couplage entre la logique métier et les outils techniques pour faciliter une migration.

---

### Risque 4 – Indisponibilité ou changement des sources de données externes

- **Probabilité** : Moyenne  
- **Impact** : Élevé  
- **Justification** : La plateforme dépend de sources externes comme Planifium et des fichiers de résultats agrégés. Si l’API change, devient indisponible ou si les formats de données sont modifiés, certaines fonctionnalités (horaires, préalables, statistiques) peuvent cesser de fonctionner.  
- **Plan de mitigation** :
  - Isoler les intégrations externes dans des modules clairement définis.  
  - Prévoir une couche d’adaptation pour gérer les changements de format.  
  - Mettre en place des scripts de validation des données importées.  
  - Définir des comportements de repli (fallback) : affichage partiel ou message explicatif en cas d’indisponibilité.

---

### Risque 5 – Non-conformité ou mauvaise gestion des données personnelles (Loi 25)

- **Probabilité** : Basse à moyenne  
- **Impact** : Élevé  
- **Justification** : Le projet manipule des données liées à des étudiants (avis, éventuellement identifiants techniques). Une mauvaise anonymisation ou une conservation trop longue des données peut aller à l’encontre des exigences de la Loi 25 et nuire à la confiance des utilisateurs.  
- **Plan de mitigation** :
  - Limiter les données personnelles collectées au strict nécessaire.  
  - Anonymiser les avis avant leur stockage et leur affichage.  
  - Réinitialiser régulièrement la base de données (par session, par exemple).  
  - Documenter les pratiques de gestion des données et obtenir un consentement explicite lorsque requis.

## Modification du processus opérationnel

En fonction des risques identifiés, le processus de développement et d’exploitation de la plateforme devra être ajusté :

- **En cas de Risque 3 (technologie inadaptée)** :
  - Geler l’ajout de nouvelles fonctionnalités pendant la migration.
  - Planifier une phase de refactorisation ciblée (modules les plus critiques en premier).
  - Mettre à jour les tests automatisés pour sécuriser le retour en arrière.
  - Communiquer clairement les impacts et l’échéancier aux parties prenantes.

- **En cas de Risque 4 (problèmes avec les sources externes)** :
  - Ajouter un contrôle systématique de la disponibilité et du format des données externes.
  - Adapter le processus d’importation (scripts, mapping) dès qu’un changement est détecté.
  - Mettre à jour la documentation interne sur les flux de données.

- **En cas de Risque 5 (problèmes de données personnelles)** :
  - Réviser les procédures de collecte, d’anonymisation et de purge des données.
  - Ajuster les consentements et les messages d’information aux utilisateurs.
  - Renforcer les contrôles d’accès à la base de données.
