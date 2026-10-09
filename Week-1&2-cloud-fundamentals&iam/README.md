# Weeks 1 and 2 — AWS Account Setup and Identity Center

**Topics:** Bootcamp orientation, introduction to cloud computing and AWS fundamentals (Week 1); identity and access management (Week 2)
**Original session dates:** June 7, 2025 (Week 1) and June 14, 2025 (Week 2)

## Goals

### Week 1
- [x] Set up an AWS account
- [x] Provide evidence of an active account (AWS Management Console showing the account ID or billing dashboard)

### Week 2
- [x] Configure AWS IAM Identity Center in the account
- [x] Create a new user
- [x] Assign the user a permission set using the predefined `SecurityAudit` job function policy
- [x] Provide evidence of the Identity Center instance, the user created, and the assigned permission set

## What I did

### Week 1: AWS account setup
Set up my AWS account using my personal email address and signed in to
the AWS Management Console. To confirm the account was active, I
captured the console showing my account details.

### Week 2: Identity Center
Enabled **AWS IAM Identity Center** in the account, which provides a
central place to manage workforce access. I then:

1. Created a new user in Identity Center.
2. Created a permission set based on the predefined **`SecurityAudit`**
   job function policy, which gives read-focused access suited to
   security auditing.
3. Assigned the user to my AWS account with that permission set, so
   they can sign in through the AWS access portal with auditing
   permissions only.

(fill in: the user and permission set names you used, and whether you
signed in as the new user through the access portal to test it.)

## Additional practice: classic IAM users and groups
Beyond the task, I practised with regular IAM to see how policies
behave:

- Created an IAM group and one user (`Puseletso`), attached the
  `AIOpsReadOnlyAccess` managed policy to the group, then logged out
  of the root account and signed in as the IAM user to confirm the
  credentials worked.
- Tried to launch an EC2 instance as this user, and it failed. At
  first I assumed this was expected read-only behaviour, but on
  checking the policy more closely, `AIOpsReadOnlyAccess` only grants
  read access to CloudWatch Investigations (AIOps) and a few Identity
  Center permissions. It has no EC2 permissions at all, read or
  write, so the failure wasn't "blocked from creating", it was "no
  access to EC2 in any form".
- **Correction:** in IAM, I opened the group, detached
  `AIOpsReadOnlyAccess`, and attached the general `ReadOnlyAccess`
  managed policy instead.
- Logged back in as `Puseletso` and re-tested:
  - Viewing and describing EC2 instances worked.
  - Launching a new EC2 instance was still denied, as expected.

## Issues encountered
- Assumed `AIOpsReadOnlyAccess` was a general read-only policy, which
  led to the EC2 access error described above. I resolved it by
  reading the policy's actual permissions and switching to
  `ReadOnlyAccess`.
- (fill in: any issues with account creation or Identity Center setup,
  or delete this line if there were none.)

## Key takeaways
- Identity Center manages access centrally: users get permissions
  through permission sets assigned to specific accounts, rather than
  by creating separate IAM users in every account.
- Job function policies such as `SecurityAudit` are AWS-managed and
  designed around a specific role, which makes it easy to give
  someone auditing access without writing a custom policy.
- Policy names can mislead. `AIOpsReadOnlyAccess`, `ReadOnlyAccess`
  and `ViewOnlyAccess` cover very different things, so check what a
  policy actually allows before attaching it.
- `ReadOnlyAccess` grants list and describe permissions across most
  AWS services, but still correctly blocks any create, modify or
  delete action, including `ec2:RunInstances`.
- The earlier EC2 denial wasn't really about read versus write until
  I switched to the correct read-only policy. Before that,
  `AIOpsReadOnlyAccess` simply didn't cover EC2 at all.
- An access denied error is IAM working as designed, and it is a
  useful signal for working out which permission is missing.

## Screenshots / Evidence
**Week 1: AWS Management Console showing an active account**
<img width="1887" height="950" alt="Screenshot 2026-10-09 142017" src="https://github.com/user-attachments/assets/272c98f6-0a4f-4ecc-a0d9-0ba0ec1f569e" />

**Week 2: Identity Center instance**
<img width="1372" height="956" alt="Screenshot 2026-10-09 143323" src="https://github.com/user-attachments/assets/102c808a-e163-4f65-aaef-8f8e360b173e" />

**Week 2: User created in Identity Center**
<img width="1893" height="965" alt="Screenshot-3" src="https://github.com/user-attachments/assets/cfe7fce6-17ab-4a30-8086-931e1d1c8e08" />

**Week 2: SecurityAudit permission set assigned**
<img width="1902" height="956" alt="Screenshot-4" src="https://github.com/user-attachments/assets/ba9a6baf-f534-4683-8132-5ef9d5ce4aad" />

**Additional practice: EC2 launch denied with the wrong policy**
