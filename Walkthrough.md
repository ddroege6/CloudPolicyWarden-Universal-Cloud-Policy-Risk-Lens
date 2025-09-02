# CloudShield: Universal Cloud Policy Risk Lens - Visual Walkthrough

This guide demonstrates the complete workflow of analyzing cloud policies using CloudShield, from initial policy input through final remediation recommendations.

---

## Screenshot Guide

### 1. App Interface Overview
<img width="1128" height="1299" alt="Image" src="https://github.com/user-attachments/assets/17803dbf-c9c4-4939-8b83-ed35e290b461" />

**Caption:** *The Universal Cloud Policy Risk Lens interface - paste any cloud policy and get instant security analysis*

---

### 2. Policy Input Example
*<img width="2143" height="1308" alt="Image" src="https://github.com/user-attachments/assets/39169ac5-02b2-49b5-823a-e3177b16b47b" />*
**Caption:** *Analyzing a production AWS IAM policy with high strictness settings*

---

### 3. Policy Analysis Report
Includes:
- Risk Score
- Key Findings
- Recommendations
- Remediation Timeline
  
[![Watch the demo](assets/thumb.png)](https://github.com/user-attachments/assets/9eae2709-aaab-42e3-98de-115268001ea4 "Click to watch")

**Caption:** *Instant risk assessment with platform auto-detection and severity classification*

---

### 4. Security Consultatant Chat

[![Watch the demo](assets/thumb.png)](https://github.com/user-attachments/assets/c2059b14-fe8b-4227-80ae-46859fb8c893 "Click to watch")

---

## Comparison Screenshots

### Before/After Risk Score Comparison
*<img width="2068" height="925" alt="Image" src="https://github.com/user-attachments/assets/125902be-6ecb-4369-b947-b4360c9e9cf7" />*

**After a few remediation implementations, the Risk Score is dramatically decreased.**

<img width="2067" height="917" alt="Image" src="https://github.com/user-attachments/assets/21647178-23bd-412f-8133-735b0a505c0d" />


---

### Policy Code Comparison

---


**Bad**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3FullAccess",
      "Effect": "Allow",
      "Action": [
        "s3:*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "EC2AndIAMAccess",
      "Effect": "Allow",
      "Action": [
        "ec2:*",
        "iam:PassRole",
        "iam:CreateUser",
        "iam:AttachUserPolicy",
        "iam:PutUserPolicy"
      ],
      "Resource": "*"
    },
    {
      "Sid": "SecretsAccess",
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue",
        "secretsmanager:CreateSecret",
        "secretsmanager:UpdateSecret",
        "secretsmanager:DeleteSecret"
      ],
      "Resource": "arn:aws:secretsmanager:*:*:secret:*"
    },
    {
      "Sid": "DatabaseAccess",
      "Effect": "Allow",
      "Action": [
        "rds:*",
        "dynamodb:*"
      ],
      "Resource": "*"
    }
  ]
}
```

**Good**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyPublicAccess",
      "Effect": "Deny",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ],
      "Condition": {
        "Bool": {
          "s3:PublicAccessBlock": "false"
        }
      }
    },
    {
      "Sid": "DenyBucketPolicyModification",
      "Effect": "Deny",
      "Action": "s3:PutBucketPolicy",
      "Resource": "arn:aws:s3:::your-bucket-name"
    },
    {
      "Sid": "RestrictedS3Access",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::your-bucket-name/*",
        "arn:aws:s3:::your-bucket-name"
      ],
      "Condition": {
        "StringEquals": {
          "s3:x-amz-server-side-encryption": "aws:kms",
          "s3:VersionStatus": "Enabled"
        },
        "Bool": {
          "aws:SecureTransport": "true",
          "aws:MultiFactorAuthPresent": "true"
        },
        "IpAddress": {
          "aws:SourceIp": ["YOUR-ALLOWED-IP-RANGE"]
        }
      }
    },
    {
      "Sid": "RestrictedS3Delete",
      "Effect": "Allow",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*",
      "Condition": {
        "Bool": {
          "aws:MultiFactorAuthPresent": "true"
        }
      }
    },
    {
      "Sid": "RestrictedEC2Access",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "arn:aws:ec2:*:*:instance/i-*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Environment": "Production",
          "ec2:Region": ["us-east-1", "us-west-2"],
          "ec2:MetadataHttpTokens": "required",
          "ec2:InstanceType": ["t3.micro", "t3.small", "t3.medium"]
        },
        "IpAddress": {
          "aws:SourceIp": ["YOUR-ALLOWED-IP-RANGE"]
        }
      }
    },
    {
      "Sid": "LimitedIAMPassRole",
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::123456789012:role/specific-application-role",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "ec2.amazonaws.com"
        },
        "NumericLessThan": {
          "iam:MaxSessionDuration": "3600"
        }
      }
    },
    {
      "Sid": "RestrictedSecretsAccess",
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue"
      ],
      "Resource": "arn:aws:secretsmanager:region:account-id:secret:specific-secret-name-??????",
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "true"
        },
        "DateGreaterThan": {
          "secretsmanager:RotationAge": "30"
        },
        "StringEquals": {
          "secretsmanager:VersionStage": "AWSCURRENT",
          "secretsmanager:KmsKeyId": "arn:aws:kms:region:account-id:key/specific-key-id"
        },
        "IpAddress": {
          "aws:SourceIp": ["YOUR-ALLOWED-IP-RANGE"]
        }
      }
    },
    {
      "Sid": "RestrictedDatabaseAccess",
      "Effect": "Allow",
      "Action": [
        "rds:DescribeDBInstances",
        "dynamodb:GetItem",
        "dynamodb:Query"
      ],
      "Resource": [
        "arn:aws:rds:*:*:db:prod-db-*",
        "arn:aws:dynamodb:*:*:table/prod-table"
      ],
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "true"
        },
        "StringEquals": {
          "aws:SourceVpce": "vpce-YOUR-ENDPOINT-ID",
          "aws:ResourceTag/Environment": "Production"
        }
      }
    }
  ]
}
```


### Findings Reduction Table
I ended up doing more remediations so this isn't exact but the concept is the same.

**Table format:**
| Finding Type | Before | After | Improvement |
|--------------|--------|--------|-------------|
| Critical Issues | 5 | 0 | ✅ 100% Resolved |
| High Risk | 8 | 1 | ✅ 87% Reduced |
| Medium Risk | 12 | 3 | ✅ 75% Reduced |
| **Total Risk Score** | **CRITICAL** | **LOW** | **✅ Major Improvement** |

---

## Multi-Platform Demo Screenshots

### Platform Detection Showcase
*<img width="2061" height="913" alt="Image" src="https://github.com/user-attachments/assets/3f034132-5e1c-4b36-b795-5a2de9b797d9" />*
<img width="2059" height="916" alt="Image" src="https://github.com/user-attachments/assets/59bca40a-cabe-4140-8bc0-5414aaea1e2b" />
<img width="2063" height="915" alt="Image" src="https://github.com/user-attachments/assets/5470d6f3-dcdb-49c6-aa20-11ab9e9a82b9" />

---
