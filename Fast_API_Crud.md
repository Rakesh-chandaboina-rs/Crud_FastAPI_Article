# CRUD OPERATIONS USING FASTAPI
## FastAPI:
### Overview:
FastAPI is a modern, high-performance Python web framework specifically designed for building APIs. It's known for its speed, ease of use, and ability to generate interactive API documentation automatically. FastAPI is built using Python 3.6+ and leverages type hints, Pydantic, and Starlette.

## ✅Key Features:
1. **Fast**: Very fast performance—comparable to Node.js and Go.

2. **Automatic Validation**: Uses Python type hints to validate and serialize data.

3. **Interactive Docs**: Automatically creates Swagger UI and ReDoc API documentation.

4. **Asynchronous Support**: Natively supports async/await for handling high-concurrency tasks.

5. **Data Handling**: Built on Pydantic for data parsing and validation.

6. **Web Handling**: Built on Starlette for web routing, middleware, and more.

7. **Security**: Includes tools for authentication, OAuth2, JWT, etc.

## 🚀Why FastAPI?

- FastAPI was created to solve common problems in backend development:

- Writing APIs with minimal code

- Ensuring automatic validation and documentation

- Supporting asynchronous programming (async/await)

- Providing high performance like Node.js or Go

## 🔨 Installation

- pip install fastapi
- pip install "uvicorn[standard]"  # ASGI server to run the app

## 🧪Hello World Example:
```python

from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Welcome to FastAPI!"}
```

## Run the app:

- uvicorn main:app --reload

## Visit:

- Swagger docs: http://127.0.0.1:8000/docs

- Redoc docs: http://127.0.0.1:8000/redoc

## 🧱 Key Concepts

### 1. Path Parameters

```python
@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

### 2. Query Parameters

```python
@app.get("/search/")
def search_items(q: str = None):
    return {"query": q}
```

### 3. Request Body with Pydantic

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float

@app.post("/items/")
def create_item(item: Item):
    return item
```

### 4. Response Model

```python
@app.post("/items/", response_model=Item)
def create_item(item: Item):
    return item
```

## 🧩 Dependency Injection Example

```python
from fastapi import Depends

def get_token():
    return "secure-token"

@app.get("/profile")
def read_profile(token: str = Depends(get_token)):
    return {"token": token}
```

## 🛡 Security and Authentication

### FastAPI supports:

- OAuth2

- JWT (JSON Web Tokens)

- HTTP Basic Auth

- Custom schemes with middleware

## Example:

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

@app.get("/users/me")
def read_users_me(token: str = Depends(oauth2_scheme)):
    return {"token": token}
```


## 📈 Use Cases

- Microservices

- Machine Learning model APIs

- IoT backends

- Real-time dashboards

- Serverless apps (AWS Lambda, etc.)


## 🧠 Extra Tools with FastAPI

- SQLModel or SQLAlchemy: For databases

- Alembic: For migrations

- PyJWT / Authlib: For auth

- Pydantic: For validation

- Uvicorn / Gunicorn: For ASGI serving


## 🧾 Conclusion

-  FastAPI is a powerful, modern Python framework designed to make web API development fast and fun. Whether you're building a microservice, a data API, or deploying an ML model, FastAPI has you covered with speed, structure, and simplicity.

# 🛠CRUD Operations Using FastAPI:

## 📌Introduction:

### CRUD stands for:

- Create (POST)

- Read (GET)

- Update (PUT)

- Delete (DELETE)


FastAPI makes it easy to create these operations with Python and automatic validation using Pydantic.


## ⚙ Setup->

Install FastAPI and Uvicorn

pip install fastapi uvicorn


## 📁 Project Structure:

```python
crud_fastapi/
├── main.py
├── models.py
├── database.py
```


## Create models.py

```python
from pydantic import BaseModel

class Item(BaseModel):
    id: int
    name: str
    description: str
    price: float
```
- BaseModel from Pydantic automatically handles validation.

- Each item will have: id, name, description, and price.

## Create database.py (In-memory DB)

```python
from typing import List
from models import Item

items: List[Item] = []  # This simulates a database
```

## Create main.py (CRUD API)

```python
from fastapi import FastAPI, HTTPException
from schemas import Item
import database

app = FastAPI()
```

## Create
```python
@app.post("/items/", response_model=Item)
def create_item(item: Item):
    for db_item in database.items:
        if db_item.id == item.id:
            raise HTTPException(status_code=400, detail="Item already exists")
    database.items.append(item)
    return item
```

## Read All
```python
@app.get("/items/", response_model=list[Item])
def read_items():
    return database.items
```

## Read One
```python
@app.get("/items/{item_id}", response_model=Item)
def read_item(item_id: int):
    for item in database.items:
        if item.id == item_id:
            return item
    raise HTTPException(status_code=404, detail="Item not found")
```
## Update
```python
@app.put("/items/{item_id}", response_model=Item)
def update_item(item_id: int, updated_item: Item):
    for i, item in enumerate(database.items):
        if item.id == item_id:
            database.items[i] = updated_item
            return updated_item
    raise HTTPException(status_code=404, detail="Item not found")
```

## Delete
```python
@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    for i, item in enumerate(database.items):
        if item.id == item_id:
            del database.items[i]
            return {"message": "Item deleted successfully"}
    raise HTTPException(status_code=404, detail="Item not found")
```


## ▶ Run the API

- uvicorn main:app --reload

- Visit: http://127.0.0.1:8000/docs


## 📦 Sample POST JSON:
```python
{
  "id": 1,
  "name": "Notebook",
  "description": "100 pages notebook",
  "price": 45.0
}
```


## ✅ Output Example:

- POST /items/ → Creates an item

- GET /items/ → Lists all items

- GET /items/1 → Gets item with ID 1

- PUT /items/1 → Updates item

- DELETE /items/1 → Deletes item


# ✅ Advantages of FastAPI for CRUD->

- Automatic validation with Pydantic

- Interactive documentation via Swagger UI

- Asynchronous support (if needed)

- Type safety and auto-completion in editors

- Easy to extend with databases, auth, and more


## 🧾 Conclusion:

This is a basic CRUD API using FastAPI with in-memory storage.

✅ Easy to test
✅ Auto documentation
✅ Ready to extend with database and auth

## INTERVIEW Questions?
1. What is FastAPI and why is it used?

2. What does CRUD stand for?

3. How do you define a POST endpoint in FastAPI?

4. What is Pydantic used for in FastAPI?

5. What is the purpose of the BaseModel in FastAPI?

6. How do you perform input validation in FastAPI?

7. What HTTP methods are used for CRUD operations in FastAPI?

8. How do you use path parameters and query parameters in FastAPI?

9. How do you return custom status codes in FastAPI?

10. What is the use of response_model in FastAPI?

11. How can you organize your FastAPI CRUD code in a real-world project?

12. How do you handle errors like "item not found" in FastAPI?

13. How do you test CRUD APIs in FastAPI?

14. What are dependencies in FastAPI and how can they be used in CRUD?

15. How would you implement an in-memory database in a FastAPI CRUD app?
