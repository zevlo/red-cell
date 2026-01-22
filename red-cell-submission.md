# Red Cell - AWS 10,000 AIdeas Competition Submission

**Team Name:** Red Cell Group
**Category:** Workplace Efficiency
**AWS AI Services:** Amazon Bedrock, Amazon Bedrock AgentCore, Kiro, Strands
**AWS Free Tier Services:** Amazon S3, AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito

---

## Big Idea

Red Cell, named for the Navy's adversarial analysis teams, is an AI premortem that finds the hidden assumptions in your plan, stress-tests them until they break, and hands you a kill-criteria playbook before reality finds the holes first.

---

## Vision - What We're Building

Red Cell is a strategy stress test tool. Users upload a strategy document such as a pitch deck, launch plan, or roadmap. Red Cell analyzes it and returns four deliverables.

An agentic workflow reads the document in passes and references the source repeatedly as it works.

**Assumption Map** - Surfaces explicit and implicit assumptions and ties each one to the exact sentence or slide that implies it.

**Fragility Score** - Ranks assumptions by uncertainty today multiplied by blast radius if wrong.

**Premortem Scenarios** - Shows realistic ways the plan fails when the top assumptions break, including cascading effects.

**Trigger Playbook** - Measurable kill criteria, validation experiments, and contingency actions such as pivoting channels if CAC stays above a threshold for two sprints.

---

## How It Makes a Difference

Founders pressure test pitch decks before investors do. Product managers validate launch assumptions before committing engineering resources. Project leads surface implicit bets that risk registers miss.

Red Cell solves a simple but expensive problem. Teams act on unspoken assumptions until reality proves them wrong. A startup can burn months building enterprise software without single sign on because nobody challenged whether customers would accept email login. Red Cell makes those invisible assumptions visible while change is still cheap.

The opportunity is shifting failure from production to rehearsal. Teams validate the riskiest assumptions first, turn vague plans into an experiment backlog, and make faster go or no go decisions.

My background includes the Navy Office of Warfighting Advantage where assumption based planning was core methodology for high stakes decisions. Red Cell brings military planning rigor to any team making consequential choices.

---

## Game Plan

### Phase 1: Core Workflow

Build the core workflow in a serverless app. Users upload documents to S3 and sign in with Cognito. A Bedrock AgentCore agent orchestrates the multi-step pipeline:

1. Extract assumptions with citations
2. Build an assumption-to-claim dependency graph stored in DynamoDB
3. Score fragility
4. Generate premortem scenarios and trigger playbook

The agent re-reads the source text at each step to keep outputs grounded. Lambda runs orchestration and report generation behind API Gateway.

### Phase 2: Quality and Trust

Refine prompts and add checks that enforce specificity, require measurable triggers, and reduce generic warnings. Validate on a small set of real plans across a few domains and track whether users mark top assumptions as new and useful.

### Phase 3: Expanded Capabilities

Add optional retrieval over public planning templates and checklists, plus export to PDF and CSV. Strands is a candidate Python SDK for implementing agent tools and workflow logic while using Bedrock models and AgentCore for orchestration.

Free Tier fit comes from document size limits, async jobs for longer runs, and token budgets.

---

## Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────────────┐
│   User      │────▶│  Cognito    │────▶│    API Gateway      │
│  (Upload)   │     │  (Auth)     │     │                     │
└─────────────┘     └─────────────┘     └──────────┬──────────┘
                                                   │
                    ┌──────────────────────────────▼──────────────────────────────┐
                    │                        Lambda                                │
                    │              (Orchestration + Report Gen)                    │
                    └──────────────────────────────┬──────────────────────────────┘
                                                   │
        ┌──────────────────┬───────────────────────┼───────────────────┐
        ▼                  ▼                       ▼                   ▼
┌───────────────┐  ┌───────────────┐  ┌─────────────────────┐  ┌─────────────┐
│      S3       │  │   DynamoDB    │  │  Bedrock AgentCore  │  │   Strands   │
│  (Documents)  │  │ (Assumption   │  │  (Multi-pass Agent) │  │ (Agent SDK) │
│               │  │   Graph)      │  │                     │  │             │
└───────────────┘  └───────────────┘  └─────────────────────┘  └─────────────┘
```

---

## Summary

| Field | Value |
|-------|-------|
| Team Name | Red Cell Group |
| Category | Workplace Efficiency |
| Primary AI | Amazon Bedrock + AgentCore |
| Agent Framework | Strands SDK |
| Development | Kiro IDE |
| Differentiator | Military assumption-based planning methodology |
| Core Output | Assumption map, fragility scores, premortems, trigger playbook |
| Target Users | Founders, PMs, project leads, strategy teams |
