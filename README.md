# 🧾 Monthly Billing Flow Dashboard

> **Live HTML dashboard powered by Google Apps Script, reads Excel data directly from Google Drive and visualizes billing trends, business performance, and month-over-month analysis in real time**

---

## 🌐 Live Demo

🔗 **[Open Live Dashboard](https://script.google.com/a/macros/nu.edu.pk/s/AKfycbxAwjdjmBAa3gzT-VN_HltXWcSMie0v0b5S8_AKtuLgCu1fyUw-YNoy36-ADDfaqXFk/exec)**

> Live data is pulled directly from Google Drive. No login required to view.

---

## 📌 Overview

This is a fully custom, browser-based analytics dashboard built without any BI tool. It was developed using pure HTML, CSS, and JavaScript, deployed via Google Apps Script as a Web App. The dashboard reads a live `.xlsx` file from Google Drive, processes it in the backend, compresses it, and serves it to the frontend — updating automatically whenever the source file changes.

---

## 🎯 Business Problem

The operations team needed a centralized billing dashboard to:
* Track monthly bill creation volume and billed amounts
* Monitor business-wise billing performance
* Analyze month-over-month trends and identify dips
* Understand billing timing (when bills are created vs date of service)
* Export filtered data for reporting

---

## 🔄 How It Works — Architecture

```
📁 Google Drive          ⚙️ Google Apps Script        🌐 Browser
────────────────         ──────────────────────        ────────────
Excel (.xlsx)   ──────►  Code.gs                ──────► Index.html
(Updated by              - Finds latest .xlsx           (Renders charts,
 operations              - Converts to temp Sheet        tables & KPIs
 team)                   - Reads & builds CSV            in real time)
                         - Gzip compresses it
                         - Caches result
                         - Serves to frontend
```

**Key technical details:**
* Backend (`Code.gs`) uses **Google Drive API** to locate the latest `.xlsx` in a specified folder
* Converts it to a temporary Google Sheet to extract data, then **deletes the temp copy**
* Compresses data with **gzip** and encodes as base64, sent inline to the page
* **Smart caching**, only re-reads the file when it has actually changed (uses file ID + last modified timestamp)
* Frontend (`Index.html`) decompresses the data in the browser using the **DecompressionStream API**
* Full **XLSX parser built from scratch** in JavaScript, reads shared strings, inline strings, sparse rows, serial dates, and columns beyond Z

---

## 📊 Dashboard Features

### 🔢 KPI Summary Cards
| Metric | Description |
|--------|-------------|
| Total Bills Created | Count of unique STUDY_ID values |
| Total Billed Amount | Sum of AMOUNTBILLED |
| Average Bill Amount | Billed amount ÷ bills |
| Businesses | Unique businesses in selection |

### 📈 Charts & Visualizations
- **Bills Created Flow** — Monthly bill volume trend (line chart)
- **Total Billed Amount Flow** — Monthly billed amount trend (line chart)
- **Top Businesses by Bills** — Horizontal bar chart, top 10
- **Top Businesses by Billed Amount** — Horizontal bar chart, top 10

### 📋 Analysis Tables
 **Bill Creation Analysis** : Business × Month matrix with MoM % change (↑/↓)
 **Billed Amount Analysis** : Business × Month matrix with MoM % change
 **Billing Timing Analysis** : Date of Service vs Bill Created Month cross-tab
 **Monthly Summary** : Month-by-month totals with diff %
 **Business Summary** : All businesses with total bills and billed amount

### 💡 Auto Insights
Dynamic analysis section that auto-calculates:
 Peak bill volume month
 Peak billed amount month
 Top performing business
 Latest month movement vs prior month
 Average bill amount
 Business concentration %

### 🎛️ Filters
* Date range (From / To)
* Business dropdown
* Business search (live text filter)
* Reset all filters button

### ⬇️ Export Options
 **Upload Excel** — Load a different file for the session
 **Capture Dashboard (PDF)** — Browser print to PDF with full table expansion
 **Download Template** — Get the exact Excel template with required columns
 **Export Data** — Download filtered data as CSV

---

## 🗂️ Required Excel Columns (Data Sheet)

| Column | Description |
|--------|-------------|
| BUSINESSID | Business/client identifier |
| BILLCREATEDDATE | Date the bill was created |
| STUDY_ID | Unique bill identifier |
| AMOUNTBILLED | Billed amount |
| DATEOFSERVICE | Date of service (for timing analysis) |

> The workbook must contain a sheet named **"Data"** with these columns.

---

## 🔧 Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Backend** | Google Apps Script (Code.gs) | File reading, data processing, caching, serving |
| **Frontend** | HTML + CSS + Vanilla JS (Index.html) | Dashboard UI, charts, tables, filters |
| **Data Source** | Google Drive (.xlsx) | Live data storage |
| **API** | Google Drive API (Advanced Service) | Locate and read Excel files from Drive |
| **Compression** | Gzip + Base64 | Efficient data transfer from backend to frontend |
| **Storage** | IndexedDB | Client-side state persistence across sessions |
| **Charts** | Custom SVG (no library) | Pure JS line charts and bar charts |

---

## 🚀 How to Deploy Your Own Copy

### Prerequisites
- A Google account
- An `.xlsx` file with the required columns in a Google Drive folder

### Steps

1. **Open Google Apps Script** : go to [script.google.com](https://script.google.com) → New Project
2. **Add Code.gs** : paste the backend code into `Code.gs`
3. **Add Index.html** : click `+` → HTML file → name it `Index` → paste the frontend code
4. **Enable Drive API** — click `+` next to Services → add **Drive API**
5. **Set folder name** : change `FOLDER_NAME = 'Dashboard'` to match your Drive folder
6. **Deploy** : click `Deploy` → `New deployment` → type: `Web app`
   - Execute as: **Me**
   - Who has access: **Anyone**
7. **Copy the web app URL** — share it with your team

### Refreshing Data
- Add `?refresh=1` to the URL to force a fresh read from Drive
- Or simply update the `.xlsx` file — the dashboard auto-detects changes via cache stamp


---

## 💡 Key Technical Highlights

- **Zero external libraries** : charts, XLSX parser, ZIP reader all built from scratch in vanilla JS
- **Serverless architecture** : no database, no server, no hosting costs
- **Smart caching** : file ID + last-modified timestamp used as cache key; avoids unnecessary re-reads
- **PDF export** : expands all scrollable tables before printing for a complete PDF capture
- **Offline-capable** : embeds last-known data as a fallback if Drive is unreachable

---

## 🖥️ Other Dashboards

> 📞 **All other dashboards are available on request. Feel free to reach out to schedule a call or Google Meet.**

---

## 👤 Author

**Mirza Wajeeh Baig** — Data Analyst
[![GitHub](https://img.shields.io/badge/GitHub-mirzawajeehbaig-181717?style=flat&logo=github)](https://github.com/wajeehbaigmirza-cmd)
