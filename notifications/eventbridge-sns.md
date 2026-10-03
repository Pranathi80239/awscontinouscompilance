# EventBridge + SNS Notification Workflow

## Objective

Notify an administrator when an AWS Config rule changes a resource to NON_COMPLIANT.

## Workflow

```text
AWS Config
    |
    v
Compliance Change Event
    |
    v
Amazon EventBridge
    |
    v
Amazon SNS Topic
    |
    v
Email Subscriber
```

## Setup

1. Create an SNS topic.
2. Add an email subscription.
3. Confirm the email subscription.
4. Create an EventBridge rule.
5. Configure the event pattern for AWS Config compliance changes.
6. Set the SNS topic as the target.
7. Test with a lab resource.

## Example Conceptual Event Pattern

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

Verify the actual event schema in the AWS console before using it in production.
