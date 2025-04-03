# Jenkins Kubernetes Policy Pipeline

This repository demonstrates a Jenkins pipeline that performs the following:

1. **Scans a Docker image using Trivy**
2. **Validates a Kubernetes manifest using Kyverno**
3. **Applies the manifest if policies are met**
4. **Fails the build if vulnerabilities or policy violations are found**

## Repository Structure

```
.
├── Jenkinsfile
└── manifests/
    ├── deployment.yaml
    └── kyverno-policy.yaml
```

## Jenkinsfile Stages

- **Trivy Scan**: Scans `adidix/jenkins-agent-k8s:arm64` for high/critical vulnerabilities.
- **Kyverno Validation**: Applies a Kyverno policy requiring CPU and memory limits on containers.
- **Deployment**: If all checks pass, deploys the manifest to your Kubernetes cluster.

## Requirements

- Jenkins with Docker, Kyverno CLI, Kubectl, and Trivy installed on the agent.
- Kubernetes cluster access configured via `kubectl` on the Jenkins node.

## Usage

1. Clone this repository.
2. Connect your Jenkins project to this repo using “Pipeline script from SCM”.
3. Watch the pipeline run Trivy + Kyverno before deploying the workload.

## Credits

Built for DevSecOps compliance demos using Trivy, Kyverno, and Jenkins.
