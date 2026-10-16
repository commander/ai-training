# Addendum (2025–2026): AI and LLM Courses for Engineers and Architects

**Created/updated:** 2026-10-10

This file extends `ai-courses.md`. It adds courses, lecture series, academies, and books from 2025 and 2026. The check date is 2026-10-10. Each item is from a university, a major AI lab, DeepLearning.AI, Hugging Face, or a known instructor or publisher. We checked each title, provider, and URL against the provider's own page or a reliable source. Items marked **[2026]** started or had a new edition in 2026.

---

## 1. LLM Mastery (Foundations + Architecture)

**Stanford CS336: Language Modeling from Scratch** **[2026]**
- Provider: Stanford University (Tatsunori Hashimoto, Percy Liang)
- Date: Spring 2026 offering (Spring 2025 and Spring 2024 are archived on the same site)
- Cost: Free (lectures on YouTube, assignments public)
- URL: https://cs336.stanford.edu/
- You build a language model across the full stack: data, tokenizer, transformer, GPU kernels, parallel training, scaling laws, inference, and alignment. It gives more depth on internals than the Udemy and Coursera courses in the original list. It needs strong Python, PyTorch, and math skills.

**Stanford CS25: Transformers United V6** **[2026]**
- Provider: Stanford University (Steven Feng, Karan P. Singh, Michael C. Frank, Christopher Manning)
- Date: Spring 2026 (March 30 – June 3, 2026)
- Cost: Free (public Zoom stream and recordings)
- URL: https://web.stanford.edu/class/cs25/
- This is a seminar series. Each week, a researcher presents current transformer work, for example state space models, large-scale training, and production serving. Use it to keep up with new architectures after you finish a foundation course.

**Hugging Face LLM Course (was the NLP Course)**
- Provider: Hugging Face
- Date: April 2025 (renamed and extended)
- Cost: Free, with certificate
- URL: https://huggingface.co/learn/llm-course
- Hugging Face renamed the NLP Course to the LLM Course. New chapters cover LLM fine-tuning and reasoning models in the style of DeepSeek R1. It teaches the Transformers, Datasets, and Tokenizers libraries, so it supports the SLM work in the original list.

**Post-training of LLMs**
- Provider: DeepLearning.AI (instructor: Banghua Zhu, University of Washington and Nvidia)
- Date: 2025
- Cost: Free during the DeepLearning.AI platform beta
- URL: https://www.deeplearning.ai/short-courses/post-training-of-llms/
- This short course (about 1 hour 16 minutes) explains three post-training methods: supervised fine-tuning (SFT), Direct Preference Optimization (DPO), and online reinforcement learning. It adds the theory behind the QLoRA fine-tuning in the original list.

---

## 2. Agentic AI (RAG, MCP, Multi-Agent Systems)

**Agentic AI (Andrew Ng)**
- Provider: DeepLearning.AI (Andrew Ng)
- Date: October 2025
- Cost: Paid (DeepLearning.AI Pro, about USD 25–30 per month)
- URL: https://www.deeplearning.ai/courses/agentic-ai/
- The course has 5 modules and takes about 10 hours. It teaches four agent design patterns in plain Python: reflection, tool use, planning, and multi-agent collaboration. It uses no framework, so you learn the patterns before you use LangGraph or CrewAI.

**Hugging Face Agents Course**
- Provider: Hugging Face
- Date: February 2025 (launch); bonus units added later
- Cost: Free, with certificate
- URL: https://huggingface.co/learn/agents-course
- It covers agent basics, then smolagents, LlamaIndex, and LangGraph. Bonus units cover fine-tuning for function calling and agent observability and evaluation. The final unit is a benchmark task.

**Hugging Face MCP Course**
- Provider: Hugging Face, in partnership with Anthropic
- Date: May 2025 (launch); still in update in October 2026
- Cost: Free, with certificate
- URL: https://huggingface.co/learn/mcp-course
- It teaches the Model Context Protocol (MCP) in theory and practice. You build an end-to-end MCP application and then deploy one on Hugging Face. It is a vendor-supported alternative to the Udemy MCP courses in the original list.

**MCP: Build Rich-Context AI Apps with Anthropic**
- Provider: DeepLearning.AI and Anthropic (instructor: Elie Schoppik)
- Date: 2025
- Cost: Free during the DeepLearning.AI platform beta
- URL: https://www.deeplearning.ai/short-courses/mcp-build-rich-context-ai-apps-with-anthropic/
- This short course (about 1 hour 38 minutes) shows the MCP client-server architecture. You build an MCP server with tools, resources, and prompts. Then you build a host application with many MCP clients.

**Hugging Face Context Course** **[2026]**
- Provider: Hugging Face
- Date: April 2026 (launch); updated to October 2026
- Cost: Free
- URL: https://huggingface.co/learn/context-course
- This course is about context engineering for code agents such as Claude Code, Codex, and OpenCode. Units cover skills, MCP, plugins, subagents, and hooks. A bonus unit shows how to build your own small agent harness.

**5-Day AI Agents Intensive Course with Google**
- Provider: Kaggle and Google
- Date: November 10–14, 2025 (live); now a self-paced Kaggle Learn guide
- Cost: Free
- URL: https://www.kaggle.com/learn-guide/5-day-agents
- Each day has a technical whitepaper and two codelabs. The codelabs use Gemini and the Google Agent Development Kit (ADK). Topics are agent architecture, tools, memory, evaluation, and the move from prototype to production.

---

## 3. Building AI-Powered Applications

**Claude Code: A Highly Agentic Coding Assistant**
- Provider: DeepLearning.AI and Anthropic (instructor: Elie Schoppik)
- Date: August 2025
- Cost: Free
- URL: https://www.deeplearning.ai/short-courses/claude-code-a-highly-agentic-coding-assistant/
- This short course (about 1 hour 50 minutes) shows how to use Claude Code to explore, test, refactor, and debug a codebase. It connects Claude Code to MCP servers such as Playwright and Figma. The projects include a RAG chatbot codebase.

**Claude Academy (Anthropic official courses)** **[2026]**
- Provider: Anthropic
- Date: 2026 (current site; `anthropic.com/learn` now redirects here)
- Cost: Free
- URL: https://academy.claude.com/
- This is the official Anthropic training site. It has tracks for Claude Code and the Claude Platform (API, Console, MCP), and AI Fluency courses such as "AI capabilities and limitations." Use it for current, vendor-correct API and agent guidance.

**OpenAI Academy**
- Provider: OpenAI
- Date: March 2025 (launch); course tracks updated in 2026
- Cost: Free
- URL: https://academy.openai.com/
- The "Build with AI" track teaches Codex and the OpenAI API. Most other tracks are for non-technical users. It is the official source for the OpenAI tools in the original list, such as the OpenAI Agents SDK.

---

## 4. Deploying and Evaluating LLM Systems

**AI Evals For Engineers & PMs**
- Provider: Maven (Hamel Husain, Shreya Shankar)
- Date: Current cohort October 10 – November 21, 2026 **[2026]**; earlier cohorts in 2025
- Cost: Paid (USD 4,200)
- URL: https://maven.com/parlance-labs/evals
- This live course teaches error analysis, LLM-as-judge evaluators, evals in CI/CD, red-teaming, and cost-accuracy trade-offs. The original list has no course on evaluation. This repo has the syllabus in `AI Evals For Engineers & PMs — Syllabus.pdf`.

---

## Books (adds to Phase 4)

**AI Engineering: Building Applications with Foundation Models**
- Author / publisher: Chip Huyen, O'Reilly Media
- Date: January 2025
- Cost: Paid (also on the O'Reilly platform, which the ACM bundle includes)
- URL: https://bookshop.org/p/books/ai-engineering-building-applications-with-foundation-models-chip-huyen/d1a4892f5eb5134f (ISBN 9781098166304)
- This book follows "Designing Machine Learning Systems," which the original plan lists. It covers evaluation, prompt engineering, RAG, agents, fine-tuning, and inference optimization for foundation models. It is a good architecture-level book for the leadership track.

**Build a Reasoning Model (From Scratch)** **[2026]**
- Author / publisher: Sebastian Raschka, Manning
- Date: 2026 (print release; retailers list mid-2026)
- Cost: Paid
- URL: https://www.manning.com/books/build-a-reasoning-model-from-scratch
- The book shows how to add reasoning to an LLM in code. It covers inference-time scaling and reinforcement learning methods. It pairs well with CS336 and the post-training course above.

---

## Outdated or changed courses

**Generative AI with Large Language Models (Coursera, DeepLearning.AI and AWS)**
- Status: Still live (4.8 rating, about 449,000 enrolled).
- URL: https://www.coursera.org/learn/generative-ai-with-llms
- Note: The course is from 2023. It does not cover agents, MCP, reasoning models, or modern post-training (DPO, online RL). Use it only for the certificate. Add the Hugging Face LLM Course or "Post-training of LLMs" for current content.

**Hugging Face NLP Course (not in the original list, but often linked)**
- Status: Renamed to the LLM Course in April 2025.
- Note: Old links to the NLP Course now go to `https://huggingface.co/learn/llm-course`. Update your bookmarks.

**Anthropic courses at `anthropic.com/learn`**
- Status: Moved. The URL now redirects to Claude Academy at `https://academy.claude.com/`.

**Udemy course titles with a year (for example "2026" or "2027 Bootcamp")**
- Note: Udemy authors change the year in a title to show that the course is current. The year in the title is not a release date. Before you buy, check the "Last updated" date and the curriculum for current topics (MCP, agent SDKs, evals).
