# Systematic Debugging

Random fixes waste time and create new bugs. Find root cause first.

## Iron Law

**No fixes without root cause investigation.** If you haven't completed Phase 1, you cannot propose fixes.

## Phase 1: Root Cause Investigation

1. **Read error messages carefully** — full stack traces, line numbers, error codes. They often contain the answer.

2. **Reproduce consistently** — exact steps, every time. If not reproducible, gather more data — don't guess.

3. **Check recent changes** — git diff, new dependencies, config changes, environment differences.

4. **Instrument multi-component systems** — before proposing fixes, add diagnostic logging at each component boundary:
   ```
   For EACH boundary: log what enters, what exits, verify env/config propagation.
   Run once → see WHERE it breaks → investigate THAT component.
   ```

5. **Trace data flow backward** — where does the bad value originate? What called this? Keep tracing up until you find the source. Fix at source, not symptom.

## Phase 2: Pattern Analysis

1. **Find working examples** — similar code in the codebase that works
2. **Compare differences** — list every difference, however small
3. **Understand dependencies** — what config, environment, assumptions does the broken code need?

## Phase 3: Hypothesis and Testing

1. **Form one specific hypothesis** — "X is the root cause because Y"
2. **Test minimally** — smallest possible change, one variable at a time
3. **Verify** — worked? → Phase 4. Didn't work? → new hypothesis. Don't pile fixes on top.

## Phase 4: Fix

1. **Write a failing test** reproducing the bug
2. **Implement single fix** addressing root cause — no "while I'm here" improvements
3. **Verify** — test passes, no regressions, no new warnings

## Common Traps

| Trap | Reality |
|------|---------|
| "Quick fix, obvious cause" | Obvious causes have root causes too |
| "Let me try this..." (no hypothesis) | Guessing creates new bugs |
| "It works now" (no understanding why) | It'll break again |
| "Too simple to debug systematically" | Simple bugs have systematic fixes |
| Multiple fixes at once | Can't tell which one worked |
