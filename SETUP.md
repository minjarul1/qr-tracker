# QR Code Tracker - Setup Guide

## How It Works

1. **QR Code Generated** → Points to tracking URL
2. **User Scans QR** → Opens link, triggers tracking
3. **Data Collected** → Sends to n8n webhook
4. **View Results** → Check n8n workflow execution

## Tracking Data Sent to n8n

```json
{
  "event": "page_view | click",
  "platform": "facebook | whatsapp",
  "visitor_id": "unique_user_id",
  "timestamp": "ISO date",
  "url": "page URL",
  "referrer": "source",
  "user_agent": "browser info",
  "screen_resolution": "1920x1080",
  "language": "en-BD",
  "ip_address": "via n8n geolocation",
  "country": "Bangladesh",
  "city": "Dhaka"
}
```

## n8n Workflow Setup

### Step 1: Create Webhook
1. Go to https://minjarul.dpdns.org
2. Create new workflow
3. Add **Webhook** node:
   - HTTP Method: POST
   - Path: `tracker`
   - Authentication: None
   - Response Mode: On Received

### Step 2: Add HTTP Request Node (Get Location)
Add after webhook:
- **URL**: `https://ipapi.co/json/{IP}` or use IP from headers
- **Method**: GET

### Step 3: Add Set Node (Format Data)
Extract and format:
- visitor_id
- event type
- platform
- timestamp
- location data

### Step 4: Add Response Node
Return success message.

## Webhook URL
```
https://minjarul.dpdns.org/webhook/tracker
```

## Generate QR Codes

### Facebook Group QR
```
https://minjarul1.github.io/qr-tracker/?link=facebook
```

### WhatsApp Group QR
```
https://minjarul1.github.io/qr-tracker/?link=whatsapp
```

### Track All Link Clicks
```
https://minjarul1.github.io/qr-tracker/
```

## View Collected Data

In n8n:
1. Open your workflow
2. Click on webhook execution
3. See all collected visitor data

## Sample Response from n8n

When someone scans QR and clicks:
```json
{
  "success": true,
  "message": "Visitor tracked successfully",
  "data": {
    "visitor_id": "user_1234567890_abc123",
    "event": "click",
    "platform": "facebook",
    "timestamp": "2026-10-05T13:30:00.000Z",
    "ip_address": "103.xx.xx.xx",
    "country": "Bangladesh",
    "city": "Dhaka",
    "device": "Mobile",
    "browser": "Chrome"
  }
}
```
