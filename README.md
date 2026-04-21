1. Create Next.js app

```bash
mkdir samurai-square-test
cd samurai-square-test
mkdir frontend backend

cd frontend
npx create-next-app@latest .
```



2. shadcn + FastAPI + Docker compose final setup

  - 2.1. Install Shadcn in frontend
   ```bash
   npx shadcn@latest init
  
   ```

> Recommended:
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
