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
aws s3 sync . s3://fia-test-web-cloudfront-821788677871/ --exclude ".git/*" --exclude "Intelligent-Feedback-Agent-FIA/*" --exclude ".cursor/*" --exclude ".claude/*" --delete
aws cloudfront create-invalidation --distribution-id EYRDYX9BP9YNQ --paths "/*"
```

**Note:** the `--exclude` flags above matter — `aws s3 sync` mirrors the whole
directory verbatim and is NOT git-aware, so without them it also publishes
whatever FIA install files exist locally (including the owner's secret
`fia.owner.local.json`) to this public CloudFront distribution.

Live URL: **https://d3j616vws7mkty.cloudfront.net**

## Register this tool on FIA

System type: **Web widget**
Allowed Origins: `https://d3j616vws7mkty.cloudfront.net`

## Teardown

When resetting/uninstalling this test, also remove the AWS resources (not
just the FIA kit files): delete the CloudFront distribution
(`EYRDYX9BP9YNQ`, must be disabled first, then deleted once fully
disabled) and the S3 bucket (`fia-test-web-cloudfront-821788677871`).
