# Proposal: a one-currency cap for Forex (audit AU13) — PROPOSAL ONLY, nothing built
Written by Claude (Opus 5.5), Sat 21 Nov 2026 ~07:10 Brisbane. Not part of any locked version. Markus decides.

## The problem
The Forex rules allow up to 4 open trades at 0.5% risk each, on 7 pairs that all have the US dollar on one side. Four "different" trades can be one bet on the dollar.
- 17 Nov (Fable's audit): all four open trades were long the US dollar, about 1.5x the account.
- 21 Nov (read-only check on OANDA, 01:58 UTC): the same again. AUD/USD sold (51,857 units), USD/CHF bought (37,013), NZD/USD sold (64,601), EUR/USD sold (39,040). Every one of them is long the US dollar.

So the "at most 4 open" limit does not spread risk. One dollar move hits all four trades, and the four stops can all be hit together.

## Options (each would be a new dated strategy version, never a change to the running one)
1. **Net cap per currency.** The sum of all open positions' exposure to any one currency (counting USD in every pair) stays at or below a set multiple of the account, for example 1.0x. A new trade that would break it is cut down or skipped. This is closest to the real risk; it needs one number chosen in advance.
2. **Same-side count.** At most 2 of the 4 slots may be on the same side of the US dollar. Simple, easy to read on the page. It ignores sizes, and two big trades can still be most of the account.
3. **Correlation-scaled risk.** Shrink each new trade by how closely it moves with the trades already open. This is the most precise option and the hardest to check and explain.

## How to test it, without touching the live account
- Pre-register the version: the option, the number, and the pass bar, all written down before any test runs.
- Run it in the existing backtest beside FX-BT1, on the same data and costs including overnight financing. Count it as one more variant tried on this idea.
- If registered before the strategy lock (about 12–13 Dec), run it as a Shadow League line, for example FX-BT1-CAP, on fake money through 2027. Judge it by the Forex family pass bar agreed 20 Nov: above zero after costs and above FX-BT1.
- Only then could it be considered for the real Forex account, as a new version with its own start date.

## Recommendation
Option 1 (net cap per currency at 1.0x the account), tested first as a league line from the lock, with nothing changed on the live account in 2027. Option 2 is the fallback if a simpler rule is wanted.

UNKNOWN until tested: whether the cap helps or hurts returns. It lowers the chance of all four stops being hit together, but it also cuts the size of the bot's strongest single-direction calls.

## Decision needed from Markus
1. Yes or no to registering a capped Forex line in the league before the lock.
2. If yes: option 1 at 1.0x (recommended), option 1 with another number, or option 2.
