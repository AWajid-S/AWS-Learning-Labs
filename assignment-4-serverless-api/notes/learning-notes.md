# Learning Notes – Project 4 (Serverless API)

## Objective

The goal of this project was to understand how AWS serverless services work together to build a production-style backend API without managing servers.

---

## What I Learned

### 1. How serverless architecture works
I learned how requests flow through multiple AWS-managed services:

Client  
→ API Gateway  
→ Lambda  
→ DynamoDB  
→ CloudWatch Logs  

Each service has a single responsibility and communicates using IAM permissions rather than network access.

---

### 2. DynamoDB data modelling
- DynamoDB does not support traditional SQL queries.
- Data access is entirely based on primary keys.
- On-demand capacity automatically scales without manual configuration.
- Proper key design is critical because it cannot be changed later.

---

### 3. Lambda execution and permissions
- Lambda functions are stateless.
- Every action must be explicitly allowed through IAM.
- Missing permissions cause runtime failures, not deployment failures.
- CloudWatch logs are essential for debugging Lambda behaviour.

---

### 4. API Gateway behaviour
- API Gateway is only a routing layer.
- Business logic must live inside Lambda.
- Proxy integration simplifies request and response handling.
- CORS must be explicitly enabled for browser-based clients.

---

### 5. IAM least-privilege principles
- Lambda roles should only allow required actions.
- Over-permissive policies are dangerous and unnecessary.
- Policies must specify:
  - Exact DynamoDB table
  - Specific actions (PutItem, Scan)
- AWS managed logging policies simplify CloudWatch access.

---

### 6. Observability with CloudWatch
- Logs are created automatically per Lambda function.
- Logs persist even after the function is deleted.
- Log groups can generate small costs if not cleaned up.
- CloudWatch is the primary debugging tool for serverless systems.

---

### 7. API protection concepts
- API keys provide basic access control.
- Usage plans define request limits.
- AWS WAF can enforce rate limiting before traffic reaches Lambda.
- Security is layered, not handled by one service.

---

## Challenges Faced and How I Solved Them

### Challenge 1: Data not appearing in DynamoDB
**Issue:**  
Requests returned 200 OK but no items were saved.

**Cause:**  
Lambda execution role was missing `dynamodb:PutItem` permission.

**Solution:**  
Updated the IAM role to allow PutItem on the specific DynamoDB table ARN.  
After redeploying, items were successfully written.

---

### Challenge 2: API returning unexpected JSON structure
**Issue:**  
The API response contained escaped JSON inside the body field.

**Cause:**  
API Gateway proxy integration wraps Lambda responses automatically.

**Solution:**  
Adjusted the Lambda return structure to correctly format:
- statusCode
- headers
- body (stringified JSON)

This produced clean client responses.

---

### Challenge 3: WAF resources not appearing
**Issue:**  
WAF initially showed “no resources available” when attaching to API Gateway.

**Cause:**  
The Web ACL scope and region must exactly match the API Gateway region.

**Solution:**  
Created the WAF as a **regional Web ACL** in the same region as the REST API.  
After this, the API Gateway stage appeared correctly.

---

### Challenge 4: Understanding AWS costs
**Issue:**  
Billing showed active services even after deleting resources.

**Cause:**  
AWS billing reflects usage during the month, not currently running resources.

**Solution:**  
Verified all resources were deleted and confirmed no ongoing charge-generating services remained.

---

## Key Takeaway

This project demonstrated how real-world backend systems are built using loosely coupled, managed cloud services.

It highlighted the importance of:
- IAM permissions
- Observability
- Security layers
- Cost awareness
- Clean resource teardown

This closely reflects how production serverless systems are designed and maintained.
