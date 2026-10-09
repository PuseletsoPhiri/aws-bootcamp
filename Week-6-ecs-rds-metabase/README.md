# Week 6 — Connecting ECS Services to RDS Securely

**Topic:** Configuring ECS Services to Communicate with RDS Databases Securely within a Private VPC
**Original session date:** July 12, 2025

## Goals
- [ ] Create an Amazon RDS PostgreSQL instance
- [ ] Deploy Metabase on Amazon ECS using the Fargate launch type (official Metabase Docker image)
- [ ] Configure the environment variables needed for database connectivity
- [ ] Place ECS and RDS in the same VPC
- [ ] Configure the RDS Security Group to allow inbound traffic on port 5432 from the ECS task
- [ ] Reach the Metabase setup screen confirming a successful database connection

## What I did
Created an RDS PostgreSQL instance and deployed Metabase on ECS using
the Fargate launch type, with the official Metabase image from Docker
Hub. Both resources were placed in the same VPC so they could
communicate privately.

Configured the Metabase task definition with the environment
variables it needs to connect to PostgreSQL:

| Variable | Purpose |
|----------|---------|
| `MB_DB_TYPE` | (`postgres`) |
| `MB_DB_DBNAME` | metabase |
| `MB_DB_PORT` | (`5432`) |
| `MB_DB_USER` | postgres |
| `MB_DB_PASS` | ******** |
| `MB_DB_HOST` |  |

Set the RDS Security Group's inbound rule to allow PostgreSQL traffic
on port `5432`, with the **ECS task's Security Group as the source**
(rather than a fixed IP address), so only the ECS task can reach the
database.

Final outcome: once the service was running, I reached the Metabase setup screen and confirmed a successful connection to the RDS PostgreSQL database

## Issues encountered
- The ECS service remained in a deployment-in-progress state for an
  extended period and did not reach a stable running state. The
  service showed as **Active**, but with no tasks running or pending.
- Verified that the service's desired task count was already set to
  1, which ruled out a misconfigured desired count as the cause.
- Created a separate cluster to rule out a cluster-specific problem.
  The same behaviour occurred: the load balancer, listener, target
  group, and security group all reached `CREATE_COMPLETE`, while the
  ECS service remained at `CREATE_IN_PROGRESS`.
- Investigated the service events, deployments, stopped task
  reasons, service networking configuration (VPC, subnets, public IP
  assignment, security groups), and the target group health check
  settings (path and port) as possible causes.
- **Root cause:** a problem with the ECS **task execution role**. This
  is the role ECS uses to pull the container image and perform other
  setup actions on behalf of the task, so a faulty role prevents the
  task from launching at all.
- **Resolution:** re-created the task execution role and redeployed
  the service. The service then started running as expected.

## Key takeaways
- An ECS service behind a load balancer will not reach a stable
  state until its tasks pass the target group's health checks, so a
  wrong health check path or port can leave a service stuck in
  progress.
- The task execution role is separate from the task role: ECS needs
  it to pull images and ship logs before the container even starts,
  so if it is missing or misconfigured the service can sit in
  progress with no tasks running. This is worth checking early when
  a service has no running or pending tasks.
- When troubleshooting a stuck service, the Events tab, Deployments
  tab, and the Stopped reason on a stopped task are the most useful
  places to look first.
- Referencing a Security Group (rather than an IP address) as the
  source in the RDS inbound rule keeps database access limited to the
  ECS tasks, even when task IPs change.
- Metabase can take a couple of minutes to start, which matters when
  health check timing is strict.

## Screenshots / Evidence
**RDS PostgreSQL instance details**
<img width="1896" height="961" alt="Screenshot 2026-10-08 094455" src="https://github.com/user-attachments/assets/c109c2fd-e3df-438a-855d-d51271adac59" />

**ECS running service**
<img width="1911" height="945" alt="Screenshot 2026-10-08 093550" src="https://github.com/user-attachments/assets/0cfabfd0-dce6-443b-85b2-d4a2ef6c45a6" />


**Security group rules allowing ECS to RDS on port 5432**
<img width="1907" height="955" alt="Screenshot 2026-10-08 093629" src="https://github.com/user-attachments/assets/be1b8753-8841-412d-a4ad-22af09524774" />


**Metabase setup screen confirming database connection**
<img width="1908" height="967" alt="Screenshot 2026-10-08 101239" src="https://github.com/user-attachments/assets/c43d966a-2338-49a0-b5d5-088e2cf60301" />
