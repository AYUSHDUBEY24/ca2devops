# FestPass — Docker & Container Security Assessment

## Student Details

- **Name:** Ayush Kumar Dubey
- **Registration Number:** 2400290120081
- **Port:** 161
- **Docker Hub Username:** dubey2501
- **Docker Hub Repository:** https://hub.docker.com/r/dubey2501/fest-backend

## Project Overview

FestPass is a three-tier registration application containerized using Docker Compose. It consists of an Nginx frontend, a Flask backend API, and a PostgreSQL database.

The stack uses separate networks to isolate the database from the frontend, a named volume for persistent registration data, Trivy for vulnerability scanning, and Cosign for container image signing and verification.

## Technology Stack

- Docker and Docker Compose
- Python, Flask and PostgreSQL
- Nginx
- Trivy vulnerability scanner
- Docker Hub
- Cosign image signing and verification

## Run the Application

1. Clone this repository.
2. Create a local `.env` file using `.env.example` and set the required values.
3. Start the services:

   ```powershell
   docker compose up -d --build
   ```

4. Open `http://localhost:161`.
5. Check the backend health endpoint at `http://localhost:161/api/health`.

Do not commit `.env` or private signing keys.

## Docker Hub Image

- **Image:** `dubey2501/fest-backend`
- **Version tags:** `1.0.0`, `1.0`
- **Image digest:** `sha256:648d7b401ab63c7b380c6b2998e7b0f9bebf59297dad0c97b5a1dba77812c6ee`

## Trivy Vulnerability Scan

The initial backend image was scanned, then the image was hardened by updating the Python base image and dependencies and running the application as a non-root user.

| Metric | Before (`0.1`) | After (`0.2`) |
|---|---:|---:|
| CRITICAL vulnerabilities | Fill from report | Fill from report |
| HIGH vulnerabilities | Fill from report | Fill from report |
| Image size | Fill from Docker output | Fill from Docker output |

Reports are available in `reports/trivy-before.txt` and `reports/trivy-after.txt`.

**Note:** The updated image still has reported vulnerabilities. The scan must not be described as completely vulnerability-free.

## Image Signing

Cosign was used to sign the `1.0.0` image by digest and verify its signature using `cosign.pub`. The private key `cosign.key` is excluded from Git.

## AI Tools Used

- **ChatGPT:** Used for understanding the assessment requirements, troubleshooting Docker and Git commands, interpreting Trivy results, and preparing project documentation. Commands were executed and tested locally.

## Incomplete Tasks

List any assessment tasks that could not be completed, one per line. If all tasks are complete, write “None”.
