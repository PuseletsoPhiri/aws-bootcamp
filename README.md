# AWS Cloud Bootcamp

A week-by-week log of my hands-on work through the **CSN (CloudSec Network) AWS Cloud Bootcamp** curriculum, originally run June to August 2025. I'm completing the weekly tasks on my own and documenting each one here: what I built, what went wrong, and what I learned.

Each week has its own folder with a README covering the task, how I did it, issues I ran into, key takeaways, and screenshots as evidence.

## Progress

| Week | Topic | Status | Notes |
|------|-------|--------|-------|
| 1 and 2 | Cloud Computing and AWS Fundamentals, IAM (users, groups, roles and policies) | ✅ Done | [Weeks 1 and 2](Week-1&2-cloud-fundamentals&iam) |
| 3 | EC2 and Security Groups (Windows Server, RDP) | ✅ Done | [Week 3](Week-3-ec2-security-groups) |
| 4 | VPCs, subnets and VPC peering | ✅ Done | [Week 4](Week-4-vpc-peering) |
| 5 | ECS with Fargate (Grafana) | ✅ Done | [Week 5](Week-5-ecs-grafana) |
| 6 | ECS connected to RDS PostgreSQL (Metabase) | ✅ Done | [Week 6](Week-6-ecs-rds-metabase) |
| 7 | CloudWatch monitoring of an ECS service | ✅ Done | [Week 7](Week-7-cloudwatch-monitoring) |
| 8 | To be added | ⬜ Started | |
| 9 | To be added | ⬜ Not started | |
| 10 | To be added | ⬜ Not started | |
| 11 | Revision and Q&A | ➖ No task | Session only, no hands-on task |
| 12 | Bootcamp recap, career readiness and AMA | ➖ No task | Session only, no hands-on task |

Still ahead: S3 and CloudFront, Route 53 and ACM, and Lambda (Weeks 8 to 10). Weeks 11 and 12 were revision and Q&A sessions with no tasks, so they have no folders in this repo.

## Skills practised so far
- **Identity and access:** IAM users, groups and managed policies
- **Compute:** launching and securing EC2 instances, connecting through RDP and Fleet Manager
- **Networking:** VPCs, public and private subnets, route tables, VPC peering
- **Containers:** ECS with Fargate, task definitions, services, load balancers and target groups
- **Databases:** RDS PostgreSQL, security group rules between ECS and RDS
- **Monitoring:** CloudWatch dashboards for CPU and memory metrics
- **Troubleshooting:** diagnosing stuck ECS deployments, task execution roles, and health checks

## How the repo is organised
Each week has its own folder holding a README with the write-up, the weekly task sheet, and the screenshots used as evidence. Weeks 1 and 2 share one folder.

```
aws-bootcamp/
├── README.md
├── Week-1-2-cloud-fundamentals-iam/
│   ├── README.md
│   └── Screenshot-1.png ... Screenshot-6.png
├── Week-3-ec2-security-groups/
│   ├── README.md
│   └── AWS_Bootcamp_Week_3_Task.png
├── Week-4-vpc-peering/
│   ├── README.md
│   └── Week-4-Task.png
├── Week-5-ecs-grafana/
│   ├── README.md
│   └── Week-5-Task.png
├── Week-6-ecs-rds-metabase/
│   ├── README.md
│   └── Week-6-Task.png
└── Week-7-cloudwatch-monitoring/
    └── README.md
```

## Cost control
After documenting each task, I delete the resources I created so the account stays within AWS Free Tier limits and avoids unexpected charges. Each week's README lists what was removed.

## About
I'm Puseletso Phiri, an IT graduate from South Africa building skills in AWS Cloud.

- GitHub: [PuseletsoPhiri](https://github.com/PuseletsoPhiri)
- Email: khalabiphiri@protonmail.com

## Acknowledgements
Curriculum and weekly tasks by [CloudSec Network (CSN)](https://www.cloudsecnetwork.com). Additional learning resources: [Learn to Cloud](https://learntocloud.guide), the [AWS Skills Center](https://aws.amazon.com/training/skills-center/), and [NextWork](https://www.nextwork.org).
