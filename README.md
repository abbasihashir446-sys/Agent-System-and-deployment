# Support Ticket Triage Agent

This is a small agent that reads incoming support tickets, figures out what kind of
issue they are (billing, shipping, account, etc.), and decides what to do next. If a
refund is $100 or more, or if a ticket looks like someone's trying to trick the system,
it stops and waits for a human to approve it before anything happens.

## What's in here

- **capstone_agent_system.ipynb** — the main notebook. Walks through the design, the
  code, the tests, and the results. Every cell that could run without an internet
  connection actually ran — the outputs you see are real, not made up.
- **executive_report.pdf** — a 2-page summary for anyone who doesn't want to read the
  whole notebook.
- **slide_outline.md** — rough notes for a short presentation.
- **core.py** — the actual agent logic (the tools, the classifier, the approval step).
- **api.py** — wraps the agent in a small web API (FastAPI) so it can be called from
  outside.
- **eval_harness.py** — runs the agent against 8 test tickets and scores it.
- **tickets.json** — sample tickets used for testing.

## How to run it

```bash
pip install pandas matplotlib fastapi uvicorn anthropic

python3 eval_harness.py          # run the tests
uvicorn api:app --reload         # run the API (needs ANTHROPIC_API_KEY for replies)
```

## One honest caveat

Everything here actually runs and was tested for real — except the part where the
agent writes a reply using an AI model. This sandbox has no internet access, so that
one call can't go through here. The code tries it anyway and just tells you it failed,
instead of pretending it worked. Once you run it somewhere with internet and an API
key, that part should just work.

## One thing worth fixing later

The way tickets get categorized is just keyword matching — no confidence score. So a
ticket full of gibberish that happens to contain the word "refund" gets treated the
same as a real refund request. Found this during testing, not guessing at it. Fix is
easy: add a confidence check and send anything unclear to a human instead of guessing.
