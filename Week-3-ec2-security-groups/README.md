Week 3 — Provisioning and Securing EC2 Instances

Topic: Provisioning and Securing Amazon EC2 Instances and Security Groups for Compute Workloads
Original session date: June 21, 2025

Goals
- Launch a Windows Server EC2 instance (Amazon Windows Server 2019 Base AMI or later)
- Deploy the instance in a public subnet
- Create a Security Group allowing RDP (port 3389) only from my public IP
- Tag the instance with Name = `CSN-Bootcamp-Week3`
- Connect to the instance remotely

What I did
Launched a new EC2 instance using the Windows Server 2025 Base AMI,
placed it in a public subnet, and tagged it with `Name: CSN-Bootcamp-Week3`
as required.

Created a Security Group with an inbound rule allowing RDP (port 3389)
restricted to my own public IP address only, rather than opening it to
0.0.0.0/0 — this keeps the instance from being exposed to the wider
internet.

Instead of using a traditional RDP client, I connected using
EC2 Instance Connect / Fleet Manager (Systems Manager) via the AWS
Management Console — this gave me a remote desktop session to the
Windows instance directly in the browser, without needing to open a
separate RDP client or manage a `.rdp` file locally.

Issues encountered
- When creating the Security Group, got an error that a security
group with the same name already existed, resolved by assigning a different,
more specific name for this week's group.

Key takeaways
- Restricting RDP access to a single IP is a basic but important
  security practice — leaving port 3389 open to the world is a very
  common real-world misconfiguration and a frequent attack vector.
- Fleet Manager (via AWS Systems Manager) is a useful alternative to
  traditional RDP — it doesn't require the instance to have a
  publicly reachable RDP port at all, since the connection goes
  through the SSM agent instead. This could actually be a more secure
  option than opening port 3389, since no inbound RDP rule would be
  needed if using Fleet Manager exclusively.
- Tags like `Name` aren't just cosmetic — they make it much easier to
  identify resources at a glance in the console, especially as the
  number of resources grows.

Screenshots / Evidence
Running Windows EC2 instance, tagged CSN-Bootcamp-Week3
<img width="1912" height="960" alt="RunningInstance_Screenshot" src="https://github.com/user-attachments/assets/e24397f4-1b0a-4f52-8670-46d087878623" />

Security Group inbound rule — RDP restricted to my public IP
<img width="1913" height="960" alt="SecurityGroup_Screenshot" src="https://github.com/user-attachments/assets/6611ad45-8c87-41ce-a254-d43ed188958f" />

Successful remote connection via Fleet Manager
<img width="1915" height="960" alt="RemoteDesktopConnection_Screenshot" src="https://github.com/user-attachments/assets/a237afd2-45c1-44cd-9c89-836c4ef14928" />
