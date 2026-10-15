# Apontador Brazil Business Scraper

Extract structured data from [Apontador](https://apontador.com.br) — Apontador — Brazil's local business directory. Search any city and category for business names, addresses and phone numbers, with ratings and optional opening hours. Run it on a schedule to catch only new or changed businesses.

**[Apontador Brazil Business Scraper on Apify →](https://apify.com/blackfalcondata/apontador-com-br-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, contact information for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from Apontador on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from Apontador.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "pizzaria",
  "location": "São Paulo, SP",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | What to look for, e.g. "pizzaria", "dentista", "auto escola". Leave empty to list every business in the chosen location. Use a JSON array for several terms, e.g. ["pizzaria","padaria"]. |
| `location` | string | — | Brazilian city as "City, UF" — e.g. "São Paulo, SP", "Belo Horizonte, MG". Use a JSON array for several cities. Leave empty to use the site's default city. |
| `maxResults` | integer | `100` | Maximum total businesses (0 = as many as the site will serve). The directory serves roughly 850 businesses per search term and city — narrow the term or the city to go deeper. |
| `startUrls` | array | `[]` | Paste raw search-result URLs from apontador.com.br (e.g. https://www.apontador.com.br/local/search.html?q=pizzaria&loc=S%C3%A3o+Paulo%2C+SP). Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Open each business page for category, coordinates, CEP, opening hours, features, all phone numbers, the full description and the reviews. Turn off for name/address/phone/rating only (much faster). |
| `descriptionMaxLength` | integer | `0` | Truncate the business description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (search terms, cities, startUrls) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked set of businesses. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When Incremental Mode is on, also emit businesses with no detected changes (changeType=UNCHANGED). |
| `emitExpired` | boolean | `false` | When Incremental Mode is on, also emit businesses that disappeared since the last run (changeType=EXPIRED). |
| `skipReposts` | boolean | `false` | When incremental, skip businesses whose content matches one that disappeared in a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED businesses. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of businesses included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every record returns the same 38-field schema. Missing values are `null` — never omitted.

- `listingId`
- `placeId`
- `title`
- `url`
- `portalUrl`
- `category`
- `subcategory`
- `categorySlug`
- `description`
- `detailDescription`
- `location`
- `address`
- `addressDetail`
- `street`
- `streetNumber`
- `neighborhood`
- `city`
- `citySlug`
- `state`
- `postalCode`
- `latitude`
- `longitude`
- `phone`
- `phones`
- `openingHours`
- `rating`
- `reviewCount`
- `reviews`
- `features`
- `isApontadorMais`
- `sponsor`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "listingId": "483cff7f29273b859816d89b0471bc9557bf67cf5be884d2e422af6eb6e39a57",
  "placeId": "7A64MWHA",
  "title": "Pizzaria Ruberto's",
  "url": "https://www.apontador.com.br/local/sp/sao_paulo/restaurantes/7A64MWHA/pizzaria_rubertos.html",
  "portalUrl": "https://www.apontador.com.br/local/sp/sao_paulo/restaurantes/7A64MWHA/pizzaria_rubertos.html",
  "category": "Bares e Restaurantes",
  "subcategory": "Pizzarias",
  "categorySlug": "restaurantes",
  "description": "Venha conhecer Pizzaria Rubertos. Estamos no bairro Bela Vista em São Paulo.",
  "detailDescription": "Venha conhecer Pizzaria Rubertos. Estamos no bairro Bela Vista em São Paulo.",
  "location": "Rua Dr. Luis Barreto, 354, Bela Vista, São Paulo - SP",
  "address": "Rua Dr. Luis Barreto, 354, Bela Vista, São Paulo - SP"
}
```

*Truncated — full records contain 38 fields. See Output fields for the complete schema.*

**[Try Apontador Brazil Business Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/apontador-com-br-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.00008 |

See the [actor on Apify](https://apify.com/blackfalcondata/apontador-com-br-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape Apontador?**
Use this actor on Apify to extract structured data from apontador.com.br. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get Apontador data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape Apontador?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check Apontador's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Apontador Brazil Business Scraper](https://apify.com/blackfalcondata/apontador-com-br-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
