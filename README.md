# AWS Config Rules for Continuous Compliance Monitoring and Automated Remediation

**Author:** RATNALA PRANATHI  
**GitHub:** Pranathi80239  
**Project:** AWS Config Rules for Continuous Compliance Monitoring and Automated Remediation

## 1. Project Overview

AWS Config continuously records and evaluates the configuration of AWS resources. AWS Config Rules can be used to check whether resources follow defined security and compliance requirements.

This project demonstrates a practical workflow:

**AWS Resource → AWS Config Rule → Compliance Evaluation → Violation → Alert → Automated Remediation**

The example focuses on an S3 bucket that should not allow public read/write access.

## 2. Objectives

- Monitor AWS resource configurations continuously.
- Detect configuration violations using AWS Config Rules.
- Generate compliance status for resources.
- Send notifications when a violation is detected.
- Automatically remediate selected violations using AWS Systems Manager Automation.
- Maintain documentation and configuration files in GitHub.

## 3. AWS Services Used

| Service | Purpose |
|---|---|
| AWS Config | Records resource configuration and evaluates compliance |
| AWS Config Rules | Defines compliance conditions |
| Amazon S3 | Example resource being monitored |
| Amazon SNS | Sends compliance notifications |
| AWS Systems Manager Automation | Performs automated remediation |
| IAM | Provides required permissions |
| AWS CloudTrail | Provides audit/event visibility |

## 4. Real-World Scenario

A company stores application data in Amazon S3. A security policy requires S3 buckets to remain private.

### Problem
A developer accidentally changes a bucket policy or access configuration and makes a bucket publicly accessible.

### Detection
AWS Config evaluates the bucket against an S3 public-access compliance rule.

### Alert
The resource becomes **NON_COMPLIANT**, and an SNS notification can be sent.

### Remediation
An approved Systems Manager Automation runbook can restore the required security configuration.

## 5. Architecture

```text
                    +------------------+
                    |   AWS Resource   |
                    |   S3 Bucket      |
                    +--------+---------+
                             |
                             v
                    +------------------+
                    |    AWS Config     |
                    | Configuration    |
                    |   Recorder       |
                    +--------+---------+
                             |
                             v
                    +------------------+
                    |   Config Rule    |
                    | S3 Compliance    |
                    +--------+---------+
                             |
                    +--------+---------+
                    |                  |
                    v                  v
              COMPLIANT          NON_COMPLIANT
                                      |
                         +------------+------------+
                         |                         |
                         v                         v
                  Amazon SNS              SSM Automation
                   Alert/Email             Remediation
                         |                         |
                         +------------+------------+
                                      |
                                      v
                              Resource corrected
```

## 6. Suggested Config Rule

For an S3 security demonstration, use an AWS managed rule related to public access or bucket security. Managed rules are recommended for a beginner project because AWS maintains the rule logic.

Example rule concept:

- Resource: Amazon S3 bucket
- Requirement: Public access must be blocked
- Evaluation: Configuration change / periodic evaluation
- Result: COMPLIANT or NON_COMPLIANT

> Rule names and supported remediation actions can change between AWS Regions and AWS service updates. Verify the currently available managed rule in the AWS Config console before deployment.

## 7. Execution Procedure

### Step 1 - Enable AWS Config

1. Open the AWS Management Console.
2. Search for **AWS Config**.
3. Choose **Get started** if AWS Config is not configured.
4. Enable resource recording.
5. Select the resources to record.
6. Create or select an S3 bucket for the AWS Config delivery channel.
7. Complete the setup.

### Step 2 - Create an S3 Test Resource

Create a test S3 bucket.

Do not use production data.

Keep the bucket private initially.

### Step 3 - Create the Config Rule

1. Open AWS Config.
2. Select **Rules**.
3. Choose **Add rule**.
4. Search for an appropriate S3 public-access/security managed rule.
5. Select the rule.
6. Select the desired evaluation trigger.
7. Create the rule.

### Step 4 - Check Compliance

Open:

**AWS Config → Rules → Select Rule → Resources**

The resource will be evaluated as:

- COMPLIANT
- NON_COMPLIANT
- NOT_APPLICABLE

### Step 5 - Generate a Test Violation

For a controlled lab, intentionally change the configuration of the test resource so that it violates the rule.

After evaluation, AWS Config should report the resource as **NON_COMPLIANT**.

Restore the resource immediately after testing.

### Step 6 - Configure SNS Notification

Create an SNS topic such as:

`aws-config-compliance-alerts`

Subscribe an email address and confirm the subscription.

A notification workflow can be connected to compliance events through EventBridge.

### Step 7 - Automated Remediation

AWS Config supports remediation actions for supported rules.

Typical workflow:

```text
NON_COMPLIANT
      |
      v
AWS Config
      |
      v
EventBridge / Config remediation
      |
      v
Systems Manager Automation
      |
      v
Correct configuration
      |
      v
AWS Config reevaluates
      |
      v
COMPLIANT
```

Use only an approved remediation runbook and test it on a non-production resource first.

## 8. Event-Driven Notification Concept

An EventBridge rule can monitor AWS Config compliance-change events and route them to SNS.

Example logical pattern:

```text
AWS Config
   |
   | Compliance change
   v
Amazon EventBridge
   |
   v
Amazon SNS
   |
   v
Email / Notification
```

## 9. Sample Event Pattern

The following is a conceptual EventBridge pattern. Adjust the pattern to the exact AWS Config event format used in your account.

```json
{
  "source": ["aws.config"],
  "detail-type": ["Config Rules Compliance Change"],
  "detail": {
    "newEvaluationResult": {
      "complianceType": ["NON_COMPLIANT"]
    }
  }
}
```

## 10. Project Structure

```text
AWS_Config_Continuous_Compliance/
│
├── README.md
├── architecture/
│   └── architecture.md
├── config-rules/
│   └── rule-design.md
├── remediation/
│   └── remediation-workflow.md
├── notifications/
│   └── eventbridge-sns.md
├── dataset/
│   └── aws_resource_compliance_dataset.csv
├── screenshots/
│   └── README.md
├── docs/
│   └── execution-report.md
├── .gitignore
└── LICENSE
```

## 11. Dataset

The `dataset/aws_resource_compliance_dataset.csv` file is a **lab-created demonstration dataset**. It is not collected from AWS automatically.

It can be used for project documentation, charts, analysis, and presentation.

## 12. Expected Result

After configuration:

| Resource | Rule | Status | Action |
|---|---|---|---|
| Test S3 bucket | S3 security rule | COMPLIANT | None |
| Test S3 bucket after unsafe change | S3 security rule | NON_COMPLIANT | Alert |
| Test S3 bucket after remediation | S3 security rule | COMPLIANT | Remediated |

## 13. Security Notes

- Never test remediation against production resources without approval.
- Use a dedicated lab AWS account or test resources.
- Follow least-privilege IAM permissions.
- Do not commit AWS access keys, secret keys, passwords, or account secrets to GitHub.
- Do not upload confidential AWS configuration data.
- Delete unnecessary test resources after completing the lab.

## 14. GitHub Upload

From the extracted project folder:

```bash
git init
git add .
git commit -m "Add AWS Config continuous compliance project"
git branch -M main
git remote add origin https://github.com/Pranathi80239/AWS_Config_Continuous_Compliance.git
git push -u origin main
```

If the repository already exists:

```bash
git add .
git commit -m "Update AWS Config compliance project"
git push
```

## 15. Conclusion

AWS Config provides continuous visibility into AWS resource configurations and can evaluate resources against compliance requirements. Combining AWS Config Rules with EventBridge, SNS, IAM, and Systems Manager Automation creates a practical workflow for detecting, notifying, and remediating configuration violations.

This project demonstrates how cloud compliance can move from manual checking toward continuous monitoring and automated response.
