---
title: "Blog 3"
date: 2026-06-28
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Secure Multi-Tenant RAG with Amazon Bedrock and Verified Permissions

Large organizations building internal Generative AI applications often face a recurring challenge: controlling document access for each team or department without duplicating infrastructure for every group. In a multi-tenant environment (or even a single tenant with distinct departments), employees of a department should only access that department's documents, whereas executives require broader cross-departmental access.

Retrieval-Augmented Generation (RAG) is a solution that balances cost and performance. This article guides you on how to use a single Amazon Bedrock Knowledge Base (KB) instance to reduce costs and complexity, while combining it with Amazon Verified Permissions (AVP) to dynamically isolate data at retrieval time (runtime) using lightweight Cedar policies.

---

## Architectural Guidance

The core of this architecture is shifting from hard-coded data filters in the source code to an external dynamic policy evaluation mechanism. Instead of building a separate Knowledge Base for each department—which wastes resources—we consolidate them into a single KB and control access using metadata tags combined with centrally defined logical conditions.

**The solution architecture is now as follows:**

![Hình 1](/images/3-BlogsTranslated/3.3-Blog3/ARCHBLOG-1492-1.png)
> *Hình 1. Truy cập theo cấp độ vai trò (role) tới dữ liệu và tài nguyên dùng chung của tổ chức.*
![Hình 2](/images/3-BlogsTranslated/3.3-Blog3/ARCHBLOG-1492-2.png)
> *Hình 2. Pipeline ingestion: tài liệu upload lên Amazon S3 kích hoạt Amazon EventBridge, định tuyến qua Amazon SQS đến một AWS Lambda function ghi metadata. Một Lambda theo lịch sau đó kích hoạt job ingestion của Amazon Bedrock Knowledge Bases.*

![Hình 3](/images/3-BlogsTranslated/3.3-Blog3/ARCHBLOG-1492-3.png)
> *Hình 3. Luồng truy vấn: request của người dùng đi qua Amazon CloudFront và AWS WAF đến Amazon API Gateway, nơi Lambda Authorizer đánh giá phân quyền Lớp 1 (cấp API) với Verified Permissions. Nếu được phép, middleware Lambda đánh giá phân quyền Lớp 2 (cấp tài liệu), dựng metadata filter và gọi RetrieveAndGenerate.*

---

The solution aims for a Defense-in-Depth model with two independent layers:
- **Layer 1 (API Access)**: A Lambda Authorizer at the API Gateway calls Verified Permissions to decide whether the user has permission to invoke the API.
- **Layer 2 (Document Access)**: A Middleware Lambda coordinates calls to the Knowledge Base, querying Verified Permissions to determine which department tags the user is allowed to access, thereby building an automated query pre-filter.

This model provides logic-level isolation (filter-level) within an enterprise, enabling authorization rules to change in minutes without redeploying application code.

---

## Technology Choices and Communication Scope

| Communication Scope | Technologies / Patterns to Consider |
| :--- | :--- |
| User Group Identity & Claims | Amazon Cognito User Pools, JWT (`cognito:groups` claim) |
| Perimeter defense & rate limiting | AWS WAF, Amazon CloudFront |
| Two-layer authorization evaluation (Layer 1 & 2) | Amazon Verified Permissions (AVP), Cedar Policy Language |
| Automated ingestion & data packaging | Amazon EventBridge, Amazon SQS, AWS Lambda, Amazon S3 (Versioning & Object Lock) |
| Vector search & answer generation | Knowledge Bases for Amazon Bedrock, Amazon Titan Text Embeddings V2, Anthropic Claude 3 / Nova |

---

## System Workflow (The Security Flow)

### 1. Ingestion Pipeline (Automated Data Tagging)

For the filters to function correctly during queries, documents must be labeled by department upon storage through two phases:

- **Phase 1 (Event-driven)**: Documents uploaded to S3 under specific prefixes (e.g., `docs/dept-a/`) trigger EventBridge events. These are sent to an SQS buffer to invoke a Lambda function that writes the corresponding sidecar `.metadata.json` file (e.g., `department: dept-a`).
- **Phase 2 (Scheduled)**: Every 5 minutes, a Lambda function triggers `StartIngestionJob` for Bedrock to scan S3, perform chunking (300 tokens, 20% overlap), generate embeddings, and index vectors along with metadata attributes into the database.

### 2. Query Flow (Secure Query Processing)

When a user submits a query along with their ID Token, the system performs rigorous checks:

- **AWS WAF & API Gateway**: Verify rate limits and request validity.
- **Lambda Authorizer (Layer 1)**: Decodes the JWT, checks `cognito:groups`, and invokes AVP `is_authorized` to confirm execution permission for the query action on the API resource. If denied, it returns a 403 error.
- **Middleware Lambda (Layer 2)**: Calls AVP to check which Knowledge Base IDs the user's groups are allowed to access (e.g., `dept-a`, `dept-b`).
- **Constructing the Pre-filter**: Converts AVP's decision into a `kb_filter` structure (using `equals` or `orAll` operators) and passes it into Bedrock's `retrieve_and_generate` API.
- **Bedrock KB & Guardrails**: The vector database filters out documents without matching ownership tags before executing similarity search. The LLM generates answers based on this sanitized context, which is then verified by Guardrails for Amazon Bedrock for contextual grounding before being returned to the user.

---

## New Features in the Solution

### 1. Defining Authorization Policies with Cedar Language

All authorization logic is completely decoupled from the code and stored in Verified Permissions. For example, a Cedar policy configuration that allows the `dept-a` group to query their own data and grants cross-department access to the `dept-c` group (Executive Board):

```cedar
// Internal access for dept-a
permit(
    principal in GenAIApp::UserGroup::"dept-a",
    action == GenAIApp::Action::"query",
    resource == GenAIApp::KnowledgeBase::"dept-a"
);

// Cross-department access for executive board (dept-c)
permit(
    principal in GenAIApp::UserGroup::"dept-c",
    action == GenAIApp::Action::"query",
    resource
);
```

### 2. Setting up Ingestion Safeguards in Code

The Lambda code strictly validates the existence of the metadata sidecar file before pushing it into the indexing pipeline, preventing documents from entering the system without security labels:

```python
# Validate the presence of sidecar file before Ingestion
objects = s3.list_objects_v2(Bucket=BUCKET, Prefix=f"docs/{dept}/")
docs = [o["Key"] for o in objects.get("Contents", []) if not o["Key"].endswith(".metadata.json")]

for doc_key in docs:
    sidecar_key = f"{doc_key}.metadata.json"
    try:
        s3.head_object(Bucket=BUCKET, Key=sidecar_key)
    except s3.exceptions.ClientError:
        logger.warning(f"Skipping {doc_key}: sidecar metadata file not found")
        docs.remove(doc_key)
```

**Storage Layer Security Note:** Apply an S3 Bucket Policy to ensure only the ARN role of the Tagging Lambda has `s3:PutObject` permissions for `.metadata.json` files, preventing spoofing or unauthorized alteration of tags by other users.

### Original Article Link:
https://aws.amazon.com/blogs/security/secure-multi-tenant-rag-with-amazon-bedrock-and-amazon-verified-permissions/
