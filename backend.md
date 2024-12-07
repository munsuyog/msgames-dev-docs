# Backend Documentation

## Project Overview
The backend is built using Python with MongoDB as the database, following a modular architecture with clear separation of concerns. It implements RESTful API principles for the game management system.

## Core Technologies
- Python 3.8+
- MongoDB
- PyMongo
- Firebase Admin SDK
- Python-dotenv

## Directory Structure

```
beergame_backend/
├── __pycache__/          # Python cache
├── config/               # Configuration files
│   ├── __init__.py
│   ├── database.py      # MongoDB configuration
│   └── firebase.py      # Firebase configuration
│
├── controllers/          # Request handlers
│   ├── __init__.py
│   ├── auth.py          # Authentication logic
│   ├── game.py          # Game logic
│   └── user.py          # User management
│
├── models/              # Data models
│   ├── __init__.py
│   ├── game.py
│   └── user.py
│
├── routes/              # API routes
│   ├── __init__.py
│   ├── auth_routes.py
│   ├── game_routes.py
│   └── user_routes.py
│
├── services/            # Business logic
│   ├── __init__.py
│   ├── auth_service.py
│   ├── game_service.py
│   └── user_service.py
│
├── utils/              # Utility functions
│   ├── __init__.py
│   ├── decorators.py   # Auth decorators
│   └── helpers.py      # Helper functions
│
├── main.py            # Application entry point
├── requirements.txt   # Dependencies
└── .env              # Environment variables
```

## Component Details

### `/config`
Database and Firebase configuration:

```python
# config/database.py
from pymongo import MongoClient
import os

class DatabaseConfig:
    def __init__(self):
        self.client = MongoClient(os.getenv('MONGODB_URI'))
        self.db = self.client[os.getenv('DB_NAME')]

    def get_db(self):
        return self.db
```

### `/controllers`
Request handlers and business logic:

```python
# controllers/game.py
from models.game import GameModel
from services.game_service import GameService

class GameController:
    def __init__(self):
        self.game_service = GameService()

    def create_game(self, game_data: dict):
        return self.game_service.create_game(game_data)

    def get_game(self, game_id: str):
        return self.game_service.get_game(game_id)
```

### `/models`
Data models for MongoDB:

```python
# models/game.py
from datetime import datetime
from bson import ObjectId

class GameModel:
    def __init__(self, db):
        self.collection = db['games']

    def create(self, game_data: dict):
        game_data['created_at'] = datetime.utcnow()
        result = self.collection.insert_one(game_data)
        return str(result.inserted_id)

    def find_by_id(self, game_id: str):
        return self.collection.find_one({'_id': ObjectId(game_id)})
```

### `/services`
Business logic implementation:

```python
# services/game_service.py
from models.game import GameModel
from config.database import DatabaseConfig

class GameService:
    def __init__(self):
        db = DatabaseConfig().get_db()
        self.game_model = GameModel(db)

    def create_game(self, game_data: dict):
        # Add business logic here
        game_id = self.game_model.create(game_data)
        return {'game_id': game_id, **game_data}

    def get_game(self, game_id: str):
        return self.game_model.find_by_id(game_id)
```

## API Routes and Structure

### Game Management
```python
# routes/game_routes.py
from controllers.game import GameController

class GameRoutes:
    def __init__(self):
        self.controller = GameController()

    def create_game(self, request_data: dict):
        return self.controller.create_game(request_data)

    def get_game(self, game_id: str):
        return self.controller.get_game(game_id)
```

### Authentication
```python
# routes/auth_routes.py
from controllers.auth import AuthController

class AuthRoutes:
    def __init__(self):
        self.controller = AuthController()

    def login(self, credentials: dict):
        return self.controller.login(credentials)

    def register(self, user_data: dict):
        return self.controller.register(user_data)
```

## Database Operations

### MongoDB Helpers
```python
# utils/db_helpers.py
from bson import ObjectId
from datetime import datetime

class MongoHelper:
    @staticmethod
    def serialize_doc(doc):
        if doc.get('_id'):
            doc['id'] = str(doc['_id'])
            del doc['_id']
        return doc

    @staticmethod
    def prepare_query(query_params: dict):
        query = {}
        for key, value in query_params.items():
            if key == 'id':
                query['_id'] = ObjectId(value)
            else:
                query[key] = value
        return query
```

## Authentication

### Firebase Auth Integration
```python
# services/auth_service.py
import firebase_admin
from firebase_admin import auth
from config.firebase import firebase_app

class AuthService:
    def verify_token(self, token: str):
        try:
            decoded_token = auth.verify_id_token(token, firebase_app)
            return decoded_token
        except Exception as e:
            raise Exception("Invalid token")

    def get_user_by_email(self, email: str):
        try:
            user = auth.get_user_by_email(email)
            return user
        except Exception as e:
            return None
```

## Main Application

### Entry Point
```python
# main.py
from config.database import DatabaseConfig
from routes.game_routes import GameRoutes
from routes.auth_routes import AuthRoutes

class Application:
    def __init__(self):
        self.db = DatabaseConfig().get_db()
        self.game_routes = GameRoutes()
        self.auth_routes = AuthRoutes()

    def start(self):
        # Initialize your application
        print("Application started")

if __name__ == "__main__":
    app = Application()
    app.start()
```

## Environment Configuration

### Environment Variables
```env
MONGODB_URI=mongodb://localhost:27017
DB_NAME=beergame
FIREBASE_CREDENTIALS=path/to/credentials.json
ENVIRONMENT=development
```

## Security

### Authentication Decorator
```python
# utils/decorators.py
from functools import wraps
from services.auth_service import AuthService

def require_auth(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        auth_header = # Get auth header from request
        if not auth_header:
            raise Exception("No authorization header")
            
        token = auth_header.split(" ")[1]
        auth_service = AuthService()
        user = auth_service.verify_token(token)
        
        return f(*args, **kwargs, user=user)
    return decorated
```

## Error Handling

### Custom Exceptions
```python
# utils/exceptions.py
class GameError(Exception):
    def __init__(self, message: str, status_code: int = 400):
        self.message = message
        self.status_code = status_code
        super().__init__(self.message)

class AuthError(Exception):
    def __init__(self, message: str, status_code: int = 401):
        self.message = message
        self.status_code = status_code
        super().__init__(self.message)
```

## Deployment

### Docker Configuration
```dockerfile
FROM python:3.8-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .

CMD ["python", "main.py"]
```