# Intelligent Gmail Orchestrator (Beta v0.1.0)

An automated asynchronous message-processing pipeline deployed via n8n. This system integrates upstream Gmail events with a locally hosted Large Language Model (Ollama) to execute zero-shot text classification, utilizing conditional logic and dynamic handling to manage contextual system labels without execution faults.

---

## Project Status: Beta (Active Iteration)
This project is currently in **Beta (v0.1.0)**. The core routing engine and LLM integration are fully implemented. Current engineering efforts are focused on:
* Monitoring classification accuracy in a live environment.
* Hardening the fault-tolerant label assignment loops.
* Refining the LLM prompt context for edge-case emails.

---

## System Architecture & Mechanics
The pipeline functions as a self-correcting event-driven system:
1. **Ingress:** A `Gmail Trigger` listens for incoming messages and hands off the payload.
2. **Contextual Analysis:** The payload text is processed by a local LLM (`Ollama` node) using structured zero-shot prompting to determine the target category.
3. **Router (If Node):** Evaluates the LLM response to split traffic into dedicated logic branches.
4. **Idempotent Execution Loops:** To prevent API execution errors if a required Gmail label does not exist yet, the workflow conditionally removes conflicting labels, dynamically initializes the new label via a creation node, and safely attaches it to the message thread.

---

## Deployment / Installation Guide

To import and inspect this architecture locally within your own n8n instance, follow these steps:

### Prerequisites
* **n8n** (Self-hosted or Cloud instance)
* **Ollama** running locally (or access to a compatible LLM provider)
* **Gmail OAuth2 Credentials** configured in your n8n environment

### Import Instructions
1. Download the `intelligent-gmail-orchestrator.json` file from this repository.
2. Open your n8n dashboard and create a new, empty workflow.
3. Click the **Canvas Menu** (three dots in the top right corner) and select **Import from File**.
4. Select the downloaded `.json` file.
5. Link your personal Gmail OAuth2 credentials and your Ollama API endpoint to the respective nodes.
