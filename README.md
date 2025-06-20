# RAG on Azure foundry data vector index
![1image](https://github.com/user-attachments/assets/c911db84-3ee7-4c64-963f-d9d8fcbe986f)

### Innovation Challenge June 2025
## Team Members
🚀 Lidia Del Carmen Benitez
🚀 Monica Orellana
🚀 Enrique Aguilar Martinez.

# Advanced RAG Pipeline in Azure for Unstructured Data

![2image](https://github.com/user-attachments/assets/2470310d-ffe2-436f-a102-a8cdf176a372)

This project implements a **Retrieval-Augmented Generation (RAG)** solution on the **Azure platform**. The main goal is to tackle the challenge of handling unstructured data efficiently, overcoming the limitations of naive chunking techniques and drastically improving the consistency and quality of generative AI model responses.

---

## 🧩 Problem Summary

Organizations often get **inconsistent results** from their AI models when processing documents. A common mistake is **basic chunking** (e.g., splitting a PDF by pages), which ignores semantic context and structure, leading to poor-quality answers.

---

## ✅ Our Solution

We implemented a **semantic and intelligent data pipeline**. Instead of arbitrary splits, we divide documents into **logical, coherent chunks**, enrich them with metadata, and index them for precise retrieval—all orchestrated using Azure AI services.

---

## 🏛️ Solution Architecture

The solution is divided into two main flows:

- **Data Pipeline Flow** (for ingestion and processing)
- **Application Flow** (for user interaction)

### 📥 Data Pipeline Flow (Document Processing)

This flow prepares documents so AI can effectively use them.

```
[Original Document]
   └──> [1. Semantic Chunking]
           └──> [2. Metadata Enrichment]
                   └──> [3. Embedding Creation]
                           └──> [4. Persistence in Azure AI Search]
```

- **Document Chunking:** The document is divided into logically relevant parts, each containing a single idea or concept.
- **Enriching Chunks:** Valuable metadata (title, summary, keywords) is added to each chunk.
- **Embedding Chunks:** Each chunk and its metadata are converted into a numerical vector using an embedding model.
- **Persisting Chunks:** Processed chunks and their vectors are stored in an Azure AI Search index.

---

### 💬 Application Flow (Answering Questions)

This is the real-time flow when a user makes a query.

```
[User]
   └──> [App]
         └──> [Orchestrator (Azure AI)]
               └──> [Azure AI Search]
                     └──> [LLM Model (GPT-4o)]
                           └──> [Smart Response]
```

- **User Query:** The user asks a question.
- **Orchestration:** The orchestrator (e.g., LangChain, Semantic Kernel) receives the query.
- **Relevant Search:** A hybrid search (vector + text) is performed in Azure AI Search to find the most relevant chunks.
- **Augmented Generation:** Retrieved chunks are bundled with the original query and sent as context to the GPT-4o model.
- **Final Response:** The model generates an accurate, contextualized response that is returned to the user.

---

## ✨ Key Benefits

- **Consistent & Accurate Results:** Feeding the model with semantically relevant chunks drastically reduces hallucinations and improves response quality.
- **Deep Context Understanding:** AI doesn’t just read text—it understands the document's underlying structure.
- **Optimized Efficiency:** Fewer, more relevant tokens are sent to the model, reducing both costs and latency.
- **Enterprise-Grade Scalability & Security:** Built on Azure’s robust infrastructure—ready to scale and meet corporate security standards.

---

## 🛠️ Tech Stack & Prerequisites

To replicate this solution, you’ll need:

- An **Azure subscription**
- **Azure CLI** installed and configured
- The following Azure services:

  - **Azure AI Studio**: Centralized project and model management
  - **Azure AI Search**: For high-speed indexing and retrieval
  - **Azure OpenAI Service**: For accessing GPT-4o and embedding models

---

## 🚀 Implementation Guide

### 1. Create a Resource Group

Logical container for all project resources.

```bash
# Replace <resource-group-name> and <location> with your values
az group create --name <resource-group-name> --location <location>
```

---

### 2. Deploy Azure AI Search

Service to index your data fragments.

```bash
# Replace the placeholder values
az search service create --name <search-service-name>   --resource-group <resource-group-name> --sku standard
```

**Get Admin Key**:

```bash
az search admin-key show --service-name <search-service-name>   --resource-group <resource-group-name> --query primaryKey
```

---

### 3. Deploy Azure AI Services

This will give access to OpenAI models.

```bash
az cognitiveservices account create --name <ai-services-name>   --resource-group <resource-group-name> --kind OpenAI   --sku s0 --location <location>
```

**Get Endpoint & Keys**:

```bash
az cognitiveservices account show --name <ai-services-name>   --resource-group <resource-group-name> --query "properties.endpoint"

az cognitiveservices account keys list --name <ai-services-name>   --resource-group <resource-group-name> --query "key1"
```

---

### 4. Deploy Models in Azure AI Studio
![3image](https://github.com/user-attachments/assets/4ceee543-4d20-47fe-b1aa-ff118499cc5c)

- Go to Azure AI Studio
- Create a new **Hub** and **Project**
- In your project, navigate to **Deployments**
- Deploy:
  - A **chat model** (e.g., `gpt-4o`)
  - An **embedding model** (e.g., `text-embedding-ada-002`)

---

### 5. Connect Resources
![4image](https://github.com/user-attachments/assets/007e761d-7099-48c6-9da5-3596b5b40f54)

To enable communication between Azure components, create a connection in AI Studio.

**Create `connection.yml`:**

```yaml
# connection.yml
name: azure_openai_connection
type: azure_openai
target: <your-azure-openai-endpoint>
api_key: <your-azure-openai-key>
api_version: "2024-02-01"
```

**Deploy the connection via CLI:**

```bash
az ml connection create --file connection.yml   --resource-group $rgName --workspace-name hub-demo
```

---

### 6. Confirm Resources

Check in the Azure portal and AI Studio that all services and connections were successfully deployed. **Your RAG backend is now ready!**

---

## 🔮 Next Steps & Improvements

This project provides a solid foundation. You can further enhance the solution with:

- **Rigorous Evaluation:** Implement a framework to measure retrieval and generation quality using metrics like `context_precision` and `faithfulness`.
- **Advanced Chunking Strategies:** Test different chunk sizes, overlapping techniques, or LLM-based splitters.
- **Search Index Optimization:** Customize search profiles and compare hybrid vs. pure vector search.
- **Metadata Enrichment with AI Agents:** Use autonomous agents to analyze documents and generate richer metadata automatically.

## Thanks to Microsoft and Chaitra Vedullapalli's Women in Cloud Digital Network for the opportunity to participate in this great event.
