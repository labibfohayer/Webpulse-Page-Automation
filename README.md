# Webpulse AutoOrder Engine - E-Commerce and Social Commerce Automation Pipeline

[![Facebook Official Page](https://img.shields.io/badge/Facebook_Page-Webpulse_Automation-blue?style=for-the-badge&logo=facebook&logoColor=white)](https://www.facebook.com/webpulseautomation)

An event-driven order processing and automated invoicing engine engineered for digital storefronts and social commerce businesses. Built with Node.js, Express, Chokidar, PDFKit, and Meta Graph API Webhooks.

---

## Project Overview

Webpulse AutoOrder Engine automates the lifecycle of e-commerce orders originating from Facebook Messenger conversations, lead advertising forms, and external file drops. 

Upon receiving an order payload, the system parses customer records, computes invoice balances, renders high-resolution vector PDF invoices embedded with cryptographic QR verification codes, compiles bilingual SMS and WhatsApp notification templates, and logs transactions into persistent financial ledgers without manual human intervention.

---

## Key Features

- Automated PDF Invoice Engine: Generates itemized, vector-rendered commercial invoices using PDFKit, complete with dynamic billing calculations, customer contact details, order timestamps, and scannable QR codes for order verification.
- Meta Graph API Webhook Integration: Native webhook listener configured to receive, verify, and parse incoming Facebook Messenger interactions and order queries.
- Real-Time Filesystem Observer: Chokidar-powered background daemon monitoring `data/incoming/` for new JSON order drops. Ingested files are validated, processed through the pipeline, and archived into `data/archive/`.
- Multi-Channel Notification Dispatcher: Prepares formatted customer confirmation notifications for SMS and WhatsApp delivery, incorporating customer names, item summaries, total amounts, and delivery tracking information.
- Live Web Dashboard & Visual Pipeline: Browser-based operations console (`public/`) connected via Server-Sent Events (SSE), displaying real-time 5-stage order progression and manual trigger controls.
- Financial Ledger Synchronization: Continuously synchronizes all transactions to `output/sales_ledger.csv` and `output/sales_ledger.json` for auditing and direct import into Microsoft Excel or Google Sheets.
- Voice Audio Order Alerts: Optional text-to-speech module using Microsoft Edge TTS / Google TTS to vocalize incoming order alerts in the fulfillment warehouse.

---

## Architecture & Data Flow

```
+-------------------------------------------------------------------+
|                       Incoming Order Ingestion                     |
|  - Meta Facebook Webhook (`/webhook`)                              |
|  - Filesystem Drop Folder (`data/incoming/*.json`)                |
|  - Manual Dashboard Dispatcher                                    |
+---------------------------------+---------------------------------+
                                  |
                                  v
+-------------------------------------------------------------------+
|                 Automation Pipeline (`src/pipeline.js`)            |
|  1. Order Data Validation & Sanitization                          |
|  2. Line Item Pricing & Shipping Calculation                      |
|  3. PDF Invoice Generation (`src/invoiceGenerator.js`)            |
|  4. Notification Text Generation (`output/notifications/`)        |
|  5. Transaction Logging (`output/sales_ledger.csv`)               |
|  6. Payload Archival (`data/archive/`)                            |
+---------------------------------+---------------------------------+
                                  |
                                  v
+-------------------------------------------------------------------+
|                         Broadcast & Output                        |
|  - Real-time SSE Events -> Live Dashboard Console                 |
|  - Generated PDF Invoice Storage -> `output/invoices/*.pdf`       |
|  - Multi-Channel Customer Alerts                                  |
+-------------------------------------------------------------------+
```

---

## Tech Stack

- Runtime: Node.js (v18.x or higher)
- Web Server: Express 4
- File Monitoring: Chokidar 3
- Document Generation: PDFKit, QRCode
- Speech Synthesis: msedge-tts, google-tts-api
- Tunneling & Webhook Testing: Localtunnel
- Environment Management: Dotenv

---

## Directory Structure

```
Webpulse-Page-Automation/
├── src/
│   ├── server.js              # Express API server and SSE event broadcaster
│   ├── pipeline.js            # Core order automation and data coordinator
│   ├── invoiceGenerator.js    # PDFKit and QRCode vector invoice compiler
│   ├── facebookWebhook.js     # Meta Graph API verification and message handler
│   └── watcher.js             # Chokidar directory observer daemon
├── public/                    # Frontend control dashboard (HTML5, CSS3, JavaScript)
├── data/
│   ├── incoming/              # Ingestion directory for incoming JSON order files
│   └── archive/               # Archive repository for processed order files
├── output/
│   ├── invoices/              # Generated printable PDF customer invoices
│   ├── notifications/         # Formatted text files for SMS and WhatsApp dispatch
│   ├── sales_ledger.csv       # Delimited transaction records for spreadsheet import
│   └── sales_ledger.json      # Structured historical order database
├── bot.js                     # Standalone CLI desktop daemon runner
├── config.json                # Business profile, currency, and tax configurations
├── .env.example               # Environment variable credentials template
├── package.json               # Node.js dependencies and operational scripts
└── START_BOT.bat              # Windows batch launch executable
```

---

## Installation and Setup Guide

### Prerequisites
- Node.js version 18.x or higher
- npm (Node Package Manager)

### 1. Clone the Repository
```bash
git clone https://github.com/labibfohayer/Webpulse-Page-Automation.git
cd Webpulse-Page-Automation
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Configure Environment Variables
Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```
Populate the configuration values:
```env
PORT=3000
META_VERIFY_TOKEN=your_custom_webhook_verification_token
META_PAGE_ACCESS_TOKEN=your_facebook_page_access_token
```

### 4. Running the Application

- Option A: Start the Full Server with Web Dashboard
```bash
npm start
```
Access the operational dashboard at `[http://localhost:3000](http://localhost:3000) (Live Automation Service: [facebook.com/webpulseautomation](https://www.facebook.com/webpulseautomation))`.

- Option B: Run the Headless Desktop Bot
```bash
node bot.js
```
The file watcher will immediately begin polling `data/incoming/` for order payloads.

---

## Order Ingestion Payload Format

To process an order programmatically or via file drop, save a JSON file in `data/incoming/` using the following schema:

```json
{
  "orderId": "ORD-2026-1049",
  "customer": {
    "name": "Tanvir Ahmed",
    "phone": "+8801700000000",
    "address": "House 12, Road 4, Dhanmondi, Dhaka",
    "district": "Dhaka"
  },
  "items": [
    {
      "name": "Premium Cotton Panjabi",
      "size": "42",
      "quantity": 1,
      "price": 2850
    }
  ],
  "shippingCost": 100,
  "discount": 0,
  "paymentMethod": "Cash on Delivery",
  "notes": "Please deliver after 4 PM"
}
```

---

## Author and Contact

Developed by **Labib Fohayer** at **Webpulse Automation**.

- Website: [https://labibfohayer.github.io](https://labibfohayer.github.io)
- GitHub: [https://github.com/labibfohayer](https://github.com/labibfohayer)

---

## License

This project is licensed under the ISC License.
