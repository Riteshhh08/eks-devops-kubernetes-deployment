# EKS DevOps Production-Grade Deployment

This repository is my end-to-end DevOps deployment project for running a containerized application on Amazon EKS with a simple CI/CD flow built around GitHub, AWS CodePipeline, AWS CodeBuild, Amazon ECR, and Kubernetes manifests.

What started as a learning exercise was cleaned up and improved into a more production-minded setup. I focused on deployment reliability, cleaner manifests, safer rollout behavior, and reducing manual work across build and deploy stages.

## About the Project

The project deploys a small NGINX-based application to Amazon EKS through an automated pipeline:

- Source code is stored in GitHub
- CodeBuild builds the Docker image and pushes it to Amazon ECR
- A deploy stage updates the Kubernetes deployment with the new image
- Kubernetes serves the app through a Service and ALB Ingress
- External DNS integration can be used for Route53-based DNS automation

This repo is intentionally small, but the workflow reflects the kind of practical DevOps work I care about: repeatable delivery, stable deployments, readable infrastructure, and fast troubleshooting.

## Architecture Overview

`GitHub -> CodePipeline -> CodeBuild (build) -> Amazon ECR -> CodeBuild (deploy) -> Amazon EKS -> Service -> ALB Ingress -> Route53 / ExternalDNS`

Environment-specific values such as ECR URI, EKS cluster name, IAM role ARN, ACM certificate ARN, and DNS hostname are left as clear placeholders so the project can be reused cleanly.

## Tech Stack

- AWS CodePipeline
- AWS CodeBuild
- Amazon ECR
- Amazon EKS
- Kubernetes
- Docker
- NGINX
- IAM / STS AssumeRole
- ALB Ingress Controller
- ExternalDNS

## CI/CD Workflow

1. A change is pushed to GitHub.
2. CodeBuild creates a Docker image and tags it from the commit SHA.
3. The image is pushed to Amazon ECR.
4. The deploy stage injects the new image reference into the Kubernetes deployment manifest.
5. `kubectl apply` updates the workload in EKS and waits for rollout completion.

This keeps the delivery path simple and traceable while still covering the important parts of a real deployment pipeline.

## Kubernetes / Deployment Improvements

I rewrote the manifests to make them more production-ready and easier to maintain:

- added CPU and memory requests/limits
- added readiness and liveness probes for better rollout safety
- improved rolling update behavior for more stable deployments
- cleaned up the Service definition and named ports clearly
- removed tutorial-specific domains, certificate values, and sample branding
- simplified Ingress configuration so environment-specific values are easy to replace
- kept the manifests readable and easy to debug during pipeline runs

## AI-Assisted Engineering Workflow

I used AI tools in a practical way while building and refining this project, mainly to move faster on repetitive work and troubleshoot more efficiently:

- GitHub Copilot for Terraform, Bash, Python, and general implementation assistance
- Claude for architecture review, debugging dead ends, and sharpening technical decisions
- ChatGPT for documentation, runbooks, incident notes, and explanation support
- Cursor when refactoring larger sections or cleaning up structure
- Amazon Q for AWS-specific troubleshooting and service guidance
- K8sGPT to surface Kubernetes issues faster during debugging

These tools were used as accelerators, not replacements for engineering judgment. They helped reduce setup effort, improve clarity, and save time while keeping the project low-cost and practical to build.

## Engineering Approach

I treated this as a hands-on implementation project rather than a tutorial copy. The work was iterative: build the pipeline, validate the deploy path, tighten the Kubernetes manifests, debug what broke, and keep simplifying anything that felt too manual or fragile.

The main priorities were:

- make deployments more reliable
- keep the repo easy to understand
- reduce repetitive manual steps
- stay mindful of cloud cost while testing
- learn by building and improving the real workflow

## Outcome / What I Learned

- how to connect GitHub, CodeBuild, ECR, and EKS into a usable delivery pipeline
- how small Kubernetes changes like probes, resources, and rollout settings improve reliability
- how IAM role assumptions and deployment permissions affect secure cluster automation
- how AI tools can speed up debugging, documentation, and refinement without replacing hands-on validation

## Author

**Ritesh Vishwakarma**  
DevOps & Cloud Engineer with hands-on experience across AWS and Azure, focused on CI/CD, infrastructure automation, cloud-native deployments, reliability, and practical AI-assisted engineering workflows.

- LinkedIn: [ritesh-vishwakarma08](https://www.linkedin.com/in/ritesh-vishwakarma08/)
- GitHub: [Ritesh](https://github.com/Ritesh)
- Email: [riteshvishwakarma.work@gmail.com](mailto:riteshvishwakarma.work@gmail.com)
