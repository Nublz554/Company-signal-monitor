COMPANY SIGNAL MONITOR  —  an OSINT skill for Hermes Agent

Fans subagents across a company's public record and merges it into one cited report.
Every source is free and keyless. No API keys, no accounts, no paid tier.

WHAT IT PULLS
- SEC EDGAR: every filing, 8-K items decoded into named events, full XBRL history
- Who owns who: 10-K Exhibit 21 subsidiary maps, 13D/13G crossings, 13F quarter-over-quarter
  diffs, insider Form 4 clusters, proxy ownership tables
- Deal history: 8-K 2.01 (closed) vs 1.01 (agreed), S-4, 425, tender offers
- USAspending federal awards, with agency concentration
- Price behaviour + the cluster test: is the move the company, or the whole factor?
- Satellite imagery of company facilities - dated Sentinel-2 pairs, auto-cropped and stitched
- News volume, category mix, Wikipedia pageview velocity
- Brand impersonation on the company's own handles

IT DOES NOT DO PEOPLE
The subject is an organization. A person appears only as a named filing role - a CFO resigning
is an event about the company. No addresses, no family, no associates, no movements, no
dossiers. Enforced in the contract file every subagent loads first, with a hard out-of-scope
return path that makes a child stop and file a gap rather than improvise.

TWO THINGS WORTH YOUR TIME

1. The score is not predictive, and it says so. There is a point-in-time backtest in scripts/ -
   40 large caps across 11 sectors, 4 years, forward 90-day excess return over SPY, no lookahead.
   Monthly sampling suggested significance (t = -2.51). Non-overlapping sampling killed it
   (t = -1.74, not significant) and the buckets went non-monotonic. The skill documents its own
   failed result instead of hiding it, and the experiment ships so you can rerun it.

2. Satellite at 10m cannot count cars, and it refuses to try. Measured: given the two crops
   described in separate calls, the vision pass invented construction that was not there. Given
   the same two crops stitched side by side in ONE frame, it correctly returned UNCHANGED on all
   five features and refused to estimate lot occupancy with the right reason. The composite is
   the honesty mechanism, not a convenience.

VERIFIED LIVE, NOT CLAIMED
- TSLA peaked 2025-12-16 while SPY and QQQ peaked 2026-10-06 -> the move is idiosyncratic, not
  sector-wide. -17.6% from peak, -9.1% on the year, 35% realized vol.
- PLTR pulled $32.1M in federal awards over 90 days, 100% from a single agency (DHS), down from
  $73.8M the prior window. Tesla pulled $0.00.
- 12 subagents ran the full module set in 9m24s on a free model.

INSTALL
Drop the folder into your skills directory:
    company-signal-monitor/   ->   ~/.hermes/skills/research/

Requires: Python 3.11+, requests (rasterio + pillow for the imagery module).
sherlock-project optional, brand module only.

NOTE ON RATE LIMITS
This fans out. SEC wants a descriptive User-Agent with a contact address (the scripts take one).
GDELT allows one request per 5 seconds. Don't point twelve agents at Yahoo on a loop; the
scripts already back off on 429/503 - a hand-rolled version won't.

MIT. Author: T_1
