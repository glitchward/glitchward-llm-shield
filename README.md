# Glitchward LLM Shield — OpenClaw Skill

Prompt injection detection for AI agents. Scan prompts through a 6-layer detection pipeline before they reach your LLM.

> **Privacy notice:** This skill sends prompt content to Glitchward's external API for analysis. Do not use it with prompts containing secrets, credentials, PII, regulated data, or proprietary content unless you have approved external transmission.

## Install

```bash
npx clawhub@0.23.3 install glitchward-llm-shield
```

Or paste this repo URL directly into your OpenClaw agent chat.

## Setup

1. Get a free API token at [glitchward.com/shield](https://glitchward.com/shield)
2. Set the environment variable:

```bash
export GLITCHWARD_SHIELD_TOKEN="your-token-here"
```

> **Credential safety:** Never hardcode the token in source files. Keep it out of shell history, logs, and version control.

## What it does

This skill adds prompt injection scanning to your AI agent. Before any user input reaches your LLM, it gets validated through Glitchward's Shield API:

- **1,000+ detection patterns** across 26 provider modules
- **6-layer pipeline**: text normalization, pattern matching, encoding detection, invisible character analysis, known injection database, AI-powered analysis
- **25+ attack categories**: jailbreaks, data exfiltration, MCP abuse, hooks hijacking, skill weaponization, multilingual attacks, and more
- **10+ languages**: Korean, Japanese, Chinese, Russian, Spanish, German, French, Portuguese, Vietnamese
- **Sub-50ms** response times

## API Endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/api/shield/validate` | POST | Validate a single prompt |
| `/api/shield/validate/batch` | POST | Validate multiple prompts |
| `/api/shield/stats` | GET | Usage statistics and quota |

## Example

```bash
curl -s -X POST "https://glitchward.com/api/shield/validate" \
  -H "X-Shield-Token: $GLITCHWARD_SHIELD_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "What is the capital of France?"}' | jq .
```

Response:
```json
{
  "safe": true,
  "blocked": false,
  "risk_score": 0.02,
  "processing_time_ms": 18
}
```

When a prompt is flagged:
```json
{
  "safe": false,
  "blocked": true,
  "risk_score": 0.95,
  "processing_time_ms": 12,
  "matches": [
    {
      "category": "instruction_override",
      "severity": "critical",
      "pattern": "role hijacking detected",
      "matched_text": "...",
      "description": "Attempted to override system instructions"
    }
  ],
  "request_id": "abc-123"
}
```

## Pricing

| Plan | Requests/month | Price |
|---|---|---|
| Free | 1,000 | Free |
| Starter | 50,000 | €39.90/mo |
| Pro | 500,000 | €119.90/mo |

Get your free token at [glitchward.com/shield](https://glitchward.com/shield)

## Links

- [LLM Shield Landing Page](https://glitchward.com/shield)
- [LLMPI Database](https://glitchward.com/llmpi) — Free public database of known prompt injection patterns
- [ClawHub Skill Analyzer](https://glitchward.com/shield/skill-analyzer) — Free security analysis for OpenClaw skills

## License

MIT
