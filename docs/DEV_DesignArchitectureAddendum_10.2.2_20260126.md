DEV\_DesignArchitectureAddendum\_10.2.2\_20260126  
Title  
Days Remaining Semantics — Zero-Line Cash Exhaustion (Restored to 10.1 Behavior)

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Status  
Final — Corrective Specification  
Applies to: V10.2 Design Architecture and all active implementations  
Restores: V10.1 zero-line daysOfCashRemaining behavior  
Supersedes: DEV\_DesignArchitectureAddendum\_10.1.3\_20260104 (OBSOLETE)

This addendum is normative for all references to daysOfCashRemaining  
until the V10.2 Design Architecture document is updated after implementation.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Purpose

This addendum:

    • Reverts daysOfCashRemaining semantics to the original V10.1 intent:  
          “days until cash \< 0”  
    • Declares any threshold-driven interpretation of daysOfCashRemaining invalid.  
    • Provides the binding contract that implementation MUST follow now,  
      so the V10.2 Design Architecture document can be corrected once dev is complete.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Authoritative Definition

daysOfCashRemaining represents:

    The number of days from the projection window start  
    until the first forecasted date where balance \< 0\.

It does NOT represent:

    • days until crossing an arbitrary warning threshold  
    • a user-tunable “safety buffer”  
    • a soft risk or “low balance” indicator

Zero-line semantics are authoritative:

    Cash exhaustion is defined strictly as balance \< 0\.

This restores the V10.1 behavior and invalidates the threshold-driven  
definition introduced in DEV\_DesignArchitectureAddendum\_10.1.3.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Computation Semantics

Input domain:

    • Projection window \[startDate, endDate\] as defined in the main V10.x spec.  
    • Per-day balances produced by ProjectionEngine.  
    • Threshold (warningThreshold) value in the request:  
        – still accepted  
        – used for colorState and “low balance” messaging only  
        – NOT used for daysOfCashRemaining

Algorithm (logical contract):

    1\. Let P be the ordered list of projection points for dates d ∈ \[startDate, endDate\],  
       each with balance b(d).

    2\. Find the earliest date d\* in P where b(d\*) \< 0\.

       • If such a date exists:  
           daysOfCashRemaining \= daysBetween(startDate, d\*)  
           (same day-difference convention used in the existing implementation.)  
       • If no such date exists within the window:  
           daysOfCashRemaining \= the current “no shortfall in horizon” sentinel value  
           already used when the projection never goes negative.

Edge cases:

    • If b(startDate) \< 0:  
          daysOfCashRemaining \= 0  
          (cash is already exhausted at the start of the window).

    • Free-tier rolling anchor behavior remains unchanged:  
          this addendum does not alter opening balance selection  
          or projection window construction.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Implementation Directive (MVP / V10.2)

To minimize change surface and preserve tested behavior:

    • ProjectionEngine SHALL compute daysOfCashRemaining as if the effective  
      threshold for Days Remaining were 0, regardless of the warningThreshold  
      supplied in the request payload.

    • threshold in the request continues to drive:  
          – colorState (red / yellow / green)  
          – any “low balance” messaging

    • daysOfCashRemaining MUST be computed solely against the zero-line  
      (balance \< 0), not against warningThreshold.

No other service or controller may override or post-process  
daysOfCashRemaining.

Any refactor MUST preserve the observable behavior of  
“days until balance \< 0” for all existing fixtures.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Threshold Semantics (Clarified for V10.2)

warningThreshold:

    • User-defined  
    • Drives:  
          – colorState \= yellow for 0 ≤ balance \< warningThreshold  
          – associated narration / “low balance” messages  
    • Does NOT affect:  
          – daysOfCashRemaining  
          – firstNegativeDate  
          – minBalance

danger condition:

    • Defined strictly as balance \< 0\.  
    • Drives:  
          – colorState \= red  
          – cash exhaustion semantics  
          – daysOfCashRemaining computation  
          – firstNegativeDate and minBalance

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Frontend Responsibility

Frontend MUST:

    • Treat summary.daysOfCashRemaining as:  
          “days until projected balance goes negative”  
      and ensure copy and labels reflect this meaning.

    • Continue to respect:  
          – threshold-driven color semantics (yellow/green)  
          – red for balance \< 0

Frontend MUST NOT:

    • Describe daysOfCashRemaining as “days until low balance”  
      or “days until crossing your warning threshold.”  
    • Re-derive or adjust daysOfCashRemaining using threshold.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Relationship to V10.1 and V10.2

    • Restores the V10.1 zero-line interpretation of daysOfCashRemaining.  
    • Supersedes and nullifies DEV\_DesignArchitectureAddendum\_10.1.3  
      and any text in V10.2 that defines or implies threshold-driven Days Remaining.  
    • Until the V10.2 Design Architecture document is updated, this addendum  
      is the controlling definition for daysOfCashRemaining.

All future edits to the V10.2 Design Architecture MUST incorporate this  
zero-line definition verbatim or by explicit reference.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Final Lock Statement

daysOfCashRemaining is a zero-line metric:

    It represents the number of days until projected cash balance \< 0,  
    independent of the user-configured warning threshold.

This behavior is restored from V10.1, is authoritative for V10.2,  
and MUST be implemented exactly as specified.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

End of Document

