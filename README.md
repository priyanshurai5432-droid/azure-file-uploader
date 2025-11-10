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
