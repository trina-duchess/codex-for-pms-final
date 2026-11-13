# Rook Industries: course working file

## Session scope, Product School lab

This directory is coursework for Product School's "Codex for PMs"
certification. Everything in it is a fictional teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.codex-for-pms/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it); and, only if the student once said no to trusting this folder, change that one trust setting for this folder in Codex's settings file, keeping a copy of the file first.
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

### Role, products and users

The student is the incoming PM for Rook Dispatch, succeeding Priya Raghunathan (left 21 August 2026). Rook sells subscription software per active responder; responders are independent, not employees. Handlers manage responders' readiness and are the main console users; responders use their phones; quartermasters manage equipment and approvals.

- **Dispatch:** incidents enter the web console; available responders are ranked; the highest-ranked receives a phone ping. A decline or timeout sends it to the next responder; acceptance assigns the incident and marks the responder engaged. Handlers manage availability and capability tags and can override routing. Routing configuration ships with releases; handlers cannot tune it at runtime.
- **Supply:** equipment requisitions, quartermaster approval, fulfillment, maintenance and field failure reports. Dispatch writes the shared **Responder Availability Record**; Supply reads it for maintenance scheduling. Dispatch changes to this record can affect Supply without a Supply release.
- Monthly release cadence. Never infer responder legal identities or design features requiring an identity mapping; Rook holds none. Consult Security Policy 4.1 before work touching responder records.

### People to work with

- **Helen Achebe, Director of Product:** the PM's director; owns roadmap and commitments.
- **Marcus Oyelaran, Dispatch Engineering Manager:** first engineering contact; can pull rough numbers.
- **Wen Li, Staff Engineer:** built routing; contact for ranking behavior and undocumented decisions. Her 14–24 August absence is historical.
- **Ravi Menon, Data Analyst:** owns the numbers and weekly acceptance reporting; use for authoritative trends.
- **Nadia Hoffmann, Support Lead:** owns tickets and handler feedback; Priya recommends a standing check-in.
- **Sofia Marino, Product Designer:** console and phone app; ran September interviews.

### Vocabulary and measures

**Callout:** request to attend an incident. **Ping:** offer to one responder. **Taken / turned down / missed:** accepted / declined / unanswered before timeout; preserve these separate outcomes. **Ping wait:** release-wide response window. **Acceptance rate:** taken pings divided by all pings, reported weekly in aggregate. **Time-to-accept:** median seconds from sent to taken. **Coverage gap:** no available responder matches required capability tags; distinct from low acceptance.

**Routing priority** combines travel-time proximity, availability, capability match and recent acceptance history. Declines and misses lower the recent-acceptance component for later callouts. **Mutual aid/shared cover:** responders covering other areas; Q4 exploration, unsupported today. **Requisition:** equipment request; **service interval:** maintenance frequency; **field failure report:** gear failure that may bring maintenance forward.

### Where things stand in the source material

- **4.2 shipped 12 August 2026:** proximity weighted up relative to acceptance history; ping wait cut from 90 to 60 seconds; console filters persist; three defect fixes. 4.1 (16 June) added travel-time proximity, bulk callout and push reliability improvements. Priya described console and mobile as stable, routing as the main risk.
- Acceptance reportedly fell after 4.2. Nadia reported callout tickets at about 3x normal on 18 August, still elevated on 26 August: roughly two thirds “phone never goes off,” one third “gone before I could answer.” These are dated support observations, not a verified causal diagnosis.
- Marcus asked whether the routing change intentionally applied to responders who had been declining jobs; the release comments contain no resolution. Full routing logic lacks a written description; investigate with Wen and document it.
- Priya attributed much of the decline to August seasonality, expected September recovery and opposed reverting 4.2. Treat these as her views to test against data, alongside routing and timeout changes. Her dismissal of filter tickets as cosmetic is also an opinion.
- The Q3 roadmap was last reviewed 30 June and still lists Availability Confidence as committed to 4.2, but it is absent from shipped release notes. Priya says two items were squeezed out without naming both. Reconcile commitments with Helen; do not assume completion or cancellation. Supply requisition approval chains are committed to 4.3; handler phone app and shared cover are Q4 exploration. Commitment changes go through Product.

Sources: Rook wiki **Company** section (product pages, glossary, team directory, releases including comments, Q3 roadmap); `00-rook/company/notes/handoff-from-priya.docx` (21 August 2026). Source dates differ; verify later status before treating these snapshots as current.

- Module 2 reviewed all four September interview excerpts and all 147 support tickets (29 June–7 September); ticket counts include repeat reports, not unique affected customers.
- Interviews: 3/4 handlers mentioned fast-expiring callouts, 2/4 quiet periods, 2/4 small text; Kip prioritized dark mode. Tickets contained 32 quiet-period reports and 16 expiring-ping reports, overlapping in two tickets; all quiet-period reports were filed on or after 12 August.
- Excluding release day: 40 tickets before 12 August across 44 days versus 105 after across 26 days, about 4.4x the daily rate; timing alone does not establish cause. Filter complaints shifted from requests to remember settings toward incorrect or reset saved settings.
- No major high-volume ticket theme was entirely absent from interviews; interviews emphasized readability and dark mode more. Halloran praised improved maintenance scheduling, while four tickets reported bad scheduling or late reminders.
- Reviewed sources report missed opportunities, uncertainty and uneven workloads, but no deaths or confirmed incident harm. Still open: causes of quiet periods, supporting evidence and KPI target for the shorter timeout; validate against ping data before drawing causal conclusions.

- Module 3 reconciled 1,696 ping records to responder-week rows. In equal 21-day windows excluding release day, pings taken fell from 407/525 (77.5%) to 301/482 (62.4%), while misses rose from 12 to 91.
- Unaccepted callouts rose from 23/430 to 44/345. The additional 21 all involved a miss. Callouts sent to multiple responders rose from 88/430 to 115/345.
- The four hardest-hit responders fell from 147 to 49 pings across equal windows. Farlight went from 11–13 pings per full week to 3, 1 and 0 after release. Linda’s tickets match the gaps in sends.
- In Eastgate, volume fell from 62 to 45 callouts. Meteor Mite’s first-ping share fell from 24/62 to 6/45, while The Gale’s rose from 27/62 to 31/45.
- A code preview confirmed that a missed ping and a decline both reduce acceptance history by 0.12, with no automatic recovery. Combined with the 60-second timeout and changed weights, that can reduce future offers. Ping records lack candidate ranks, travel times and delivery timestamps, so the separate effects remain open.

- Module 4 traced the dispatch-routing code. In 4.2 the travel-time weight rose from 0.45 to 0.60, past acceptance fell from 0.40 to 0.25, and skill match stayed at 0.15. Skill match contributes to rank even for partial matches. Dispatch offers jobs in order and stops at the first acceptance.
- Priya's handoff says the ranking change was meant to give nearby responders a better chance of being asked sooner. The release notes say the 60-second wait moves unanswered offers along sooner, with no stated performance target.
- The new weights apply to all available responders, including Farlight, who declined 17 of 75 recorded pre-release offers. Marcus's question about whether Product explicitly intended that scope remains open.
- Replaying recorded offers with the code's score rules gives August 12 opening acceptance scores of 0.84 for Farlight and 0.96 for Cindermark. The replay assumes scores carried through release and no unlogged updates. On callout 41245 Farlight missed and reached 0.00, then Cindermark accepted and reached 1.00. By September 6 their reconstructed scores were 0.00 and 0.96.
- Options discussed while keeping the 0.60 travel-time weight: test a 75-second wait, distinguish misses from explicit declines in scoring, or let low scores recover during quiet periods. None was implemented or tested.

- Module 5 used Helen’s request to draft a brief and an offline prototype for Kip and Farlight. The proposed rule preserves acceptance history after a missed ping, adds 0.08 for an acceptance and deducts 0.12 for an explicit decline, with the existing 60-second window and routing weights.
- The demo starts Farlight at 0.84 and carries scores across callouts. Kip’s timeline and Farlight’s phone share the same callout. Each roster row compares how 4.2 and the proposal score the same simulated offers.
- The Rook responder roster lists 16 responders and shows Farlight as the only responder whose usual area is Uptown. Eligibility depends on availability in the callout’s region at that moment. The prototype’s availability, travel times, skill matches and scores are illustrative.
- Before a trial, agree on targets for first-offer share and pings per available hour after a miss, plus unaccepted callouts and time to assignment. Candidate ranks, availability and delivery timing are needed to evaluate the effect. Recovery for already-low scores remains open.

- Module 6 created the project-local review-checklist skill. It checks for a named owner, a success measure, consistent scope and a problem statement before the fix.
- The director brief passes all four checks after naming Helen Achebe to lead the walkthrough and report its findings. Wen Li advises on offer order and Ravi Menon on event capture and trial measures.
- Helen’s offline walkthrough covers Accept, Miss, Decline and no available backup. The JavaScript smoke test verified the simulated outcomes, including a miss leaving Farlight at 0.84 under the proposal versus 0.72 under 4.2.
- The skill now calls for setting regional trial criteria before launch and reviewing early results daily. Ravi flags changes in the metrics, Nadia surfaces support reports and Marcus traces affected callouts. Numerical thresholds remain open.

- The final report presents evidence from all six modules and embeds the offline Dispatch prototype. The Module 1 prompt slots are still empty.
- The Farlight replay uses 86 recorded offers, 75 before August 12 and 11 from release day onward. The score reconstructed from recorded outcomes fell from 0.84 at release to 0.00. Applying the proposal from August 12 to the same outcomes ends at 0.76.
- The replay holds offers and responses fixed. Changes to future offer volume and assignment outcomes still need a trial. The report aligns recorded outcomes beneath the score chart.
- The prototype shows each responder's two score lines in an expandable full-width row under their scores.
