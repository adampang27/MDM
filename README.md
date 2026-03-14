# Multi-Agent Data Matching (MDM) Pipeline

## Business Context
This pipeline was developed to tackle data entropy in enterprise systems, specifically targeting the resolution of 90K unmatched company records from a 5.5M customer Master Data Management (MDM) system. 

## Overview
Traditional data enrichment tools often suffer from single-source bias, where a single API provider might miss a niche business or return outdated physical addresses. This project implements a consensus-based, multi-agent architecture to resolve ambiguous or incomplete corporate records. It orchestrates concurrent queries across multiple search providers, normalizes the heterogeneous outputs, and synthesizes a final, high-confidence enriched record.

## Tech Stack & Search Providers
* **Language:** Python (Async execution for unblocking batch operations)
* **Interfaces:** CLI for batch processing, Gradio for interactive debugging and human-in-the-loop validation
* **Search APIs:** SerpAPI, Google Knowledge Graph, Tavily, OpenAI (for dynamic query generation)

## Core System Components
* **Multi-Agent Orchestration:** Routes missing records to all four search APIs simultaneously. If a company name is highly ambiguous, an LLM agent generates diverse query variations to improve recall.
* **Registry Boost:** A verification mechanism that anchors search results to known-good data registries, significantly reducing false positives in website and headquarters address fields.
* **13-Category Classification:** Instead of returning a binary "match/no-match", the pipeline categorizes resolution failures into 13 distinct buckets, providing granular explainability for why a specific record could not be confirmed.

## Known Limitations & Future Work
*(Note: This is an Alpha build designed to validate the multi-agent consensus thesis)*
* **API Quotas & Resiliency:** The system is heavily dependent on external API availability. Moving to production requires implementing strict token-bucket rate limiting to respect vendor quotas and handle network timeouts gracefully.
* **Heuristic Scoring:** The current confidence scoring mechanism is heuristic-based. A future roadmap item is transitioning to a probabilistic model trained on labeled internal data.
* **Security & Observability:** The current Gradio UI lacks authentication and is strictly for local debugging. Additionally, the system relies on standard console output; a production deployment would require migrating to structured JSON logging for observability platforms.

## Local Setup & Execution

1. **Clone the repository:**
   bash
   git clone [https://github.com/zerg26/MDM.git](https://github.com/zerg26/MDM.git)
   cd MDM


2. **Install dependencies:**
bash
pip install -r requirements.txt




3. **Environment Variables:**
Create a `.env` file in the root directory and add your API keys (see `.env.example` for the required format):

  * OPENAI_API_KEY=your_key_here
  * TAVILY_API_KEY=your_key_here
  * SERPAPI_API_KEY=your_key_here
  * Add Google KG keys if applicable


4. **Running the Pipeline:**
* **Batch Processing CLI:** `python src/cli.py --input path/to/input.csv`
* **Interactive Debugging UI:** `python app.py` (Launches local Gradio server)

MIT License