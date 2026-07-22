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
