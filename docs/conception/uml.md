---
title: Conception - Diagrammes UML
---

# Diagrammes UML

## Diagrammes de classes

![image info](Class.png)

## Diagrammes de séquence

![image info](Seq_1.png)

![image info](Seq_2.png)

![image info](Seq_3.png)

Dans le design, j’ai séparé l’application en couches (UI, contrôleurs, services, accès aux données) pour garder une bonne abstraction. Ça réduit le couplage entre parties et augmente la cohésion, donc chaque module a un rôle clair et peut évoluer sans casser le reste. J’ai aussi respecté l’encapsulation en cachant les détails internes (appels API, structure des données) derrière des méthodes simples. Enfin, j’ai choisi JSON pour stocker/échanger les données parce que c’est simple, léger et facile à faire évoluer : on peut ajouter des champs ou modifier la structure sans gros changements, ce qui aide la flexibilité et l’intégration.
