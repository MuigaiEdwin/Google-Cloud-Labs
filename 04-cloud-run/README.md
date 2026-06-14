# Lab 04 — Hello Cloud Run

## Overview
Built a simple Node.js application, containerized it, pushed the image
to Artifact Registry, and deployed it to Cloud Run as a serverless
stateless container accessible via HTTPS.

## What I Did

### Task 1 — Enabled APIs & Configured Environment
- Enabled Cloud Run and Artifact Registry APIs via Cloud Shell
- Set compute region and LOCATION environment variable

```bash
gcloud services enable run.googleapis.com artifactregistry.googleapis.com
gcloud config set compute/region us-east4
LOCATION="us-east4"
```

### Task 2 — Wrote the Sample Application
- Created a Node.js Express app that responds to HTTP requests
- App listens on `PORT` environment variable (default 8080)
- Created `package.json` and `index.js`

### Task 3 — Created Artifact Registry Repository
- Created a Docker repository `my-repository` in Artifact Registry
- Configured Docker to authenticate with Artifact Registry

```bash
gcloud artifacts repositories create my-repository \
    --repository-format=docker \
    --location=$LOCATION \
    --description="Docker repository"

gcloud auth configure-docker $LOCATION-docker.pkg.dev
```

### Task 4 — Containerized App and Pushed to Artifact Registry
- Created a `Dockerfile` using `node:20-slim` base image
- Built and pushed container image using Cloud Build in one command
- Tested locally via Cloud Shell Web Preview on port 8080

```bash
gcloud builds submit \
  --tag $LOCATION-docker.pkg.dev/$GOOGLE_CLOUD_PROJECT/my-repository/helloworld

docker run -d -p 8080:8080 \
  $LOCATION-docker.pkg.dev/$GOOGLE_CLOUD_PROJECT/my-repository/helloworld
```

### Task 5 — Deployed to Cloud Run
- Deployed containerized app to Cloud Run with public access
- Cloud Run automatically scales up on demand, scales to zero when idle
- App served via secure HTTPS URL

```bash
gcloud run deploy helloworld \
    --image $LOCATION-docker.pkg.dev/$GOOGLE_CLOUD_PROJECT/my-repository/helloworld \
    --allow-unauthenticated \
    --region=$LOCATION
```

### Task 6 — Cleaned Up
- Deleted container image from Artifact Registry
- Deleted Cloud Run service to avoid charges

```bash
gcloud artifacts docker images delete \
  $LOCATION-docker.pkg.dev/$GOOGLE_CLOUD_PROJECT/my-repository/helloworld

gcloud run services delete helloworld --region=us-east4
```

## Key Concepts Learned
- **Cloud Run** — serverless container platform, no infrastructure
  to manage *(similar to AWS Fargate / App Runner)*
- **Artifact Registry** — managed container image registry
  *(equivalent to AWS ECR — Elastic Container Registry)*
- **Cloud Build** — builds and pushes container images in one command
  *(similar to AWS CodeBuild)*
- **Serverless scaling** — scales to zero when idle, scales up
  automatically on demand — you only pay per request
- **`--allow-unauthenticated`** — makes the service publicly accessible
  without requiring auth tokens

## GCP → AWS Equivalent

| GCP Service | AWS Equivalent |
|-------------|----------------|
| Cloud Run | AWS Fargate / App Runner |
| Artifact Registry | ECR (Elastic Container Registry) |
| Cloud Build | AWS CodeBuild |
| Cloud Shell | AWS CloudShell |
| Cloud Run URL (HTTPS) | ALB / API Gateway endpoint |

## Result
✅ Node.js app containerized, pushed to Artifact Registry, and deployed
to Cloud Run — live on a public HTTPS URL with zero infrastructure
management.
