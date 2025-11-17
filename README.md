# 💰 AI-Powered Expense Tracker & Analyzer

An intelligent n8n workflow that automatically tracks bank transactions from email notifications, categorizes expenses using AI, and provides conversational expense analysis through a chat interface.

## 🎯 What This Workflow Does

### **Automated Email Processing (Flow 1)**
1. **Monitors Email**: Continuously watches your inbox for bank transaction notifications via IMAP
2. **Extracts Transaction Data**: Parses email content to extract:
   - Transaction date
   - Merchant/recipient name
   - Transaction amount (debited/credited)
3. **Logs to Google Sheets**: Automatically appends each transaction to a "Transactions" sheet
4. **Aggregates by Merchant**: Calculates total spending per merchant
5. **AI Categorization**: Uses Mistral AI to analyze each merchant's total spending and:
   - Categorizes as "Yeah, that's fine" / "Need concern" / "Its overbudget"
   - Provides personalized financial tips
   - Distinguishes between necessities (utilities, groceries) and discretionary spending (food delivery, shopping)
6. **Updates Balance Sheet**: Maintains a running summary with AI-generated insights

### **Conversational Analysis (Flow 2)**
1. **Chat Interface**: Provides a chat-based interface to query your expenses
2. **Context-Aware AI**: Reads your expense data and engages in conversation to:
   - Ask clarifying questions about unclear transactions
   - Identify unusual spending patterns
   - Generate detailed expense reports on demand
3. **Memory-Enabled**: Remembers conversation context across multiple messages
4. **Interactive Reporting**: Only generates reports after understanding all transactions and getting your approval

## 🏗️ Workflow Architecture

### **Flow 1: Email → AI Analysis → Google Sheets**
Email Trigger (IMAP)
↓
Extract Transaction Data (Code)
↓
Append to Transactions Sheet
↓
Read All Transactions
↓
Calculate Merchant Totals (Code)
↓
Update Balance Sheet with Totals
↓
AI Expense Analyzer (Mistral)
↓
Parse AI Response (Code)
↓
Update Balance Sheet with AI Insights


### **Flow 2: Chat → Data Retrieval → AI Analysis**
Chat Trigger
↓
Merge with Balance Sheet Data
↓
Format Data for AI (Code)
↓
AI Expense Assistant (Mistral + Memory)
↓
Return Analysis/Report


## 📊 Google Sheets Structure

### **Sheet 1: Transactions**
| Date | Merchant | Amount |
|------|----------|--------|
| 10 Nov 2025 | ZOMATO | 450 |
| 11 Nov 2025 | JIO RECHARGE | 299 |

### **Sheet 2: Balance Sheet**
| Merchant | Total Amount | Note | Category |
|----------|--------------|------|----------|
| ZOMATO | 1250 | Review monthly budget | Need concern |
| JIO RECHARGE | 299 | Normal utility bill | Yeah, that's fine |

## 🔧 Required Credentials

1. **IMAP Email Account**
   - Your email provider's IMAP settings
   - Used to monitor transaction notification emails

2. **Google Sheets OAuth2**
   - Google account with access to your expense tracking spreadsheet
   - Permissions: Read and write to sheets

3. **Mistral Cloud API**
   - API key from Mistral AI
   - Used for expense categorization and conversational analysis

## ⚙️ Setup Instructions

### **1. Import the Workflow**
- Download `expense-tracker.json`
- In n8n: Click "+" → "Import from File" → Select the JSON file

### **2. Configure Email Monitoring**
- Open "Email Trigger (IMAP)" node
- Add your IMAP credentials:
  - Host: `imap.gmail.com` (or your provider)
  - Port: `993`
  - Email & password
- Set mailbox to `INBOX`

### **3. Set Up Google Sheets**
- Create a new Google Spreadsheet with two sheets:
  - **Transactions**: Columns: Date, Merchant, Amount
  - **Balance sheet**: Columns: Merchant, Total Amount, Note, Category
- Copy the spreadsheet URL
- Update all Google Sheets nodes with your spreadsheet URL
- Connect your Google account via OAuth2

### **4. Configure AI**
- Get Mistral API key from [console.mistral.ai](https://console.mistral.ai)
- Add credentials to both Mistral Chat Model nodes

### **5. Customize Transaction Parsing (Optional)**
- Edit "Code in JavaScript" node if your bank's email format differs
- Adjust regex patterns to match your transaction notification format

### **6. Activate the Workflow**
- Toggle the workflow to "Active"
- Test by sending a sample transaction email

## 💬 Using the Chat Interface

1. **Access the chat**: Click on "When chat message received" node to get the chat URL
2. **Ask questions**:
   - "What are my total expenses?"
   - "Show me my highest spending categories"
   - "Where can I save money?"
3. **The AI will**:
   - Ask clarifying questions about unclear transactions
   - Request approval before generating reports
   - Provide actionable insights




## 🔍 How It Works

### **Transaction Extraction Logic**
The workflow uses regex patterns to extract:
- **Amount**: Matches "debited by" or "credited by" followed by numbers
- **Date**: Extracts dates in format `DDMmmYY` (e.g., 10Nov25)
- **Merchant**: Captures text after "trf to" and before reference number

### **AI Categorization**
Mistral AI analyzes spending based on:
- **Amount thresholds**: Different limits for different spending levels
- **Merchant type**: Distinguishes necessities vs. discretionary
- **Context awareness**: Recognizes common services (Jio, Zomato, etc.)

### **Conversational Analysis**
The chat AI:
- Reads all expense data from the Balance Sheet
- Asks questions about unclear or unusual transactions
- Waits for user approval before generating reports
- Maintains conversation context using memory

## 📈 Use Cases

- **Personal Finance Tracking**: Automatically log all bank transactions
- **Budget Monitoring**: Get AI alerts when spending exceeds thresholds
- **Expense Analysis**: Chat with AI to understand spending patterns
- **Financial Planning**: Identify areas to reduce unnecessary expenses
- **Monthly Reports**: Generate comprehensive expense summaries on demand
