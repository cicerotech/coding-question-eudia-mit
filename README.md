# Coding Challenge: Legal Transcript QA System

## Overview

Submit your solution by September 23, 2025 11 PM EST: https://forms.gle/ibPWz6zbw1s7pmuT9

Build a **Legal Transcript QA System** for deposition review. Your system must:  

- Parse a raw transcript (`transcript.txt`) into structured segments.  
- Answer **time-based natural language queries** efficiently and intelligently
- Add **annotations**: speaker, glossary keywords, entities/dates.  
- [OPTIONAL] Support **combined queries** natural language queries.  

**Notes / Disclaimers:**
- You’re encouraged to use AI tools, but your solution should still be understandable and readable.
- The transcript is **fully fictional**; no real individuals or cases are represented.   
- Focus on **design, efficiency, and correctness**, not just copying the transcript text.  .  
- You are **not required** to use any specific model or API.  
- If you choose to explore model-based approaches, some **commonly accessible open-source models** are available on HuggingFace, etc. 
- You may also use closed-source models or APIs with minimal costs, but please note that any usage fees are **your responsibility** — we will not be reimbursing paid API usage.

## Deliverables

You are expected to submit **two main items**:

### 1. Code
- Runnable end-to-end, with clean and modular code
- Should include:
  - Parsing the transcript
  - QA system (time-based and combined queries)
  - Annotation layer
- Output JSON files:
  - `qa_results.json`
  - `data_structure.json`
  - Optional serialized data structure for the transcript
- **Folder structure example:**
```
legal-qa-project/
├── transcript.txt          # Raw transcript (input)
├── glossary.json           # Legal glossary (JSON)
├── code/
│   ├── parse_transcript.py # Parse transcript into structured nodes
│   ├── qa_system.py        # Time-based QA & query handling
│   ├── annotations.py      # Extract annotations (keywords, entities)
│   └── run_demo.py         # Example script to run parsing + QA + annotations
├── output/
│   ├── qa_result.json      # JSON output of queries (answers, evidence, source)
│   └── data_structure.json # Efficient Data Structure with embedded annotations
└── README.md               # Instructions & how to run project
```


> **Important:** We may not run your code directly. The demo video will be the main proof of functionality. Code evaluation will focus on:
> - Reasonableness of the implementation
> - Data structure design and efficiency
> - QA system robustness and handling of queries  
> - Your code **should be runnable**, but evaluation focuses on **how well your system is designed and demonstrated**, rather than exhaustively executing all possible queries.


### 2. Demo Video
- **Min length:** 30 seconds 
- **Max length:** 1 minute 30 seconds 
- **Purpose:** Show the effectiveness of your QA system in action
- **Focus:** Demonstrate functionality using the **specific queries provided**  
- **Optional:** Add voice-over to highlight your approach or key features  

**IMPORTANT:** The demo is **not meant to take a lot of time**. We are not expecting polished production—spend no more than ~5 minutes recording.

**Tip:** Focus on showing your system working—no need for edits, transitions, or polished production.

**Why the video is required:**  
Since the challenge is open-ended and hard to evaluate solely from code or outputs, the demo allows us to quickly see that your system works as intended and handles the queries correctly.

**Required Demo Queries (Part 2 — Time-Based QA)**

Your system *must demonstrate answers* to these queries in the video:
1. What was said at 00:45?  
2. What was said before the objection?  
3. Summarize everything between 00:30 and 00:50.  
4. What did the witness say after being asked about the contract?  
5. Show all speaker statements mentioning "contract".  
6. List all dates mentioned between 00:30 and 01:00.  
7. List all statements by WITNESS that mention “funding” or “grant.”

> If you **did not implement Part 4**, the last three queries (5–7) can be replaced with **additional Part 2-style queries**:
- List everything EXPERT 1 said between 00:40 and 01:20.  
- Show all statements by WITNESS after 05:00.  
- Summarize what was said regarding funding milestones.  


## Provided Materials
- **Raw Transcript** (`transcript.txt`) — lightly messy, includes timestamps and speakers  
- **Legal Glossary** (`glossary.json`): `["contract", "agreement", "objection", "liability"]`  


## Project Parts

### Part 1 — Parsing & Structuring (30–40 min)

Parse the transcript file and create a **structured data representation**. Each segment/node should include:

- `start_s` — timestamp in seconds  
- `speaker` — who is speaking  
- `text` — the transcript content  
- `annotations` — optional initially; can include keywords/entities  

👉 Store the transcript in a way that supports fast and efficent time-based lookup.

💡 Hint: Think about what happens if you need to quickly find “everything between 00:30 and 00:50.”

---
### Part 2 — Time-Based Question Answering (45–60 min)
Your QA system should answer queries intelligently, not just consume the entire transcript. Queries may involve:

Answer queries such as:
- “What was said at 00:45?”  
- “What was said before the objection?”  
- “Summarize everything between 00:30 and 00:50.”  

**Each query must produce JSON:**
```json
{
  "query": "What was said at 00:45?",
  "answer": "March 2020",
  "evidence": "WITNESS: March 2020",
  "source": {
    "start_s": 45,
    "end_s": 46,
    "speaker": "WITNESS"
  }
}
```

💡 Hints / Guidance:
- Your system should avoid ingesting the entire transcript blindly unless absolutely required.
- Optimize for efficiency: try to use as little context as necessary to answer each query.
- Think about modular tools or functions your system can call to retrieve, summarize, or filter segments.
- Decide carefully whether answers should be extracted directly from the transcript or generated/derived.
- Aim for fast lookups and concise answers while preserving accuracy and relevant evidence.

### Part 3 — Annotation Map (30–40 min)
Create multi-layer annotations for each transcript segment/node. Each segment/node should include:

- **keywords** — relevant terms based on the glossary
- **entities** — optional important information (dates, amounts, milestones, names) you identify from the text

---

#### Example Segment

```json
{
  "text": "The grant was awarded on January 15th of last year.",
  "start_s": 305,
  "end_s": 310,
  "speaker": "WITNESS",
  "annotations": {
    "keywords": ["grant"],
    "entities": ["January 15th"]
  }
}
```

Notes:
- Apply keywords and entities per segment.
- Focus on terms from the glossary; for entities, you can extract dates, numbers, project milestones, or names.
- The data structure should still support efficient traversal and time-based queries.
- The goal is to make the transcript searchable and queryable, not to exhaustively identify every entity.

### Part 4 — Creative Query Layer (Optional, 30–40 min)
This part is *optional* and intended for those who want to demonstrate advanced queries combining annotations and time.

Example creative queries (optional):
- "Find all WITNESS utterances mentioning 'contract'."
- "List all dates mentioned between 00:30 and 01:00."
- "Show LAWYER’s objections and statements immediately before them."

**Requirements:**
- Queries may combine multiple filters: speaker + keyword + time range.
- Output JSON should follow the same structure as Part 2.

**Guidance / Hints:**
- Think about how to **interpret and plan query execution** before extracting data.   
- Combine multiple filters (speaker, keyword, time) in a structured way, rather than scanning everything blindly.  
- Leverage annotations from Part 3 (keywords, entities).


## Hugging Face Models — Optional Guidance

Feel free to explore **public models on Hugging Face** to help with your QA system. This is **not the only way** and you are free to use other methods. You don’t need to use a specific one — these are just examples that worked for us:   

**Steps to get started:**  
1. **Create a free Hugging Face account**  
   - Sign up at [huggingface.co](https://huggingface.co).  

2. **Generate a token**  
   - Go to your account settings → Access Tokens → Create new token  
   - Scope: `read` (only needed to access public models) [might need to play around with the settings based on the model] 

3. **Select a model**  
   - Some public models we successfully tested locally:  
     - [LLaMA 3.1 8B](https://huggingface.co/meta-llama/Llama-3.1-8B)  
     - [Gemma 2B](https://huggingface.co/google/gemma-2b)  
     - [TinyLLaMA 1.1B Chat](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0)  

4. **Download and run locally**  
   - You can download the model weights and run on CPU 
   - This avoids sharing any API keys  

**Tips / Best Practices:**  
- Ensure your token only has **read access**  
- You **do not need to share your API key** in submissions  
- This guidance is optional — your system can also work without any external model  

> ⚠️ Disclaimer: This is just an example workflow that worked for us. You may choose different models, configurations, or workflows.

