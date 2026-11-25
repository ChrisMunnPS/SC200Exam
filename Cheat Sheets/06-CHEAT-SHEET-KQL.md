# SC-200 cheat sheet 3/5 — KQL for detections

*One page. Facts snapshot 2026-11; verify current behaviour on Microsoft Learn before the exam.*

## Query shape

| Item | What to remember |
|---|---|
| Anatomy | `Table \| operator \| operator ...` — each operator consumes the previous result set, left to right. |
| Order matters | You can only reference a column **after** the operator that creates it: `summarize Failed = count() by User` then `extend High = Failed > 20`. |
| Time filter first | `where TimeGenerated > ago(24h)` prunes before later operators run. `ago()` accepts `5m`, `1h`, `7d`, `30d`; absolute times use `datetime(2026-11-01)`. |
| `search` | `search "term"` scans many tables and is expensive; prefer a named table. |
| `let` | Binds scalars, tables, or functions for reuse: `let since = 7d; let vip = _GetWatchlist('VIP') \| project SearchKey;` |

## Core tabular operators

| Operator | Effect |
|---|---|
| `where` | Filter rows with a Boolean expression. |
| `project` / `project-away` | Keep (and order) named columns, or remove named columns and keep the rest. |
| `extend` | Add calculated columns; original columns stay. |
| `summarize` | Group and aggregate: `summarize count() by Col`. Result has only the group and aggregate columns. |
| `sort by` / `order by` / `top N by` | Order rows (`desc`); `top N by` sorts and limits in one step. |
| `take N` / `limit` | Return N arbitrary rows — no ordering guarantee. |
| `distinct` | Unique combinations of the listed columns. |
| `join` | Match rows sideways on a key (`kind=` controls the join). |
| `union` | Stack rows from tables/queries with compatible columns. |
| `lookup` | Enrich with a dimension table, keeping the left side only. |
| `parse` | Extract with a literal pattern: `parse Raw with "User=" User " Action=" Action`. |
| `extract` | Extract with a regex and capture group: `extract(@"id=(\d+)", 1, Url)`. |
| `mv-expand` | One row per element of a dynamic array. |
| `bin(Col, 1h)` | Bucket a timestamp or number; use inside `summarize` for time series. |
| `render` | Visualise (`timechart`, `barchart`, `piechart`, `table`). Last operator in the query. |

## Aggregations

| Function | Returns |
|---|---|
| `count()` | Number of rows in the group. |
| `countif(pred)` | Rows matching the predicate — one pass for several counters. |
| `dcount(col)` | Approximate number of **distinct** values; add `hll` precision if needed. |
| `sum`, `avg`, `min`, `max` | Numeric aggregates; `percentile(col, 95)` for tail values. |
| `arg_max(TimeGenerated, *)` | The whole row with the latest timestamp per group; `arg_min` for earliest. |
| `make_set(col)` | Dynamic array of **distinct** values in the group. |
| `make_list(col)` | Dynamic array of **all** values, duplicates included. |

## Strings, types, and comparisons

| Item | What to remember |
|---|---|
| `has` / `has_cs` | Whole-term match, indexed and fast (`"mimikatz"` matches on token boundaries). |
| `contains` / `contains_cs` | Substring match; slower, matches inside other words. |
| `startswith`, `endswith` | Prefix and suffix tests. |
| `matches regex` | Regular expression match; most expensive of the set. |
| `in (…)`, `!in (…)` | Membership test; ideal for lists and subqueries. |
| `between (a .. b)` | Inclusive range test. |
| Case sensitivity | Unadorned operators are case-insensitive; the `_cs` suffix makes them case-sensitive. |
| `tostring()` | Convert a dynamic scalar to string before comparing or displaying. |
| `todynamic()` / `parse_json()` | Convert a JSON string into a dynamic object; read keys with dot notation (`AdditionalFields.OSVersion`), and arrays with `[0]` or `mv-expand`. |
| `isempty()`, `isnull()` | Test for empty string or null; KQL uses `==` for equality, never `=`. |

## Tables you will be asked about

| Table | Holds |
|---|---|
| `SigninLogs` | Entra ID sign-ins: user, app, IP, location, `ResultType`, conditional access, risk. |
| `AuditLogs` | Entra ID directory changes: `OperationName`, `InitiatedBy`, `TargetResources`. |
| `SecurityEvent` | Windows security events from servers and DCs: `EventID`, `Computer`, `Account`, `LogonType`. |
| `SecurityAlert` | Alert records raised by analytics rules and connected products. |
| `SecurityIncident` | Incident records: status, owner, severity, classification, related alert IDs. |
| `OfficeActivity` | Microsoft 365 audit: `OfficeWorkload`, `Operation`, `UserId` (Exchange, SharePoint, OneDrive, Teams). |
| `CommonSecurityLog` | CEF appliance events: `DeviceVendor`, `SourceIP`, `DestinationIP`, `Activity`. |
| `Syslog` | Linux and appliance syslog via AMA. |
| `DeviceProcessEvents` | Process creation: `FileName`, `ProcessCommandLine`, `InitiatingProcessFileName`, `AccountName`. |
| `DeviceNetworkEvents` | Connections: `RemoteIP`, `RemotePort`, `RemoteUrl`, `InitiatingProcessFileName`. |
| `DeviceFileEvents` | File create/modify/rename/delete, folder path, hash. |
| `DeviceEvents` | Many endpoint event types distinguished by `ActionType` (ASR blocks, PowerShell, etc.). |
| `ThreatIntelligenceIndicator` | IOCs: `NetworkIP`, `Url`, `FileHashValue`, `DomainName`. |
| `Watchlist` | Watchlist rows; also reachable with `_GetWatchlist('name')`. |

## Joins

| Item | What to remember |
|---|---|
| Default kind | `innerunique` — deduplicates the **left** key; use `kind=inner` when every left row must survive. |
| Kinds | `inner`, `innerunique`, `leftouter`, `rightouter`, `fullouter`, `leftanti`, `leftsemi`, `anti`, `semi`. |
| `leftanti` | Left rows with **no** match — perfect for "users with no MFA". |
| `leftsemi` | Left rows that **do** have a match, without duplicating columns. |
| Column clash | Prefix with `$left.` and `$right.`; `project` before the join to avoid ambiguity. |

## Performance rules

| Rule | Why |
|---|---|
| Filter before `extend`, `summarize`, `join` | Fewer rows carried through expensive operators. |
| `project` early | Fewer columns means less memory and less shuffle. |
| Prefer `has` over `contains` | Term matching uses the index. |
| Avoid `search *` | Full-workspace scans are the most expensive pattern. |
| Use `materialize()` | Cache a subquery reused several times in one query. |

## Patterns worth memorising

| Goal | Pattern |
|---|---|
| Top talkers | `summarize N = count() by UserPrincipalName`, then `top 10 by N desc` |
| Latest row per entity | `summarize arg_max(TimeGenerated, *) by DeviceName` |
| Threshold on a group | `summarize Files = count() by UserId, bin(TimeGenerated, 1h)`, then `where Files > 500` |
| Exclude a watchlist | `where UserPrincipalName !in` the watchlist's `SearchKey` values |
| IOC match | `join kind=inner` network events to `ThreatIntelligenceIndicator` on the IP column |

**Exam tip:** read a query inside out — filter, aggregate, sort, limit — and remember `where` on an aggregate must come **after** `summarize`.

*Source: Create queries for Microsoft Sentinel using Kusto Query Language (KQL).*
