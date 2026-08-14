# Field Reference

This project assumes Sysmon data normalized to Elastic Common Schema-style fields. Your integration may use different names.

| Concept | Expected field | Example | Notes |
|---|---|---|---|
| Sysmon event ID | `event.code` | `"11"` | May be stored as text or a number. |
| File name | `file.name` | `invoice.exe` | Usually the final component of the path. |
| File directory | `file.directory` | `C:\\Users\\analyst\\Downloads` | Directory without the filename. |
| Registry location | `registry.path` | `HKCU\\...\\CurrentVersion\\Run\\Updater` | Often includes the value name. |
| Destination port | `destination.port` | `4444` | Usually numeric; do not add quotes unless your mapping stores text. |

## Validate mappings

1. Open a known Sysmon event in Kibana Discover.
2. Expand the event and locate its event ID, file, registry, or network fields.
3. Compare those names and data types with the table above.
4. Update a copy of the query if your environment uses different mappings.

## Common mapping differences

- Event IDs may appear under `winlog.event_id` instead of `event.code`.
- A complete file path may appear under `file.path` rather than separate name and directory fields.
- Raw Sysmon fields may appear under an integration-specific namespace.
- Backslash and wildcard behavior can vary with field type and analyzer settings.

Treat a zero-result search as a reason to check time range, data availability, and mappings—not immediate proof that the activity did not occur.

