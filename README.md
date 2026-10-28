# Docker & Container Security — CA-2

## Student Details

| Field | Details |
|---|---|
| Name | Ayush Kumar Dubey |
| Registration Number | 2400290120081 |
| Application Port | 161 |
| Docker Hub Username | `dubey2501` |
| GitHub Repository | [ca2devops](https://github.com/AYUSHDUBEY24/ca2devops) |
| Docker Hub Repository | [fest-backend](https://hub.docker.com/r/dubey2501/fest-backend) |

## Project Overview

This project demonstrates containerization, Docker Compose networking, persistent storage, container image vulnerability scanning, image hardening, and image signing and verification.

### Technology Stack

- Docker and Docker Compose
- Python and Flask
- PostgreSQL
- Trivy for vulnerability scanning
- Cosign for container image signing and verification
- Git and GitHub for version control
- Docker Hub for image publishing

## Project Structure

```text
ca2devops/
├── backend/
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/
├── reports/
│   ├── trivy-before.txt
│   └── trivy-after.txt
├── screenshots/
├── compose.yaml
├── .env.example
├── .gitignore
├── cosign.pub
└── README.md
```

## Running the Application

### Prerequisites

- Docker Desktop
- Docker Compose
- Git

### Start the application

From the project directory, run:

```powershell
docker compose up -d --build
```

Check running containers:

```powershell
docker compose ps
```

Open the application at:

http://localhost:161

### Stop the application

```powershell
docker compose down
```

Persistent database data is configured through Docker volumes so that application data can survive container recreation.

## Container Security and Hardening

The backend image was hardened to reduce unnecessary packages, improve dependency security, and run the application with reduced privileges.

Security measures include:

- Using a slim Python base image.
- Running the backend as a non-root user.
- Updating Python packaging tools.
- Updating application dependencies to patched versions where applicable.
- Scanning the image with Trivy.
- Publishing the image to Docker Hub.
- Signing the published image with Cosign and verifying its signature.

## Trivy Vulnerability Scanning

Trivy was used to scan the backend image before and after hardening. The scan reports are available in the `reports/` directory.

### Recorded Scan Results

| Metric | Before | After |
|---|---:|---:|
| Image | `ca2devops-backend:latest` | `ca2devops-backend:0.2` |
| Base OS reported by Trivy | Debian 13.1 | Debian 12.15 |
| Total reported findings | 13,357 | 270 |
| CRITICAL findings | 234 | 2 |
| HIGH findings | 2,243 | 59 |
| Image disk usage | 1.61 GB | 232 MB |

**Important note:** These are the totals recorded in the saved Trivy reports. The before and after images use different Debian base versions, so the results are not a perfectly like-for-like comparison. Vulnerability database updates and the scanned package set can also affect totals.

A separate filtered scan was executed using:

```powershell
trivy image --severity CRITICAL --ignore-unfixed --exit-code 1 ca2devops-backend:0.2
```

The command completed with exit code `0` and reported no findings matching the selected CRITICAL severity and `--ignore-unfixed` filter. This does not mean that the image has no vulnerabilities of any severity.

### Image Size Reduction

The recorded disk usage decreased from 1.61 GB to 232 MB after hardening.

The hardened image is smaller and uses a slim base image, helping reduce the image footprint and unnecessary components.

## Docker Hub Image

Published repository:

https://hub.docker.com/r/dubey2501/fest-backend

Published tags include:

- `dubey2501/fest-backend:1.0`
- `dubey2501/fest-backend:1.0.0`

### Version 1.0.0 Digest

```text
sha256:648d7b401ab63c7b380c6b2998e7b0f9bebf59297dad0c97b5a1dba77812c6ee
```

The digest identifies the specific image manifest used for signing and verification.

## Image Signing and Verification with Cosign

Cosign was used to sign the published image by digest and verify its signature using the public key.

Public key file:

```text
cosign.pub
```

The private signing key, `cosign.key`, must remain private and must not be committed to GitHub.

Example verification command:

```powershell
cosign verify --key cosign.pub dubey2501/fest-backend:1.0.0
```

Verification should be performed against the same image reference and signature configuration used during signing. A successful verification confirms that the image signature matches the verification key.

## Docker Compose and Persistence

Docker Compose is used to manage the application services and their configuration.

The setup includes:

- Frontend service
- Backend service
- PostgreSQL database service
- Service networking
- Database persistence using a Docker volume
- Environment-based configuration

The application was tested on port `161`, and database persistence was checked by stopping and restarting the Compose deployment.

## AI Tools Used

**ChatGPT**

Used for:
- Understanding Docker and container security concepts.
- Troubleshooting Docker, Trivy, and Cosign commands.
- Reviewing vulnerability scan output.
- Improving documentation and README structure.
- Understanding image signing and verification workflows.

All commands and project changes should be checked against the actual local environment and assessment requirements.

## Screenshots and Reports

The `screenshots/` directory contains assessment evidence. The `reports/` directory contains the saved Trivy scan reports.

Screenshots should demonstrate the relevant application, container, scanning, Docker Hub, and signing/verification results completed during the assessment.

## Incomplete Tasks

Update this section before final submission.

- If every required task is complete, write: `None — all required tasks completed and verified.`
- If the unsigned `1.0.1` image was not built and pushed, or the expected Cosign verification rejection was not demonstrated, list that task here.
- List any other assessment requirement that remains incomplete.

## Security Notes

- Never commit private signing keys, passwords, or other secrets.
- Keep `cosign.key` excluded through `.gitignore`.
- Use `.env.example` as a configuration template and do not publish real credentials.
- Review vulnerability scan results and update affected packages where fixes are available.

## Conclusion

This project demonstrates a containerized application using Docker Compose, persistent database storage, Trivy vulnerability scanning, image hardening, Docker Hub publishing, and Cosign image signature verification. The saved reports and screenshots provide supporting evidence for the assessment.
