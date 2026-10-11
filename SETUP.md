# Launch and moderation setup

## Publish the site on Vercel

1. In Vercel, import the `Com-Adarsh/Digital-Wall` GitHub repository and select the repository root as the project root.
2. Use the build command `bundle exec jekyll build` and output directory `_site`. Vercel may detect Jekyll automatically.
3. Deploy the project. Vercel creates a production deployment from `main` and preview deployments for proposed changes.
4. Protect `main` so only editors can merge. A merge triggers the production deployment.
5. For a custom domain, add it in Vercel's project settings and configure the DNS records Vercel provides.

## Create the public form

Create a Tally form with required fields `Author name`, `Category` (Stories, Art & making, Campus life, Ideas), `Title`, `Post text`, and a consent checkbox confirming authorship/permission to publish. Optional fields: `Image URL`, `Image alt text`, and a private `Contact email` for editorial follow-up. Never include contact email in the public post. Explain that approved posts become public and tell students how to request a correction or removal.

The published form at `https://tally.so/r/eq170e` is already embedded on `/submit/` with dynamic height. Keep form responses private and set a retention period.

## Connect Tally to GitHub through Make

The flow is **new Tally response → validate → create a branch and Markdown file → open a pull request**. Never publish student submissions directly to `main`.

1. In Make, start with Tally's `Watch New Responses` trigger and select the published form. This is a native Tally-to-Make integration; a custom webhook is an alternative.
2. Require title, author, category, post text, and consent. Accept only the four categories. Limit title length and omit contact email from the generated file. Escape HTML characters in student text before saving it as Markdown so submitted HTML cannot run on the public site.
3. Create a unique branch such as `submission/<response-id>`.
4. Create `_stories/YYYY-MM-DD-<response-id>.md` on that branch; use the response ID, not student text, in the filename.
5. Format the content using the template below. Escape metadata correctly; keep submitted text outside YAML front matter.
6. Open a PR from the branch to `main`, include the response ID in the PR body to prevent duplicates, and apply an editorial label if available.
7. Do not let the integration approve or merge PRs. A human editor reviews text, consent, safety, image rights, and formatting before merging.

Make's GitHub connector may not offer branch, file, and PR creation in one module. If needed, use Make's HTTP module with GitHub's REST API to create a branch ref from the current `main` commit, create the file on that branch, and open the PR. Store the token only in Make's secured connection/secret store, scope it to this repository, and grant only the necessary contents and pull-request permissions. Never put a token in the form or repository. Test end-to-end with a sample submission before launch.

### Markdown template

```markdown
---
title: "Student supplied title"
author: "Preferred name"
category: "Stories"
date: 2026-10-11
image: "https://example.com/public-image.jpg"
image_alt: "Short image description"
---

Student supplied post text goes here.
```

Omit image fields when there is no image. Verify image links are public, stable, and permitted to publish. For uploaded files, confirm their link remains accessible; do not expose private information in the public Markdown.

## Review and publish

Editors request changes or close the PR if needed. Merge approved submissions. The push to `main` starts the Vercel deployment and publishes the story. To remove a post, delete its Markdown file in a reviewed commit; the next deployment removes it from the site.

Assign at least two trusted editors, enable two-factor authentication, and require human review for submission PRs. Keep Make logs and form responses private. Test the entire path from form to live site before sharing the form publicly.
