# TrustGuard AI: Liveness Verification

A browser-based active liveness check for digital KYC. The user is asked to blink naturally twice within six seconds. A printed photo or a paused video cannot respond to this challenge, so those attempts are flagged for manual review. The prototype was built as the Round 2 proof of concept for TVS Credit EPIC 8.0.

## How It Works

Face detection and 68-point facial landmarks are computed in real time with `face-api.js` (Tiny Face Detector). For every frame, the eye aspect ratio (EAR) is calculated from the six landmarks around each eye and averaged across both eyes. A blink is registered when the EAR falls below 0.24 and then rises again. The check passes if two blinks are detected within the six-second window, and otherwise the result is a failure with escalation to manual KYC review.

All processing happens in the browser. No video or image is uploaded or stored anywhere.

| Setting | Value |
|---|---|
| Landmark model | Tiny Face Detector with 68-point tiny landmarks |
| EAR blink threshold | 0.24 |
| Blinks required | 2 |
| Challenge duration | 6 seconds |

## Running the Demo

The project is a single HTML file with no build step.

1. Open `cam.html` in a modern browser such as Chrome.
2. Click **Start Camera** and allow camera access.
3. Click **Start Liveness Challenge** and blink twice.

An internet connection is required on first load because `face-api.js` and the model weights are fetched from the jsDelivr CDN. If the browser blocks camera access when opening the file directly, serve the folder locally and open it from localhost:

```
python -m http.server 8000
```

Then visit `http://localhost:8000/cam.html`.

## Scope and Roadmap

This module covers the active challenge layer of the liveness pipeline. The planned next layer is passive deepfake-artifact scanning, including frame-consistency and lighting analysis, which would also protect against pre-recorded videos in which the person blinks. The EAR threshold can additionally be calibrated per user to handle differences in eye shape, glasses and lighting.
