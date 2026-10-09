# Week 7 — Monitoring ECS Workloads with Amazon CloudWatch

**Topic:** Monitoring, Logging, and Auditing AWS Resources Using Amazon CloudWatch and AWS CloudTrail
**Original session date:** July 19, 2025

## Goals
- [x] Deploy a simple containerized application (Nginx or `grafana/grafana`) on Amazon ECS with Fargate
- [x] Set appropriate CPU and memory settings in the task definition
- [x] Run the service in a public subnet
- [x] Create a CloudWatch dashboard with `CPUUtilization` and `MemoryUtilization` widgets
- [ ] (Optional) Simulate load and observe how the metrics change

## What I did
Deployed a containerized application on an ECS cluster using the
**Fargate** launch type, running it as a service in a public subnet.
In the task definition I set explicit CPU and memory values for the
task:

| Setting | Value used |
|---------|-----------|
| Application / image | (`grafana/grafana`) |
| Task CPU | (1024 1vCPU) |
| Task memory | (3072 3GiB) |

Once the service was running, I created a **CloudWatch dashboard**
and added widgets for the two ECS service metrics from the `AWS/ECS`
namespace:
- `CPUUtilization`
- `MemoryUtilization`

These widgets let me watch the task's resource usage in near real
time from a single view.

I stimulated the load by refreshing the app

## Key takeaways
- Fargate requires you to choose CPU and memory at the task level,
  and only certain combinations are valid, so sizing is a decision
  you make up front rather than something tied to an instance type.
- ECS publishes `CPUUtilization` and `MemoryUtilization` to
  CloudWatch automatically for each service, with no extra agent or
  setup. Both are shown as a percentage of what the task definition
  reserved, so they only make sense relative to the CPU and memory
  you configured.
- A dashboard turns individual metrics into one view you can read
  at a glance, which is much quicker for spotting a spike than
  opening each metric separately.
- Setting CPU and memory too low risks throttling or the container
  being stopped, while setting them too high wastes money, which is
  why monitoring is useful for right-sizing.

## Screenshots / Evidence
**ECS cluster and running service**
<img width="1907" height="940" alt="Screenshot 2026-10-09 102909" src="https://github.com/user-attachments/assets/93858da2-3342-4048-a88e-48945d4ff9eb" />

**Task definition with CPU and memory settings**
<img width="1892" height="951" alt="Screenshot 2026-10-09 102935" src="https://github.com/user-attachments/assets/3ef4448d-9f8b-494d-9e0c-df05743581d7" />

**CloudWatch dashboard with CPU and memory widgets**
<img width="1916" height="956" alt="Screenshot 2026-10-09 105147" src="https://github.com/user-attachments/assets/7b4f75fa-cb10-4bad-a9a0-f0136162dc62" />

**(Optional) Metrics under simulated load**
<img width="1907" height="952" alt="Screenshot 2026-10-09 105732" src="https://github.com/user-attachments/assets/1d5a7913-d7f9-409b-b594-eaa55819be8f" />
