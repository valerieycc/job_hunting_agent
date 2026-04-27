Job Search System
An agentic pipeline for finding, triaging, and applying to AI PM roles — built as both a personal tool and a working demonstration of agentic system design.
Why
Most "AI job search" tools optimize for volume — auto-apply, mass-spray, generic resumes. That doesn't fit how senior roles get filled, and it doesn't represent how good AI products should be built.

This is the opposite: a small, opinionated, observable system that does a few things deliberately well, with a clear evaluation framework.

It's also a portfolio piece. The architecture itself is the credential — every component is something a senior AI PM should be able to design and ship.
System overview
Five layers, each with its own agent or component.

1. Master profile — master_profile.yaml. The keystone. A structured representation of career history, accomplishments tagged by target archetype, voice samples, and hard constraints. Every other component reads from it.

2. Discovery — three parallel sub-agents that surface candidate roles from different sources, because the three target archetypes live in different sourcing channels:

ATS sweep — Greenhouse / Lever / Ashby boards on a watchlist of AI-native companies
Aggregator scrape — YC Work at a Startup, Wellfound, VC firm Pallet boards
Network / social monitor — founder Twitter, AI newsletters, RSS for funding announcements

3. Triage — an LLM-driven scorer that rates each candidate role 0–100 against the master profile and three target archetypes, with explicit reasoning and red/green flags.

4. Tailoring — for high-fit roles, generates up to three resume variants (one per archetype) plus a draft cover letter in the user's voice and a one-page interview-prep sheet.

5. Delivery & tracking — daily packets land in a dated folder; an application tracker maintains state from discovery through interview / offer / reject.
Target archetypes
The system is designed around three role shapes, each with its own resume variant and positioning:

Agentic builder — AI / agentic PM at AI-native companies
Founding PM — 0-to-1 PM at YC and Series A startups
CX-AI specialist — domain PM for customer-experience-AI products

Each accomplishment in the master profile is tagged with which archetypes it serves AND how it should be framed for each. That's what enables genuinely different stories per variant — not keyword-shuffled clones of the same resume.
Repo structure
Current:

.

├── master_profile.yaml    # Career data, archetypes, tagged accomplishments

├── .gitignore

└── README.md

Planned as the system grows:

├── agents/                # Triage, tailoring, cover letter, interview prep

├── discovery/             # Crawlers and watchlists per sub-agent

├── jobs/                  # Discovered role records (JSON)

├── packets/               # Generated daily packets

└── eval/                  # Golden set + recall measurement
Build status
Master profile schema, populated from current resume
Triage agent — scoring rubric and prompt
Resume tailoring agent — multi-variant generation
Discovery sub-agent 1: ATS sweep
Discovery sub-agent 2: aggregator scrape
Discovery sub-agent 3: network / social monitor
Cover letter agent
Interview prep agent
Application tracker + eval framework
Stack
Profile data as YAML — agent-readable and human-editable
Python for crawlers and orchestration
LLM via API (Claude / OpenAI) for triage, tailoring, and generation
File system as data store; no database until necessary
Eval
Recall is the headline metric: every week, tag the roles actually applied to (from any source) and measure how many the discovery agent surfaced. The triage agent's 0–100 score is calibrated by checking how often "high fit" calls correlate with roles that progressed past the screen.
Status
Private. Personal project, in active development.

