# Backend – Projeto Docker

Esta pasta contém a **API em Node.js** que serve como backend para o projeto Web Aula 27.  
Ela é responsável por fornecer endpoints REST para o frontend e conectar-se ao banco de dados PostgreSQL.

---

## 🛠️ Estrutura da API

```
backend/
├── server.js        # Arquivo principal do servidor Express
├── package.json     # Dependências e scripts
└── Dockerfile       # Dockerfile para criar imagem do backend
```

---

## 🚀 Como rodar localmente

1. Instale dependências:

```bash
npm install
```

2. Configure variáveis de ambiente criando um arquivo `.env` na raiz do projeto backend (ou use o `.env` principal da raiz):

```env
PORT=3000
DB_HOST=db
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=appdb
CORS_ORIGIN=*
```

3. Inicie o servidor:

```bash
npm start
```

*O backend ficará disponível em `http://localhost:3000/api`.*

---

## 🔧 Docker

Para rodar via Docker (recomendado):

1. Certifique-se de que o `docker-compose.yml` está configurado na raiz do projeto.  
2. Suba apenas o backend:

```bash
docker-compose up --build backend
```

O container do backend irá:

- Conectar-se ao banco de dados PostgreSQL definido no `docker-compose.yml`.  
- Servir a API no porto definido em `.env`.  

---

## 📝 Endpoints Principais

Exemplo de endpoints:

- `GET /api/users` → Lista todos os usuários  
- `POST /api/users` → Cria um novo usuário  
- `GET /api/users/:id` → Retorna usuário por ID  
- `PUT /api/users/:id` → Atualiza usuário  
- `DELETE /api/users/:id` → Remove usuário

> ⚠️ Ajuste os endpoints de acordo com a implementação atual do `server.js`.

---

## ✅ Conclusão

O backend está estruturado para:

* **Conectar ao PostgreSQL** de forma segura usando variáveis de ambiente.  
* **Servir a API** de forma estável e isolada em container Docker.  
* **Ser facilmente integrado** ao frontend ou a qualquer outra aplicação que consuma os dados.
