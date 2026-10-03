# AWS Config Continuous Compliance Architecture

## Workflow

1. AWS resource is created or modified.
2. AWS Config records the configuration.
3. Config Rule evaluates the resource.
4. Resource receives a compliance status.
5. If NON_COMPLIANT, an event/notification workflow can be triggered.
6. An approved remediation action can correct the resource.
7. AWS Config evaluates the resource again.

## Architecture Diagram

```text
+--------------------+
| AWS S3 Test Bucket |
+---------+----------+
          |
          v
+--------------------+
| AWS Config Recorder|
+---------+----------+
          |
          v
+--------------------+
|   AWS Config Rule  |
+---------+----------+
          |
     +----+----+
     |         |
     v         v
 COMPLIANT  NON_COMPLIANT
               |
        +------+------+
        |             |
        v             v
   EventBridge   Config/SSM
        |         Remediation
        v             |
       SNS <----------+
        |
        v
   Email/Alert
```
