# SoCom — Product Overview

**Last updated:** July 2026
**Domain:** socom.io (placeholder)
**Source:** PRD.md · ARCHITECTURE.md

---

## Table of Contents

1. [What Is SoCom?](#1-what-is-socom)
2. [Target Customers & Pain Points](#2-target-customers--pain-points)
3. [North Star Metrics](#3-north-star-metrics)
4. [Competitive Positioning](#4-competitive-positioning)
5. [Features](#5-features)
   - [Tier 1 — Core (MVP)](#tier-1--core-mvp)
   - [Tier 2 — Should Have (Month 3–6)](#tier-2--should-have-month-36)
   - [Tier 3 — Nice to Have (Month 6+)](#tier-3--nice-to-have-month-6)
6. [Gamification & Retention Logic](#6-gamification--retention-logic)
7. [Foundation Infrastructure](#7-foundation-infrastructure)
8. [How It Works](#8-how-it-works)
9. [Pricing](#9-pricing)

---

## 1. What Is SoCom?

**Category:** AI-Powered Broadcast & Live Commerce Management Platform (B2B SaaS)

SoCom is a comprehensive operating system designed to orchestrate the entire lifecycle of Live Commerce—from resource scheduling, real-time broadcast management, to post-live AI content generation and automated compliance. 

While platforms like Shoplive or Restream focus purely on video distribution, SoCom acts as the **"Growth Engine"** and **"Conflict Resolution Engine"** for MCNs (Multi-Channel Networks) and Brands. It unifies physical asset management (studios, equipment), human resources (Hosts, Mods), intelligent scripting (ROS), and automated short-video distribution.

> **SoCom transforms live commerce from a chaotic, manual hustle into a structured, highly efficient, and automated performance machine.**

### The Problem We Solve

| Reality Today | SoCom's Answer |
|---|---|
| **Resource Clashes** | Double-booked studios or exhausted hosts. SoCom's **Smart Calendar & Conflict Resolution Engine** checks availability (Host + Mod + Room + Equipment) before locking. |
| **Manual Scripting & Prompting** | Scripts are on Google Docs; hosts forget selling points. SoCom offers an **AI Scripting Hub** with a realtime **Prompter View** synced to the Run of Show (ROS). |
| **Fragmented Omni-Channel Chats** | Mods jump between Shopee, TikTok, and FB tabs. SoCom provides a **Unified Inbox (Mod View)** and a **Live Flying Desk (Director View)**. |
| **Blind QA & Compliance** | Brands only know host performance via sales. SoCom uses **Realtime Transcription & Banned Words** alerting, plus **AI Reconcile** to score host compliance against the ROS. |
| **Expensive Post-Live Content** | Editors spend days cutting live streams into shorts. SoCom's **AI Creative Content Factory** auto-clips highlights and publishes them via the **Social Automation** module. |

---

## 2. Target Customers & Pain Points

### Personas

| Persona | Description | Key Pain Points |
|---|---|---|
| **MCN (Multi-Channel Network) Director** | Manages dozens of hosts and studios simultaneously. | Scheduling nightmares, calculating complex payrolls (base + commission - penalties), and monitoring livestream quality across multiple rooms at once. |
| **Brand Live-Commerce Manager** | In-house team running live streams for a specific brand. | Ensuring hosts don't use banned words (e.g., mentioning competitor platforms on TikTok), guaranteeing Brand Voice, and maximizing ROI from live sessions. |
| **The Moderator (Mod/Support)** | Handles chat, pins products, and drops vouchers during live. | Overwhelmed by spam, struggling to keep up with chats across 5 different platforms concurrently. |
| **The Livestream Host (KOC/KOL)** | The face on the camera. | Burnout from back-to-back sessions, forgetting key selling points (USPs), and struggling with unreadable script formats. |

---

## 3. North Star Metrics

**Primary NSM:** `Live Hours Successfully Orchestrated` and `AI-Generated Content Published`

| Stage | Signal | Target |
|---|---|---|
| **Activation** | Agency/Brand creates a workspace, adds > 3 Hosts/Rooms, and completes a Smart Booking workflow. | >= 60% of signups |
| **Retention** | Agency runs at least 5 live sessions per week using the Prompter and Mod View. | >= 75% W1 retention |
| **Monetization** | Account upgrades to Premium for AI Reconcile, Content Factory, and Multi-channel broadcasting. | > 15% conversion rate |

---

## 4. Competitive Positioning

| Feature Category | SoCom | Restream / StreamYard | Shoplive | KiotViet / Haravan |
|---|---|---|---|---|
| **Core Value** | E2E Live Commerce Ops + AI Growth | Multi-streaming distribution | In-app Livestreaming | Retail & Order Sync |
| **Resource Scheduling** | **Yes (Host/Room/Gear)** | No | No | No |
| **Live Mod & Prompter** | **Yes (Unified Inbox & ROS)** | Basic Chat | Basic | Basic / None |
| **Compliance & QA** | **AI Reconcile & Banned Words** | No | No | No |
| **Post-Live Growth** | **AI Auto-clipping & Publishing** | Video recording only | Video recording only | No |

---

## 5. Features

### Tier 1 — Core (MVP)

#### 🏢 Module 1: Entity & Resource Management
- **Host/Mod Profiles:** Tier rates, burnout alerts, skill matching (e.g., tech vs. fashion).
- **Brand & Campaign Management:** Whitelist/Blacklist keywords, target budgets, and Host-matching engine.
- **Studio Inventory:** Manage rooms, cameras, mics. Track availability and maintenance schedules.

#### 📅 Module 2: Core Scheduling (Smart Calendar)
- **Conflict Resolution Engine:** 1-click booking that locks Host + Mod + Room + Gear.
- **Multidimensional Timeline:** Filter by Brand, Host, or Studio to spot "dead hours".
- **Agentic RAG Scheduling:** AI suggests the optimal Host and time slot based on historical sales data (e.g., Host A converts 3x better on Sunscreen than Cleansers; 8PM is peak time for Beauty SKUs).

#### 📜 Module 3: Run of Show (ROS) & AI Scripting Hub
- **Timeline Editor:** Drag-and-drop ROS blocks (Intro, Flash Sale, Minigame).
- **Prompter View:** High-contrast, realtime synced teleprompter for the Host.
- **Agentic RAG Scripting:** AI creates personalized scripts by automatically retrieving past successful selling points and context from the Vector DB for the specific Host-SKU combination.

#### 🎛️ Module 4: Omni-Channel Operation (Flying Desk)
- **Mod View:** Unified Inbox across platforms, macro replies, product/voucher pushing.
- **Director View (Flying Desk):** Multiview monitoring, scene switching, network latency alerts.

---

### Tier 2 — Should Have (Month 3–6)

#### ⚖️ Module 5: AI Reconcile & Compliance
- **Realtime Transcription & Banned Words:** AI flags forbidden words instantly to the Mod.
- **ROS Reconciliation:** AI grades the Host based on the transcript vs. the ROS (e.g., did they mention the voucher code?).

#### 📊 Module 6: Analytics, Performance Dashboard & Billing
- **Omni-channel Sales Dashboard:** Unified CCU, GMV, and Conversion Rate.
- **Host Scorecard & Automated Payouts:** Calculates payroll (Base + Affiliate - Penalty) and generates invoices.

---

### Tier 3 — Nice to Have (Month 6+)

#### 🏭 Module 7: AI Creative Content Factory
- **Auto Image & Video Gen:** Nano Banana (Packshots), Veo 3 / Kling (Cinematic teasers, B-roll).
- **Voice & Digital Clone:** ElevenLabs dubbing, Higgsfield character consistency.
- **Live-to-Shorts Pipeline:** Auto-clipping, auto-framing (16:9 to 9:16), dynamic captions.

#### 🚀 Module 8: Social Automation & Auto Publishing
- **Smart Auto-Posting:** Waterfall posting (TikTok -> FB Reels -> YT Shorts) with auto watermark removal.
- **AI Captioning & Hashtags:** Trend-optimized SEO hooks and captions.

---

## 6. Gamification & Retention Logic

For **Hosts**:
- **Performance Scorecards:** Hitting >90% compliance on ROS unlocks higher tier rates or bonus commissions.
- **Burnout Protection:** The system actively blocks overbooking, making Hosts feel cared for.

For **Agencies/Brands**:
- **Yield Reports:** "Your Studio 2 was empty 40% of the time last week. Click here to auto-generate a promotional campaign."
- **AI ROI Tracking:** Seeing a direct correlation between AI-generated Shorts and the resulting Live Session CCU.

---

## 7. Foundation Infrastructure

SoCom employs an **Event-Driven Microservices** architecture with a Hybrid Cloud strategy.

| Layer | Technology | Rationale |
|---|---|---|
| **Core API & Real-time** | Go (Gin) | High concurrency for Resource Locking, API Gateway, and WebSockets/SSE for the Mod/Prompter views. |
| **AI & Media Processing** | Python (FastAPI) | Wrapper for Generative AI (Kling, Veo, ElevenLabs) + FFmpeg/OpenCV processing + Local NLP. |
| **Relational Data** | MySQL | ACID compliance for billing, booking, and physical inventory. |
| **Search & Logging** | OpenSearch | Vector matching for Host-Brand pairing, unified high-speed chat indexing, and transcript logs. |
| **Message Queue** | RabbitMQ | Async communication between Go (API) and Python (AI Workers), handling delays and retries. |
| **Infrastructure** | AWS + Local Nodes | Cloudflare R2/S3 for media egress. EC2 for stateful websockets. Local high-compute nodes for heavy AI workloads (Transcribing). |

### Architectural Decisions

1. **E-commerce API Integration (TikTok/Shopee):**
   - **Hybrid Approach:** 100% reliance on Official APIs is impossible due to strict ecosystem limits (especially TikTok Shop). SoCom uses **Official APIs** for authorized critical actions (Order Sync, basic chat) combined with **Internal Headless Workers (Puppeteer/Playwright)** as a fallback to scrape realtime CCU, extract missing chat logs, and perform unauthorized bulk actions (e.g., auto-pinning without an official token).
2. **Stateful WebSockets for "Live Flying Desk":**
   - **Standalone Stateful Services:** AWS API Gateway WebSockets are too restrictive (2-hour timeouts, high costs per message). Instead, SoCom deploys **Dedicated Stateful Golang Pods (e.g., on ECS or standalone EC2 instances)** using solutions like Centrifugo or raw Gorilla WebSockets, load-balanced via a Network Load Balancer (NLB). This guarantees persistent, low-latency connections for hours-long streams without hitting serverless timeout walls.

---

## 8. How It Works

```text
1. PRE-LIVE (PLANNING & TEASING)
   Brand creates Campaign -> Smart Calendar finds matching Host & Room -> ROS is generated by AI.
   AI Content Factory creates teaser videos -> Auto-published to TikTok/FB 24h before.

2. GOING LIVE (EXECUTION)
   Host reads Prompter -> Mod handles Omni-channel inbox -> Director monitors Multi-view.
   Host accidentally says "Shopee" on TikTok -> AI transcribes -> Red flag flashes on Mod's screen instantly.

3. POST-LIVE (RECONCILE & GROWTH)
   Stream ends -> AI Reconcile grades the Host (85/100).
   Live-to-Shorts Pipeline auto-cuts 10 viral moments -> Scheduled for auto-posting over the next week to drive traffic to the next live.
```

---

## 9. Pricing

SoCom utilizes a **B2B SaaS Tiered Model**:

| Plan | Target Audience | Features | Pricing Model |
|---|---|---|---|
| **Starter** | Independent Streamers / Small Brands | Smart Calendar, ROS Editor, 1-Platform Mod View. | **$49/month** |
| **Pro Agency** | MCNs, Mid-size Agencies | Omni-channel, Basic AI Reconcile, Payroll automation, 5 Rooms. | **$299/month + Usage fees** |
| **Enterprise Growth** | Large MCNs, Top Brands | Full AI Content Factory, Live-to-Shorts, Dedicated Local Nodes for AI, Custom SSO. | **Custom Pricing** |

*Note: AI generations (Voice, Video, Transcribing) are billed on a credit system based on compute usage.*
