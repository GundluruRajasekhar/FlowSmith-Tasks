# FlowSmith

Describe a task in plain English. FlowSmith plans it, pulls facts from your own documents (RAG), hands work between four agents, calls tools, remembers past runs, and shows every step. It is both a small automation prototype and a hands-on way to learn LLMs, RAG and agents.

## Try it
1. Open `index.html` in a browser. No install or build step.
2. Click **Example 1** and press **Run automation**.
3. Watch the pipeline move through seven stages and read the run log.
4. Approve the actions when asked, then read the result.
5. Run the same example again and notice the "Remember" stage recalling the first run.

Sample documents (refund policy, shipping rules, support handbook) are preloaded. Add your own by pasting text or uploading `.txt` or `.md` files.

## How the pipeline maps to the topics

| Stage | What happens | Concept |
|---|---|---|
| 1. Understand | Reads the request, picks trigger, format and tools | How LLMs work |
| 2. Instruct | Builds the prompt from your system rules, example and task | Prompt engineering |
| 3. Retrieve | Embeds the request and finds the closest document chunks | Embeddings, vector database, retrieval pipeline |
| 4. Remember | Recalls the closest note from earlier runs | Memory |
| 5. Collaborate | Planner, Researcher, Writer and Reviewer pass work along | Multi-agent systems |
| 6. Act | Runs tools with retries, after your approval if enabled | Tool calling |
| 7. Run | Handles trigger, saves a note, finishes | Autonomous workflows |

The **Learn lab** at the bottom explains all ten concepts, including transformers, and has a token demo and an embedding similarity demo.

## Using a real LLM (optional)
Paste a Claude API key into the field at the top right. The Writer agent will then call the Claude API to draft the result from the retrieved chunks. Without a key, a built-in offline writer is used, so the app always works.

- The key stays in the page and is not saved.
- Only the task, retrieved chunks and one past note are sent to the API.
- Putting a key in a browser is fine for local testing. For anything shared or deployed, move the call to a backend.

## What is real and what is simulated
Real: chunking, embedding, vector search, similarity scores, memory recall and storage (in your browser), the agent sequence, the approval gate, retries, and the optional Claude call.

Simulated: the planner uses keyword rules rather than an LLM, the embeddings are simple word hashes rather than a neural model (so synonyms are not matched), the email, Slack, calendar and web tools do nothing real, and schedule and new-document triggers fire only when you press Run. Tool failures are randomly injected on the first try to demonstrate retries.

## Files
| File | Purpose |
|---|---|
| `index.html` | The whole app (HTML, CSS, JavaScript) |
| `requirements.md` | Functional and non-functional requirements, architecture, acceptance criteria |
| `readme.md` | This file |
| `tasks.md` | Build checklist and roadmap |

## Deploying
Static hosting works: GitHub Pages, Netlify or any file server. Upload `index.html` as the site root.

## Moving to a real automation tool
1. Add a small backend (FastAPI or Node) that holds the API key and calls the LLM.
2. Replace the hashed embeddings with an embeddings API and store vectors in Chroma or pgvector.
3. Let the LLM plan using structured JSON output and tool calling instead of keyword rules.
4. Replace simulated tools with real integrations, each with validation and approval.
5. Add a scheduler and webhooks for real triggers, and persist run history.
