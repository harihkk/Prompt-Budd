# Prompt-Budd

A Chrome extension + FastAPI backend that enhances your prompts in real-time across ChatGPT, Claude, Gemini, DeepSeek, Grok, and Perplexity. It scores prompt quality, detects PII, suggests better prompt templates, and recommends the best LLM for your use case.

## Features

- **Prompt Scoring** - Rates your prompt as low/medium/high quality using Gemini with OpenAI fallback
- **PII Detection** - Regex-based detection for credit cards, SSNs, emails, phone numbers, API keys, passwords, and more
- **Prompt Enhancement** - Rewrites vague prompts into structured templates (short or descriptive mode)
- **LLM Recommendation** - Analyzes your last 5 prompts and suggests the best model (ChatGPT, Claude, Gemini, DeepSeek, etc.)
- **Session Summary** - Generates a context summary of recent prompts using Groq
- **MCP Server** - Expose prompt enhancement as an MCP tool for Cursor, Claude Desktop, LangChain, and OpenAI agents

## Architecture

```
Prompt-Budd/
├── backend/
│   ├── main.py                    # FastAPI app with all endpoints
│   ├── detect_pii.py              # PII detection and masking (regex-based)
│   ├── prompt_classifier.py       # LLM recommendation based on prompt history
│   ├── prompt_score.py            # Prompt quality scoring (Gemini + OpenAI fallback)
│   ├── prompt_template_desc.py    # Descriptive prompt enhancement (Groq + OpenAI fallback)
│   ├── prompt_templates_short.py  # Short prompt template generation (OpenAI)
│   └── summary_gen.py             # Session summary generator (Groq + OpenAI fallback)
├── chrome-extension/
│   ├── manifest.json              # Chrome extension manifest v3
│   ├── background.js              # Extension toggle handler
│   ├── content.js                 # Main UI injection and logic
│   └── icons/                     # Extension icons
├── mcp-server/
│   ├── server.py                  # FastMCP server with prompt enhancement tools
│   └── README.md                  # MCP integration guide
├── .github/workflows/
│   ├── deploy.yml                 # CI/CD for backend to Cloud Run
│   └── deploy-mcp.yml             # CI/CD for MCP server to Cloud Run
├── Dockerfile                     # Backend container
├── Dockerfile.mcp                 # MCP server container
└── requirements.txt
```

## Setup

1. **Clone the repo**
   ```bash
   git clone https://github.com/harihkk/Prompt-Budd.git
   cd Prompt-Budd
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Create a `.env` file** with your API keys
   ```
   OPENAI_API_KEY=your_openai_key
   GEMINI_API_KEY=your_gemini_key
   GROQ_API_KEY=your_groq_key
   ```

4. **Run the backend**
   ```bash
   uvicorn backend.main:app --reload --port 8080
   ```

5. **Load the Chrome extension**
   - Go to `chrome://extensions`
   - Enable Developer Mode
   - Click "Load unpacked" and select the `chrome-extension/` folder

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/suggest-templates` | POST | Generate short optimized prompt templates |
| `/suggest-templates-descriptive` | POST | Generate structured descriptive templates |
| `/prompt-score` | POST | Score prompt quality (low/medium/high) |
| `/prompt_classifier` | POST | Recommend best LLM based on prompt history |
| `/detect-pii` | POST | Check text for PII |
| `/summary-gen` | POST | Generate session context summary |

## Deployment

The project deploys to Google Cloud Run via GitHub Actions. Push to `main` triggers automatic deployment for both the backend and MCP server.

## License

MIT
