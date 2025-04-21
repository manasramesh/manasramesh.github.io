---
title: "Detection as Code: Modernizing Security Operations with Panther SIEM"
date: 2025-04-21 12:00:00 -0500
categories: [Security, DevOps, SIEM]
tags: [detection-as-code, siem, security-operations, devsecops, cicd, panther]
---

# Detection as Code: Modernizing Security Operations with Panther SIEM

Picture this: You're a security engineer, and you've just discovered a new threat pattern. Instead of clicking through a UI to create a detection rule, you write some code, test it locally, and push it to production through your CI/CD pipeline. That's Detection as Code (DaC) in action - and it's revolutionizing how security teams operate.

In this deep dive, we'll explore how Panther SIEM implements Detection as Code, its benefits, and how it fits into modern security operations. We'll walk through real examples, best practices, and even some gotchas to watch out for.

## The Evolution of Security Detection

### Traditional SIEM Approach: The Pain Points

Before we dive into Detection as Code, let's understand why we need it. Traditional SIEM approaches have several significant drawbacks:

1. **Manual Configuration**
    - Clicking through UIs to create rules
    - No version control
    - Difficult to track changes
    - Error-prone manual processes

2. **Limited Testing**
    - Rules are tested directly in production
    - No staging environment
    - Difficult to simulate real-world scenarios
    - High risk of false positives/negatives

3. **Poor Collaboration**
    - Single-user interface
    - No code review process
    - Difficult to share knowledge
    - Siloed security teams

4. **Slow Deployment**
    - Manual deployment processes
    - No automation
    - Time-consuming updates
    - Difficult to scale

5. **Limited Documentation**
    - Rules exist only in the SIEM
    - No self-documenting code
    - Knowledge transfer challenges
    - Difficult to maintain

### Complete Detection Example

1. **Python Rule Logic** (`rules/aws_root_user_activity.py`):

    ```python
    from panther_base_helpers import deep_get

    def rule(event):
        # Only look at IAM events
        if event.get("eventSource") != "iam.amazonaws.com":
            return False
        
        # Check for root user activity
        if deep_get(event, "userIdentity", "type") == "Root":
            return True
        
        return False

    def title(event):
        return f"Root user activity detected in account {event.get('recipientAccountId')}"

    def dedup(event):
        return event.get("recipientAccountId")

    def severity(event):
        return "HIGH"

    def reference(event):
        return "https://docs.panther.com/detections/aws_root_user_activity"

    def runbook(event):
        return """
        1. Verify if the root user activity was authorized
        2. Check if MFA was used
        3. Review the specific actions taken
        4. If unauthorized, rotate root user credentials
        """
    ```

2. **YAML Metadata** (`rules/aws_root_user_activity.yml`):

    ```yaml
    AnalysisType: rule
    Enabled: true
    Filename: aws_root_user_activity
    RuleID: aws.root.user.activity
    LogTypes:
      - AWS.CloudTrail
    Description: Alerts on any root user activity in AWS accounts
    DisplayName: AWS Root User Activity
    Tags:
      - AWS
      - IAM
      - Critical
    Tests:
      - Name: Root user activity
        Log: |
          {
            "eventSource": "iam.amazonaws.com",
            "userIdentity": {
              "type": "Root"
            },
            "recipientAccountId": "123456789012"
          }
        ExpectedResult: true
      - Name: Non-root user activity
        Log: |
          {
            "eventSource": "iam.amazonaws.com",
            "userIdentity": {
              "type": "IAMUser"
            },
            "recipientAccountId": "123456789012"
          }
        ExpectedResult: false
      - Name: Non-IAM event
        Log: |
          {
            "eventSource": "ec2.amazonaws.com",
            "userIdentity": {
              "type": "Root"
            },
            "recipientAccountId": "123456789012"
          }
        ExpectedResult: false
    ```

### Directory Structure

```bash
panther-analysis/
├── rules/
│   ├── aws_root_user_activity.py
│   ├── aws_root_user_activity.yml
│   ├── aws_suspicious_api_activity.py
│   └── aws_suspicious_api_activity.yml
├── tests/
│   └── test_aws_root_user_activity.py
└── global_helpers/
    └── aws_helpers.py
```

### Unit Tests (`tests/test_aws_root_user_activity.py`)

```python
from rules.aws_root_user_activity import rule

def test_root_user_activity():
    # Test case 1: Root user activity
    assert rule({
        "eventSource": "iam.amazonaws.com",
        "userIdentity": {
            "type": "Root"
        }
    }) is True
    
    # Test case 2: Non-root user activity
    assert rule({
        "eventSource": "iam.amazonaws.com",
        "userIdentity": {
            "type": "IAMUser"
        }
    }) is False
    
    # Test case 3: Non-IAM event
    assert rule({
        "eventSource": "ec2.amazonaws.com",
        "userIdentity": {
            "type": "Root"
        }
    }) is False
```

[Rest of content remains the same...] 