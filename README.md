1. Create Next.js app

```bash
mkdir samurai-square-test
cd samurai-square-test
mkdir frontend backend

cd frontend
npx create-next-app@latest .
```



2. Shadcn Install

  - 2.1 Install Shadcn in frontend
   ```bash
   npx shadcn@latest init
  
   ```

> Recommended:
- Select a component library : Radix
- Which `preset` would you like to use? : Luma
- style: default
- base color: slate
- CSS variables: yes
- React Server Components: yes
- app/globals.css: yes
- components path: default

Then install the components we’ll likely use first:

```bash
npx shadcn@latest add button card input switch badge textarea
```


3. Setup Backend(/backend)
```bash
cd backend
```
- Create Files
  ```bash
  touch main.py requirements.txt
  ```
- On `requirements.txt` paste the below
  ```python
  fastapi
  uvicorn[standard]
  sqlalchemy
  psycopg2-binary
  python-dotenv
  requests
  ```
- On `main.py`, paste the below
```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI(title="Samurai Square Test API")

app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:3000",
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.get("/")
def root():
    return {"message": "Backend running"}

@app.get("/api/health")
def health():
    return {"status": "ok"}
```
- Install packages
```bash
pip3 install -r requirements.txt
```
- Run Backend
```bash
uvicorn main:app --reload --port 8000
```
- Check in browser
```bash
http://localhost:8000
```
- Output :
```json
{"message":"Backend running"}
```

- Then
```json
{"message":"Backend running"}
```

- Output
```json
{"status":"ok"}
```

4. Docker compose final setup
- Create frontend/Dockerfile
```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "dev"]
```

- Create backend/Dockerfile
```docker
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "-m", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
```
- Create root `.env`

```env
DATABASE_URL=postgresql://postgres:postgres@db:5432/samurai_tax

SQUARE_ENV=sandbox
SQUARE_CLIENT_ID=YOUR_SQUARE_SANDBOX_CLIENT_ID
SQUARE_CLIENT_SECRET=YOUR_SQUARE_SANDBOX_CLIENT_SECRET
SQUARE_REDIRECT_URI=http://localhost:8000/api/square/oauth/callback
SQUARE_BASE_URL=https://connect.squareupsandbox.com

FRONTEND_URL=http://localhost:3000
```

- Create root `docker-compose.yml`

```yml
version: "3.9"

services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: samurai_frontend
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app
      - /app/node_modules
    environment:
      NEXT_PUBLIC_API_BASE_URL: http://localhost:8000
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: samurai_backend
    ports:
      - "8000:8000"
    volumes:
      - ./backend:/app
    env_file:
      - .env
    depends_on:
      - db

  db:
    image: postgres:16
    container_name: samurai_postgres
    restart: always
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: samurai_tax
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

- Run Docker
```bash
docker compose up -d --build```
```
- Confirm it works
```bash
Frontend → http://localhost:3000
Backend → http://localhost:8000
Health → http://localhost:8000/api/health
```

- Folder Structure
```bash
  samurai-square-test/
├── frontend/
├── backend/
├── docker-compose.yml
└── .env

project/
├── frontend/
│   ├── app/
│   ├── components/
│   └── lib/
│
└── backend/
    ├── app/
    │   ├── api/
    │   │   └── square_oauth.py
    │   ├── core/
    │   │   ├── config.py
    │   │   └── square.py
    │   ├── services/
    │   │   └── token_store.py
    │   └── main.py
    └── requirements.txt


```
