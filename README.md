# Build-Multi-Payment-Workflows
# n8n Workflow: Automated Order & Payment Emails from Google Sheets

This n8n workflow automatically sends emails when a new row is added to a Google Sheet. It checks the payment method (Card / COD / Wallet), sends a confirmation email, and finally sends an order summary.

🔧 How It Works

1. **Trigger** – `Google Sheets Trigger`  
   Monitors a Google Sheet for new rows (every minute).

2. **Switch Router** – `Switch`  
   Reads the `payment` column and routes to one of three branches:
   - `card` (credit/debit card)
   - `cod` (cash on delivery)
   - `wallet` (digital wallet)

3. **Payment Emails** – `Send a message`, `Send a message1`, `Send a message2`  
   Each sends a personalized email thanking the customer and confirming the payment method.

4. **Merge** – `Merge`  
   Combines the three possible output branches back into one stream.

5. **Final Order Email** – `Send a message3`  
   Sends a second email containing the order ID and a completion message.

## 📋 Prerequisites

- [n8n](https://n8n.io/) (self-hosted or cloud)
- Google account (for Sheets & Gmail)
- Gmail API enabled
- Google Sheets API enabled

## ⚙️ Setup Instructions

### 1. Import the Workflow
- In n8n, go to **Workflows** → **Import from File**.
- Upload the sanitized JSON file (after replacing placeholders).

### 2. Replace Placeholders
Edit the workflow nodes and replace:

| Placeholder | What to put |
|-------------|--------------|
| `YOUR_GOOGLE_SHEET_DOCUMENT_ID` | The ID from your Google Sheets URL (`/d/.../edit`) |
| `YOUR_SHEET_NAME` | The sheet tab name (e.g., `Sheet1`) |
| `YOUR_GOOGLE_SHEETS_CREDENTIAL_ID` | Your OAuth credential ID for Google Sheets |
| `YOUR_GMAIL_CREDENTIAL_ID` | Your OAuth credential ID for Gmail |
| `YOUR_CREDENTIAL_NAME` | A descriptive name (e.g., "My Google Account") |
| `YOUR_WEBHOOK_ID_PLACEHOLDER` | Any unique string or leave as is (optional) |

### 3. Connect Your Accounts
- **Google Sheets Trigger**: Authenticate with the Google account that owns the sheet.
- **Gmail nodes**: Authenticate with the Gmail account that will send emails.

### 4. Prepare the Google Sheet
Your sheet must have at least these columns:
- `name` – customer name
- `email` – customer email address
- `payment` – values: `card`, `cod`, or `wallet`
- `order ID` – order identifier

### 5. Activate the Workflow
Toggle the **Active** switch in n8n. New rows will now trigger emails automatically.

## 📝 Example Sheet Row

| name        | email                 | payment | order ID |
|-------------|-----------------------|---------|----------|
| John Doe    | john@example.com      | card    | #12345   |

## 🔒 Security Notes

- **Never commit real credentials or document IDs** to GitHub.
- This repository includes a **sanitized** JSON. Before using it, replace all placeholders with your own secrets.
- Use n8n’s **credential** system – don’t hard‑code tokens inside the workflow.

## 📎 Troubleshooting

- **No email received** – Check that the email address exists and the Gmail node is authenticated.
- **Switch not routing** – Ensure the `payment` column exactly matches `card`, `cod`, or `wallet` (case‑sensitive).
- **Trigger not firing** – Verify the Google Sheet is shared with the authenticated account and the document ID is correct.

## 🧩 License

Feel free to use, modify, and share this workflow. Attribution is appreciated but not required.
