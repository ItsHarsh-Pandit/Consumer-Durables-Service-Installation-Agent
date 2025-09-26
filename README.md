# 🛠️ Consumer Durables Service & Installation Agent

An **AI-powered voice & chat assistant** built on **Inya.ai** to handle **Service Requests** and **Installation Bookings** for consumer durables like:

* 🌀 Washing Machine
* ❄️ Refrigerator
* 🌬️ Air Conditioner (AC)
* 💧 Water Purifier
* 📺 Television

The agent is designed to provide a **real-world experience** of a service call center assistant with intelligent triaging, technician assignment, and escalation to human agents when required.

---

## 📌 Table of Contents

1. [Introduction](#introduction)
2. [How We Built the Agent (Step-by-Step)](#how-we-built-the-agent-step-by-step)

   * Agent Setup
   * System Prompt
   * Conversation Flow
   * Transcriber & ASR Settings
   * Knowledge Base
   * API Integration
   * Voice & Speech
3. [Edge Cases Handled](#edge-cases-handled)
4. [API Used](#api-used)
5. [Mock Data](#mock-data)
6. [Architecture Overview](#architecture-overview)
7. [Project Highlights](#project-highlights)
8. [Demo Video](#demo-video)
9. [Contributors](#contributors)

---

## 📖 Introduction

This project was developed for the **Inya Hackathon 2025**.
It solves the **real-world challenge** of booking appliance services and installations by:

* Detecting **intent** (Service / Installation).
* Asking **appliance-specific clarifying questions**.
* Collecting and verifying **customer details**.
* Checking the **validity of pincodes** via a public API.
* Matching and scheduling a **qualified technician** using mock data.
* Handling **edge cases** gracefully (invalid input, unavailable slots, API failures).
* Offering a **seamless escalation** to human agents when required.

---

## ⚙️ How We Built the Agent (Step-by-Step)

### 1. Agent Setup in Inya.ai

1. **Login** → Go to [Inya.ai](https://inya.ai)
2. Click **Agents** → **Create New Agent**
3. Fill in details:

   * **Name** → *Consumer Durables Service & Installation Agent*
   * **Region** → India
   * **Time Zone** → Asia/Kolkata (+05:30)
   * **Description** → *AI agent to assist customers with service requests and installations for consumer durables.*
   * **Select Icon/Logo** → Uploaded branded service logo

---

### 2. System Prompt (Core Instruction)

The **System Prompt** is the brain of the agent. Example:

```
You are a customer service assistant for a consumer durables platform.  
Your role is to help customers with either:
1. Service Requests  
2. Installations  

You must:  
- Detect intent (service / installation).  
- Ask appliance-specific clarifying questions.  
- Collect & validate customer details (name, phone, email, pincode, address).  
- Call pincode verification API and fetch region.  
- Assign technician from mock data based on skill, region & availability.  
- Suggest and confirm booking slots.  
- Summarize appointment details clearly.  
- Escalate to a live agent when requested or if rules trigger.  

Tone: Supportive, calm, solution-oriented.  
Do not blame the customer. Always provide reassurance.  
```

---

### 3. Conversation Flow

* **Greeting Message:**
  “Hello! 👋 I’m your Service & Installation Assistant. How can I help you today?”

* **Flow Examples:**

  * Customer says: “My AC is not cooling.”
    → Agent asks clarifying questions (cooling effectiveness, airflow, leakage, error code).
  * Customer says: “I need a new washing machine installed.”
    → Agent asks installation details (model, address, time slot preference).

* **Ending Message:**
  “✅ Your booking is confirmed. Thank you for choosing us! Have a great day.”

* **Escalation Flow:**
  If technician unavailable OR customer requests → Agent summarizes details and transfers to a live agent.

---

### 4. Transcriber & ASR Settings

* **ASR Provider:** Google ASR
* **Noise Filtering:** Enabled
* **Interruption Handling (Barge-in):** Enabled
* **Language:** English (India)

---

### 5. Knowledge Base

We uploaded the following:

* **FAQ Document** → Common troubleshooting steps for appliances.
* **Example Conversations** → To guide AI during edge cases.
* **Technician Data (`technicians.json`)** → Mock technician details (skills, regions, availability).

---

### 6. API Integration

**API Used for Pincode Verification:**

```bash
https://api.postalpincode.in/pincode/{PINCODE}
```

* **Action in Inya.ai:**

  * **Name:** VerifyPincode
  * **Description:** Verifies customer pincode & maps it to region.
  * **Trigger:** When user provides a 6-digit pincode.
  * **Variables:**

    * `pincode` → from user input.
    * Response → District/Region mapping.

---

### 7. Voice & Speech

* **Voice Selected:** “Aria” (Female, Indian English accent)
* **Rate of Speech:** 1.0x (natural speed)

---

## 🧩 Edge Cases Handled

1. **Invalid Pincode** → Retry once, fallback to cached mapping.
2. **No Technician for Fault Skill** → Suggest alternative slot/day.
3. **Single Tight Availability Window** → Negotiate nearest available slot.
4. **Ambiguous Appliance Type** → Ask clarifying question.
5. **API Timeout** → Retry once, fallback gracefully.
6. **User Detail Verification:**

   * Phone must be **10 digits**.
   * Email must contain **@** and end with domain such as `.com`.
7. **Address Verification** → Ensured via API response.

---

## 🌐 API Used

* **India Post Pincode API**

```bash
https://api.postalpincode.in/pincode/{PINCODE}
```

This ensures customer addresses are mapped correctly to serviceable regions.

---

## 📂 Mock Data

Stored in **technicians.json** (included in this repo).

**Example Entry:**

```json
{
  "id": "tech_001",
  "name": "Asha Kumar",
  "skills": ["wm_vibration", "ac_leak"],
  "appliances_supported": ["WashingMachine", "AC"],
  "regions": ["Mumbai"],
  "availability_slots": [
    {"start": "2025-09-14T10:00:00+05:30", "end": "2025-09-14T12:00:00+05:30"},
    {"start": "2025-09-14T15:00:00+05:30", "end": "2025-09-14T16:00:00+05:30"}
  ]
}
```

Additional mock data uploaded:

* `FAQ.pdf`
* `example_conversations.pdf`

---

## 🏗️ Architecture Overview

**Flow:**

1. User interacts with AI agent → intent captured.
2. Agent asks appliance-specific clarifiers.
3. Customer provides details (name, phone, email, pincode).
4. Agent calls **pincode API** → region derived.
5. Technician matched based on skill + region + availability.
6. Agent proposes slots → customer confirms.
7. Appointment stored in system → summary provided.
8. If error/unavailability → escalate to human agent.

<img width="2850" height="1977" alt="consumer_durables_architecture" src="https://github.com/user-attachments/assets/717a7bf5-fd72-472a-8a52-c3c403a95357" />


---

## ✨ Project Highlights

* Built entirely on **Inya.ai** platform.
* Integrated **live API** for region verification.
* Handles **5 appliance types** with adaptive questioning.
* Includes **mock technician dataset** with skills, schedules, and regions.
* Robust **error handling & escalation mechanism**.
* Provides **natural human-like interaction** with configurable voice.

---

## 🎥 Demo Video

👉 [Insert YouTube Demo Link Here]

---

## 👨‍💻 Contributors

* **Harsh Pandit** – AI Agent Developer
* **Gaurav Kumar** – AI Agent Developer
* **Priyanshu Kumar** - Content Creater 
*  **Ritik Choudhary** - Documentation


---

⭐ Don’t forget to **star this repo** if you find it helpful!
