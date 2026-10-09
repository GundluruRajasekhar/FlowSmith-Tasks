# FlowSmith: Tasks

Legend: [x] done, [ ] to do

## Phase 1: Working demo (single index.html)
- [x] Layout: task input, pipeline rail, stage detail, run log, result, learn lab
- [x] Seven-stage pipeline with active and done states
- [x] Sample documents preloaded
- [x] Add documents by paste and file upload; remove documents
- [x] Chunking, hashed embeddings, cosine search, top-k setting
- [x] Keyword planner for trigger, tools and format
- [x] Editable system prompt and few-shot example
- [x] Agents: Planner, Researcher, Writer, Reviewer
- [x] Offline writer for summary, checklist and JSON
- [x] Simulated tools with retry on failure
- [x] Approval gate before actions
- [x] Long-term memory in browser storage with recall by similarity
- [x] Optional Claude API call for the Writer with offline fallback
- [x] Learn lab: ten concept explainers, token demo, similarity demo
- [x] Responsive layout, focus styles, reduced-motion support
- [x] requirements.md, readme.md, tasks.md

## Phase 2: Test and polish
- [ ] Open in Chrome, Firefox and Safari and fix any differences
- [ ] Check layout at 360, 768 and 1280 px widths
- [ ] Run each example three times and confirm logs and results make sense
- [ ] Test with a valid API key, an invalid key, and no network
- [ ] Test uploading a large file (over 100 KB) and an empty file
- [ ] Add a "download result" button
- [ ] Add export and import of documents and memory as JSON
- [ ] Keyboard-only walkthrough of a full run

## Phase 3: Make it smarter
- [ ] Let the LLM do the planning with structured JSON output
- [ ] Use real tool calling (tool definitions sent to the model, results returned)
- [ ] Add a reranking step after retrieval
- [ ] Show an attention or token-probability view in the Learn lab
- [ ] Prompt versions with side-by-side comparison of results
- [ ] Add a small evaluation set (question, expected source) and a score

## Phase 4: Make it real
- [ ] Backend (FastAPI or Node) that holds the API key
- [ ] Neural embeddings and a real vector database (Chroma or pgvector)
- [ ] Real integrations: email, Slack, calendar, with input validation
- [ ] Scheduler and webhook triggers; persistent run history
- [ ] User accounts and per-user knowledge bases
- [ ] Prompt-injection checks on retrieved documents
- [ ] Rate limits, cost tracking and per-run step limits
- [ ] Deploy and add monitoring

## Phase 5: Submission and demo
- [ ] Deploy index.html to GitHub Pages
- [ ] Record a 2-minute walkthrough: describe, run, approve, show memory on second run
- [ ] Write a short project blurb from readme.md
- [ ] Final check that all four files are in the repository root
