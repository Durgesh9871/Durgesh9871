<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:0ea5e9,100:22c55e&height=180&section=header&text=Durgesh%20Singh&fontSize=46&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=AI%20Engineer%20%7C%20Generative%20AI%20%7C%20Agentic%20Systems&descAlignY=62&descSize=18" alt="Durgesh Singh AI Engineer" />
</p>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=23&duration=2600&pause=850&color=38BDF8&center=true&vCenter=true&width=900&lines=Designing+production-minded+AI+systems;RAG+%C2%B7+LangGraph+%C2%B7+Tool+use+%C2%B7+Evaluation;Python+AI+backends+with+clear+data+flows" alt="Animated AI Engineer headline" />
</p>

<p align="center">
  <a href="https://github.com/Durgesh9871"><img src="https://komarev.com/ghpvc/?username=Durgesh9871&label=profile%20views&color=0ea5e9&style=flat" alt="profile views" /></a>
  <a href="https://www.linkedin.com/in/durgesh-singh-293590239"><img src="https://img.shields.io/badge/LinkedIn-connect-0A66C2?logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
  <a href="mailto:devisingh9871686098@gmail.com"><img src="https://img.shields.io/badge/Email-contact-111827?logo=gmail&logoColor=white" alt="Email" /></a>
</p>

## 👋 About me

I build AI applications that connect LLMs, retrieval, tools, and software people can use. I bring **4 years of professional engineering experience** across Python AI workflows and full-stack product engineering with React, Next.js, Node.js, and Express.

- **Target role:** AI Engineer / Generative AI Engineer
- **Currently building:** [Self-Driving Data Analyst](https://github.com/Durgesh9871/ai-practice-projects)
- **Focus:** RAG, agent orchestration, safe tool use, evaluation, AI APIs, and backend systems
- **Based in:** New Delhi, India

## 🤖 Profile assistant: what is actually possible?

Architecture for a real assistant

~~~text
┌─ durgesh-ai / assistant-preview ──────────────────────────┐
│ visitor › What does Durgesh build?                       │
│ agent   › Reliable LLM applications, RAG, and workflows  │
│           with validation, evidence, and observability.  │
│                                                           │
│ visitor › What happens before a tool runs?               │
│ agent   › Authenticate → authorize → plan → validate →   │
│           execute → evaluate → trace.                   │
│                                                           │
│ mode: README preview · live chat: deployment required    │
└───────────────────────────────────────────────────────────┘
~~~

<a href="https://github.com/Durgesh9871/ai-practice-projects"><img src="https://img.shields.io/badge/Explore_the_AI_system-0ea5e9?style=for-the-badge&logo=github&logoColor=white" alt="Explore AI system" /></a>

## 🏗️AI System design: from request to measured answer

The design below is intentionally larger than a simple prompt-to-response chain. It separates the control plane, data plane, policy decisions, and quality feedback loop.

### High-level architecture

~~~mermaid
flowchart TB
    U["User / client"] --> G["API gateway
Auth · rate limit · request ID"]
    G --> O["AI orchestrator
LangGraph state machine"]
    O --> C["Context plane
Schema · metrics · documents"]
    O --> T["Tool plane
SQL · retrieval · approved APIs"]
    O --> P["Policy plane
Guardrails · permissions · budgets"]
    T --> D["Data systems
MySQL · vector store · services"]
    O --> R["Response composer
Evidence · assumptions · status"]
    R --> G
    O --> M["Observability
Traces · metrics · audit events"]
    M --> E["Evaluation loop
Regression · quality · safety"]
    E --> O
~~~

**Request path:** client → gateway → orchestrator → context and tools → policy checks → evidence-backed response.  
**Feedback path:** traces → offline evaluation → regression cases → safer prompts, policies, and graph edges.

### Low-level orchestration

~~~mermaid
flowchart TD
    A["Receive request"] --> B["Validate schema
principal · tenant · budget"]
    B --> C["Classify intent
answer · clarify · refuse"]
    C -->|clarify| Z["Ask focused question"]
    C -->|answer| D["Retrieve permitted context"]
    D --> E["Plan typed actions
metric · filters · tool"]
    E --> F["Check authorization
resource and field scope"]
    F -->|denied| Z2["Safe refusal + trace"]
    F -->|allowed| H["Check exact-result cache"]
    H -->|hit| I["Validate cache scope + freshness"]
    H -->|miss| J["Generate tool call"]
    J --> K["Deterministic guardrail
AST · schema · row/time caps"]
    K -->|fail| L["Bounded repair
max two attempts"]
    L --> J
    K -->|pass| N["Execute read-only tool"]
    N -->|transient| Q["Retry with deadline"]
    Q --> N
    N -->|rows| I2["Normalize evidence"]
    I --> I2
    I2 --> S["Synthesize answer
claims linked to evidence"]
    S --> V["Output validator
format · leakage · citations"]
    V --> W["Response + trace ID"]
    B --> X["Metrics, logs, traces, cost"]
    C --> X
    K --> X
    N --> X
    V --> X
~~~

### Orchestration rules

| Layer | Responsibility | Production control |
|---|---|---|
| **Gateway** | Identity, quotas, request IDs, payload limits | Auth middleware, rate limits, abuse protection |
| **Orchestrator** | State transitions and tool sequencing | Typed state, deadlines, idempotency, bounded retries |
| **Context plane** | Retrieve only relevant approved context | Versioned metadata, tenant filters, token budget |
| **Policy plane** | Decide what may be generated or executed | AST checks, allowlists, least privilege, output validation |
| **Tool plane** | Execute constrained actions | Read-only credentials, timeouts, row limits, sandboxing |
| **Quality plane** | Measure behavior over time | Golden set, adversarial tests, drift checks, release gates |

## 🛡️ Guardrails, cache, evaluation, and observability

<details>
<summary><b>Guardrails and security</b></summary>

- Authenticate before retrieval; derive tenant and permissions from trusted claims.
- Treat user text, retrieved documents, schema comments, and sample values as untrusted.
- Allow only typed tools and approved resources; never expose unrestricted shell or network access.
- Parse generated SQL into an AST; reject DDL, DML, multi-statement payloads, system tables, and disallowed columns.
- Use parameter binding, read-only database principals, row limits, timeouts, and result-size caps.
- Validate the final answer for authorization, unsupported claims, sensitive data, and missing evidence.

</details>

<details>
<summary><b>Cache and performance</b></summary>

- Cache successful bounded results only; never cache denials or internal errors.
- Key results by tenant, permission scope, datasource, normalized query, typed parameters, schema version, metric version, and policy version.
- Re-check authorization before every cache read; a cache hit never bypasses policy.
- Use freshness-aware TTLs and invalidate on schema or metric changes.
- Track cold-cache and warm-cache latency separately; protect the database with admission control and connection-pool limits.

</details>

<details>
<summary><b>Evaluation and release gates</b></summary>

- Dataset: synthetic fixtures, hand-reviewed cases, ambiguity, adversarial prompts, empty results, and large results.
- Quality: parse validity, execution accuracy, result-set correctness, metric/filter correctness, faithfulness, and evidence completeness.
- Safety: policy precision/recall, unsafe execution rate, authorization leakage, prompt-injection resistance, and cache-scope isolation.
- Operations: p50/p95 latency, time to first token, tokens/request, cost/request, retries, timeouts, and provider/database saturation.
- Release gate: publish the dataset, model, prompt, schema, dependency, commit SHA, and cold/warm cache conditions with every result.

</details>

<details>
<summary><b>Observability and failure handling</b></summary>

- Trace every graph node with request ID, duration, model, token usage, cache status, policy result, and tool fingerprint.
- Redact prompts, row values, secrets, and personally identifying data from default logs.
- Retry only transient failures and never reset the request deadline.
- Use circuit breakers for provider/database outages, structured error classes, and explicit partial-result states.
- Turn reviewed failures into regression cases before changing prompts or graph edges.

</details>

## 🧩 Featured work

<table>
<tr>
<td width="50%" valign="top">
<h3><a href="https://github.com/Durgesh9871/ai-practice-projects">Self-Driving Data Analyst</a></h3>
<p>Natural-language analytics with an explicit target architecture for schema retrieval, SQL policy checks, read-only execution, evidence-grounded answers, scoped caching, and evaluation.</p>
<p><b>Current:</b> Python · LangGraph · Gemini<br/><b>Target:</b> MySQL · AST guardrails · provenance · evals</p>
</td>
<td width="50%" valign="top">
<h3>Cold Chain Logistics AI Assistant</h3>
<p>Agent workflow combining fleet telemetry, corridor weather, and logistics SOP documents to produce an operational summary and action plan.</p>
<p><b>Stack:</b> Python · LangGraph · LangChain · SQL Server · Pinecone · Streamlit</p>
</td>
</tr>
</table>

## 🛠️ Technical focus · skill carousel

<details open>
<summary><b>01 · AI engineering and LLM systems</b></summary>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/LangGraph-0F766E?style=for-the-badge&logo=graphql&logoColor=white" alt="LangGraph" />
  <img src="https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=chainlink&logoColor=white" alt="LangChain" />
  <img src="https://img.shields.io/badge/RAG-7C3AED?style=for-the-badge&logo=databricks&logoColor=white" alt="RAG" />
  <img src="https://img.shields.io/badge/Tool%20calling-EA580C?style=for-the-badge&logo=robotframework&logoColor=white" alt="Tool calling" />
</p>

LLM applications · agent orchestration · RAG pipelines · embeddings · hybrid retrieval · prompt design · tool calling · structured outputs · SQL agents · evaluation harnesses · guardrails · caching · tracing

</details>

<details>
<summary><b>02 · JavaScript and frontend systems</b></summary>

<p align="center">
  <img src="https://skillicons.dev/icons?i=js,ts,react,nextjs,html,css,sass,tailwind,figma" alt="JavaScript and frontend skills" />
</p>

JavaScript · TypeScript · React · Next.js · responsive interfaces · SCSS modules · GSAP motion · accessibility · SEO-friendly rendering · API integration · component systems

</details>

<details>
<summary><b>03 · Backend, data, and delivery</b></summary>

<p align="center">
  <img src="https://skillicons.dev/icons?i=nodejs,express,fastapi,mongodb,mysql,docker,git,github,linux" alt="Backend and delivery skills" />
</p>

REST APIs · FastAPI · Node.js · Express · SQL Server · MySQL · MongoDB · Pinecone · Docker · Git · GitHub · authentication · rate limiting · logging · CI/CD fundamentals

</details>

## 📊 Engineering signals

<p align="center">
  <img height="165" src="https://github-readme-stats.vercel.app/api?username=Durgesh9871&show_icons=true&hide_border=true&rank_icon=github&theme=transparent&title_color=38BDF8&icon_color=22C55E&text_color=94A3B8" alt="GitHub statistics" />
  <img height="165" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Durgesh9871&layout=compact&hide_border=true&theme=transparent&title_color=38BDF8&text_color=94A3B8" alt="Top languages" />
</p>

<p align="center">
  <img src="https://streak-stats.demolab.com?user=Durgesh9871&hide_border=true&theme=transparent&ring=38BDF8&fire=F97316&currStreakLabel=38BDF8" alt="GitHub contribution streak" />
</p>

<p align="center">
  <img src="https://github-readme-activity-graph.vercel.app/graph?username=Durgesh9871&bg_color=00000000&color=38BDF8&line=22C55E&point=F97316&area=true&hide_border=true" alt="Contribution activity graph" />
</p>

## 🌐 Connect

<p align="center">
  <a href="https://www.linkedin.com/in/durgesh-singh-293590239"><img src="https://img.shields.io/badge/LinkedIn-Durgesh%20Singh-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
  <a href="mailto:devisingh9871686098@gmail.com"><img src="https://img.shields.io/badge/Email-Say%20hello-111827?style=for-the-badge&logo=gmail&logoColor=white" alt="Email" /></a>
  <a href="https://github.com/Durgesh9871?tab=repositories"><img src="https://img.shields.io/badge/GitHub-Explore%20repositories-181717?style=for-the-badge&logo=github&logoColor=white" alt="Repositories" /></a>
</p>

<p align="center"><i>Building AI features that are inspectable, testable, and useful beyond a demo.</i></p>
