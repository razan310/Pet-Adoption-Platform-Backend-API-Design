# 📘 Pet Adoption Platform – Backend API Design

## 🧠 Project Goal
Design the backend structure and required APIs for a platform that helps users:

1. Browse adoptable pets in their city.
2. Find emergency veterinary clinics currently open.
3. Read pet care articles by pet type.

---

## 📁 Suggested Project Structure
```
pet-adoption-api/
│
├── src/
│   ├── controllers/            # Request handlers (logic)
│   │   ├── pet.controller.js       # Handles pet-related logic
│   │   ├── clinic.controller.js    # Handles vet clinic logic
│   │   └── article.controller.js   # Handles article-related logic
│   │
│   ├── routes/                 # API route definitions
│   │   ├── pet.routes.js          # Routes for /api/pets
│   │   ├── clinic.routes.js       # Routes for /api/clinics
│   │   └── article.routes.js      # Routes for /api/articles
│   │
│   ├── services/              # Business logic abstraction
│   │   ├── pet.service.js
│   │   ├── clinic.service.js
│   │   └── article.service.js
│   │
│   ├── models/                # Mongoose schemas
│   │   ├── Pet.js
│   │   ├── Clinic.js
│   │   └── Article.js
│   │
│   └── utils/                 # Utility functions (e.g., date/time helpers)
│       └── timeChecker.js
│
├── config/
│   └── db.js                   # MongoDB connection setup
│
├── .env                        # Environment variables (PORT, DB_URI, etc.)
├── app.js                      # Express setup, middleware, route mounting
└── README.md                   # Project documentation
```

---

## 📦 Required Libraries
| Library        | Purpose                                |
|----------------|----------------------------------------|
| express        | HTTP server and routing                |
| mongoose       | MongoDB interaction                    |
| dotenv         | Environment variable configuration     |
| cors, helmet   | Security enhancements                  |
| joi            | Request validation                     |
| morgan         | HTTP request logger                    |

---

## 🔌 API Design

### 1. Get Adoptable Pets by City
- **Method:** `GET`
- **Route:** `/api/pets`
- **Query Params:** `city`
- **Response Example:**
```json
[
  {
    "name": "Fluffy",
    "type": "cat",
    "age": 2,
    "city": "Dubai"
  }
]
```
- **Common Errors:**
  - Missing city param
  - Invalid city value

---

### 2. Find Emergency Vet Clinics (Open Now)
- **Method:** `GET`
- **Route:** `/api/clinics/emergency`
- **Query Params:** `city`
- **Logic:** Returns clinics where `isOpenNow === true`
- **Response Example:**
```json
[
  {
    "name": "24/7 Vet Clinic",
    "city": "Dubai",
    "isOpenNow": true
  }
]
```
- **Common Errors:**
  - No clinics found
  - Invalid city param

---

### 3. Get Pet Care Articles by Type
- **Method:** `GET`
- **Route:** `/api/articles`
- **Query Params:** `type` (e.g., `cat`, `dog`)
- **Response Example:**
```json
[
  {
    "title": "How to care for your cat",
    "type": "cat",
    "content": "..."
  }
]
```
- **Common Errors:**
  - Missing type param
  - Unsupported type

---

## ⚠️ User Error Handling
Each API should:
- Validate query inputs using Joi.
- Return meaningful messages with status codes (e.g., 400 Bad Request).
- Send a 404 response if no data found.

---

## 🚀 Tips
- `routes/`: each file defines route + binds to controller (e.g. `router.get('/', getPetsByCity)`)
- `controllers/`: contains logic to validate requests, call services, and respond (e.g. checks for query `city`, then uses `petService.getByCity(city)`)
- `services/`: only holds logic for database queries and business rules (no HTTP code)
- Use Mongoose models to interact with MongoDB collections
- Utilities like `timeChecker.js` can check if a clinic is open based on working hours

---
