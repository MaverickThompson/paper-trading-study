PROTOCOL.md
60-DAY LIVE PAPER-TRADING STUDY — PRE-REGISTRATION

Author: Maverick
Pre-registration written: 2026-08-07
Status: FROZEN on commit. No parameter in this document may change during the study window.

This is a research protocol. It is not an offer, solicitation, or advertisement of any
investment service. No outside capital is involved or invited. Nothing here is financial advice.

================================================================
0. TIMESTAMP INTEGRITY — READ FIRST
================================================================

The credibility of a pre-registration rests on provable precedence: the protocol must
demonstrably exist before the data does.

INTENDED MECHANISM: a git commit, tagged v1.0-prereg, whose timestamp is independently
verifiable by any reader.

ACTUAL MECHANISM: this protocol was committed verbatim to a public git
repository at github.com/MaverickThompson/paper-trading-study before the
study window opened. The commit hash and timestamp are publicly verifiable
and constitute the pre-registration of record.

This is a WEAKER form of evidence. Drive revision history is visible to the account owner
and can in principle be circumvented by creating a new document. A git commit hash cannot.

REQUIRED REMEDY before Week 1 begins: initialise a repository, commit this file verbatim,
tag v1.0-prereg, and record the commit hash in Section 12 of this document. Until that is
done, the study is running without a hard pre-registration and the final paper must say so
in the Limitations section, in those words.

If the remedy is not completed, the paper reports: "Pre-registration was timestamped by
cloud document creation rather than cryptographic commit. Readers should weight the
pre-registration claim accordingly."

================================================================
1. RESEARCH QUESTION
================================================================

PRIMARY (engineering / methodology):
Can a retail-scale, connector-driven equity research pipeline be specified in advance,
operated on a fixed schedule for 60 calendar days without parameter modification, and
produce a complete, auditable record of every signal it generated — including the ones
it rejected?

SECONDARY (preliminary, underpowered):
What were the realised performance characteristics of that system over the window,
relative to two benchmark controls?

WHAT THIS STUDY CANNOT ANSWER:
Whether the strategy has edge. Sixty days at the specified trade frequency yields an
expected n of roughly 25-45 closed positions. Detecting a 2% per-trade excess return
against a per-trade excess-return standard deviation of ~15% requires approximately
(1.96 x 0.15 / 0.02)^2 = 216 independent observations. This study is underpowered by
roughly an order of magnitude BY DESIGN, and no result within it — positive or negative —
constitutes evidence of skill or its absence.

Any reading of the results as evidence of edge is a misreading of the protocol.

================================================================
2. STUDY WINDOW
================================================================

Duration: 60 calendar days of market operation.
Start: the first regular US market session following a successful dry run (Section 10).
End: 60 calendar days after start, inclusive. Positions open at the end of the window are
     marked to market on the final session and reported separately from closed trades.
No extension. No early stop. The window does not move for any reason, including the
publication deadline described in Section 11.

================================================================
3. UNIVERSE
================================================================

DEFINITION (fixed):
All US-listed common equities satisfying ALL of the following at the time of screening:
  - Market capitalisation >= $2,000,000,000
  - Average daily volume >= 1,000,000 shares
  - Positive trailing twelve-month diluted EPS
  - Listed on NYSE, NASDAQ or NYSE American
  - Not an OTC or pink-sheet listing
  - Not an ETF, closed-end fund, ADR of a foreign private issuer, SPAC, or unit

EXPECTED SIZE: approximately 1,200-1,800 names.

ACCESS METHOD: the universe is not enumerable directly through available connectors.
It is sampled each session via Webull ranking endpoints:
  - get_52_week_high_low (NEAR_LOW, NEW_LOW), minimum 4 pages of 40, sorted by market value
  - get_most_active (RELATIVE_VOLUME_10D)
  - get_gainers_losers (MONTH_1, WEEK_52)
  - get_high_dividend
  - get_market_sectors_detail

MINIMUM COVERAGE PER SCANNING SESSION: 200 distinct symbols, across at least three of the
above endpoints. The symbol count is logged every session. A session that cannot state its
symbol count is recorded as a FAILED SCAN in signals.csv and reported in the weekly review.

DOCUMENTED LIMITATION: this is a sampled universe, not a complete one. The ranking
endpoints are themselves filters, which introduces a selection bias toward names at price
extremes or with unusual volume. The paper must state that the effective universe is
"names surfaced by momentum- and value-oriented ranking endpoints" rather than "all US
equities above $2B", and must not claim broad-market coverage.

================================================================
4.0 PROVENANCE AND ATTRIBUTION
================================================================

THE STRATEGY IS NOT ORIGINAL TO THIS STUDY.

Source: [NAME / AUTHOR / SITE]
Location: [URL OR CITATION]
Accessed: [DATE]

The operator did not design this strategy. It was found published and is
being implemented as specified. The contribution of this study is therefore
NOT the strategy. It is:
  - independent, pre-registered implementation of a published method
  - a complete signal-level record including rejected candidates
  - honest reporting of what happened over a fixed 60-day window

The paper must state this in the abstract and introduction, in plain terms.
It must NOT describe the strategy as designed, developed or discovered by
the operator. The defensible claim is:

  "I implemented a published strategy without modification, pre-registered
   the protocol, ran it for 60 days, and documented the outcome."

That is a replication study. Replication is undersupplied in retail trading
literature and is a legitimate contribution. Misrepresenting it as original
work is not, and would invalidate the study on discovery.

DEVIATIONS FROM THE SOURCE, if any, are listed here and nowhere else:
  [NONE / list each with reason]
The system is a TWO-LAYER process. Both layers are logged separately.

LAYER 1 — MECHANICAL FILTER (fully reproducible)
A candidate advances only if ALL hold:
  a. Passes the universe definition in Section 3.
  b. Price is within 15% of its 52-week low.
  c. Trailing twelve-month diluted EPS is positive.
  d. Most recent quarter's diluted EPS is positive.
  e. Trailing P/E is between 3 and 40 inclusive.
  f. One share costs <= 25% of portfolio net liquidation value.
  g. Implied share count at the position-sizing rule (Section 6) is >= 1.

LAYER 2 — ADVERSARIAL ANALYSIS (discretionary; see limitation below)
Candidates surviving Layer 1 are analysed using a fixed procedure:
  1. Neutral fact collection. Quarterly financials pulled alongside annual, six periods
     minimum. Annual-only analysis is prohibited.
  2. Bull case, written and committed before the bear case is begun.
  3. Bear case, constructed independently from the same facts, not as rebuttals.
  4. Adjudication on which case requires fewer assumptions.
  5. Insider transactions checked. Acquisitions recorded at $0.00 are grants or vesting and
     are NOT purchases. Open-market buying is signal; routine selling largely is not.
  6. If the security pays a dividend, coverage is verified against FORWARD earnings.
     A yield above 8% is presumed to indicate an expected cut until shown otherwise.
  7. For REITs and BDCs, EPS-based analysis is explicitly inapplicable. FFO/AFFO, NAV and
     distributable cash flow are substituted, and the substitution is logged.
  8. A single anomalous quarter (net margin more than 3x the trailing four-quarter median)
     invalidates any trailing multiple derived from it. This must be checked before any
     "cheap" characterisation is recorded.

MATERIAL LIMITATION, TO BE STATED PROMINENTLY IN THE PAPER:
Layer 2 contains irreducible discretionary judgment. It is a procedure, not an algorithm.
A third party following this protocol would not necessarily produce identical decisions
from identical inputs. This means the study is NOT fully pre-registered in the sense the
term carries in clinical trials, and the paper must not claim that it is.

The mitigation, and the reason the design is still worth running: Layer 1 and Layer 2
outcomes are logged separately, so the paper can report how frequently Layer 2 rejected
what Layer 1 accepted. That override rate is itself a reportable result and is one of the
more interesting quantities this study can produce.

================================================================
5. ENTRY AND EXIT CONDITIONS (FROZEN)
================================================================

ENTRY — all conditions required:
  - Layer 1 passed, Layer 2 concluded in favour.
  - A written thesis exists containing: entry zone, stop, Target 1, Target 2,
    predicted probability, and a falsification condition — all recorded BEFORE entry.
  - Stop derived from 2x ATR(14) or a structural level (prior swing low), whichever is
    tighter. Round-number stops are prohibited.
  - Reward-to-risk to Target 1 >= 2.0 : 1, computed with entry at the ASK and exits at
    the BID.
  - Predicted probability >= breakeven probability + 10 percentage points, where
    breakeven = 1 / (1 + R:R).
  - Expected value positive: EV = p x reward - (1 - p) x risk.
  - Resulting position within all Section 6 limits.
  - No scheduled earnings release within 48 hours of entry.
  - Price inside the recorded entry zone at the time of the fill.

EXIT — whichever occurs first:
  - Stop touched. Exit at the next available bid. Stops are NEVER widened.
  - Target 1 touched. 50% of the position closed. Stop on the remainder moves to entry.
  - Target 2 touched. Remainder closed.
  - Falsification condition observed. Full exit regardless of unrealised P/L.
  - Time stop: 45 calendar days from entry with neither target nor stop reached.
  - End of study window: marked to market, reported separately from closed trades.

No discretionary exits. No exceptions.

================================================================
6. POSITION SIZING AND RISK LIMITS (FROZEN)
================================================================

Simulated starting capital: $100,000.

  Risk per trade                       1.0% of current net liquidation value
  Maximum concurrent positions         10
  Maximum total open risk              10.0% of net liquidation value
  Maximum single-position weight       25%
  Maximum single-sector weight         40%
  Minimum cash reserve                 10%
  New positions per week               uncapped
  Consecutive-loss review trigger      none (see note)

  shares = floor( (0.01 x NLV) / (entry_ask - stop) )
  Rounding is always DOWN. Rounding up breaches the risk cap by construction.

DELIBERATE DEVIATION FROM THE LIVE ACCOUNT, DECLARED IN ADVANCE:
The operator's live trading rules specify a 4% total open risk cap, a 2-per-week new
position cap, and a mandatory review after 3 consecutive losses. This study raises the
open-risk ceiling to 10%, removes the weekly cap, and removes the consecutive-loss trigger.

REASON, RECORDED BEFORE ANY DATA EXISTS: at 10 concurrent positions each risking 1%, a 4%
aggregate ceiling is arithmetically unreachable — the two constraints are mutually
inconsistent. The weekly cap and loss trigger exist to protect a small live account from
behavioural overtrading; neither is a strategy parameter, and both would suppress sample
size in a study where no capital is at risk. Removing them raises n without altering any
entry or exit condition.

The paper must state that study limits differ from live limits, and why. It must NOT
present study results as representative of what the live account would have done.

================================================================
7. EXECUTION AND FILL MODEL
================================================================

INTENDED: Alpaca paper-trading account, with broker-side order records.

ACTUAL AT TIME OF WRITING: no Alpaca connector exists in the available connector registry,
and no paper-trading account is exposed through the connected brokerage API. Adding Alpaca
as a custom MCP server is possible and remains the preferred path.

FALLBACK FILL MODEL, used unless and until Alpaca is connected:
  - Entry filled at the quoted ASK at signal time.
  - Exit filled at the quoted BID at exit time.
  - Stop exits filled at the BID at the first observation at or below the stop.
  - Commissions: $0.00 (matching the operator's live commission-free broker).
  - No partial fills. No queue position. No market impact.
  - Quote timestamps recorded with every fill. Stale or out-of-session quotes are logged
    as such and the fill is deferred to the next regular session.

DOCUMENTED LIMITATIONS — to appear in the paper's Limitations section verbatim:
  - Fills are idealised. There is no slippage beyond the quoted spread, no partial fill
    risk, no queue priority, and no market impact.
  - Without a broker-side paper account, there is no independent execution record. Fills
    are computed by the study's own software from quoted prices. This is materially weaker
    evidence than broker-confirmed paper fills and a reader is entitled to discount it.
  - Quote data is sourced from a retail brokerage API. Coverage, latency and consolidation
    characteristics are not documented by the vendor at the level a research paper would
    prefer. Intraday timing precision should not be assumed.
  - Paper results systematically overstate live results. This is a known, general effect
    and is not corrected for here.

================================================================
8. BENCHMARKS (FROZEN)
================================================================

CONTROL 1 — SPY BUY-AND-HOLD
$100,000 into SPY at the opening price of day 1, held to the final session. Total return,
Sharpe and maximum drawdown computed over the identical window.

CONTROL 2 — RANDOM SELECTION PORTFOLIO
On each session the strategy opens a position, a paired random control position is opened:
  - Drawn uniformly from the Layer 1 survivor set for that session, excluding the name
    actually selected.
  - Identical dollar risk, identical stop distance in ATR terms, identical exit rules.
  - If the Layer 1 survivor set has fewer than 2 members, no control is drawn and the
    omission is logged.
  - Random draws use a seed fixed and recorded in Section 12 before day 1, so the control
    portfolio is reproducible.

PER-TRADE EXCESS RETURN, the primary reported quantity:
  Excess_i = R_trade_i - R_SPY(identical entry and exit dates)
Reported as: cumulative excess, mean excess, hit rate versus SPY, and
  t = mean(Excess) / (sd(Excess) / sqrt(n))
The t-statistic is reported for completeness and will almost certainly be insignificant.
Reporting an insignificant t-statistic honestly is a feature of this study, not a failure.

CONFOUND TO BE STATED: beating SPY is not evidence of stock-selection skill if the
portfolio carries factor exposure. Position beta is recorded at entry, and the paper must
note that no factor-model attribution was performed.

================================================================
9. METRICS REPORTED (FROZEN — full set, every week and in the paper)
================================================================

  n (closed trades)                    reported first and most prominently
  Total return
  Annualised return                    labelled "extrapolated from 60 days; not a forecast"
  Sharpe ratio                         daily returns, annualised, risk-free rate stated
  Maximum drawdown
  Ulcer index
  Win rate
  Average win vs average loss, in R
  Expectancy in R                      E = W x avgWin_R - (1-W) x avgLoss_R
  Payoff ratio
  Tail ratio                           largest loss / average win
  Exposure                             percent of window with capital deployed
  Turnover
  Cumulative excess vs SPY
  Mean per-trade excess vs SPY
  t-statistic of excess returns
  Random control comparison
  Brier score                          over resolved probability predictions
  Layer 2 override rate                fraction of Layer 1 survivors rejected by Layer 2
  Signal count, taken and rejected
  System uptime / missed sessions

Every report containing any return figure must state n adjacent to it, and must state that
results are not statistically significant at this sample size.

================================================================
10. LOGGING (SCHEMAS FROZEN — NEVER CHANGE)
================================================================

signals.csv
  timestamp, ticker, signal_type, triggered_rule, action_taken,
  reason_if_rejected, price_at_signal, notes

trades.csv
  entry_timestamp, ticker, direction, entry_price, size, thesis_at_entry,
  invalidation_condition, exit_timestamp, exit_price, pnl, pnl_pct,
  exit_reason, was_thesis_correct

RULES:
  - EVERY signal is logged, including rejected ones and errors. Logging only executed
    trades produces survivorship bias and is indistinguishable from cherry-picking.
  - thesis_at_entry and invalidation_condition are written BEFORE the outcome is known.
    They are the point of the exercise; they convert a trade log into a record of judgment.
  - Append-only. Corrections are new rows with a note referencing the original. No row is
    ever edited or deleted.
  - Failed scans, connector outages and missed sessions are logged as signal rows with
    action_taken = "SYSTEM_ERROR".

DRY RUN REQUIREMENT: one full session must run end to end and populate both files
correctly before day 1. A dry run that produces no rows does not count as passing.

================================================================
11. AMENDMENT PROCEDURE
================================================================

Parameters are frozen. The only permissible changes are corrections to defects that
prevent the protocol from executing as written — a broken connector, a logging bug, a
data-feed failure.

Any such change requires, in this order:
  1. The clock stops.
  2. An entry in the amendment log (Section 12) recording the defect, the change, the
     reason, and the timestamp.
  3. A declared decision to either restart the window or mark a v1/v2 break in results.
  4. The change is reported in the paper's "What broke" section at full length.

EXPLICITLY PROHIBITED, WITHOUT EXCEPTION:
  - Any parameter change motivated by results.
  - Any parameter change motivated by a deadline.
  - Any change intended to produce more trades, faster results, or better-looking data
    before any date, including the publication deadline falling mid-window.

A publication deadline has no bearing on how the system trades. If the temptation arises,
the answer is no. A quietly tuned parameter is the single most common mechanism by which
studies of this kind become fiction, and it is usually rationalised at the moment it
happens rather than planned.

No counterfactuals. The phrase "would have returned" does not appear in any output of this
study. The system did what it did.
================================================================
12. STUDY REGISTER (append-only)
================================================================

PRE-REGISTRATION
  Drafted:            2026-08-07
  Committed:          2026-08-31
  Repository:         github.com/MaverickThompson/paper-trading-study
  Commit of record:   4065efb38484c2ce866a6437f8a5da26a7ae4e1c
                      ("add research protocol for 60 day trading study")
  Correction commit:  87695b83fc262ffaec481b78037d4199d9c50f4f
                      (Section 0 updated to reflect that the repository
                       now exists; original text preserved in history)
  Tag:                [pending — v1.0-prereg]
  Random control seed: 764960210
                      Generated 2026-08-31T05:26:10Z via Python
                      secrets.randbelow(10**9), before any trade existed.

  NOTE ON DATES: the drafting date is asserted, not provable. The binding
  timestamp is the commit date of 4065efb. Any claim of precedence rests
  on that commit, not on the drafting date.

EXECUTION VENUE
  Alpaca paper account PA3CS6MXKSX5, verified ACTIVE with $100,000 cash
  and zero open positions on 2026-08-31, before day 1.

DRY RUN
  Date:               [pending]
  signals.csv rows:   [pending]
  trades.csv rows:    [pending]
  Result:             [pending]

STUDY WINDOW
  Day 1:              [pending]
  Day 60:             [pending]

AMENDMENT LOG
  2026-08-31 — Section 7 fill model.
    Defect: at pre-registration no Alpaca connector was available, so the
    protocol specified a fallback model of study-computed fills from quoted
    bid/ask.
    Change: Alpaca paper account connected and authenticated. Fills will be
    broker-executed with independent order IDs and timestamps. The Section 7
    fallback model is superseded; its caveat about "no independent execution
    record" no longer applies.
    Reason: the intended mechanism became available. No entry, exit, sizing
    or universe parameter was altered.
    Timing: before day 1. No v1/v2 break required.

================================================================
13. WHAT WOULD MAKE THIS STUDY WORTHLESS
================================================================

Recorded in advance so that it cannot be rationalised later:

  - Changing a parameter after seeing results, for any reason.
  - Logging only the trades that were taken.
  - Reporting return without n.
  - Claiming edge, or implying it through selective emphasis.
  - Extending or truncating the window.
  - Editing a prior log row or weekly review instead of appending a correction.
  - Writing any counterfactual.
  - Presenting this as a fund, a track record, or a solicitation.

If any of the above occurs, the honest action is to report it in the paper rather than
conceal it. A study that documents its own contamination is still useful. One that hides
it is not.

END OF PROTOCOL — FROZEN ON COMMIT

