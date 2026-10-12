# Week 8 — Static Website Hosting with Amazon S3 and CloudFront

**Topic:** Managing Static and Dynamic Content Delivery with Amazon S3 and CloudFront Integration
**Original session date:** July 26, 2025

## Goals
- [x] Build a simple personal static website (an About Me page)
- [x] Upload the HTML files to an Amazon S3 bucket
- [x] Enable static website hosting on the bucket
- [x] Configure the bucket for public access with an appropriate bucket policy
- [x] Create a CloudFront distribution that uses the S3 bucket as its origin
- [x] Access the live website through the CloudFront domain

## What I did
Built a simple **HTML and CSS About Me page** to use as the static site.

Created an S3 bucket and uploaded the website files to it, then
enabled **static website hosting** in the bucket's properties,
setting the index document to my About Me page. To make the content
readable by visitors, I turned off the bucket's block public access
setting and added a **bucket policy** allowing public read access to
the objects.

Next, I created a **CloudFront distribution** with the S3 bucket as
its origin, so the site is delivered through CloudFront's global edge
network instead of directly from S3. Once the distribution's status
changed to **Deployed**, I opened the site using the CloudFront domain
name and confirmed the About Me page loaded correctly.

| Setting | Value used |
|---------|-----------|
| Bucket name | (csn-static-about-me-puseletso-website) |
| Index document | (`about.html`) |
| CloudFront origin | (S3 static website endpoint, public access (no origin access control)) |
| Viewer protocol policy | (Redirect HTTP to HTTPS) |

## Key takeaways
- S3 can host a static website with no servers to manage, but a
  bucket serving a site publicly needs both block public access
  turned off and a bucket policy that grants read access.
- CloudFront sits in front of S3 and serves cached copies from edge
  locations close to visitors, which improves load times and lets the
  site be served over HTTPS on the CloudFront domain.
- A new distribution takes a few minutes to reach the **Deployed**
  status, and the CloudFront domain won't work reliably until it does.
- CloudFront caches content, so after changing a file in S3 an old
  version can keep appearing until the cache expires or an
  invalidation is created.
- A public bucket policy keeps the files readable straight from S3
  as well as through CloudFront. A tighter setup keeps the bucket
  private and lets only CloudFront read it using origin access
  control, which I want to try next.

## Cleanup
After completing and documenting this task, I deleted the resources
I created to stay within AWS Free Tier limits and avoid unexpected
charges. Resources removed: the CloudFront distribution, and the S3
bucket with its files.

## Screenshots / Evidence
**S3 bucket with static website hosting enabled**
<img width="1891" height="876" alt="Screenshot 2026-10-12 103642" src="https://github.com/user-attachments/assets/5eb4070d-ca3b-458c-9bd3-b9ae6364891a" />

**Bucket policy allowing public access**
<img width="1875" height="837" alt="Screenshot 2026-10-12 103711" src="https://github.com/user-attachments/assets/88015833-4314-4562-b7f0-fdb2496dd65c" />

**CloudFront distribution settings and Deployed status**
<img width="1877" height="865" alt="Screenshot 2026-10-12 103309" src="https://github.com/user-attachments/assets/7a57c0d5-04f9-4cd4-84b4-a2a0a87cfdc3" />

**Live website served through the CloudFront domain**
<img width="1872" height="956" alt="Screenshot 2026-10-12 103342" src="https://github.com/user-attachments/assets/e2eea933-eace-4886-a8f7-dbd718da432c" />
