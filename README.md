# AWS Certified Generative AI Developer - Professional (AIP-C01) Exam Study Notes

## Useful Links
- [Official Exam Guide](https://d1.awsstatic.com/onedam/marketing-channels/website/aws/en_US/certification/approved/pdfs/docs-aip/AWS-Certified-Generative-AI-Developer-Pro_Exam-Guide.pdf)
- [Ultimate AWS Certified Generative AI Developer Professional - Udemy - Frank Kane + Stephane Maarek](https://www.udemy.com/course/ultimate-aws-certified-generative-ai-developer-professional) -- cleaner organization then AWS SkillBuilder , watch in 1.25x+ 

## Exam overview & recurring themes

- **Professional‑level, scenario‑heavy exam.**  
- **Architecture + AI.**  The exam tests your ability to design **production‑grade GenAI solutions** on AWS: not just prompts, but data ingestion, vector search, security, orchestration and cost optimisation.  Treat it as a hybrid of the AWS Solutions Architect Pro and a GenAI certification.
- **Emphasis on Amazon Bedrock and Retrieval‑Augmented Generation (RAG).**  Many questions require selecting appropriate **foundation models**, integrating **Bedrock APIs**, and designing **RAG pipelines** (ingest → chunk → embed → store → retrieve).
- **Cross‑service integration.**  Expect scenarios combining **API Gateway**, **Lambda**, **Step Functions**, **S3** and vector stores with Bedrock.  Knowing when to use **AWS Glue**, **SageMaker**, or **Kendra** for data ingestion and search is critical.
- **Security & governance.**  Questions  test **least‑privilege IAM**, **encryption (KMS)**, **VPC endpoints/PrivateLink**, **data privacy** and **AI guardrails**.  You must recognise threats like **prompt injection** and design for compliance.
- **Cost, performance & monitoring.**  Understand pricing for **Bedrock vs. SageMaker**, when to use **serverless vs. provisioned endpoints**, and how to monitor token usage, latency and model drift.
- **Decision‑point trade‑offs.**  Know when to **fine‑tune** vs. use **RAG**, when to choose **Bedrock** vs. **SageMaker**, and when to favour **HNSW** vs. **IVF** vector indices.

## General AI concepts (core definitions)

| Term | Meaning |
| --- | --- |
| **Foundation Model (FM)** | Large model pre‑trained on vast data; used as a base for downstream tasks. |
| **Fine‑Tuning** | Updating **model weights** using **labelled task‑specific data** to specialise behaviour. |
| **Continued Pre‑Training (CPT)** | Extending a model’s knowledge with **unlabelled domain data**, similar to fine‑tuning but unsupervised. |
| **Low‑Rank Adaptation (LoRA)** | Efficient tuning method adding small trainable matrices to a frozen model; the base weights remain unchanged. |
| **Retrieval‑Augmented Generation (RAG)** | Combining **information retrieval** (e.g., vector search) with generation to ground responses in external data. |
| **Embeddings** | Dense **vector representations** of data used for semantic similarity and vector search. |

## AWS AI & ML services (alphabetical)

### Language & text

- **Amazon Comprehend** – Natural language processing: **sentiment**, **entity/key‑phrase extraction**, **topic modelling** and **custom entities**; includes **Comprehend Medical** for clinical texts.
- **Amazon Kendra** – Enterprise document search with connectors; can be used as a retrieval layer in RAG architectures.
- **Amazon Lex** – Conversational interface (chatbot) service.
- **Amazon Q** – Generative AI assistant: **Q Business** (with **data connectors**, **plugins**, **Q Apps**) and **Q Developer** for code assistance.
- **Amazon Textract** – OCR plus extraction of structured data from documents.
- **Amazon Transcribe** – Speech‑to‑text with **custom vocabularies** and **custom language models**.

### Vision & multimodal

- **Amazon Rekognition** – Image/video analysis with **Custom Labels** and **Custom Moderation** for content safety.

### Safety & governance

- **Amazon Macie** – Detects and classifies **PII** and sensitive data in S3.
- **Amazon Augmented AI (A2I)** – Human‑in‑the‑loop review of machine learning predictions.  Use for high‑risk GenAI outputs.

### Other AI/ML helpers

- **SageMaker** family – See dedicated section below.
- **AWS Glue** – Data integration: **Crawlers**, **Data Catalog**, **Studio** and **Data Quality**.  For GenAI pipelines it can extract, transform and load (ETL) data before embedding.
- **AppFlow** – Managed SaaS data transfer (e.g., Salesforce to AWS).

## Amazon Bedrock

Bedrock is AWS’s managed platform for **foundation models** and GenAI tools.

### Core Bedrock services

- **Model catalog** – Access to multiple foundation models (Titan, Claude, Llama, etc.).  Understand model families and when to choose each.
- **APIs** – **Completions**, **embeddings**, **agents** and **flows**.
- **Knowledge Bases (KB)** – Fully managed **RAG pipeline**: ingestion, chunking, embedding, storage and retrieval.  Supports **OpenSearch Serverless**, **Aurora PostgreSQL (pgvector)**, **Neptune Analytics** and **S3 Vectors** as backing stores.
- **Agents** – Managed systems that call APIs/tools in response to prompts.  You define **action groups** for each tool.  Use **Bedrock Agent Tracing** and **Agent Observability** via CloudWatch for debugging.
- **Data Automation (BDA)** – Extracts structured data from unstructured sources via **blueprints**.
- **Batch Inference** – Submit multiple prompts via S3 and retrieve outputs asynchronously.
- **Cross‑Region Inference** – Distribute inference across multiple regions.
- **Intelligent Prompt Routing** – Routes requests to different models based on complexity to optimise cost and performance.
- **CountTokens API** – Returns token count for a prompt without performing inference; used for budgeting.
- **Model/Agent Evaluations** – Evaluate model quality using metrics or custom datasets.
- **Bedrock Flows** – Visual pipeline orchestration connecting FMs with data sources/tools.

### Bedrock Guardrails & safety

- **Content filters** – Block harmful content categories (includes prompt attacks, violence, hate speech etc.).
- **Sensitive‑info filters** – Detect and redact **PII** and other sensitive data.
- **Denied topics** – Block responses on policy‑defined topics.
- **Word filters** – Custom blocklists or regex patterns.
- **Contextual grounding checks** – Ensure answers are grounded in retrieved documents to reduce hallucinations.
- **Automated reasoning checks** – Enforce logical constraints or policies.
- **Tiers** – Standard tiers provide improved robustness (typo tolerance, multi‑language support).

### Agents & AgentCore

- **Bedrock Agents** – Provide orchestrated reasoning and tool invocation.  Use **action groups** to define accessible APIs.
- **Bedrock AgentCore** – Managed runtime to deploy agents at scale; works with any agent framework (including **Strands Agents**).  Includes **AgentCore Gateway** for scalable access to external APIs/tools.

### Bedrock Knowledge Base vs. custom RAG

- Use **Knowledge Bases** when you want a fully managed RAG solution with minimal code.  AWS manages ingestion, embedding and retrieval across supported vector stores.
- Build **custom RAG** when you need control over chunking, embeddings or storage.  You might integrate **OpenSearch**, **Aurora pgvector**, **Neptune**, **S3 Vectors**, **ElastiCache**, **MemoryDB**, **Pinecone** or third‑party vector stores.

## Multi‑agent systems & patterns

- **Orchestrator** – Breaks down tasks and delegates to specialised agents.
- **Router** – Routes work to appropriate specialised agents.
- **Synthesiser** – Merges outputs from multiple agents.
- **Prompt chaining** – Sequence of LLM calls with intermediate prompts; may include **gates** (conditional paths).
- **Evaluator/optimizer** – One model grades or improves another model’s output.

### Strands Agents vs. AWS Agent Squad

- **Strands Agents** – Lightweight framework for experimentation or custom logic; you manage orchestration and scaling.  Good for prototypes or local workflows.
- **AWS Agent Squad** – Managed multi‑agent orchestration for production workloads with governance and scaling.  Integrates tightly with Bedrock and AgentCore.  Use when you need secure, auditable, production‑scale agent workflows.

## Model selection & generation parameters

Choosing the right model and tuning its generation parameters are essential for high‑quality outputs and cost control.

### Model types

- **Text (regular) models** – Foundation models that take text as input and output text.  Use for tasks like conversation, summarisation, code generation and language translation.
- **Embedding models** – Produce numerical vector representations (embeddings) rather than human‑readable text.  Use when you need to store text in a vector store for semantic search or to build RAG systems.
- **Multi‑modal models** – Accept and/or generate multiple modalities (e.g. text + images).  Use for tasks like image captioning, visual question answering or cross‑modal retrieval.  Bedrock currently provides select multi‑modal models; the exam may test understanding of when to select them.

### Choosing a model

- Identify the **task** (e.g. chat, summarisation, code completion, multi‑modal understanding) and select a model family designed for that task.
- Consider **context window size**, **token limits**, **supported modalities**, **cost** and **performance**.
- For RAG, you typically use an **embedding model** to encode documents and a **text generation model** to compose answers.
- For image‑aware tasks, choose a **multi‑modal model** that can process images.  Multi‑modal models are often more expensive and may have longer inference latency.

### Temperature vs. top_p (nucleus sampling)

- **Temperature** controls the randomness of token sampling.  A value of **0** forces deterministic outputs (always picks the highest‑probability next token).  Higher values introduce more randomness, leading to more creative or diverse responses.  In practice, temperatures between **0.2** and **0.8** are common; you should lower the temperature for factual tasks and raise it for creative tasks.
- **Top p** (nucleus sampling) sets a cumulative probability threshold.  Instead of sampling from the entire vocabulary, the model considers only the smallest set of tokens whose total probability mass is **p** (e.g. 0.9).  Lower **top p** values restrict the sampling to more probable tokens and reduce randomness; higher values allow more diverse candidates.  Using **top p** in combination with temperature allows finer control over creativity: temperature affects how likely low‑probability tokens are chosen; top p limits which tokens are even considered.

## Evaluating model outputs

| Metric | Purpose | Use case |
| --- | --- | --- |
| **BLEU** | N‑gram precision metric | Machine translation |
| **ROUGE** | Recall‑focused overlap | Text summarisation |
| **BERTScore** | Embedding‑based semantic similarity | Evaluating meaning/quality of generated text |

Choose the metric based on the task: translation → **BLEU**; summarisation → **ROUGE**; semantic quality → **BERTScore**.

## Quality & Safety Gates in a Production GenAI Pipeline (Training vs Inference)

| Layer question | Applies to | What it protects against | Typical problems | Common AWS tools |
|---|---|---|---|---|
| **Is the data structurally sane?** | **Training + Inference** | Garbage input | Empty records, missing fields, unsupported values, schema drift | AWS Glue Data Quality, AWS Glue ETL, AWS Glue Data Catalog |
| **Is the data safe to use?** | **Training + Inference** | Sensitive or malformed input | PII, PHI, mixed languages, disfluent text | Amazon Comprehend (PII + language detection), AWS Lambda (normalization/masking), Amazon Transcribe (speaker labels, language ID) |
| **Is the output safe to return?** | **Inference only** | Harmful or non-compliant output | Toxic content, policy violations, leakage | Amazon Bedrock Guardrails, Bedrock content filters |
| **Is the output correct and useful?** | **Inference (primary)** | Wrong or low-quality answers | Hallucinations, irrelevance, inconsistency | Amazon Bedrock Knowledge Bases, Amazon OpenSearch (vector search), metadata filtering, prompt templates, temperature / top_p |

## PII Detection on AWS — When to Use What

| Service | Where it runs in the pipeline | Applies to | What it’s best at | Typical use cases | When it’s the WRONG choice |
|---|---|---|---|---|---|
| **Amazon Bedrock Guardrails** | At model invocation (input + output) | **Inference only** | Preventing PII from reaching or leaving the model | Redact/mask PII in prompts and responses; enforce privacy with minimal code; ensure PII is never returned | Cleaning historical data; batch processing S3 objects; non-GenAI workloads |
| **Amazon Comprehend** | Before the model (data preprocessing) | **Training + Inference** | Detecting and transforming PII in raw text | Redact PII in transcripts or documents; normalize text before RAG; language detection + entity extraction | Real-time GenAI enforcement; output filtering; zero-code pipelines |
| **Amazon Macie** | After storage (S3 scanning) | **Training data / at rest** | Discovering sensitive data at rest | Find where PII exists in S3; compliance audits; security posture visibility | Preventing storage of PII; redaction or transformation; inline application flows |

## Amazon SageMaker family

- **Data Wrangler** – Visual data preparation.
- **JumpStart** – Pre‑built models and algorithms; one‑click deployment.
- **Feature Store** – Centralised feature repository.
- **Ground Truth / Ground Truth Plus** – Data labelling (Plus = fully managed).
- **Model Monitor** – Detects data drift and bias.
- **Clarify** – Explains model predictions and detects bias.
- **Model Registry** – Stores and versions models for deployment.
- **ML Lineage Tracking** – Tracks datasets, code and models across experiments.
- **Neo** – Train once, deploy anywhere (edge devices).
- **Unified Studio** – End‑to‑end ML IDE.
- **Pipelines** – Declarative ML workflow orchestration.
- **MLflow on SageMaker** – Experiment tracking integration.

*Note:*  Use **SageMaker** when you need full control over training or hosting models in a VPC.  Use **Bedrock** when you want managed foundation models and serverless inference.  In the exam, be ready to choose between these options based on requirements such as control vs. convenience, data privacy, cost and supported frameworks.

## AWS Glue (data prep)

- **Crawlers** – Discover and infer schema from data sources.
- **Data Catalog** – Central metadata store for tables and partitions.
- **Glue Studio** – Visual ETL development environment.
- **Data Quality** – Rule‑based quality checks and profiling.

Glue can appear in scenarios for **ETL** preceding embeddings or fine‑tuning.

## AI data stores & vector databases

### OpenSearch

- **Search & analytics engine** (not OLTP) with vector capabilities.
- **Vector search** types:
  - **Exact nearest neighbour (NN)** – High precision, slower.
  - **Approximate NN (ANN)** – Trade recall for speed.  Two key algorithms:
    - **HNSW (Hierarchical Navigable Small World)** – High recall and low latency; uses more RAM.  Good for low‑latency, high‑quality search.
    - **IVF (Inverted File)** – Good for very large datasets; allows recall‑speed tuning.
- **Neural plugin** – Built‑in embedding and search pipelines (simplifies RAG).

*When to use:*  Choose **HNSW** for performance‑critical queries; choose **IVF** for extremely large datasets or when memory savings are important.

### S3 Vectors

- Lowest‑cost vector store; managed via S3.  Suitable for large, cold datasets.  AWS often recommends combining **S3 Vectors** for bulk storage with **OpenSearch** for hot, low‑latency queries.

### Aurora pgvector

- Amazon Aurora (PostgreSQL) supports the **pgvector** extension.  Use for small/medium datasets when you need SQL capabilities alongside vector similarity search.  Supports **HNSW** and **IVF** indices.

### ElastiCache & MemoryDB

- **ElastiCache (Redis/Valkey)** – Provides in‑memory vector search for ultra‑low‑latency queries.
- **MemoryDB** – Durable, in‑memory vector store; fully managed and designed for high‑throughput workloads.

### DynamoDB

- Not used for vectors but valuable for storing **session state**, **metadata** and **conversation memory**.

### Pinecone

- **Pinecone** – Managed, serverless vector database that automatically scales and offers simple APIs.  It integrates with AWS services and Bedrock Knowledge Bases as an external vector store option.  Use **Pinecone** when you need hassle‑free setup, auto‑scaling and multi‑cloud portability; choose AWS‑native stores for tighter integration, lower latency within AWS and potentially lower cost.

### Vector store selection summary

- **OpenSearch** – Best general‑purpose engine for high‑performance RAG.
- **S3 Vectors** – Cheapest storage for large collections.
- **Aurora pgvector** – SQL + vectors for moderate datasets.
- **ElastiCache / MemoryDB** – Ultra‑fast, in‑memory search.
- **Pinecone** – Managed, serverless and auto‑scaling; good for ease of use and cross‑cloud portability.

## Orchestration & workflows

- **AWS Step Functions** – Orchestrates stateful workflows.  Often used to chain data ingestion, embedding, calling FMs, and storing outputs.
- **Lambda** – Event‑driven compute; used for chunking text, generating embeddings or gluing services together.
- **API Gateway** – Exposes a REST/HTTP interface for your GenAI application.
- **EventBridge** – Bus for event‑driven architectures.
- **AppConfig** – For runtime **feature flags** and dynamic model selection; can be used to switch FMs based on criteria.

## Security & governance patterns

- **Threats:**  **Prompt injection**, **data exfiltration**, **tool misuse**.  Always sanitise user inputs, restrict tool access and implement guardrails.
- **Least privilege:**  Use fine‑grained **IAM policies**, role assumption and scoped credentials.  For multi‑tenant systems, isolate per‑tenant data sources and encryption keys.
- **Encryption:**  Use **KMS** for data at rest; enforce **TLS** in transit; store embeddings in encrypted buckets or databases.
- **Network isolation:**  Use **VPC endpoints/PrivateLink** to call Bedrock or SageMaker privately; configure security groups and subnets.
- **Auditability:**  Log prompts, responses and tool invocations via **CloudWatch** and **AWS CloudTrail**.
- **Guardrails & A2I:**  For high‑risk tasks, implement content filters and send outputs for **human review**.

## Designing RAG pipelines

1. **Ingest & chunk:**  Use **Glue**, **Lambda**, or custom scripts to extract data from documents, chunk text (size/overlap matters for recall), and pre‑process.
2. **Generate embeddings:**  Use Bedrock **embedding APIs** or frameworks like SentenceTransformers; decide on vector dimension.
3. **Store embeddings:**  Choose a vector store (OpenSearch, S3 Vectors, Aurora, Pinecone, etc.) based on dataset size and latency requirements.
4. **Retrieve relevant chunks:**  Perform **vector search** (may combine with keyword search for hybrid retrieval).
5. **Ground responses:**  Provide retrieved context to the FM with instructions to use only that information; enforce via guardrails and grounding checks.
6. **Evaluate & refine:**  Use evaluation datasets, human feedback and metrics (BERTScore, ROUGE) to iterate and catch hallucinations.

## Multi‑tenant GenAI considerations

- **Tenant isolation:**  Separate data sources, embeddings and encryption keys per tenant; filter queries by tenant ID.
- **Per‑tenant access control:**  Enforce IAM and RBAC at retrieval and tool layers.
- **No cross‑tenant training:**  Do not mix tenant data in fine‑tuning unless explicit permission.
- **Observability:**  Monitor usage and errors by tenant; alert on anomalies.

## General Tips

### Data, Governance, and Auditability
- **Custom domain rule checking** → AWS Lambda  
- **Auditable access** → CloudTrail + IAM (not custom application logs)  
- **Tracking S3 data sources and lineage** → AWS Glue Data Catalog  
- **Regulated industries** → Glue Data Catalog, CloudTrail, metadata tags, IAM-based access control  
- **Data cleaning and PII masking before LLMs** → Lambda + Comprehend (**not Guardrails**)  

### Networking and Security
- **Secure private service access** → VPC endpoints / PrivateLink  
- **On-prem execution** → AWS Outposts  
- **5G / edge workloads** → AWS Wavelength  

### RAG Quality, Explainability, and Caching
- **RAG explainability** → propagate metadata into embeddings  
- **Reduce hallucinations** → RetrieveAndGenerate with Bedrock Knowledge Bases  
- **Retrieve-only RAG evaluation** → measure retrieval quality independent of generation  
- **Hierarchical chunking** → small child chunks for search, return larger parent chunks for context  
- Use hierarchical chunking when documents are **sectioned** and answers need surrounding context  

### Performance and Cost Optimization
- **Massive datasets + throttling + idle compute** → use Bedrock Batch Inference (not InvokeModel)  
- **Static or repeated prompt content** → Bedrock prompt caching  
- **Identical public requests** → CloudFront edge cache  
- **Similar but not identical requests** → semantic cache  

### Streaming and Real-Time Use Cases
- **Real-time token streaming + serverless** → API Gateway WebSocket + Lambda  
- **High-volume real-time ingestion** → Kinesis Data Streams  
- **Near-real-time delivery** → Kinesis Firehose  

### Agents and Tooling
- **Agents should not speak REST**  
- If agents call strict or mutable external APIs → place an **MCP tool boundary** in front  
- Validate arguments **before** calling the external API  
- **MCP standardizes interfaces, not compute** → deploy each MCP server on compute that matches workload  

### Data Movement
- **Large data transfers (on-prem ↔ AWS or AWS ↔ AWS)** → AWS DataSync  

## Exam‑specific advice & pitfalls

- **Hands‑on is invaluable.**  Deploy sample Bedrock models, perform embeddings and RAG, configure Step Functions and Lambda; the exam expects practical understanding.
- **Review core AWS services.**  Do not neglect **API Gateway**, **S3**, **Step Functions**, **Lambda**, **IAM** and **KMS**; they appear in many questions.
- **Expect long scenario questions.**  Many questions have long preambles; read the final requirement first to focus on relevant details.  Some early test‑takers noted grammar issues and ambiguous answers – choose the most secure and cost‑effective option.
- **Be ready for trade‑off questions.**  For example: fine‑tune vs. RAG; Bedrock vs. SageMaker; serverless vs. always‑on; HNSW vs. IVF.
- **Security & governance matter.**  Evaluate whether the solution uses **least‑privilege IAM**, **encryption**, **data privacy controls**, and **guardrails**.
- **Cost and performance.**  Understand pricing models for Bedrock (pay per inference), SageMaker (instance‑based), Step Functions, etc.  Consider caching and batching to reduce cost.
- **Time management.**  There are roughly 75–85 questions in about 3–3.5 hours; practise pacing and mark tricky questions to revisit later.
- **Treat it as an architecture exam.**  Build mental patterns for end‑to‑end GenAI systems on AWS; rely on well‑architected principles.
