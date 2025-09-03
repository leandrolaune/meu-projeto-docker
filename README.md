````markdown
# Projeto : "Meu-projeto-docker" – Versão com Docker

Este projeto foi baseado no repositório fornecido em aula e modificado para rodar em **containers Docker** com orquestração via **Docker Compose**.

Ele é composto por três serviços principais:

- **db** → Banco de dados PostgreSQL
- **backend** → API em Node.js (Express + PG)
- **frontend** → Aplicação web servida via Nginx

---

## 🚀 Como rodar

1. Crie um arquivo `.env` na raiz do projeto (baseado no exemplo abaixo):

```env
# Banco de Dados
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=appdb

# Backend
PORT=3000
CORS_ORIGIN=*

# DB acessado pelo backend
DB_HOST=db
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=appdb

# Frontend
API_URL=http://localhost:3000/api
````

2. Suba os containers:

```bash
docker-compose up --build
```

3. Acesse no navegador:

* Frontend: [http://localhost:8080](http://localhost:8080)
* Backend API: [http://localhost:3000/api](http://localhost:3000/api)

---

## 🛠️ Estrutura de Serviços

* **db** → PostgreSQL com persistência em volume e script de inicialização.
* **backend** → API Node.js conectada ao banco, com variáveis de ambiente via `.env`.
* **frontend** → Aplicação web estática em Nginx, comunicando-se com a API.

---

## 🔧 Melhorias realizadas

### 1. Backend – Dockerfile

Alterado para usar:

```dockerfile
RUN npm ci --omit=dev || npm install --omit=dev
CMD ["npm", "start"]
```

**Motivo:**
Evita instalar dependências de desenvolvimento em produção e garante que o container rode no modo produção (`npm start`), não em desenvolvimento (`npm run dev`).

---

### 2. Frontend – app.js

Linha alterada de:

```javascript
const API = "http://localhost:3000/api";
```

para:

```javascript
const API = window.API_URL || "http://localhost:3000/api";
```

**Motivo:**
Agora a URL da API pode ser configurada dinamicamente via variável global `window.API_URL` (injetada no container). Isso evita modificar o código ao mudar de ambiente (dev → produção).

---

### 3. Orquestração – docker-compose.yml

* Substituição de blocos `environment` por:

```yaml
env_file:
  - .env
```

**Motivo:**
Centraliza variáveis em um único arquivo `.env`, simplificando manutenção.

* Adição de:

```yaml
environment:
  - API_URL=http://localhost:3000/api
```

no frontend.

**Motivo:**
Injeta dinamicamente a URL da API no container, permitindo que o `app.js` use o valor configurado.

---

## 📂 Estrutura do Projeto

```
.
├── backend/        # API Node.js
│   ├── server.js
│   ├── package.json
│   └── Dockerfile
├── frontend/       # Aplicação frontend (HTML + JS)
│   ├── app.js
│   └── Dockerfile
├── db/
│   └── init.sql
├── docker-compose.yml
├── .env
└── README.md
```

---

## ⚙️ Como o Projeto Funciona

1. **Frontend**

   * Interface web simples que consome a API backend.
   * Recebe a URL da API via `window.API_URL` e faz requisições AJAX.

2. **Backend**

   * Servidor Node.js usando Express.
   * Expõe endpoints REST para manipulação de dados no PostgreSQL.
   * Conecta ao banco usando credenciais do `.env`.

3. **Banco de Dados (db)**

   * PostgreSQL inicializado com script `init.sql`.
   * Persistência em volume Docker, mantendo dados mesmo após reiniciar containers.

4. **Comunicação entre serviços**

   * Backend conecta ao DB usando hostname `db` (nome do serviço no `docker-compose.yml`).
   * Frontend acessa backend via variável `API_URL`.

5. **Orquestração Docker**

   * `docker-compose` gerencia a criação, inicialização e rede entre containers.
   * Cada serviço roda isolado, mas em rede interna comum para comunicação.

---

## ✅ Conclusão

As modificações foram pequenas, mas importantes para garantir que o projeto seja:

* **Portável:** roda em qualquer máquina com Docker.
* **Configurável:** variáveis centralizadas no `.env`.
* **Próximo de produção:** backend rodando com dependências corretas, frontend apontando para a API dinamicamente.
* **Bem estruturado:** separação clara de frontend, backend e banco, facilitando manutenção e deploy.

```
```

