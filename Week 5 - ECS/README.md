# Week 5 — Deploying Containerized Applications with Amazon ECS

**Topic:** Deploying Containerized Applications Using Amazon ECS, Load Balancers, and Task Definitions
**Original session date:** July 12, 2025

## Goals
- [x] Deploy Grafana using Amazon ECS with Fargate
- [x] Create a task definition using the official `grafana/grafana` Docker image
- [x] Expose port 3000 in the task definition
- [x] Run the task/service in a public subnet
- [x] Configure the Security Group to allow inbound traffic on port 3000
- [x] Access the Grafana login page via `http://<PUBLIC-IP>:3000`

## What I did
Created an ECS cluster using the **Fargate** launch type (serverless —
no EC2 instances to manage). Defined a task definition using the
official `grafana/grafana` Docker image, exposing container port
`3000` since that's the default port Grafana listens on.

Ran the task as an ECS **service** in a public subnet, so it would get
a public IP directly (no load balancer was required for this task,
since the deliverable just asked for direct access via public IP).

Updated the Security Group attached to the task/service to allow
inbound traffic on port **3000**, matching the exposed container port.

Once the service reached a running state, grabbed the task's public
IP from the ECS console and opened `http://<PUBLIC-IP>:3000` in the
browser — this loaded the Grafana login page. Logged in using the
default credentials (`admin` / `admin`).

## Issues encountered
-The ECS task initially remained in a PENDING state longer than expected before transitioning to RUNNING. 
-This was resolved by allowing additional time for the Fargate task to provision and pull the Docker image.
-The Security Group was not initially configured with an inbound rule for port 3000, which prevented the Grafana login page from loading in the browser. 
-This was resolved by adding the correct inbound rule for port 3000.
-The Fargate task did not initially have a public IP assigned, which meant it could not be reached directly from the browser. This was resolved by enabling "Auto-assign public IP" when configuring the service/task in the public subnet.
-On first login with the default credentials (admin/admin), Grafana prompted for the password to be changed before granting access to the dashboard.

## Key takeaways
- Fargate removes the need to provision or manage EC2 instances for
  containers — you just define CPU/memory in the task definition and
  AWS handles the underlying compute.
- The container's exposed port (in the task definition) and the
  Security Group's inbound rule have to match — missing either one
  means the app runs but isn't reachable from a browser.
- Fargate tasks need "Auto-assign public IP" enabled (when running in
  a public subnet with no load balancer) in order to be reachable
  directly via a public IP.
- Grafana ships with default `admin`/`admin` credentials, which is a
  good reminder that default credentials should always be changed
  immediately in anything beyond a lab/test environment.

## Screenshots / Evidence
**ECS cluster and running service**
<img width="1908" height="965" alt="Screenshot 2026-08-02 165236" src="https://github.com/user-attachments/assets/37fdee7a-f975-45bd-a204-786e51d17a64" />

**Task definition using the grafana/grafana image**
<img width="1910" height="968" alt="Screenshot 2026-08-02 165715" src="https://github.com/user-attachments/assets/c1c52482-c314-4109-b073-b530665371c6" />

**Security Group rule allowing port 3000**
<img width="1915" height="968" alt="Screenshot 2026-08-02 171400" src="https://github.com/user-attachments/assets/6d6b5689-21c6-4a4b-a6cf-06318339311b" />

**Grafana login page in browser**
<img width="1913" height="947" alt="Screenshot 2026-08-02 165006" src="https://github.com/user-attachments/assets/7d5e6545-ad8b-4ec3-92a7-030e22bdeeae" />
<img width="1905" height="962" alt="Screenshot 2026-08-02 165141" src="https://github.com/user-attachments/assets/ef313967-5e68-420f-ba08-2db227ef7d77" />

