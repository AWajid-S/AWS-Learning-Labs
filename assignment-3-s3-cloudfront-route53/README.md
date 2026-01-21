# Assignment 3 — S3 Static Website + CloudFront CDN + Route 53

## Objective

Create a production-ready static website deployed using AWS managed services.

The website must be hosted on Amazon S3, delivered through CloudFront for global CDN and HTTPS support, and routed via Route 53 DNS.

This assignment focuses on real-world DevOps concepts such as static hosting, content delivery networks, caching, HTTPS, and DNS routing.

---

## Architecture Summary

Internet → Route 53 → CloudFront (HTTPS + CDN) → S3 Static Website

---

## Tasks

### 1. Static Website Bucket

- Create an S3 bucket
- Enable Static Website Hosting
- Upload:
  - `index.html`
  - `error.html`
- Configure bucket policy to allow public read access
- Verify website endpoint works

---

### 2. CloudFront Distribution

- Create a CloudFront distribution
- Origin: S3 website endpoint
- Enable:
  - HTTPS
  - Object compression
- Cache policy:
  - Managed – CachingOptimized
- Default behavior:
  - Allow GET and HEAD methods
- (Optional) Attach ACM certificate for custom domain

---

### 3. Route 53 Configuration

- Create a hosted zone (if not already present)
- Add an A record (Alias)
- Point the record to the CloudFront distribution
- Verify domain resolves correctly

---

### 4. Testing

- Access website via custom domain
- Confirm content is served by CloudFront
- Validate caching behavior:
  - Update `index.html`
  - Create CloudFront cache invalidation
  - Refresh page and confirm update

---

## Bonus (Mandatory)

### CI/CD Pipeline

- Create GitHub Actions workflow
- Automatically deploy website files to S3 on `git push`
- Invalidate CloudFront cache after deployment

### Security Enhancements

- Add security headers using CloudFront Functions
- Implement URL rewriting using Lambda@Edge

---

## Evidence

- Screenshots stored in `/screenshots`
- Learning notes stored in `/notes`
