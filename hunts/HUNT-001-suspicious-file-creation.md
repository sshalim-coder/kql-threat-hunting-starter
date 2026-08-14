# HUNT-001: Suspicious File Creation

## Goal

Find executable or library files created in user-writable locations. Attackers sometimes place payloads in Downloads, Temp, or AppData because standard users can usually write there.

## Data source

- Sysmon Event ID: **11 (File Create)**
- Primary fields: `event.code`, `file.name`, `file.directory`

## Hypothesis

If an executable or DLL is created in a user-writable directory, the event may represent an initial payload, a dropped component, or normal software activity that deserves validation.

## Query

See [`../queries/event-11-file-create.kql`](../queries/event-11-file-create.kql).

## Plain-English explanation

The query asks for Event ID 11 **AND** a filename ending in `.exe` **or** `.dll` **AND** a directory containing Downloads, Temp, or AppData. Parentheses keep the `OR` choices together.

## Investigation steps

1. Identify the host and user associated with the event.
2. Review `file.name` and `file.directory`. Does the location make sense?
3. Inspect the process that created the file, if process fields are available.
4. Check the file hash or signature with an approved internal reputation source.
5. Search nearby events for execution, registry changes, or outbound connections.
6. Record why the event is benign, suspicious, or unresolved.

## Common benign causes

- Browser downloads
- Software installers and auto-updaters
- Temporary files created during application installation
- Developer build output

## Tuning ideas

- Exclude a specific trusted updater only after validating its path and signer.
- Focus on uncommon filenames or hosts.
- Add process or signature context instead of excluding an entire directory.

## Evidence to capture

- Timestamp, host, and user
- Full file path and hash
- Creating process and parent process
- Relevant events immediately before and after the creation
- Analyst conclusion and rationale

