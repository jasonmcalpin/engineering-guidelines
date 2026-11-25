# Capability and information lifecycles

Architectural evolution requires subtraction as well as addition. A replaced implementation that remains reachable is still a maintenance, security and correctness obligation. A superseded information source may remain structurally valid while no longer representing current truth.

| State | Obligation |
| --- | --- |
| Active | Supported, maintained, secured and tested. |
| Transitional | Explicitly bounded support while replacement proceeds. |
| Deprecated | Still supported under a documented compatibility contract. |
| Retired | No operational access or implicit fallback to the obsolete capability. |
| Archived | Retained under explicit historical/retention policy, isolated from current authority. |
| Removed | Obsolete implementation and dependencies eliminated; physical data deletion separately authorized. |

These are review terms, not a required runtime state machine. A retired implementation can be removed while historical data remains archived. An intentionally supported archive reader is itself an active capability with an owner.

## Replacement versus addition

Replace mechanisms at stable responsibility boundaries where domain contracts remain valid. A new database does not require a second CLI dispatcher; a new provider does not automatically require new tool names. Parallel mechanisms need actual requirements. A temporary bridge has named consumers, owner, maintenance and security obligations, tests, exit condition and tracked removal task. Authorized breaking changes need no compatibility shim, but affected consumers and deployment prerequisites must be accounted for.

## Information validity and authority

Distinguish current authority, derived data, historical evidence and obsolete copies. Review freshness, version, validity, ownership, invalidation and revalidation. Stale search indexes, embeddings, caches, policies or credentials can influence behavior even after their source is retired. Exclude them from current resolution, refresh or remove them; labels or prompts alone are insufficient. Historical records may remain intentionally accessible under a separate historical contract without becoming current truth.

Retention and deletion are separate decisions. Respect audit, privacy, legal, contractual and recovery requirements; do not destroy data or revoke unrelated access without authorization. Where legacy access is prohibited, verify from the replacement and report missing evidence rather than reopening old sources.

## Completion

Inventory consumers and operational dependencies, coordinate rollout and recovery, and verify replacement success, obsolete-path absence, stale-data isolation, fail-closed behavior and authorized historical access. Verify deployed capabilities, not just source. A retained compatibility obligation is not completed retirement. See [skill](../.agents/skills/capability-retirement/SKILL.md) and [acceptance cases](CAPABILITY_RETIREMENT_CASES.md).
