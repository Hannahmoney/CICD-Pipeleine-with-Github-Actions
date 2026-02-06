# Deploy React App to AWS S3 (GitHub Actions)

This repo deploys a React build to an AWS S3 bucket automatically using GitHub Actions.

## What the pipeline does
On every push to the `main` branch, GitHub Actions will:
1. Checkout the code
2. Install Node.js
3. Install dependencies (`npm install`)
4. Build the React app (`npx react-scripts build`)
5. Configure AWS credentials from GitHub Secrets
6. Sync the `build/` folder to your S3 bucket

---

## Prerequisites (AWS)

### 1) Create an S3 bucket
Create an S3 bucket (example used in the workflow):
- `githubactions-cicdz`

Make sure it is in the same region as your workflow (`us-east-1`).

### 2) Enable static website hosting (optional but common for React)
In S3:
- Go to **Properties** → **Static website hosting**
- Enable it
- Set **Index document** to: `index.html`

### 3) Make the bucket public (Public Read for GetObject)
You need public access so people can load the website files.

**A. Allow public access**
- S3 → your bucket → **Permissions**
- Turn OFF **Block all public access** (only for this bucket)
  - Be careful, this makes it possible to serve content publicly

**B. Add a bucket policy**
Replace `githubactions-cicdz` with your bucket name:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::githubactions-cicdz/*"
    }
  ]
}
