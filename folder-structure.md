## Frontend Project
```bash
frontend/
├─ app/
│  ├─ layout.tsx
│  ├─ page.tsx
│  ├─ globals.css
│  └─ api/
│     └─ health/
│        └─ route.ts
│
├─ components/
│  ├─ ui/
│  │  └─ ...shadcn components
│  │
│  ├─ square-connect-card.tsx
│  ├─ receipt-search-card.tsx
│  ├─ json-result-viewer.tsx
│  └─ page-header.tsx
│
├─ lib/
│  ├─ api.ts
│  ├─ constants.ts
│  ├─ utils.ts
│  └─ types.ts
│
├─ public/
│
├─ Dockerfile
├─ docker-compose.yml
├─ package.json
├─ tsconfig.json
├─ next.config.ts
└─ .env.local

```

## Backend Project
```bash
backend/
├─ app/
│  ├─ main.py
│  │
│  ├─ api/
│  │  ├─ square.py
│  │  ├─ receipts.py
│  │  └─ health.py
│  │
│  ├─ models/
│  │  ├─ square_connection.py
│  │  └─ receipt_transaction.py
│  │
│  ├─ schemas/
│  │  ├─ square.py
│  │  └─ receipt.py
│  │
│  ├─ services/
│  │  ├─ square_oauth.py
│  │  ├─ square_transactions.py
│  │  └─ receipt_service.py
│  │
│  ├─ db/
│  │  ├─ session.py
│  │  ├─ base.py
│  │  └─ init_db.py
│  │
│  └─ core/
│     └─ config.py
│
├─ requirements.txt
├─ Dockerfile
├─ docker-compose.yml
└─ .env

```


```mermaid

flowchart TD

subgraph DB["Database / Postgres"]
direction TD

A["schema.sql"]
B["seed.sql"]

subgraph T["Tables"]
direction TD
C["square_connections"]
D["receipt_transactions"]
end

subgraph S["square_connections Columns"]
direction TD
E["id"]
F["merchant_id"]
G["environment"]
H["access_token"]
I["refresh_token"]
J["expires_at"]
K["created_at"]
end

subgraph R["receipt_transactions Columns"]
direction TD
L["id"]
M["receipt_no (JUwA)"]
N["merchant_id"]
O["square_payment_id"]
P["square_order_id"]
Q["amount_money"]
R1["currency"]
S1["status"]
T1["raw_square_response JSONB"]
U["created_at"]
end

A --> T
B --> T
C --> E
C --> F
C --> G
C --> H
C --> I
C --> J
C --> K

D --> L
D --> M
D --> N
D --> O
D --> P
D --> Q
D --> R1
D --> S1
D --> T1
D --> U

end

style DB fill:transparent,stroke:#dc2626,stroke-width:2px
style T fill:transparent,stroke:#ef4444,stroke-width:2px
style S fill:transparent,stroke:#f87171,stroke-width:1.5px
style R fill:transparent,stroke:#f87171,stroke-width:1.5px

```
