# QR Code Link Tracker

A tracking system that collects visitor data when users open links via QR codes.

## Features
- QR code generation with custom tracking links
- Visitor tracking (location, device, browser info)
- n8n webhook integration for data collection
- Real-time statistics dashboard

## How It Works
1. Generate QR code with tracking link
2. User scans QR code and opens link
3. System collects visitor data and sends to n8n
4. View collected data in n8n workflows

## Tracking Data Collected
- Visitor ID (unique per browser)
- Timestamp
- IP Address (via n8n)
- Geolocation (approximate)
- User Agent
- Screen Resolution
- Language
- Referrer
- Connection Type
