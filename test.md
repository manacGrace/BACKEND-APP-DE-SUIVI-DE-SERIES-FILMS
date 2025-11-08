# Données de Tests pour l'API de Suivi de Séries et Films

## Informations Générales

- **URL Base v1**: `http://localhost:3000/api/v1`
- **URL Base v2**: `http://localhost:3000/api/v2`
- **Format d'ID**: Les IDs MongoDB utilisent un format personnalisé (ex: "id0001", "id0002")

---

## 🔐 Authentification (v2)

### Token JWT
Après authentification, utiliser le token dans le header:
```
Authorization: Bearer <token>
```

---

## 📋 API V1 (DEPRECATED - Utiliser v2)

### 1. Media Routes

#### GET `/api/v1/medias`
**Description**: Récupérer tous les médias avec filtres optionnels et pagination

**Requêtes de test**:
```json
// Test 1: Récupérer tous les médias
GET http://localhost:3000/api/v1/medias

// Test 2: Filtrer par type
GET http://localhost:3000/api/v1/medias?type=Film
GET http://localhost:3000/api/v1/medias?type=Serie

// Test 3: Filtrer par genre
GET http://localhost:3000/api/v1/medias?genre=Action

// Test 4: Filtrer par année
GET http://localhost:3000/api/v1/medias?year=2020

// Test 5: Filtres combinés
GET http://localhost:3000/api/v1/medias?type=Film&genre=Action&year=2020

// Test 6: Pagination - page 1 (par défaut)
GET http://localhost:3000/api/v1/medias?page=1&limit=20

// Test 7: Pagination - page 2
GET http://localhost:3000/api/v1/medias?page=2&limit=20

// Test 8: Pagination avec filtres
GET http://localhost:3000/api/v1/medias?type=Film&genre=Action&page=1&limit=10

// Test 9: Limite personnalisée
GET http://localhost:3000/api/v1/medias?limit=50

// Test 10: Pagination avec tous les filtres
GET http://localhost:3000/api/v1/medias?type=Film&genre=Action&year=2020&page=2&limit=15
```

#### GET `/api/v1/medias/:id`
**Description**: Récupérer un média par ID

**Requêtes de test**:
```json
// Test 1: ID valide
GET http://localhost:3000/api/v1/medias/5001

// Test 2: ID inexistant
GET http://localhost:3000/api/v1/medias/9999
```

#### POST `/api/v1/medias`
**Description**: Créer un nouveau média

**Body de test**:
```json
// Test 1: Film
{
  "title": "Inception",
  "type": "Film",
  "genre": "Science-Fiction",
  "year": 2010,
  "rating": 8.8,
  "platform": "Netflix",
  "duration": 148,
  "status": "Finished"
}

// Test 2: Série
{
  "title": "Breaking Bad",
  "type": "Serie",
  "genre": "Drama",
  "year": 2008,
  "rating": 9.5,
  "platform": "Netflix",
  "status": "Finished"
}

// Test 3: Données invalides (rating > 10)
{
  "title": "Test Film",
  "type": "Film",
  "genre": "Action",
  "year": 2020,
  "rating": 15,
  "platform": "Netflix",
  "duration": 120,
  "status": "Finished"
}
```

#### PUT `/api/v1/medias/:id` (Admin seulement)
**Description**: Mettre à jour un média

**Body de test**:
```json
// Test 1: Mise à jour complète
{
  "title": "Inception (Director's Cut)",
  "genre": "Sci-Fi Thriller",
  "year": 2010,
  "rating": 9.0,
  "platform": "Amazon Prime",
  "duration": 160,
  "status": "Finished"
}

// Test 2: Mise à jour partielle
{
  "rating": 9.2
}
```

#### DELETE `/api/v1/medias/:id` (Admin seulement)
**Description**: Supprimer un média

**Requêtes de test**:
```json
// Test 1: Suppression valide
DELETE /api/v1/medias/5001

// Test 2: ID inexistant
DELETE /api/v1/medias/9999
```

---

### 2. Film Routes

#### POST `/api/v1/films` (Admin seulement)
**Description**: Créer un nouveau film

**Body de test**:
```json
// Test 1: Film complet
{
  "title": "The Matrix",
  "genre": "Science-Fiction",
  "year": 1999,
  "rating": 8.7,
  "duration": 136
}

// Test 2: Film avec durée minimale
{
  "title": "Short Film",
  "genre": "Drama",
  "year": 2020,
  "rating": 7.5,
  "duration": 15
}

// Test 3: Film avec données invalides (durée négative)
{
  "title": "Invalid Film",
  "genre": "Action",
  "year": 2020,
  "rating": 8.0,
  "duration": -50
}
```

---

### 3. Serie Routes

#### POST `/api/v1/series` (Admin seulement)
**Description**: Créer une nouvelle série

**Body de test**:
```json
// Test 1: Série complète
{
  "title": "A Song of Ice and Fire",
  "genre": "Fantasy",
  "year": 2011,
  "rating": 9.3,
  "status": "Finished"
}

// Test 2: Série en cours
{
  "title": "The Conquest of Aegon",
  "genre": "Horror",
  "year": 2016,
  "rating": 8.7,
  "status": "Ongoing"
}

// Test 3: Série en pause
{
  "title": "House of the Wolf",
  "genre": "Mystery",
  "year": 2010,
  "rating": 9.1,
  "status": "On_hold"
}
```

#### GET `/api/v1/series/:id/episodes`
**Description**: Récupérer les épisodes d'une série

**Requêtes de test**:
```json
// Test 1: ID valide
GET http://localhost:3000/api/v1/series/5002/episodes

// Test 2: ID inexistant
GET http://localhost:3000/api/v1/series/9999/episodes
```

---

### 4. Season Routes

#### POST `/api/v1/seasons`
**Description**: Créer une nouvelle saison

**Body de test**:
```json
// Test 1: Saison complète
{
  "seriesId": 5002,
  "seasonNumber": 1,
  "episodes": [
    {
      "title": "Pilot",
      "episodeNumber": 1,
      "duration": 47,
      "watched": false
    },
    {
      "title": "Cat's in the Bag...",
      "episodeNumber": 2,
      "duration": 48,
      "watched": false
    }
  ]
}

// Test 2: Saison avec un seul épisode
{
  "seriesId": 5002,
  "seasonNumber": 2,
  "episodes": [
    {
      "title": "Seven Thirty-Seven",
      "episodeNumber": 1,
      "duration": 47,
      "watched": true
    }
  ]
}
```

---

### 5. Episode Routes

#### POST `/api/v1/episodes`
**Description**: Ajouter un épisode

**Body de test**:
```json
// Test 1: Épisode complet
{
  "seriesId": 5002,
  "seasonNumber": 1,
  "title": "Pilot",
  "episodeNumber": 1,
  "duration": 58,
  "watched": false
}

// Test 2: Épisode déjà visionné
{
  "seriesId": 5002,
  "seasonNumber": 1,
  "title": "Full Measure",
  "episodeNumber": 13,
  "duration": 47,
  "watched": true
}
```

#### PATCH `/api/v1/episodes/:id`
**Description**: Mettre à jour le statut de visionnage d'un épisode

**Body de test**:
```json
// Test 1: Marquer comme vu
{
  "watched": true
}

// Test 2: Marquer comme non vu
{
  "watched": false
}
```

---

### 6. User Routes

#### GET `/api/v1/users/:id/medias`
**Description**: Récupérer les médias favoris d'un utilisateur

**Requêtes de test**:
```json
// Test 1: Utilisateur avec favoris
GET http://localhost:3000/api/v1/users/1/medias

// Test 2: Utilisateur sans favoris
GET http://localhost:3000/api/v1/users/2/medias

// Test 3: Utilisateur inexistant
GET http://localhost:3000/api/v1/users/9999/medias
```

---

### 7. Logging Routes

#### GET `/api/v1/logs`
**Description**: Récupérer les logs de l'application

**Requêtes de test**:
```json
// Test 1: Récupérer les logs
GET http://localhost:3000/api/v1/logs

// Réponse attendue:
{
  "lastAction": "GET http://localhost:3000/api/v1/medias",
  "timestamp": "2023-09-01T10:00:00.000Z"
}
```

---

## 🚀 API V2 (Moderne avec MongoDB)

### 1. Auth Routes

#### POST `/api/v2/auth/register`
**Description**: Enregistrer un nouvel utilisateur

**Body de test**:
```json
// Test 1: Inscription utilisateur standard
{
  "email": "john.doe@example.com",
  "username": "johndoe",
  "password": "SecurePass123!"
}

// Test 2: Inscription avec username long
{
  "email": "marie.martin@example.com",
  "username": "marie_martin_2023",
  "password": "MySecureP@ssw0rd"
}

// Test 3: Données invalides - email invalide
{
  "email": "invalid-email",
  "username": "testuser",
  "password": "password123"
}

// Test 4: Données invalides - username trop court
{
  "email": "test@example.com",
  "username": "ab",
  "password": "password123"
}

// Test 5: Données invalides - password trop court
{
  "email": "test@example.com",
  "username": "testuser",
  "password": "short"
}

// Test 6: Email déjà existant (devrait retourner 409)
{
  "email": "john.doe@example.com",
  "username": "anotheruser",
  "password": "Password123!"
}
```

#### POST `/api/v2/auth/login`
**Description**: Connexion utilisateur

**Body de test**:
```json
// Test 1: Connexion valide
{
  "email": "john.doe@example.com",
  "password": "SecurePass123!"
}

// Test 2: Mot de passe incorrect
{
  "email": "john.doe@example.com",
  "password": "WrongPassword"
}

// Test 3: Email inexistant
{
  "email": "nonexistent@example.com",
  "password": "SomePassword123"
}

// Test 4: Format invalide - email manquant
{
  "password": "SecurePass123!"
}
```

---

### 2. User Routes

#### GET `/api/v2/users/me` (JWT requis)
**Description**: Récupérer le profil de l'utilisateur connecté

**Headers**:
```
Authorization: Bearer <token>
```

**Requêtes de test**:
```json
// Test 1: Token valide
GET http://localhost:3000/api/v2/users/me
Headers: { "Authorization": "Bearer <token>" }

// Test 2: Token manquant
GET http://localhost:3000/api/v2/users/me

// Test 3: Token invalide
GET http://localhost:3000/api/v2/users/me
Headers: { "Authorization": "Bearer invalid_token" }
```

#### PATCH `/api/v2/users/me` (JWT requis)
**Description**: Mettre à jour le profil utilisateur

**Body de test**:
```json
// Test 1: Mise à jour du username
{
  "username": "new_john_doe"
}

// Test 2: Mise à jour des favoris
{
  "favorites": ["id0001", "id0002", "id0003"]
}

// Test 3: Mise à jour complète
{
  "username": "john_updated",
  "favorites": ["id0001", "id0005"]
}

// Test 4: Favoris vide
{
  "favorites": []
}

// Test 5: Données invalides - username trop court
{
  "username": "ab"
}
```

#### GET `/api/v2/users/:id` (Admin seulement)
**Description**: Récupérer un utilisateur par ID

**Requêtes de test**:
```json
// Test 1: ID valide avec token admin
GET http://localhost:3000/api/v2/users/id0001
Headers: { "Authorization": "Bearer <admin_token>" }

// Test 2: Accès non-admin (devrait retourner 403)
GET http://localhost:3000/api/v2/users/id0001
Headers: { "Authorization": "Bearer <user_token>" }

// Test 3: ID inexistant
GET http://localhost:3000/api/v2/users/id9999
Headers: { "Authorization": "Bearer <admin_token>" }
```

#### PATCH `/api/v2/users/:id` (Admin seulement)
**Description**: Mettre à jour un utilisateur (admin)

**Body de test**:
```json
// Test 1: Changer le rôle en admin
{
  "role": "admin",
  "username": "promoted_user"
}

// Test 2: Mise à jour des favoris
{
  "favorites": ["id0001", "id0002"]
}

// Test 3: Mise à jour complète
{
  "username": "updated_user",
  "role": "admin",
  "favorites": ["id0001", "id0003", "id0005"]
}
```

#### DELETE `/api/v2/users/:id` (Admin seulement)
**Description**: Supprimer un utilisateur

**Requêtes de test**:
```json
// Test 1: Suppression valide
DELETE /api/v2/users/id0005
Headers: { "Authorization": "Bearer <admin_token>" }

// Test 2: Suppression utilisateur inexistant
DELETE /api/v2/users/id9999
Headers: { "Authorization": "Bearer <admin_token>" }
```

---

### 3. Movie Routes

#### GET `/api/v2/movies`
**Description**: Récupérer les films avec pagination et filtres

**Requêtes de test**:
```json
// Test 1: Récupérer tous les films (première page)
GET http://localhost:3000/api/v2/movies

// Test 2: Pagination - page 2
GET http://localhost:3000/api/v2/movies?page=2&limit=20

// Test 3: Filtrer par titre
GET http://localhost:3000/api/v2/movies?title=Avengers

// Test 4: Filtrer par genre
GET http://localhost:3000/api/v2/movies?genre=Action

// Test 5: Filtrer par année (plage)
GET http://localhost:3000/api/v2/movies?minYear=2020&maxYear=2024

// Test 6: Filtrer par durée (plage)
GET http://localhost:3000/api/v2/movies?minDuration=90&maxDuration=150

// Test 7: Filtres combinés
GET http://localhost:3000/api/v2/movies?genre=Action&minYear=2010&maxYear=2020&minDuration=120&page=1&limit=10

// Test 8: Limite maximale (200)
GET http://localhost:3000/api/v2/movies?limit=200

// Test 9: Limite dépassée (devrait être limité à 200)
GET http://localhost:3000/api/v2/movies?limit=300
```

#### POST `/api/v2/movies` (Admin seulement)
**Description**: Créer un nouveau film

**Body de test**:
```json
// Test 1: Film complet
{
  "_id": "mov001",
  "title": "The Avengers",
  "genres": ["Action", "Adventure", "Sci-Fi"],
  "synopsis": "Earth's mightiest heroes must come together and learn to fight as a team if they are to stop the mischievous Loki and his alien army from enslaving humanity.",
  "releaseDate": "2012-05-04",
  "durationMin": 143
}

// Test 2: Film minimal (champs requis uniquement)
{
  "_id": "mov002",
  "title": "Inception",
  "genres": ["Science Fiction", "Thriller"],
  "durationMin": 148
}

// Test 3: Film avec plusieurs genres
{
  "_id": "mov003",
  "title": "The Dark Knight",
  "genres": ["Action", "Crime", "Drama", "Thriller"],
  "synopsis": "Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice.",
  "releaseDate": "2008-07-18",
  "durationMin": 152
}

// Test 4: Film court métrage
{
  "_id": "mov004",
  "title": "Short Film Example",
  "genres": ["Drama"],
  "durationMin": 15
}

// Test 5: Film très long
{
  "_id": "mov005",
  "title": "Extended Edition",
  "genres": ["Fantasy"],
  "durationMin": 240
}

// Test 6: Données invalides - titre manquant
{
  "_id": "mov006",
  "genres": ["Action"],
  "durationMin": 120
}

// Test 7: Données invalides - durée hors limites
{
  "_id": "mov007",
  "title": "Invalid Duration",
  "genres": ["Action"],
  "durationMin": 700
}

// Test 8: Données invalides - titre trop long (>200 caractères)
{
  "_id": "mov008",
  "title": "A very long title that exceeds the maximum character limit of two hundred characters which should be rejected by the validation middleware and return an error to the user making the request",
  "genres": ["Action"],
  "durationMin": 120
}
```

#### GET `/api/v2/movies/:id`
**Description**: Récupérer un film par ID

**Requêtes de test**:
```json
// Test 1: ID valide
GET http://localhost:3000/api/v2/movies/mov001

// Test 2: ID inexistant
GET http://localhost:3000/api/v2/movies/mov999
```

#### PATCH `/api/v2/movies/:id` (Admin seulement)
**Description**: Mettre à jour un film

**Body de test**:
```json
// Test 1: Mise à jour partielle - titre
{
  "title": "The Avengers (Director's Cut)"
}

// Test 2: Mise à jour partielle - genres
{
  "genres": ["Action", "Adventure", "Sci-Fi", "Fantasy"]
}

// Test 3: Mise à jour partielle - durée
{
  "durationMin": 150
}

// Test 4: Mise à jour complète
{
  "title": "The Avengers - Extended",
  "genres": ["Action", "Adventure", "Sci-Fi"],
  "synopsis": "Updated synopsis with more details...",
  "durationMin": 155
}
```

#### DELETE `/api/v2/movies/:id` (Admin seulement)
**Description**: Supprimer un film

**Requêtes de test**:
```json
// Test 1: Suppression valide
DELETE /api/v2/movies/mov001
Headers: { "Authorization": "Bearer <admin_token>" }

// Test 2: Suppression film inexistant
DELETE /api/v2/movies/mov999
Headers: { "Authorization": "Bearer <admin_token>" }
```

---

### 4. Series Routes

#### GET `/api/v2/series`
**Description**: Récupérer les séries avec pagination et filtres

**Requêtes de test**:
```json
// Test 1: Récupérer toutes les séries
GET http://localhost:3000/api/v2/series

// Test 2: Filtrer par titre
GET http://localhost:3000/api/v2/series?title=Breaking

// Test 3: Filtrer par genre
GET http://localhost:3000/api/v2/series?genre=Drama

// Test 4: Filtrer par statut
GET http://localhost:3000/api/v2/series?status=ended
GET http://localhost:3000/api/v2/series?status=ongoing

// Test 5: Filtres combinés avec pagination
GET http://localhost:3000/api/v2/series?genre=Crime&status=ended&page=1&limit=10
```

#### POST `/api/v2/series` (Admin seulement)
**Description**: Créer une nouvelle série

**Body de test**:
```json
// Test 1: Série complète - terminée
{
  "_id": "ser001",
  "title": "Breaking Bad",
  "genres": ["Drama", "Crime", "Thriller"],
  "status": "ended"
}

// Test 2: Série complète - en cours
{
  "_id": "ser002",
  "title": "Stranger Things",
  "genres": ["Horror", "Sci-Fi", "Drama"],
  "status": "ongoing"
}

// Test 3: Série avec un seul genre
{
  "_id": "ser003",
  "title": "The Office",
  "genres": ["Comedy"],
  "status": "ended"
}

// Test 4: Série avec plusieurs genres
{
  "_id": "ser004",
  "title": "Game of Thrones",
  "genres": ["Fantasy", "Drama", "Action", "Adventure"],
  "status": "ended"
}

// Test 5: Données invalides - statut invalide
{
  "_id": "ser005",
  "title": "Invalid Series",
  "genres": ["Drama"],
  "status": "cancelled"
}

// Test 6: Données invalides - titre manquant
{
  "_id": "ser006",
  "genres": ["Action"],
  "status": "ongoing"
}
```

#### GET `/api/v2/series/:id`
**Description**: Récupérer une série par ID

**Requêtes de test**:
```json
// Test 1: ID valide
GET http://localhost:3000/api/v2/series/ser001

// Test 2: ID inexistant
GET http://localhost:3000/api/v2/series/ser999
```

#### PATCH `/api/v2/series/:id` (Admin seulement)
**Description**: Mettre à jour une série

**Body de test**:
```json
// Test 1: Mise à jour du statut
{
  "status": "ended"
}

// Test 2: Mise à jour des genres
{
  "genres": ["Drama", "Crime", "Thriller", "Dark Comedy"]
}

// Test 3: Mise à jour complète
{
  "title": "Breaking Bad (Complete Edition)",
  "genres": ["Drama", "Crime", "Thriller"],
  "status": "ended"
}
```

#### DELETE `/api/v2/series/:id` (Admin seulement)
**Description**: Supprimer une série

**Requêtes de test**:
```json
// Test 1: Suppression valide
DELETE /api/v2/series/ser001
Headers: { "Authorization": "Bearer <admin_token>" }

// Test 2: Suppression série inexistante
DELETE /api/v2/series/ser999
Headers: { "Authorization": "Bearer <admin_token>" }
```

---

### 5. Season Routes

#### GET `/api/v2/series/:seriesId/seasons`
**Description**: Récupérer les saisons d'une série

**Requêtes de test**:
```json
// Test 1: Récupérer toutes les saisons
GET http://localhost:3000/api/v2/series/ser001/seasons

// Test 2: Avec pagination
GET http://localhost:3000/api/v2/series/ser001/seasons?page=1&limit=10

// Test 3: Série inexistante
GET http://localhost:3000/api/v2/series/ser999/seasons
```

#### POST `/api/v2/series/:seriesId/seasons` (Admin seulement)
**Description**: Créer une nouvelle saison

**Body de test**:
```json
// Test 1: Saison complète
{
  "_id": "sea001",
  "seasonNo": 1,
  "episodes": 10
}

// Test 2: Saison avec plus d'épisodes
{
  "_id": "sea002",
  "seasonNo": 2,
  "episodes": 13
}

// Test 3: Saison spéciale
{
  "_id": "sea003",
  "seasonNo": 0,
  "episodes": 3
}

// Test 4: Données invalides - numéro de saison invalide
{
  "_id": "sea004",
  "seasonNo": -1,
  "episodes": 10
}

// Test 5: Données invalides - nombre d'épisodes négatif
{
  "_id": "sea005",
  "seasonNo": 1,
  "episodes": -5
}

// Test 6: Saison déjà existante (devrait retourner 409)
{
  "_id": "sea001",
  "seasonNo": 1,
  "episodes": 10
}
```

---

### 6. Episode Routes

#### GET `/api/v2/series/:seriesId/seasons/:seasonId/episodes`
**Description**: Récupérer les épisodes d'une saison

**Requêtes de test**:
```json
// Test 1: Récupérer tous les épisodes
GET http://localhost:3000/api/v2/series/ser001/seasons/sea001/episodes

// Test 2: Avec pagination
GET http://localhost:3000/api/v2/series/ser001/seasons/sea001/episodes?page=1&limit=10

// Test 3: Filtrer par durée minimale
GET http://localhost:3000/api/v2/series/ser001/seasons/sea001/episodes?minDuration=40

// Test 4: Filtrer par durée maximale
GET http://localhost:3000/api/v2/series/ser001/seasons/sea001/episodes?maxDuration=60

// Test 5: Plage de durée
GET http://localhost:3000/api/v2/series/ser001/seasons/sea001/episodes?minDuration=45&maxDuration=55

// Test 6: Saison inexistante
GET http://localhost:3000/api/v2/series/ser001/seasons/sea999/episodes
```

#### POST `/api/v2/series/:seriesId/seasons/:seasonId/episodes` (Admin seulement)
**Description**: Créer un nouvel épisode

**Body de test**:
```json
// Test 1: Épisode complet
{
  "_id": "ep001",
  "epNo": 1,
  "title": "Pilot",
  "durationMin": 58
}

// Test 2: Épisode court
{
  "_id": "ep002",
  "epNo": 2,
  "title": "Cat's in the Bag...",
  "durationMin": 47
}

// Test 3: Épisode long
{
  "_id": "ep003",
  "epNo": 3,
  "title": "Extended Episode",
  "durationMin": 90
}

// Test 4: Données invalides - numéro d'épisode invalide
{
  "_id": "ep004",
  "epNo": 0,
  "title": "Invalid Episode",
  "durationMin": 45
}

// Test 5: Données invalides - durée hors limites
{
  "_id": "ep005",
  "epNo": 4,
  "title": "Too Long Episode",
  "durationMin": 350
}

// Test 6: Données invalides - titre manquant
{
  "_id": "ep006",
  "epNo": 5,
  "durationMin": 45
}

// Test 7: Épisode déjà existant (devrait retourner 409)
{
  "_id": "ep001",
  "epNo": 1,
  "title": "Duplicate Episode",
  "durationMin": 58
}
```

#### GET `/api/v2/series/:seriesId/seasons/:seasonId/episodes/:episodeId`
**Description**: Récupérer un épisode par ID

**Requêtes de test**:
```json
// Test 1: ID valide
GET http://localhost:3000/api/v2/series/ser001/seasons/sea001/episodes/ep001

// Test 2: ID inexistant
GET http://localhost:3000/api/v2/series/ser001/seasons/sea001/episodes/ep999
```

---

### 7. Rating Routes

#### GET `/api/v2/ratings`
**Description**: Récupérer les notes avec pagination et filtres (nécessite authentification selon Swagger, mais semble public dans la route)

**Requêtes de test**:
```json
// Test 1: Récupérer toutes les notes
GET http://localhost:3000/api/v2/ratings

// Test 2: Filtrer par type de cible
GET http://localhost:3000/api/v2/ratings?target=movie
GET http://localhost:3000/api/v2/ratings?target=episode

// Test 3: Filtrer par ID de cible
GET http://localhost:3000/api/v2/ratings?targetId=mov001

// Test 4: Filtrer par utilisateur
GET http://localhost:3000/api/v2/ratings?userId=id0001

// Test 5: Filtrer par score minimum
GET http://localhost:3000/api/v2/ratings?minScore=7.0

// Test 6: Filtrer par score maximum
GET http://localhost:3000/api/v2/ratings?maxScore=9.0

// Test 7: Plage de scores
GET http://localhost:3000/api/v2/ratings?minScore=7.5&maxScore=8.5

// Test 8: Filtres combinés
GET http://localhost:3000/api/v2/ratings?target=movie&targetId=mov001&minScore=8.0&page=1&limit=10

// Test 9: Avec pagination
GET http://localhost:3000/api/v2/ratings?page=2&limit=20
```

#### POST `/api/v2/ratings` (JWT requis)
**Description**: Créer une nouvelle note

**Body de test**:
```json
// Test 1: Note pour un film
{
  "target": "movie",
  "targetId": "mov001",
  "score": 8.5,
  "review": "Great movie with excellent action sequences and character development!"
}

// Test 2: Note pour un épisode
{
  "target": "episode",
  "targetId": "ep001",
  "score": 9.0,
  "review": "Amazing pilot episode that sets up the entire series perfectly."
}

// Test 3: Note sans commentaire
{
  "target": "movie",
  "targetId": "mov002",
  "score": 7.5
}

// Test 4: Note maximale
{
  "target": "movie",
  "targetId": "mov003",
  "score": 10,
  "review": "Perfect masterpiece!"
}

// Test 5: Note minimale
{
  "target": "movie",
  "targetId": "mov004",
  "score": 0,
  "review": "Terrible movie, waste of time."
}

// Test 6: Note moyenne
{
  "target": "episode",
  "targetId": "ep002",
  "score": 5.0,
  "review": "Average episode, nothing special."
}

// Test 7: Commentaire long
{
  "target": "movie",
  "targetId": "mov005",
  "score": 9.5,
  "review": "This is a very detailed review that explains all the aspects of the movie including plot, character development, cinematography, music, and overall impact on the viewer. The film manages to balance multiple storylines while maintaining coherence and emotional depth..."
}

// Test 8: Données invalides - score hors limites
{
  "target": "movie",
  "targetId": "mov001",
  "score": 15
}

// Test 9: Données invalides - type de cible invalide
{
  "target": "series",
  "targetId": "ser001",
  "score": 8.0
}

// Test 10: Données invalides - champs requis manquants
{
  "target": "movie",
  "score": 8.0
}
```

#### GET `/api/v2/ratings/:id`
**Description**: Récupérer une note par ID

**Requêtes de test**:
```json
// Test 1: ID valide
GET http://localhost:3000/api/v2/ratings/rat001

// Test 2: ID inexistant
GET http://localhost:3000/api/v2/ratings/rat999
```

#### PATCH `/api/v2/ratings/:id` (JWT requis - propriétaire seulement)
**Description**: Mettre à jour une note

**Body de test**:
```json
// Test 1: Mise à jour du score
{
  "score": 9.0
}

// Test 2: Mise à jour du commentaire
{
  "review": "Updated review: After rewatching, I appreciate it even more!"
}

// Test 3: Mise à jour complète
{
  "score": 9.5,
  "review": "Updated review: Changed my mind, this is even better than I thought!"
}

// Test 4: Note d'un autre utilisateur (devrait retourner 403)
PATCH /api/v2/ratings/rat002
Headers: { "Authorization": "Bearer <other_user_token>" }
```

#### DELETE `/api/v2/ratings/:id` (JWT requis - propriétaire seulement)
**Description**: Supprimer une note

**Requêtes de test**:
```json
// Test 1: Suppression valide
DELETE /api/v2/ratings/rat001
Headers: { "Authorization": "Bearer <owner_token>" }

// Test 2: Suppression note d'un autre utilisateur (devrait retourner 403)
DELETE /api/v2/ratings/rat002
Headers: { "Authorization": "Bearer <other_user_token>" }

// Test 3: Note inexistante
DELETE /api/v2/ratings/rat999
Headers: { "Authorization": "Bearer <token>" }
```

#### GET `/api/v2/ratings/avg/movie/:movieId`
**Description**: Récupérer la note moyenne d'un film

**Requêtes de test**:
```json
// Test 1: Film avec plusieurs notes
GET http://localhost:3000/api/v2/ratings/avg/movie/mov001

// Test 2: Film sans notes
GET http://localhost:3000/api/v2/ratings/avg/movie/mov999

// Test 3: Film inexistant
GET http://localhost:3000/api/v2/ratings/avg/movie/inexistent
```

#### GET `/api/v2/ratings/avg/series/:seriesId`
**Description**: Récupérer la note moyenne d'une série

**Requêtes de test**:
```json
// Test 1: Série avec plusieurs notes d'épisodes
GET http://localhost:3000/api/v2/ratings/avg/series/ser001

// Test 2: Série sans notes
GET http://localhost:3000/api/v2/ratings/avg/series/ser999

// Test 3: Série inexistante
GET http://localhost:3000/api/v2/ratings/avg/series/inexistent
```

---

## 📊 Scénarios de Test Complets

### Scénario 1: Workflow Complet - Création et Notation d'un Film
```json
// 1. Créer un compte utilisateur
POST /api/v2/auth/register
{
  "email": "movielover@example.com",
  "username": "movielover",
  "password": "SecurePass123!"
}
→ Sauvegarder le token reçu

// 2. Créer un film (admin requis)
POST /api/v2/movies
Headers: { "Authorization": "Bearer <admin_token>" }
{
  "_id": "mov_test001",
  "title": "Test Movie",
  "genres": ["Drama", "Thriller"],
  "synopsis": "A test movie for demonstration",
  "releaseDate": "2023-01-15",
  "durationMin": 120
}

// 3. Consulter le film créé
GET http://localhost:3000/api/v2/movies/mov_test001

// 4. Noter le film
POST /api/v2/ratings
Headers: { "Authorization": "Bearer <user_token>" }
{
  "target": "movie",
  "targetId": "mov_test001",
  "score": 8.5,
  "review": "Great test movie!"
}

// 5. Consulter la moyenne des notes
GET http://localhost:3000/api/v2/ratings/avg/movie/mov_test001
```

### Scénario 2: Workflow Complet - Création d'une Série avec Saisons et Épisodes
```json
// 1. Créer une série (admin)
POST /api/v2/series
Headers: { "Authorization": "Bearer <admin_token>" }
{
  "_id": "ser_test001",
  "title": "Test Series",
  "genres": ["Drama", "Mystery"],
  "status": "ongoing"
}

// 2. Créer la saison 1
POST /api/v2/series/ser_test001/seasons
Headers: { "Authorization": "Bearer <admin_token>" }
{
  "_id": "sea_test001",
  "seasonNo": 1,
  "episodes": 10
}

// 3. Créer un épisode
POST /api/v2/series/ser_test001/seasons/sea_test001/episodes
Headers: { "Authorization": "Bearer <admin_token>" }
{
  "_id": "ep_test001",
  "epNo": 1,
  "title": "Pilot",
  "durationMin": 45
}

// 4. Consulter tous les épisodes de la saison
GET http://localhost:3000/api/v2/series/ser_test001/seasons/sea_test001/episodes

// 5. Noter l'épisode
POST /api/v2/ratings
Headers: { "Authorization": "Bearer <user_token>" }
{
  "target": "episode",
  "targetId": "ep_test001",
  "score": 9.0,
  "review": "Excellent pilot!"
}

// 6. Consulter la moyenne des notes de la série
GET http://localhost:3000/api/v2/ratings/avg/series/ser_test001
```

### Scénario 3: Gestion des Favoris Utilisateur
```json
// 1. Se connecter
POST /api/v2/auth/login
{
  "email": "movielover@example.com",
  "password": "SecurePass123!"
}
→ Sauvegarder le token

// 2. Consulter son profil
GET http://localhost:3000/api/v2/users/me
Headers: { "Authorization": "Bearer <token>" }

// 3. Ajouter des films aux favoris
PATCH /api/v2/users/me
Headers: { "Authorization": "Bearer <token>" }
{
  "favorites": ["mov001", "mov002", "mov003"]
}

// 4. Vérifier les favoris mis à jour
GET http://localhost:3000/api/v2/users/me
Headers: { "Authorization": "Bearer <token>" }
```

---

## 🔍 Tests de Cas Limites et Erreurs

### Tests d'Erreurs Courantes

#### 400 - Bad Request
- Données de validation manquantes
- Types de données incorrects
- Valeurs hors limites (score > 10, durée négative, etc.)
- Format d'email invalide
- Username trop court (< 3 caractères)

#### 401 - Unauthorized
- Requête sans token JWT
- Token JWT expiré
- Token JWT invalide

#### 403 - Forbidden
- Utilisateur standard tentant d'accéder à une route admin
- Utilisateur tentant de modifier/supprimer une note qui ne lui appartient pas

#### 404 - Not Found
- Ressource inexistante (ID invalide)
- Film/Série/Épisode/Note non trouvé

#### 409 - Conflict
- Email déjà utilisé lors de l'inscription
- Saison/Épisode déjà existant pour une série

#### 500 - Server Error
- Erreurs de base de données
- Erreurs de validation interne

---

## 📝 Notes Importantes

1. **IDs MongoDB**: Les IDs utilisent un format personnalisé (ex: "mov001", "ser001", "id0001")
2. **Dates**: Format ISO 8601 (YYYY-MM-DD ou YYYY-MM-DDTHH:mm:ss.sssZ)
3. **Pagination**: Par défaut, page=1 et limit=20, maximum limit=200
4. **Validation**: Les champs requis doivent toujours être présents
5. **Authentification**: Les routes admin nécessitent un token JWT avec le rôle "admin"
6. **Permissions**: Les utilisateurs ne peuvent modifier/supprimer que leurs propres notes

---

## 🎯 Priorités de Test

### Priorité Haute
- Authentification (register/login)
- CRUD Films
- CRUD Séries
- Création de notes

### Priorité Moyenne
- Pagination et filtres
- Gestion des saisons et épisodes
- Calcul des moyennes de notes
- Gestion des favoris

### Priorité Basse
- Routes v1 (dépréciées)
- Logging
- Tests de cas limites complexes

---

*Document généré pour faciliter les tests de l'API de suivi de séries et films*
