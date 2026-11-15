# Avaloka Claim Verifier: warnings copied from my chat session (15 Nov 2026)

These warnings appeared under Avaloka's answers in the chat. The text below was copied from the chat at the time. Times are IST, from the chat timestamps.

## 6:39 AM: pdays comparison ("Compare subscription rate for pdays = 999 vs pdays < 999.")

2 of 4 claims in this answer did not match the computed results. Treat this answer as unchecked.

- Figure not in the results: not found in the computed results: cites 54.6 %, which does not appear in the computed evidence within tolerance
- Significance without a test: goes beyond the evidence: uses 'significant' with no p-value, confidence interval or test statistic in the evidence; readers will hear a statistical claim

## 7:05 AM: clean rerun (asked for only the rates and intervals, no derived numbers, no "significant")

pdays = 999: 9.26% (95% CI: 8.98% - 9.55%)
pdays < 999: 63.83% (95% CI: 61.38% - 66.21%)

No warning was shown.

## 7:06 AM: job with row counts

1 of 6 claims in this answer did not match the computed results. Treat this answer as unchecked.

- Contradicts the result's ordering: says 'lowest' at 875, but the min of 'row_count' in the computed result is 330

## 7:09 AM: month with row counts

2 of 5 claims in this answer did not match the computed results. Treat this answer as unchecked.

- Contradicts the result's ordering: says 'smallest' at 13,769, but the min of 'total_rows' in the computed result is 182
- Contradicts the result's ordering: says 'highest' at 182, but the max of 'total_rows' in the computed result is 13769

Avaloka also noted this answer was served by a fallback model (the primary was rate-limited).

## 7:32 AM: poutcome with row counts

2 of 5 claims in this answer did not match the computed results. Treat this answer as unchecked.

- Contradicts the result's ordering: says 'highest' at 1,373, but the max of 'total_rows' in the computed result is 35563
- Figure not in the results: not found in the computed results: cites 51 %, which does not appear in the computed evidence within tolerance
