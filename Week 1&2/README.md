Week 2 — AWS IAM

Topic: Identity and Access Management for secure user, role, and policy management
Original session date: June 14, 2026

Goals
-Create IAM users and groups
-Attach a permissions policy to a group
-Log in and authenticate as an IAM user
-Create and assume an IAM role
-Set up MFA

What I did
Created an IAM group and one user (`Puseletso`), then attached the
`AIOpsReadOnlyAccess` managed policy to the group. Logged out of the
root account and signed in as the IAM user to confirm the credentials
worked.

Then tried to launch an EC2 instance as this user — it failed. At
first I assumed this was expected "read-only" behavior, but on
checking the policy more closely, `AIOpsReadOnlyAccess` only grants
read access to CloudWatch Investigations (AIOps) and a few
Identity Center permissions, it has no EC2 permissions at all,
read or write. So the failure wasn't "blocked from creating," it was
"no access to EC2 in any form."

Corrected policy from AIOpsReadOnlyAccess to ReadOnlyAccess

After realizing `AIOpsReadOnlyAccess` was scoped only to CloudWatch
Investigations and had no EC2 permissions at all, I went back into
IAM → Groups → detached `AIOpsReadOnlyAccess` and attached the
general `ReadOnlyAccess` managed policy instead.

Logged back in as `Puseletso` and re-tested:
-Viewing/describing EC2 instances → (fill in: worked / didn't work)
-Launching a new EC2 instance → (fill in: still denied, as expected)

Key takeaways
- `ReadOnlyAccess` grants list/describe (view) permissions across
  most AWS services, but still correctly blocks any create, modify,
  or delete action including `ec2:RunInstances`.
- This confirms the earlier EC2 denial wasn't really about "read vs.
  write" until I switched to the *correct* read-only policy — before
  that, `AIOpsReadOnlyAccess` simply didn't cover EC2 in any capacity.
- Good practical lesson: always check what a policy actually contains
  in the JSON/console rather than assuming from its name.

