# Domain 2: Implement Generative AI Solutions (15-20%)

## Overview
This domain covers Azure OpenAI Service, prompt engineering, Retrieval Augmented Generation (RAG), and building production-ready generative AI applications. This is one of the most rapidly evolving and heavily tested areas.

## Key Skill Areas

### 2.1 Azure OpenAI Service Fundamentals

#### Available Models (as of Dec 2024)

**GPT-4 Family** (Most capable):
- **GPT-4 Turbo**: 128k token context, latest training data, function calling
- **GPT-4**: 8k/32k context, highly capable for complex tasks
- **GPT-4o** (optimized): Faster, more efficient, multimodal (text + images)
- **Use cases**: Complex reasoning, analysis, code generation, content creation

**GPT-3.5 Family** (Fast and efficient):
- **GPT-3.5 Turbo**: 16k context, fast responses, cost-effective
- **GPT-3.5 Turbo Instruct**: Optimized for instruction-following
- **Use cases**: Chatbots, simple classification, summarization

**Embeddings Models**:
- **text-embedding-ada-002**: 1536 dimensions, semantic search, similarity
- **text-embedding-3-small/large**: Newer, more efficient
- **Use cases**: Semantic search, clustering, recommendations, RAG

**DALL-E Models** (Image generation):
- **DALL-E 3**: High-quality image generation from text
- **DALL-E 2**: Previous generation

**Model Selection Criteria**:
- **Complexity**: GPT-4 for complex reasoning, GPT-3.5 for simpler tasks
- **Cost**: GPT-3.5 is ~10x cheaper than GPT-4
- **Speed**: GPT-3.5 Turbo is faster
- **Context length**: Use models with sufficient token capacity
- **Multimodal needs**: GPT-4o for images + text

**Exam focus**: Know which model to recommend for different scenarios based on requirements.

#### Deployment and Configuration

**Deployment Types**:
1. **Standard**: Pay-per-token, no commitment
2. **Provisioned Throughput**: Reserved capacity, consistent latency
   - PTU (Provisioned Throughput Units)
   - Best for high-volume, consistent workloads
   - Eliminates rate limiting

**Key Configuration Parameters**:

**Temperature** (0.0 - 2.0):
- **0.0-0.3**: Deterministic, consistent outputs (classification, fact extraction)
- **0.4-0.7**: Balanced creativity (general chat, Q&A)
- **0.8-2.0**: High creativity (brainstorming, creative writing)

**Top_p** (Nucleus sampling, 0.0 - 1.0):
- Alternative to temperature
- 0.1 = very focused, 1.0 = consider all options
- Microsoft recommendation: Adjust temperature OR top_p, not both

**Max_tokens**:
- Maximum response length
- Count includes both prompt and completion
- Set appropriately to control costs

**Frequency_penalty** (-2.0 to 2.0):
- Reduces repetition of tokens based on frequency
- Positive values discourage repetition

**Presence_penalty** (-2.0 to 2.0):
- Encourages model to talk about new topics
- Positive values increase topic diversity

**Stop sequences**:
- Strings that halt generation (e.g., "\n\n", "###")
- Useful for structured outputs

**Exam scenarios**: "Application needs consistent, factual responses" → Temperature: 0.2, Top_p: 0.1

### 2.2 Prompt Engineering Mastery

#### Core Prompting Techniques

**1. Zero-Shot Prompting**:
Direct instruction without examples.
```
Classify the sentiment: "This product is amazing!"
```
- **Pros**: Simple, no examples needed
- **Cons**: Less accurate for complex tasks
- **Use when**: Task is straightforward, model has sufficient knowledge

**2. Few-Shot Prompting**:
Provide examples in the prompt.
```
Classify sentiment:
"I love this!" → Positive
"This is terrible" → Negative
"It's okay" → Neutral
"Best purchase ever!" → ?
```
- **Pros**: Significantly better accuracy
- **Cons**: Uses more tokens
- **Use when**: Need better accuracy, task requires specific format

**3. Chain-of-Thought (CoT) Prompting**:
Ask model to show reasoning steps.
```
Problem: If John has 5 apples and gives 2 to Mary, 
then buys 3 more, how many does he have?

Let's solve this step by step:
1) John starts with 5 apples
2) He gives 2 to Mary: 5 - 2 = 3
3) He buys 3 more: 3 + 3 = 6
Answer: 6 apples
```
- **Pros**: Better for complex reasoning, explainable
- **Cons**: More tokens, slower
- **Use when**: Math, logic, multi-step problems

**4. Self-Consistency**:
Generate multiple responses and vote on the answer.
- Run same prompt multiple times (temperature > 0)
- Choose most common answer
- Improves accuracy for reasoning tasks

**5. ReAct (Reasoning + Acting)**:
Combines reasoning with tool use.
```
Thought: I need to find current weather
Action: Call weather_api(city="Seattle")
Observation: 65°F, cloudy
Thought: Now I can answer the user
Answer: It's currently 65°F and cloudy in Seattle
```

#### System Messages vs. User Messages

**System Message**:
- Sets the behavior and context
- Processed first, influences all responses
- Not visible in conversation history
- Example:
```
You are a helpful customer service agent for Contoso Bank. 
Be professional, concise, and always verify customer identity 
before sharing account information.
```

**User Message**:
- The actual user query
- What the user says to the bot

**Assistant Message**:
- Model's response
- Can include previous responses for context

**Exam focus**: Know when to use system vs. user messages for different behaviors.

#### Advanced Prompt Engineering Patterns

**Meta Prompting**:
Ask model to create prompts.
```
Generate a prompt that will help classify customer support tickets 
into: Billing, Technical, or Account.
```

**Prompt Chaining**:
Break complex tasks into steps, chain outputs.
```
Step 1: Extract key entities from text
Step 2: Use entities to search knowledge base
Step 3: Generate answer using retrieved info
```

**Constitutional AI**:
Add rules and constraints to prompts.
```
Answer the question but:
- Never provide financial advice
- Always cite sources
- If uncertain, say "I don't know"
```

**Format Specification**:
Explicitly define output format.
```
Extract entities in this JSON format:
{
  "person": [],
  "organization": [],
  "location": []
}
```

**Exam scenarios**: "Need consistent JSON output" → Use format specification in system message.

### 2.3 Retrieval Augmented Generation (RAG)

#### RAG Architecture

**The Problem**: LLMs have:
- Knowledge cutoff dates
- Hallucination tendencies
- No access to private data

**The Solution**: RAG Pattern
```
User Query
    ↓
1. Generate Embedding (user question)
    ↓
2. Search Vector Store (find relevant docs)
    ↓
3. Retrieve Top-K Documents
    ↓
4. Augment Prompt (inject context)
    ↓
5. Generate Response (grounded in data)
```

#### RAG Implementation with Azure AI Search

**Step 1: Data Preparation**
- Chunk documents (500-1000 tokens per chunk)
- Overlapping chunks (50-100 tokens) for context continuity
- Maintain metadata (source, date, author)

**Step 2: Generate Embeddings**
```python
# Azure OpenAI Embeddings API
response = client.embeddings.create(
    model="text-embedding-ada-002",
    input="Your text to embed"
)
embedding = response.data[0].embedding  # 1536-dim vector
```

**Step 3: Index in Azure AI Search**
- Create search index with vector field
- Vector search configuration (HNSW algorithm)
- Hybrid search: Vector + keyword combined

**Step 4: Query-Time Retrieval**
```python
# 1. Embed user query
query_embedding = get_embedding(user_query)

# 2. Vector search
search_results = search_client.search(
    search_text=user_query,
    vector_queries=[{
        "vector": query_embedding,
        "k": 5,  # Top 5 results
        "fields": "content_vector"
    }]
)

# 3. Extract top documents
context = "\n\n".join([doc["content"] for doc in search_results])

# 4. Augment prompt
prompt = f"""
Use the following information to answer the question.

Context:
{context}

Question: {user_query}

Answer based only on the context provided. If the answer isn't 
in the context, say "I don't have enough information."
"""

# 5. Generate response
response = openai_client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": prompt}
    ]
)
```

**RAG Best Practices**:
1. **Chunking strategy**: Balance between context and specificity
2. **Metadata filtering**: Filter by date, category before vector search
3. **Hybrid search**: Combine vector and keyword for better recall
4. **Reranking**: Use semantic ranker to improve relevance
5. **Citation**: Include source references in responses
6. **Prompt engineering**: Clear instructions to use only provided context
7. **Fallback**: Handle cases with no relevant documents

**Exam focus**: Understand RAG architecture, when to use it, and how to implement with Azure AI Search.

### 2.4 Function Calling (Tool Use)

#### What is Function Calling?

Allows model to call external functions/APIs when needed.

**Use cases**:
- Retrieve real-time data (weather, stock prices, database queries)
- Perform actions (send email, create calendar event)
- Access external systems (CRM, inventory management)

#### Implementation Pattern

**1. Define Function Schema**
```python
functions = [
    {
        "name": "get_current_weather",
        "description": "Get the current weather in a location",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "City and state, e.g., San Francisco, CA"
                },
                "unit": {
                    "type": "string",
                    "enum": ["celsius", "fahrenheit"]
                }
            },
            "required": ["location"]
        }
    }
]
```

**2. Call Azure OpenAI with Functions**
```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "user", "content": "What's the weather in Seattle?"}
    ],
    functions=functions,
    function_call="auto"  # Let model decide when to call
)
```

**3. Handle Function Call Response**
```python
if response.choices[0].finish_reason == "function_call":
    function_name = response.choices[0].message.function_call.name
    function_args = json.loads(
        response.choices[0].message.function_call.arguments
    )
    
    # Execute the actual function
    if function_name == "get_current_weather":
        result = get_current_weather(**function_args)
    
    # Send function result back to model
    second_response = client.chat.completions.create(
        model="gpt-4",
        messages=[
            {"role": "user", "content": "What's the weather in Seattle?"},
            response.choices[0].message,
            {
                "role": "function",
                "name": function_name,
                "content": str(result)
            }
        ],
        functions=functions
    )
```

**Function Calling Best Practices**:
1. **Clear descriptions**: Model relies on descriptions to decide when to call
2. **Parameter validation**: Validate function arguments before execution
3. **Error handling**: Handle function execution errors gracefully
4. **Security**: Never execute arbitrary code, validate all inputs
5. **Fallback**: Handle cases where function isn't available
6. **Prompt hints**: Mention available capabilities in system message

**Function Calling vs. Direct Prompting**:
- **Function Calling**: Structured, validated, safe, model-initiated
- **Direct Prompting**: Flexible, but requires parsing, less reliable

**Exam scenarios**: "Chatbot needs to query inventory database" → Use function calling.

### 2.5 Content Filtering and Safety

#### Azure OpenAI Content Filters

**Filter Categories**:
1. **Hate**: Attacks or discrimination based on protected attributes
2. **Sexual**: Sexually explicit or suggestive content
3. **Violence**: Graphic violence or glorification of violence
4. **Self-Harm**: Content promoting or describing self-injury

**Severity Levels**:
- **Safe**: No harmful content
- **Low**: Minimal harmful content
- **Medium**: Moderate harmful content
- **High**: Severe harmful content

**Filter Configuration Per Category**:
```
Allow: No filtering
Low: Block low, medium, high severity
Medium: Block medium and high severity  
High: Block only high severity
```

**Content Filtering on Input and Output**:
- **Input filtering**: User prompts are checked
- **Output filtering**: Model responses are checked
- Both return HTTP 400 if blocked

**Custom Content Filters**:
- Create custom blocklists
- Regex patterns for specific terms
- Allowlists for exceptions

**Prompt Shields** (Advanced protection):
- **Jailbreak detection**: Attempts to bypass safety
- **Indirect attacks**: Hidden instructions in documents

**Exam focus**: Know default filter settings, when to customize, and how to handle blocked content errors.

#### Responsible AI in Practice

**Monitoring for Abuse**:
- Log unusual patterns (high error rates, repeated blocks)
- Rate limiting per user
- Alert on suspicious activity

**Human in the Loop**:
- Critical decisions require human review
- Escalation workflows
- Confidence thresholds for automation

**Transparency**:
- Disclose AI use to users
- Explain limitations
- Provide feedback mechanisms

### 2.6 Token Management and Optimization

#### Token Basics

**What is a token?**
- Approximate to 4 characters (English)
- "Hello world" ≈ 2-3 tokens
- Tokenization varies by language (Chinese uses more tokens per character)

**Token counting**:
- Use tiktoken library (Python) or token counters
- Count prompt + completion for cost calculation

**Model Token Limits**:
- GPT-3.5 Turbo: 16,385 tokens
- GPT-4: 8,192 or 32,768 tokens
- GPT-4 Turbo: 128,000 tokens
- GPT-4o: 128,000 tokens

**Pricing by Token**:
- Input tokens (prompt) typically cheaper
- Output tokens (completion) more expensive
- GPT-4 ~30x more expensive than GPT-3.5

**Exam focus**: Understand token limits and cost implications.

#### Optimization Strategies

**1. Prompt Optimization**:
- Remove unnecessary words and formatting
- Use abbreviations where context is clear
- Avoid redundant examples

**2. Response Length Control**:
- Set max_tokens to prevent excessive responses
- Use stop sequences to terminate at logical points

**3. Streaming**:
- Stream responses for better UX
- Display tokens as generated
- User can stop if satisfied early

**4. Caching Strategy**:
- Cache common queries/responses
- Reduce redundant API calls
- Implement semantic cache (similar queries → cached response)

**5. Model Selection**:
- Use GPT-3.5 for simpler tasks (10x cost savings)
- Reserve GPT-4 for complex reasoning

**6. Batch Processing**:
- Group related queries when possible
- Process asynchronously for non-real-time needs

**7. Prompt Compression**:
- Summarize long context
- Extract key information only
- Use smaller embeddings for RAG (text-embedding-3-small)

### 2.7 Azure OpenAI Assistants API

#### What are Assistants?

Persistent AI assistants with:
- **Threads**: Conversation sessions with message history
- **Built-in retrieval**: Automatic RAG on uploaded files
- **Code interpreter**: Execute Python code for calculations/data analysis
- **Function calling**: Tool use capabilities

**Difference from Chat Completions**:
- Chat: Stateless, you manage history
- Assistants: Stateful, Azure manages history and tools

#### Assistants Components

**1. Assistant**:
```python
assistant = client.beta.assistants.create(
    name="Data Analyst",
    instructions="You help analyze sales data using Python",
    tools=[{"type": "code_interpreter"}],
    model="gpt-4-turbo"
)
```

**2. Thread** (Conversation):
```python
thread = client.beta.threads.create()
```

**3. Message**:
```python
message = client.beta.threads.messages.create(
    thread_id=thread.id,
    role="user",
    content="Analyze this CSV and show trends"
)
```

**4. Run**:
```python
run = client.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id=assistant.id
)
```

**5. File Upload** (for retrieval):
```python
file = client.files.create(
    file=open("data.csv", "rb"),
    purpose="assistants"
)

assistant = client.beta.assistants.create(
    model="gpt-4-turbo",
    tools=[{"type": "retrieval"}],
    file_ids=[file.id]
)
```

**Use cases**:
- Multi-turn conversational applications
- Data analysis assistants
- Document Q&A with automatic RAG
- Code generation and execution

**Exam focus**: Understand Assistants API capabilities and when to use vs. Chat Completions API.

### 2.8 Fine-Tuning vs. Prompt Engineering

#### When to Use Each

**Prompt Engineering (Start here)**:
- **Pros**: Fast, no training data needed, flexible, instant updates
- **Cons**: Limited customization, uses more tokens, less consistent
- **Use when**: Task can be described in instructions, need flexibility

**Few-Shot Learning**:
- **Pros**: Better than zero-shot, still no training required
- **Cons**: Uses more tokens for examples
- **Use when**: Need better accuracy than zero-shot

**Fine-Tuning**:
- **Pros**: Best task performance, consistent outputs, can use shorter prompts
- **Cons**: Requires training data (50+ examples), time-consuming, less flexible
- **Use when**: 
  - Specific domain language/terminology
  - Consistent output format required
  - Long-term production use case
  - Cost-sensitive (shorter prompts)

**Decision Tree**:
```
Can you describe task in prompt? → Prompt Engineering
Need better accuracy? → Few-shot examples
Still not accurate enough? → Fine-tuning
Need specific behavior/tone? → Fine-tuning
Need consistent format? → Fine-tuning
```

**Exam scenarios**: "Company has 10,000 customer support tickets and needs consistent categorization" → Fine-tuning.

---

## Exam Preparation Tips for Domain 2

### High-Priority Topics:
1. ⭐ RAG architecture and implementation with Azure AI Search
2. ⭐ Prompt engineering techniques (zero-shot, few-shot, CoT)
3. ⭐ Function calling implementation
4. ⭐ Content filtering configuration
5. ⭐ Model selection based on requirements

### Common Exam Scenarios:
- "Chatbot hallucinates and provides incorrect info" → Implement RAG
- "Need to access real-time inventory data" → Function calling
- "Application blocked by content filter" → Review content filter settings, implement retry logic
- "High costs with GPT-4" → Use GPT-3.5 for simpler tasks, optimize prompts, implement caching
- "Inconsistent classification results" → Lower temperature, use few-shot examples, or fine-tune

### Key Concepts to Memorize:
- GPT-4 vs GPT-3.5 characteristics and use cases
- Temperature range (0.0 = deterministic, 2.0 = creative)
- Token limits for each model
- Four content filter categories (Hate, Sexual, Violence, Self-Harm)
- RAG workflow steps
- Function calling components

### Hands-On Practice:
1. Deploy Azure OpenAI resource and model
2. Implement basic chat completion
3. Create RAG solution with Azure AI Search
4. Implement function calling
5. Configure content filters
6. Test different temperature and top_p settings
7. Create embeddings and vector search
8. Build Assistants API application
9. Optimize prompts for token efficiency
10. Implement streaming responses

---

## Quick Reference

### Model Selection Guide:
```
Simple task + Cost-sensitive? → GPT-3.5 Turbo
Complex reasoning? → GPT-4
Need 100k+ context? → GPT-4 Turbo
Images + Text? → GPT-4o
Semantic search? → text-embedding-ada-002
High-volume production? → Provisioned Throughput
```

### Temperature Selection:
```
Classification/Extraction → 0.0 - 0.3
Q&A/Chat → 0.4 - 0.7
Creative writing → 0.8 - 1.5
Brainstorming → 1.5 - 2.0
```

### Common Errors and Solutions:
```
HTTP 429: Rate limited → Retry with exponential backoff, scale up tier
HTTP 400 (content filter): Blocked content → Review input/output, adjust filters
HTTP 401: Authentication failed → Check API key, token expiration
Token limit exceeded → Reduce prompt length, use shorter-context model
```

This is the **fastest-growing domain** on the exam. Stay current with Azure OpenAI updates!
