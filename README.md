# TickSync - N8N Automated Ticketing System 🎯

## Project Overview

**TickSync** is an N8N-based automated ticketing system that accepts customer support requests via webhooks, tracks them in Google Sheets, and processes secure ticket resolutions with HMAC signature validation.

The system creates support tickets, monitors their status, logs resolutions, and sends automated email confirmations - all through workflow automation!

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Current Status](#current-status)
- [Features](#features)
- [Project Structure](#project-structure)
- [Technical Stack](#technical-stack)
- [Getting Started](#getting-started)
- [API Endpoints](#api-endpoints)
- [Workflow Details](#workflow-details)
- [Google Sheets Setup](#google-sheets-setup)
- [Credentials & Authentication](#credentials--authentication)
- [Testing Guide](#testing-guide)
- [Common Issues & Solutions](#common-issues--solutions)
- [Next Steps](#next-steps)
- [Documentation Files](#documentation-files)

---

## 🚀 Current Status

### Completion Progress:
- ✅ **STEP 1:** Project Planning & Setup
- ✅ **STEP 2:** Support Request Workflow (COMPLETE)
- 🔄 **STEP 3:** Resolution Webhook (IN PROGRESS)
  - ✅ NODE 1: Webhook - Resolution Request
  - ✅ NODE 2: HMAC Signature Validation
  - ✅ NODE 3: IF Signature Valid Decision
  - ✅ NODE 4: Google Sheets - Find Ticket
  - ✅ NODE 5: Google Sheets - Update Status
  - ⏳ NODE 6: Activity Log (PENDING)
  - ⏳ NODE 7-9: Response Nodes (PENDING)
- ⏳ **STEP 4:** Comprehensive Testing (8 Test Cases)
- ⏳ **STEP 5:** Video Demo & Final Documentation

---

## ✨ Features

### ✅ Implemented:

1. **Support Request Webhook** (`/support`)
   - Accept POST requests with customer support data
   - Validate input fields (email, summary, detail, urgency)
   - Automatic duplicate detection (same email + summary within 24 hours)
   - Create tickets in Google Sheets
   - Send confirmation emails to customers

2. **Data Validation**
   - Input validation for all required fields
   - Duplicate ticket prevention
   - Data mapping and transformation

3. **Google Sheets Integration**
   - Read/Write operations to Ticket Tracking sheet
   - Automatic timestamp recording
   - Error logging to Error Log sheet
   - Activity logging capability

4. **Email Notifications**
   - Send confirmation emails via Gmail SMTP
   - Customizable email templates
   - Support for HTML formatting

### 🔄 In Progress:

1. **Resolution Webhook** (`/resolution`)
   - Accept POST requests for ticket resolutions
   - HMAC-SHA256 signature validation
   - Update ticket status to "Resolved"
   - Log activity and timestamps

2. **Security**
   - HMAC signature validation
   - Secure webhook endpoints
   - Admin alert system for errors

### ⏳ Coming Soon:

1. **Advanced Logging**
   - Detailed activity logs
   - Error tracking and alerts
   - Admin notifications

2. **Testing Suite**
   - 8 comprehensive test cases
   - Postman collection
   - Performance testing

3. **Documentation**
   - Video walkthrough (max 10 minutes)
   - API documentation
   - Deployment guide

---

## 🏗️ Project Structure

```
TickSync/
├── Workflows
│   ├── Support Request Flow (/support webhook)
│   │   ├── NODE 1: Webhook - Support Request
│   │   ├── NODE 2: Validate Input
│   │   ├── NODE 3: Read Ticket Tracking
│   │   ├── NODE 4: Check Duplicate Logic
│   │   ├── NODE 5: IF Branch (Duplicate?)
│   │   ├── NODE 6: Map Data for API
│   │   ├── NODE 7: HTTP Request (Create Ticket)
│   │   ├── NODE 8: Append to Ticket Tracking
│   │   └── NODE 9: Send Confirmation Email
│   │
│   ├── Resolution Workflow (/resolution webhook)
│   │   ├── NODE 1: Webhook - Resolution Request
│   │   ├── NODE 2: Validate HMAC Signature
│   │   ├── NODE 3: IF Signature Valid
│   │   ├── NODE 4: Find Ticket in Sheet
│   │   ├── NODE 5: Update Ticket Status
│   │   ├── NODE 6: Activity Log Entry
│   │   ├── NODE 7: Response Success
│   │   └── NODE 8: Response Error
│   │
│   └── Error Handling Workflow
│       ├── Error Trigger
│       ├── Log to Error Sheet
│       └── Send Admin Alert
│
├── Google Sheets (TickSync Automation)
│   ├── Sheet 1: Ticket Tracking
│   ├── Sheet 2: Error Log
│   └── Sheet 3: Activity Log
│
└── Documentation
    ├── README.md (this file)
    ├── CHAT-REFERENCE-SESSION-SUMMARY.md
    ├── STEP3-RESOLUTION-WORKFLOW-NODES-3-4-5.md
    └── STEP3-RESOLUTION-WORKFLOW-NODES-3-4-5.pdf
```

---

## 🛠️ Technical Stack

| Component | Technology | Status |
|-----------|-----------|--------|
| **Workflow Engine** | N8N | ✅ Installed & Running |
| **Data Storage** | Google Sheets API | ✅ Connected |
| **Email Service** | Gmail SMTP | ✅ Configured |
| **Authentication** | OAuth2 (Google) | ✅ Working |
| **Security** | HMAC-SHA256 | ✅ Implemented |
| **HTTP Requests** | N8N HTTP Node | ✅ Working |
| **Testing Tool** | Postman | ✅ Ready |

---

## 🚀 Getting Started

### Prerequisites:
1. N8N instance running
2. Google Account with Sheets API enabled
3. Gmail account configured for SMTP
4. Postman or similar HTTP client for testing

### Setup Steps:

**Step 1: Import Workflow**
```
1. Open N8N dashboard
2. Create new workflow
3. Add both flows to same canvas (Support + Resolution)
4. Configure all nodes as per documentation
```

**Step 2: Configure Google Sheets**
```
1. Create "TickSync Automation" spreadsheet
2. Create 3 sheets:
   - Ticket Tracking (columns: Timestamp, CustomerEmail, IssueSummary, IssueDetail, Urgency, TicketID, Status, Resolved At)
   - Error Log (columns: ErrorTimestamp, Node, ErrorCode, ErrorMessage, Payload)
   - Activity Log (columns: LogTimestamp, Step, Details)
```

**Step 3: Set Up Credentials**
```
1. Google Sheets OAuth2 authentication
2. Gmail SMTP credentials
3. TickSync API credentials (if using actual API)
```

**Step 4: Test Endpoints**
```
Support: https://n8n-instance/webhook/support
Resolution: https://n8n-instance/webhook/resolution
```

---

## 🔌 API Endpoints

### 1. Support Request Endpoint

**URL:** `POST https://n8n-instance/webhook/support`

**Request Body:**
```json
{
  "customer_email": "user@example.com",
  "issue_summary": "Login Issues",
  "issue_detail": "Cannot access account with correct password",
  "urgency": "critical"
}
```

**Response (Success - 201):**
```json
{
  "json": {
    "contact_email": "user@example.com",
    "issue_title": "Login Issues",
    "issue_details": "Cannot access account with correct password",
    "priority": "urgent",
    "ticket_id": "TICKET-001"
  }
}
```

**Response (Duplicate - 409):**
```json
{
  "error": "Duplicate ticket detected",
  "message": "A similar ticket from this email already exists"
}
```

---

### 2. Resolution Webhook Endpoint

**URL:** `POST https://n8n-instance/webhook/resolution`

**Request Body:**
```json
{
  "ticket_id": "TICKET-001",
  "new_status": "Resolved",
  "resolved_at": "2026-10-06T14:00:00Z",
  "signature": "f21e43d7071ff9cddd0db63565e9cb0c3da4f72cc8d39ebce7ec845de79c4217"
}
```

**Response (Success - 200):**
```json
{
  "message": "Ticket resolved successfully",
  "ticket_id": "TICKET-001",
  "status": "Resolved",
  "resolved_at": "2026-10-06T14:35:22Z"
}
```

**Response (Invalid Signature - 401):**
```json
{
  "error": "Unauthorized",
  "message": "Invalid signature"
}
```

---

## 🔄 Workflow Details

### FLOW 1: Support Request Workflow

```
Customer Submits Request
        ↓
/support Webhook receives data
        ↓
Validate Input (Extract fields)
        ↓
Read Ticket Tracking Sheet
        ↓
Check for Duplicate
        ├─ YES: Send 409 (Conflict)
        └─ NO: Continue
            ↓
            Map Data to API Format
            ↓
            Create Ticket (HTTP POST)
            ↓
            Append to Google Sheets
            ↓
            Send Confirmation Email
            ↓
            Success Response (201)
```

### FLOW 2: Resolution Workflow

```
Resolution Request
        ↓
/resolution Webhook receives data
        ↓
Validate HMAC Signature
        ├─ INVALID: Send 401
        └─ VALID: Continue
            ↓
            Read Ticket Tracking Sheet
            ↓
            Find Ticket by ID
            ↓
            Update Status to "Resolved"
            ↓
            Update Resolved Timestamp
            ↓
            Log Activity
            ↓
            Success Response (200)
```

---

## 📊 Google Sheets Setup

### Spreadsheet Name: "TickSync Automation"

#### Sheet 1: Ticket Tracking
```
Column A: Timestamp (ISO format)
Column B: CustomerEmail
Column C: IssueSummary
Column D: IssueDetail
Column E: Urgency
Column F: TicketID
Column G: Status (Created, In Progress, Resolved)
Column H: Resolved At (ISO format or null)
```

#### Sheet 2: Error Log
```
Column A: ErrorTimestamp
Column B: Node (Node name where error occurred)
Column C: ErrorCode
Column D: ErrorMessage
Column E: Payload (JSON data)
```

#### Sheet 3: Activity Log
```
Column A: LogTimestamp
Column B: Step (Description of action)
Column C: Details (Additional information)
```

---

## 🔐 Credentials & Authentication

### 1. Google Sheets OAuth2
- **Service:** Google Sheets API
- **Authentication:** OAuth2 (User's personal account)
- **Status:** ✅ Connected & Working

### 2. Gmail SMTP
- **Host:** smtp.gmail.com
- **Port:** 587
- **Username:** Your Gmail address
- **Password:** App-specific password (not regular password)
- **Status:** ✅ Configured & Sending emails

### 3. HMAC Configuration
- **Algorithm:** SHA-256
- **Secret Key:** `super-secret-key-2025`
- **Format:** `ticket_id|new_status|resolved_at`
- **Output:** Hexadecimal hash

### 4. TickSync API Credentials (Mock)
- **Client ID:** ticksync-automation-123
- **Client Secret:** sec_abc123def456
- **Token URL:** https://api.ticksync.mock/oauth/token
- **Status:** Configured (not tested with actual API yet)

---

## 🧪 Testing Guide

### Test Environment:
- **HTTP Testing URL:** https://httpbin.org/post (for mock testing)
- **Actual API URL:** https://api.ticksync.mock/v1/issues (for production)

### Test Case 1: Successful Support Request
```
POST /support
{
  "customer_email": "test@example.com",
  "issue_summary": "Login Issues",
  "issue_detail": "Cannot access account",
  "urgency": "high"
}

Expected: 201 Created + Email sent + Row added to Sheets
```

### Test Case 2: Duplicate Detection
```
POST /support (same email + summary within 24 hours)

Expected: 409 Conflict + No new row
```

### Test Case 3: Valid Signature Resolution
```
POST /resolution
{
  "ticket_id": "TICKET-001",
  "new_status": "Resolved",
  "resolved_at": "2026-10-06T14:00:00Z",
  "signature": "f21e43d7071ff9cddd0db63565e9cb0c3da4f72cc8d39ebce7ec845de79c4217"
}

Expected: 200 OK + Status updated in Sheets
```

### Test Case 4: Invalid Signature
```
POST /resolution
{
  "ticket_id": "TICKET-001",
  "new_status": "Resolved",
  "resolved_at": "2026-10-06T14:00:00Z",
  "signature": "invalid123456"
}

Expected: 401 Unauthorized
```

### Postman Collection:
[Create and use Postman collection with pre-configured requests]

---

## 🐛 Common Issues & Solutions

### Issue 1: Google Sheets - No Output
**Error:** Google Sheets node returns empty response

**Solution:**
1. Enable "Always Output Data" setting in node
2. Verify sheet exists and is accessible
3. Check authentication credentials

---

### Issue 2: Email Not Sending
**Error:** Email node fails, credentials error

**Solution:**
1. Use Gmail App Password (not regular password)
2. Enable "Less secure app access" in Gmail settings
3. Verify SMTP credentials: smtp.gmail.com:587
4. Check TLS/SSL settings

---

### Issue 3: Duplicate Detection Not Working
**Error:** Duplicate tickets still being created

**Solution:**
1. Check Function node logic
2. Verify timestamp format (ISO 8601)
3. Compare full email + summary (case-sensitive)
4. Check 24-hour window calculation

---

### Issue 4: HMAC Signature Validation Fails
**Error:** Valid signatures marked as invalid

**Solution:**
1. Verify secret key: `super-secret-key-2025`
2. Check message format: `ticket_id|new_status|resolved_at`
3. Ensure hexadecimal output format
4. Compare case-sensitive strings

---

### Issue 5: HTTP Response Data Undefined
**Error:** Cannot access response fields (undefined)

**Solution:**
1. Check nested path: `$json.json.field` (note double `.json`)
2. Inspect httpbin response structure
3. Use actual API endpoint instead of httpbin
4. Log response data in Function node for debugging

---

## ✅ Testing Results Summary

### STEP 2 - Support Flow (COMPLETE)
- ✅ Webhook receives data correctly
- ✅ Input validation working
- ✅ Google Sheets read operations
- ✅ Duplicate detection logic
- ✅ IF branching (TRUE/FALSE)
- ✅ Data mapping to API format
- ✅ HTTP requests to httpbin
- ✅ Google Sheets append operations
- ✅ Email sending via Gmail
- ✅ Error handling configured

### STEP 3 - Resolution Flow (IN PROGRESS)
- ✅ Webhook configured (/resolution)
- ✅ HMAC signature validation logic
- ✅ IF decision branching
- ✅ Google Sheets read operations
- ✅ Google Sheets update operations
- ⏳ Activity logging
- ⏳ Response nodes (200/401)
- ⏳ End-to-end testing

---

## 📅 Next Steps

### STEP 3 Completion (This Week):
- [ ] Build NODE 6: Activity Log Entry
- [ ] Build NODE 7-9: Response Nodes
- [ ] Complete end-to-end testing
- [ ] Verify all data flows

### STEP 4: Testing Phase
- [ ] Test Case 1: Successful support request
- [ ] Test Case 2: Duplicate detection
- [ ] Test Case 3: Email confirmation
- [ ] Test Case 4: Valid signature resolution
- [ ] Test Case 5: Invalid signature handling
- [ ] Test Case 6: Error log verification
- [ ] Test Case 7: Activity log verification
- [ ] Test Case 8: Retry & admin alerts

### STEP 5: Documentation & Demo
- [ ] Record Loom video (max 10 minutes)
- [ ] Create Postman collection export
- [ ] Write deployment guide
- [ ] Document troubleshooting guide
- [ ] Create API reference

---

## 📁 Documentation Files

| File | Purpose | Status |
|------|---------|--------|
| **README.md** | Project overview & setup | ✅ Current |
| **CHAT-REFERENCE-SESSION-SUMMARY.md** | Complete session notes | ✅ Complete |
| **STEP3-RESOLUTION-WORKFLOW-NODES-3-4-5.md** | Detailed node documentation | ✅ Complete |
| **STEP3-RESOLUTION-WORKFLOW-NODES-3-4-5.pdf** | PDF version of STEP 3 docs | ✅ Complete |
| **error-handling-workflow.json** | Error handling flow (importable) | ✅ Created |
| **Ticket_Tracking.csv** | Sheet template | ✅ Created |
| **Error_Log.csv** | Error log template | ✅ Created |
| **Activity_Log.csv** | Activity log template | ✅ Created |

---

## 🎯 Key Learnings

### Data Flow Patterns:
```
Webhook Input → Validate → Check Duplicates
    ↓
    ├─ IF Branch (Decision)
    │
    ├─ Read Sheets
    │
    ├─ Transform Data (Set node)
    │
    ├─ External API Call (HTTP)
    │
    ├─ Write to Sheets (Append/Update)
    │
    └─ Send Notifications (Email)
```

### Node-to-Node References:
```javascript
$json.field                           // Current node data
$node["Node Name"].json.field         // Reference to other node
$node["Validate Input"].json.field    // Specific node example
{{ new Date().toISOString() }}        // Current timestamp
```

### Important Settings:
- **Always Output Data:** Enable in Sheets nodes when data might be empty
- **Expression Mode:** Toggle for dynamic field values
- **Nested JSON Access:** Use `.json.json` for httpbin responses
- **HMAC Validation:** Case-sensitive hash comparison required

---

## 👤 Project Information

- **User:** Mussab Arshad
- **Project:** TickSync - N8N Ticketing System Integration
- **Start Date:** October 6, 2026
- **Current Status:** STEP 3 IN PROGRESS
- **Language:** Roman Urdu + English
- **Communication Style:** Step-by-step, node-by-node guidance

---

## 📞 Support & Contact

For issues, questions, or clarifications:
1. Check **CHAT-REFERENCE-SESSION-SUMMARY.md** for previous discussions
2. Review **STEP3-RESOLUTION-WORKFLOW-NODES-3-4-5.pdf** for detailed guidance
3. Refer to **Common Issues & Solutions** section above
4. Check Google Sheets for error logs and activity logs

---

## 📝 Document Info

- **Version:** 1.0
- **Created:** October 6, 2026
- **Last Updated:** October 6, 2026
- **Status:** ACTIVE & MAINTAINED
- **Next Review:** After STEP 3 completion

---

**TickSync - Making Support Ticket Management Automated & Secure! 🚀**
