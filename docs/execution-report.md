# Execution Report

## Title

AWS Config Rules for Continuous Compliance Monitoring and Automated Remediation

## Aim

To implement continuous AWS resource compliance monitoring using AWS Config Rules and demonstrate notification and automated remediation concepts.

## Requirements

- AWS account
- AWS Config
- Amazon S3
- Amazon SNS
- Amazon EventBridge
- AWS Systems Manager
- IAM permissions

## Procedure

### 1. Configure AWS Config
Enable the AWS Config recorder and delivery channel.

### 2. Create Test Resource
Create a dedicated S3 bucket for testing.

### 3. Add Config Rule
Add an appropriate AWS Config managed rule for S3 security/public access.

### 4. Evaluate
Check the resource compliance status.

### 5. Create Controlled Violation
Modify the test resource so that it violates the selected rule.

### 6. Observe Alert
Use EventBridge and SNS to notify an administrator of the compliance change.

### 7. Remediate
Use an approved AWS Config remediation action / Systems Manager Automation workflow.

### 8. Re-evaluate
Confirm that the resource returns to COMPLIANT.

## Result

The project demonstrates the continuous compliance lifecycle:

**Monitor → Detect → Alert → Remediate → Re-evaluate**

## Screenshots to Add

After executing the lab, add screenshots showing:

1. AWS Config dashboard
2. Config rule details
3. COMPLIANT resource
4. NON_COMPLIANT resource
5. SNS notification
6. EventBridge rule
7. Systems Manager Automation execution
8. Final COMPLIANT status

Save screenshots in the `screenshots/` directory.
