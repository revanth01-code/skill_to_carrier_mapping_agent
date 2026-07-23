# 🎯 Skill-to-Career Mapping Agent

An AI-powered agent built with **LangChain** and **Groq (LLaMA 3.3 70B)** that helps students and job seekers explore industry demand for skills and discover real job openings — all from a single natural language query.

---

## 🚀 What It Does

Ask the agent something like:

> *"What's the demand for Software Engineer role in the industry and show me related job openings in India?"*

And it will:
- 🔍 **Search the web** for skill demand trends, salary insights, and career outlooks (via Tavily)
- 💼 **Fetch live job listings** matching the skill and location (via JSearch / RapidAPI)
- 📋 **Summarize everything** in a clean, readable format

---

## 🧠 Architecture

```
User Query
    │
    ▼
LangChain Agent (LLaMA 3.3 70B via Groq)
    ├── TavilySearch Tool      → Industry demand, salary trends, career insights
    └── search_jobs Tool       → Live job listings from JSearch API (RapidAPI)
```

The agent decides which tools to invoke and in what order, based on the user's query.

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| LLM | `llama-3.3-70b-versatile` via Groq |
| Agent Framework | LangChain |
| Web Search | Tavily Search API |
| Job Listings | JSearch API (RapidAPI) |
| Runtime | Google Colab |

---

## 📦 Installation

Run the following in your Colab notebook cells:

```bash
pip install -U langchain
pip install -qU langchain-groq
pip install -U langchain-tavily
```

---

## 🔑 API Keys Required

This project uses three external APIs. Store them as **Colab Secrets** (the 🔑 icon in the left sidebar):

| Secret Name | Where to Get It |
|-------------|----------------|
| `GROQ_API_KEY` | [console.groq.com](https://console.groq.com) |
| `TAVILY_API` | [app.tavily.com](https://app.tavily.com) |
| `RAPID_API` | [rapidapi.com](https://rapidapi.com) — subscribe to [JSearch](https://rapidapi.com/letscrape-6bRBa3QguO5/api/jsearch) |

---

## ▶️ Usage

1. Open the notebook in Google Colab
2. Add your API keys to Colab Secrets
3. Run all cells in order
4. Modify `user_query` in the last cell to ask about any skill or role:

```python
user_query = "What's the demand for Data Scientist role in the industry and show me related job openings in India"

response = agent.invoke({
    "messages": [{"role": "user", "content": user_query}]
})
```

---

## 📁 Project Structure

```
📦 skill-to-career-agent
 ┣ 📓 A1.ipynb       # Main Colab notebook
 ┗ 📄 README.md      # This file
```

---

## ⚠️ Known Limitations & Fixes

**Token limit error with Groq free tier:**
The JSearch API returns very detailed job descriptions. The free Groq tier has a 12,000 TPM limit. The `search_jobs` tool is capped to return only the **top 3 jobs** with trimmed fields to stay within limits.

```python
jobs = data.get("data", {}).get("jobs", [])[:3]  # capped at 3 to avoid token overflow
```

If you hit rate limits, consider:
- Reducing `max_results` further
- Upgrading to Groq Dev Tier
- Switching to a different LLM provider

---

## 🙌 Contributing

Pull requests are welcome! If you'd like to extend this project, here are some ideas:
- Add filters for job type (remote, full-time, etc.)
- Support multiple locations in one query
- Add a Streamlit or Gradio UI frontend
- Cache job results to reduce API calls

---

## 📄 License

MIT License — free to use, modify, and distribute.
