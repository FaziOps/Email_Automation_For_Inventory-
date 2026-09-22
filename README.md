# AI Customer Support Email Assistant (n8n Workflow)

## Overview
This n8n workflow acts as an automated, AI-powered customer support email assistant. It monitors a Gmail inbox for new messages, analyzes the sender's intent, categorizes the email, and uses Google Gemini to automatically generate and save a highly professional draft reply directly in Gmail. 

## How It Works
1. **Gmail Trigger**: Polls your connected Gmail account every minute for new emails.
2. **AI Text Classification**: Uses Google Gemini to analyze the incoming email snippet and classify it into one of two categories:
   * **Order**: Questions about placing orders, quotations, modifications, or cancellations.
   * **Enquiry**: General questions about products, services, pricing, or technical support.
3. **AI Reply Generation**: Sends the email content to the **Gemini 2.5 Flash** model with a strict prompt to act as an expert copywriter. It generates a complete, polite, and contextually accurate email response (including a subject line) tailored to the classified category.
4. **Create Gmail Draft**: Takes the AI-generated response and automatically creates a draft in your Gmail account, ready for your final review and sending.

## Prerequisites
To use this workflow, you will need the following credentials configured in your n8n instance:
* **Gmail OAuth2 Credentials**: To allow n8n to read incoming emails and create drafts.
* **Google Gemini (PaLM) API Key**: To power the AI classification and text generation nodes.

## Workflow Nodes Breakdown
* `Gmail Trigger` (`n8n-nodes-base.gmailTrigger`): Fetches new emails.
* `Text Classifier` (`@n8n/n8n-nodes-langchain.textClassifier`): Determines if the email is an "Order" or an "Enquiry".
* `Google Gemini Chat Model` (`@n8n/n8n-nodes-langchain.lmChatGoogleGemini`): Provides the language model backend for the text classifier.
* `Message a model` (`@n8n/n8n-nodes-langchain.googleGemini`): The primary LLM node (using `gemini-2.5-flash`) that generates the actual email body and subject based on an extensive copywriting prompt.
* `Create a draft` (`n8n-nodes-base.gmail`): Creates the email draft using the output from the Gemini model.

## Installation & Setup
1. Open your n8n instance.
2. Click on **Add Workflow** in the top right corner.
3. Select **Import from File** or simply copy the JSON code and paste it directly into the n8n workflow canvas.
4. Connect your **Gmail account** in the `Gmail Trigger` and `Create a draft` nodes.
5. Connect your **Google Gemini API** credentials in the `Google Gemini Chat Model` and `Message a model` nodes.
6. Click **Test Workflow** to ensure everything is working correctly.
7. Toggle the workflow to **Active** to start automating your email drafts!

## Customization
You can easily customize the behavior of this assistant:
* **Modify Categories:** Open the `Text Classifier` node to add more categories (e.g., "Refunds", "Complaints") and update the descriptions.
* **Tweak the AI Persona:** Open the `Message a model` node and edit the System Prompt. You can change the tone (e.g., make it more casual or more formal) or provide specific company details (links, policies) for the AI to include in its replies.
* **Change Polling Interval:** By default, the `Gmail Trigger` checks for new emails every minute. You can adjust this in the trigger settings to save resources if needed.

## Notes
* The AI is strictly instructed to output ready-to-send text without conversational filler (like "Here is your email:"). 
* Since the final output is saved as a **Draft**, you maintain complete control over what gets sent to your customers.

