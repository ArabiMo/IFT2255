---
title: Évaluation et tests
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Tests et évaluation

## Plan de test

### Types de tests réalisés

Conformément aux consignes du projet :

- **Tests unitaires (JUnit 5)**

  - Minimum exigé : **3 tests unitaires par membre**.
  - Pour ma partie (contrôleur `UserController`), j’ai écrit **7 tests unitaires** dans la classe `UserControllerTest`, ce qui dépasse le minimum requis.
  - Les tests portent uniquement sur des **méthodes publiques “intéressantes”** (handlers du contrôleur) :
    - `getAllUsers`
    - `getUserById`
    - `register`
    - `login`
  - **Méthodes explicitement non testées**, conformément aux consignes :
    - Constructeurs
    - Getters / Setters
    - Méthodes privées

- **Tests manuels**
  - Appels manuels des endpoints REST (via un client HTTP / navigateur) pour vérifier :
    - Les codes HTTP (200, 201, 400, 404).
    - Le format et le contenu des réponses JSON.
    - Les messages d’erreur retournés au client.

### Outils utilisés

- **JUnit 5 (Jupiter)** : framework de tests unitaires.
- **Mockito** : mock de `UserService` et de `Context` Javalin pour isoler `UserController`.
- **Maven / IDE (IntelliJ)** : exécution automatisée de la suite de tests et détection rapide de régressions.
- **Client HTTP** (équivalent Postman) : tests manuels des routes.

---

## Oracle de tests

Pour chaque test, le tableau suivant précise :

- le **cas d’utilisation** couvert,
- le **jeu d’arguments / données d’entrée**,
- le **retour attendu** (status + JSON),
- les **effets de bord attendus** (appels au service, etc.).

| Nom du test                                 | Cas d’utilisation                              | Entrée / arguments principaux                                                                               | Retour attendu                                                                                            | Effets de bord attendus                                                                                                   |
| ------------------------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `getAllUsers_returnsMappedUserResponses`    | UC: Consulter la liste des utilisateurs        | `service.getAllUsers()` renvoie `[User(1,"A","a@x.com"), User(2,"B","b@x.com")]`                            | `ctx.json(List<UserResponse>)` avec 2 éléments, noms `"A"` puis `"B"`. Statut implicite 200 (non modifié) | `service.getAllUsers()` appelé une fois. Le contrôleur mappe bien les entités `User` en `UserResponse`.                   |
| `getUserById_found_returnsUserResponse`     | UC: Consulter un utilisateur (succès)          | `ctx.pathParam("id") = "5"` ; `service.getUserById(5)` renvoie `Optional.of(User(5,"Mohamed"))`             | `ctx.json(UserResponse)` représentant l’utilisateur id=5, nom `"Mohamed"`                                 | `service.getUserById(5)` appelé une fois ; **aucun** appel à `ctx.status(404)`.                                           |
| `getUserById_notFound_returns404`           | UC: Consulter un utilisateur (échec : inconnu) | `ctx.pathParam("id") = "5"` ; `service.getUserById(5)` renvoie `Optional.empty()`                           | `ctx.status(404)` puis `ctx.json(ResponseUtil.formatError("User not found"))`                             | `service.getUserById(5)` appelé une fois ; le contrôleur signale l’erreur avec un 404 + message standardisé.              |
| `register_missingName_returns400`           | UC: Inscription – validation du nom            | Body: `RegisterRequest(name="   ", email="test@x.com", password="secret123")`                               | `ctx.status(400)` ; `ctx.json(ResponseUtil.formatError("Name is required"))`                              | **Aucune** interaction avec `service` (`verifyNoInteractions(service)`) ; la requête est rejetée au niveau du contrôleur. |
| `register_invalidEmail_returns400`          | UC: Inscription – validation de l’email        | Body: `RegisterRequest(name="Mohamed", email="not-an-email", password="secret123")`                         | `ctx.status(400)` ; `ctx.json(ResponseUtil.formatError("Invalid email format"))`                          | Service non appelé ; le contrôleur valide le format de l’email et renvoie immédiatement une erreur 400.                   |
| `register_shortPassword_returns400`         | UC: Inscription – validation du mot de passe   | Body: `RegisterRequest(name="Mohamed", email="m@x.com", password="123")`                                    | `ctx.status(400)` ; `ctx.json(ResponseUtil.formatError("Password must be at least 6 chars"))`             | Service non appelé ; rejet immédiat si le mot de passe est trop court.                                                    |
| `register_valid_createsUser_returns201`     | UC: Inscription – scénario valide              | Body: `RegisterRequest("Mohamed","m@x.com","secret123")` ; `service.createUser(...)` renvoie `User(10,...)` | `ctx.status(201)` ; `ctx.json(UserResponse)` pour l’utilisateur créé                                      | `service.createUser("Mohamed","m@x.com","secret123")` appelé une fois ; l’utilisateur est créé côté service.              |
| `login_callsService_andReturnsUserResponse` | UC: Connexion d’un utilisateur                 | Body: `LoginRequest(email="m@x.com", password="secret123")` ; `service.login(...)` renvoie `User(3,...)`    | `ctx.json(UserResponse)` pour l’utilisateur connecté (id=3, email `"m@x.com"`)                            | `service.login("m@x.com","secret123")` appelé une fois ; le contrôleur délègue complètement l’authentification.           |

Ces oracles permettent de vérifier, pour chaque test :

- la correspondance entre les **cas d’utilisation** du système et les scénarios de test,
- la cohérence des **retours HTTP** et des **effets de bord** (appels de service, absence d’appel en cas d’erreur de validation, etc.).

---

## Résultats des tests

### Résumé qualitatif

- Tous les tests de la classe `UserControllerTest` passent avec succès.
- Les scénarios suivants sont couverts :
  - Lecture de tous les utilisateurs.
  - Lecture d’un utilisateur existant / inexistant.
  - Inscription avec données invalides (nom vide, email invalide, mot de passe trop court).
  - Inscription valide.
  - Connexion valide.
- Le comportement observé est conforme aux **oracles de tests** définis ci-dessus :
  - Codes de retour appropriés (`200` implicite, `201`, `400`, `404`).
  - Messages d’erreur clairs et cohérents via `ResponseUtil.formatError(...)`.
  - Délégation correcte de la logique métier à `UserService`.

### Résumé quantitatif

- **Nombre de tests unitaires sur `UserController`** : 7.
- **Taux de réussite** : 100 %.
- **Couverture** : les principales branches de contrôle du contrôleur sont couvertes (succès / échecs, validations, not found).
- **Temps d’exécution** : négligeable (quelques millisecondes), adapté à une exécution fréquente (ex. à chaque build).

---

## Évaluation du système

- **Qualité globale perçue**

  - Le contrôleur `UserController` se comporte de manière prévisible et robuste face aux entrées invalides.
  - La présence de tests unitaires ciblés réduit fortement le risque de régression lors de modifications futures.

- **Facilité d’utilisation**

  - Les endpoints REST sont simples (listage, consultation par id, inscription, connexion).
  - Les messages d’erreur explicites facilitent le débogage côté client.

- **Performance en conditions réelles**

  - Pour un environnement académique et une base JSON, les performances sont largement satisfaisantes.
  - Le contrôleur est léger et ne constitue pas un goulot d’étranglement.

- **Maintenabilité**
  - Les tests unitaires, combinés à l’utilisation de Mockito, permettent de tester le contrôleur **en isolation**.
  - Les oracles de tests documentés servent de référence pour valider toute évolution future du comportement de l’API.

En résumé, l’ensemble des tests unitaires écrits pour `UserController`, conforme aux consignes (JUnit, méthodes publiques, oracles documentés), offre un bon niveau de confiance dans la qualité de cette partie du système.
