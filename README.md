*Intuz — Your automation partner, one workflow at a time.*

<p align="center">
  <picture>
    <img alt="Banner Image" src="https://github.com/user-attachments/assets/210f97fc-0fce-404a-b647-7dfe1302cd37" />
  </picture>
</p>

[Intuz](https://www.intuz.com) helps organizations orchestrate AI, automation, and enterprise systems through scalable workflows. Our repository showcases proven implementations across healthcare, operations, customer support, document processing, sales, and back-office functions, enabling teams to accelerate automation initiatives without starting from scratch.

[N8N Creator](https://n8n.io/creators/intuz/) · [AI Agent Development Company](https://www.intuz.com/ai-agents-for-business-automation/) · [n8n Workflow Automation](https://www.intuz.com/workflow-automation-services/) · [For Custom Workflow Automation](https://www.intuz.com/get-started/)

# Answer internal policy questions from Google Drive with OpenAI and Qdrant

## Quick overview

This workflow keeps a Qdrant knowledge base synced with Google Drive documents and lets users ask questions via n8n Chat or Slack, using OpenAI for embeddings and responses with source citations.

## How it works

1. Triggers every minute when a file is created or updated in a specific Google Drive folder and downloads the file.
2. Detects whether the file is a PDF, Google Doc, DOCX, or TXT and extracts plain text (DOCX files are copied to a temporary Google Doc, exported as text, then deleted).
3. Deletes any existing vectors in the Qdrant collection that match the same Google Drive file ID to prevent outdated or duplicate content.
4. Splits the extracted text into chunks, attaches metadata (file ID, name, Drive URL, last modified time), creates OpenAI embeddings, and inserts the chunks into the Qdrant collection.
5. Triggers when a message is received from the public n8n Chat endpoint or when the Slack app is mentioned/receives a message, then normalizes both inputs into a single format and ignores bot-generated Slack events.
6. Uses an OpenAI chat model with conversation memory and a Qdrant “company_knowledge_base” retrieval tool to draft an answer based only on retrieved document content and includes formatted source citations.
7. If the question came from Slack, posts the formatted response back to the same Slack channel/thread.

## Setup

1. Add credentials for Google Drive OAuth2, OpenAI, and Qdrant (REST API for delete-by-filter and Qdrant API for vector store insert/retrieval).
2. Create a Qdrant collection named `company_docs` and create a payload index on `metadata.file_id` (type `keyword`) so delete-by-filter works.
3. Set the Google Drive folder ID to watch (and ensure the account has access) and confirm your documents are supported (PDF, Google Docs, TXT, and DOCX via the copy/export flow).
4. If using Slack, connect Slack OAuth credentials, configure a Slack app with `app_mention` and `message.im` (and/or message events) subscriptions, reinstall the app to the workspace, and set the target channel in the Slack trigger.
5. Example Slack Message: @IntuzBot What is the notice-period policy? IntuzBot answers general greetings and identity questions directly, searches the knowledge base for company-related questions, and gives a friendly fallback when the requested information is not available.

## Requirements

- n8n with LangChain nodes enabled.
- Google Drive account with access to the source folder.
- OpenAI API key for chat responses and embeddings.
- Qdrant Cloud cluster and API key.
- Qdrant collection named company_docs, or update the collection name in both workflows.
- A Google Drive folder containing supported documents.
- Slack credentials and a configured Slack app if Slack responses are required.
- A Qdrant payload index on the file ID field for delete-before-insert synchronization.
- Supported document extraction or conversion for PDF, TXT, Google Docs, and DOCX files.

## FAQ

**What types of documents does this workflow support?**  
It supports PDF, Google Docs, DOCX (via copy/export flow), and TXT files.

**How does the workflow keep the Qdrant knowledge base updated?**  
It triggers every minute on file creation or update in a Google Drive folder, deletes outdated vectors by file ID, and inserts new embeddings for the updated content.

**How are user questions answered in Slack or n8n Chat?**  
User questions trigger the workflow which uses OpenAI chat models with Qdrant retrieval to draft answers based on document content, then posts responses back to Slack or n8n Chat with source citations.

## Related n8n templates from Intuz

- [demand forecasting](https://github.com/Intuz-production/demand-forecasting)
- [ev battery usage behavior](https://github.com/Intuz-production/ev-battery-usage-behavior)
- [Firebase Phone Verification for native iOS application](https://github.com/Intuz-production/Firebase-Phone-Verification-iOS)

[See all of Intuz's free n8n templates](https://www.intuz.com/n8n-workflow-automation-templates/)

## Connect with us

Intuz is a USA-based AI & workflow automation company with 16+ years of experience building custom AI-enabled workflow automations for SMBs and Enterprises, specializing in agentic AI, LLM integrations, and CRM/ERP sync across Healthcare, FinTech, eCommerce, Manufacturing, and Real Estate.

* **Website:** [https://www.intuz.com](https://www.intuz.com)
* **Email:** [getstarted@intuz.com](mailto:getstarted@intuz.com)
* **LinkedIn:** https://www.linkedin.com/company/intuz/
* **Get Started:** https://n8n.partnerlinks.io/intuz

## For Custom Workflow Automation

[Click here - Get Started](https://www.intuz.com/get-started/)
