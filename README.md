1. Create Next.js app

```bash
mkdir samurai-square-test
cd samurai-square-test
mkdir frontend backend

cd frontend
npx create-next-app@latest .
```



2. shadcn + FastAPI + Docker compose final setup

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


- 2.2 Setup Backend(/backend)
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
pip install -r requirements.txt
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
