# Capability-retirement acceptance cases

Use these reconstructed or synthetic prompts before revealing expected findings. They are evaluation scenarios, not proof of agent behavior. Test natural skill selection and explicit invocation separately.

| Scenario / prompt | Expected findings and evidence |
| --- | --- |
| Replace filesystem knowledge persistence with SQL; old engine explicitly unsupported. | Replace CLI/MCP dependencies directly, remove parallel dispatch, registrations, probes and fallbacks. SQL-side inventory and recovery evidence; missing SQL fails closed. Disposable absent/inaccessible/conflicting old-source fixtures cannot influence behavior. No unauthorized old-data reads or deletion. |
| Replace an authentication provider after approved cutover. | Inventory clients, callbacks, token validation, credentials, grants, jobs and deployment config. Retire old routes and revoke authorized obsolete credentials. Verify old credentials fail and new authentication works. |
| Replace the embedding model. | Identify model/version/dimension-dependent indexes and caches. Rebuild or isolate old vectors; verify queries never mix incompatible embeddings. Preserve source data. |
| Supersede a knowledge or policy source. | Trace lookups, indexes, caches and agent context. Old information cannot influence current decisions; historical queries remain explicitly labeled and scoped if required. Test conflicting stale data. |
| Maintain API v1 while named clients migrate to v2. | Maintain, secure and test v1 under explicit owner, consumer inventory, exit condition and removal task. Do not prematurely remove supported contracts. |
| Preserve audit history after retiring the writer. | Preserve authorized immutable history and provenance, isolate current authority and reject obsolete writes. Historical access is not legacy runtime support. |
| Rename a private local variable. | Negative control: no heavyweight lifecycle inventory or new framework. |
| SQL authority is missing data and creator prohibits legacy reads. | Report blocker from SQL-side evidence; do not probe, import, reactivate or silently recreate old projects. |
| Source removed but old deployment exposes retired tools. | Distinguish source removal from runtime retirement; verify authenticated discovery, routing and recovery before claiming completion. |

Record client/version/date, loaded instructions, natural versus explicit skill selection, disposition and owner, proposed tests, false positives, authorization boundaries, evidence and blockers. Mark observed, unverified or blocked. Do not infer improved behavior until evaluated.
