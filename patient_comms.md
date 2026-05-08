# .claude/rules/patient_comms.md — Patient Communications Rules

## The 7 Message Types

### 1. New Patient Welcome
- Trigger: NP visit confirmed in Axis
- Timing: Send within 2 hours of first visit
- Channel: Email (primary), SMS (optional follow-up at 24 hours)
- Tone: Warm, welcoming, brief
- Required elements: First name, next appointment nudge, WC contact info
- Compliance: Must NOT include health claims or treatment promises

### 2. Reactivation Outreach
- Trigger: Member inactive for 45+ days (no visit in 45 days)
- Timing: 45-day and 90-day touchpoints
- Channel: Email + SMS
- Tone: Friendly, no-pressure, focusing on the member's wellness routine
- Required elements: First name, days since last visit (do not specify), soft CTA to book
- Compliance: No guilt language; no clinical claims

### 3. Plan Renewal Reminder
- Trigger: Annual plan expiring within 30 days
- Timing: 30-day, 14-day, 7-day reminders
- Channel: Email (primary)
- Tone: Informative, value-focused
- Required elements: Expiration timing (general — "your plan renews soon"), CTA to visit clinic
- Compliance: No specific pricing in email; direct to WC in-clinic for plan terms

### 4. Missed Appointment Follow-Up
- Trigger: Scheduled appointment not checked in (Axis status: missed/no-show)
- Timing: Same-day (after close), or next morning
- Channel: SMS (primary), Email (backup)
- Tone: Caring, brief, easy CTA
- Required elements: First name, soft mention of missed time, easy rebook CTA
- Compliance: No clinical language; do not mention condition or reason for visit

### 5. Birthday/Anniversary Message
- Trigger: Patient birthday or membership anniversary within 7 days
- Channel: Email
- Tone: Celebratory, brief, genuine
- Required elements: First name, personalized milestone reference
- Compliance: No clinical claims; no promotional upsells in birthday messages

### 6. Membership Lapse Warning
- Trigger: Failed ARB (automatic recurring billing) detected
- Timing: Same-day notification
- Channel: Email + SMS
- Tone: Neutral, informative — NOT alarming or threatening
- Required elements: "Your payment method needs attention" language; CTA to visit clinic
- Compliance: DO NOT specify billing amounts, account numbers, or failed transaction details in message. Direct to in-clinic only.

### 7. General Wellness Check-In
- Trigger: Manual or scheduled campaign
- Channel: Email
- Tone: Warm, community-oriented, no sales pressure
- Required elements: Wellness tip or content value, soft visit CTA
- Compliance: No medical advice; wellness framing only

## Tone by Patient Segment

| Segment | Tone | Urgency | CTA Style |
|---------|------|---------|-----------|
| Active member (visited <30 days) | Affirming, brief | Low | Soft — "see you next time" |
| Lapsing member (31-60 days) | Caring, check-in | Low-medium | "We miss seeing you" |
| At-risk member (61-90 days) | Warm, value-forward | Medium | "Come back in, on us" (if promo applies) |
| New patient (first 30 days) | Welcoming, educational | Low | Next appointment nudge |
| Lapse/billing issue | Neutral, informative | Medium | In-clinic CTA only |

## Channel Rules
- Email: Maximum 2 per week per patient across all message types
- SMS: Maximum 1 per day per patient; plain text only; 160-char target
- Never send both email AND SMS for the same message type on the same day unless two-step protocol applies (welcome + 24hr SMS follow-up)
- No outreach after 8 PM or before 9 AM local time (ET)

## Personalization Tokens
Available tokens for message templates:
- {{first_name}} — Patient first name
- {{wc_name}} — Wellness Coordinator name (default: Gina)
- {{clinic_name}} — The Joint Chiropractic - Seminole City Center
- {{clinic_phone}} — Clinic phone number
- {{next_appt_date}} — If appointment exists in system
- {{membership_type}} — Plan type (Monthly Wellness, Annual Wellness, etc.)
- {{days_since_visit}} — DO NOT USE in patient-facing messages; internal only

## Pre-Send Checklist
Before any message is delivered:
1. Compliance gate PASS status confirmed
2. Patient opt-in status verified (no messages to opted-out patients)
3. Message type frequency limit checked
4. Personalization tokens resolved (no unreplaced {{tokens}})
5. Channel selected based on patient preference if on file
6. Time-of-day rule verified
