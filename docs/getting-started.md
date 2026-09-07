# NovaAPI — Getting Started Guide

NovaAPI is an OpenAI-compatible API gateway that gives developers outside China easy access to top Chinese frontier models (DeepSeek, and soon Qwen, GLM, Kimi, MiniMax) — no Chinese phone number, no ID verification, no VPN required.

- **Base URL:** `https://api.lbase.com/v1` (OpenAI format) — Anthropic-format clients point at `https://api.lbase.com`
- **Sign in / console:** `https://api.lbase.com`
- **Billing:** PayPal & USDT (TRC20), pay-as-you-go

---

## 1. Create your account

1. Go to **https://api.lbase.com/register**.
2. Enter your **email address** and a password (min. 6 characters).
3. That's it — no phone number, no credit card required to register.

> If your email doesn't arrive, check your spam folder.

---

## 2. Top up your balance

NovaAPI is **pay-as-you-go**: you prepay a balance, and each request deducts from it based on tokens used. No subscription.

1. Log in and open **Balance / Top Up** in the console.
2. Choose a payment method:
   - **PayPal** — you'll be redirected to PayPal to approve the payment.
   - **USDT (TRC20)** — you'll get a TRON wallet address; send USDT from a TRC20-compatible wallet (make sure you keep a little TRX for the network fee).
3. Enter the amount in **USD** and confirm. Your balance updates automatically once the payment is confirmed (usually within a minute).

Top-ups are non-refundable except where required by law (see Terms).

---

## 3. Create an API key

1. In the console open **API Keys → Create Key**.
2. Give the key a name (e.g. `my-app`).
3. **Select a model channel (group).** Your key must belong to a channel — a channel is a model family (e.g. *deepseek*). Currently available channels appear in the picker; more are added as providers come online.
4. Copy the key (**`sk-...`**). It is shown only once — treat it like a password.

> ⚠️ A key that is not assigned to a channel cannot be used (API returns `API Key is not assigned to any group`). Always pick a channel when creating a key.

---

## 4. Make your first request

NovaAPI is a drop-in replacement for the OpenAI API. Point your existing client at `https://api.lbase.com/v1` and use your key.

### cURL

```bash
curl https://api.lbase.com/v1/chat/completions \
  -H "Authorization: Bearer sk-your-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "deepseek-v4-flash",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

### Python (OpenAI SDK)

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-your-key-here",
    base_url="https://api.lbase.com/v1",
)

resp = client.chat.completions.create(
    model="deepseek-v4-flash",
    messages=[{"role": "user", "content": "Hello!"}],
)
print(resp.choices[0].message.content)
```

### Node.js (OpenAI SDK)

```js
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "sk-your-key-here",
  baseURL: "https://api.lbase.com/v1",
});

const resp = await client.chat.completions.create({
  model: "deepseek-v4-flash",
  messages: [{ role: "user", content: "Hello!" }],
});
console.log(resp.choices[0].message.content);
```

### List available models

```bash
curl https://api.lbase.com/v1/models \
  -H "Authorization: Bearer sk-your-key-here"
```

The exact model IDs available to you depend on the channel your key belongs to. Use `deepseek-v4-flash` (fast, cheap) for general tasks; larger reasoning models are listed under `/v1/models`.

### Anthropic format (Claude-style clients)

NovaAPI also speaks the Anthropic Messages protocol at `https://api.lbase.com/v1/messages`, so Claude-flavoured clients work too:

```bash
curl https://api.lbase.com/v1/messages \
  -H "Authorization: Bearer sk-your-key-here" \
  -H "Content-Type: application/json" \
  -H "anthropic-version: 2023-06-01" \
  -d '{
    "model": "deepseek-v4-flash",
    "max_tokens": 256,
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

---

## 5. Use it inside the tools you already use

Because NovaAPI speaks the OpenAI protocol, it works with most AI tools by changing the **base URL** and **API key**.

| Tool | How to point it at NovaAPI |
|---|---|
| **Claude Code** | `export ANTHROPIC_BASE_URL=https://api.lbase.com` `export ANTHROPIC_AUTH_TOKEN=sk-...` (Claude Code appends `/v1/messages` itself; use a NovaAPI key as the token) |
| **OpenAI Codex CLI** | `export OPENAI_BASE_URL=https://api.lbase.com/v1` `export OPENAI_API_KEY=sk-...` |
| **Cursor** | Settings → Models → OpenAI-compatible: base URL `https://api.lbase.com/v1`, key `sk-...`, model `deepseek-v4-flash` |
| **Cline / Roo Code** | Provider: OpenAI Compatible → base URL `https://api.lbase.com/v1`, API key `sk-...` |
| **Cherry Studio / ChatBox** | Add provider → OpenAI-compatible → base URL + key, then pick the model |
| **Dify / LangChain / any OpenAI SDK** | Set the model provider base URL to `https://api.lbase.com/v1` and use your key |

> Model availability per channel is listed in the console when you create a key. If a tool asks for a model list, start with `deepseek-v4-flash`.

---

## 6. Check usage & balance

- **Usage page** in the console shows token usage, requests, and cost per key.
- **Balance** is deducted per request; top up any time.
- Watch the **Usage Dashboard** for per-model and per-day breakdowns.

---

## FAQ

**Do I need a Chinese phone number?**
No. Register with any email address.

**Which models are available?**
The *deepseek* channel is live (e.g. `deepseek-v4-flash`). Qwen, GLM, Kimi and MiniMax channels are being onboarded — check the channel picker in the console.

**Is my existing OpenAI code compatible?**
Yes — change `base_url`/`baseURL` to `https://api.lbase.com/v1` and keep everything else the same.

**How am I billed?**
Per token, from your prepaid USD balance. No monthly fee. Prices are shown in the console.

**What payment methods do you accept?**
PayPal and USDT (TRC20). More methods are planned.

**Is my data shared?**
Requests are forwarded to the model provider you use (e.g. DeepSeek) to fulfill them. See the Terms & Privacy Policy on the homepage.

**Where do I get support?**
Email **hello@lbase.com**.

---

*NovaAPI is a product of L-Base. © 2026 NovaAPI.*
