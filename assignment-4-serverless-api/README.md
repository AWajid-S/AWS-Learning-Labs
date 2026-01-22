# Project 4 – Serverless API (Lambda, DynamoDB & API Gateway)

## Overview

This assignment focused on building a fully serverless backend using AWS managed services.

The objective was to design an API-driven system without provisioning servers, while applying correct IAM permissions, logging, and security controls.

---

## Architecture

Client  
→ API Gateway (REST API)  
→ Lambda Function  
→ DynamoDB  
→ CloudWatch Logs  

Optional:
→ AWS WAF for rate limiting

---

## Services Used

- Amazon DynamoDB
- AWS Lambda
- Amazon API Gateway (REST)
- AWS IAM
- Amazon CloudWatch
- AWS WAF (bonus)

---

## What Was Built

- DynamoDB table with on-demand capacity
- Lambda function generating UUIDs and timestamps
- POST API endpoint integrated with Lambda
- IAM execution role using least privilege
- CloudWatch logging for observability
- API key and usage plan configuration
- Basic WAF rate-limiting rules

---

## Outcome

A working serverless API capable of accepting requests, storing structured data, and returning JSON responses without managing servers.

---

## Cleanup

All AWS resources were deleted after testing to prevent ongoing charges.
