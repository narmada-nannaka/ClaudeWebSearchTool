# Claude Web Search Tool

A Jupyter notebook demonstrating how to use Claude's built-in **web search tool** via the Anthropic Python SDK. The notebook shows how to give Claude real-time internet access during a conversation, with optional domain restrictions to control which sites it can query.

## What it does

`006_web_search.ipynb` walks through:

1. **Client setup** — loads your API key from a `.env` file and creates an `Anthropic` client targeting `claude-sonnet-4-5`.
2. **Conversation helpers** — reusable functions (`add_user_message`, `add_assistant_message`, `chat`, `text_from_message`) that manage a message list and call `client.messages.create`.
3. **Web search schema** — configures the `web_search_20250305` built-in tool with:
   - `max_uses: 5` — caps how many searches Claude may perform per response.
   - `allowed_domains: ["nih.gov"]` — restricts searches to a trusted domain (easily changed or removed).
4. **Example query** — sends *"What's the best exercise for gaining leg muscle?"* and forces Claude to use web search (`tool_choice`), then prints the structured response.

## Prerequisites

| Requirement | Version |
|---|---|
| Python | 3.9+ |
| Jupyter | any (notebook or lab) |
| anthropic SDK | `>=0.49` (supports `web_search_20250305`) |
| python-dotenv | any |

## Setup

1. **Clone the repo**

   ```bash
   git clone https://github.com/<your-username>/ClaudeWebSearchTool.git
   cd ClaudeWebSearchTool
   ```

2. **Install dependencies**

   ```bash
   pip install anthropic python-dotenv jupyter
   ```

3. **Add your API key**

   Create a `.env` file in the project root (already git-ignored):

   ```
   ANTHROPIC_API_KEY=sk-ant-...
   ```

   Get a key at [console.anthropic.com](https://console.anthropic.com).

4. **Launch the notebook**

   ```bash
   jupyter notebook 006_web_search.ipynb
   ```

## Usage

Run the cells in order from top to bottom. To ask a different question, edit the string passed to `add_user_message` in the last cell. To allow all domains (no restriction), remove the `allowed_domains` key from `web_search_schema`.

```python
# Unrestricted search example
web_search_schema = {
    "type": "web_search_20250305",
    "name": "web_search",
    "max_uses": 5,
}
```

To let Claude decide *whether* to search (instead of forcing it), change `tool_choice`:

```python
response = chat(messages, tools=[web_search_schema], tool_choice={"type": "auto"})
```

## Project structure

```
ClaudeWebSearchTool/
├── 006_web_search.ipynb   # Main notebook
├── .env                   # API key (git-ignored, create manually)
├── .gitignore
├── LICENSE
└── README.md
```

## License

See [LICENSE](LICENSE).
