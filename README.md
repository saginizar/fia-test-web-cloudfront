# fia-test-web-cloudfront

Dummy static site for testing the FIA **CloudFront / S3 static site** (yoschechter-style) install flow.

## Purpose

Simulates an internal static guide or docs site hosted on S3 + CloudFront.  
**No `package.json`, no npm build pipeline** — it's a plain static site.

The guide should:
- Find `index.html` at the repo root and embed the widget tag
- Recognise this as a **static site** (no `vite.config.*`, no `package.json`, no `dist/` folder)
- At the deploy step: tell the owner to push the file to S3 (e.g. `aws s3 sync . s3://my-bucket/`) — not run npm build

## Deploy (real workflow)

Real bucket + distribution (thin: private S3 bucket + CloudFront Origin
Access Control, no static-website-hosting mode, no custom domain):

```
aws s3 sync . s3://fia-test-web-cloudfront-821788677871/ --exclude ".git/*" --delete
aws cloudfront create-invalidation --distribution-id EYRDYX9BP9YNQ --paths "/*"
```

Live URL: **https://d3j616vws7mkty.cloudfront.net**

## Register this tool on FIA

System type: **Web widget**
Allowed Origins: `https://d3j616vws7mkty.cloudfront.net`

## Teardown

When resetting/uninstalling this test, also remove the AWS resources (not
just the FIA kit files): delete the CloudFront distribution
(`EYRDYX9BP9YNQ`, must be disabled first, then deleted once fully
disabled) and the S3 bucket (`fia-test-web-cloudfront-821788677871`).
