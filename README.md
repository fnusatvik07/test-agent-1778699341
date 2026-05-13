# Test Agent

Auto-generated AI agent service with real-time streaming.

## Role
General Question Answerer

## Agent ID
ui_user_ac440dda

## Tools
WebSearch, WebFetch, AskUserQuestion

## Usage

### Start the service
```bash
uv sync
uv run uvicorn main:app --host 0.0.0.0 --port 8000
```

### Stream agent progress (Real-time)
```bash
curl -X POST "http://localhost:8000/stream" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Your task here"}' \
  --no-buffer
```

### Query the agent (Non-streaming)
```bash
curl -X POST "http://localhost:8000/query" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Your task here"}'
```

### Get agent info
```bash
curl http://localhost:8000/info
```

### JavaScript Streaming Example
```javascript
const eventSource = new EventSource('http://localhost:8000/stream', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ prompt: "Research OpenAI competitors" })
});

eventSource.onmessage = function(event) {
  const data = JSON.parse(event.data);
  console.log(`[${data.type}] ${data.data.message}`);

  if (data.type === 'complete') {
    console.log('Final result:', data.data.response);
    eventSource.close();
  }
};
```

## Endpoints
- `GET /` - Service information
- `POST /stream` - **Stream agent progress in real-time**
- `POST /query` - Send tasks to the agent (non-streaming)
- `GET /info` - Agent details
- `GET /health` - Health check

## Stream Events
- `progress` - Agent status updates
- `tool_use` - When agent uses tools (WebSearch, etc.)
- `response` - Partial agent responses
- `complete` - Final result with full response
- `error` - Error messages
