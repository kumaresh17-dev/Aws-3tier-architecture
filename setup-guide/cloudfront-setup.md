# Secure CloudFront + S3 Setup (With Origins & Behaviors)

---

## 📌 Overview

This setup uses:

- Amazon S3 → Private static storage  
- CloudFront → CDN + secure access layer  
- Origin Access Control (OAC) → restrict S3 access  

---

## 🏗️ Architecture Flow

User → Route53 → CloudFront → S3 (Private)

---

# 🧱 PART 1: S3 SETUP (PRIVATE)

---

## Step 1: Create S3 Bucket

- Bucket name: my-secure-static-bucket  
- Block Public Access: ✅ ENABLE ALL  

---

## Step 2: Upload Files

- index.html  
- /static (CSS, JS, images)

---

## ⚠️ Important

- Do NOT enable static website hosting  
- Do NOT allow public access  

---

# 🌍 PART 2: CLOUDFRONT SETUP

---

## Step 3: Create Distribution

Go to CloudFront → Create Distribution

---

## Step 4: Origin Configuration

### 🔹 Origin 1 (S3)

- Origin Name: s3-origin  
- Origin Type: S3  
- Select bucket  

---

## Step 5: Create Origin Access Control (OAC)

- Name: my-oac  
- Signing: Enabled  

Attach to S3 origin

---

## Step 6: (Optional Advanced) Add Second Origin (ALB)

👉 Only if using dynamic routing

- Origin Name: alb-origin  
- Origin Type: Custom  
- Domain: ALB DNS  

---

# 🎯 Step 7: Behaviors (VERY IMPORTANT)

---

## 🔹 Default Behavior (/*)

- Path Pattern: `*`  
- Origin: s3-origin  
- Viewer Protocol Policy: Redirect HTTP → HTTPS  
- Allowed Methods: GET, HEAD  
- Cache Policy: CachingOptimized  

👉 All default traffic → S3

---

## 🔹 Additional Behavior (Optional)

### Example: API routing

- Path Pattern: `/api/*`  
- Origin: alb-origin  
- Allowed Methods: GET, POST, PUT, DELETE  

👉 API calls → ALB

---

## 🔹 Static Files Optimization

- Path Pattern: `/static/*`  
- Origin: s3-origin  
- Cache Policy: Aggressive caching  

---

# ⚙️ Step 8: Distribution Settings

- Price Class: All locations  
- Alternate Domain: optional  
- SSL: Default CloudFront  

---

## 🚀 Step 9: Create Distribution

Click Create  
Wait until **Status = Deployed**

---

# 🔐 PART 3: S3 BUCKET POLICY (AUTO BY OAC)

CloudFront will auto update

---

# 🔄 Final Flow

Static:
User → CloudFront → S3  

Dynamic (optional):
User → CloudFront → ALB  

---

# 🧪 Testing

- Open CloudFront URL  
- `/index.html` → S3  
- `/static/file.css` → S3  
- `/api/test` → ALB (if configured)

---

# 🎯 Final Result

✔ Secure S3 (no public access)  
✔ CloudFront CDN enabled  
✔ Path-based routing working  
✔ Optimized caching  

---

# ⚡ Common Mistakes

❌ No behavior configuration  
❌ Wrong origin mapping  
❌ Public S3 enabled  
❌ Using S3 website endpoint  

✔ Always use OAC + correct origins  

---

# 💡 Pro Tips

- Enable compression  
- Enable logging  
- Use custom domain (Route53)  
- Use HTTPS only  

---

# 🧠 Why Origins & Behaviors Matter

- Origins define backend sources (S3, ALB)  
- Behaviors define routing rules  

👉 Without behaviors → no control  
👉 With behaviors → production-level routing  
