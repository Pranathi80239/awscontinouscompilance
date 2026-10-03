# Config Rule Design

## Control

S3 buckets used by the lab should not be publicly accessible.

## Resource Type

Amazon S3 bucket.

## Evaluation

Use an AWS Config managed rule that checks the relevant S3 public-access/security configuration.

## Compliance States

### COMPLIANT
The bucket satisfies the rule.

### NON_COMPLIANT
The bucket violates the rule.

### NOT_APPLICABLE
The rule does not apply to the evaluated resource.

## Testing

Use a dedicated test bucket. Change its configuration in a controlled manner to create a violation, observe the Config result, and restore the secure configuration immediately.

## Important

AWS Config rule identifiers and available remediation actions may vary. Select the current rule from the AWS Config console for the AWS Region being used.
