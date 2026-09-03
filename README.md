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

```
aws s3 sync . s3://my-cloudfront-bucket/ --exclude ".git/*" --delete
aws cloudfront create-invalidation --distribution-id XXXXX --paths "/*"
```

## Register this tool on FIA

System type: **Web widget**  
Allowed Origins: `https://d3testexample12345.cloudfront.net` *(or your actual CF URL)*
