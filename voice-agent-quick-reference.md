# AI-102 Quick Reference & Exam Cheat Sheet

## 🎯 Exam Structure (Updated Dec 2024)

- **Duration**: 100 minutes
- **Questions**: 40-60 questions
- **Passing Score**: 700/1000 (approximately 70%)
- **Question Types**: Multiple choice, multiple response, drag-and-drop, case studies
- **Languages**: English, Japanese, Chinese, Korean, German, French, Spanish, Portuguese, Italian
- **Cost**: $165 USD

### Exam Domains & Weights:
1. Plan and Manage Azure AI Solutions: **20-25%**
2. Implement Generative AI Solutions: **15-20%**
3. Implement Agentic Solutions: **15-20%**
4. Implement Computer Vision Solutions: **15-20%**
5. Implement Natural Language Processing: **15-20%**
6. Implement Knowledge Mining: **10-15%**

---

## 📋 Domain-by-Domain Quick Reference

### Domain 1: Plan and Manage (20-25%)

#### Responsible AI Principles (MEMORIZE):
1. **F**airness - Treat all people fairly
2. **R**eliability & Safety - Perform reliably and safely
3. **P**rivacy & Security - Protect personal data
4. **I**nclusiveness - Benefit everyone
5. **T**ransparency - Understandable AI
6. **A**ccountability - People are accountable

**Mnemonic**: **FR-PITA** or "Fair, Reliable, Private, Inclusive, Transparent, Accountable"

#### Authentication Quick Decisions:
```
Dev/Test? → API Keys
Enterprise/Production? → Azure AD + Managed Identity
Azure service calling AI? → Managed Identity (preferred)
Non-Azure calling AI? → Service Principal
```

#### Common HTTP Error Codes:
- **400**: Bad request or content filter blocked
- **401**: Authentication failed (invalid key/token)
- **403**: Forbidden (insufficient permissions)
- **429**: Rate limit exceeded (implement exponential backoff)
- **500**: Internal server error

#### Monitoring Services:
- **Azure Monitor**: Metrics, alerts, basic monitoring
- **Application Insights**: Distributed tracing, dependencies
- **Log Analytics**: KQL queries, custom analysis

#### Cost Optimization Checklist:
- ✓ Use commitment tier for predictable workloads
- ✓ Cache frequent requests
- ✓ Right-size SKU (don't over-provision)
- ✓ Implement auto-scaling
- ✓ Set budgets and alerts
- ✓ Use GPT-3.5 instead of GPT-4 when possible
- ✓ Optimize token usage

---

### Domain 2: Generative AI (15-20%)

#### Model Selection Matrix:
| Use Case | Model | Why |
|----------|-------|-----|
| Simple chatbot | GPT-3.5 Turbo | Fast, cheap |
| Complex reasoning | GPT-4 | Most capable |
| Long documents | GPT-4 Turbo | 128k context |
| Cost-sensitive | GPT-3.5 Turbo | 10x cheaper |
| Semantic search | text-embedding-ada-002 | Best for embeddings |
| Image generation | DALL-E 3 | Latest image model |
| Images + Text | GPT-4o | Multimodal |

#### Temperature Settings:
```
0.0 - 0.3: Classification, extraction, deterministic
0.4 - 0.7: Chat, Q&A, balanced
0.8 - 1.5: Creative writing, brainstorming
1.6 - 2.0: Maximum creativity
```

#### Token Limits (Memorize):
- GPT-3.5 Turbo: **16,385 tokens**
- GPT-4: **8,192 or 32,768 tokens**
- GPT-4 Turbo: **128,000 tokens**
- GPT-4o: **128,000 tokens**

#### RAG Implementation Steps:
1. Chunk documents (500-1000 tokens)
2. Generate embeddings (text-embedding-ada-002)
3. Index in Azure AI Search (vector field)
4. Query: Embed user question
5. Vector search (top-K results)
6. Inject context into prompt
7. Generate grounded response
8. Include citations

#### Content Filter Categories (Memorize):
1. **Hate** - Discrimination, attacks
2. **Sexual** - Explicit content
3. **Violence** - Graphic violence
4. **Self-Harm** - Self-injury content

**Severity Levels**: Safe, Low, Medium, High

#### Function Calling Components:
1. Define function schema (name, description, parameters)
2. Send to OpenAI with functions array
3. Check if finish_reason == "function_call"
4. Execute actual function
5. Send result back to model
6. Get final response

---

### Domain 3: Agentic Solutions (15-20%)

#### Azure Bot Service Architecture:
```
User → Channel (Teams, Web Chat, Slack)
     → Bot Service
     → Bot Application (your code)
     → Bot Framework SDK
     → Azure AI Services
```

#### Key Bot Components:
- **Activity**: Message or event
- **Turn**: One interaction cycle
- **Conversation**: Series of turns
- **Dialog**: Conversation flow manager
- **State**: Conversation data storage

#### Dialog Types:
1. **Waterfall Dialog**: Sequential steps
2. **Component Dialog**: Reusable dialog modules
3. **Prompt Dialog**: Collect user input (Text, Choice, Confirm, Number)

#### Bot State Management:
- **User State**: Persists across conversations (preferences)
- **Conversation State**: Current conversation data
- **Private Conversation State**: Channel-specific data

#### Prompt Flow Components:
- **Nodes**: Actions (LLM, Python, Prompt)
- **Edges**: Flow connections
- **Inputs**: Variables
- **Outputs**: Results
- **Variants**: A/B testing configurations

#### Agent Orchestration Patterns:
1. **Sequential**: Chain steps one after another
2. **Parallel**: Execute multiple tasks simultaneously
3. **Conditional**: Branch based on results
4. **Loop**: Repeat until condition met

---

### Domain 4: Computer Vision (15-20%)

#### Azure AI Vision API Capabilities:
- **Analyze Image**: Tags, objects, faces, brands, colors
- **OCR**: Extract text from images
- **Spatial Analysis**: People detection, zone crossing
- **Background Removal**: Extract foreground objects
- **Smart Crops**: Thumbnail generation

#### Custom Vision Use Cases:
- **Classification**: "Is this a cat or dog?"
- **Object Detection**: "Where are the defects in this product?"

**When to Use**:
- Need industry-specific models
- Unique objects/scenarios
- Pre-built API doesn't meet needs

#### Training Requirements:
- **Classification**: 30+ images per tag minimum (50+ recommended)
- **Object Detection**: 15+ images per tag (50+ recommended)
- Balance classes for better accuracy

#### Model Evaluation Metrics:
- **Precision**: Of predicted positives, how many are correct?
- **Recall**: Of actual positives, how many were found?
- **F1 Score**: Harmonic mean of precision and recall
- **mAP (mean Average Precision)**: Overall detection quality

**Improving Accuracy**:
1. Add more diverse training images
2. Balance class distribution
3. Increase training iterations
4. Use transfer learning
5. Adjust probability threshold

#### Face API Capabilities:
- **Detection**: Find faces, get attributes (age, emotion, pose)
- **Verification**: Are two faces the same person?
- **Identification**: Who is this person? (match against group)
- **Find Similar**: Find similar faces

**Important**: Limited access policy requires application approval

#### Video Indexer Features:
- Face detection and recognition
- Speech transcription
- OCR in video
- Scene detection
- Sentiment analysis
- Content moderation

---

### Domain 5: Natural Language Processing (15-20%)

#### Azure AI Language Service Capabilities:
- **Sentiment Analysis**: Positive, negative, neutral, mixed
- **Key Phrase Extraction**: Important phrases
- **Entity Recognition**: Person, location, organization, etc.
- **PII Detection**: Personal identifiable information
- **Language Detection**: Identify language
- **Entity Linking**: Link to Wikipedia
- **Text Summarization**: Extractive or abstractive

#### Conversational Language Understanding (CLU):
**Concepts**:
- **Intent**: What user wants to do (BookFlight, CancelReservation)
- **Entity**: Key information (City, Date, Time)
- **Utterance**: Example user input

**Entity Types**:
- **Learned**: Learned from context (ProductName)
- **List**: Predefined values (Color: red, blue, green)
- **Prebuilt**: Built-in (Email, URL, Phone, DateTime)

**Training Process**:
1. Define intents
2. Add sample utterances (15+ per intent)
3. Label entities in utterances
4. Train model
5. Test and evaluate
6. Deploy to prediction endpoint

#### Question Answering Service:
- Successor to QnA Maker
- Q&A pairs from documents, URLs, FAQs
- Active learning improves over time
- Chitchat personality support

**Sources**:
- FAQs (URLs)
- Documents (PDF, DOCX)
- Manual Q&A pairs
- Multi-turn conversations

#### Translation Service:
- **Text Translation**: 100+ languages
- **Document Translation**: Maintain formatting
- **Custom Translator**: Domain-specific terminology

**Key Features**:
- Language detection
- Transliteration
- Dictionary lookup
- Custom models with parallel documents

---

### Domain 6: Knowledge Mining (10-15%)

#### Azure AI Search Architecture:
```
Data Sources → Indexer → Skillset → Index → Query
```

#### Key Components:
1. **Data Source**: Where data lives (Blob, SQL, Cosmos)
2. **Indexer**: Crawls and extracts data
3. **Skillset**: AI enrichment pipeline
4. **Index**: Searchable structure
5. **Indexer Schedule**: Incremental updates

#### Built-in Cognitive Skills:
- **Text**: Language detection, key phrases, sentiment, translation
- **Vision**: OCR, image analysis, face detection
- **Structured**: Entity recognition, PII detection
- **Content**: Merge, split, conditional

#### Custom Skills:
- Azure Function or web API
- Receives JSON input
- Returns JSON output
- Integrates into skillset pipeline

#### Vector Search:
- HNSW algorithm for approximate nearest neighbor
- Embeddings stored in vector field
- Hybrid search: Vector + keyword combined
- Use for semantic similarity

#### Knowledge Store:
- Persist enriched data
- **Table projections**: Azure Table Storage
- **Object projections**: JSON blobs
- **File projections**: Images, extracted files

#### Query Syntax:
- **Simple**: Basic keyword search (default)
- **Full Lucene**: Advanced (wildcards, fuzzy, regex, proximity)

**Simple Example**: `search=hotel +airport -expensive`

**Full Lucene Example**: `search=hotel~ AND (airport OR "downtown area")~5`

#### Document Intelligence (Form Recognizer):
- **Prebuilt Models**: Invoice, receipt, ID, business card, W-2
- **Custom Models**: Train on your forms (5+ samples)
- **Layout**: Extract text, tables, structure
- **General Document**: Extract key-value pairs

---

## 🔥 Most Tested Concepts (High-Yield Topics)

### 1. RAG Implementation ⭐⭐⭐
- How it works (retrieval + augmentation)
- When to use vs. fine-tuning
- Implementation with Azure AI Search
- Chunking strategies

### 2. Content Filtering ⭐⭐⭐
- Four categories
- Severity levels
- Configuration per category
- Handling HTTP 400 errors

### 3. Authentication & Security ⭐⭐⭐
- Managed identities (system vs. user)
- When to use keys vs. Azure AD
- VNet and private endpoints
- Encryption (at rest, in transit)

### 4. Function Calling ⭐⭐⭐
- When to use (real-time data, actions)
- Implementation pattern
- Function schema definition
- Handling responses

### 5. Custom Vision vs. Pre-built ⭐⭐
- When to use each
- Training requirements
- Classification vs. object detection

### 6. CLU (Conversational Language Understanding) ⭐⭐
- Intents and entities
- Training process
- Entity types (learned, list, prebuilt)

### 7. Azure AI Search Skillsets ⭐⭐
- Built-in skills
- Custom skills
- Indexer pipeline

### 8. Bot Framework Dialogs ⭐⭐
- Waterfall vs. component
- State management
- Turn context

### 9. Responsible AI Principles ⭐⭐⭐
- All six principles
- Scenario-based questions
- Implementation practices

### 10. Cost Optimization ⭐⭐
- Commitment tiers
- Model selection (GPT-3.5 vs. GPT-4)
- Token optimization
- Caching strategies

---

## 🚨 Common Exam Traps

### Trap 1: Hallucination Problem
**Question**: "LLM is providing incorrect information"
**Wrong Answer**: Fine-tune the model
**Correct Answer**: Implement RAG to ground responses in your data

### Trap 2: Real-Time Data
**Question**: "Bot needs current stock prices"
**Wrong Answer**: Use RAG
**Correct Answer**: Use function calling to query API

### Trap 3: Edge Deployment
**Question**: "IoT device with intermittent connectivity needs AI"
**Wrong Answer**: Call Azure API directly
**Correct Answer**: Use containerized AI service for edge deployment

### Trap 4: Rate Limiting
**Question**: "Getting HTTP 429 errors"
**Wrong Answer**: Immediately retry
**Correct Answer**: Implement exponential backoff retry logic

### Trap 5: Model Selection
**Question**: "Need 50,000 token context"
**Wrong Answer**: GPT-4
**Correct Answer**: GPT-4 Turbo (128k context)

### Trap 6: Cost Reduction
**Question**: "High OpenAI costs for simple classification"
**Wrong Answer**: Fine-tune GPT-4
**Correct Answer**: Switch to GPT-3.5 Turbo (10x cheaper for simple tasks)

### Trap 7: Authentication
**Question**: "Azure VM needs to call AI service securely"
**Wrong Answer**: Store API key in VM
**Correct Answer**: Use system-assigned managed identity

### Trap 8: Custom Vision Training
**Question**: "Custom Vision model has low accuracy"
**Wrong Answer**: Increase training iterations only
**Correct Answer**: Add more diverse training images, balance classes, check data quality

---

## 📝 Pre-Exam Checklist

### 1 Week Before:
- ✓ Review all six domains
- ✓ Take practice assessments
- ✓ Identify weak areas
- ✓ Complete hands-on labs for each domain
- ✓ Memorize key facts (principles, token limits, HTTP codes)

### 3 Days Before:
- ✓ Focus on weak areas only
- ✓ Review architecture patterns
- ✓ Practice scenario-based questions
- ✓ Review quick reference materials
- ✓ Get hands-on with Azure Portal

### 1 Day Before:
- ✓ Light review only (no cramming)
- ✓ Review this cheat sheet
- ✓ Memorize Responsible AI principles
- ✓ Review common exam traps
- ✓ Prepare ID, test center info
- ✓ Get good sleep!

### Exam Day:
- ✓ Arrive 15 minutes early
- ✓ Read questions carefully (twice)
- ✓ Flag difficult questions for review
- ✓ Use elimination strategy for multiple choice
- ✓ Watch for "select all that apply" questions
- ✓ Manage time: ~1.5-2 minutes per question
- ✓ Review flagged questions before submitting

---

## 🎯 Exam Strategy Tips

### Time Management:
- **100 minutes** for 40-60 questions
- Budget **1.5-2 minutes** per question
- Reserve **10-15 minutes** for review
- Flag difficult questions, return later

### Question Reading:
1. Read question completely
2. Identify **key requirements** (security, cost, performance)
3. Eliminate obviously wrong answers
4. Look for qualifiers: "most", "best", "least"
5. Watch for **negatives**: "NOT", "EXCEPT"

### Case Study Questions:
- Read case study overview first
- Refer back to case study for each question
- Requirements often hidden in details
- Multiple questions from same case study

### Multiple Response Questions:
- Says "Select all that apply" or "Select TWO"
- Usually harder (more cognitive load)
- Partial credit NOT given
- Must get all correct answers

### Scenario-Based Strategy:
1. Identify the **core problem**
2. List **requirements** (security, scale, cost)
3. Map requirements to **Azure services**
4. Choose **simplest** solution that meets all requirements

---

## 💡 Last-Minute Memory Tricks

### The "FAR-PIT" Responsible AI:
- **F**airness - No bias
- **A**ccountability - Human responsibility
- **R**eliability - Perform safely
- **P**rivacy - Protect data
- **I**nclusiveness - Everyone benefits
- **T**ransparency - Explainable

### HTTP Error Codes Song:
- "**400** bad request, your content's a mess"
- "**401** denied, your auth failed the test"
- "**429** rate limited, slow down the pace"
- "**500** server error, Azure's red face"

### Model Selection Rhyme:
- "**3.5** for cheap and speed"
- "**GPT-4** when brains you need"
- "**Turbo** for context long"
- "**Embeddings** make search strong"

### RAG Steps Acronym: "**CESAR**"
- **C**hunk documents
- **E**mbed chunks
- **S**tore in search
- **A**ugment prompt
- **R**espond with context

---

## 🏆 Confidence Boosters

### You Know You're Ready When:
- ✓ Can explain RAG architecture from memory
- ✓ Know all six Responsible AI principles
- ✓ Can choose the right model for any scenario
- ✓ Understand when to use function calling vs. RAG
- ✓ Know HTTP error codes and solutions
- ✓ Can design multi-region AI architecture
- ✓ Understand cost optimization strategies
- ✓ Know CLU training process
- ✓ Can explain Custom Vision use cases
- ✓ Understand Azure AI Search skillsets

### Common Score Distribution:
- **700-750**: Pass, but room for improvement
- **750-850**: Solid pass, good knowledge
- **850-900**: Strong pass, expert level
- **900-1000**: Exceptional, top 5%

### Post-Exam:
- Results shown immediately (pass/fail)
- Detailed score report sent within 24 hours
- Certificate available in Microsoft Learn profile
- Digital badge for LinkedIn
- Valid for **12 months**, then must renew

---

## 🔗 Essential Resources

### Official Microsoft:
- [AI-102 Study Guide](https://aka.ms/ai102-StudyGuide)
- [Microsoft Learn AI-102 Path](https://learn.microsoft.com/training/courses/ai-102t00/)
- [Azure AI Documentation](https://learn.microsoft.com/azure/ai-services/)
- [Practice Assessment](https://learn.microsoft.com/credentials/certifications/azure-ai-engineer/practice/assessment)

### Hands-On Labs:
- [Azure AI Labs](https://microsoftlearning.github.io/AI-102-AIEngineer/)
- [Azure Portal](https://portal.azure.com)
- [Azure AI Foundry](https://ai.azure.com)

### Community:
- [Microsoft Tech Community](https://techcommunity.microsoft.com/)
- [Azure AI Discord/Reddit](https://reddit.com/r/AZURE)

---

**Remember**: This exam tests **practical knowledge**, not memorization. Understand the **WHY** behind each service and **WHEN** to use it. You've got this! 🚀
