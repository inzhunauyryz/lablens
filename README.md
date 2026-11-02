# LabLens: a plain-English lab result explainer

Built for **DSH Hacks V2** (theme: AI x Healthcare).

**Live demo:** https://inzhunauyryz.github.io/lablens

## The problem
Lab reports are full of numbers, abbreviations and reference ranges. Many people receive results before they can talk to a doctor and leave confused or anxious. LabLens helps adults understand their results in simple language and know what to ask their doctor.

## Features
- Three common panels: blood count, fasting glucose, lipids
- Status (low / in range / high / urgent) decided by a **deterministic rules engine**, never by the AI
- Urgent-value warning for dangerously abnormal results
- Sex-specific ranges and unit conversion (mg/dL and mmol/L)
- Optional field for the lab's own reference range, which takes priority over built-in values
- AI explanation (Google Gemini) in plain language with questions to ask a doctor
- Privacy by design: only the test name, value, unit and status are sent to the AI
- 14 built-in self-tests for the status logic

## How it works
```
Browser (index.html on GitHub Pages)
   -> rules engine classifies each value
   -> Cloudflare Worker (worker.js): hides the API key, allows only our site, validates input
   -> Google Gemini writes the explanation
   -> result shown with an "AI-generated" label
```
If the AI is unavailable, the rule-based results still work.

## Files
- `index.html`: the web app and rules engine
- `worker.js`: Cloudflare Worker that proxies requests to Gemini

## Run your own copy
1. Host `index.html` (for example with GitHub Pages).
2. Create a Cloudflare Worker with the code from `worker.js`.
3. Add the worker settings: `GEMINI_API_KEY` (secret), `ALLOWED_ORIGIN` (your site address, for example `https://yourname.github.io`) and optionally `MODEL`.
4. Set `AI_URL` in `index.html` to your worker address.

## Limitations
- Educational tool only. It does not diagnose and is not a medical device.
- Reference ranges were checked against MedlinePlus (blood counts), the American Diabetes Association (glucose) and the American Heart Association (cholesterol). Urgent thresholds follow published hospital critical-value lists (for example ARUP Laboratories) and are slightly more cautious in places. Labs differ, so use your own lab's range when available.
- AI text can contain mistakes. The status always comes from fixed rules.
- Adults only.

## Next steps
More languages (for example Kazakh and Russian), photo scanning of lab sheets, more panels, review by healthcare professionals.

## AI use disclosure
Developed with the help of an AI assistant (Claude by Anthropic) for planning and coding. Explanations are generated at runtime with Google Gemini.
