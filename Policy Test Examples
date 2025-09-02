# Cloud Policy Test Examples

This collection contains deliberately insecure policies from various cloud platforms to test the Universal Cloud Policy Risk Lens. Each example contains multiple security issues of varying severity.

---

## Usage Instructions

1. **Copy any policy above** into your Universal Cloud Policy Risk Lens
2. **Set environment** (Dev/Test/Prod) and **strictness level** (1-5)  
3. **Compare results** across different configurations
4. **Use the "Low Risk" example** to see how secure policies are analyzed

## Expected Analysis Results

- **🔴 Critical/High Risk**: Should identify privilege escalation paths, recommend immediate fixes
- **🟡 Medium Risk**: Should highlight overbroad permissions, suggest scoping improvements  
- **🟢 Low Risk**: Should validate security posture, suggest minor optimizations

---

## 🔴 AWS IAM Policy - High Risk
**Expected Risk Level**: HIGH/CRITICAL  
**Key Issues**: Wildcard permissions, privilege escalation paths, overly broad resources

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

---

## 🟡 Azure RBAC Custom Role - Medium Risk  
**Expected Risk Level**: MEDIUM/HIGH  
**Key Issues**: Broad authorization permissions, subscription-wide scope

```json
{
  "Name": "Custom Application Manager",
  "Id": "12345678-1234-1234-1234-123456789012",
  "IsCustom": true,
  "Description": "Manages applications and related resources",
  "Actions": [
    "Microsoft.Compute/*",
    "Microsoft.Storage/*",
    "Microsoft.Web/*",
    "Microsoft.Authorization/*/write",
    "Microsoft.Authorization/*/delete",
    "Microsoft.Resources/subscriptions/resourceGroups/*",
    "Microsoft.KeyVault/vaults/secrets/*"
  ],
  "NotActions": [],
  "DataActions": [
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/*"
  ],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/12345678-1234-1234-1234-123456789012"
  ]
}
```

---

## 🔴 GCP IAM Policy - High Risk
**Expected Risk Level**: HIGH/CRITICAL  
**Key Issues**: Owner role binding, service account impersonation, project-wide access

```json
{
  "version": 1,
  "etag": "BwXhqDSt6L8=",
  "bindings": [
    {
      "role": "roles/owner",
      "members": [
        "serviceAccount:app-service@project-123456.iam.gserviceaccount.com",
        "user:developer@company.com"
      ]
    },
    {
      "role": "roles/iam.serviceAccountTokenCreator",
      "members": [
        "serviceAccount:app-service@project-123456.iam.gserviceaccount.com"
      ]
    },
    {
      "role": "roles/compute.admin",
      "members": [
        "group:developers@company.com"
      ]
    },
    {
      "role": "roles/storage.admin",
      "members": [
        "allUsers"
      ]
    },
    {
      "role": "roles/secretmanager.admin",
      "members": [
        "serviceAccount:app-service@project-123456.iam.gserviceaccount.com"
      ]
    }
  ]
}
```

---

## 🔴 Kubernetes RBAC - Critical Risk
**Expected Risk Level**: CRITICAL  
**Key Issues**: Cluster-admin equivalent permissions, wildcard resources/verbs

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: overprivileged-app-role
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "list", "create", "update", "delete"]
  resourceNames: []
- apiGroups: ["rbac.authorization.k8s.io"]
  resources: ["clusterroles", "clusterrolebindings", "roles", "rolebindings"]
  verbs: ["*"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: overprivileged-app-binding
subjects:
- kind: ServiceAccount
  name: app-service-account
  namespace: default
- kind: User
  name: developer@company.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: overprivileged-app-role
  apiGroup: rbac.authorization.k8s.io
```

---

## 🟡 Terraform AWS Configuration - Medium Risk
**Expected Risk Level**: MEDIUM/HIGH  
**Key Issues**: Overly permissive security groups, public S3 buckets, weak IAM policies

```hcl
# Overly permissive security group
resource "aws_security_group" "web_sg" {
  name        = "web-security-group"
  description = "Security group for web servers"

  ingress {
    from_port   = 0
    to_port     = 65535
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# Public S3 bucket with broad permissions
resource "aws_s3_bucket" "public_data" {
  bucket = "my-public-data-bucket-123456"
}

resource "aws_s3_bucket_policy" "public_policy" {
  bucket = aws_s3_bucket.public_data.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid       = "PublicReadGetObject"
        Effect    = "Allow"
        Principal = "*"
        Action    = ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"]
        Resource  = "${aws_s3_bucket.public_data.arn}/*"
      }
    ]
  })
}

# Overprivileged IAM role
resource "aws_iam_role_policy" "app_policy" {
  name = "app-policy"
  role = aws_iam_role.app_role.id

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "s3:*",
          "dynamodb:*",
          "lambda:*",
          "iam:PassRole",
          "ec2:*"
        ]
        Resource = "*"
      }
    ]
  })
}
```

---

## 🟠 Docker Compose - Medium Risk
**Expected Risk Level**: MEDIUM  
**Key Issues**: Privileged containers, host network mode, volume mounts

```yaml
version: '3.8'
services:
  web-app:
    image: nginx:latest
    privileged: true
    network_mode: host
    volumes:
      - /:/host:rw
      - /var/run/docker.sock:/var/run/docker.sock:rw
      - /etc/passwd:/etc/passwd:ro
    environment:
      - MYSQL_ROOT_PASSWORD=password123
    ports:
      - "80:80"
      - "443:443"
    user: root
    cap_add:
      - ALL
    security_opt:
      - seccomp:unconfined
      - apparmor:unconfined

  database:
    image: mysql:8.0
    privileged: true
    environment:
      - MYSQL_ROOT_PASSWORD=admin
      - MYSQL_ALLOW_EMPTY_PASSWORD=yes
    volumes:
      - /var/lib/mysql:/var/lib/mysql:rw
    network_mode: host
    user: "0:0"
    cap_add:
      - SYS_ADMIN
      - NET_ADMIN
```

---

## 🟢 AWS IAM Policy - Low Risk (Good Example)
**Expected Risk Level**: LOW  
**Key Issues**: Minor best practice improvements, but mostly secure

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3BucketAccess",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::my-app-bucket/uploads/*",
        "arn:aws:s3:::my-app-bucket/public/*"
      ],
      "Condition": {
        "StringEquals": {
          "s3:x-amz-server-side-encryption": "AES256"
        }
      }
    },
    {
      "Sid": "DynamoDBTableAccess",
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:Query",
        "dynamodb:UpdateItem"
      ],
      "Resource": "arn:aws:dynamodb:us-east-1:123456789012:table/UserProfiles"
    },
    {
      "Sid": "CloudWatchLogs",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:us-east-1:123456789012:log-group:/aws/lambda/my-app:*"
    }
  ]
}
```

---

## 🔴 HashiCorp Vault Policy - High Risk
**Expected Risk Level**: HIGH  
**Key Issues**: Root-equivalent permissions, wildcard paths, admin capabilities

```hcl
# Overly broad admin policy
path "*" {
  capabilities = ["create", "read", "update", "delete", "list", "sudo"]
}

# Database secrets with no restrictions
path "database/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# PKI with dangerous permissions
path "pki/*" {
  capabilities = ["create", "read", "update", "delete", "list", "sudo"]
}

# Auth method administration
path "auth/*" {
  capabilities = ["create", "read", "update", "delete", "list", "sudo"]
}

# System backend access
path "sys/*" {
  capabilities = ["create", "read", "update", "delete", "list", "sudo"]
}

# Identity secrets engine
path "identity/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}
```

---

## 🟡 Kubernetes Pod Security Context - Medium Risk
**Expected Risk Level**: MEDIUM  
**Key Issues**: Privileged containers, dangerous capabilities, host access

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: insecure-pod
  labels:
    app: risky-application
spec:
  hostNetwork: true
  hostPID: true
  hostIPC: true
  containers:
  - name: app-container
    image: nginx:latest
    securityContext:
      privileged: true
      runAsUser: 0
      runAsGroup: 0
      allowPrivilegeEscalation: true
      readOnlyRootFilesystem: false
      capabilities:
        add:
          - SYS_ADMIN
          - NET_ADMIN
          - SYS_PTRACE
          - SYS_MODULE
        drop: []
    volumeMounts:
    - name: host-root
      mountPath: /host
      readOnly: false
    - name: docker-socket
      mountPath: /var/run/docker.sock
    ports:
    - containerPort: 80
      hostPort: 80
  volumes:
  - name: host-root
    hostPath:
      path: /
      type: Directory
  - name: docker-socket
    hostPath:
      path: /var/run/docker.sock
      type: Socket
  serviceAccountName: admin-service-account
```



These examples demonstrate the tool's ability to analyze real-world security issues across multiple cloud platforms and infrastructure-as-code tools.
