# D484 - Cloud Security Audit Lab
(Independent Recreation)

> Independent recreation in my personal AWS free-tier account. Not WGU material. Built to practice concepts.f

## Objective
Practice cloud auditing: identifying over-permissive S3 policies and applying least privilege. 

## My Lab Setup (My own Account)
- Created 3 S3 buckets:
  - my-test-bucket-secure-2026 - Block all public access ON, private ACL
  - my-test-bucket-policy-issue-2026 - Added public-read ACL to test detection
  - my-test-bucket-policy-issue-2026 - Added wildcard Principal to test detection
  - Created custom IAM tole
    SecurityAuditorRole with read-only access

## Tools Used
- ScoutSuite - open-source cloud audit tool
- AWS CLI - get-bucket-acl and get-bucket-policy
- Prowler for CIS checks

## What I Found
1. ACL Check: 1 bucket with public-read ACL - high risk
2. Policy Check: 1 bucket allowed Principal* - violates least privilege
3. Logging: No bucket has server access logging enabled

## How I Fixed It
aws s3api put-bucket-acl --bucket my-test-bucket-acl-issue-2026 
--acl private aws s3api put-public-access-block --bucket my-test-bucket
-acl-issue-2026 --public-access-block-configuration
BlockPublicAcls=true,IgnorrPublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
Replaced wildcard policy with specific IAM role ARN.

## Key Takeaways
- Block S3 public access by default
- Avoid Principal * - use specific ARNs
- ScoutSuite great for quick audits

## Skills Learned
AWS S3 security, IAM Least Privilege, ScoutSuite, CLoud Auditing