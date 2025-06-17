# Oracle Cloud Procurement Agent Uploader 🚀

<!-- Insert your architecture/sequence diagram here -->
![image](https://github.com/user-attachments/assets/8b48ead7-d10a-4ac6-bec9-1fab53e0799f)


## 📘 Overview
This repository contains Python scripts and a supporting SQL query designed to automate the creation and management of Procurement Agents in **Oracle Fusion Cloud Procurement** via REST APIs. It eliminates the manual entry process through the Fusion UI, increasing efficiency and accuracy.

## 🔍 Contents
- **`AgenttoAgentId.sql`** – Run this in your Fusion database to generate the `AgenttoAgentId_Report`, which maps agent names to internal IDs.
- **`deploy_auto.py`** – Batch mode: reads `input.xlsx` (usernames + Business Units), builds the payload, and invokes Fusion Procurement APIs automatically.
- **`deploy_ui.py`** – GUI wrapper over `deploy_auto.py`, enabling file selection and configuration through a dialog interface.
- **`deploy_manual.py`** – Manually prompt for agent name + BU in the console; useful for quick, one-off uploads.

## 🧩 How It Works
1. Use `.env` to store your Fusion REST credentials (`FUSION_USERNAME`, `FUSION_PASSWORD`, `FUSION_BASE_URL`).
2. `deploy_auto.py` / `deploy_ui.py` / `deploy_manual.py` read your input data.
3. Scripts build payloads with:
   - Status = Active  
   - Permissions (e.g. `ManageRequisitionsAllowedFlag = True`, `AccessLevelToOtherAgentsRequisitions = "Full"`)
   - …plus additional access and management flags.
4. The REST API is called to PATCH Procurement Agents in Fusion.

## 🛠️ Prerequisites
- Python 3.x
- Fusion REST access permissions
- Packages: `pandas`, `requests`, `python-dotenv`, `openpyxl`
- OS tools: For `deploy_ui.py`, GUI file-dialog support

## ⚙️ Setup & Usage
```bash
git clone https://github.com/WilsonH918/Oracle-Fusion-Role-Automation-Pipeline.git
cd Oracle-Fusion-Role-Automation-Pipeline
pip install -r requirements.txt
```
1. Add your Fusion credentials to a `.env` file:
```bash
FUSION_USERNAME=...
FUSION_PASSWORD=...
FUSION_BASE_URL=https://your-fusion-instance.oraclecloud.com
```

3. Place the following files in the project root:
- `AgenttoAgentId_Report.xlsx` (generated from the SQL script)
- `input.xlsx` (list of agents and BUs to be uploaded)

3. Run one of the following scripts:
```bash
python deploy_auto.py   # Fully automated, no prompts
python deploy_ui.py     # File dialog GUI
python deploy_manual.py # Manual prompt for agent and BU
```
## 🧪 Examples

- **Bulk Upload Example:**
   Prepare `input.xlsx` with the following structure:
   | Agent Name | Business Unit |
   |------------|----------------|
   | Jane Smith | US BU          |
   | John Doe   | UK BU          |

   Then run:
   ```bash
   python deploy_auto.py
   python deploy_manual.py
   ```

## 📘 Oracle Fusion Context
This solution is tailored for use within the Oracle Fusion Procurement module:
Oracle requires Procurement Agent IDs to be tied to users in order to grant purchasing privileges.
This repo uses Fusion’s REST API (/fscmRestApi/resources/11.13.18.05/procurementAgents) to configure those relationships.
Ideal for system integrators and support teams automating large-scale Procurement agent setups or updates.


