# Azure File Uploader (Python, Azure Functions)

Small serverless API that uploads the HTTP request body to **Azure Blob Storage** (`uploads` container).

## Architecture
Client (POST) → Azure Function (HTTP trigger) → Blob Storage (`uploads/filename`)

## Local Dev
```bash
python -m venv .venv
. .\.venv\Scripts\Activate.ps1
pip install -r requirements.txt

# create local.settings.json manually (do NOT commit)
# {
#   "IsEncrypted": false,
#   "Values": {
#     "FUNCTIONS_WORKER_RUNTIME": "python",
#     "AZURE_STORAGE_CONNECTION_STRING": "YOUR_CONNECTION_STRING_HERE"
#   }
# }

func start
# POST http://localhost:7071/api/upload_file?filename=test.txt
Deploy
bash
Copy code
func azure functionapp publish <YOUR_FUNCTION_APP_NAME>
Live Usage
Authenticated call (function-level auth):

powershell
Copy code
# Retrieve function key
$KEY = $(az functionapp function keys list `
  --name <YOUR_FUNCTION_APP_NAME> `
  --resource-group <YOUR_RG> `
  --function-name upload_file `
  --query "default" -o tsv)

# Upload
Invoke-RestMethod -Uri "https://<YOUR_FUNCTION_APP_NAME>.azurewebsites.net/api/upload_file?filename=sample.txt&code=$KEY" -Method POST -Body "Hello"
Security
Storage container is private

No secrets in code; use Azure App Settings in production

Share files using SAS links (time-limited) if needed

Notes
This repo intentionally excludes local.settings.json and any keys.

For large files, the SDK handles chunked uploads & retries.

sql
Copy code

Then commit & push:
```bash
git add README.md
git commit -m "Add README with setup, deploy, and security notes"
git push
Quick check & question (explain your reasoning):
Why is it dangerous to include a function key or connection string inside README examples, and how did we avoid that in the commands above?





You said:
can you give me full redme 



ChatGPT said:
Absolutely — here’s a clean, production-safe README.md you can paste directly into your repo.
Everything’s already structured for a recruiter, interviewer, or peer to read easily.

markdown
Copy code
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

yaml
Copy code

- The Function acts as a bridge between your client and Azure Storage.  
- The Blob SDK handles authentication, chunking, retries, and data integrity.  
- All credentials are stored securely in **Azure App Settings**, not in code.

---

## ⚙️ Local Development

### 1️⃣ Prerequisites
- Python 3.10+
- Azure CLI
- Azure Functions Core Tools (`func`)
- An Azure Storage account

### 2️⃣ Setup
```bash
# create virtual environment
python -m venv .venv
. .\.venv\Scripts\Activate.ps1

# install dependencies
pip install -r requirements.txt
3️⃣ Local configuration
Create local.settings.json (DO NOT commit this file):

json
Copy code
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AZURE_STORAGE_CONNECTION_STRING": "YOUR_CONNECTION_STRING_HERE"
  }
}
4️⃣ Run locally
bash
Copy code
func start
# test in another terminal
Invoke-RestMethod -Uri "http://localhost:7071/api/upload_file?filename=test.txt" -Method POST -Body "Hello from local!"
Expected response:

bash
Copy code
✅ Uploaded 'test.txt'
🚀 Deployment to Azure
Ensure you already created:

Resource Group

Storage Account

Function App

Publish the Function:

bash
Copy code
func azure functionapp publish func-fileuploader-realtime
Retrieve your function key:

bash
Copy code
$KEY = $(az functionapp function keys list `
  --name func-fileuploader-realtime `
  --resource-group rg-fileuploader-dev `
  --function-name upload_file `
  --query "default" -o tsv)
Test live:

bash
Copy code
Invoke-RestMethod -Uri "https://func-fileuploader-realtime.azurewebsites.net/api/upload_file?filename=live_test.txt&code=$KEY" -Method POST -Body "Hello from Azure!"
🔐 Security Practices
Risk	Safe Practice
Secrets in code	Use Azure App Settings or Environment Variables
Public container	Keep container private
Sharing files	Use SAS links (time-limited, read-only)
Function exposure	Keep authLevel=function (requires key)

💸 Cost Model
Runs on the Consumption Plan → you pay only when the Function executes.
No uploads = no charges.

🧠 Concepts Learned
Event-driven serverless architecture

HTTP triggers and function keys

Azure Blob SDK usage (Python)

Secure configuration management

Scaling and cost efficiency of serverless

📂 Repository Structure
bash
Copy code
file-uploader/
│
├── function_app.py           # main Function logic
├── host.json                 # function host config
├── requirements.txt          # Python dependencies
├── .gitignore                # excludes secrets / build files
└── README.md                 # documentation (this file)
⚠️ Notes
local.settings.json is intentionally excluded — never commit secrets.

For large files, Azure Blob SDK automatically performs chunked uploads and retries.

You can extend this project to include:

SAS link generation for downloads

File listing API

Front-end integration (React, Flutter, etc.)

🧾 License
Open for educational and personal use.

Author: Priyanshu Rai
GitHub: @priyanshurai5432-droid

yaml
Copy code

---

When you paste this into `README.md`, run:

```bash
git add README.md
git commit -m "Add full project documentation"
git push
✅ Quick reflection (one sentence):
Why do you think this README avoids showing real keys and uses placeholders instead?





You said:
no like a full redmen which i can just copy paste 



ChatGPT said:
Got it — here’s a ready-to-paste, complete README.md for your GitHub repo.
It’s written exactly as you can publish it — professional tone, fully formatted, no missing pieces, and no secrets.
Just copy everything between the lines and paste it into your README.md.

markdown
Copy code
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

yaml
Copy code

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
Create local.settings.json (DO NOT commit this file):

json
Copy code
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AZURE_STORAGE_CONNECTION_STRING": "YOUR_CONNECTION_STRING_HERE"
  }
}
4️⃣ Run Locally
bash
Copy code
func start
# test in another terminal
Invoke-RestMethod -Uri "http://localhost:7071/api/upload_file?filename=test.txt" -Method POST -Body "Hello from local!"
Expected response:

bash
Copy code
✅ Uploaded 'test.txt'
🚀 Deployment to Azure
Ensure these are already created:

Resource Group

Storage Account

Function App

Deploy the Function:

bash
Copy code
func azure functionapp publish func-fileuploader-realtime
Retrieve your function key:

bash
Copy code
$KEY = $(az functionapp function keys list `
  --name func-fileuploader-realtime `
  --resource-group rg-fileuploader-dev `
  --function-name upload_file `
  --query "default" -o tsv)
Test live:

bash
Copy code
Invoke-RestMethod -Uri "https://func-fileuploader-realtime.azurewebsites.net/api/upload_file?filename=live_test.txt&code=$KEY" -Method POST -Body "Hello from Azure!"
Expected response:

bash
Copy code
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
bash
Copy code
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

yaml
Copy code

---

✅ **After pasting:**  
Run:
```bash
git add README.md
git commit -m "Add full project documentation"
git push
