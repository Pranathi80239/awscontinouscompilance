# Automated Remediation Workflow

## Purpose

Automated remediation is used to restore an AWS resource to an approved configuration after a compliance violation.

## Workflow

```text
Config Rule
    |
    v
NON_COMPLIANT
    |
    v
Remediation Action
    |
    v
AWS Systems Manager Automation
    |
    v
Resource configuration corrected
    |
    v
AWS Config reevaluation
    |
    v
COMPLIANT
```

## Safe Implementation

1. Start with a test resource.
2. Identify the exact non-compliant configuration.
3. Select an AWS-supported remediation action where available.
4. Review the IAM permissions required by the automation.
5. Test the remediation manually.
6. Enable automatic remediation only after successful testing.
7. Monitor the Config compliance result after remediation.

## Rollback

If remediation causes an unexpected result:

1. Disable automatic remediation.
2. Inspect the Systems Manager Automation execution.
3. Restore the known-good configuration.
4. Re-run Config evaluation.
5. Document the issue.
