# FlowSmith: Requirements

## 1. Purpose
FlowSmith turns a plain-English task into a working AI automation and shows every step, so users both get a result and learn how LLMs, RAG and agents fit together.

## 2. Users
- Students and developers learning LLM concepts by seeing them run.
- Small teams who want to prototype an automation (for example, drafting support replies from their own policy documents) before building it for real.

## 3. Scope of concepts
| Area | Concept | Where it appears in the app |
|---|---|---|
| Foundations | How LLMs work | Understand stage, Learn lab |
| Foundations | Prompt engineering | Instruct stage (editable system prompt, example, output format) |
| Foundations | Transformers | Learn lab, token demo |
| RAG | Embeddings | Retrieve stage, similarity demo |
| RAG | Vector databases | In-memory vector store searched by cosine similarity |
| RAG | Retrieval pipelines | Chunk, embed, search, top-k, cite |
| Agents | Tool calling | Act stage with simulated tools |
| Agents | Memory | Remember stage (short-term run context, long-term saved notes) |
| Agents | Multi-agent systems | Planner, Researcher, Writer, Reviewer |
| Agents | Autonomous workflows | Run stage: triggers, approval gate, retries |

## 4. Functional requirements
### Task input
- FR-1 The user can describe an automation in free text and pick from example prompts.
- FR-2 The user can choose a trigger (detect, manual, schedule, new document) and a result format (detect, summary, checklist, JSON).
- FR-3 The user can require approval before actions run.

### Knowledge base (RAG)
- FR-4 The user can add documents by pasting text or uploading .txt/.md files.
- FR-5 Documents are split into chunks (about 50 to 60 words) and embedded in the browser.
- FR-6 Retrieval returns the top-k chunks (k from 1 to 6) with similarity scores and source names.
- FR-7 The user can remove a document, which removes its chunks.
- FR-8 Sample documents are preloaded so the app works on first open.

### Pipeline
- FR-9 The pipeline has seven stages in this order: Understand, Instruct, Retrieve, Remember, Collaborate, Act, Run.
- FR-10 Each stage can be selected to show a plain-language explanation and its settings.
- FR-11 During a run, the active stage is highlighted and finished stages are marked done.
- FR-12 The system prompt and few-shot example are editable and used in the next run.

### Agents and tools
- FR-13 Four agents run in order: Planner, Researcher, Writer, Reviewer, each logged separately.
- FR-14 The Reviewer flags drafts with no sources, no citations, or weak matches.
- FR-15 Tools are chosen from keywords in the request: search_kb, send_email, create_task, notify_slack, create_event, web_search.
- FR-16 All tools except search_kb are simulated and labeled as such.
- FR-17 A failed tool call is retried up to 3 times and each attempt is logged.

### Memory
- FR-18 After each run a short note is saved to long-term memory (browser storage).
- FR-19 At the start of a run, the closest past note is recalled if similarity passes a threshold.
- FR-20 The user can clear all memory.

### LLM integration
- FR-21 Without an API key, a built-in offline writer produces the result.
- FR-22 With a Claude API key, the Writer step calls the Claude API. If the call fails, the app falls back to the offline writer and logs why.

### Output and learning
- FR-23 Results can be copied to the clipboard.
- FR-24 A Learn lab explains all ten concepts and includes a token demo and an embedding similarity demo.

## 5. Non-functional requirements
- NFR-1 Runs from a single index.html with no build step; works offline except for web fonts and the optional API call.
- NFR-2 Documents never leave the browser. Only the optional Claude call sends text (the task, retrieved chunks and past note) to the API.
- NFR-3 The API key is read from the input field, held only in the page, and never saved.
- NFR-4 Usable on screens from 360 px wide up; keyboard accessible with visible focus; respects reduced-motion settings.
- NFR-5 A full simulated run finishes in under 15 seconds.
- NFR-6 Text contrast meets WCAG AA.

## 6. Architecture (current version)
```
Browser (index.html)
 ├─ UI: input panel, pipeline rail, stage detail, log, result, learn lab
 ├─ Embedding: hashed word and bigram vectors (256 dims), cosine similarity
 ├─ Vector store: in-memory array of chunks
 ├─ Run engine: stage runner, agents, tool executor with retry, approval gate
 ├─ Memory: localStorage notes recalled by similarity
 └─ Optional: fetch to Claude Messages API for the Writer step
```

## 7. Data flow for one run
1. Read the task, trigger, format and approval setting.
2. Plan: detect tools, trigger and format from the text.
3. Assemble the prompt (system rules, example, task).
4. Embed the task and retrieve the top-k chunks.
5. Recall the closest long-term note.
6. Planner, Researcher, Writer and Reviewer run in order.
7. If approval is on, wait for the user, then run tools with retries.
8. Save a note to memory and show the result.

## 8. Security and safety
- Simulated tools cannot send anything. Real tools in a future version must validate inputs, use least-privilege credentials and require approval for anything that sends, spends or deletes.
- Retrieved documents are treated as data, not instructions. Future versions should add prompt-injection checks.
- A browser-held API key is acceptable for local testing only. Production use needs a backend proxy.

## 9. Acceptance criteria
- Opening index.html and pressing Run on an example produces a log, a sourced result, and a memory note without any setup.
- Asking about refunds retrieves the refund policy first; asking about shipping retrieves the shipping rules first.
- Rejecting the approval leaves no tool calls in the log.
- Adding a valid API key changes the Writer log line and the result text.
- A bad API key falls back to the offline writer without breaking the run.
- The page has no horizontal scroll at 360 px width.

## 10. Out of scope for this version
Real email, Slack or calendar integrations; real schedulers or webhooks; user accounts; neural embeddings; reranking models; multi-user storage.
