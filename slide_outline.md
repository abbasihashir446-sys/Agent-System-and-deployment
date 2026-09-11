# Slide Outline — Support Ticket Triage Agent
### For a quick 5-7 minute talk

**1. The problem**
- Right now a person has to read every ticket, figure out what it is, and decide what
  to do. That's slow, and people aren't always consistent about it.

**2. What we built**
- An agent that reads a ticket, sorts it into a category, and handles it — except
  refunds over $100 or anything suspicious, which get sent to a human first.

**3. How it works**
- Ticket comes in → gets checked → gets categorized → routed to either "just handle
  it" or "wait for a person to approve it" → gets logged.
- Important part: even if someone tries to sneak instructions into a ticket to trick
  the system into approving a big refund, it still gets stopped and sent to a human.

**4. Why we built it this way**
- We used plain Python instead of a fancier framework like LangGraph or CrewAI,
  because the logic here is simple — one path, one decision point. No need for the
  extra complexity yet.

**5. Does it actually work?**
- Tested on 8 tickets, including a blank one and one trying to trick the system.
- Got every category right, caught the trick attempt, and ran in under a millisecond
  each time.
- Found one real weak spot: a nonsense ticket that happened to include the word
  "refund" got misclassified with false confidence. We know the fix.

**6. What's not tested yet**
- The part where the agent actually writes a reply needs a live AI model and internet
  access, which we didn't have in this test environment. Everything else here is real
  — that one part just needs to be tried in a proper setup.

**7. What we'd do next**
- Connect the approval step to something real, like Slack.
- Test the reply-writing step for real once it's hooked up to a live model.
- Run a lot more test tickets before trusting this with real customers.
- Ask: green light to test this properly with a live model?
