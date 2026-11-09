# Jenkins Lab 23 — CI/CD and GitOps with Argo CD

This project builds, tests, scans, and publishes the iVolve Node.js application. Jenkins then updates the image tag in `k8s/deployment.yaml` and pushes that commit to the watched Git branch. Argo CD detects the Git change and reconciles Kubernetes; Jenkins does not apply the application manifests directly.

## Project layout

- `agent/Dockerfile`: Jenkins agent image with the required tools.
- `vars/BuildApp.groovy`: installs dependencies, runs tests, and checks syntax.
- `vars/BuildImage.groovy`: builds, scans, pushes, and cleans the Docker image.
- `vars/DeployOnK8s.groovy`: updates the deployment image tag and pushes the manifest commit for Argo CD.
- `k8s/`: Kubernetes manifests watched by Argo CD.
- `test/`: application tests.

## Pipeline flow

Checkout → Build App → Build Image, Scan and Push → Update Manifest for Argo CD

## Jenkins requirements

- Global shared library name: `ivolve-lab22`, configured to load `vars/` from this project repository.
- Agent label: `lab22-agent`.
- Docker credential ID: `docker`.
- GitHub username/password credential ID: `github-write-creds`; the token needs contents read/write on the watched repository.
- The agent needs Git, Node.js/npm, Docker CLI connected to a working daemon, and Trivy.

The pipeline pushes `HEAD` to `main` on the configured `origin` remote. Keep the project files, including `Jenkinsfile`, `vars/`, and `k8s/`, at the repository root so the Argo CD path remains `k8s`. Ensure Jenkins and Argo CD use the same repository and branch. Credentials are read from Jenkins Credentials and must never be committed or printed.

## Argo CD settings

Configure the `ivolve` Application to watch the repository's `main` branch and `k8s` path, targeting the `ivolve` namespace, with automatic sync enabled. After Jenkins pushes a manifest commit, verify the Application becomes **Synced** and **Healthy**.