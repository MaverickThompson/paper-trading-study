# paper-trading-study

A 60-day live paper-trading study, pre-registered before it started.

## What this is

An independent implementation of a published strategy, run on a fixed schedule with every parameter frozen in advance, logging every signal it generates — including the ones it rejects.

The claim is narrow, and stating it plainly is the point: I implemented a published strategy without modification, pre-registered the protocol, ran it for a fixed window, and documented what happened. That is a replication study. The strategy is not mine.

## What this is not

Evidence that the strategy has edge. The protocol says so itself: 60 days yields an expected 25 to 45 closed trades, roughly an order of magnitude short of the sample needed to detect a 2 percent per-trade excess return. The study is underpowered by design. Any reading of the results as evidence of skill, in either direction, is a misreading.

It is also not an offer, a solicitation, or a track record. No outside capital is involved or invited.

## The pre-registration

PROTOCOL.md is the study document. It is frozen on commit — commit 4065efb is the timestamp of record — and it fixes in advance the universe, the two-layer selection process, and the entry and exit conditions; position sizing and risk limits, including where the study limits deliberately differ from live ones and why; two controls, SPY buy-and-hold and a random-selection portfolio drawn from the same survivor set with the seed recorded before day 1; and the full metric set, with n reported first and most prominently.

It closes with a section titled "What would make this study worthless", written before any data existed so that it cannot be rationalised afterward.

## Logs

signals.csv records every signal, including rejected candidates and system errors. Logging only executed trades produces survivorship bias and is indistinguishable from cherry-picking.

trades.csv records each position along with the thesis and the falsification condition written at entry, before the outcome is known. Those two fields are the point of the exercise: they turn a trade log into a record of judgment.

Both files are append-only. Corrections are new rows, never edits.

## Status

Pre-registration committed 2026-08-31. The study register in PROTOCOL.md Section 12 records the dry run, the window dates, and any amendments as they happen.
