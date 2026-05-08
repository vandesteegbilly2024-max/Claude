# .claude/rules/compliance.md — Compliance Gate Rules

## Purpose
This file defines the compliance rules enforced by the Seminole AI Operator for all patient-facing, public-facing, and internal outputs. Rules are loaded by the compliance gate agent and the deterministic gate (src/compliance/gate.py).

## Rule Priority
1. BLOCKED (Hard Block) — Output is rejected and cannot be delivered. No override permitted.
2. DISCLAIMER_REQUIRED — Output can proceed only with mandatory disclaimer text appended.
3. HUMAN_REVIEW — Output is flagged and held for WC (Gina Villegas) review before sending.
4. PASS — Output cleared for delivery.

## Hard Blocks — Clinical Language

The following language is BLOCKED in all outputs regardless of context:

### Diagnosis Claims
- "chiropractic treats [condition]"
- "adjustment cures", "cures back pain", "cures headaches"
- "fixes your spine", "corrects your posture permanently"
- "heals [condition]"
- "eliminates [condition]"
- "diagnosis", "diagnosed with", "you have [condition]"
- Any claim that chiropractic treats a specific medical condition

### Treatment Efficacy Claims
- "proven to treat", "clinically proven"
- "scientifically proven" (unless citing a specific peer-reviewed study by full citation)
- "guaranteed results", "guaranteed relief"
- "will fix", "will cure", "will eliminate"
- Specific outcome promises: "you'll feel better in X days"

### Medical Advice
- "you should take [medication]"
- "stop taking your medication"
- "you don't need surgery"
- Any advice that substitutes chiropractic for medical treatment

## Hard Blocks — Account Change Language

The following language is BLOCKED in patient communications because account changes must be handled in-clinic by WC only:

- "cancel your plan", "how to cancel"
- "downgrade your plan"
- "change your billing"
- "update your payment method" (unless directing to in-clinic visit)
- "freeze your account"
- "pause your membership"
- Any language that facilitates account changes without in-person WC involvement

Exception: Directing a patient to "come in and speak with our Wellness Coordinator" is APPROVED.

## Disclaimer Triggers

When the following topics are detected, the output must include the appropriate disclaimer:

### Wellness/Health Topics
Trigger: Any mention of health outcomes, pain relief, wellness benefits
Disclaimer: "Chiropractic care is not a substitute for medical treatment. Results may vary. Consult your physician for medical conditions."

### Plan/Pricing Mentions
Trigger: Any mention of plan costs, billing, or membership terms
Disclaimer: "Plan terms and pricing subject to change. See your Wellness Coordinator for current options."

### New Patient Promotions
Trigger: Any promotional offer for new patients
Disclaimer: "Offer valid for new patients only at participating locations. See clinic for details."

## Approved Phrasing (Safe Language)

Use these patterns instead of blocked language:

| Instead of | Use |
|------------|-----|
| "treats back pain" | "may support back wellness" or "many members report back comfort improvements" |
| "cures headaches" | "wellness care for overall health" |
| "will feel better" | "many of our members notice improved comfort" |
| "fixes your spine" | "spinal adjustment as part of your wellness routine" |
| "cancel your plan" | "speak with our Wellness Coordinator in clinic" |
| "clinically proven" | "wellness benefits reported by members" |

## Output States

```
BLOCKED     — Do not send. Log reason. Notify WC if patient-facing.
PASS        — Cleared for delivery via appropriate channel.
DISCLAIMER  — Append required disclaimer text before delivery.
REVIEW      — Hold for WC review. Do not auto-send.
```

## Florida-Specific Notes
- FL Statute 456.52: Chiropractic advertising must not be deceptive or misleading.
- FL Statute 460.4167: Scope of practice limitations apply to all communications.
- IRF (Incident Report Form) language: All IRF-related communications require human review before sending.
- Workers' Comp patients: No promotional communications. Status communications only.

## Enforcement
- Deterministic gate (src/compliance/gate.py) enforces BLOCKED patterns via regex before any LLM review.
- LLM compliance gate (prompts/agents/compliance_gate_prompt.md) handles DISCLAIMER and REVIEW cases.
- All compliance decisions are logged to Notion compliance log DB with timestamp, output type, and rule triggered.
