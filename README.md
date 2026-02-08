# Support Ticket Classifier Workflow (n8n)

This n8n workflow automatically handles incoming support emails, classifies them using AI, and routes notifications based on ticket urgency. It is designed as a real-world automation workflow suitable for IT, customer support, or operations automation roles.

---

## **Workflow Overview**

1. **Gmail Trigger**  
   - Watches your inbox for new emails.  
   - Extracts sender, subject, and email snippet as `ticket_text`.  

2. **Set Node (Initial Data)**  
   - Stores `ticket_text`, `TicketID`, and `Sender` for later use.  

3. **OpenAI Node (Ticket Classification)**  
   - Classifies tickets into **Category**: Billing, Technical, Account, Complaint, General  
   - Determines **Urgency**: High, Medium, Low  
   - Rules for classification:
     - **High** urgency: mentions `refund`, `charged twice`, `cannot login`, `service down`, or angry tone  
     - **Medium** urgency: minor payment issues, errors, or requests affecting work slightly  
     - **Low** urgency: general inquiries with no impact  

4. **Set Node (Parse AI Output)**  
   - Converts OpenAI JSON text output into actual workflow variables:
     - `category`, `urgency`, `TicketID`, `Sender`, `ticket_text`  

5. **IF / Switch Node (Routing)**  
   - Routes tickets based on urgency or category:  
     - **High → Telegram notification**  
     - **Medium → Gmail notification**  
     - **Low → Optional Gmail confirmation**  

6. **Gmail / Telegram Node (Notification)**  
   - Sends automated messages to the ticket sender including:  
     - Ticket ID  
     - Category  
     - Urgency  
     - Ticket snippet  

---

## **Sample Email Inputs and Expected Output**

| Sample Email | Category | Urgency |
|-------------|----------|---------|
| "I cannot login to my account, please help immediately." | Account | High |
| "My payment hasn’t been reflected yet." | Billing | Medium |
| "Can I receive my invoices as PDF instead of Excel?" | Billing | Low |

---

## **Import Instructions**

1. Download `support-ticket-workflow.json` from this repository.  
2. Open **n8n** → **Workflows → Import Workflow**.  
3. Select the JSON file.  
4. Update API credentials:
   - Gmail (for triggers and sending emails)  
   - OpenAI (for AI classification)  
   - Telegram (optional, for high-priority notifications)  

5. Activate the workflow if you want it to run automatically.

---

## **Notes**

- All variables are stored as `$json` fields (`category`, `urgency`, `TicketID`, `Sender`, `ticket_text`)  
- Designed to be modular — you can expand routing or add new notification channels.  
- Tested with realistic high, medium, and low-priority emails.  

---

*This workflow demonstrates practical n8n automation skills including email triggers, AI integration, conditional logic, and notifications — suitable for portfolio or professional showcase.*
