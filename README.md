# freebuff2api

OpenAI-compatible API adapter for Codebuff Freebuff

## Endpoints

- `GET /v1/models`
- `POST /v1/chat/completions`
- `GET /healthz`

## Configuration

### Getting a Token

No need to install Freebuff / Codebuff CLI. You can directly access the public page to automatically obtain a token:

```text
https://freebuff.071129.xyz/
```

Usage:

1. Open the URL above
2. Select Freebuff
3. Click "Start Authentication" and complete the authorization on the redirected page
4. Return to the page and copy the displayed token
5. Write the copied token to the `.env` file in this project

Example:

```dotenv
FREEBUFF_TOKEN=your Freebuff Bearer token
```

Multiple accounts can be separated with English commas. Concurrent requests will be prioritized to idle accounts, preventing the global active free session of a single Freebuff account from being overridden by concurrent model-switching requests:

```dotenv
FREEBUFF_TOKEN=token-a,token-b,token-c
```

Copy `.env.example` to `.env`, then fill in the upstream token:

```powershell
Copy-Item .env.example .env
```

`.env` example:

```dotenv
FREEBUFF_TOKEN=your Freebuff Bearer token
FREEBUFF_API_KEY=local OpenAI API key, can be left empty
FREEBUFF_AD_PROVIDERS=gravity,zeroclick
FREEBUFF_PROXY_ENABLED=false
FREEBUFF_PROXY_URL=
FREEBUFF_DEBUG=false
FREEBUFF_LOG_LEVEL=INFO
FREEBUFF_LOG_BODY_CHARS=2000
FREEBUFF_LOG_COLOR=true
FREEBUFF_HOST=0.0.0.0
FREEBUFF_PORT=8000
```

Proxy is disabled by default. All upstream requests are direct connections and do not read system `HTTP_PROXY` / `HTTPS_PROXY`.

To route all upstream requests through a proxy, enable it in `.env`:

```dotenv
FREEBUFF_PROXY_ENABLED=true
FREEBUFF_PROXY_URL=http://127.0.0.1:7890
```

Supports HTTP and SOCKS proxies, for example:

```dotenv
FREEBUFF_PROXY_URL=http://127.0.0.1:7890
FREEBUFF_PROXY_URL=socks5://127.0.0.1:1080
FREEBUFF_PROXY_URL=socks5h://127.0.0.1:1080
```

Currently built-in Freebuff models:

- `deepseek/deepseek-v4-flash`
- `deepseek/deepseek-v4-pro`
- `moonshotai/kimi-k2.6`
- `minimax/minimax-m2.7`
- `minimax/minimax-m3`
- `google/gemini-2.5-flash-lite`
- `google/gemini-3.1-flash-lite-preview`
- `google/gemini-3.1-pro-preview`
- `mimo/mimo-v2.5`
- `mimo/mimo-v2.5-pro`

When debugging empty returns or upstream exceptions:

```dotenv
FREEBUFF_DEBUG=true
FREEBUFF_LOG_LEVEL=DEBUG
FREEBUFF_LOG_BODY_CHARS=0
```

## Running

```powershell
uv sync
uv run freebuff2api
```

Or:

```powershell
python -m pip install -e .
python main.py
```

## Usage Examples

```powershell
curl http://127.0.0.1:8000/v1/chat/completions `
  -H "Authorization: Bearer $env:FREEBUFF_API_KEY" `
  -H "Content-Type: application/json" `
  -d '{
    "model": "deepseek/deepseek-v4-flash",
    "messages": [{"role": "user", "content": "Hello"}],
    "stream": false
  }'
```

Streaming:

```powershell
curl -N http://127.0.0.1:8000/v1/chat/completions `
  -H "Authorization: Bearer $env:FREEBUFF_API_KEY" `
  -H "Content-Type: application/json" `
  -d '{
    "model": "deepseek/deepseek-v4-flash",
    "messages": [{"role": "user", "content": "Write a Python quicksort"}],
    "stream": true
  }'
```

## Thanks

> [FreeBuff](https://freebuff.com)
