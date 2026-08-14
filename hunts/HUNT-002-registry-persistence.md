# HUNT-002: Registry Run-Key Modification

## Goal

Identify values written to common Windows Run and RunOnce locations. These keys can launch programs when a user signs in.

## Data source

- Sysmon Event ID: **13 (Registry Value Set)**
- Primary fields: `event.code`, `registry.path`

## Hypothesis

If a process sets a value under a Run or RunOnce registry path, it may be establishing persistence—or it may be legitimate software configuration that should be verified.

## Query

See [`../queries/event-13-registry-value-set.kql`](../queries/event-13-registry-value-set.kql).

## Plain-English explanation

The query asks for Event ID 13 **AND** a `registry.path` containing either `CurrentVersion\\Run` or `CurrentVersion\\RunOnce`. Wildcards allow characters before and after those path fragments.

## Investigation steps

1. Review the complete registry path and value data.
2. Identify the process and user that changed the value.
3. Determine which program the value launches and where that program is stored.
4. Check whether the software is approved, signed, and expected on the host.
5. Search for the referenced file's creation and later execution.
6. Compare with similar activity on peer systems.

## Common benign causes

- Collaboration or cloud-storage clients
- Security software
- Hardware utilities
- Approved applications configured to start at sign-in

## Tuning ideas

- Maintain a reviewed list of expected value names and signed applications.
- Prioritize values that launch from user-writable directories.
- Avoid excluding an entire Run key; doing so would hide the behavior being hunted.

## Evidence to capture

- Timestamp, host, user, and complete registry path
- Registry value data
- Modifying process and parent process
- Referenced file path, hash, and signature
- Analyst conclusion and rationale

