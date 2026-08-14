# My Ecommerce App — NestJS + React + MySQL + Docker

A full-stack application built with **NestJS**, **React**, and **MySQL**, with Docker Compose for local development.

## 🏗️ Project Structure

```text
my-project/
├── backend/
│   ├── src/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── package.json
│   └── ...
│
├── frontend/
│   ├── src/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── package.json
│   └── ...
│
├── docker-compose.yml
├── .gitignore
├── .env.example
└── README.md
```

## 🚀 Tech Stack

### Backend

* NestJS
* Node.js
* TypeScript
* MySQL

### Frontend

* React
* JavaScript / TypeScript
* Vite

### Development

* Docker
* Docker Compose
* Git / GitHub

## 🐳 Docker Services

The project uses three Docker services:

| Service  |   Port | Description              |
| -------- | -----: | ------------------------ |
| Frontend | `5173` | React development server |
| Backend  | `3000` | NestJS API               |
| MySQL    | `3306` | MySQL database           |

Architecture:

```text
                    ┌──────────────────┐
                    │     Browser      │
                    └────────┬─────────┘
                             │
                             │ :5173
                             ▼
                    ┌──────────────────┐
                    │      React       │
                    │    Frontend      │
                    └────────┬─────────┘
                             │
                             │ API
                             │ :3000
                             ▼
                    ┌──────────────────┐
                    │      NestJS      │
                    │      Backend     │
                    └────────┬─────────┘
                             │
                             │ :3306
                             ▼
                    ┌──────────────────┐
                    │      MySQL       │
                    │     Database     │
                    └──────────────────┘
```

## 📋 Requirements

Install the following before starting:

* Git
* Docker
* Docker Compose

Check your installation:

```bash
docker --version
docker compose version
git --version
```

## 📥 Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/my-project.git
```

Enter the project:

```bash
cd my-project
```

## ⚙️ Environment Variables

Create environment files from the provided examples.

### Backend

```bash
cd backend
cp .env.example .env
cd ..
```

### Frontend

If the frontend requires environment variables:

```bash
cd frontend
cp .env.example .env
cd ..
```

Update the values according to your local environment.

**Never commit real `.env` files to GitHub.**

## 🐳 Start the Development Environment

From the project root:

```bash
docker compose up --build
```

To run in the background:

```bash
docker compose up --build -d
```

After the containers start:

### Frontend

Open:

```text
http://localhost:5173
```

### Backend

Open:

```text
http://localhost:3000
```

### MySQL

MySQL is available from the host at:

```text
127.0.0.1:3306
```

> `http://localhost:3306` is not a web page. Port `3306` uses the MySQL database protocol.

## 🔄 Development Hot Reload

The Docker setup mounts the source directories into the containers:

```yaml
volumes:
  - ./backend:/app
  - ./frontend:/app
```

This allows changes made locally to be reflected inside the containers.

### Backend

NestJS runs:

```bash
npm run start:dev
```

Changes to the backend source automatically trigger a reload.

### Frontend

Vite runs:

```bash
npm run dev -- --host 0.0.0.0
```

Changes to React source files are automatically detected.

## 🗄️ Database

The MySQL container uses a Docker volume to persist database data:

```yaml
volumes:
  mysql_data:
```

Therefore, restarting the containers does not normally remove your database.

To stop the containers:

```bash
docker compose down
```

To stop the containers **and delete the database volume**:

```bash
docker compose down -v
```

> Be careful with `docker compose down -v`. It permanently removes the MySQL Docker volume and its data.

## 🔌 Database Connection

Inside the Docker network, NestJS should connect to MySQL using the service name:

```env
DB_HOST=mysql
DB_PORT=3306
```

Do **not** use:

```env
DB_HOST=localhost
```

when NestJS is running inside Docker.

The important distinction is:

```text
NestJS container → mysql:3306
Your host machine → localhost:3306
Browser → localhost:5173
Browser → localhost:3000
```

## 🛠️ Useful Docker Commands

### Start containers

```bash
docker compose up
```

### Start and rebuild images

```bash
docker compose up --build
```

### Start in background

```bash
docker compose up -d
```

### Stop containers

```bash
docker compose down
```

### View all containers

```bash
docker compose ps
```

### View backend logs

```bash
docker compose logs -f backend
```

### View frontend logs

```bash
docker compose logs -f frontend
```

### View MySQL logs

```bash
docker compose logs -f mysql
```

### Restart a service

```bash
docker compose restart backend
```

### Open a shell inside the backend

```bash
docker compose exec backend sh
```

### Open a MySQL shell

```bash
docker compose exec mysql mysql -u root -p
```

## 🧪 Running Tests

### Backend

Run tests inside the backend container:

```bash
docker compose exec backend npm test
```

Watch tests:

```bash
docker compose exec backend npm run test:watch
```

### Frontend

Run frontend tests according to the testing framework configured in the project.

## 📦 Installing New Packages

If you install a new backend dependency:

```bash
docker compose exec backend npm install package-name
```

For example:

```bash
docker compose exec backend npm install @nestjs/config
```

For frontend:

```bash
docker compose exec frontend npm install package-name
```

After changing dependencies, rebuild if necessary:

```bash
docker compose up --build
```

## 🗂️ Git Workflow

Check changes:

```bash
git status
```

Add changes:

```bash
git add .
```

Commit:

```bash
git commit -m "Add feature"
```

Push:

```bash
git push
```

## 🔐 Security

Do not commit sensitive information such as:

```text
.env
database passwords
JWT secrets
API keys
private credentials
access tokens
```

Use `.env.example` for documenting required environment variables.

Example:

```env
DB_HOST=mysql
DB_PORT=3306
DB_NAME=myapp
DB_USER=myapp
DB_PASSWORD=your_password
JWT_SECRET=your_secret
```

Use real values only in your local `.env`.

## 🧹 Cleaning Docker

Remove containers:

```bash
docker compose down
```

Remove containers and database volume:

```bash
docker compose down -v
```

Rebuild everything:

```bash
docker compose build --no-cache
```

Then start again:

```bash
docker compose up
```
