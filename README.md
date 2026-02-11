# Go Service Pipeline

A lightweight Go web service with a complete CI/CD pipeline for automated Docker image building and deployment.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [Local Development](#local-development)
  - [Docker](#docker)
- [CI/CD Pipeline](#cicd-pipeline)
- [Project Structure](#project-structure)
- [API Endpoints](#api-endpoints)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This project demonstrates a production-ready Go web service with automated CI/CD workflows. It showcases modern DevOps practices including containerization with Docker, multi-stage builds for optimized image sizes, and GitHub Actions for continuous integration and deployment.

The service provides a simple HTTP server that responds with a greeting message, serving as a foundation for more complex microservices.

## ✨ Features

- **Lightweight HTTP Server**: Built with Go's standard `net/http` package
- **Docker Support**: Multi-stage Dockerfile for minimal image size
- **CI/CD Automation**: GitHub Actions workflow for automated testing and deployment
- **Container Registry Integration**: Automatic Docker Hub image publishing
- **Production Ready**: Follows Go and Docker best practices

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- **Go**: Version 1.22 or higher ([Download](https://golang.org/dl/))
- **Docker**: Latest version ([Download](https://www.docker.com/get-started))
- **Git**: For version control

## 🚀 Getting Started

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd go_service_pipeline
   ```

2. **Install dependencies**
   ```bash
   go mod download
   ```

3. **Run the application**
   ```bash
   go run main.go
   ```

4. **Test the service**
   ```bash
   curl http://localhost:8080
   ```
   
   Expected response:
   ```
   Hello from Go + Docker
   ```

### Docker

#### Build the Docker image locally

```bash
docker build -t go-service-pipeline:local .
```

#### Run the container

```bash
docker run -p 8080:8080 go-service-pipeline:local
```

The service will be available at `http://localhost:8080`

#### Stop the container

```bash
docker stop $(docker ps -q --filter ancestor=go-service-pipeline:local)
```

## 🔄 CI/CD Pipeline

This project uses GitHub Actions for continuous integration and deployment. The pipeline is triggered on every push to the `main` branch.

### Pipeline Stages

1. **Checkout Code**: Retrieves the latest code from the repository
2. **Setup Go Environment**: Configures Go 1.22
3. **Run Tests**: Executes all test suites with `go test ./...`
4. **Docker Hub Login**: Authenticates with Docker Hub
5. **Build & Push**: Creates Docker image and pushes to Docker Hub

### Required Secrets

Configure the following secrets in your GitHub repository settings:

- `DOCKERHUB_USERNAME`: Your Docker Hub username
- `DOCKERHUB_TOKEN`: Your Docker Hub access token ([Create one](https://hub.docker.com/settings/security))

**Note**: The workflow file contains a typo in the secret name (`DOCKERHUB_USERNMAE` should be `DOCKERHUB_USERNAME`). Make sure to set up the secret with the correct spelling.

### Workflow File

The CI/CD workflow is defined in `.github/workflows/ci-cd.yml`

## 📁 Project Structure

```
go_service_pipeline/
├── .github/
│   └── workflows/
│       └── ci-cd.yml          # CI/CD pipeline configuration
├── main.go                     # Main application entry point
├── go.mod                      # Go module dependencies
├── Dockerfile                  # Multi-stage Docker build
└── README.md                   # Project documentation
```

## 🔌 API Endpoints

| Method | Endpoint | Description | Response |
|--------|----------|-------------|----------|
| GET | `/` | Health check and greeting | `Hello from Go + Docker` |

## ⚙️ Configuration

### Application Configuration

The service runs on port `8080` by default. To change the port, modify the `main.go` file:

```go
log.Fatal(http.ListenAndServe(":YOUR_PORT", nil))
```

### Docker Configuration

The Dockerfile uses a multi-stage build:
- **Builder Stage**: Uses `golang:1.25-alpine` for compiling
- **Runtime Stage**: Uses `alpine:latest` for minimal footprint

## 🌐 Deployment

### Manual Deployment

Pull and run the latest image from Docker Hub:

```bash
docker pull <your-dockerhub-username>/go-docker-demo:latest
docker run -d -p 8080:8080 <your-dockerhub-username>/go-docker-demo:latest
```

### Kubernetes Deployment (Example)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: go-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: go-service
  template:
    metadata:
      labels:
        app: go-service
    spec:
      containers:
      - name: go-service
        image: <your-dockerhub-username>/go-docker-demo:latest
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: go-service
spec:
  selector:
    app: go-service
  ports:
  - port: 80
    targetPort: 8080
  type: LoadBalancer
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is open-source and available under the [MIT License](LICENSE).

---

## 🐛 Known Issues

- The CI/CD workflow has a typo: `DOCKERHUB_USERNMAE` should be `DOCKERHUB_USERNAME` in the login step

## 📧 Contact

For questions or support, please open an issue in the repository.

---

**Built with ❤️ using Go and Docker**

