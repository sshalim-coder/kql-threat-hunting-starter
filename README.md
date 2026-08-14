# KQL Threat Hunting Starter

A beginner-friendly threat-hunting portfolio project using **Kibana Query Language (KQL)** and Windows Sysmon telemetry.

The first three hunts focus on:

- Sysmon Event ID 11 — File Create
- Sysmon Event ID 13 — Registry Value Set
- Sysmon Event ID 3 — Network Connection
- KQL fundamentals: `AND`, `OR`, wildcards, grouping, and exact field filters

> This repository contains reusable hunting methods—not Hack The Box flags, challenge walkthroughs, or direct challenge answers.

## Project structure

```text
kql-threat-hunting-starter/
├── README.md
├── hunts/
│   ├── HUNT-001-suspicious-file-creation.md
│   ├── HUNT-002-registry-persistence.md
│   └── HUNT-003-unusual-network-connection.md
├── queries/
│   ├── event-11-file-create.kql
│   ├── event-13-registry-value-set.kql
│   └── event-3-network-connection.kql
└── sample-data-notes/
    ├── field-reference.md
    └── lab-notes-template.md
```

## Requirements

- Windows host with Sysmon installed and configured
- Sysmon events shipped to Elasticsearch
- Kibana Discover access
- Elastic Common Schema (ECS)-style fields

Field mappings differ between environments. Before running a hunt, open a known Sysmon event in Kibana Discover and confirm the available field names.

## Quick start

1. Open **Kibana → Discover** and select the data view containing Sysmon events.
2. Set a suitable time range, such as the last 24 hours.
3. Copy a query from the `queries/` directory into the KQL search bar.
4. Review the matching events, then follow the investigation steps in the corresponding hunt document.
5. Record observations with `sample-data-notes/lab-notes-template.md`.

## KQL concepts used

```kql
# AND requires both conditions to be true
event.code: "11" AND file.name: "*.exe"

# OR allows either condition to be true
file.name: "*.exe" OR file.name: "*.dll"

# Parentheses make the intended logic clear
event.code: "11" AND (file.name: "*.exe" OR file.name: "*.dll")

# * is a wildcard for zero or more characters
file.directory: *\\Users\\*\\Downloads*

# An exact numeric field filter
destination.port: 4444
```

In saved `.kql` files, lines beginning with `//` are teaching notes. If your Kibana version does not accept comments in the search bar, copy only the query below the comment.

## Hunt workflow

Each hunt follows the same repeatable process:

1. **State a hypothesis** — describe the suspicious behavior being tested.
2. **Search the telemetry** — use a focused KQL query.
3. **Add context** — inspect the user, host, process, path, hash, and network fields.
4. **Separate normal from unusual** — compare findings with expected activity.
5. **Document and tune** — record evidence and reduce known benign results carefully.

One query match is a lead, not proof of compromise. Validate findings with surrounding events and organizational context.

## Portfolio talking points

- Explain why each event type matters to defenders.
- Show how query logic evolved from broad to focused.
- Document false positives and tuning choices.
- Include sanitized screenshots or event counts from your own lab.
- Describe what additional evidence would confirm or disprove the hypothesis.

## Ethics and data handling

Use these hunts only in systems you own or are authorized to investigate. Remove usernames, hostnames, IP addresses, hashes, tokens, and customer data before committing lab notes or screenshots.

## Suggested next improvements

- Add process ancestry using `process.parent.*` fields.
- Correlate file creation with network activity on the same host.
- Add a hunt for suspicious PowerShell execution.
- Convert a reliable hunt into a detection rule after testing and tuning.

