# Transparent PNG API: Geekflare Comparison Shows Cheaper Pricing, How to Actually Use Free Tier and 500 Free Credits for Developers Building Dashboards, Newsletters, and Automated Reports (Full Cost Breakdown Inside)

Developers building dashboards, newsletters, or automated reports constantly hit a wall when they need a reliable, transparent PNG of a live web page. The pain isn't just "how do I get an image?" – it's "how do I get an image that looks exactly like a real browser, respects ad-blocking, runs JavaScript, and doesn't explode my monthly invoice because the provider sneaks in hidden multipliers?"

Enter the **Geekflare Screenshot API**. It offers a real-browser, headless-Chrome rendering pipeline that returns a transparent PNG (or JPEG/WebP) with full control over viewport, device emulation, and post-capture tweaks such as background removal or timestamp watermarks. All of this lives behind a single API key that also powers Geekflare's other data-extraction APIs, so you never have to juggle dozens of credentials.

In this guide you'll get an end-to-end walk-through of how the API works, how Geekflare's credit system translates into real-world costs, a concise comparison with four well-known competitors, and a production-ready code sample you can paste into a CI job today. I'll also expose the limitations that matter when you're planning a large-scale deployment, because surprise credit invoices are the last thing any backend engineer wants.

> **TL;DR** – If you need a transparent PNG that matches a user's browser, want predictable billing, and value a highly available service backed by Google Cloud Platform and Cloudflare, Geekflare is the provider that keeps the math honest while still offering advanced controls like geo-proxy rendering and AI-vision annotations.

---

## How Credit Multipliers Inflate Your Monthly Invoice

Most screenshot services advertise a "price per capture" or a "price per 1 K images." Those numbers look tidy until you start mixing in optional features—proxy routing, higher-resolution captures, or AI-enhanced extraction. Each add-on adds a **credit multiplier** that is rarely disclosed up front.

Geekflare's model is deliberately simple: every API call consumes a **fixed number of credits**. The credit cost is the same regardless of the HTTP status (5xx failures are free) and the only way to spend more credits is to ask for more work. Below is a quick credit-budget exercise that shows why other vendors can look cheap on paper but end up costing far more once you enable the features you actually need.

| Scenario | Credits per call | Monthly free credit pool | Credits needed for 1,000 captures | Credits needed for 1,000 **proxy** captures |
|----------|------------------|--------------------------|-----------------------------------|---------------------------------------------|
| Basic PNG (no proxy) | 5 | 500 | 5,000 | – |
| PNG with geo-proxy (paid plan only) | 9 | – | – | 9,000 |

If you run a daily newsletter that snapshots the top story on **Hacker News** (`https://news.ycombinator.com`) and you want a transparent PNG for each edition, you'll need roughly **30 captures per month**. That fits comfortably in the free tier (500 credits ≈ 100 captures). However, if you also need to route those captures through a US-based proxy to avoid geo-blocking, each request jumps from 5 to 9 credits. That bumps you into the **Starter** plan (10,000 credits) with a modest $19 /mo price tag.

Now compare that to a provider that charges $0.01 per capture **plus** $0.005 for each proxy hop. The headline price looks lower, but the *effective* cost per capture becomes $0.015 – a noticeable increase that only shows up on your invoice after you've already built the feature. Geekflare avoids this surprise by bundling the proxy cost into a **+4-credit add-on** that only appears on paid plans, and by making the credit cost per call **transparent** in the docs.

### Real-World Credit Math (No Hidden Multipliers)

- **Standard capture** – 5 credits → Free tier gives ~100 captures, Starter $9.50 / 1 K captures, Growth $3.45 / 1 K captures, Business $1.75 / 1 K captures.  
- **Proxy-enabled capture** – 9 credits (5 + 4) → Starter $17.10 / 1 K, Growth $6.21 / 1 K, Business $3.14 / 1 K.  

Notice that the per-1 K dollar figures are already divided by the credit cost, so you can directly map a workload to a plan without any hidden arithmetic.

**Why Credit-Based Billing Beats "Per-Capture" Pricing**

1. **Predictability** – You know exactly how many captures a plan supports.  
2. **Scalability** – If you need more, you either upgrade the tier or purchase a one-time credit pack (Spark, Blaze, Inferno, Supernova).  
3. **No rollover surprises** – Credits reset on the 1st of each month, so you never carry an unused balance that silently inflates the next bill.  

---

## Geekflare Subscription Tiers

Geekflare's pricing sheet lists five tiers. All tiers share the same **credit pool** across all 14 APIs, so you can mix screenshot calls with SERP, web-scraping, or AI extraction without opening separate accounts.

| Tier | Monthly Cost | Credits Included | Credit Cost | Rate Limit (req/s) | Team Seats | Log Retention | Proxy Access  | Link |
|------|--------------|------------------|-------------|--------------------|------------|----------------|-------------- | --- |
| **Free** | $0 | 500 | – | 1 | 1 | 7 days | ❌  | [Start Free](https://geekflare.com/pricing/) |
| **Starter** | $19 | 10,000 | $0.0019 | 5 | 3 | 30 days | ✅ (+4 credits per proxy call)  | [Get Starter](https://geekflare.com/pricing/) |
| **Growth** | $69 /mo **or** $58 /mo billed yearly | 100,000 | $0.00069 (monthly) | 10 | 5 | 30 days | ✅ (+4 credits)  | [Get Growth](https://geekflare.com/pricing/) |
| **Business** | $349 | 1,000,000 | $0.000349 | 25 | 25 | 90 days | ✅ (+4 credits)  | [Get Business](https://geekflare.com/pricing/) |
| **Enterprise** | Custom | Custom | Custom | Up to 1,000 | Custom | Custom | ✅ (+4 credits)  | [Contact Sales](https://geekflare.com/pricing/) |

**Key operational rules** (sourced from Geekflare's docs):

- **No credit rollover** – Unused credits vanish on the first day of the next billing cycle.  
- **Failed requests are free** – 5xx errors or captures that time out do not consume credits.  
- **Usage alerts** – Email at 90% consumption; requests are throttled at 100% until you add a credit pack or upgrade.  
- **Refund guarantee** – Cancel within 7 days and you've used less than 10% of your credits → full refund.  

Because the credit cost per capture drops dramatically as you move up the tiers, the **Growth** plan is the sweet spot for most SaaS products that need a few thousand screenshots a month. The **Business** tier becomes attractive for high-volume media-monitoring services that regularly exceed 100 K captures.

---

## Competitor Snapshot (Features & Pricing)

| Provider | Free Tier | Transparent PNG | Geo-Proxy | Credit-Based Billing | Notable Extras |
|----------|-----------|----------------|-----------|----------------------|----------------|
| ApiFlash | No free tier listed | ✅ (Chrome headless) | ❌ | ❌ (per-capture pricing) | CDN caching, AWS Lambda integration |
| **Geekflare** | 500 credits / month, 1 req/s | ✅ (full-browser, JS, ad-block) | ✅ (`proxyCountry`, +4 credits) | ✅ (fixed credit cost per call) | AI-vision link highlighting, timestamp watermark, Python/Node SDKs |
| Site-Shot | Limited free captures | ✅ (basic) | ❌ | ❌ (prepaid packages) | Simple web UI |
| ScreenshotOne | 100 free captures | ✅ (GPU-accelerated) | ✅ (proxy available) | ❌ (per-1 K dollar rates) | Direct S3 upload, custom script hooks |
| Microlink | Free tier with limited requests | ✅ (basic) | ❌ | ❌ (tiered request limits) | OpenGraph metadata extraction |

**Takeaway** – Geekflare is the only provider that pairs **transparent PNG** rendering with a **transparent, credit-based billing model** and **geo-proxy** capability without hidden per-capture fees. Competitors either lack compliance-focused features (timestamp watermark, link highlighting) or charge higher per-1 K rates that quickly outpace Geekflare once you need more than a few hundred captures.

---

## How Geekflare Renders Transparent PNGs Without Multiplier Surprises

**Core Rendering Pipeline**

When you POST to `https://api.geekflare.com/screenshot`, Geekflare spins up a real Chrome instance in a GCP-backed sandbox. The request body lets you specify:

| Parameter | Default | Options / Range | Effect |
|-----------|---------|----------------|--------|
| `url` | – | Any public URL | Target page |
| `format` | `png` | `png`, `jpeg`, `webp` | Output image type |
| `fullPage` | `false` | `true` / `false` | Capture the entire scroll height |
| `selector` | – | CSS selector | Crop to element (fallback to full page) |
| `device` | `desktop` | `desktop` / `mobile` | Emulate device UA & DPR |
| `viewportWidth` / `viewportHeight` | 1280 × 720 | 320-3840 px / 240-2160 px | Custom viewport size |
| `scaleFactor` | 1 | 1-5 | Retina-style upscaling |
| `theme` | `auto` | `light` / `dark` / `auto` | Force CSS media theme |
| `delay` | 0 | 0-10 s | Wait after load before capture |
| `disableAnimations` | `false` | `true` / `false` | Freeze CSS animations |
| `removeBackground` | `false` | `true` / `false` | Produce a transparent PNG |
| `addTimestamp` | `false` | `true` / `false` | Embed a legal watermark |
| `highlightLinks` | `false` | `true` / `false` | Draw bounding boxes for AI vision |
| `inline` | `false` | `true` / `false` | Return Base64 data URI instead of CDN link |
| `proxyCountry` | – | ISO-3166-1 alpha-2 (194 countries) | Route through rotating proxy (adds +4 credits) |

All of the above defaults to a fully functional, ad-blocked, CAPTCHA-bypassed screenshot that costs exactly **5 credits**. The only extra credit cost is the **proxy add-on** (`+4 credits`), which is only available on paid plans.

**Rate Limits & Reliability**

Geekflare enforces **requests per second** limits based on your tier (1 / 5 / 10 / 25 req/s, up to 1,000 req/s for Enterprise). Exceeding the limit returns a `429 Too Many Requests` response without consuming credits, preventing runaway invoices caused by loops in your code.

Uptime is **highly available**, backed by a multi-region deployment on Google Cloud Platform and Cloudflare's edge network. The service also guarantees **failed renders are free**, so you can safely retry transient 5xx errors without worrying about credit leakage.

**Integration Ecosystem**

Geekflare ships SDKs for **Python** (`pip install geekflare-api`) and **Node/TypeScript** (`npm install @geekflare/api-node`). The SDKs automatically inject the `x-api-key` header and expose a thin wrapper around the REST endpoint. For teams that prefer a no-code approach, the **Managed Cloud Platform (MCP)** at `https://mcp.geekflare.com/mcp` offers a UI to run the screenshot tool alongside 17 other utilities, and connectors exist for **n8n**, **Zapier**, **Make**, and **Google Sheets**.

If you need AI-augmented extraction (e.g., OCR or object detection) you can pipe the screenshot into Geekflare's AI extraction endpoint, which adds **+7 credits** (8 total) but stays within the same credit pool.

---

## Minimal Production-Ready Example

Below are two self-contained examples that you can drop into a CI job, a Lambda function, or a local script. Both hit the same endpoint with identical parameters: a transparent PNG of the Hacker News front page, full-page capture, background removed, and the result returned as an inline Base64 data URI.

### Python SDK Example

```python
# Install the SDK first:
# pip install geekflare-api

import os
import base64
from geekflare_api import GeekflareClient

# Your API key – keep it secret!
API_KEY = os.getenv("GEEKFLARE_API_KEY")

client = GeekflareClient(api_key=API_KEY)

payload = {
    "url": "https://news.ycombinator.com",
    "format": "png",
    "fullPage": True,
    "removeBackground": True,
    "addTimestamp": True,
    "inline": True,               # Returns Base64 data URI
    "device": "desktop",
    "viewportWidth": 1920,
    "viewportHeight": 1080,
    "scaleFactor": 2,             # Retina quality
    "disableAnimations": True,
    "delay": 2                    # Wait for lazy-loaded content
}

response = client.screenshot(payload)

if response.status_code == 200:
    data_uri = response.json()["image"]
    # Write the Base64 image to a file for verification
    with open("hn_screenshot.png", "wb") as out:
        out.write(base64.b64decode(data_uri.split(",", 1)[1]))
    print("✅ Screenshot saved as hn_screenshot.png")
else:
    print(f"❌ Failed: {response.status_code} – {response.text}")
```

**What you see:** The request consumes **5 credits** because we didn't use a proxy. On the **Free** tier you have 500 credits → 100 such captures per month.

### cURL One-Liner

```bash
curl -X POST "https://api.geekflare.com/screenshot" \
  -H "Content-Type: application/json" \
  -H "x-api-key: $GEEKFLARE_API_KEY" \
  -d '{
        "url": "https://news.ycombinator.com",
        "format": "png",
        "fullPage": true,
        "removeBackground": true,
        "addTimestamp": true,
        "inline": true,
        "device": "desktop",
        "viewportWidth": 1920,
        "viewportHeight": 1080,
        "scaleFactor": 2,
        "disableAnimations": true,
        "delay": 2
      }' | jq -r .image > hn_screenshot.base64

base64 -d hn_screenshot.base64 > hn_screenshot.png
echo "✅ Saved transparent PNG to hn_screenshot.png"
```

The cURL call mirrors the SDK payload exactly, demonstrating that you don't need a language-specific wrapper to get full control.

**Tip:** If you need a US-based proxy, add `"proxyCountry": "US"` to the JSON payload. That bumps the credit cost to **9 credits** (5 + 4) and is only allowed on Starter, Growth, Business, or Enterprise plans.

---

## Final Trade-Offs: Making an Objective Call for Your Stack

Geekflare's transparent PNG screenshot API is purpose-built for teams that require **pixel-perfect, real-browser renders** with **predictable, credit-based billing**. The service delivers:

- **Full-browser rendering** with JavaScript, ad-blocking, and CAPTCHA bypass out of the box (5 credits).  
- **Transparent PNG** via `removeBackground` – perfect for overlaying on custom UI components.  
- **Compliance-friendly features** such as timestamp watermarks and link highlighting for AI-vision pipelines.  
- **Geo-proxy rendering** on paid tiers for sites that block certain regions (+4 credits).  
- **Robust SDKs** and a **no-code MCP** that integrate with n8n, Zapier, Make, and Google Sheets.  

The **limitations** are clear:

- No direct S3/GCS upload hook; you must fetch the hosted CDN URL or the inline Base64 and store it yourself.  
- Rendering runs on standard Chrome instances; there is no GPU-accelerated tier for massive batch jobs.  
- No built-in scheduler; external orchestration (CI, cron, or a no-code workflow) is required to trigger periodic captures.  

When you weigh those constraints against the **high availability** guarantee and the **$0.0019-$0.000349 per credit** cost structure, the decision becomes a matter of volume and feature need. Small teams can stay on the **Free** tier forever if they stay under 100 captures per month. Mid-size SaaS products typically land on **Growth** ($69 /mo or $58 /mo yearly) and still have ample headroom for proxy-enabled, high-resolution PNGs. Enterprise customers who need >1,000 req/s can negotiate a custom plan that preserves the same credit-first philosophy.

In short, if your stack already uses Geekflare for SERP or web-scraping, adding the screenshot capability costs **no extra integration work** and keeps the credit accounting unified. If you're evaluating a new provider, the hidden multipliers in per-capture pricing will likely surface once you enable the very features you need – proxy, high-DPI, or AI extraction. Geekflare's transparent credit model eliminates that surprise and lets you focus on building, not billing.

---
