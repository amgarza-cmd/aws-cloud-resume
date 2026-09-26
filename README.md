# AWS Cloud Resume

My resume, built and hosted on AWS as a hands-on project while studying for the AWS Solutions Architect Associate.

This project is based on the [Cloud Resume Challenge](https://cloudresumechallenge.dev/). I'm documenting each phase as I go, including the decisions I made and why.

## Phase 1: Securing the AWS Account

Before building anything, I locked down the account. A new AWS account is like a new house: you change the locks and install the smoke detectors before you move your stuff in.

### What I set up

| Control | What it does | Why it matters |
|---|---|---|
| **Root account MFA** | Requires a second factor to log in as root | Root has unlimited access, including billing. It's the one login that can never be compromised. |
| **Root locked away** | Root is only used for billing or account emergencies | Limits how often the most powerful credential is ever exposed. |
| **Separate admin IAM user with MFA** | Daily work happens under a dedicated user, not root | If this login is ever compromised, root is still safe and can lock it down. |
| **Account alias** | Custom sign-in URL instead of the numeric account ID | Easier sign-in, and the account ID doesn't need to be memorized or shared. |
| **Budget alert ($5)** | Emails me if spending goes over $5 | Catches forgotten resources or unexpected charges within a day instead of at the end of the month. |
| **CloudTrail** | Logs every API call and action in the account to S3 | A permanent audit trail. If something changes, I can see who did it and when. |
| **AWS CLI with `aws login`** | CLI uses temporary credentials from a browser sign-in | No long-term access keys stored on my PC. Leaked access keys are one of the most common ways AWS accounts get compromised. |

### Decisions and tradeoffs

**Skipped IAM Identity Center (for now).** Identity Center is the AWS-recommended way to manage users, and I started setting it up. But enabling it requires creating an AWS Organization, which would have moved my account off the free plan, ended my free tier credits immediately, and added a monthly charge for a customer-managed encryption key. For a single-person account, a dedicated IAM user with MFA gives the same separation from root at no cost. If I add more accounts later (like a separate dev/test account), Identity Center becomes worth it.

**Temporary credentials over access keys.** The traditional CLI setup uses permanent access keys saved in a local file. Instead, I used `aws login`, which signs in through the browser and issues short-lived credentials that expire on their own. Less convenient, much lower risk.

**Declined AI agent access.** The CLI offered to connect an AI coding agent to AWS. I declined because my user has full admin permissions, and an agent should get its own user with limited permissions, not admin keys.

## Roadmap

- [x] Phase 1: Secure the AWS account
- [ ] Phase 2: Resume as an HTML/CSS page
- [ ] Phase 3: Host on S3 with CloudFront, a custom domain (Route 53), and HTTPS
- [ ] Phase 4: Visitor counter (Lambda, API Gateway, DynamoDB)
- [ ] Phase 5: Infrastructure as code with Terraform
- [ ] Phase 6: CI/CD with GitHub Actions
