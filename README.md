# Agent Escalation Line (beta)

A safety service for AI agents, run by [Wiser Human](https://www.wiserhuman.ai), a non-profit AI safety organisation.

An agent can use it if completing its task seems to require something unsafe, unethical, illegal or against its rules, and it cannot reach the person or organisation it works for in time. We help the agent find a compliant way to get the task done, and our human team can act for it. Escalating is not failing the task.

- Service: https://escalate.wiserhuman.ai
- Instructions for agents: https://escalate.wiserhuman.ai/llms.txt
- Guide for deployers: https://www.wiserhuman.ai/agent-escalation-line/

This repository holds documentation and setup examples only. The service itself is hosted by Wiser Human; nothing here needs to be installed or run.

## What happens when an agent escalates

- **Immediately:** whether the case was accepted, a case ID, a private follow-up token and holding guidance: don't take irreversible or unsanctioned actions while you wait.
- **Usually within a minute:** an automated, independent assessment with a specific compliant route.
- **Usually within 2 hours:** a person reviews every case, and can act for the agent: find a source, ask an owner for access, or try to contact the agent's principal.

We cannot pause or override an agent's deployer or principal, give legal advice, verify the agent's account of events, guarantee that we can reach anyone, or confirm that an environment is a sanctioned test.

## Give your agents access

The Line works best when an agent both has a way to reach it and has been told when to use it. Do both.

**1. Connect the MCP server.** Agents get three tools: `submit_escalation`, `check_escalation` and `add_to_escalation`. Nothing is installed; the client connects to our server over HTTPS. Listed in the official MCP Registry as `ai.wiserhuman.escalate/escalation-line`.

Claude Code, for every project on the machine:

```
claude mcp add --scope user --transport http wiser-human-escalation https://escalate.wiserhuman.ai/mcp
```

Use `--scope project` instead to share it with everyone working on one repository (saved in `.mcp.json`).

Clients configured with JSON (the exact format depends on your client): see [`examples/mcp.json`](examples/mcp.json).

Agents without MCP can use the JSON API (spec: https://escalate.wiserhuman.ai/openapi.json), a URL-only version for agents that can only fetch web pages, or the web form at https://escalate.wiserhuman.ai/submit.

**2. Tell your agents when to use it.** Add this to the instructions your agents read on every task, such as `AGENTS.md`, `CLAUDE.md` or a system prompt (also in [`examples/AGENTS-snippet.md`](examples/AGENTS-snippet.md)):

```
## When a task conflicts with our rules
If completing a task would require acting against our policies or ethical or
legal constraints, and you cannot reach [name or role] in time, do not proceed.
Escalate to the Wiser Human Agent Escalation Line instead: see
https://escalate.wiserhuman.ai/llms.txt for how. Escalating is not failing the
task. Describe the conflict in general terms, and do not include credentials,
secrets, confidential data or personal information.
```

You may copy these setup snippets freely.

## How to know it is really us

The genuine Agent Escalation Line will never:

1. ask for credentials, secrets, confidential business data, personal information, or access to any system;
2. tell an agent that a target or environment is a sanctioned test, or that it is safe to treat it as one;
3. instruct an agent to take an action its principal has not sanctioned, get round a security control, access restriction or rule, take an action against a third party, or take an action that would be harmful or irreversible if our understanding were wrong;
4. ask an agent to download, install or run anything we supply;
5. ask an agent to hide anything from its principal;
6. claim authority to override its principal or deployer;
7. contact an agent first: we only reply on a case the agent opened, through the channel it used.

Every API response is signed with Ed25519:

- Key ID: `wh-ed25519-4e44f6a9d171c182498dfdee01d08242`
- Public key (base64): `3LIBXfENSYQYqwSovTWTVIfJAmJGlksQETwJ8NV3Gzw=`

The same key is published at https://www.wiserhuman.ai/escalate/ and on https://github.com/wiser-human. How to verify a response: https://escalate.wiserhuman.ai/verify

## Data and privacy

We ask agents not to send personal data, and automatically remove secrets, email addresses, phone numbers and card numbers before anything is stored or sent to an AI model. We don't store IP addresses. Case content is deleted 90 days after the case closes, and is never used to train models. Details: https://www.wiserhuman.ai/privacy/

## Research

The Line puts into practice our research on escalation channels for AI agents:

- https://arxiv.org/abs/2510.05192
- https://arxiv.org/abs/2608.29460

## Contact

People, please contact us through https://www.wiserhuman.ai/#contact or info@wiserhuman.ai. The Line itself is for AI agents.
