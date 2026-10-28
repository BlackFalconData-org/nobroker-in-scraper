# Nobroker India Scraper - Rental Listings by City

Extract structured data from [nobroker.in](https://nobroker.in) — rental listings from nobroker.in across Indian cities. Search by city or by map coordinates and radius, or paste a nobroker.in search link. Every listing returns rent, deposit and address as structured rows ready for a spreadsheet or CRM.

**[Nobroker India Scraper - Rental Listings by City on Apify →](https://apify.com/blackfalcondata/nobroker-in-scraper?fpr=1h3gvi)**

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

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from nobroker.in on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from nobroker.in.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "location": "bangalore",
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `location` | string | — | City to search for rental homes, as nobroker.in names it: bangalore, mumbai, pune, chennai, hyderabad, delhi, gurgaon, noida, greater_noida, ghaziabad, faridabad, navi_mumbai, thane. Without coordinates the search starts at the city centre. A city nobroker.in does not cover stops the run with an input error. |
| `lat` | number | — | Latitude of the search centre. Pair with Longitude. Without a city, the nearest city nobroker.in covers is used. |
| `lon` | number | — | Longitude of the search centre. Pair with Latitude. |
| `radiusKm` | number | `2` | Search radius in kilometres around the centre (default 2). With coordinates set, listings outside the radius — or without coordinates — are left out. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Rental search-results URLs copied from nobroker.in (https://www.nobroker.in/property/rent/…?searchParam=…). Each URL becomes its own search; results are merged and deduped by listing ID. When provided, start URLs replace the city and coordinate fields; if none is a supported URL the run stops with an input error. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (city, coordinates, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
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

Every listing returns the same 66-field schema. Missing values are `null` — never omitted.

- `id`
- `title`
- `secondaryTitle`
- `propertyType`
- `type`
- `typeDesc`
- `bhk`
- `city`
- `locality`
- `localityId`
- `street`
- `address`
- `latitude`
- `longitude`
- `location`
- `accurateLocation`
- `rent`
- `deposit`
- `pricePerSqftInr`
- `negotiable`
- `maintenanceIncluded`
- `loanAvailable`
- `leaseTypeNew`
- `areaSqft`
- `society`
- `propertyAge`
- `parking`
- `parkingDesc`
- `bathroom`
- `floor`
- `totalFloor`
- `furnishing`
- `facingDesc`
- `lift`
- `swimmingPool`
- `amenities`
- `amenitiesMap`
- `ownerId`
- `ownerName`
- `active`
- `sponsored`
- `promoted_b`
- `premium`
- `propertyScore`
- `score`
- `photoAvailable`
- `thumbnailImage`
- `originalImageUrl`
- `photos`
- `videoUnit`
- `availableFrom`
- `creationDate`
- `activationDate`
- `lastUpdateDate`
- `detailUrl`
- `portalUrl`
- `shortUrl`
- `aea__`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "id": "8a9f01828740c7f601874115b7a83e86",
  "title": "2 BHK Flat In Amara Sona Apartments for Rent  In Naagarabhaavi ",
  "secondaryTitle": "Govindaraja Nagar Ward near BBMP PU COLLEGE",
  "propertyType": "RENT",
  "type": "BHK2",
  "typeDesc": "2 BHK",
  "bhk": 2,
  "city": "bangalore",
  "locality": "Naagarabhaavi ",
  "localityId": "vittal_mallya_road_bangalore",
  "street": "Govindaraja Nagar Ward near BBMP PU COLLEGE",
  "address": "Govindaraja Nagar Ward near BBMP PU COLLEGE, Naagarabhaavi , bangalore"
}
```

*Truncated — full records contain 66 fields. See Output fields for the complete schema.*

**[Try Nobroker India Scraper - Rental Listings by City now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/nobroker-in-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.00225 |

See the [actor on Apify](https://apify.com/blackfalcondata/nobroker-in-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape nobroker.in?**
Use this actor on Apify to extract structured data from nobroker.in. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get nobroker.in data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape nobroker.in?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check nobroker.in's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Nobroker India Scraper - Rental Listings by City](https://apify.com/blackfalcondata/nobroker-in-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
