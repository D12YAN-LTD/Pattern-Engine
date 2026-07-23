Pattern Engine Specification
Checkpoint 1 — Power Move
Objective

Detect an institutional accumulation move that establishes a new trend.

Required Criteria
Rule	Default
Gain	≥30%
Duration	3–15 trading days
Start	Confirmed swing low
End	Highest high within move
Volume	≥1 accumulation day (≥2× SMA20 volume)
Accumulation Day

A day qualifies when:

Volume ≥ 2 × SMA20(volume)
Close > Open
Close in upper 50% of day's range (optional later)
Candidate Selection

If multiple qualifying moves exist:

Highest gain wins.
If tied, shortest duration wins.
If still tied, most accumulation days wins.
Output

CP1 returns:

Start Bar
Start Price

End Bar
End Price

Gain %

Bars

Accumulation Days

PASS / FAIL
What this gives us

Now the code has a fixed target.

Instead of:

"Does this look right?"

we ask:

"Does the code satisfy the specification?"

That's a much easier question to answer.

Step 2: Add Issues to GitHub

I'd create these issues:

✅ #1 Rewrite CP1 Engine
✅ #2 Visual Debug Overlay
✅ #3 Candidate Scoring
✅ #4 CP2 – Healthy Pullback
✅ #5 TradingView Screener

That gives us a visible roadmap.

Step 3: Tag the current state

Create a Git tag:

v0.30

This becomes our baseline.

If we break something in v0.31, we can always compare against v0.30.

Step 4: Start v0.31

Once the specification is committed, we'll write the new engine to the specification, not to our intuition.

One suggestion that I think will really help

As this project grows, I'd like us to separate business logic from presentation.

Instead of this:

Detection
↓

Table

↓

Drawing

I'd structure it like this:

Detection Engine
        │
        ├── Dashboard
        ├── Chart Visuals
        ├── Screener Output
        └── Alerts

That way, the detection engine computes the result once, and every other feature simply consumes that result. It avoids duplicated logic and makes future changes much easier.

My proposal

Let's make Specification First our development rule.

Every checkpoint (CP1–CP5) gets a written specification before we write any code. That gives us a stable target, reduces rewrites, and makes the Git history much more meaningful because each implementation can be checked against a clear definition. I think that discipline will save us a lot of time as the Pattern Engine becomes more sophisticated.



















Pattern Engine v0.20
CP2 – Orderly Pullback Functional Specification v1.0
Purpose

Determine whether a stock has paused in a healthy, orderly manner following a successful CP1 Power Move while preserving institutional sponsorship.

CP2 is based predominantly on Qullamaggie's methodology.

Entry Condition

CP2 starts when:

CP1 PASS
      ↓
CP1 LOCK
      ↓
Confirmed Pivot High
      ↓
CP2 ACTIVE
States
Idle
    ↓
Waiting
    ↓
Measuring
   ↙     ↘
PASS    FAIL
Waiting State

Wait for the first confirmed Pivot High after CP1 has locked.

When detected:

Store

Pivot High Price
Pivot High Bar

Transition to Measuring.

Measuring State

The pullback is now active.

During every bar we monitor:

Hard Rules
Rule 1

Maximum closes below the 8 EMA

0 = Excellent

1 = Acceptable

2 = FAIL
Rule 2

Maximum closes below the 20 EMA

0 = PASS

1 = FAIL

No exceptions.

Rule 3

Overall volume should contract during the pullback.

For Version 1 this is measured only.

It is not a fail condition.

Measurements

Record:

Pullback Start Bar
Lowest Low
Lowest Low Bar
Number of Closes below 8 EMA
Number of Closes below 20 EMA
Average Pullback Volume
Lowest distance from 20 EMA
Highest distance from 8 EMA

These are retained for future optimisation but do not affect pass/fail (except the EMA close counts).

Pass Condition

Version 1 uses Option B.

CP2 completes when:

Pullback

↓

Price stabilises

↓

First Close back above the 8 EMA

↓

CP2 PASS

Measurements freeze.

Control passes to CP3.

Fail Condition

CP2 fails immediately if either occurs:

Second Close below the 8 EMA

or

Any Close below the 20 EMA

The pattern is abandoned.

Important Design Decisions
No pullback percentage

Pullback depth (%) is intentionally ignored.

Trend behaviour is considered more important than retracement depth.

20 EMA

The 20 EMA is support, not a target.

A stock:

may remain above it
may touch it
may trade intraday through it

but it must not close below it.

Strong Stocks

CP2 does not require a deep pullback.

Very strong leaders that remain above the 8 EMA are still valid and simply transition through CP2 more quickly.

Execution

Intraday execution is outside the Pattern Engine.

The following remains a separate module:

30m Green Candle

↓

Break High

↓

Long Entry

↓

Stop below 30m candle
Guiding Principle

The Pattern Engine is not trying to identify every pullback.

It is trying to identify the pullbacks that Qullamaggie would still consider healthy enough to continue monitoring.
