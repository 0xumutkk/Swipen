<div align="center">

# 🎯 Swipen

### *Swipe right on the future.*

**A TikTok-style feed for live prediction markets — built as a Base Mini App.**
Scroll. Read the odds. Take the trade. All onchain, all on Base.

<br />

![Base](https://img.shields.io/badge/Built%20on-Base-0052FF?style=for-the-badge&logo=coinbase&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js%2015-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)
![React 19](https://img.shields.io/badge/React%2019-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![wagmi](https://img.shields.io/badge/wagmi%20%2B%20viem-1C1B1F?style=for-the-badge&logo=ethereum&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)

<br />

![Realtime](https://img.shields.io/badge/⚡_Realtime-SSE_stream-FF6B6B?style=flat-square)
![Auth](https://img.shields.io/badge/🔐_Auth-SIWE_sessions-8B5CF6?style=flat-square)
![Markets](https://img.shields.io/badge/📈_Markets-Limitless_API-10B981?style=flat-square)
![Status](https://img.shields.io/badge/🚧_Status-Active_development-F59E0B?style=flat-square)

</div>

---

## ✨ What is this?

Prediction markets are the internet's best argument-settling machine — but they usually look like a Bloomberg terminal that lost a fight. **Swipen** puts them in the format your thumb already knows: a full-screen vertical feed, one market at a time, live odds ticking as you scroll.

> 🗳️ *"Will it happen?"* → swipe up for the next take, tap to back your call.

Markets stream in live from **Limitless** on **Base**, trades are real onchain intents, and the whole thing runs inside the Base App as a Mini App.

---

## 🎬 The Feed Experience

| | |
|---|---|
| 📱 **Vertical, virtualized feed** | Infinite scroll that stays buttery — only what's on screen is rendered |
| ⚡ **Live odds over SSE** | Prices update in place; the stream connects and disconnects with visibility |
| 💼 **Portfolio & positions** | See what you're holding, exit early, or redeem after settlement |
| 🧾 **Activity trail** | Every intent, every fill, in one place |
| 🔌 **Read-only first load** | No surprise wallet popups — you connect when *you* decide |

---

## 🧩 Under the Hood

<table>
<tr><td width="33%" valign="top">

### 🔵 Base Foundation
- Dedicated miniapp wallet connector
- Injected wallet fallback
- Base App metadata via `NEXT_PUBLIC_BASE_APP_ID`
- Builder code support
- Zero auto-connect on boot

</td><td width="33%" valign="top">

### 📡 Market Data Plane
- Limitless API v1 polling (`GET /markets/active`)
- Point-budget protection so we stay a good citizen
- Redis-backed snapshot cache + in-memory fallback
- `GET /api/markets` — snapshot
- `GET /api/markets/stream` — realtime SSE

</td><td width="33%" valign="top">

### ⛓️ Trade Intents
- `POST /api/trade/intent` → `buy` · `sell` · `redeem`
- SIWE auth required in production
- Market venue metadata used first
- Full env-based contract fallbacks
- Optional approve step per action

</td></tr>
</table>

### 🛡️ Launch Security

```
🚦  Rate limiting ........ markets · stream · trade intent
🔐  SIWE verification .... POST /api/auth/siwe → signed session cookie
🍪  Session endpoint ..... GET | DELETE /api/auth/session (HttpOnly)
🎟️  Beta allowlist ....... optional gated access
💓  Health probe ......... GET /api/health
```

---

## 🚀 Quick Start

```bash
npm install
npm run dev
```

Then open **http://localhost:3000** and start scrolling. 🎉

<details>
<summary>🧊 <b>Optional: Redis cache</b></summary>

```bash
docker compose up -d redis
```

</details>

<details>
<summary>🛠️ <b>Optional: background indexer worker</b></summary>

```bash
npm run worker
```

</details>

<details>
<summary>🧪 <b>Run the tests</b></summary>

```bash
npm test
```

</details>

---

## 🔧 Environment

Copy `.env.example` → `.env.local` and fill in production values.

#### ✅ Required
| Variable | What it does |
|---|---|
| `NEXT_PUBLIC_MINI_APP_URL` | Public origin of the mini app |
| `AUTH_SESSION_SECRET` | Signs the session cookie |

#### 💡 Recommended
| Variable | What it does |
|---|---|
| `REDIS_URL` | Shared snapshot cache |
| `LIMITLESS_API_BASE_URL` | Market data source |
| `NEXT_PUBLIC_BASE_APP_ID` | Base App identity |
| `NEXT_PUBLIC_BASE_BUILDER_CODE` | Builder attribution |

#### 🧭 If market metadata is incomplete
```
LIMITLESS_TRADE_CONTRACT_ADDRESS
LIMITLESS_TRADE_FUNCTION_SIGNATURE
LIMITLESS_TRADE_ARG_MAP
```

#### 🔁 For position exits & settlement claims
```
LIMITLESS_SELL_CONTRACT_ADDRESS      LIMITLESS_REDEEM_CONTRACT_ADDRESS
LIMITLESS_SELL_FUNCTION_SIGNATURE    LIMITLESS_REDEEM_FUNCTION_SIGNATURE
LIMITLESS_SELL_ARG_MAP               LIMITLESS_REDEEM_ARG_MAP
```

Plus `USDC_TOKEN_ADDRESS` and `TRADE_APPROVE_ACTIONS` to round things out.

---

## 🗺️ Map of the Codebase

| 📄 File | 🧠 Why you'd open it |
|---|---|
| [`src/components/vertical-market-feed.tsx`](src/components/vertical-market-feed.tsx) | The star of the show — the swipeable feed |
| [`src/components/market-card.tsx`](src/components/market-card.tsx) | One market, one screen |
| [`src/components/providers.tsx`](src/components/providers.tsx) | Wagmi + React Query wiring |
| [`src/components/miniapp-auth-provider.tsx`](src/components/miniapp-auth-provider.tsx) | Mini App session lifecycle |
| [`src/lib/wagmi.ts`](src/lib/wagmi.ts) | Connectors and chain config |
| [`src/lib/security/miniapp-auth.ts`](src/lib/security/miniapp-auth.ts) | SIWE + cookie signing |
| [`src/app/api/markets/route.ts`](src/app/api/markets/route.ts) | Snapshot endpoint |
| [`src/app/api/markets/stream/route.ts`](src/app/api/markets/stream/route.ts) | SSE realtime endpoint |
| [`src/app/api/trade/intent/route.ts`](src/app/api/trade/intent/route.ts) | Buy · sell · redeem intents |
| [`src/app/api/auth/siwe/route.ts`](src/app/api/auth/siwe/route.ts) | Sign-In With Ethereum |
| [`src/app/api/auth/session/route.ts`](src/app/api/auth/session/route.ts) | Session read / logout |

---

<div align="center">

⚠️ **Not financial advice.** Prediction markets are risky and may be restricted where you live.
Trade responsibly, and only what you can afford to lose.

<br />

**Made with 🔵 on Base**

</div>
