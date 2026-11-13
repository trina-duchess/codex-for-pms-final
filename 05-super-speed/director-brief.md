# Keep Farlight in contention after a missed offer

**To Helen Achebe · Rook Dispatch · Working draft**

**Who it’s for.** Farlight went from 11–13 pings per full week before 4.2 to 3, 1 and then 0. Kip is the handler in this walkthrough and needs to see where each callout went.

**Decision requested.** Approve a walkthrough of two consecutive callouts before Engineering changes routing. When Farlight’s ping expires, Dispatch should still move to the next responder, while preserving Farlight’s acceptance score for the next callout. An explicit decline still deducts 0.12.

Helen Achebe will lead the walkthrough and report its findings.

### What changes for Farlight and Kip

| Moment | Kip at the console | Farlight on the phone |
| --- | --- | --- |
| Farlight’s first offer expires | Kip sees **Farlight · Offer expired**, followed by the next responder’s offer event once Dispatch sends it. | If Farlight opens the stale notification, the app shows **This offer has ended** and removes the **Accept** action. |
| A new suitable callout comes in | In this example, Farlight’s preserved score is enough to rank first. Kip sees the offer, then the assignment once Farlight accepts. | Farlight receives a fresh offer with incident details and time to respond, taps **Accept** and sees the assignment. |

**Scope.** This prototype uses the 60-second window and existing ranking weights. It applies prospectively, with stored scores carried forward. Notification delivery and recovery for already-low scores are outside this prototype. Future offers depend on availability and rank.

### Why test it

In the 21 days before and after 4.2, excluding release day, taken pings fell from 77.5% to 62.4% and misses rose from 12 to 91. The four hardest-hit responders received 147 pings before release and 49 after. The code deducts 0.12 for either a timeout or a decline, with no automatic recovery. That creates a way for unanswered offers to reduce someone’s chances of getting future work. The records lack delivery timestamps, candidate ranks and travel times, so we can’t isolate how much the shorter window, ranking changes or notification delivery contributed.

### Three questions before Engineering builds it

1. **What happens if Farlight taps Accept as the timer expires?** Use the server deadline and record one final outcome per ping. If expiry wins, a late tap shows the expired state and Dispatch continues down the ranked list.
2. **What should Kip see if Farlight’s offer expires and nobody else is available?** Show the expired event followed by **Unassigned**. Show the next responder’s offer only after Dispatch actually sends it.
3. **How would we know this helps Farlight without delaying assignments?** In the walkthrough, use the same two candidate lists under both scoring rules and pick a second callout where the preserved score changes the order. For a trial, ask Wen and Ravi which rank and delivery events can be captured. Compare first-offer share, misses, declines, time to assignment and unaccepted callouts.
