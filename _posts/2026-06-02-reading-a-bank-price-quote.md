---
layout: post
title: "Reading a Bank Price Quote"
date: 2026-06-02
categories:
---

When a bank provides a forward rate quote, the price is typically built from three layers:
 
```
Forward rate = Spot rate + Swap points + Mark-up
```
 
## Swap Points
 
Swap points represent the **fair adjustment for the interest rate differential** between the two currencies over the contract period. They are derived from **interbank money market rates** (e.g. SOFR for USD, EURIBOR for EUR, TONAR for JPY) — not the bank's discretion.
 
These money market rates are closely anchored to central bank rates (e.g. Fed Funds Rate, ECB rate) but can deviate slightly due to liquidity conditions and supply/demand in the repo market.
 
- If the base currency has **higher** interest rates → negative swap points (forward discount)
- If the base currency has **lower** interest rates → positive swap points (forward premium)
This reflects the **cost of carry**: what it costs to hold the position until settlement.
 
**Example:**
- Spot USD/JPY = 150.00
- US interest rate: 5% | Japan interest rate: ~0%
- 3-month forward rate: ~148.50
- Swap points: **−150** (i.e. −1.50 in price terms)
The dollar trades at a forward *discount* because US rates are higher — you're compensated for giving up USD yield.
 
## Mark-up
 
The mark-up is the bank's **commercial margin**, layered on top of the theoretical fair rate (spot + swap points). It is usually embedded invisibly in the quoted rate rather than shown as a separate fee.
 
It covers:
 
- **Credit risk** — the possibility you default before settlement
- **Operational costs** — processing, compliance, hedging
- **Profit** — the bank's margin for providing the service
> Because the mark-up is hidden in the rate, comparing quotes across multiple banks is the most effective way to identify how much you are paying.
 
## Summary
 
| Component | What it represents | Who sets it |
|-----------|-------------------|-------------|
| Spot rate | Current market price | Interbank market |
| Swap points | Cost of time / interest rate differential | Interbank money markets (SOFR, EURIBOR, etc.) |
| Mark-up | Bank's commercial margin | The bank |
 
---
 
## Worked Example: EUR/CHF 3-Month Forward
 
**Scenario:** A Swiss exporter will receive €1,000,000 from a European client in 3 months and wants to lock in the EUR/CHF rate today to protect against the euro weakening against the franc.
 
**Given:**
- Spot EUR/CHF = 0.9500
- SARON (CHF money market rate) = 1.50%
- EURIBOR (EUR money market rate) = 3.50%
- Contract period = 90 days

**Step 1 — Calculate swap points**
 
Using the forward rate formula:
 
```
Swap points = Spot × (CHF rate − EUR rate) × (days / 360)
            = 0.9500 × (1.50% − 3.50%) × (90 / 360)
            = 0.9500 × (−2.00%) × 0.25
            = 0.9500 × (−0.005)
            = −0.00475  →  approx. −48 pips
```
 
Since EUR rates are higher than CHF rates, the EUR trades at a forward *discount* — you receive fewer francs per euro in the future.
 
**Fair forward rate (no mark-up):**
```
0.9500 − 0.0048 = 0.9452
```
 
**Step 2 — Bank applies mark-up**
 
The bank shaves ~8 pips off the rate as its margin:
```
0.9452 − 0.0008 = 0.9444  ← rate quoted to the client
```
 
**Step 3 — Settlement**
 
At maturity, the company delivers €1,000,000 and receives:
```
€1,000,000 × 0.9444 = CHF 944,400
```
 
Instead of the fair rate payout of:
```
€1,000,000 × 0.9452 = CHF 945,200
```
 
**The mark-up cost: CHF 800** — invisible in the rate, but real.
 
---
 
## Worked Example: USD/JPY 3-Month Forward
 
**Scenario:** A US exporter will receive ¥150,000,000 from a Japanese client in 3 months and wants to lock in the USD/JPY rate today to protect against the yen weakening against the dollar. The company will sell JPY and buy USD at maturity.
 
**Given:**
- Spot USD/JPY = 150.00
- SOFR (USD money market rate) = 5.00%
- TONAR (JPY money market rate) = 0.10%
- Contract period = 90 days

**Step 1 — Calculate swap points**
 
Using the forward rate formula:
 
```
Swap points = Spot × (JPY rate − USD rate) × (days / 360)
            = 150.00 × (0.10% − 5.00%) × (90 / 360)
            = 150.00 × (−4.90%) × 0.25
            = 150.00 × (−0.0123)
            = −1.8375  →  approx. −184 pips
```
 
Since USD rates are higher than JPY rates, the USD trades at a forward *discount* — the forward rate is lower than spot. However, this benefits the US company: they are **buying** USD, and they lock in today's higher rate before it drops further.
 
**Fair forward rate (no mark-up):**
```
150.00 − 1.84 = 148.16
```
 
**Step 2 — Bank applies mark-up**
 
The bank adds ~10 pips to the ask side as its margin:
```
148.16 − 0.10 = 148.06  ← rate quoted to the client
```
 
**Step 3 — Settlement**
 
At maturity, the company delivers ¥150,000,000 and receives:
```
¥150,000,000 ÷ 148.06 = $1,013,240
```
 
Instead of the fair rate payout of:
```
¥150,000,000 ÷ 148.16 = $1,012,556
```
 
**The mark-up cost: ~$684** — invisible in the rate, but real.
 
> Note how the forward discount works differently here than in the EUR/CHF example. The US company is *buying* the discounted currency (USD), so the lower forward rate still protects them — they secure a known USD amount rather than being exposed to JPY weakening further.
