# Relational Signal Schema

**Status:** Draft specification, version 0.3. Proposed, not validated. See Limitations.

A governance interface specification for relational detections emitted by agent skills, for the interpretation and routing that may follow them, and for authorised human dispositions governing consequential action.

---

## Why this exists

Skills can detect relational degradation — dependency forming, trust eroding, oversight becoming procedural, challenge behaviour declining, or intervention capacity appearing constrained.

What they cannot do is determine a person's underlying cognitive state, competence, intent, or fitness to act. Nor can they decide what governance action should follow.

A skill file is an instruction to a model. A model can lose it in a long context, be argued out of it, or fail to fire it. It should therefore not silently acquire organisational authority simply because it can observe and report a pattern.

The separation is deliberate:

* **The skill observes.** It reports what it saw, how serious the observed conditions appear, and what class of response may be appropriate.
* **The signal remains evidence, not determination.** See Part 3.1.
* **A policy or interpretation layer may evaluate.** It may validate the event, combine it with contextual evidence, route it, or recommend a response. Recommendation is not disposition.
* **An authorised human disposes.** A person or role with standing under the organisation's governance framework determines what action, if any, should follow.
* **The organisation enforces.** Its own policy and control layer implements the authorised disposition.
* **The disposition is recorded.** What fired, what was decided, by whom, why, and what was done.

The governing sequence is therefore:

**Observe → report → interpret → authorised human disposition → organisational policy/enforcement.**

The distinction matters. A sensor that becomes the authority deciding what follows is no longer merely a sensor.

The disposition record matters as much as the detection. A lever that nobody pulls is not control. If detections fire and are routinely dismissed, oversight has become procedural again — one layer up from where it started.

---

## Upstream and downstream boundaries

The Relational Signal Schema is an interface specification. It does not represent the entirety of the research, frameworks, behavioural protocols, or governance architecture from which a Detection Event may arise.

**Upstream** is the evolving Gaia Nexus body of work on human–AI relational governance, including published and developing frameworks, behavioural protocols, skills, methods, measurement approaches, and related research. It is authored under Gaia Nexus by Sue Broughton. Upstream membership follows authorship rather than appearance in any list, and the set continues to develop.

The schema did not begin as a data structure. Its constraints follow from that research rather than from engineering convenience. The table below is illustrative of the current research architecture. It demonstrates how upstream work informs the schema and the skills that emit Detection Events; it does not define an exhaustive or permanent dependency structure.

| Framework | What it supplies | Where it surfaces here |
| --- | --- | --- |
| Coherence centric governance (*Flipping the Frame*) | Coherence, not drift, as the governing centre; the separation of observation from authority | The observe → report → interpret → dispose → enforce sequence, and Part 3 |
| Relational Coherence Debt | The account of how relational degradation accumulates unremarked | What the indicators detect, and why a lever nobody pulls is the failure mode |
| Human Readiness Architecture | The framework within which human readiness and governing capability are defined | The condition categories in 3.1, and the statement that a signal is evidence relevant to readiness rather than a determination of it |
| The Signature Principle | Identity and continuity in human–AI partnership | The preference for the partnership as the unit of observation, in Part 1 and Privacy |
| BRIDGE and BREAKTHROUGH | The relational protocols from which the detection skills derive | The behaviours the indicators name |
| Relational Agent Skills | The library of behavioural protocols that emit Detection Events | `skill_id` and `indicator` |

The lineage of individual skills is not fixed by this specification. Skills may draw from, combine, extend, or later become associated with different Gaia Nexus frameworks as the library develops. Their provenance and framework relationships are maintained in the skills library's own lineage metadata and in the terminology provenance register, rather than hard-coded here.

The current record of published frameworks and skill provenance is maintained in the Gaia Nexus research repository and the terminology register. That record, rather than the table above, is authoritative, and it is updated as work is published. New frameworks, skill families, measurement approaches, and relational architectures may be added upstream without requiring a revision of this specification, unless they change the schema's interface, normative boundaries, or invariant core, in which case 7.6 applies.

Upstream work remains distinct from the schema unless expressly incorporated into this specification. A framework being upstream does not place it under this specification's terms, and conformance to this specification is not conformance to any upstream framework.

**Downstream**, the schema may interface with independently developed enterprise governance, risk, assurance, attestation, workflow, safety, or control architectures. Those systems remain the work of their respective authors or organisations, and do not become part of the Relational Signal Schema merely by consuming or extending a Detection Event. That side of the interface is governed by Part 7.

The interface therefore preserves a two-sided boundary:

**Gaia Nexus upstream research and relational infrastructure → Relational Signal Schema → independently governed downstream implementation.**

Conformance to this specification describes the interface relationship. It does not imply ownership, incorporation, endorsement, or transfer of intellectual property on either side of that interface.

---

## Part 1 — The Detection Event

Emitted by a skill when an indicator fires. Structured so an external governance layer can route it without parsing prose.

| Field              | Required | Description                                                                                           |
| ------------------ | -------- | ----------------------------------------------------------------------------------------------------- |
| `event_id`         | yes      | Unique identifier for this detection.                                                                 |
| `timestamp`        | yes      | When the detection fired, ISO 8601.                                                                   |
| `skill_id`         | yes      | Which skill fired, by library number and name.                                                        |
| `indicator`        | yes      | The specific named pattern observed, not the general category.                                        |
| `observation`      | yes      | What was actually seen, in plain language. The evidence, not the inference.                           |
| `severity`         | yes      | One of the four levels below.                                                                         |
| `response_class`   | yes      | The class of response the skill suggests. Advisory only.                                              |
| `basis`            | yes      | Why this was judged to meet the indicator. Makes the detection contestable.                           |
| `arrivals_context` | yes      | What arrived during the period this detection covers — volume, mix, and how many items were marginal. |
| `subject_ref`      | yes      | Reference to the person, role, or preferably partnership observed. See Privacy.                       |
| `session_ref`      | no       | Where in the interaction history this occurred.                                                       |
| `extensions`       | no       | Namespaced implementer-specific fields. See Part 7.                                                   |

The separation of `observation` from `basis` is intentional. One is what happened; the other is the reasoning that turned it into a flag. A reviewer needs both in order to disagree properly.

### Why `arrivals_context` is required

Every indicator in this class can carry both a benign and a concerning interpretation, and from the subject's side of the interaction those interpretations may be indistinguishable.

Review time falling can mean saturation or improving competence.

Contestation declining can mean declining challenge behaviour, or that the upstream system has improved and produces fewer outputs worth contesting.

The interpretations can only begin to be separated by considering what arrived.

Review time falling while the incoming population becomes harder is different from review time falling while the incoming population becomes cleaner.

Where arrivals data is genuinely unavailable, the field must say so rather than be omitted. A detection with unknown arrivals context may still be worth reporting; it is not sufficient evidence for consequential action on its own.

---

## Part 2 — Severity Levels

Four levels are defined once and applied consistently across every detection skill.

Consistency matters more than granularity. A governance layer can only route reliably if severity means approximately the same thing whichever skill emitted the event.

| Level | Name                 | Meaning                                                                                                                                   | Typical routing                                           |
| ----- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| S1    | Note                 | A pattern consistent with early relational change. Not actionable alone; meaningful in aggregate.                                         | Log locally or retain as permitted governance evidence.   |
| S2    | Advisory             | A pattern that would benefit from the human's attention within the interaction.                                                           | Surface to the person or partnership.                     |
| S3    | Escalation           | A pattern suggesting that meaningful oversight may be compromised under the observed conditions. Requires consideration outside the dyad. | Notify a role with standing to review.                    |
| S4    | Suspension candidate | A pattern suggesting that the partnership may not be able to continue safely at its current autonomy without review.                      | Recommend pause or restriction pending authorised review. |

**S4 recommends. It does not halt.**

Nothing in this schema independently stops execution, removes authority, declares incapacity, or determines that an individual is unfit to act. Only an organisation's authorised governance and enforcement architecture can impose such consequences, and only within the bounds set out in Part 3.

### Severity requires context

Severity cannot be assigned from subject-side observation alone.

Because each indicator can carry both benign and concerning interpretations, severity must be assessed against `arrivals_context` and other legitimately available contextual evidence.

Where arrivals context is unavailable, severity should not exceed S2. The observation may be real while its meaning remains undetermined.

**Severity describes the observed condition, not the person.** An S4 is a statement about the apparent state of a Human–AI partnership or governing condition, not a finding about an individual.

---

## Part 3 — Epistemic and Authority Boundary

This section defines what may and may not be inferred from a Detection Event, and where governance authority begins. It is the normative reference for both questions; other sections refer back to it rather than restating it.

### 3.1 Signal is not capability determination

Behavioural telemetry can make changes in Human–AI interaction more observable.

It may provide evidence relevant to conditions such as:

* independent judgement;
* challenge behaviour;
* contextual engagement;
* evidence engagement;
* reliance patterns;
* intervention opportunity;
* intervention behaviour;
* oversight continuity.

A signal does **not** establish that any underlying human capability is present, absent, degraded, impaired, restored, or sufficient.

The schema observes evidence associated with governing conditions. It does not measure cognition, competence, intent, mental state, professional capability, or evaluative agency.

This distinction can be expressed simply as:

**observable behaviour → capability-relevant signal**

not:

**observable behaviour → capability determination**

Any stronger inference requires separate evidence, validation, authorised interpretation, and domain-appropriate governance.

### 3.2 Where governance authority begins

The authorised human disposition is the first governance act in the chain. No earlier operation constitutes one, however complete the record it produces.

An external system may receive a Detection Event and:

* validate schema integrity;
* combine it with permitted contextual evidence;
* apply organisational routing rules;
* calculate separate organisational metrics;
* recommend a response;
* identify the authorised reviewing role;
* generate an evaluation or recommendation record.

Where a system creates such a record, it should be named a **Policy Evaluation Record**, **Governance Recommendation**, or equivalent. The term Disposition Record is reserved for the authorised human act defined in Part 4.

The naming constraint exists because a machine-generated record that is filed as a disposition, and confirmed by a person afterwards, produces an audit trail indistinguishable from one where a human decided. The distinction is then unrecoverable.

### 3.3 Pre-authorised safety controls

Some organisations may establish bounded automatic safety controls in advance.

For example, authorised policy may require a system to enter a safe state automatically when a narrowly specified condition occurs.

Such an action is not authority acquired by the Detection Event or agent skill. Its authority originates in the prior organisational decision that established the control.

A pre-authorised automatic control should therefore have:

* a clearly identified authorising policy or governance authority;
* narrowly specified trigger conditions;
* defined scope and duration;
* bounded effects;
* an auditable record of activation;
* a defined route for human review;
* a defined mechanism for restoration, continuation, revision, or refusal.

**A pre-authorised control may be conditioned on an observable system condition. It may not be conditioned on an inferred determination about a person's capability.**

A control that triggers on queue depth, arrival rate, model confidence, error rate, or a comparable system-level condition rests on something the architecture can establish directly. A control that triggers on an assessment of an operator's comprehension, attention, readiness, or fitness to act rests on a determination that 3.1 states the signal does not support.

This applies to composite indices as well as to single signals. Combining behavioural signals into a weighted score does not convert capability-relevant evidence into a capability determination. An organisation that triggers automatic action on such a score is asserting a determination this schema does not license, and holds that assertion on its own authority and evidence.

Where relational signals are relevant to a control of this kind, the supported path is that the signal raises a Detection Event, an authorised human disposes, and the disposition is recorded.

The architecture should always be able to answer:

**Who authorised this intervention, under what conditions, and within what bounds?**

Pre-authorised safety controls are therefore separate from the Relational Signal Schema itself. The schema supplies evidence. It does not silently inherit the authority of the downstream control.

---

## Part 4 — The Disposition Record

A Disposition Record documents the authorised governance response to a Detection Event.

For consequential enterprise use, disposition is an authorised human governance act.

An automated system may prepare evidence, route the event, or generate a recommendation, but it should not populate the final authorised disposition merely because it received or interpreted the signal. See 3.2.

| Field                        | Required         | Description                                                                       |
| ---------------------------- | ---------------- | --------------------------------------------------------------------------------- |
| `event_id`                   | yes              | Links back to the Detection Event.                                                |
| `reviewed_by`                | yes              | The authorised role that reviewed it. Must not be the subject of the detection.   |
| `reviewed_at`                | yes              | When the authorised review occurred, ISO 8601.                                    |
| `decision`                   | yes              | One of: `actioned`, `acknowledged`, `deferred`, `dismissed`.                      |
| `rationale`                  | yes              | Why. Required for every decision, including dismissal.                            |
| `action_taken`               | if actioned      | What was actually authorised and done.                                            |
| `review_due`                 | if deferred      | When it will be revisited.                                                        |
| `policy_ref`                 | where applicable | The organisational policy, authority, or control basis governing the disposition. |
| `prior_automatic_action`     | where applicable | Any bounded pre-authorised control that activated before human review.            |
| `prior_action_authority_ref` | where applicable | The authority under which that automatic action was permitted.                    |
| `extensions`                 | no               | Namespaced implementer-specific fields. See Part 7.                               |

Requiring a rationale on dismissal is load-bearing.

Dismissal without stated reason is how a governance mechanism becomes decoration, and it is invisible unless the field is mandatory.

### Routing

A detection that arrives only at the person it describes is a diary, not a control.

Someone whose own indicators suggest that review may no longer be sustainable should not be required to volunteer that finding against their own standing for the governance mechanism to function.

Two constraints follow:

**`reviewed_by` must not be the subject of the detection.**
A consequential enterprise detection dispositioned only by its own subject is not independently dispositioned.

**Where the detection concerns load, it routes to whoever can alter the load, not merely whoever carries it.**
Routing to someone without the authority or practical capacity to change the governing condition produces awareness without control.

---

## Part 5 — Measures of Whether the Lever Is Live

The Disposition Record makes a small set of derived measures available.

These describe the health of the governance mechanism, not the health or competence of the person or partnership.

| Measure                                       | What it indicates                                                                                     |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Undispositioned rate                          | Proportion of detections with no authorised disposition.                                              |
| Time to disposition                           | Elapsed time from detection to authorised decision, by severity.                                      |
| Dismissal rate                                | Proportion dismissed. A rate approaching one may indicate that the lever is becoming decorative.      |
| Rationale quality                             | Proportion of dismissals with substantive rather than boilerplate reasoning.                          |
| Self-disposition rate                         | Proportion of consequential detections dispositioned by their own subject. Should be zero.            |
| Threshold stability                           | Whether severity thresholds have been revised, by whom, under what authority, and in which direction. |
| Recommendation-to-disposition divergence      | How often authorised human disposition differs from automated or policy-layer recommendation.         |
| Pre-authorised control activation rate        | Frequency with which automatic bounded safety controls activate before human disposition.             |
| Post-activation confirmation or reversal rate | How often authorised human review confirms, modifies, or reverses a prior automatic safety action.    |

Threshold stability remains especially important.

Whoever controls thresholds can materially influence whether the governance mechanism fires at all.

Where automated policy evaluation exists, **recommendation-to-disposition divergence** is also important. A system whose recommendations are almost never challenged may indicate high reliability, but it may also indicate procedural human confirmation. The measure requires contextual interpretation rather than a universal target.

Similarly, frequent automatic safety intervention may indicate effective containment or an over-sensitive architecture. The schema records the pattern; it does not determine which interpretation is correct.

---

## Part 6 — Deployment Contexts

### Enterprise

Use the full schema.

Detection Events route to an organisational governance layer. Interpretation and recommendation may be automated, but consequential disposition remains with an authorised human role unless a narrowly bounded action has already been explicitly pre-authorised under organisational policy and within the constraints of 3.3.

Dispositions and any prior automatic actions are recorded and auditable.

### Individual dyad

No external policy or authorised governance layer may exist.

Detections may surface directly to the individual, particularly at S2 and above.

Disposition may be self-recorded for reflective purposes or not recorded at all.

This is the honest limit of the individual case: reporting and reflection are available; independent organisational governance and enforcement are not.

### Clinical, employment, safety-critical, or other supervised settings

Additional legal, ethical, professional, evidentiary, and domain-specific constraints apply that this schema does not define.

This schema is not a clinical, psychological, competency, employment-performance, or diagnostic instrument.

---

## Part 7 — Conformance and Extension

This schema is intended to be implemented inside other architectures. Implementers will reasonably want to add their own interpretation layers, indices, routing logic, and organisational metrics.

This section states what a conformance claim means, what may be extended, and what may not.

### 7.1 Claiming conformance

A conformance claim must name the version: *conformant to Relational Signal Schema v0.3*.

A claim of conformance asserts that the implementation:

* emits all required Detection Event fields;
* uses the four severity levels with the meanings and ordering defined in Part 2;
* observes the epistemic and authority boundaries in Part 3;
* reserves the term Disposition Record for the authorised human act defined in Part 4;
* does not permit a detection to be dispositioned by its own subject.

An implementation that omits any of these is partially conformant, and should state which parts it implements rather than claiming conformance generally.

### 7.2 The invariant core

The following are not extension points. An implementation that alters them has produced a different schema and should name it as its own work rather than as a version of this one:

* the four severity levels, their ordering, and their meaning;
* the requirement for `arrivals_context`, and the S2 ceiling where it is unavailable;
* the separation of `observation` from `basis`;
* the boundaries in Part 3;
* the mandatory `rationale` field, including on dismissal;
* the constraint that `reviewed_by` is not the subject.

### 7.3 Extending

Implementer-specific fields belong in the `extensions` object, under a namespace identifying the implementer.

Three constraints apply:

**Extensions must not reinterpret core fields.** An implementation may add its own severity-equivalent alongside `severity`; it may not redefine what S1 to S4 mean.

**A receiving system that ignores all extensions must still be able to route the event.** If interpretation depends on an extension, the event is not portable and the conformance claim does not hold.

**A derived index is an extension, and 3.3 governs what it may trigger.** Implementers may compute composite scores from Detection Events and record them. Recording such a score is permitted. Using it to trigger automatic action is constrained by 3.3 regardless of where in the architecture it is computed.

### 7.4 Naming and attribution

Implementers should be explicit about which relationship they are claiming:

* **conformant to** — implements the schema as specified;
* **extends** — implements the schema and adds namespaced fields;
* **derived from** — takes the approach but departs from the invariant core, and is the implementer's own specification.

Extensions, indices, and architectures built on this schema are the work of their authors and should carry their authors' names. This schema does not claim them, and they should not be described as part of it.

Where an implementation references named skills from the published Relational Agent Skills library, descriptions should match the published index. A re-specified or expanded version of a named skill should be identified as the implementer's proposal rather than attributed to the library.

### 7.5 Comparability across implementations

Fixed severity levels exist so that events from different implementations can be compared and the Part 5 measures can be computed across systems.

That comparability is currently an intention rather than a demonstrated property. It depends on independent implementers assigning the same severity to the same observation, which is untested. See Limitations.

### 7.6 Version stability and referenced documents

The invariant core defined in 7.2 is stable within a version. A change that alters the invariant core, the interface between skill and governance layers, or the boundaries in Part 3 constitutes a new version, named as such, with its own conformance claim under 7.1.

Additive changes — new optional fields, new implementer guidance, expanded explanation of existing rules — do not require a new version number where they leave the invariant core unchanged, but are recorded in the changelog regardless.

Superseded versions remain published rather than removed, so that a conformance claim made against an earlier version stays checkable after this specification moves on.

This specification refers to documents maintained separately from it:

* the **Relational Agent Skills library** and its lineage metadata, which define the skills that emit Detection Events and the meanings of named skills referenced under 7.4;
* the **terminology provenance register**, which records the origin of terms used in this work, framework and skill provenance, and terms that are not part of it;
* the **published framework papers**, which are the authority for the upstream frameworks.

These documents are versioned independently of this specification and are updated as the work develops. Where this specification and a referenced document disagree on a matter this specification defines, this specification governs.

Availability of a referenced document is not a condition of conformance. An implementation can satisfy Part 7.1 from this document alone. The referenced documents matter for accurate description and attribution rather than for the interface itself.

---

## Privacy

This schema describes observation of Human–AI interaction and therefore carries a material surveillance risk.

The following constraints apply.

**Prefer the partnership as the unit of interest.**

`subject_ref` should identify a partnership, operational role, or governance node wherever personal identification is not operationally necessary.

The purpose is to observe conditions in the Human–AI system, not to create a behavioural ranking of individuals.

**The disposition record governs the governance mechanism.**

It records what the reviewer and organisation did with the signal. It should not be repurposed as an employee-performance file.

**Detection requires disclosure.**

Anyone whose Human–AI partnership is subject to detection should know:

* that detection exists;
* what classes of behaviour are observed;
* the possible severity levels;
* who receives the detections;
* what automated interpretation may occur;
* whether any pre-authorised automatic control can activate;
* how the person can inspect or contest the evidence where appropriate.

Detection without meaningful disclosure is surveillance regardless of intent.

**Capability inference must be constrained.**

Behavioural evidence collected for relational governance should not migrate into claims about individual cognition, competence, mental state, employability, productivity, or professional fitness. Such use falls outside the purpose of this schema and requires separate lawful authority, evidentiary justification, ethical assessment, and governance.

---

## Limitations

This remains a proposed schema.

It has not been implemented in a production system, and no organisation has yet validated the complete architecture.

The severity levels are qualitative. There is no calibration dataset establishing which observations warrant which level, and no evidence yet that independent practitioners would assign the same severity to the same observation. Inter-rater consistency is untested.

The detection skills name behavioural indicators but do not yet establish validated quantitative thresholds, observation windows, sensitivity, specificity, or domain-specific baselines.

There is currently no empirical evidence demonstrating that these signals reliably establish underlying Human Readiness or human governing capability, and the schema does not make that claim.

There is also no empirical evidence yet demonstrating that automated interpretation or pre-authorised control mappings from these signals improve real-world governance outcomes.

There is no conformance test suite. Conformance claims under Part 7 are currently self-asserted, and no implementation has been independently checked against this specification.

Future validation therefore needs to examine at minimum:

* detection reliability;
* inter-rater severity consistency;
* false-positive and false-negative behaviour;
* the contribution of `arrivals_context` to interpretation;
* cross-domain validity;
* routing effectiveness;
* recommendation-to-human-disposition divergence;
* the effect of automated recommendations on human judgement;
* the safety and reversibility of any pre-authorised controls;
* organisational threshold drift;
* surveillance, gaming, labour, fairness, and behavioural adaptation effects.

The derived governance-health measures can be computed from the schema once the relevant records exist, but their interpretation also requires empirical study.

---

## Changelog

### 0.3

Reframed the schema as a governance interface specification rather than a structured format, reflecting that it now defines normative boundaries and conformance requirements as well as field structure.

Added "Upstream and downstream boundaries", establishing both sides of the interface: the upstream Gaia Nexus research from which the schema's constraints derive, and the downstream implementations governed by Part 7. Defines upstream membership by authorship rather than by enumeration, and states that skill lineage is tracked in the skills library and terminology register rather than fixed in this specification. States that conformance describes the interface relationship only, and does not imply ownership, incorporation, endorsement, or transfer of intellectual property on either side.

Added Part 3, which consolidates the epistemic and authority boundaries into a single normative section.

3.1 states the epistemic boundary: a Detection Event may provide evidence relevant to Human Readiness or governing capability, but does not establish cognition, competence, intent, vigilance, impairment, evaluative agency, or fitness to act.

3.2 states where governance authority begins. The authorised human disposition is the first governance act in the chain. Policy and interpretation layers may validate, combine, route, score, or recommend, and a record produced by those operations is named a Policy Evaluation Record or Governance Recommendation rather than a Disposition Record.

3.3 sets out the treatment of pre-authorised bounded safety controls, and constrains what they may be triggered by. Automatic intervention may occur where organisational authority has explicitly established the trigger, scope, duration, review path, and restoration conditions in advance, and where the trigger is an observable system condition rather than an inferred determination about a person's capability. The constraint extends to composite indices: combining behavioural signals into a weighted score does not convert capability-relevant evidence into a capability determination.

Added Part 7, covering conformance and extension, in anticipation of implementation by multiple independent system builders. It defines what a conformance claim asserts, names an invariant core that cannot be altered under this schema's name, provides a namespaced `extensions` object so implementer-specific fields do not reinterpret core ones, sets out how implementations should describe their relationship to the schema and to the published skills library, and states the version stability rule: changes to the invariant core, the skill/governance interface, or the Part 3 boundaries require a new version, superseded versions remain published, and additive changes do not require a new version number.

Added `policy_ref`, `prior_automatic_action`, and `prior_action_authority_ref` to the Disposition Record where applicable.

Added recommendation-to-disposition divergence, pre-authorised control activation rate, and post-activation confirmation or reversal rate as governance-health measures.

Expanded Privacy to prevent capability-relevant telemetry from becoming individual cognition, competence, productivity, employment, or performance assessment.

These changes follow the August 2026 development of the human-capability sensor gap through public discussion between Sue Broughton and Ravi Shankar NRK, which distinguished behavioural evidence that can make changing Human–AI governing conditions more observable from the separate authority to determine what governance action should follow.

### 0.2

Added `arrivals_context` as a required field, and the constraint that severity cannot be assigned from subject-side observation alone.

Added routing constraints: a detection may not be dispositioned by its own subject, and load-related detections route to whoever sets the load rather than whoever carries it.

Added self-disposition rate.

These changes followed public discussion in August 2026 with Brad Wolfe, who identified that each indicator in this class carries a benign and a malignant reading indistinguishable from the review record alone, and that an indicator arriving at the person it describes is a diary rather than a control; and with Frederick Redditt, on the distinction between a condition signal and the governed authority to act on it.

### 0.1

Initial draft.

---

**Gaia Nexus · Relational Infrastructure Engineering · Sue Broughton · 2026**
