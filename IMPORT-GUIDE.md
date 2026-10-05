# n8n Workflow JSON Import Guide

## Workflow: QR Link Tracker - Collect Visitor Data

---

## How to Import

1. Go to **https://minjarul.dpdns.org**
2. Click **Workflows** → **Import from File**
3. Upload `n8n-workflow.json`
4. Click **Create Workflow**

---

## Prerequisites

### 1. Create Data Table First
Before importing the workflow, create this data table in n8n:

**Table Name:** `click_trackers`

**Columns:**
| Column Name | Type | Description |
|-------------|------|-------------|
| id | text | Auto-generated ID |
| visitor_id | text | Unique visitor ID |
| event | text | page_view or click |
| platform | text | facebook or whatsapp |
| timestamp | text | ISO date |
| url | text | Page URL |
| referrer | text | Source page |
| ip_address | text | IP address |
| country | text | Country name |
| city | text | City name |
| device | text | Device type |

---

### 2. Webhook Configuration

**Webhook URL:**
```
https://minjarul.dpdns.org/webhook/tracker
```

**Settings:**
- HTTP Method: POST
- Path: `tracker`
- Authentication: None
- Response Mode: On Received
- Body Content Type: application/json

---

## Data Flow

```
┌─────────────┐     ┌─────────────────┐     ┌──────────────┐     ┌────────────────┐     ┌─────────────┐
│   Webhook   │────▶│ Get Geolocation │────▶│ Format Data  │────▶│ Store in Table │────▶│  Response   │
└─────────────┘     └─────────────────┘     └──────────────┘     └────────────────┘     └─────────────┘
       │                    │
       │                    ▼
       │            https://ipapi.co/
       │           /{ip}/json/
       │                    │
       └────────────────────┘
    (sends tracking data)
```

---

## Test with cURL

```bash
curl -X POST https://minjarul.dpdns.org/webhook/tracker \
  -H "Content-Type: application/json" \
  -d '{
    "event": "click",
    "platform": "facebook",
    "visitor_id": "test_user_123",
    "timestamp": "2026-10-05T13:30:00.000Z",
    "url": "https://minjarul1.github.io/qr-tracker/",
    "referrer": "direct",
    "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 16_0 like Mac OS X)",
    "screen_resolution": "390x844",
    "language": "en-BD",
    "ip_address": "103.123.45.67"
  }'
```

**Expected Response:**
```json
{
  "success": true,
  "message": "Visitor tracked successfully",
  "data": {
    "visitor_id": "test_user_123",
    "event": "click",
    "platform": "facebook",
    "country": "Bangladesh",
    "city": "Dhaka"
  }
}
```

---

## View Collected Data

1. Open n8n workflow editor
2. Click on **Webhook** node
3. Enable **Test** mode
4. Run workflow manually
5. Check **Store in Data Table** output

Or query the data table directly in n8n's Data Store tab.

---

## Sample Incoming Data from Website

```json
{
  "event": "page_view",
  "platform": null,
  "visitor_id": "user_1728141234567_abc123xyz",
  "timestamp": "2026-10-05T13:27:14.567Z",
  "url": "https://minjarul1.github.io/qr-tracker/?link=facebook",
  "referrer": "",
  "user_agent": "Mozilla/5.0 (Linux; Android 13; SM-G991B) AppleWebKit/537.36",
  "screen_resolution": "412x915",
  "language": "bn-BD",
  "ip_address": "103.123.45.67"
}
```

---

## Data Table Query Example

To view all clicks for Facebook:
```
Platform = facebook
```

To view today's clicks:
```
Timestamp contains "2026-10-05"
```

To count total visitors:
```
Count rows where visitor_id is unique
```
