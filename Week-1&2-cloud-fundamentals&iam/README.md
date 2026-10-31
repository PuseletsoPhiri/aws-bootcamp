# Weeks 1 and 2 — AWS Account Setup and IAM Access Control

**Topics:** Bootcamp orientation, introduction to cloud computing and AWS fundamentals (Week 1); identity and access management (Week 2)
**Original session dates:** June 7, 2025 (Week 1) and June 14, 2025 (Week 2)

## Goals

### Week 1
- [x] Set up an AWS account
- [x] Provide evidence of an active account (AWS Management Console showing the account ID or billing dashboard)

### Week 2
The task asked for AWS IAM Identity Center with a user assigned a permission set based on the predefined `SecurityAudit` job function policy.
- [ ] Configure AWS IAM Identity Center (not done, see "Issues encountered")
- [x] Create a user and give it the `SecurityAudit` job function policy using IAM instead
- [x] Test the permissions the policy grants using the IAM Policy Simulator
- [x] Provide evidence of the user, the attached policy, and the test results

## What I did

### Week 1: AWS account setup
Set up my AWS account using my personal email address and signed in to
the AWS Management Console. To confirm the account was active, I
captured the console showing my account details.

### Week 2: SecurityAudit access with IAM
When I tried to enable IAM Identity Center, the console warned that it
requires creating an AWS Organization, which automatically upgrades a
free plan account to pay-as-you-go pricing and makes the free tier
credits expire immediately. Because I want to stay within free-tier
limits, I did not enable it. Instead, I achieved the same access pattern
with standard IAM:

1. Created an IAM user named csn-intern-puseletso.
2. Attached the AWS-managed **`SecurityAudit`** job function policy directly to the user, which gives read-focused access suited to security auditing.
3. Tested the policy with the **IAM Policy Simulator**. I selected **Access Analyzer** as the service, **selected all of its actions**, and ran the simulation. The results listed each action as allowed or denied, which showed exactly what the user can and cannot do with the the `SecurityAudit` permissions.

**Results:** The simulator showed `GetAnalyzedResource` as **allowed**
and `CheckNoPublicAccess` as **denied**, along with many other actions
that were each marked allowed or denied. Because some actions were
allowed and others denied, the test confirmed that `SecurityAudit`
grants only a specific set of Access Analyzer permissions, not full
access to the service.

## Additional practice: read-only policies
As an extra challenge, I used the same IAM user (`csn-intern-puseletso`), detached `SecurityAudit`, and tested two other policies one after the other to see how they behave:

- Attached the `AIOpsReadOnlyAccess` managed policy to a group
  containing my IAM user, then logged out of the root account and
  signed in as the IAM user to confirm the credentials worked.
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
- Logged back in as the user and re-tested:
  - Viewing and describing EC2 instances worked.
  - Launching a new EC2 instance was still denied, as expected.

## Issues encountered
- **IAM Identity Center would have changed my account's pricing plan.**
  The Enable page warned that creating the required organization
  upgrades the account from the free plan to pay-as-you-go and expires
  the free tier credits immediately. I chose not to proceed and used a
  standard IAM user with the `SecurityAudit` policy instead. Identity
  Center permission sets also need an organization instance, so an
  account-only instance would not have worked for this task either.
- Assumed `AIOpsReadOnlyAccess` was a general read-only policy, which
  led to the EC2 access error described above. I resolved it by
  reading the policy's actual permissions and switching to
  `ReadOnlyAccess`.

## Key takeaways
- Check what a feature requires before enabling it. Identity Center
  needs AWS Organizations, and on a free plan account that changes the
  account's billing plan.
- Identity Center manages access centrally through permission sets
  assigned to accounts, while plain IAM attaches policies directly to
  users and groups. Both can use the same AWS-managed policies, such as
  the `SecurityAudit` job function policy.
- Job function policies are AWS-managed and designed around a specific
  role, so they give someone auditing access without writing a custom
  policy.
- The IAM Policy Simulator lets you check what a policy allows or
  denies for specific actions without making real changes to the
  account, which is a safe way to verify permissions before relying on
  them.
- Policy names can mislead. `AIOpsReadOnlyAccess`, `ReadOnlyAccess`
  and `ViewOnlyAccess` cover very different things, so check what a
  policy actually allows before attaching it.
- `ReadOnlyAccess` grants list and describe permissions across most
  AWS services, but still correctly blocks any create, modify or
  delete action, including `ec2:RunInstances`.
- An access denied error is IAM working as designed, and it is a
  useful signal for working out which permission is missing.

## Screenshots / Evidence
**Week 1: AWS Management Console showing an active account**
<img width="838" height="632" alt="Screenshot 2026-10-31 085937" src="https://github.com/user-attachments/assets/abdb2265-10c0-4677-a7ba-662e00cbc82a" />

**Week 2: Identity Center warning about the plan upgrade**
<img width="1850" height="595" alt="Screenshot 2026-10-31 085903" src="https://github.com/user-attachments/assets/1bb7bad7-e342-40cf-90c7-b906fd152648" />

**Week 2: IAM user with the SecurityAudit policy attached**
<img width="1491" height="692" alt="Screenshot 2026-10-31 090344" src="https://github.com/user-attachments/assets/36b3f472-9c63-4f3c-9d9f-4e5f76500585" />

**Week 2: IAM Policy Simulator results for the SecurityAudit policy**
<img width="1343" height="412" alt="Screenshot 2026-10-31 091311" src="https://github.com/user-attachments/assets/b27854ea-e403-495a-96d0-6fedcf59f106" />

**Additional practice: EC2 launch denied with the wrong policy**
<img width="1525" height="531" alt="Screenshot 2026-10-31 152935" src="https://github.com/user-attachments/assets/ba6eccba-a160-45f2-bc2a-1e4926d0cbdc" />

