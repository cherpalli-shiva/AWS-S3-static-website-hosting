# Project-3: AWS S3 Static Website Hosting 🚀

![AWS](https://img.shields.io/badge/AWS-S3-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Static Website](https://img.shields.io/badge/Static-Website-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![HTML](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)

> Deployed a static portfolio website on Amazon S3 with public access configuration, bucket policy management, and static website hosting — part of the **DevOps Zero to Hero** roadmap.

---

## 🌐 Live URL

```
http://amazon-s3-demo-website.s3-website-us-east-1.amazonaws.com
```

---

## 📌 What This Project Covers

- Creating an S3 bucket with a globally unique name
- Configuring **Block Public Access** settings
- Writing and applying an **S3 Bucket Policy** (JSON)
- Enabling **Static Website Hosting** on S3
- Uploading `index.html` and making it publicly accessible
- Diagnosing and fixing **IAM / Access Denied** errors

---

## 🏗️ Architecture

```
User (Browser)
      │
      ▼
Amazon S3 Bucket
(amazon-s3-demo-website)
      │
      ├── Static Website Hosting: Enabled
      ├── Block Public Access: OFF
      └── Bucket Policy: Public GetObject
            │
            ▼
        index.html served via HTTP
```

---

## 🪣 S3 Bucket Configuration

### Bucket Details

| Property         | Value                          |
|-----------------|-------------------------------|
| Bucket Name     | `amazon-s3-demo-website`      |
| Region          | `us-east-1`                   |
| Hosting Type    | Static Website                |
| Access          | Public (read-only)            |
| Index Document  | `index.html`                  |

---

## 📋 Bucket Policy (JSON)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amazon-s3-demo-website/*"
    }
  ]
}
```

---

## 🔧 Step-by-Step Setup

### Step 1: Create S3 Bucket

```bash
aws s3 mb s3://amazon-s3-demo-website --region us-east-1
```

### Step 2: Disable Block Public Access

Via AWS Console:
> S3 → Bucket → Permissions → Block Public Access → Edit → Uncheck All → Save

### Step 3: Apply Bucket Policy

Via AWS Console:
> S3 → Bucket → Permissions → Bucket Policy → Edit → Paste JSON above → Save

### Step 4: Enable Static Website Hosting

Via AWS Console:
> S3 → Bucket → Properties → Static Website Hosting → Edit → Enable → Index document: `index.html` → Save

### Step 5: Upload Website Files

```bash
# Upload single file
aws s3 cp index.html s3://amazon-s3-demo-website/

# Or sync entire folder
aws s3 sync . s3://amazon-s3-demo-website/
```

### Step 6: Access Your Website

```
http://<bucket-name>.s3-website-<region>.amazonaws.com
```

---

## 🐛 Errors Encountered & Fixes

### Error 1: Access Denied on Block Public Access Settings

```
User: arn:aws:iam::461837195980:root is not authorized to perform:
s3:GetBucketPublicAccessBlock with an explicit deny in a resource-based policy
```

**Root Cause:** Existing bucket policy had an explicit `Deny` for all principals — which overrides even the root account.

**Fix:**
1. Go to S3 → Permissions → Bucket Policy → Edit
2. Delete the entire existing policy content
3. Save (empty policy = no restrictions)
4. Then disable Block Public Access
5. Re-apply the correct public read policy

---

### Error 2: 403 Forbidden after enabling Static Website Hosting

**Root Cause:** Block Public Access was still enabled — S3 blocks public reads even with hosting enabled.

**Fix:** Disable all Block Public Access options first, then re-upload and apply bucket policy.

---

## 📂 Project Structure

```
project-3-s3-static-website/
├── index.html          # Main website file (DevOps portfolio)
├── README.md           # This file
└── bucket-policy.json  # S3 bucket policy
```

---

## 💡 Key Learnings

| Concept | What I Learned |
|---------|---------------|
| S3 Bucket Policy | Explicit `Deny` always wins — even over root account |
| Block Public Access | Must be disabled BEFORE applying public bucket policy |
| Static Hosting | S3 serves files via HTTP — HTTPS requires CloudFront |
| IAM vs Bucket Policy | Bucket policy is resource-level; overrides IAM grants |
| Global Unique Names | S3 bucket names must be unique across ALL AWS accounts |

---

## 🔐 Security Notes

> ⚠️ This bucket is intentionally public for demo purposes.
> For production use:
> - Enable HTTPS via **CloudFront**
> - Use **S3 Origin Access Control (OAC)** instead of public access
> - Never expose sensitive data in public buckets

---

## 🗺️ DevOps Roadmap Progress

| Project | Description | Status |
|---------|------------|--------|
| Project-1 | AWS EC2 Static Website (Apache2 + Node.js) | ✅ Done |
| Project-2 | Jenkins CI/CD Pipeline Setup | ✅ Done |
| **Project-3** | **AWS S3 Static Website Hosting** | ✅ **Done** |
| Project-4 | Terraform + Docker + Kubernetes | 🔜 Next |

---

## 📚 References

- [AWS S3 Static Website Hosting Docs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [S3 Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html)
- [Block Public Access Settings](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)

---

## 👤 Author

**Shiva Cherpalli** — DevOps / Cloud Engineer

[![GitHub](https://img.shields.io/badge/GitHub-cherpalli--shiva-181717?style=flat&logo=github)](https://github.com/cherpalli-shiva)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-cherpalli--shiva-0077B5?style=flat&logo=linkedin)](https://linkedin.com/in/cherpalli-shiva)
