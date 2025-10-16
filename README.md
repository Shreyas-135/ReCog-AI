# ReCog-AI

A simple Node.js web application that uses **Amazon Rekognition** to analyse images for labels, detect and extract text, and return face details (including emotions). See [https://aws.amazon.com/rekognition/](https://aws.amazon.com/rekognition/) for Rekognition service details.

---

## Table of contents

* [Features](#features)
* [Quick demo](#quick-demo)
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [Configuration (AWS credentials)](#configuration-aws-credentials)
* [Run the app](#run-the-app)
* [Usage / Endpoints](#usage--endpoints)
* [Security notes](#security-notes)
* [Project structure](#project-structure)
* [TODO / Roadmap](#todo--roadmap)
* [Troubleshooting](#troubleshooting)
* [Contributing](#contributing)
* [License](#license)

---

## Features

* Upload an image (JPG/PNG, < 5 MB).
* Get image labels (textual description / object labels).
* Extract printed text and words from images (OCR).
* Get face details and emotions (happy, sad, calm, etc.) returned by Rekognition.
* Simple, responsive EJS-based UI.

## Quick demo

1. Start the app (see below).
2. Open `http://localhost:3000/` in your browser.
3. Upload an image from `/images` or your local disk and inspect results.

## Prerequisites

* Node.js (v14+ recommended) and npm
* An AWS account with an IAM user that has the `AmazonRekognitionFullAccess` policy (or a least-privilege policy granting Rekognition APIs used by the app).

## Installation

```bash
# clone repository
git clone https://github.com/Shreyas-135/ReCog-AI.git
cd ReCog-AI

# install dependencies
npm install
```

## Configuration (AWS credentials)

Create a config from the sample and add your AWS access keys and region. **Never commit real credentials to git.**

```bash
cd config
cp aws-config-sample.json aws-config.json
# edit aws-config.json and add values for accessKeyId, secretAccessKey, region
```

`aws-config.json` (example format):

```json
{
  "accessKeyId": "AKIA...",
  "secretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
  "region": "us-east-1"
}
```

**Alternative (recommended):** Use standard AWS environment variables or shared credentials file instead of storing secrets in repo files. Example environment variables:

```bash
export AWS_ACCESS_KEY_ID=AKIA...
export AWS_SECRET_ACCESS_KEY=...
export AWS_REGION=us-east-1
```

## Run the app

```bash
# from project root
node app.js
```

By default the server listens on port `3000`. Open `http://127.0.0.1:3000/` in your browser.

## Usage / Endpoints

* `GET /` — UI home page (image upload form).
* `POST /upload` — Upload an image and run Rekognition jobs (labels, text, faces).
* Results are shown in the UI and include labels, detected text blocks, and face details returned from Rekognition.

**Notes on files:** drop sample images into `/images` and use the UI to upload them.

## Project structure

```
/config            # example AWS creds config file (do not commit secrets)
/controllers       # controllers: HTTP handlers + Rekognition API wrappers
/images            # sample images (add your own here)
/models            # UI data models
/public            # CSS, favicon, static assets
/screenshots       # screenshots used in README/UI
/views             # EJS templates and partials
app.js             # application entry point
```

## Security notes

* **Do not** commit `aws-config.json` or any file containing credentials.
* Prefer environment variables or AWS SDK credential providers.
* Lock down the IAM user with least privilege in production — give only the Rekognition actions the app needs.
* If deploying publicly, ensure HTTPS, rate limiting, and authentication.

## TODO / Roadmap

* Video recognition functionality (Rekognition Video)
* Capture webcam screenshots from browser
* Batch recognition and classification for folders / bulk uploads
* Continuous recognition (poll webcam frames)
* Text-to-speech for recognition results
* Hardware recognition prototype (edge device integration)

## Troubleshooting

**Common issues**

* `AccessDenied` from AWS Rekognition: check IAM policy and region.
* Large images rejected: ensure images are under 5 MB and in .jpg/.png format.
* Slow responses: Rekognition is a network call — check your network and region latency.

## Contributing

PRs and issues welcome. When contributing:

* Don’t add secrets or credentials.
* Add clear reproduction steps and sample images for bugs.
* Follow existing code style for controllers and views.

## License

MIT — see `LICENSE` if present or add one.

---

*Last updated: October 16, 2025 — updated README with clearer installation, security guidance, and usage notes.*
