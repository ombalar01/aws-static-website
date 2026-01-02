# aws-static-website
# 🌐 AWS Static Website Hosting using S3, GitHub & GitHub Actions (CI/CD)

This project demonstrates how to host a **static website on Amazon S3** and automate deployment using **GitHub Actions CI/CD**.  
Any change pushed to the GitHub repository is automatically deployed to the S3 bucket.

---

## 🚀 Live Demo
🔗 **Website URL:**  
http://<your-s3-bucket-name>.s3-website-<region>.amazonaws.com

---

## 📌 Project Overview

- Static website built using **HTML & CSS**
- Hosted on **Amazon S3 (Static Website Hosting)**
- Source code managed using **GitHub**
- Automated deployment using **GitHub Actions**
- CI/CD pipeline uploads files to S3 on every push

---

## 🛠️ Technologies Used

- **Amazon S3** – Static website hosting
- **GitHub** – Source code management
- **GitHub Actions** – CI/CD automation
- **HTML5 & CSS3** – Frontend
- **AWS IAM** – Secure access for deployment

---


---

## ⚙️ AWS S3 Configuration Steps

1. Create an **S3 bucket**
   - Bucket name must be **globally unique**
   - Disable **Block all public access**

2. Enable **Static Website Hosting**
   - Index document: `index.html`

3. Add **Bucket Policy** to allow public read access

4. Note the **S3 website endpoint URL**

---

## 🔐 IAM Setup (For GitHub Actions)

1. Create an **IAM User**
2. Attach policy:
   - `AmazonS3FullAccess` *(or limited S3 policy)*
3. Generate:
   - **AWS_ACCESS_KEY_ID**
   - **AWS_SECRET_ACCESS_KEY**

---

## 🔑 GitHub Secrets Configuration

In your GitHub repository:

**Settings → Secrets and variables → Actions → New repository secret**

Add:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`
- `S3_BUCKET_NAME`

---

## 🔄 CI/CD Pipeline (GitHub Actions)

Whenever you push code to the `main` branch:
- GitHub Actions is triggered
- Website files are uploaded to S3 automatically
- Live website updates instantly

---

## 📄 Sample GitHub Actions Workflow (`deploy.yml`)

```yaml
name: Deploy Static Website to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Deploy to S3
        run: |
          aws s3 sync . s3://${{ secrets.S3_BUCKET_NAME }} --delete


