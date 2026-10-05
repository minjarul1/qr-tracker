# n8n QR Tracker Webhook Setup

## Create This Workflow in n8n

**Webhook URL:** `https://minjarul.dpdns.org/webhook/tracker`

---

### Step 1: Add Webhook Node
- **Path:** `tracker`
- **HTTP Method:** POST
- **Authentication:** None
- **Response Mode:** On Received
- **Body Content Type:** application/json

---

### Step 2: Add HTTP Request Node (Get IP Location)
```
Method: GET
URL: https://ipapi.co/{{ $json.ip_address }}/json/
```

Note: Extract IP from webhook headers:
- `x-forwarded-for` or `x-real-ip`

---

### Step 3: Add Set Node (Format Output)
Create this JSON structure:

```json
{
  "visitor_id": "{{ $json.visitor_id }}",
  "event": "{{ $json.event }}",
  "platform": "{{ $json.platform }}",
  "timestamp": "{{ $json.timestamp }}",
  "url": "{{ $json.url }}",
  "referrer": "{{ $json.referrer }}",
  "user_agent": "{{ $json.user_agent }}",
  "screen_resolution": "{{ $json.screen_resolution }}",
  "language": "{{ $json.language }}",
  "ip_address": "{{ $json.ip_address }}",
  "country": "{{ $json.country }}",
  "city": "{{ $json.city }}",
  "device": "{{ $json.device }}",
  "browser": "{{ $json.browser }}"
}
```

---

### Step 4: Add Response Node
- **Response Code:** 200
- **Response Body:** `{"success": true, "message": "Visitor tracked"}`

---

## Collect Data Fields

| Field | Source | Description |
|-------|--------|-------------|
| visitor_id | localStorage | Unique user ID |
| event | JS | page_view or click |
| platform | JS | facebook or whatsapp |
| timestamp | JS | ISO 8601 format |
| url | JS | Current page URL |
| referrer | JS | Where they came from |
| user_agent | JS | Browser/device info |
| screen_resolution | JS | Display size |
| language | JS | Browser language |
| ip_address | HTTP headers | From n8n request |
| country | ipapi.co | Geolocation |
| city | ipapi.co | Geolocation |

---

## View Collected Data

In n8n workflow editor:
1. Click on the webhook node
2. Check "Test" mode
3. Send test payload
4. See execution data

Or use n8n's **Data Store** to save and query results.

---

## Test the Webhook

```bash
curl -X POST https://minjarul.dpdns.org/webhook/tracker \
  -H "Content-Type: application/json" \
  -d '{"event":"test","platform":"facebook","visitor_id":"test123","timestamp":"'$(date -u +%Y-%m-%dT%H:%M:%SZ)'"}'
```
