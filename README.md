# 🚀 Automated Ticketing System (API + Workflow Automation)

## 📌 Overview
This project automates the **support ticketing process** using **Trello API, Python, and Zapier**. It allows support teams to:
- 📌 **Log new tickets into Trello automatically** from a CSV or API.
- 📌 **Send Slack/email notifications** when new tickets are created.
- 📌 **Update ticket statuses** automatically when resolved.

---

## 🔧 Tech Stack
- **Python** (`requests`, `pandas`) → API calls & CSV handling
- **Trello API** → Ticket management
- **Zapier (Optional)** → Automates Slack/email notifications
- **Postman (Optional)** → For API testing

---

## 📌 Setup Instructions

1. **Install Required Software**
   - [Download Python](https://www.python.org/downloads/)
   - [Download Postman](https://www.postman.com/downloads/)
   - [Download VS Code (Optional)](https://code.visualstudio.com/Download)

2. **Install Required Python Libraries**  
   Run the following command in your terminal:
   ```bash
   pip install requests pandas

### 📌 Step 3: Get Trello API Credentials

To connect this project with Trello, you need an **API Key, Token, and List ID**.

#### **1️⃣ Get Your Trello API Key & Token**
1. **Sign up for Trello** → [Trello Signup](https://trello.com/)
2. **Get Your API Key** → [Trello API Keys](https://trello.com/app-key)  
   - Copy your **API Key** and save it.
3. **Get Your Trello Token**  
   - On the same API page, click **"Generate a Token"**.
   - Approve the request and copy your **Token**.

#### **2️⃣ Get Your Trello List ID**
Trello organizes data as **Boards → Lists → Cards**. You need the **List ID** where new tickets will be stored.

1. **Create a Trello Board** (e.g., `"Support Tickets"`).
2. **Create a List** inside the board (e.g., `"Open Tickets"`).
3. **Find Your List ID** by using this API request in your browser:

4. - Replace `YOUR_BOARD_ID` with your **Trello Board ID**.
- Replace `YOUR_API_KEY` and `YOUR_TOKEN` with your actual credentials.
- The **response will show multiple lists** with their corresponding IDs.
- Copy the `"id"` value for the `"Open Tickets"` list.

#### **3️⃣ Save Your Credentials for Later**
Once you have:
- **API Key**
- **Token**
- **List ID**

You’ll need to **replace these in your Python scripts** for the automation to work.

✅ **Now you’re ready to integrate Trello with Python!**

### 📌 Step 4: Create a Sample Ticketing System (CSV)

To simulate a help desk ticketing system, we’ll use a **CSV file** to store incoming support tickets before they are logged into Trello. This file will act as a **mock database**, making it easy to test our automation.

Create a file named **`sample_ticket_data.csv`** inside your project folder and add the following content:

```csv
ticket_id,title,description,status
1,Login Issue,User unable to log in,Open
2,Network Down,Building automation system offline,Open
3,API Error,OptigoVN API failing to authenticate,Open

### 📌 Step 4: Create a Sample Ticketing System (CSV)

To simulate a help desk ticketing system, we’ll use a **CSV file** to store incoming support tickets before they are logged into Trello. This file will act as a **mock database**, making it easy to test our automation.

Create a file named **`sample_ticket_data.csv`** inside your project folder and add the following content:

```csv
ticket_id,title,description,status
1,Login Issue,User unable to log in,Open
2,Network Down,Building automation system offline,Open
3,API Error,OptigoVN API failing to authenticate,Open
```
This CSV file contains four columns:

ticket_id → Unique identifier for each ticket.
title → Short summary of the issue.
description → Detailed explanation of the problem.
status → The current ticket status (Open, Resolved, etc.).
This CSV file will allow us to:

Store sample support tickets before integrating with an actual ticketing system.
Process and update tickets automatically using Python and Trello API.
Easily expand functionality by adding more fields if needed.
✅ Now, your project has a dataset that can be used to automate ticket processing!

### 📌 Step 5: Create a Python Script to Log Tickets in Trello

Now that we have our sample support tickets stored in a CSV file, we will create a Python script to **read the tickets and send them to Trello automatically** using the Trello API.

#### **1️⃣ Create a Python File**
1. In your project folder, create a new file named **`trello_integration.py`**.
2. Copy and paste the following Python script into the file:

   ```python
   import requests
   import pandas as pd

   # Trello API Credentials
   API_KEY = "YOUR_TRELLO_API_KEY"
   TOKEN = "YOUR_TRELLO_TOKEN"
   LIST_ID = "YOUR_TRELLO_LIST_ID"
   TRELLO_URL = "https://api.trello.com/1/cards"

   # Load tickets from CSV
   df = pd.read_csv("sample_ticket_data.csv")

   def create_trello_ticket(title, description):
       params = {
           "key": API_KEY,
           "token": TOKEN,
           "idList": LIST_ID,
           "name": title,
           "desc": description
       }
       response = requests.post(TRELLO_URL, params=params)
       if response.status_code == 200:
           print("✅ Ticket added to Trello:", response.json()["shortUrl"])
       else:
           print("❌ Error:", response.json())

   # Loop through all Open tickets
   for _, row in df[df["status"] == "Open"].iterrows():
       create_trello_ticket(row["title"], row["description"])
✅ Now, all open tickets from the CSV file will be automatically added to Trello!

### 📌 Step 6: Automate Ticket Notifications with Zapier

Now that tickets are being logged in Trello, we will use **Zapier** to send automatic **Slack or Email notifications** whenever a new ticket is created.

#### **1️⃣ Sign Up for Zapier**
1. Go to **[Zapier Signup](https://zapier.com/)** and create a free account.
2. Click **"Create Zap"** to begin setting up an automation.

#### **2️⃣ Set Up the Zapier Automation**
- **Trigger:**  
  1. Select **Trello** as the trigger app.  
  2. Choose **"New Card in List"** as the event.  
  3. Connect your Trello account.  
  4. Select your **Trello Board** and **List** (e.g., `"Open Tickets"`).  
  5. Click **Continue** and test the trigger to pull sample data.

- **Action:**  
  1. Choose your preferred notification method:
     - **Slack:** Select `"Send Channel Message"`.
     - **Email:** Select `"Send Outbound Email"`.
  2. Connect your Slack or Email account.
  3. Customize the message:
     ```
     🚀 New Support Ticket Created!
     🔹 Title: {{Card Name}}
     🔹 Description: {{Card Description}}
     🔹 View Ticket: {{Card URL}}
     ```
  4. Click **Test & Continue**.

#### **3️⃣ Activate the Zap**
- Click **"Turn on Zap"** to activate the automation.
- Now, every time a new ticket is created in Trello, **Zapier will send an instant Slack or Email notification**.

✅ **Your support team is now automatically alerted whenever a new ticket is added!**







