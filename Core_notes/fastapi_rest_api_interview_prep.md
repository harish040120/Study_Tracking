# FastAPI & REST API Interview Prep Notes

---

## 1. REST API Fundamentals

### What is REST?
REST (Representational State Transfer) is an architectural style for designing networked applications. It relies on stateless, client-server communication, typically over HTTP.

### Key REST Principles
- **Statelessness** – Each request from client to server must contain all information needed to understand it; server stores no client context between requests.
- **Client-Server separation** – UI concerns separated from data storage concerns.
- **Uniform Interface** – Consistent way to interact with resources (URIs, HTTP verbs, representations).
- **Cacheability** – Responses must define themselves as cacheable or not.
- **Layered System** – Client can't tell if it's connected directly to the server or an intermediary (proxy, load balancer).
- **Code on Demand (optional)** – Servers can extend client functionality by sending executable code (e.g., JavaScript).

### HTTP Methods (Verbs)
| Method | Purpose | Idempotent? | Safe? |
|--------|---------|-------------|-------|
| GET | Retrieve a resource | Yes | Yes |
| POST | Create a resource | No | No |
| PUT | Update/replace a resource | Yes | No |
| PATCH | Partially update a resource | No (usually) | No |
| DELETE | Remove a resource | Yes | No |
| HEAD | Same as GET, no body | Yes | Yes |
| OPTIONS | Describe communication options | Yes | Yes |

**Idempotent** = calling it multiple times has the same effect as calling it once.
**Safe** = does not modify server state.

### Common HTTP Status Codes
- **1xx** – Informational
- **2xx** – Success
  - 200 OK, 201 Created, 202 Accepted, 204 No Content
- **3xx** – Redirection
  - 301 Moved Permanently, 304 Not Modified
- **4xx** – Client Error
  - 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 405 Method Not Allowed, 409 Conflict, 422 Unprocessable Entity, 429 Too Many Requests
- **5xx** – Server Error
  - 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout

### REST vs SOAP
- REST is lightweight, uses JSON/XML, stateless, uses standard HTTP methods.
- SOAP is a protocol with strict standards, XML-only, supports WS-Security, more overhead.

### REST vs GraphQL
- REST: multiple endpoints, fixed data shape, can lead to over/under-fetching.
- GraphQL: single endpoint, client specifies exactly what data it needs.

### What makes an API RESTful?
- Resource-based URLs (`/users/1` not `/getUser?id=1`)
- Proper use of HTTP verbs
- Stateless requests
- Standard status codes
- HATEOAS (Hypermedia as the Engine of Application State) — optional but part of true REST maturity (Richardson Maturity Model level 3)

### Richardson Maturity Model
- **Level 0**: Single endpoint, one HTTP method (RPC-style)
- **Level 1**: Multiple endpoints (resources), still one HTTP method
- **Level 2**: Multiple endpoints + proper HTTP verbs and status codes
- **Level 3**: HATEOAS – responses include links to related actions/resources

### API Versioning Strategies
- URI versioning: `/api/v1/users`
- Query param: `/api/users?version=1`
- Header versioning: `Accept: application/vnd.myapi.v1+json`
- Media type versioning

### Authentication & Authorization in REST APIs
- **Basic Auth** – username/password base64 encoded (not secure alone, use with HTTPS)
- **API Keys** – simple token in header/query
- **OAuth2** – delegated authorization framework
- **JWT (JSON Web Tokens)** – self-contained token with claims, signed
- **Session-based auth** – server maintains session state (less RESTful since it breaks statelessness)

### Idempotency Example
- Calling `DELETE /users/5` multiple times results in the same state (user deleted) — idempotent.
- Calling `POST /users` multiple times creates multiple users — NOT idempotent.

### Pagination, Filtering, Sorting
- Pagination: `?page=2&limit=20` or `?offset=20&limit=20`
- Filtering: `?status=active&role=admin`
- Sorting: `?sort=created_at&order=desc`

### Rate Limiting
Controls how many requests a client can make in a time window. Often implemented via API Gateway or middleware (token bucket, sliding window algorithms). Returns `429 Too Many Requests`.

---

## 2. FastAPI Fundamentals

### What is FastAPI?
A modern, high-performance Python web framework for building APIs, based on standard Python type hints. Built on top of **Starlette** (for web parts) and **Pydantic** (for data validation).

### Why FastAPI? (Key Selling Points)
- Very high performance (comparable to NodeJS/Go, thanks to Starlette + ASGI)
- Automatic interactive API docs (Swagger UI at `/docs`, ReDoc at `/redoc`)
- Type hints for validation and editor support
- Async support (native `async`/`await`)
- Dependency Injection system built-in
- Based on open standards: OpenAPI and JSON Schema

### Basic Example
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str | None = None):
    return {"item_id": item_id, "q": q}
```

### WSGI vs ASGI
- **WSGI** (e.g., Flask, Django classic) – synchronous, one request at a time per worker.
- **ASGI** (e.g., FastAPI, Starlette) – asynchronous, supports WebSockets, HTTP/2, concurrent requests within a single worker via event loop.
- FastAPI runs on ASGI servers like **Uvicorn** or **Hypercorn**.

### Path Parameters vs Query Parameters
```python
@app.get("/users/{user_id}")   # path parameter (required, part of URL)
def get_user(user_id: int, active: bool = True):  # query parameter (optional, after ?)
    ...
```

### Request Body with Pydantic
```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    is_offer: bool | None = None

@app.post("/items/")
def create_item(item: Item):
    return item
```

### Pydantic — Role in FastAPI
- Defines data models with type validation.
- Automatically validates request bodies, serializes responses.
- Raises `422 Unprocessable Entity` on validation failure.
- Pydantic v2 (used in FastAPI 0.100+) is much faster (Rust-based core).

### Response Models
```python
@app.post("/items/", response_model=Item)
def create_item(item: Item) -> Item:
    return item
```
- Filters/validates output data.
- Can hide sensitive fields (e.g., password) by using a different response model than the input model.

### Dependency Injection (`Depends`)
```python
from fastapi import Depends

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@app.get("/items/")
def read_items(db=Depends(get_db)):
    return db.query(Item).all()
```
- Used for shared logic: DB sessions, auth checks, pagination params, config.
- Supports sub-dependencies (dependencies depending on other dependencies).
- `Depends` can also be a class.

### Async vs Sync Path Operations
```python
@app.get("/sync")
def sync_route():
    # blocking code — FastAPI runs this in a threadpool
    return {"type": "sync"}

@app.get("/async")
async def async_route():
    # non-blocking, runs directly on the event loop
    await some_async_call()
    return {"type": "async"}
```
- Use `async def` when using `await`-able I/O (DB drivers like `asyncpg`, HTTP clients like `httpx`).
- Don't use blocking calls (like `time.sleep` or sync DB calls) inside `async def` — it blocks the event loop.

### Middleware
```python
@app.middleware("http")
async def add_process_time_header(request, call_next):
    start = time.time()
    response = await call_next(request)
    response.headers["X-Process-Time"] = str(time.time() - start)
    return response
```
Used for logging, CORS, authentication, timing, etc.

### CORS in FastAPI
```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://example.com"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### Background Tasks
```python
from fastapi import BackgroundTasks

def write_log(message: str):
    with open("log.txt", "a") as f:
        f.write(message)

@app.post("/send-notification/")
async def send_notification(email: str, background_tasks: BackgroundTasks):
    background_tasks.add_task(write_log, f"notification sent to {email}")
    return {"message": "Notification sent"}
```
Runs after returning the response — useful for lightweight async jobs (not for heavy tasks; use Celery/RQ for that).

### Exception Handling
```python
from fastapi import HTTPException

@app.get("/items/{item_id}")
def read_item(item_id: int):
    if item_id not in items:
        raise HTTPException(status_code=404, detail="Item not found")
    return items[item_id]
```
Custom exception handlers:
```python
@app.exception_handler(CustomException)
async def custom_exception_handler(request, exc):
    return JSONResponse(status_code=418, content={"message": str(exc)})
```

### FastAPI Routers (Modular Apps)
```python
from fastapi import APIRouter

router = APIRouter(prefix="/users", tags=["users"])

@router.get("/")
def list_users():
    return []

app.include_router(router)
```
Used to organize large apps into modules.

### Dependency Overrides (Testing)
```python
app.dependency_overrides[get_db] = get_test_db
```
Useful for swapping real DB with a mock/test DB in unit tests.

### Testing FastAPI
```python
from fastapi.testclient import TestClient

client = TestClient(app)

def test_read_main():
    response = client.get("/")
    assert response.status_code == 200
```
Uses `httpx`/`starlette.testclient` under the hood.

### Validation with Field / Query / Path
```python
from fastapi import Query, Path
from pydantic import Field

@app.get("/items/")
def read_items(q: str = Query(default=None, min_length=3, max_length=50)):
    ...

class Item(BaseModel):
    price: float = Field(gt=0, description="must be greater than zero")
```

### Custom Validators (Pydantic)
```python
from pydantic import field_validator

class User(BaseModel):
    username: str

    @field_validator("username")
    def username_alphanumeric(cls, v):
        assert v.isalnum(), "must be alphanumeric"
        return v
```

### Security in FastAPI
- `OAuth2PasswordBearer` for token-based auth
- `Depends` to inject the current authenticated user
```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

@app.get("/users/me")
def read_current_user(token: str = Depends(oauth2_scheme)):
    return decode_token(token)
```

### FastAPI vs Flask vs Django
| Feature | FastAPI | Flask | Django |
|---|---|---|---|
| Async support | Native | Limited (via extensions) | Since Django 3.1 (partial) |
| Data validation | Built-in (Pydantic) | Manual/extensions | Forms/DRF serializers |
| Auto docs | Yes (Swagger/ReDoc) | No (needs extension) | No (DRF has some) |
| Performance | Very high | Moderate | Moderate |
| Learning curve | Low-Medium | Low | Medium-High |
| Best for | APIs, microservices | Small apps/APIs | Full-stack apps |

### Uvicorn / Gunicorn
- **Uvicorn**: lightning-fast ASGI server, used to run FastAPI apps.
  ```bash
  uvicorn main:app --reload
  ```
- **Gunicorn + Uvicorn workers**: used in production for multi-process scaling.
  ```bash
  gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
  ```

### Common FastAPI Gotchas
- Mutable default arguments in Pydantic models can cause shared state bugs — use `Field(default_factory=list)`.
- Blocking calls inside `async def` routes block the entire event loop.
- Order of routes matters — more specific paths should be declared before generic/dynamic ones.
- `response_model` doesn't validate the return type at runtime for performance unless explicitly configured.

---

## 3. Common Interview Questions (Quick Answers)

**Q: What is the difference between PUT and PATCH?**
A: PUT replaces the entire resource; PATCH updates only the specified fields.

**Q: Why is FastAPI faster than Flask?**
A: FastAPI is built on Starlette/ASGI enabling async I/O and concurrency, while Flask is WSGI-based and synchronous by default.

**Q: How does FastAPI generate docs automatically?**
A: It uses type hints and Pydantic models to build an OpenAPI schema, rendered via Swagger UI (`/docs`) and ReDoc (`/redoc`).

**Q: What's the difference between `@app.get()` and `@app.post()`?**
A: Both register route handlers, but bind to different HTTP methods — GET retrieves data (no body expected), POST sends data to create/process a resource (expects a body).

**Q: What is `Depends()` used for?**
A: Dependency injection — reusing logic like DB sessions, auth, or shared parameters across path operations.

**Q: How do you secure a FastAPI endpoint?**
A: Using `Depends` with security schemes like `OAuth2PasswordBearer`, API key headers, or JWT validation functions.

**Q: What status code should a successful POST that creates a resource return?**
A: `201 Created`, often with a `Location` header pointing to the new resource.

**Q: What is idempotency and which REST methods are idempotent?**
A: An idempotent operation produces the same result no matter how many times it's called. GET, PUT, DELETE, HEAD are idempotent; POST and PATCH generally are not.

**Q: How would you handle versioning in a FastAPI project?**
A: Typically via URL prefix using routers, e.g., `/api/v1/...`, `/api/v2/...`, each with its own `APIRouter`.

**Q: What is the difference between `sync def` and `async def` route handlers in FastAPI?**
A: `async def` runs directly on the event loop (non-blocking); `def` routes are automatically run in a separate threadpool by FastAPI, so blocking code doesn't stall the event loop.

**Q: How do you validate query/path parameters beyond type checking?**
A: Using `Query()`, `Path()`, and `Field()` with constraints like `min_length`, `max_length`, `gt`, `lt`, regex patterns, etc.

**Q: What's the purpose of `response_model` in FastAPI?**
A: It defines the shape of the response, filters out extra fields, and documents the response schema in OpenAPI docs.

---

## 4. Quick Cheat Sheet

- **Statelessness** = no session in REST
- **Idempotent** methods: GET, PUT, DELETE, HEAD
- **FastAPI stack**: Starlette (web) + Pydantic (data) + Uvicorn (server)
- **Docs**: `/docs` (Swagger), `/redoc` (ReDoc)
- **201** = Created, **204** = No Content, **422** = Validation Error
- **Depends()** = dependency injection
- **BackgroundTasks** = lightweight async post-response jobs
- **CORSMiddleware** = handle cross-origin requests
