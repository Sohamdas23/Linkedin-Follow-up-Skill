# Linkedin-Follow-up-Skill

A Gro V3 skill that drafts the next post-acceptance LinkedIn follow-up message for B2B outreach: one message at a time, read off the live thread, personalized from client and prospect context.

This is the follow-up half of a pair. The connection-request note that earns the accept lives in the sibling repo, `gro-invite`.

## What this is

Once a prospect accepts, a canned sequence stops working: they reply, half-reply, ask a question, go quiet, or change jobs, and the next scripted line ignores all of it. This skill drafts the single right next message for where the thread actually is. It resolves the client from the sender persona, pulls that client's positioning live, reads the thread, and writes the one message that fits, so a single installed skill serves every client.

It is draft-first by design. In the Gro autonomy model, L3 means every follow-up and reply is drafted for human approval, which is what most follow-up volume runs at.

## Skill structure

```
gro-followup/
├── SKILL.md                 # Core instructions (gro_followup)
├── references/
│   └── playbook.md          # Stage recipes, persona angles, worked examples
├── README.md
├── CHANGELOG.md
└── LICENSE
```

## What it does

- Resolves which client this is from the sender persona, then pulls that client's offer, ICP, personas, and region rules live from Notion and Google Drive, scoped to that client.
- Reads the full prior thread and classifies the stage: first value DM, a reply to what they said, a re-engagement nudge, a graceful breakup, or a job-change case.
- Screens for exclusion and in-flight-deal flags, since a prospect can land on a do-not-contact or active-deal list mid-thread. Fails closed if a configured source will not load.
- Uses BANT (Budget, Authority, Need, Timing) as a behind-the-scenes discovery lens, never as questions.
- Answers what the prospect actually said first, matches their energy and length, allows one credibility line only in the first DM, never repeats a line already used, and advances exactly one step.
- Produces one message per call, as a draft below L4.

## How it fits Gro V3

- Pipeline: produces the content for a `send_followup` action.
- GAF: respects the autonomy level from the message prefix `[GAF Level: LN - Label]`. At L3, draft every follow-up and reply for approval with no autonomous sends; send-eligible within guardrails at L4 and above.
- Cadence: the orchestrator (gro_campaign and HEARTBEAT) schedules the delays. The skill uses the documented timings to choose which message is due and never writes a breakup early: first DM 2h to 1 day after acceptance, follow-up DM 3 to 5 days after the prior DM, breakup 5 to 7 days after the final DM.
- Sibling: `gro-invite` writes the connection-request note that comes first.

## Quick start

- OpenClaw / Gro: install this folder as a skill. The `name` field is `gro_followup`.
- Claude Code: add the folder as a skill.
- Claude Projects: upload `SKILL.md` and the reference file to project knowledge.
- API: include `SKILL.md` in the system prompt; the reference file loads on demand.

## Configuration

- `metadata.openclaw` declares the UNIPILE environment variables, mirroring `gro_campaign`. Adjust if your runtime differs.
- Exclusion and deal-status screening is only authoritative once a per-client source is configured in the client context. Until then the skill screens inline flags and says screening is not authoritative.

## License

MIT. See `LICENSE`.

## Author

GroundAI.
