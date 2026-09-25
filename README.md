# n8n-smart-upsert-crm

# 🔀 PROJECT 05: Smart CRM Upsert Logic & Deduplication Pipeline

**Category:** Workflow Logic, Database Deduplication, CRM Automation  
**Level:** Intermediate  
**Status:** Completed  

## 🎯 Business Problem
Duplicate data is a major pain point in CRM systems. When users submit forms multiple times (e.g., to update their status), traditional automations blindly create duplicate rows, leading to messy databases and inaccurate reporting.

## 💡 Solution
Engineered a "Smart Upsert" (Update or Insert) workflow in n8n. The system intercepts incoming webhook data, queries the database to check for existing records, and uses conditional routing to decide whether to update an existing row or create a brand new one.

## ⚙️ Architecture & Logic
<img width="1691" height="780" alt="image" src="https://github.com/user-attachments/assets/09546613-c8f3-4fde-96c1-566c9cf6276f" />

*   **Trigger:** `Webhook Node` (Receives POST requests with `email` and `status`).
*   **Validation:** `Get row(s)` (Queries the database for the exact email).
*   **Routing (The "Brain"):** `IF Node` 
    *   **Condition:** Evaluates if the returned database `id` is empty.
    *   **True Branch (Data Exists):** Routes to the `Update row(s)` node to modify the status using Cross-node Referencing.
    *   **False Branch (No Data):** Routes to the `Insert row` node to create a new lead.
*   **Error Handling:** Implemented "Always Output Data" to prevent execution stops on null queries, and enabled automatic type casting (Number to String) for database ID validation.

## 📈 Business Value & Impact
*   **Zero Duplicates:** Ensures 100% database hygiene.
*   **Dynamic Referencing:** Utilized n8n's expression engine `{{ $('Node').item.json... }}` to pull data across complex workflow branches.
