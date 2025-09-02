# Universal Cloud Policy Risk Lens - Visual Walkthrough

This guide demonstrates the complete workflow of analyzing cloud policies using the Universal Cloud Policy Risk Lens, from initial policy input through final remediation recommendations.

---

## 📸 Screenshot Guide

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

## 📊 Comparison Screenshots

### Before/After Risk Score Comparison
*<img width="2068" height="925" alt="Image" src="https://github.com/user-attachments/assets/125902be-6ecb-4369-b947-b4360c9e9cf7" />*

**After a few remediation implementations, the Risk Score is dramatically decreased.**

<img width="2067" height="917" alt="Image" src="https://github.com/user-attachments/assets/21647178-23bd-412f-8133-735b0a505c0d" />


```

### Policy Code Comparison
*[Screenshot: policy-code-before-after.png]*

**Side-by-side code blocks showing:**
- **Before**: Dangerous wildcards and broad permissions
- **After**: Scoped permissions with proper conditions
- **Highlighting**: Changes in different colors






### Findings Reduction Table
*[Screenshot: findings-reduction-table.png]*

**Table format:**
| Finding Type | Before | After | Improvement |
|--------------|--------|--------|-------------|
| Critical Issues | 5 | 0 | ✅ 100% Resolved |
| High Risk | 8 | 1 | ✅ 87% Reduced |
| Medium Risk | 12 | 3 | ✅ 75% Reduced |
| **Total Risk Score** | **CRITICAL** | **LOW** | **✅ Major Improvement** |

---

## 🎯 Multi-Platform Demo Screenshots

### Platform Detection Showcase
*[Screenshot: multi-platform-detection.png]*

**Grid layout showing:**
- AWS IAM → "Platform Detected: AWS IAM"
- Azure RBAC → "Platform Detected: Azure RBAC"  
- GCP IAM → "Platform Detected: GCP IAM"
- Kubernetes → "Platform Detected: Kubernetes RBAC"

### Cross-Platform Risk Patterns
*[Screenshot: cross-platform-patterns.png]*

**Shows similar security anti-patterns across platforms:**
- Wildcard permissions (AWS: "s3:*", Azure: "Microsoft.Compute/*")
- Privilege escalation (GCP: "roles/owner", K8s: "cluster-admin")
- Resource overbreadth (All platforms showing broad access)

---


This walkthrough structure will create a compelling visual story that demonstrates both technical expertise and practical business value.
