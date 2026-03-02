
  ## Project Overview

  This project deploys **Outline** which is a collaborative knowledge base platform, on **AWS ECS Fargate** using **Terraform**.  

- Containerised deployment of the Outline knowledge base web page using Docker Serverless execution on AWS ECS Fargate
- Secure, private infrastructure with VPC, private subnets, and isolated ECS tasks
- Public-facing Application Load Balancer (ALB) handling HTTPS and routing
- Persistent storage via Amazon RDS (PostgreSQL) and caching via Redis
- Infrastructure as code using modular Terraform (VPC, ECS, ALB, RDS, Redis, ACM, SG)
- Automated CI/CD pipeline using GitHub Actions for Docker builds and Terraform deployment
- Domain, DNS, and security protection managed through Cloudflare



## Folder Structure

```text
TECH-TEST/
├── app/
│   ├── app/               
│   ├── plugins/          
│   ├── public/           
│   ├── server/           
│   ├── shared/           
│   ├── Dockerfile         
│   ├── package.json       
│   ├── tsconfig.json     
│   ├── vite.config.ts     
│   └── yarn.lock
│
│   
├── terraform/
│   ├── backend.tf
│   ├── main.tf
│   ├── provider.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── bootstrap/
│   └── modules/
│       ├── acm/
│       ├── alb/
│       ├── ecs/
│       ├── rds/
│       ├── redis/
│       ├── s3/
│       ├── sg/
│       └── vpc/
│
└── .github/
    └── workflows/
        ├── deploy.yml
        ├── terraform.yml
        └── destroy.yml

```

## Infrastructure
- Built entirely with Terraform following a fully modular structure.
- Bootstrap module: Sets up the Terraform backend by creating the S3 state bucket, DynamoDB lock table, and ECR repository required before deploying the main infrastructure.
- VPC module provisions networking: public/private subnets, routing, IGW, NAT Gateways.
- ECS module deploys the application on ECS Fargate with secure private subnets.
- ALB module creates an Application Load Balancer for HTTPS traffic routing.
- ACM module manages SSL certificates used by the ALB.
- RDS module provisions the PostgreSQL database for Outline.
- Redis module sets up ElastiCache Redis for caching and sessions.
- Security Groups module defines strict access control between ALB, ECS, RDS, and Redis.
- S3 module enables object storage for uploads or application assets.
- IAM module provides all required roles and policies for ECS, Terraform, and deployment.


## Application

- Docker file builds the outline App into a container image for ECS Fargate
- app/ contains the core front-end and UI logic for the Outline knowledge base.
- server/ includes backend APIs, authentication, routing, and business logic.
- shared/ holds utility functions, reusable logic, and shared TypeScript modules.
- plugins/ extends Outline with integrations, extra features, and optional modules.
- public/ contains static assets served by the application (icons, images, CSS).
- package.json defines app dependencies and scripts for building or running locally.
- tsconfig.json & vite.config.ts configure TypeScript and the build system.

## Deployment
- Build & Push Image: Builds the Outline Docker image, tags it with the commit SHA, and pushes it to Amazon ECR.
- Terraform Deploy: Runs init → plan → apply to provision or update all AWS infrastructure 
- Terraform Destroy: Manual workflow that safely tears down all infrastructure with terraform destroy.



## Further improvements
- VPC Endpoints
- secrets manager
- Waf Integration 
