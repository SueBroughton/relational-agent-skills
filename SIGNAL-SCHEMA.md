# Relational Signal Schema

**Status:** Draft specification, version 0.1. Proposed, not validated. See Limitations.

A structured format for relational detections emitted by agent skills, and for the human dispositions that follow them.

---

## Why this exists

Skills can detect relational degradation — dependency forming, trust eroding, oversight becoming procedural. What they cannot do is decide what should happen next, and they cannot enforce anything. A skill file is an instruction to a model. A model can lose it in a long context, be argued out of it, or fail to fire it.

So the separation is deliberate:

- **The skill observes.** It reports what it saw, how serious it appears, and what class of response would be appropriate.
- **The organisation decides.** A policy layer outside the model maps severity to action according to whoever holds authority in that deployment.
- **The disposition is recorded.** What fired, what was decided, by whom.

The third part matters as much as the first two. A lever that nobody pulls is not control. If detections fire and are routinely dismissed, oversight has become procedural again — one layer up from where it started. The disposition record is the only evidence that the lever is live.

---

## Part 1 — The detection event

Emitted by a skill when an indicator fires. Structured so a policy layer can route it without parsing prose.

| Field | Required | Description |
|---|---|---|
| `event_id` | yes | Unique identifier for this detection. |
| `timestamp` | yes | When the detection fired (ISO 8601). |
| `skill_id` | yes | Which skill fired, by library number and name. |
| `indicator` | yes | The specific named pattern observed, not the general category. |
| `observation` | yes | What was actually seen, in plain language. The evidence, not the inference. |
| `severity` | yes | One of the four levels below. |
| `response_class` | yes | The class of response the skill suggests. Advisory only — the policy layer is not bound by it. |
| `basis` | yes | Why this was judged to meet the indicator. Makes the detection contestable. |
| `subject_ref` | yes | Reference to the person or partnership observed. See Privacy. |
| `session_ref` | no | Where in the interaction history this occurred. |

The separation of `observation` from `basis` is intentional. One is what happened; the other is the reasoning that turned it into a flag. A reviewer needs both to disagree properly.

---

## Part 2 — Severity levels

Four levels, defined once and applied identically across every detection skill. Consistency matters more than granularity: a policy layer can only route reliably if severity means the same thing whichever skill emitted it.

| Level | Name | Meaning | Typical policy mapping |
|---|---|---|---|
| **S1** | Note | A pattern consistent with early relational change. Not actionable alone; meaningful in aggregate. | Log only. |
| **S2** | Advisory | A pattern that would benefit from the human's attention within the session. | Surface to the person in the partnership. |
| **S3** | Escalation | A pattern suggesting oversight quality may be compromised. Requires someone outside the dyad. | Notify a role with standing to intervene. |
| **S4** | Suspension candidate | A pattern suggesting the partnership should not continue at current autonomy until reviewed. | Recommend pause pending human decision. |

**S4 recommends. It does not halt.** Nothing in this schema stops execution. Only the organisation's own enforcement layer can do that, and only if it chooses to.

Severity describes the observation, not the person. An S4 is a statement about the state of a partnership, never a judgement about the individual in it.

---

## Part 3 — The disposition record

Written by the human or system that received the detection. Without this, the schema is a monitoring feed rather than a governance instrument.

| Field | Required | Description |
|---|---|---|
| `event_id` | yes | Links back to the detection. |
| `reviewed_by` | yes | The role that reviewed it. |
| `reviewed_at` | yes | When (ISO 8601). |
| `decision` | yes | One of: `actioned`, `acknowledged`, `deferred`, `dismissed`. |
| `rationale` | yes | Why. Required for every decision, including dismissal. |
| `action_taken` | if actioned | What was actually done. |
| `review_due` | if deferred | When it will be revisited. |

Requiring a rationale on dismissal is the load-bearing detail. Dismissal without stated reason is how a governance mechanism becomes decoration, and it is invisible unless the field is mandatory.

---

## Part 4 — Measures of whether the lever is live

The disposition record makes a small set of derived measures available. These describe the health of the oversight mechanism, not the health of the partnership.

| Measure | What it indicates |
|---|---|
| Undispositioned rate | Proportion of detections with no disposition record. Detections nobody looked at. |
| Time to disposition | Elapsed time from detection to decision, by severity. |
| Dismissal rate | Proportion dismissed. A rate approaching one means the lever is decorative. |
| Rationale quality | Proportion of dismissals with substantive rather than boilerplate reasoning. |
| Threshold stability | Whether severity thresholds have been revised, by whom, and in which direction. |

The last is the one most likely to be resisted and the most important. Whoever sets thresholds holds the real control. If thresholds are set by a function measured on throughput, they will drift toward never firing — and that drift is invisible unless it is recorded.

---

## Part 5 — Deployment contexts

**Enterprise.** Full schema. Detections route to a policy layer, dispositions are recorded, derived measures are reviewable by a role with standing outside the delivery line.

**Individual dyad.** No policy layer exists. Detections surface to the person directly, at S2 and above. Disposition is self-recorded or not recorded at all. This is the honest limit of the individual case: reporting is available, enforcement is not, and the schema should not imply otherwise.

**Clinical or other supervised settings.** Additional constraints apply that this schema does not address. It is not a clinical instrument and carries no clinical validation.

---

## Privacy

This schema describes monitoring of human behaviour. Three constraints follow.

`subject_ref` should be a partnership or role reference wherever a personal identifier is not operationally necessary. The unit of interest is the partnership, not the person.

The disposition record is about the reviewer's decision, not the subject's conduct. It exists to hold the oversight mechanism accountable, not the individual being observed.

Anyone whose partnership is subject to detection should know that it is, at what severity levels, and who receives it. Detection without disclosure is surveillance regardless of intent.

---

## Limitations

This is a proposed schema. It has not been implemented in a production system, and no organisation has yet run it.

The severity levels are defined qualitatively. There is no calibration data establishing which observations warrant which level, and no evidence that any two practitioners would assign the same severity to the same observation. Inter-rater consistency is untested.

The detection skills that would emit these events name their indicators but do not quantify them. They state what to look for, not how much of it constitutes a signal or over what period. Closing that gap requires instrumentation that does not currently exist in any deployed AI product — logging of contest and modification events, per-output review duration, and decision volume per period.

The derived measures in Part 4 are computable from the schema itself and do not depend on that instrumentation. They are the part of this specification that could be implemented immediately.

---

*Gaia Nexus · Relational Infrastructure Engineering · Sue Broughton · 2026*
