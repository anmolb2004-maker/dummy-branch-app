Dummy Loan Service API – DevOps CI/CD Project

A containerized Flask-based microservice that manages loan creation and reporting.
Fully automated CI/CD deployment to AWS EC2 using Docker + GitHub Actions.

Features-

Create, list, and analyze loans

PostgreSQL database integration

Dockerized for development, staging & production

Automated deployment pipeline using GitHub Actions

Health check endpoint:

http://<EC2-PUBLIC-IP>:8000/health

API Endpoints-

| Method | Endpoint     | Description       |
| ------ | ------------ | ----------------- |
| GET    | `/health`    | Check API status  |
| GET    | `/api/loans` | List all loans    |
| POST   | `/api/loans` | Create a new loan |
| GET    | `/api/stats` | Loan statistics   |

curl -X POST http://localhost:8000/api/loans \
-H 'Content-Type: application/json' \
-d '{
  "borrower_id": "usr_india_999",
  "amount": 12000.50,
  "currency": "INR",
  "term_months": 6,
  "interest_rate_apr": 24.0
}'

# Clone repository
git clone https://github.com/anmold2004-maker/dummy-branch-app.git
cd dummy-branch-app

# Start containers
docker-compose -f docker-compose.dev.yml up --build

Open in browser:

http://localhost:8000/health

Deploy Environments-

| Environment | Command                                                      |
| ----------- | ------------------------------------------------------------ |
| Development | `docker-compose -f docker-compose.dev.yml up`                |
| Staging     | `docker-compose -f docker-compose.staging.yml up -d --build` |
| Production  | `docker-compose -f docker-compose.prod.yml up -d --build`    |


CI/CD Pipeline (GitHub Actions)

| Stage         | Purpose                                             |
| ------------- | --------------------------------------------------- |
| Test Stage    | Run tests, stop pipeline if failure                 |
| Build Stage   | Build docker image & tag using commit SHA           |
| Security Scan | Scan image for vulnerabilities (future improvement) |
| Deploy Stage  | Push app to EC2 and restart containers              |

Pipeline Trigger
on:
  push:
    branches: [ "main" ]

Deployment Flow
Developer push code → GitHub Actions → SSH to EC2 → Copy project → docker-compose up → App deployed

System Architecture-

                         +-----------------------+
                         |       Developer       |
                         |  (push code to main)  |
                         +-----------+-----------+
                                     |
                                     v
                         +-----------------------+
                         |     GitHub Actions    |
                         |  CI/CD Build & Deploy |
                         +-----------+-----------+
                                     |
                         SSH / SCP   |
                                     v
                         +-----------------------+
                         |         AWS EC2       |
                         |   Docker + App image  |
                         |  Running Flask API    |
                         +-----------------------+
                                     |
                                     v

Design Decisions-

Docker chosen for environment consistency

GitHub Actions for seamless automation

AWS EC2 for affordable scalability

No authentication for simplicity in prototype phase

Contact

Made by Anmol Bhonsle
DevOps Intern Candidate                        
