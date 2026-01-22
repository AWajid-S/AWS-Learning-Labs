# Learning Notes – Project 3 (S3, CloudFront & DNS)

## Overview

This project focused on understanding how static websites are hosted using managed cloud services.

While the original goal was to deploy a full AWS-native setup using S3, CloudFront, and Route 53, the exercise became a realistic learning experience around DNS ownership, HTTPS certificates, and provider boundaries.

This reflected real production behaviour rather than a simplified lab.

---

## What I Built

- S3 bucket configured for static website hosting
- Public read-only bucket policy
- Static website endpoint tested successfully
- CloudFront distribution created in front of S3
- Attempted custom domain configuration using AWS services
- Identified DNS and certificate conflicts due to existing Cloudflare setup

---

## Key Learning Outcomes

### 1. S3 can host websites, not just store files

When static website hosting is enabled on an S3 bucket:

- AWS provides a website endpoint
- `index.html` is served automatically
- No servers or compute are required

This demonstrated how object storage can function as a lightweight web host.

---

### 2. S3 website endpoints do not support HTTPS

The S3 website endpoint:

- Only supports HTTP
- Cannot attach TLS certificates
- Is not suitable for production exposure

This explains why S3 should always sit behind another service when used publicly.

---

### 3. CloudFront provides HTTPS and public access

CloudFront was introduced as the front-facing layer.

It provides:

- HTTPS termination
- Global caching
- Public access point for users
- Secure communication with S3

Using CloudFront’s default domain successfully served the site over HTTPS.

---

### 4. Custom domains depend on DNS authority

When attempting to use a custom domain (`nginxwajid.uk`), the configuration failed despite correct AWS setup.

Reason:

- The domain’s DNS was managed by Cloudflare
- Route 53 was not authoritative
- Route 53 records therefore had no effect

Key learning:

> DNS records only work where the domain’s nameservers point.

Creating a hosted zone alone does not give control.

---

### 5. HTTPS certificate errors are usually DNS problems

The browser error: NET::ERR_CERT_COMMON_NAME_INVALID

occurred because:

- CloudFront was serving its default certificate
- The custom domain was not validated
- DNS validation could not complete without authority

This showed that TLS failures are often caused by DNS ownership issues rather than service misconfiguration.

---

### 6. Mixing DNS and CDN providers adds complexity

Because Cloudflare was already providing:

- DNS
- CDN
- HTTPS

Introducing Route 53 and CloudFront created overlapping responsibilities.

This highlighted an important production rule:

> One DNS authority should exist per domain.

Multiple CDNs and DNS providers should only be used intentionally.

---

## Cost Awareness

Important services that must always be cleaned up after labs:

- CloudFront distributions (must be disabled before deletion)
- Route 53 hosted zones (monthly cost)
- S3 buckets with stored objects

Failure to remove these can lead to ongoing charges.

---

## Final Reflection

This project provided a strong real-world lesson.

I learned:

- How DNS authority controls everything above it
- Why HTTPS depends on DNS validation
- Why certificates fail even when services appear correct
- How cloud problems are often architectural, not technical
- When to stop and reassess instead of forcing a broken setup

Although the full AWS-native flow was not completed, the understanding gained was more realistic than a successful walkthrough.
