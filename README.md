# Meta Ad Library Scraper & Multimodal Creative Analysis Pipeline

An automated, direct-response creative intelligence system built with **n8n**, **Apify**, and **Google Gemini 1.5 Flash**.

This repository provides an end-to-end workflow to scrape active acquisition creatives from the Meta (Facebook & Instagram) Ad Library, extract their direct-response structural anatomy (**Hook**, **Body copy**, and **Call-to-Action**), and compute an objective **AIDA-L Composite Performance Score** to identify top-performing creatives.

---

## Architecture Overview

The pipeline executes as a clean, linear workflow with zero circular state dependencies:

```
[Manual / Scheduled Trigger]
             │
             ▼
[Workflow Configuration] (Target URL, Brand Query, Model Settings)
             │
             ▼
[Build Scraper Request] (Apify Actor Parameter Formulation)
             │
             ▼
[Apify Meta Ad Library Scraper] (Synchronous Dataset Retrieval)
             │
             ▼
[Normalize Ads Data] (Schema Harmonization: Copy, Video CDN Streams, Longevity)
             │
             ▼
[Prepare Gemini Prompt] (Direct-Response Multimodal Prompting)
             │
             ▼
[Google Gemini 1.5 Flash] (Multimodal Creative Extraction via JSON Schema)
             │
             ▼
[Score Ad & Deconstruct] (AIDA-L Weighted Scoring Engine)
             │
             ▼
[Rank Ads & Select Best Winner] (Algorithmic Ranking & Executive Synthesis)
```

---

## Core Capabilities

1. **Multimodal Video & Creative Analysis:**  
   Evaluates vertical video creatives (9:16 UGC skits, motion graphics, screen recordings) and static banners. Decouples visual thumb-stop rates (first 3 seconds) from verbal audio hooks.

2. **Empirical Longevity Signal:**  
   Leverages Meta's `sort_data[mode]=total_impressions&sort_data[direction]=desc` parameter. In Meta media buying, unprofitable ads are eliminated within 72 hours. Sustained delivery duration (e.g. 40–70+ days) is factored into the scoring engine as empirical proof of positive ROAS and stable Customer Acquisition Cost (CAC).

3. **Quantitative AIDA-L Scoring Matrix:**  
   Replaces subjective creative evaluations with a mathematical index:
   $$\text{Score} = (\text{Hook} \times 0.30) + (\text{Body} \times 0.25) + (\text{CTA} \times 0.20) + (\text{Creative Format} \times 0.10) + (\text{Meta Longevity} \times 0.15)$$

4. **Zero-Cost Operation:**  
   Runs entirely on free tiers using Apify's perpetual free allocation and Google AI Studio's Gemini 1.5 Flash free quota (15 RPM). Includes an offline deterministic fallback dataset for zero-latency demonstrations.

---

## Project Structure

```
├── workflow.json                          # Complete, import-ready n8n workflow
├── facebook_ad_library_mysivi_report.html # Executive HTML audit report (Print-ready CSS for PDF export)
├── facebook_ad_library_mysivi_report.md   # Markdown summary of findings and scorecards
├── README.md                              # Technical documentation
└── .gitignore                             # Ignored local files and environments
```

---

## Quick Start Guide

### Prerequisites
* An active instance of **n8n** (Cloud, Self-Hosted Docker, or Desktop v1.0+).
* (Optional) **Apify API Token** from [apify.com](https://apify.com) for live scraping.
* (Optional) **Google Gemini API Key** from [aistudio.google.com](https://aistudio.google.com) for live multimodal analysis.

### Step 1: Import Workflow
1. Open your n8n dashboard.
2. In the top navigation, click **"Add workflow"** → Click `...` (options) → **"Import from File"**.
3. Select `workflow.json`.

### Step 2: Configure Environment
Double-click the **`Workflow Configuration`** node to view or update parameters:
```javascript
{
  // Target Meta Ad Library URL (India, Active, Sorted by Total Impressions)
  adLibraryUrl: "https://www.facebook.com/ads/library/?active_status=active&ad_type=all&country=IN&is_targeted_country=false&media_type=all&q=%22MySivi%22&search_type=keyword_exact_phrase&sort_data[direction]=desc&sort_data[mode]=total_impressions",
  brandQuery: "MySivi",
  targetCountry: "IN",
  apifyApiToken: "DEMO_FREE_TIER",  // Insert your Apify token for live runs
  geminiApiKey: "DEMO_GEMINI_KEY",   // Insert your Google AI Studio key for live runs
  geminiModel: "gemini-1.5-flash",
  maxAdsToScrape: 5,
  useMockDataFallback: true          // Set to true for offline demo mode
}
```

### Step 3: Execute Workflow
Click **"Execute Workflow"** (or **"Test workflow"**). All nodes will process sequentially, delivering structured JSON output at the `Rank Ads & Select Best Winner` terminal node.

---

## Sample Analytical Output

### Primary Creative Selected: Ad Archive ID `108492019348123`
* **Format:** 9:16 Vertical UGC Video (34 Seconds)
* **Scale Status:** 68+ Days Active | 100K – 500K Impressions
* **Composite Score:** **94.5 / 100**
* **Extracted Hook:** *"English samajh aati hai, par bolte waqt atakte ho? Interview mein 'Introduce yourself' sunte hi dimaag blank ho gaya?"*
* **Extracted Body:** *"Stop mugging up grammar rules. Meet Arya, your personal AI English Coach on MySivi. Practice 1:1 mock interviews anytime... bina kisi judgement ke! 15 minutes daily."*
* **Extracted CTA:** *"Download MySivi & start speaking fluently today! [Install Now]"*

For complete comparative matrices and video timeline teardowns, refer to `facebook_ad_library_mysivi_report.html`.

---

## License
MIT License. Free for commercial and non-commercial use.
