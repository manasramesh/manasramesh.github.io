## Panther's Detection as Code Approach

Panther uses a combination of Python for detection logic and YAML for metadata, tests, and configuration. This separation of concerns makes detections more maintainable and testable. Let's look at a complete example:

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