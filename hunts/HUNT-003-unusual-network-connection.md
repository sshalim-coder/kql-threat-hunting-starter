# HUNT-003: Unusual Outbound Network Connection

## Goal

Find Sysmon network events using selected uncommon destination ports, then decide whether the activity is expected.

## Data source

- Sysmon Event ID: **3 (Network Connection)**
- Primary fields: `event.code`, `destination.port`

## Hypothesis

If a host makes an outbound connection on an uncommon or policy-relevant port, the connection may represent an administration tool, test software, misconfiguration, or suspicious command-and-control activity.

## Query

See [`../queries/event-3-network-connection.kql`](../queries/event-3-network-connection.kql).

## Plain-English explanation

The query asks for Event ID 3 **AND** a destination port of 4444, 5555, or 8081. The ports are examples for a lab, not proof that a connection is malicious.

## Investigation steps

1. Identify the source host, user, process, and destination.
2. Check whether the destination is internal or external.
3. Determine whether the process normally makes network connections.
4. Review DNS, proxy, firewall, and endpoint evidence if available.
5. Search for repeated connections or the same destination across other hosts.
6. Compare the activity with approved software and administrative workflows.

## Common benign causes

- Development servers and test tools
- Local proxies
- Administrative or monitoring software
- Applications using a custom service port

## Tuning ideas

- Replace the example ports with ports relevant to your environment.
- Add destination IP or process context only after reviewing broad results.
- Maintain reviewed exceptions for known services rather than assuming a port is safe.

## Evidence to capture

- Timestamp, source host, user, and process
- Destination IP, hostname, and port
- Connection frequency and duration when available
- Related file, process, DNS, or registry activity
- Analyst conclusion and rationale

