# aurexis-ai-engineer
AI Engineer Take Home Assignment

# AI Engineer Take-Home Assignment: Autonomous Task Planner & Executor

> Updated for 2025. Uses **Next.js 16 (App Router)** and the **AI SDK** (formerly "Vercel AI SDK").

## Overview
Build an autonomous AI agent system that can break down complex user goals into actionable steps, execute them using various tools, and provide real-time progress updates. This project tests your ability to work with AI agents, tool orchestration, streaming responses, and state management.

---

## The Challenge

Create a Next.js application where users can input high‑level goals, and an AI agent autonomously:
1. Breaks down the goal into a step‑by‑step execution plan
2. Executes each step using appropriate tools (web search, calculations, API calls)
3. Streams progress updates in real time
4. Handles failures and adjusts the plan dynamically
5. Presents results in a clear, organized format

### Example User Goals

**Example 1**
```
Research the top 5 AI startups funded in 2024, compare their funding amounts,
and create a summary with key insights
```

**Example 2**
```
Find the weather in Tokyo tomorrow, convert the temperature to Fahrenheit,
and suggest 3 outdoor activities suitable for those conditions
```

**Example 3**
```
Calculate the compound annual growth rate of Apple stock over the last 5 years
and compare it with Microsoft
```

---

## Core Requirements

### 1) Planning Phase
The agent should:
- Parse natural language input from the user
- Generate a list of tasks with clear descriptions
- Identify which tools are needed for each task
- Determine task dependencies (which tasks must complete before others can start)
- Present the plan to the user before execution begins

### 2) Tool Implementation
Implement at least **3 different tool types**:
- **Web Search** — find information online (any search API you prefer)
- **Calculator / Data Processing** — calculations, data analysis, transformations
- **Web Scraper / API Integration** — fetch data from specific sources

(Feel free to add more tools if useful.)

### 3) Execution Engine
Build a system that can:
- Execute tasks respecting dependency order
- Run independent tasks in parallel when possible
- Pass outputs to dependent tasks
- **Stream** progress updates to the UI as tasks execute
- Handle both successes and failures

### 4) Error Handling & Self‑Correction
Your system should:
- Retry with exponential backoff where appropriate
- Adjust the plan when a task fails (skip optional tasks, try alternates)
- Surface clear error messages
- Continue other tasks even when one fails

### 5) User Interface
Show:
- Input area for user goals
- Visual plan and task status (queued, running, succeeded, failed, skipped)
- Real‑time streaming updates (what the agent is doing)
- Final results clearly presented
- Appropriate loading and progress indicators

### 6) State Persistence
Persist state so that:
- Execution state survives page refreshes
- Users can revisit status and results
- Past executions can be reviewed

---

## Technical Requirements

**Must Use**
- **Next.js 16** with App Router  
- **AI SDK** for agent/LLM interactions (Core + UI)  
- **TypeScript**

**Recommended (but optional)**
- Database for persistence (Vercel Postgres, Supabase, etc.)
- Tailwind CSS (v4) for styling
- AI SDK streaming (transport‑based) for real‑time updates
- Server Actions / Route Handlers for tool execution

**System Requirements**
- **Node.js ≥ 20.9** (Next.js 16 drops Node 18)  
- TypeScript ≥ 5.1  
- Modern browsers (Chrome/Edge/Firefox ≥ 111, Safari ≥ 16.4)

---

## Example Flows

> These flows demonstrate agent behavior and UI; values are illustrative.

### Example 1: Multi‑Step Research with Parallel Execution
```
User Input: "Research the top 3 AI companies by revenue in 2024,
compare their market cap, and analyze which has the best growth trajectory"

Planning Phase:
→ Agent generates plan:
  Task 1: Search for top AI companies by revenue in 2024
  Task 2: Extract revenue data for identified companies
  Task 3a: Search for current market cap of Company 1 (depends on Task 2)
  Task 3b: Search for current market cap of Company 2 (depends on Task 2)
  Task 3c: Search for current market cap of Company 3 (depends on Task 2)
  Task 4: Search for historical growth data for all companies (depends on Task 2)
  Task 5: Calculate growth rates and compare trajectories (depends on Tasks 3a–c, 4)
  Task 6: Generate analysis and recommendations (depends on Task 5)

Execution Phase (streamed):
→ Task 1: "Searching for top AI companies by revenue..."
→ Task 1: "Found candidates"
→ Task 2: "Extracting revenue figures..."
→ Task 2: "Complete - 3 companies identified"
→ Tasks 3a–3c: "Running parallel searches for market valuations..."
→ Task 4: "Searching for historical growth data..."
→ Task 5: "Calculating comparative metrics..."
→ Task 6: "Generating final analysis..."

Final Result (example):
╔══════════════════════════════════════════════════════════╗
║ AI Companies Analysis (2024)                             ║
╠══════════════════════════════════════════════════════════╣
║ Company    │ Revenue  │ Valuation │ YoY Growth │ Val/Rev ║
║────────────┼──────────┼───────────┼────────────┼─────────║
║ Company A  │ $X       │ $Y        │ Z%         │ N×      ║
║ Company B  │ $X       │ $Y        │ Z%         │ N×      ║
║ Company C  │ $X       │ $Y        │ Z%         │ N×      ║
╚══════════════════════════════════════════════════════════╝

Key Insights (example):
- Clear leader on both revenue and growth
- Premium revenue multiples across all three
- Balanced risk/return profile favors Company A (example)
```

### Example 2: Dynamic Plan Adjustment with Error Recovery
```
User Input: "Find the current weather in London, Paris, and Tokyo,
then recommend the best city to visit this weekend based on conditions"

Plan:
  Task 1: Get weather for London
  Task 2: Get weather for Paris
  Task 3: Get weather for Tokyo
  Task 4: Compare conditions and recommend (depends on 1–3)

Execution (streamed):
→ Task 1: "✓ London fetched"
→ Task 2: "✗ Rate limited"
→ Agent: "Retrying Paris after others..."
→ Task 3: "✓ Tokyo fetched"
→ Task 2 (retry): "✓ Paris fetched from backup source"
→ Task 4: "Analyzing and recommending..."
→ Result: "Tokyo" (example)
```

### Example 3: Complex Multi‑Tool Workflow
```
User Input: "Calculate how much a $10,000 investment in Tesla stock
would be worth today if I bought it 3 years ago, factor in any stock splits,
and compare it to the S&P 500 instead"

Plan:
  1. Find TSLA price 3 years ago
  2. Find current TSLA price
  3. Check stock splits during the window
  4. Calculate current value (depends on 1–3)
  5. Get S&P 500 level 3 years ago
  6. Get S&P 500 current level
  7. Calculate S&P return (depends on 5–6)
  8. Compare (depends on 4 & 7)

Execution (streamed):
→ Parallel lookups, then calculations, then final comparison
→ Final analysis rendered with tables/charts (example values)
```

---

## Evaluation Criteria

**Functionality (40%)**
- Works reliably for varied goals
- Correct task ordering
- Robust error handling

**Code Quality (25%)**
- Clean, readable, well‑organized
- Solid TypeScript usage
- Separation of concerns

**AI Integration (20%)**
- Effective AI SDK usage (Core + UI)
- Good prompts and agent behavior
- Streaming implemented
- Proper tool orchestration

**UX/UI (10%)**
- Intuitive interface
- Clear loading & error states
- Progress and results are easy to follow

**Edge Cases (5%)**
- Graceful failure handling
- Rate‑limit strategies
- Input validation

---

## Submission Requirements

1) **GitHub Repository** (public)  
   Include:
   - Clear commit history
   - README with:
     - Setup & install instructions
     - Environment variables
     - How to run locally
     - Architecture & key decisions
     - Known limitations / trade‑offs
     - Approximate time spent

2) **Live Deployment**  
   - Deploy on Vercel (or similar)
   - Include the URL in the README
   - Ensure it's configured and functional

3) **Demo Video** (optional but encouraged)  
   - 2–3 minute walkthrough  
   - Show a few example executions  
   - Call out key technical decisions

---

## Getting Started

> The AI SDK is split into **Core** (package `ai`) and **UI** (hooks in `@ai-sdk/react`). Use a provider package for your model of choice (e.g. `@ai-sdk/openai`, `@ai-sdk/anthropic`, `@ai-sdk/google`).
```bash
# Create a new Next.js (App Router) project
npx create-next-app@latest autonomous-agent --typescript --app
cd autonomous-agent

# Install AI SDK (Core + UI) and a provider (choose one or more)
npm install ai @ai-sdk/react @ai-sdk/openai zod
# Or add other providers:
# npm install @ai-sdk/anthropic @ai-sdk/google
```

Create a `.env.local` with your provider keys:
```
# Use the variables that match the provider(s) you install
OPENAI_API_KEY=your_openai_key
ANTHROPIC_API_KEY=your_anthropic_key
GOOGLE_GENERATIVE_AI_API_KEY=your_google_genai_key

# If you add a search provider, include its key (examples):
# TAVILY_API_KEY=your_tavily_key
# SERPAPI_API_KEY=your_serpapi_key
```

Run the dev server:
```bash
npm run dev
```

(If you use Tailwind CSS, follow the official **Tailwind + Next.js** guide and prefer Tailwind **v4**.)

---

## Architectural Guidance (Suggested)

**Agent Loop & Planner**
- Use a planning step (structured output) to generate a DAG of tasks with dependencies
- Store plan + intermediate results in your DB or durable store

**Tools**
- Web search (provider API)
- HTTP fetchers / scrapers / integrations
- Data processing utilities (CAGR, unit conversions, etc.)

**Execution Engine**
- Topologically sort tasks
- Run independent tasks in parallel (`Promise.allSettled`)
- Pass prior results into dependent tasks
- Stream status events to the UI (start, progress, retry, success/fail)

**Streaming UI**
- Use `@ai-sdk/react` hooks to stream assistant output and attach custom events/status updates
- Show per‑task logs with live updates

**Error Handling**
- Exponential backoff + max retries
- Fallback data sources
- Mark optional tasks as skippable
- Don't fail the entire run when one task fails

**Persistence**
- Persist runs, steps, logs, final outputs
- Show history with filtering (goal text, date, status)

---

## Bonus Points

- Visual task tree / DAG
- Pause, resume, or edit plan mid‑run
- Token / cost estimation
- Execution history with search & filters
- Comprehensive test coverage
- Extra agentic features (e.g., tool selection heuristics)

---

## Resources

- **AI SDK (Docs landing)** — [https://sdk.vercel.ai/docs](https://sdk.vercel.ai/docs)
- **AI SDK: Streaming (Foundations)** — [https://sdk.vercel.ai/docs/foundations/streaming](https://sdk.vercel.ai/docs/foundations/streaming)
- **AI SDK: Tool Calling (Core)** — [https://sdk.vercel.ai/docs/ai-sdk-core/tools-and-tool-calling](https://sdk.vercel.ai/docs/ai-sdk-core/tools-and-tool-calling)
- **AI SDK UI: `useChat`** — [https://ai-sdk.dev/docs/reference/ai-sdk-ui/use-chat](https://ai-sdk.dev/docs/reference/ai-sdk-ui/use-chat)
- **Providers: `@ai-sdk/openai`** — [https://ai-sdk.dev/providers/ai-sdk-providers/openai](https://ai-sdk.dev/providers/ai-sdk-providers/openai)
- **Providers: `@ai-sdk/anthropic`** — [https://ai-sdk.dev/providers/ai-sdk-providers/anthropic](https://ai-sdk.dev/providers/ai-sdk-providers/anthropic)
- **Providers: `@ai-sdk/google`** — [https://ai-sdk.dev/providers/ai-sdk-providers/google-generative-ai](https://ai-sdk.dev/providers/ai-sdk-providers/google-generative-ai)
- **Next.js App Router** — [https://nextjs.org/docs/app](https://nextjs.org/docs/app)
- **Tailwind + Next.js Guide** — [https://tailwindcss.com/docs/guides/nextjs](https://tailwindcss.com/docs/guides/nextjs)

---

## FAQs

**Q: Can I use a different AI provider than OpenAI?**  
A: Yes. Install a provider package for the AI SDK (e.g., `@ai-sdk/openai`, `@ai-sdk/anthropic`, `@ai-sdk/google`) and swap models without changing app architecture.

**Q: Is a database required?**  
A: Not for core functionality. In‑memory state is acceptable, but persistence is a strong plus.

**Q: How many goal types should I support?**  
A: At least 3–4 types (search‑based, calculation‑based, multi‑step workflows, etc.).

**Q: What if I can't finish everything in 6–10 hours?**  
A: Prioritize core functionality. Document trade‑offs and how you would extend the system.

**Q: Can I use additional libraries?**  
A: Yes—use what helps build a better solution. Be ready to explain choices and trade‑offs.

---

## Questions?

Open an issue in this repo or reach out via email.

## Submission

When ready, please send:
- GitHub repository URL
- Live deployment URL
- Any additional notes or context

**Deadline:** 17th November, 2025

Good luck! We're excited to see your approach to building autonomous AI agents. 
