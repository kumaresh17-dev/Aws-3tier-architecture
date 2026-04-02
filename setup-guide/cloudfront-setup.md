# Secure CloudFront + S3 Setup (Using Origin Access Control)

---

## 📌 Overview

In this project:

- Amazon S3 is used to store static content  
- CloudFront is used to securely deliver content  
- S3 is NOT public  
- Only CloudFront can access S3  

👉 This ensures high security

---

## 🏗️ Architecture Flow

User → Route53 → CloudFront → S3 (Private)

---

# 🧱 PART 1: S3 SETUP (PRIVATE BUCKET)

---

## Step 1: Create S3 Bucket

1. Go to AWS Console → S3  
2. Click **Create bucket**

### Configuration:

- Bucket name: my-secure-static-bucket  
- Region: same as project  
- Block Public Access: ✅ ENABLE ALL  

Click Create

---

## Step 2: Upload Static Files

Upload:

- index.html  
- CSS / images  

---

## ⚠️ IMPORTANT

👉 DO NOT enable:
- Static website hosting  
- Public access  

---

## Result

- S3 bucket is private  
- Files cannot be accessed directly  

---

# 🌍 PART 2: CLOUDFRONT SETUP

---

## Step 3: Create CloudFront Distribution

1. Go to CloudFront  
2. Click **Create Distribution**

---

## Step 4: Origin Configuration

- Origin type: S3  
- Select your bucket  

---

## Step 5: Create Origin Access Control (OAC)

1. In origin settings → Click **Create new OAC**

### Settings:

- Name: my-oac  
- Signing behavior: Sign requests  

Click Create

---

## Step 6: Attach OAC to Distribution

- Select created OAC  
- Enable "Yes, update bucket policy"

---

## Step 7: Viewer Settings

- Viewer Protocol Policy: Redirect HTTP → HTTPS  
- Allowed Methods: GET, HEAD  

---

## Step 8: Create Distribution

Click Create  
⏳ Wait for deployment

---

# 🔐 PART 3: BUCKET POLICY (AUTO OR MANUAL)

CloudFront will generate policy automatically.

If needed manually:
{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Principal": {
"Service": "cloudfront.amazonaws.com"
},
"Action": "s3:GetObject",
"Resource": "arn:aws:s3:::my-secure-static-bucket/*",
"Condition": {
"StringEquals": {
"AWS:SourceArn": "arn:aws:cloudfront::<account-id>:distribution/<distribution-id>"
}
}
}
]
}


---

# 🔄 Final Flow

User → CloudFront → S3  

❌ Direct S3 access blocked  
✔ Only CloudFront allowed  

---

# 🧪 Testing

1. Open CloudFront URL  
2. Try accessing S3 URL → should FAIL  
3. Access via CloudFront → should work  

---

# 🎯 Final Result

✔ S3 is private  
✔ CloudFront serves content  
✔ Secure architecture implemented  
✔ Best practice followed  

---

# ⚡ Common Mistakes

❌ Making S3 public  
❌ Not using OAC  
❌ Using static website endpoint  
❌ Direct S3 access enabled  

✔ Always use CloudFront + private S3  

---

# 💡 Pro Tips

- Enable caching  
- Enable compression  
- Enable logging (optional)  
- Use HTTPS only  

---

# 🧠 Why This is Important

- Prevents direct S3 exposure  
- Protects content  
- Improves performance  
- Industry best practice  
