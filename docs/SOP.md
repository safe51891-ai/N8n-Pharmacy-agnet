# SOP: working the automated intake queues

**Audience:** intake specialists, PA coordinators, customer care.
**System:** n8n "Pharmacy Intake Triage Agent", table `pharmacy_intake_cases`.

## Daily
1. Open the cases table, filter by your queue.
2. **benefits_verification**: data is pre-filled. Confirm member ID against the card image, run the benefits check.
3. **prior_authorization**: check diagnosis and previous therapies on the original document before submitting. URGENT cases first.
4. **missing_info_outreach**: the `status_message` already lists what is missing. Review, send to the prescriber office, set a 24 h follow-up.
5. **customer_care**: read `next_action`, open the original letter, call the patient.
6. **manual_review**: open the original document, classify it, enter data, and add a note on why the system could not (this feeds prompt improvements).

## Rules
- Always open the original document before any payer submission or patient call. The extracted data is a draft.
- Never edit `extracted_json`; correct values in the working fields so QA can compare.
- If extracted data is wrong, tag the case `ai_error` with the field name.

## Weekly QA (intake lead, 15 min)
- Pull 20 random cases, compare fields to the source document, record errors per field.
- Check `automation_runs`: manual_review share, average confidence, average cycle time.
- Escalate to the automation owner if field error rate > 3% or manual_review > 15%.
