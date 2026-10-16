**“Context rot”** means: the model still *fits* the text in its window, but quality drops as the input gets longer — even when the hard fact is still in there.

Below is the stack of sources that actually matter, from foundational academic work to the report that named the term.

### The papers you should read first

**1. Lost in the Middle (the original, peer-reviewed finding)**  
Liu, Lin, Hewitt, Paranjape, Bevilacqua, Petroni, Liang. *Lost in the Middle: How Language Models Use Long Contexts.* TACL, 2023.  
https://arxiv.org/abs/2307.03172  

This is the authentic academic root. Models use facts near the **start** and **end** of a long prompt much better than facts in the **middle** (the U-shaped curve). Also: longer context hurts even when the answer is present. Everything later is a sequel to this.

**2. Context Rot (the report that popularized the name)**  
Hong, Troynikov, Huber. *Context Rot: How Increasing Input Tokens Impacts LLM Performance.* Chroma technical report, July 2025.  
https://research.trychroma.com/context-rot  
Code: https://github.com/chroma-core/context-rot  

This is the most cited *named* study. 18 frontier models (Claude, GPT, Gemini, Qwen). Finding: **every model gets worse as you add tokens**, including on child-simple tasks; distractors make it worse; a shuffled haystack often beat a coherent one. Not a conference paper — an industry report with a public repo — but it is the source people mean when they say “context rot.”

**3. NoLiMa (harder than needle-in-a-haystack)**  
Often cited next to Chroma. Tests whether models can use a fact that is *semantically* related, not a copy-paste needle. Advertised windows look fine on NIAH; effective length collapses on this. Good companion to (1) and (2).

**4. Diagnosing and Mitigating Context Rot in Long-horizon Search**  
Xia, Wang, Huang, Liu. arXiv:2606.29718, 2026.  
https://arxiv.org/abs/2606.29718  

Best *follow-on academic paper* that uses the term on purpose. In long agent/search traces they find **premature termination**: the model gives up or answers unsure/wrong *before* the window is full, and that rate rises with context length. Also compares context-management fixes.

**5. Classifier Context Rot**  
Martin & Roger (Anthropic Fellows / Anthropic). *Classifier Context Rot: Monitor Performance Degrades with Context Length.* arXiv:2605.12366, 2026.  
https://arxiv.org/html/2605.12366  

Most “authentic” safety-angle paper: frontier models used as **monitors** miss dangerous actions 2×–30× more often when those actions sit after hundreds of thousands of benign tokens. Directly cites Hong et al. for the name.

### Strong related work (not always using the phrase)

- **Needle-in-a-Haystack** — Greg Kamradt (2023). The popular retrieval stress test. Easy needles can still look “solved” while real tasks rot.  
- **RULER** — Hsieh et al. (2024). Better than NIAH for “do you actually use long context?”  
- **LongMemEval** — used heavily in the Chroma report for focused vs full-context prompts.

### Good articles (not primary research)

- Timothy B. Lee, *Understanding AI*: “Context rot: the emerging challenge…” — clear public explanation of Chroma + NoLiMa numbers.  
  https://www.understandingai.org/p/context-rot-the-emerging-challenge  
- TMLS write-up *Context Rot: Why Long-Context LLMs Degrade* — tries to split **position bias** (lost-in-the-middle) from **length degradation** (rot as N grows). Useful framing, not a replacement for the papers.  
  https://tmls.nyc/research/context-rot-mechanistic  

### One naming trap

“Context rot” is also used for **stale agent config files** (e.g. old `CLAUDE.md`). That is a different idea (arxiv 2606.09090). For LLM quality vs input length, stick to Liu 2023 + Chroma 2025 + Xia 2026 + Martin & Roger 2026.

**Read in this order:** Liu et al. → Chroma report → Xia et al. → Classifier Context Rot. That is the authentic line, not blog folklore.