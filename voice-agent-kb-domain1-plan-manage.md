# Domain 1: Plan and Manage Azure AI Solutions (20-25%)

## Overview
This domain focuses on the foundational planning, security, governance, and operational aspects of Azure AI solutions. It represents the enterprise readiness and production deployment capabilities required for Azure AI Engineers.

## Key Skill Areas

### 1.1 Select and Configure Azure AI Services

#### Azure AI Foundry (formerly Azure AI Studio)
**What it is**: Unified platform for building, testing, and deploying AI applications with generative AI capabilities.

**Key Components**:
- **AI Foundry Portal**: Web-based development environment
- **Hub**: Resource grouping and project management
- **Projects**: Individual workspaces for AI development
- **Model Catalog**: Access to Azure OpenAI, Meta Llama, Mistral, and other models
- **Prompt Flow**: Visual orchestration tool for AI workflows
- **Evaluation Tools**: Testing and quality assessment
- **Deployment**: Managed endpoints for production

**When to use**:
- Building generative AI applications
- Need for model experimentation and comparison
- MLOps and CI/CD integration
- Enterprise governance requirements
- Multi-model orchestration

**Exam focus**: Understand architecture, when to use Foundry vs individual services, project structure, and deployment options.

#### Individual Azure AI Services vs. Multi-Service Resources

**Individual Services**:
- Separate endpoint and keys per service
- Granular billing and cost tracking
- Service-specific configuration
- Best for: Single-service applications, cost isolation

**Multi-Service Resource**:
- Single endpoint, single key for multiple services
- Consolidated billing
- Simplified management
- Best for: Applications using multiple AI services, simplified authentication

**Exam trap**: Know that not all services are available in multi-service resources (e.g., Custom Vision, QnA Maker require individual resources).

### 1.2 Responsible AI Principles

#### The Six Principles

1. **Fairness**
   - Treat all people fairly
   - Avoid bias in training data and model outcomes
   - Example: Loan approval system shouldn't discriminate based on protected attributes
   - Tools: Fairlearn for bias detection and mitigation

2. **Reliability & Safety**
   - AI systems should perform reliably and safely
   - Handle unexpected situations gracefully
   - Example: Autonomous vehicle must fail safely
   - Tools: Error handling, fallback mechanisms, extensive testing

3. **Privacy & Security**
   - Protect personal data and maintain user trust
   - Comply with regulations (GDPR, HIPAA, CCPA)
   - Example: Healthcare chatbot must encrypt patient data
   - Tools: Azure Key Vault, encryption, differential privacy

4. **Inclusiveness**
   - AI should benefit everyone regardless of abilities
   - Design for diverse users
   - Example: Speech recognition for various accents and speech patterns
   - Tools: Accessibility testing, diverse training data

5. **Transparency**
   - Users should understand how AI makes decisions
   - Document model behavior and limitations
   - Example: Credit scoring should explain decision factors
   - Tools: Model interpretability, explainable AI techniques

6. **Accountability**
   - People must be accountable for AI systems
   - Clear governance and oversight
   - Example: Designated AI ethics review board
   - Tools: Audit trails, governance frameworks

**Exam scenarios**: You'll be asked to identify which principle applies to specific situations or recommend practices to implement a principle.

### 1.3 Security and Authentication

#### Authentication Methods

**1. API Keys (Subscription Keys)**
- Simple HTTP header: `Ocp-Apim-Subscription-Key: your-key`
- Two keys for rotation without downtime
- Key1 and Key2 - rotate regularly
- **Pros**: Easy to implement, good for dev/test
- **Cons**: Less secure, no audit trail, harder to manage at scale

**2. Azure Active Directory (Azure AD / Microsoft Entra ID)**
- OAuth 2.0 token-based authentication
- Bearer token in HTTP header: `Authorization: Bearer token`
- **Pros**: 
  - Integrated with corporate identity
  - Role-Based Access Control (RBAC)
  - Audit logging
  - Conditional access policies
  - Service principals for application identity
- **Cons**: More complex setup

**Service Principal Authentication**:
```
Tenant ID: Azure AD tenant
Application ID: App registration identity
Client Secret: App password
```

**Managed Identities** (preferred for Azure resources):
- **System-assigned**: Tied to Azure resource lifecycle
- **User-assigned**: Independent identity, can be shared across resources
- No credential management needed
- Automatically rotated by Azure

**Exam focus**: Know when to use each method, how to implement managed identities, and security best practices.

#### Network Security

**Virtual Networks (VNet)**:
- Place AI services in VNet for isolation
- Private endpoints for secure access
- No public internet exposure

**Firewall Rules**:
- IP allowlisting
- Range-based restrictions
- Virtual network rules

**Private Link**:
- Private IP address in your VNet
- Traffic stays on Microsoft backbone
- Required for high-security scenarios

**Exam scenarios**: "Your company requires AI services to be accessible only from corporate network" → VNet + Private Endpoint

### 1.4 Data Privacy and Compliance

#### Encryption

**Data at Rest**:
- Automatically encrypted using Microsoft-managed keys
- Customer-managed keys (CMK) option via Azure Key Vault
- BYOK (Bring Your Own Key) for compliance

**Data in Transit**:
- TLS 1.2+ enforced
- HTTPS only connections
- Certificate-based security

**Azure Key Vault Integration**:
- Store API keys, connection strings, certificates
- Access policies for controlled access
- Audit logging for compliance
- Automatic key rotation

**Exam focus**: Understand when CMK is required, how to configure Key Vault integration.

#### Compliance Certifications

Azure AI Services compliant with:
- **GDPR** (General Data Protection Regulation)
- **HIPAA** (Health Insurance Portability and Accountability Act)
- **ISO 27001** (Information Security Management)
- **SOC 2** (Service Organization Control)
- **FedRAMP** (Federal Risk and Authorization Management Program)
- **PCI DSS** (Payment Card Industry Data Security Standard)

**Data Residency**: Choose region for data sovereignty requirements.

**Exam scenarios**: Healthcare application → HIPAA compliance, encryption, audit logging.

### 1.5 Monitoring and Diagnostics

#### Azure Monitor Integration

**Metrics to Monitor**:
- Total Calls / Transactions
- Success Rate
- Errors (4xx, 5xx)
- Latency (response time)
- Token usage (OpenAI services)
- Throttling events
- Data processed

**Alerts Configuration**:
- Threshold-based (e.g., error rate > 5%)
- Action groups for notifications (email, SMS, webhook)
- Auto-scaling triggers

#### Application Insights

**Key features**:
- Distributed tracing for multi-service applications
- Request/response logging
- Dependency tracking
- Exception monitoring
- Custom telemetry
- Live metrics stream

**Log Analytics Workspaces**:
- Kusto Query Language (KQL) for analysis
- Custom dashboards and reports
- Retention policies
- Example query:
```kql
AzureDiagnostics
| where ResourceType == "COGNITIVESERVICES"
| where httpStatusCode_d >= 400
| summarize ErrorCount=count() by OperationName
| order by ErrorCount desc
```

**Exam focus**: Know how to configure diagnostics, write basic KQL queries, set up alerts.

### 1.6 Cost Management and Optimization

#### Pricing Models

**1. Standard (Pay-as-you-go)**
- Per-transaction pricing
- No upfront commitment
- Best for: Variable workloads, development/testing

**2. Commitment Tiers** (Reserved Capacity)
- Monthly or yearly commitment
- Significant discounts (30-50%)
- Overage charged at standard rates
- Best for: Predictable production workloads

**3. Free Tier**
- Limited transactions per month
- Good for learning and prototyping
- No SLA guarantee

**Cost Optimization Strategies**:

1. **Right-sizing**: Choose appropriate SKU (F0, S0, S1, etc.)
2. **Caching**: Cache frequent requests
3. **Batch processing**: Group requests when possible
4. **Request optimization**: Minimize unnecessary API calls
5. **Token management**: Optimize prompt length for OpenAI
6. **Auto-scaling**: Scale based on demand
7. **Regional selection**: Prices vary by region
8. **Commitment tiers**: For predictable workloads
9. **Monitoring**: Track usage patterns
10. **Resource tagging**: Track costs by project/team

**Azure Cost Management Tools**:
- Budgets and spending alerts
- Cost analysis reports
- Resource tags for chargeback
- Recommendations for optimization

**Exam scenarios**: "How do you reduce costs for an AI service with predictable daily traffic?" → Commitment tier.

### 1.7 Scaling and Performance

#### Scaling Options

**Vertical Scaling (Scale Up)**:
- Move to higher pricing tier (S0 → S1 → S2)
- Increased transactions per second (TPS)
- Higher rate limits

**Horizontal Scaling (Scale Out)**:
- Deploy multiple instances
- Load balancer distribution
- Geographic distribution

**Auto-scaling**:
- Based on metrics (CPU, requests, latency)
- Scale rules in Azure Monitor
- Schedule-based scaling

#### Rate Limits and Throttling

**HTTP 429 - Too Many Requests**:
- Indicates rate limit exceeded
- Implement exponential backoff retry logic
- Request rate limit increase if needed

**Best practices**:
- Implement retry logic with backoff
- Use queue-based load leveling
- Monitor throttling metrics
- Request quota increases proactively

**Exam focus**: Recognize throttling errors and remediation strategies.

### 1.8 High Availability and Disaster Recovery

#### Multi-Region Deployment

**Active-Active**:
- Multiple regions serving traffic simultaneously
- Azure Traffic Manager for routing
- Higher availability, load distribution

**Active-Passive**:
- Primary region serves traffic
- Secondary region on standby
- Failover when primary fails

**Azure Traffic Manager Routing Methods**:
- **Priority**: Primary/secondary failover
- **Weighted**: Distribute traffic by percentage
- **Performance**: Route to closest region
- **Geographic**: Route based on user location

#### Backup and Recovery

**Data Backup**:
- Custom models: Export and store in blob storage
- Training data: Redundant storage (LRS, GRS, RA-GRS)
- Configuration: Infrastructure as Code (Bicep, ARM, Terraform)

**RTO and RPO**:
- **Recovery Time Objective (RTO)**: How long to restore service
- **Recovery Point Objective (RPO)**: Acceptable data loss duration

**Exam scenarios**: "Application must have <5 minute downtime annually" → Multi-region active-active with Traffic Manager.

### 1.9 Containers and Edge Deployment

#### Azure AI Service Containers

**When to use containers**:
- Edge deployment (IoT devices, on-premises)
- Data sovereignty requirements
- Disconnected/intermittent connectivity
- Latency-sensitive applications
- Hybrid cloud scenarios

**Available containerized services**:
- Text Analytics (sentiment, key phrases, language detection)
- Face Detection
- Form Recognizer
- Speech (STT, TTS)
- Computer Vision (OCR, spatial analysis)

**Container requirements**:
- CPU and memory specs per container
- Connected or disconnected configuration
- Billing meter endpoint for tracking
- API key from Azure for activation

**Deployment options**:
- Azure Container Instances (ACI)
- Azure Kubernetes Service (AKS)
- Docker on-premises
- Azure IoT Edge

**Exam focus**: Recognize edge deployment scenarios and which services support containers.

### 1.10 Governance with Azure Policy

#### Azure Policy for AI Services

**Common policies**:
- Allowed locations (region restrictions)
- Required tags (cost center, environment)
- Enforce HTTPS only
- Disable public network access
- Require managed identities
- Minimum TLS version
- Customer-managed keys requirement

**Policy vs. RBAC**:
- **RBAC**: Who can do what
- **Policy**: What can be deployed and configured

**Compliance Dashboard**:
- View policy violations
- Remediation tasks
- Compliance percentage

**Exam scenarios**: "Ensure all AI resources are deployed only in US regions and tagged with cost center" → Azure Policy.

### 1.11 DevOps and CI/CD

#### Infrastructure as Code

**ARM Templates**:
```json
{
  "type": "Microsoft.CognitiveServices/accounts",
  "apiVersion": "2023-05-01",
  "name": "[parameters('accountName')]",
  "location": "[parameters('location')]",
  "sku": {
    "name": "S0"
  },
  "kind": "OpenAI",
  "properties": {}
}
```

**Bicep** (preferred, simpler syntax):
```bicep
resource openai 'Microsoft.CognitiveServices/accounts@2023-05-01' = {
  name: accountName
  location: location
  kind: 'OpenAI'
  sku: {
    name: 'S0'
  }
}
```

**Terraform**:
```hcl
resource "azurerm_cognitive_account" "openai" {
  name                = var.account_name
  location            = var.location
  resource_group_name = var.resource_group
  kind                = "OpenAI"
  sku_name            = "S0"
}
```

#### CI/CD Pipeline Components

1. **Source Control**: GitHub, Azure Repos
2. **Build**: Validate IaC templates, run tests
3. **Test Environment**: Deploy to dev/test
4. **Staging**: Pre-production validation
5. **Production**: Blue-green or canary deployment
6. **Monitoring**: Post-deployment validation

**Azure DevOps Pipeline Example**:
- Trigger on code commit
- Run unit tests
- Deploy infrastructure with Bicep
- Deploy application code
- Run integration tests
- Deploy to production with approval gate

**Exam focus**: Understand IaC benefits, deployment strategies, and pipeline stages.

---

## Exam Preparation Tips for Domain 1

### High-Priority Topics:
1. ⭐ Responsible AI principles and scenarios
2. ⭐ Authentication methods (when to use each)
3. ⭐ Monitoring and diagnostics setup
4. ⭐ Cost optimization strategies
5. ⭐ Multi-region high availability

### Common Exam Scenarios:
- "Design a secure AI solution for healthcare" → HIPAA, encryption, VNet, audit logging
- "Application needs 99.99% availability" → Multi-region, Traffic Manager
- "Reduce costs for predictable workload" → Commitment tier
- "Deploy AI service to edge devices with intermittent connectivity" → Containers
- "Ensure all AI resources comply with company standards" → Azure Policy

### Key Concepts to Memorize:
- Six Responsible AI principles
- Difference between managed identity types
- HTTP 429 error = rate limiting
- Free tier limitations
- Services that support containers
- Common KQL queries for diagnostics

### Hands-On Practice:
1. Create multi-service AI resource
2. Configure managed identity authentication
3. Set up Application Insights monitoring
4. Create diagnostic log query in KQL
5. Deploy AI service using Bicep/ARM
6. Configure Private Endpoint
7. Set up cost alert and budget
8. Implement retry logic for throttling

---

## Quick Reference

### Authentication Decision Tree:
```
Need simple dev/test? → API Keys
Enterprise security? → Azure AD + Managed Identity
Azure resource calling AI service? → Managed Identity (preferred)
Cross-tenant access? → Service Principal
```

### Scaling Decision Tree:
```
Rate limited (429 errors)? → 
  → Short term: Retry with backoff
  → Long term: Scale up tier or add regions

High latency? →
  → Multi-region deployment
  → Caching layer
  → Request optimization
```

### Monitoring Decision Tree:
```
Need basic metrics? → Azure Monitor
Need request tracing? → Application Insights
Need custom queries? → Log Analytics + KQL
Need alerting? → Azure Monitor Alerts
```

This domain is **foundational** for the exam. Master these concepts as they appear across all other domains.
