<div align="center">

# SummarizeAPI

**Up to 56× cheaper than GPT-5.5 Pro. No compromises on quality.**  
Proprietary 400B model built exclusively for summarization. $5/month for 5,000 requests. **$14/month for truly unlimited — no caps, no overages, ever.**

[![Live](https://img.shields.io/badge/status-live-brightgreen)](https://summarizeapi.com)
[![API](https://img.shields.io/badge/API-v1.0-6366f1)](https://summarizeapi.com/docs)
[![Languages](https://img.shields.io/badge/languages-119%2B-blue)](https://summarizeapi.com/docs)
[![Free tier](https://img.shields.io/badge/free_tier-100_req%2Fmo-green)](https://summarizeapi.com/signup)
[![Business](https://img.shields.io/badge/business-unlimited-purple)](https://summarizeapi.com/#pricing)

[**Live Demo**](https://summarizeapi.com/#demo) · [**API Docs**](https://summarizeapi.com/docs) · [**Pricing**](https://summarizeapi.com/#pricing) · [**Sign Up Free**](https://summarizeapi.com/signup)

</div>

---

## Why SummarizeAPI?

Most teams that need summarization at scale either pay a fortune for general-purpose LLMs (GPT-5.5 Pro charges **~$280/month** for 5,000 requests) or build brittle in-house pipelines.

SummarizeAPI is different:

| | SummarizeAPI | GPT-5.5 Pro API |
|---|---|---|
| **5,000 requests/month** | **$5** | ~$280 |
| **Per request cost** | **$0.001** | ~$0.056 |
| **Unlimited requests** | **$14/month** | Hundreds/month |
| **Built for summarization** | Purpose-built 400B model | General chatbot |
| **Response focus** | Summarization-optimized | Multi-task |
| **Free tier** | 100 req/mo, no credit card | Pay-as-you-go only |

**Up to 56× cheaper per request than GPT-5.5 Pro.** And because our model was trained exclusively on summarization tasks, it outperforms repurposed general models on precision, factual fidelity, and speed.

---

## Plans

| Plan | Price | Requests | Best for |
|------|-------|----------|---------|
| **Free** | $0/month | 100/month | Testing & prototyping |
| **Professional** | $5/month | 5,000/month | Startups & indie developers |
| **Business** | $14/month | **Unlimited** | Production apps, high-volume pipelines |

### Business Plan: Truly Unlimited

The Business plan has **no request cap, no throttle, no overage fees**. Whether you process 10,000 or 500,000 documents per month, the price stays the same: **$14/month**.

On GPT-5.5 Pro, that same unlimited volume would cost hundreds to thousands of dollars per month depending on text length. SummarizeAPI's Business plan is the only truly flat-rate, unlimited summarization API on the market.

---

## Features

- **Proprietary 400B model** — trained from scratch exclusively on summarization, not a general LLM repurposed for the task
- **119+ languages** — auto-detected, native-quality output; summarize Arabic into French, Japanese into English, and more
- **Real-time streaming** — Server-Sent Events (SSE) for progressive UI rendering
- **Output length control** — `short` (2–3 sentences), `medium` (1 paragraph), `long` (2–3 paragraphs), `extensive` (4–5 paragraphs)
- **Writing styles** — `professional`, `casual`, `academic`, `technical`, `simple`
- **Output formats** — `paragraph`, `bullets`, `mixed`
- **Keyword extraction** — 5–7 key terms extracted per document
- **Privacy-first** — text is processed in real-time and immediately discarded; we never store or train on your content
- **99.9% uptime SLA** (Business plan)

---

## Quick Start

**1. Get your free API key** at [summarizeapi.com/signup](https://summarizeapi.com/signup) — no credit card required.

**2. Send your first request:**

```bash
curl -X POST https://summarizeapi.com/api/summarize \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"text": "Your text here..."}'
```

**3. Get your summary:**

```json
{
  "success": true,
  "summary": "A concise, high-quality summary of your text.",
  "keywords": null,
  "original_length": 1234,
  "summary_length": 287,
  "processing_time": 2.85,
  "requests_remaining": 99
}
```

---

## API Reference

### Endpoint

```
POST https://summarizeapi.com/api/summarize
```

### Authentication

```http
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

### Request Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `text` | string | **required** | Text to summarize (50–50,000 characters) |
| `length` | string | `medium` | `short` · `medium` · `long` · `extensive` |
| `style` | string | `professional` | `professional` · `casual` · `academic` · `technical` · `simple` |
| `format` | string | `paragraph` | `paragraph` · `bullets` · `mixed` |
| `outputLang` | string | `auto` | Output language code (`en`, `fr`, `es`, `de`, `ar`, `zh`, `ja`, ...) or `auto` |
| `includeKeywords` | boolean | `false` | Extract 5–7 key terms |

### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `success` | boolean | `true` on success |
| `summary` | string | Generated summary |
| `keywords` | string\|null | Comma-separated key terms (when `includeKeywords: true`) |
| `original_length` | integer | Input character count |
| `summary_length` | integer | Summary character count |
| `processing_time` | float | Duration in seconds |
| `requests_remaining` | integer | Remaining requests this billing period |

### Error Codes

| Status | Meaning |
|--------|---------|
| `400` | Missing `text`, text too short, or malformed JSON |
| `401` | Invalid or missing API key |
| `429` | Monthly quota exceeded — upgrade or wait for reset |
| `500` | Server error — retry with exponential backoff |

---

## Code Examples

### JavaScript / Node.js

```js
class SummarizeAPI {
  constructor(apiKey) {
    this.apiKey = apiKey;
    this.baseURL = 'https://summarizeapi.com/api';
  }

  async summarize(text, options = {}) {
    const res = await fetch(`${this.baseURL}/summarize`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${this.apiKey}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ text, ...options })
    });
    if (!res.ok) throw new Error((await res.json()).error);
    return res.json();
  }
}

const client = new SummarizeAPI('YOUR_API_KEY');

const result = await client.summarize(
  'Your long text here...',
  { length: 'medium', style: 'professional', includeKeywords: true }
);

console.log(result.summary);
console.log(result.keywords);
```

### Python

```python
import requests

class SummarizeAPI:
    BASE_URL = "https://summarizeapi.com/api"

    def __init__(self, api_key: str):
        self.session = requests.Session()
        self.session.headers.update({"Authorization": f"Bearer {api_key}"})

    def summarize(self, text: str, **kwargs) -> dict:
        response = self.session.post(
            f"{self.BASE_URL}/summarize",
            json={"text": text, **kwargs}
        )
        response.raise_for_status()
        return response.json()

client = SummarizeAPI("YOUR_API_KEY")

result = client.summarize(
    text="Artificial intelligence is reshaping industries...",
    length="medium",
    style="professional",
    include_keywords=True
)

print(result["summary"])
print(f"Compressed by {100 - round(result['summary_length'] / result['original_length'] * 100)}%")
```

### PHP

```php
<?php

class SummarizeAPI {
    private string $apiKey;
    private string $baseUrl = 'https://summarizeapi.com/api';

    public function __construct(string $apiKey) {
        $this->apiKey = $apiKey;
    }

    public function summarize(string $text, array $options = []): array {
        $ch = curl_init($this->baseUrl . '/summarize');
        curl_setopt_array($ch, [
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_POST           => true,
            CURLOPT_POSTFIELDS     => json_encode(array_merge(['text' => $text], $options)),
            CURLOPT_HTTPHEADER     => [
                'Authorization: Bearer ' . $this->apiKey,
                'Content-Type: application/json'
            ],
            CURLOPT_TIMEOUT => 60
        ]);
        $body = curl_exec($ch);
        $code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
        curl_close($ch);

        if ($code !== 200) {
            $err = json_decode($body, true);
            throw new RuntimeException($err['error'] ?? 'API error');
        }
        return json_decode($body, true);
    }
}

$client = new SummarizeAPI('YOUR_API_KEY');
$result = $client->summarize('Your text here...', ['length' => 'medium', 'includeKeywords' => true]);
echo $result['summary'];
```

### Real-time Streaming (SSE)

Append `?stream=1` to receive the summary token by token:

```js
const res = await fetch('https://summarizeapi.com/api/summarize?stream=1', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ text: 'Your text here...' })
});

const reader = res.body.getReader();
const decoder = new TextDecoder();

while (true) {
  const { done, value } = await reader.read();
  if (done) break;

  const lines = decoder.decode(value).split('\n');
  for (const line of lines) {
    if (!line.startsWith('data: ')) continue;
    const event = JSON.parse(line.slice(6));
    if (event.type === 'summary') process.stdout.write(event.content);
    if (event.type === 'done')    console.log('\nFinished:', event.summary);
  }
}
```

---

## Use Cases

- **News apps** — Summarize articles in any language for readers on the go
- **Legal tech** — Condense contracts, filings, and case law into actionable briefs
- **Research tools** — Process academic papers, extract key findings and keywords
- **Customer support** — Summarize support tickets and conversation histories
- **Content pipelines** — Bulk-process thousands of documents per day on the Business plan (unlimited)
- **E-learning** — Convert dense course material into concise study notes
- **Newsletter tools** — Auto-generate digest summaries from RSS feeds

---

## Pricing Deep Dive

### Why is SummarizeAPI so much cheaper?

General-purpose LLMs like GPT-5.5 Pro are designed to do everything: code, chat, reason, write. That generality is expensive. We built a **400-billion-parameter model trained exclusively on summarization** — it does one thing and does it better than any general model.

The result: dramatically lower inference cost, which we pass directly to you.

### The Business Plan math

| Scenario | GPT-5.5 Pro | SummarizeAPI Business |
|----------|-------------|----------------------|
| 10,000 requests/mo | ~$560 | **$14** |
| 50,000 requests/mo | ~$2,800 | **$14** |
| 100,000 requests/mo | ~$5,600 | **$14** |

No caps. No overages. No surprises. **$14/month, unlimited.**

---

## Privacy & Security

- **No data retention**: text is processed in real-time and discarded immediately after summarization
- **No training on your data**: we never use your content to train or fine-tune models
- **HTTPS only**: all API traffic is encrypted in transit
- **API key authentication**: keys can be regenerated at any time from your dashboard

---

## Links

| Resource | URL |
|----------|-----|
| Website | [summarizeapi.com](https://summarizeapi.com) |
| API Documentation | [summarizeapi.com/docs](https://summarizeapi.com/docs) |
| Live Demo | [summarizeapi.com/#demo](https://summarizeapi.com/#demo) |
| Dashboard | [summarizeapi.com/dashboard](https://summarizeapi.com/dashboard) |
| Pricing | [summarizeapi.com/#pricing](https://summarizeapi.com/#pricing) |
| Sign Up (Free) | [summarizeapi.com/signup](https://summarizeapi.com/signup) |
| Contact & Support | [summarizeapi.com/contact](https://summarizeapi.com/contact) |

---

## Changelog

### v1.0 — Initial Release

- REST API with JSON request/response
- Real-time SSE streaming
- 119+ languages (auto-detected)
- 5 writing styles, 4 output formats, 4 length options
- Keyword extraction
- Free tier: 100 requests/month, no credit card required
- Professional plan: $5/month, 5,000 requests
- Business plan: $14/month, **unlimited requests**

---

<div align="center">

**Start for free. Scale without limits.**

[Get your free API key →](https://summarizeapi.com/signup)

*No credit card required. 100 free requests per month.*

</div>
