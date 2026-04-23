# vue-social-feed

> Vue Social Feed: Social network feed with posts, likes, comments, and real-time updates

##  Features
- JWT authentication with access + refresh tokens
- Role-based access control (admin/user)
- Full CRUD with pagination, search, and filtering
- Premium responsive UI with dark/light mode
- Real-time validation and error handling
- Audit logging and activity history
- Background jobs for long-running workflows
- Admin analytics dashboard with operational metrics
- Docker Compose for one-command startup
- Comprehensive README with API documentation
- Database migrations with Alembic/Flyway
- Health checks, readiness checks, and structured logging
- Production-oriented environment configuration

##  Tech Stack
Vue.js, TypeScript, FastAPI, PostgreSQL, Pinia

##  Architecture
Three-tier architecture: Vue.js, TypeScript backend, React/TypeScript frontend, PostgreSQL database. Containerized with Docker.

##  Quick Start

### Prerequisites
- Docker & Docker Compose
- SQLite / PostgreSQL

### Setup

```bash
# Clone the repository
git clone https://github.com/elitsuri/vue-social-feed
cd vue-social-feed

# Copy environment variables
cp .env.example .env
```

### Run

```bash
docker compose up --build
```
