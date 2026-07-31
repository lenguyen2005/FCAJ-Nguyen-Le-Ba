---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Building a Large-Scale Self-Managed RAG System with Amazon EKS and Amazon S3 Vectors

While exploring RAG (Retrieval-Augmented Generation) architectures on AWS, I noticed that many enterprises use fully-managed services like Amazon Bedrock Knowledge Bases for rapid deployment. This service is very convenient and reduces the initial infrastructure operational load. However, when the problem dives deeper into practical application and requires in-depth model customization, optimizing massive vector storage costs, or running on an existing Kubernetes (EKS) infrastructure, managed solutions sometimes start to reveal limitations in both flexibility and cost.

Therefore, I investigated an alternative solution: Building a Self-managed RAG system on Amazon EKS integrated with Amazon S3 Vectors and prominent Open Source tools such as Ray, Hugging Face TGI, and LangChain.

---

## 1. Architecture Overview and Implementation Steps

This architecture is designed to cleanly split into 2 main layers: The Knowledge Processing Layer handles the large-scale batch digitization of data, and the Response Generation Layer processes real-time user queries.

### A. Data Processing and Digitization Flow
In this layer, the system focuses on converting raw data from the storage repository into quickly queryable vector spaces:
* **Step 1:** Upload all raw documents (JSON files, PDFs, internal documents) to a General Purpose S3 Bucket acting as the central Data Lake.
* **Step 2:** Trigger a batch job running Ray Data on Amazon EKS. Ray will proceed to read the data, split documents into smaller chunks (chunking), and perform high-performance distributed computing on the GPU/CPU nodes of the Kubernetes cluster.
* **Step 3:** Utilize open-source embedding models (like `multilingual-e5-small` or `Qwen3-Embedding`) to transform text chunks into multi-dimensional Vector Embeddings.
* **Step 4:** Write all resulting vectors along with their corresponding metadata into an Amazon S3 Vectors Bucket – a specialized vector storage repository that enables high-speed similarity queries without needing to manage or maintain any underlying database infrastructure.

### B. Querying and Response Generation Flow
This layer serves as the direct interaction point with end-users via a chat interface:
* **Step 5:** The user submits a question from the Chat interface (Streamlit) deployed as a container service on the EKS cluster.
* **Step 6:** Use LangChain to convert the user's question into vector format (using the same embedding model used in the ingestion phase), then perform a vector similarity search directly in the S3 Vectors Bucket to retrieve the top-K most relevant texts.
* **Step 7:** LangChain packages the initial question together with the context just found into a contextualized prompt, sending it to the Inference Server (running Hugging Face TGI on EKS with GPUs).
* **Step 8:** The LLM model (such as Mistral-7B, Llama-3, or Qwen) generates an accurate answer based on enterprise data and returns it to the user interface along with reference sources (attribution).

---

## 2. A Few Highlights Found Useful

* **Outstanding Cost Optimization with S3 Vectors:** This is a vector storage feature integrated directly into S3. There is no need to spin up or maintain expensive Vector Database clusters (like OpenSearch or Pinecone) running 24/7. S3 Vectors automatically scales from 0 to tens of millions of vectors using a pay-per-use model, helping to save substantial budget for projects hoarding massive amounts of data.
* **Powerful Scalability Thanks to Ray:** Instead of processing single files individually as costly events, Ray smoothly distributes the massive data vectorization workload across the EKS cluster. Ray supports automatic fault tolerance, good memory management, and intelligent GPU/CPU resource allocation for both batch processing and future streaming data.
* **Full Control:** Using Hugging Face TGI (or vLLM) to serve LLMs allows proactively choosing any open-source model according to the problem, freely customizing crucial technical parameters (quantization, tensor parallelism, PagedAttention), or even easily deploying in Regions not yet supported by fully-managed AWS AI services.

---

## 3. Conclusion

The Self-Managed RAG architecture combining Amazon EKS + S3 Vectors is an extremely powerful solution for teams already experienced in working with Kubernetes and Open Source AI. It removes the barrier of large-scale vector storage costs while retaining complete control over infrastructure, models, and data processing flows for the enterprise.

---

## 4. References

* [AWS Storage Blog – Building self-managed RAG applications with Amazon EKS and Amazon S3 Vectors](https://aws.amazon.com/blogs/storage/building-self-managed-rag-applications-with-amazon-eks-and-amazon-s3-vectors/)
* [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/)
* [Ray on Kubernetes (KubeRay)](https://docs.ray.io/en/latest/cluster/kubernetes/index.html)