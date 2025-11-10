# ☁️ Azure File Uploader (Python · Azure Functions)

A lightweight **serverless file uploader** built with **Azure Functions (Python)** and **Azure Blob Storage**.  
It accepts an HTTP POST request containing raw file data, uploads it securely to a private Blob container, and returns a confirmation message.

---

## 🧱 Architecture Overview

Client (Browser / CLI)
↓ (HTTP POST)
Azure Function (HTTP Trigger · Python)
↓ (SDK call)
Azure Blob Storage (Container: uploads)


- The Function acts as a bridge between your client and Azure Storage.  
- The Blob SDK handles authentication, chunking, retries, and data integrity.  
- All credentials are stored securely in **Azure App Settings**, not in code.

---

## ⚙️ Local Development

### 1️⃣ Prerequisites
- Python 3.10+
- Azure CLI
- Azure Functions Core Tools (`func`)
- An Azure Storage Account

### 2️⃣ Setup
```bash
# create virtual environment
python -m venv .venv
. .\.venv\Scripts\Activate.ps1

# install dependencies
pip install -r requirements.txt

3️⃣ Local Configuration

Create local.settings.json (DO NOT commit this file)

{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AZURE_STORAGE_CONNECTION_STRING": "YOUR_CONNECTION_STRING_HERE"
  }
}

4️⃣ Run Locally
func start
# test in another terminal
Invoke-RestMethod -Uri "http://localhost:7071/api/upload_file?filename=test.txt" -Method POST -Body "Hello from local!"

Expected response:
✅ Uploaded 'test.txt'

🚀 Deployment to Azure

Ensure these are already created:

Resource Group

Storage Account

Function App

Deploy the Function:
func azure functionapp publish func-fileuploader-realtime

Retrieve your function key:
$KEY = $(az functionapp function keys list `
  --name func-fileuploader-realtime `
  --resource-group rg-fileuploader-dev `
  --function-name upload_file `
  --query "default" -o tsv)

Test live:
Invoke-RestMethod -Uri "https://func-fileuploader-realtime.azurewebsites.net/api/upload_file?filename=live_test.txt&code=$KEY" -Method POST -Body "Hello from Azure!"

Expected response:
✅ Uploaded 'live_test.txt'

🔐 Security Practices
Risk	Safe Practice
Secrets in code	Use Azure App Settings or Environment Variables
Public container	Keep container private
Sharing files	Use SAS links (time-limited, read-only)
Function exposure	Keep authLevel=function (requires a key)

💸 Cost Model
Runs on the Consumption Plan, so you pay only when the Function executes.
No uploads = no charges.

🧠 Key Concepts

Event-driven serverless architecture

HTTP triggers & function keys

Secure configuration management

Azure Blob SDK (Python)

Scaling & pay-per-use efficiency

📂 Repository Structure
file-uploader/
│
├── function_app.py           # main Function logic
├── host.json                 # function host config
├── requirements.txt          # Python dependencies
├── .gitignore                # excludes secrets / build files
└── README.md                 # documentation (this file)

⚠️ Notes

local.settings.json is excluded intentionally — never commit secrets.

For large files, the Azure Blob SDK automatically performs chunked uploads and retries.

You can extend this project by:

Adding SAS link generation for secure downloads

Listing uploaded files

Integrating a simple web front-end

🧾 License

Open for educational and personal use.

Author: Priyanshu Rai
GitHub: @priyanshurai5432-droid



