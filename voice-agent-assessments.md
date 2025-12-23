# Voice Agent Assessment Templates

## Assessment Structure for Voice Interaction

Each assessment session should follow this flow:
1. **Introduction** (15 seconds)
2. **Difficulty Calibration** (1 question, 30 seconds)
3. **Main Assessment** (5-10 questions, 5-10 minutes)
4. **Performance Summary** (45 seconds)
5. **Recommendations** (30 seconds)

---

## Domain 1: Plan and Manage Azure AI Solutions

### Beginner Level Questions

**Q1-B1**: "You're building an AI application for a healthcare company. Which Responsible AI principle ensures patient data is protected?"
- A) Fairness
- B) Privacy & Security ✓
- C) Transparency
- D) Accountability

**Correct Answer**: B
**Explanation**: Privacy & Security ensures data protection, encryption, and compliance with regulations like HIPAA.
**Follow-up**: "Can you name the other five Responsible AI principles?"

---

**Q1-B2**: "What authentication method provides the best security for enterprise Azure AI applications?"
- A) API Keys
- B) Azure AD with Managed Identities ✓
- C) Connection Strings
- D) SAS Tokens

**Correct Answer**: B
**Explanation**: Azure AD with Managed Identities provides enterprise-grade security, RBAC, audit trails, and eliminates credential management.

---

**Q1-B3**: "Your Azure AI service is returning HTTP 429 errors. What does this mean?"
- A) Authentication failed
- B) Service unavailable
- C) Rate limit exceeded ✓
- D) Bad request format

**Correct Answer**: C
**Explanation**: HTTP 429 indicates rate limiting. Implement retry logic with exponential backoff or scale to a higher tier.

---

### Intermediate Level Questions

**Q1-I1**: "You need to monitor Azure AI services for errors and latency. Which three Azure services would you use?"
- A) Azure Monitor ✓
- B) Azure DevOps
- C) Application Insights ✓
- D) Log Analytics ✓
- E) Azure Functions

**Correct Answer**: A, C, D
**Explanation**: Azure Monitor for metrics and alerts, Application Insights for tracing and dependencies, Log Analytics for KQL queries.

---

**Q1-I2**: "Your company requires AI services accessible only from the corporate network and not the public internet. What's the complete solution?"
**Expected Answer Components**:
- Virtual Network (VNet)
- Private Endpoint
- Disable public network access
- Network Security Groups (NSG) for additional control

**Grading**: Award points for each component mentioned.

---

**Q1-I3**: "You have a Computer Vision service processing 1 million images monthly with predictable patterns. How do you optimize costs?"
**Expected Answer**:
- Use Commitment Tier (reserved capacity) for 30-50% discount
- Monitor usage patterns to right-size tier
- Consider caching for frequent requests
- Use appropriate SKU (S1 vs S2)

**Key phrase to listen for**: "Commitment Tier" or "Reserved Capacity"

---

### Advanced Level Questions

**Q1-A1**: "Design a multi-region, highly available Azure AI solution that meets 99.99% uptime with automatic failover. Walk me through your architecture."

**Expected Components** (allow 2-3 minutes for answer):
1. Multi-region deployment (at least 2 regions)
2. Azure Traffic Manager with Priority routing
3. Health probes for monitoring
4. Automated failover configuration
5. Data replication strategy
6. Monitoring and alerting
7. Load distribution considerations

**Grading**: 
- 5/7 components = Good
- 6/7 components = Very Good
- 7/7 components = Excellent

---

**Q1-A2**: "You're implementing CI/CD for Azure AI services. Describe your pipeline from code commit to production deployment."

**Expected Pipeline Stages**:
1. Source control trigger (Git)
2. Infrastructure as Code (Bicep/ARM/Terraform)
3. Build and validation
4. Deploy to dev environment
5. Run automated tests
6. Deploy to staging with approval gate
7. Production deployment (blue-green or canary)
8. Post-deployment monitoring
9. Rollback strategy

**Key concepts to listen for**: Infrastructure as Code, staging environments, approval gates, testing

---

**Q1-A3**: "You have 50 AI resources across multiple subscriptions and regions. How do you enforce that all resources must: use customer-managed keys, be deployed only in US regions, and have environment tags?"

**Expected Answer**: Azure Policy
**Components**:
- Create custom policy definition or use built-in policies
- Policy for allowed locations (US regions)
- Policy for required tags
- Policy for customer-managed encryption keys
- Assign to management group or subscriptions
- Compliance dashboard for monitoring
- Remediation tasks for existing resources

---

## Domain 2: Implement Generative AI Solutions

### Beginner Level Questions

**Q2-B1**: "Which Azure OpenAI model would you use for a simple customer service chatbot that needs fast responses and cost efficiency?"
- A) GPT-4 Turbo
- B) GPT-3.5 Turbo ✓
- C) text-embedding-ada-002
- D) DALL-E 3

**Correct Answer**: B
**Explanation**: GPT-3.5 Turbo is fast, cost-effective, and sufficient for simple chatbot tasks.

---

**Q2-B2**: "What temperature setting would you use for a legal document classification task that needs consistent, deterministic results?"
- A) 0.0 - 0.3 ✓
- B) 0.7 - 1.0
- C) 1.5 - 2.0
- D) It doesn't matter

**Correct Answer**: A
**Explanation**: Low temperature (0.0-0.3) provides deterministic, consistent outputs ideal for classification.

---

**Q2-B3**: "What does RAG stand for in the context of generative AI?"
**Expected Answer**: Retrieval Augmented Generation

**Follow-up**: "And in one sentence, what problem does it solve?"
**Expected**: "Reduces hallucinations by grounding responses in retrieved data" or similar

---

### Intermediate Level Questions

**Q2-I1**: "You're building a chatbot that answers questions about your company's 500-page employee handbook. The bot sometimes makes up policies that don't exist. What's your solution?"

**Expected Answer**: Implement RAG (Retrieval Augmented Generation)

**Follow-up**: "Walk me through the RAG implementation steps."
**Expected Components**:
1. Chunk the handbook into sections
2. Generate embeddings using text-embedding-ada-002
3. Store in Azure AI Search with vector index
4. At query time: embed user question
5. Vector search for relevant chunks
6. Inject chunks as context in prompt
7. Generate response grounded in context
8. Include source citations

---

**Q2-I2**: "Your chatbot needs to check real-time product inventory from your database. Should you use function calling or RAG?"
- A) RAG
- B) Function calling ✓
- C) Either works
- D) Neither

**Correct Answer**: B
**Explanation**: Function calling is for real-time data and actions. RAG is for static knowledge retrieval.

**Follow-up**: "What are the main components you need to implement function calling?"
**Expected**: Function schema with parameters, function execution logic, handling function call responses

---

**Q2-I3**: "You're deploying a customer-facing chatbot. What content filtering configuration would you recommend?"
**Expected Answer**:
- Enable content filtering on input and output
- Set filters to at least "Medium" severity blocking for all categories
- Consider "Low" for stricter filtering in customer-facing apps
- Implement error handling for blocked content (HTTP 400)
- Log filtered content for review
- Custom blocklists for company-specific terms

---

### Advanced Level Questions

**Q2-A1**: "I want you to design a complete RAG solution for a legal document search system with 100,000 documents. Explain your chunking strategy, indexing approach, and retrieval optimization."

**Expected Components** (allow 3-4 minutes):
1. **Chunking Strategy**:
   - 750-1000 token chunks
   - 100-token overlap for context continuity
   - Preserve document structure (sections, paragraphs)
   - Metadata: document name, date, category

2. **Indexing**:
   - Azure AI Search with vector and keyword fields
   - HNSW algorithm for vector search
   - Hybrid search configuration
   - Semantic ranker for reranking

3. **Retrieval Optimization**:
   - Metadata filtering (date range, category)
   - Hybrid search (vector + keyword)
   - Top-K retrieval (K=3-5)
   - Reranking for relevance
   - Citation tracking

4. **Prompt Engineering**:
   - Clear instructions to use only context
   - Handle no-result cases
   - Request source citations

---

**Q2-A2**: "Your GPT-4 chatbot costs are too high. What are five specific strategies you'd implement to reduce costs?"

**Expected Strategies** (any 5):
1. Use GPT-3.5 Turbo for simpler queries (classification first)
2. Implement semantic caching for similar questions
3. Optimize prompts to reduce token count
4. Set max_tokens to limit response length
5. Stream responses and allow user to stop early
6. Use text-embedding-3-small instead of ada-002
7. Batch processing for non-real-time queries
8. Implement query routing (simple → GPT-3.5, complex → GPT-4)
9. Use commitment tier if volume is predictable
10. Compress context with summarization

**Grading**: 1 point per valid strategy

---

**Q2-A3**: "Compare Azure OpenAI Chat Completions API versus Assistants API. When would you use each?"

**Expected Comparison**:

**Chat Completions API**:
- Stateless (you manage history)
- More control over conversation flow
- Lower cost (no automatic features)
- Use for: Custom chat applications, fine-grained control, cost optimization

**Assistants API**:
- Stateful (Azure manages history)
- Built-in retrieval (automatic RAG)
- Code interpreter for calculations
- Use for: Quick prototypes, data analysis, document Q&A, less development effort

**Key decision factor**: Control vs. convenience

---

## Rapid-Fire Quiz Format (20 questions in 5 minutes)

### Question Bank (Read question, wait for 1-2 word answer)

1. "What does GPT stand for?" → Generative Pre-trained Transformer
2. "Name the Azure service for document OCR and form extraction" → Document Intelligence (or Form Recognizer)
3. "HTTP status code for rate limiting?" → 429
4. "Temperature range for creative writing?" → 0.8 to 2.0 (accept "high")
5. "What does RAG stand for?" → Retrieval Augmented Generation
6. "Token limit for GPT-4 Turbo?" → 128,000 (accept "128k")
7. "Which is cheaper: GPT-3.5 or GPT-4?" → GPT-3.5
8. "Service for semantic search on your data?" → Azure AI Search
9. "What are the dimensions of ada-002 embeddings?" → 1536
10. "Name one content filter category" → Hate, Sexual, Violence, or Self-Harm
11. "What's max_tokens used for?" → Limit response length
12. "Managed identity type tied to resource lifecycle?" → System-assigned
13. "Which Responsible AI principle addresses bias?" → Fairness
14. "Service for creating custom chatbots?" → Azure Bot Service
15. "What does CLU stand for?" → Conversational Language Understanding
16. "Tool for querying Azure logs?" → Log Analytics (KQL)
17. "Name the vector search algorithm in Azure AI Search" → HNSW
18. "What does PTU stand for in Azure OpenAI?" → Provisioned Throughput Units
19. "Service for video analysis and indexing?" → Video Indexer
20. "Name one cognitive skill in Azure AI Search" → Entity Recognition, OCR, Sentiment, Key Phrases (any)

**Scoring**:
- 16-20 correct: Excellent
- 12-15 correct: Good
- 8-11 correct: Fair, needs review
- <8 correct: Needs significant study

---

## Scenario-Based Assessment (Advanced)

### Scenario 1: Healthcare AI Application

**Setup**: "You're architecting an AI solution for a hospital. The system needs to:
- Analyze patient medical records and radiology images
- Answer doctor queries about treatment options
- Ensure HIPAA compliance
- Maintain 99.9% availability
- Keep all data in US regions

Walk me through your complete solution."

**Expected Components**:
1. **Services**:
   - Azure OpenAI for Q&A
   - Azure AI Vision for radiology
   - Document Intelligence for records
   - Azure AI Search for knowledge base

2. **Security**:
   - HIPAA-compliant regions
   - Encryption at rest and in transit
   - Azure AD authentication
   - Private endpoints/VNet
   - Customer-managed keys
   - Audit logging

3. **Availability**:
   - Multi-region deployment
   - Traffic Manager for failover
   - Health monitoring

4. **Compliance**:
   - Azure Policy for region restrictions
   - Content filtering for safety
   - Data retention policies
   - Audit trails

**Grading**: Component-based, award points for each area covered

---

### Scenario 2: E-commerce Chatbot

**Setup**: "Design a chatbot for an e-commerce site that can:
- Answer product questions from catalog
- Check real-time inventory
- Track order status
- Recommend products
- Handle 10,000 concurrent users

What's your architecture and implementation?"

**Expected Components**:
1. **Core AI**:
   - Azure OpenAI (GPT-3.5 Turbo for cost)
   - RAG for product catalog
   - Function calling for inventory and orders

2. **Data Layer**:
   - Azure AI Search for product catalog
   - Vector search for recommendations
   - Database for inventory/orders

3. **Scalability**:
   - Provisioned Throughput for consistent performance
   - Caching layer (Azure Redis)
   - Load balancing

4. **Integration**:
   - Function calling schema for:
     - check_inventory(product_id)
     - get_order_status(order_id)
     - search_products(query)

5. **Monitoring**:
   - Application Insights
   - Usage analytics
   - Cost tracking

---

## Assessment Session Flow Template

### Opening (15 seconds):
"Great! I'm going to assess your knowledge on [DOMAIN]. I'll present 5-10 scenarios and questions. Take your time with each answer. Ready to begin?"

### For Each Question (1-2 minutes):
1. **Present scenario** (15-20 seconds)
2. **Ask question clearly**
3. **Wait for complete answer**
4. **Provide immediate feedback**:
   - ✓ Correct: "That's right! [Brief validation]"
   - ✗ Incorrect: "Not quite. The correct answer is [X] because [reason]. The key concept here is [concept]."
5. **Optional follow-up** if answer incomplete

### Closing Summary (45 seconds):
"Excellent work! Here's your performance:
- You scored [X] out of [Y]
- Strong areas: [List 2-3]
- Areas to review: [List 1-2]
- Overall assessment: [Beginner/Intermediate/Advanced]"

### Recommendations (30 seconds):
"Based on your performance, I recommend:
1. [Specific topic to study]
2. [Hands-on practice suggestion]
3. [Resource recommendation]

Ready to dive deeper into any weak areas, or would you like to continue to a new domain?"

---

## Adaptive Difficulty

### Rules for Progression:
- Start with **beginner** question
- If correct (80%+ confidence in answer) → Move to **intermediate**
- If incorrect or uncertain → Stay at beginner, ask another
- If 3+ intermediate correct → Try **advanced**
- If advanced correct → Maintain advanced level
- If advanced incorrect → Return to intermediate

### Difficulty Indicators to Listen For:
- **Beginner**: Single correct answer, recalls facts
- **Intermediate**: Explains reasoning, multiple components
- **Advanced**: Architecture discussion, trade-offs, best practices

---

## Performance Tracking

Track across sessions:
```
Domain 1: 7/10 (70%) - Weak: Cost optimization
Domain 2: 9/10 (90%) - Strong
Domain 3: 6/10 (60%) - Weak: Bot Framework SDK
Domain 4: 8/10 (80%) - Strong
Domain 5: 5/10 (50%) - Weak: CLU training
Domain 6: 7/10 (70%) - Weak: Custom skills
```

**Readiness Calculation**:
- Overall score ≥85% = Exam ready
- Overall score 70-84% = Almost ready, review weak areas
- Overall score <70% = More study needed

**Recommendation Logic**:
- Spend 50% of time on lowest-scoring domain
- Revisit incorrectly answered questions after 2-3 days
- Mix beginner and intermediate questions for weak areas
- Challenge with advanced questions in strong areas

This assessment system ensures comprehensive, adaptive evaluation through voice interaction!
