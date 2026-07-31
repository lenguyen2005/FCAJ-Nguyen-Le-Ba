---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

While researching **RAG (Retrieval-Augmented Generation)** architectures on AWS, I encountered a common enterprise challenge: **How do we build an AI Q&A application serving multiple departments while ensuring employees can only access documents belonging to their own department?**

The simplest approach might be creating a separate Amazon Bedrock Knowledge Base for each department. However, this increases infrastructure costs and operational complexity. Therefore, I explored an alternative solution: **combining Amazon Bedrock Knowledge Bases and Amazon Verified Permissions to enforce document access control on a single shared Knowledge Base.**

---

## 1. Implementation Overview

To better understand how the system works, this architecture applies the **Defense in Depth** principle with two independent verification layers:

### A. Data Ingestion Pipeline
* **Step 1:** Upload PDF files to Amazon S3 organized by department folder structure (e.g., `docs/dept-a/report.pdf`).
* **Step 2:** Amazon EventBridge triggers SQS and AWS Lambda to automatically generate a tagged `report.pdf.metadata.json` sidecar file containing department metadata.
* **Step 3:** Schedule an Ingestion Job for Bedrock Knowledge Base to read the files alongside sidecar metadata and index them into the Vector Database.

### B. Query Pipeline
* **Step 4:** The user submits a question via Amazon API Gateway. A Lambda Authorizer calls Amazon Verified Permissions to verify if the user is authorized to call the API (Layer 1).
* **Step 5:** If Layer 1 passes, the Middleware Lambda calls Verified Permissions to check which departments' documents this user is allowed to read.
* **Step 6:** The Middleware Lambda constructs a `kb_filter` (Metadata Filter) and passes it into Bedrock's `RetrieveAndGenerate` API.
* **Step 7:** Bedrock searches and retrieves only text chunks matching the filter for the LLM to generate an answer.
* **Step 8:** Apply **Guardrails for Amazon Bedrock** to verify response reliability before returning it to the user (Layer 2).

---

## 2. Key Highlights & Benefits

* **Cost & Infrastructure Optimization:** Consolidate all documents into a single Knowledge Base instead of maintaining dozens of separate instances.
* **Decoupled Policy Management:** All authorization rules are written in Cedar language within Amazon Verified Permissions. Security teams can manage policies independently without modifying backend application code.
* **Two-Layer Security:** Layer 1 blocks unauthorized API requests, while Layer 2 filters data at vector search time. Even if Layer 1 is misconfigured, Layer 2 ensures the LLM never "sees" forbidden documents.
* **Instant Updates:** Policy changes in Verified Permissions take effect immediately on subsequent requests without requiring service restarts.

---

## 3. Conclusion

This advanced multi-tenant RAG architecture is a great reference pattern for building enterprise AI applications. Decoupling authorization logic from the RAG application not only keeps code clean but also delivers robust enterprise-grade data security.

---

## 4. References

* [AWS Machine Learning Blog – Secure multi-tenant RAG with Amazon Bedrock and Verified Permissions](https://aws.amazon.com/blogs/machine-learning/secure-multi-tenant-rag-with-amazon-bedrock-and-verified-permissions/)
* [Amazon Bedrock Knowledge Bases Documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)
* [Amazon Verified Permissions Developer Guide](https://docs.aws.amazon.com/verifiedpermissions/latest/userguide/what-is-avp.html)

---

<!-- ### 🔗 Published Post on Facebook Group
👉 **Post Link:** [AWS Study Group FB - Secure Multi-Tenant RAG Post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2224218628343097/) -->