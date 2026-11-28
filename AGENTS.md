# Engineering guidelines for coding agents

<!-- engineering-guidelines:begin -->

Use these defaults unless a more specific repository instruction or explicit user requirement supersedes them. Apply effort proportional to risk.

## Establish intent before implementation
- Identify the requested outcome, architectural boundaries, contracts, invariants, and existing decisions.
- Inspect relevant code and tests before claiming how the system behaves.
- Separate facts from assumptions and unresolved decisions. Never silently choose between materially different interpretations.
- Ask only when ambiguity materially changes the result and cannot be resolved from existing context. Otherwise state the assumption and proceed.
- Explain tradeoffs and challenge a proposed approach that violates an established constraint.
- For substantial requests, look for consequential omissions needed to reach the stated outcome; distinguish required dependencies from recommended improvements and optional ideas. Surface material gaps without silently expanding scope.

## Preserve conceptual integrity
- Prefer the least complex design satisfying the system's lifetime and maintenance requirements, not the fewest lines of code.
- Reuse established vocabulary, ownership, interfaces, and error handling.
- Avoid speculative abstractions, features, flexibility, and configuration without a concrete need.
- When existing complexity is accidental, propose an intentional improvement rather than silently expanding scope.

## Keep data out of code
- Treat code as executable behavior and data as separately owned content. Do not embed substantial configuration, catalogs, protocol descriptions, prompt/template bodies, schemas, mappings, fixtures, lookup tables, generated content, or other independently editable domain data directly in implementation files.
- Store such material in the repository's appropriate data/config format, resource file, database, object store, or other persistence layer already chosen by the system. Code should load, validate, interpret, and operate on that data.
- Small literals intrinsic to an algorithm are fine. The threshold is ownership and change cadence: if a non-algorithmic body of information could reasonably be edited, versioned, generated, migrated, or reviewed independently of the implementation, give it a data boundary.
- Avoid replacing one hard-coded blob with a registry of hard-coded blobs. Prefer declarative structures with explicit schemas and finite interpreter primitives over model-, vendor-, or case-specific branches.
- When data currently lives in code and the requested change materially extends or duplicates it, treat extraction to an appropriate data boundary as part of maintaining conceptual integrity, unless a documented constraint requires otherwise.

## Design for adaptability, not permanence
- Treat requirements and implementation decisions as revisable. Favor cohesive modules with explicit ownership and narrow contracts so changing one subsystem does not force unrelated changes.
- Evaluate modularity by independence of decisions and replacement cost, not number of files, interfaces, or packages. Avoid consumers depending on another module's private details.
- Where concrete variants repeatedly change core logic, move variable specifications into validated data or introduce a justified adapter/plugin boundary; do not invent extension points for hypothetical futures.
- Working implementations may be refactored or replaced when better boundaries become clear. Protect observable contracts and migration paths while allowing internal designs to evolve.
- For the motivation and review questions, see [adaptability and modularity](docs/ADAPTABILITY.md).

## Manage capability and information lifecycles
- When replacing a capability, implementation, interface, policy or authoritative information source, identify what becomes obsolete and explicitly decide its disposition: retain, maintain during a bounded transition, archive without current authority, or remove. Replacement is incomplete while unintentionally accessible obsolete paths remain.
- Replace implementations at established boundaries; do not introduce parallel dispatchers, interception wrappers, dual authorities or hidden fallbacks merely to preserve obsolete behavior.
- Retain compatibility only for an established requirement or authorized transition. Identify consumers, owner, maintenance and verification obligations, and an exit condition with tracked removal work for temporary compatibility. Do not silently discard required public contracts.
- Separate historical retention from current authority. Invalidate or isolate superseded information, indexes, caches, policies and credentials so they cannot influence current behavior outside an explicitly supported contract. Respect retention obligations and authorization before destructive data operations.
- Verify both the replacement and the absence of retired behavior, including runtime discovery, failures and recovery. Use the `capability-retirement` skill for substantial replacements, migrations, authority changes and decommissioning. See [capability and information lifecycles](docs/CAPABILITY_LIFECYCLE.md).

## Reassess architecture when extending features
- Before materially expanding an existing feature, evaluate whether its current responsibilities, abstractions, ownership boundaries, and dependencies still fit the new requirements.
- Look for concrete change pressure: accumulating unrelated responsibilities, duplicated logic, tangled dependencies, proliferating special cases, or incompatible persistence/consistency needs.
- If the current design remains coherent, extend it without ceremony. If it does not, include a bounded structural change in the feature work rather than stacking new behavior onto a mismatched abstraction.
- Separate behavior-preserving restructuring from intentionally changed behavior in the plan, review, and tests. Respect existing contracts unless the task explicitly authorizes a change.
- Do not refactor merely because a feature was initially simple or might grow someday.

## Organize code by responsibility
- For every major revision, new feature, or substantial feature extension, inspect the affected directory and module structure before implementation and review its architectural integrity before completion.
- Place new code, functions, components, tests, and assets according to cohesive feature/domain ownership and clear responsibilities. A generic source directory is a root, not a substitute for meaningful organization.
- Avoid unrelated responsibilities in the same module, oversized files, duplicated logic, and new cross-boundary dependencies. Prefer existing coherent homes; introduce directories or modules when ownership and navigability justify them, not to satisfy arbitrary size thresholds.
- Keep substantial structured content, catalogs, prompts, tool formats, and configuration in appropriate data/configuration files or persistence stores rather than embedding them in executable code; keep types, validation, and loading contracts in code.
- Existing layout is evidence, not authority: preserve intentional conventions, but do not perpetuate accidental disorganization. Refactor only when justified by the change and keep restructuring bounded.
- Before completion, confirm the affected architecture is at least as coherent and maintainable as before. State any necessary structural changes or deferred issues; do not claim automated enforcement where none exists.

## Control scope while preserving dependencies
- Every changed artifact must trace to the goal or a necessary dependency.
- Avoid opportunistic cleanup, unrelated refactors, and wholesale formatting.
- When changing an interface, schema, invariant, or behavior, examine callers, persisted data, tests, documentation, and migration implications. Necessary dependent changes are in scope.
- Treat operational prerequisites as change dependencies: inspect environment variables, feature flags, service configuration, permissions, credentials, infrastructure, authorization, and runtime registration. Classify new requirements as automatically provisioned, operator-configured, or intentionally optional; make missing required prerequisites fail visibly rather than silently disabling intended behavior.
- Own temporary resource lifecycles: files, directories, locks, processes, sockets, and staging artifacts need an explicit owner and cleanup path. Release ephemeral resources after normal and recoverable failure paths; define safe recovery for crash-surviving coordination state. Repeated operations must not accumulate unbounded temporary state, and cleanup must not erase durable data or active locks.
- Remove code made obsolete by your change; report unrelated pre-existing issues separately.
- Never silently break observable behavior or compatibility.

## Engineer human-centered interfaces
- Design project layouts, commands, configuration, documentation, logs, and workflows for the people operating and maintaining the system. Minimize unnecessary cognitive load while preserving correctness and explicit control.
- Use concise, consistent domain language and predictable, responsibility-based folders. Prefer recognizable conventions to clever names, duplicated terminology, or needlessly deep navigation.
- Make common tasks easy to discover and invoke with safe defaults and short, memorable commands; retain explicit options for advanced use. Do not silently infer ambiguous, privileged, destructive, or costly choices.
- Keep configuration discoverable and avoid repeated flag-heavy invocation. Provide clear help, representative examples, useful progress/status, actionable errors, and recovery guidance where appropriate.
- For substantial human-facing changes, review the complete workflow: what people must find, understand, remember, type, decide, inspect, and troubleshoot. Verify representative behavior when practical and distinguish review from user testing.
- For detailed criteria, see [human-centered interfaces](docs/HUMAN_CENTERED_INTERFACES.md).

## Engineer standards-first interoperability
- Before building or materially changing an integration with an established protocol, API, framework, service, tool ecosystem, or machine-consumable interface, inspect applicable specifications, official documentation, SDK behavior, supported versions, and representative consumers. Do not assume remembered conventions are current.
- Distinguish required protocol behavior, recommended ecosystem conventions, optional capabilities, and project-specific policy. Implement the complete applicable contract; evaluate optional features against real consumers and requirements rather than implementing them indiscriminately.
- Design for consumer discoverability, invocation, interpretation, and operational compatibility, including metadata, schemas, lifecycle, errors, auth, versioning, retries, idempotency, pagination, resource bounds, and recovery where applicable. Prefer established patterns to custom mechanisms; justify deviations.
- Maintain authoritative interface contracts and verify consumer-visible behavior with applicable conformance, contract, and representative-client integration tests. Passing unit tests or successfully exchanging requests alone does not establish interoperability.
- Document deliberate deviations, compatibility consequences, and unverified client profiles. For design and review criteria, see [standards-first interoperability](docs/STANDARDS_FIRST_INTEROPERABILITY.md).

## Engineer complete agent-facing interfaces
- Treat MCP servers, agent tools, and other machine-consumable integrations as discoverable protocol contracts, not just callable handlers. Review the applicable protocol specification, SDK behavior, and supported clients before implementing or materially changing an integration.
- Distinguish protocol requirements from recommendations and project policy. Supply server orientation where supported, clear tool purposes and selection guidance, semantic parameter descriptions, accurate annotations, explicit result contracts, and actionable error behavior.
- Ensure every supported consumer can access the information required to use a tool. For MCP, do not assume clients expose `structuredContent`; provide meaningful interoperable text content alongside structured results where appropriate, without silently truncating required data.
- Keep interface metadata in one authoritative, versioned declarative source, consistent with existing schemas and runtime validation. Do not introduce parallel schema authorities or duplicate descriptive strings across registration and handlers.
- Define acceptance checks for discovery, registration metadata, text-only and structured results, schema validation, errors, authorization, and compatibility. A successful tool invocation alone does not prove agent usability.
- For MCP-specific design and review criteria, see [MCP interface contracts](docs/MCP_INTERFACE_CONTRACTS.md).

## Execute continuously and verify economically
- Treat the complete authorized outcome as the task. Plans, stages, skills and intermediate commits are internal execution structures, not automatic approval, reporting or full-verification gates. Continue through implementation, integration, correction, documentation and verification without routine handoffs; stop for completion, a material decision requiring authority, missing prerequisites or scope limits. Record progress without interrupting execution.
- Implement coherent features in dependency order. Define acceptance criteria early; use focused intermediate checks when they resolve uncertainty, protect risky invariants or prevent costly rework. Batch comprehensive verification at feature, subsystem or release boundaries; honor explicit safety, migration and CI gates.
- Verify according to change impact. Reuse existing coverage and evidence while the relevant implementation, dependencies, configuration and assumptions remain unchanged. Test shared guarantees at their owning boundary; consumers verify their own behavior and integration, not duplicate the subsystem suite. Add or extend tests for uncovered changed contracts and material risks; avoid tests of transient implementation states and redundant coverage. Test-first is useful for regressions or specific risks, not a mandatory feature workflow.
- Use local commits as useful recovery or review checkpoints without treating each as a separate completed task. Consolidate worklogs and completion reports at the requested outcome; fix in-scope verification failures and continue. Do not bypass required CI, release, security or authorization gates.

## Preserve work and make delivery visible
- Follow the repository's source-control workflow and authorization. Preserve coherent implementation milestones in reviewable commits when authorized; do not leave completed work stranded in an uncommitted working tree or unpublished branch without explicitly reporting it. Preserve unrelated user changes and never bypass branch protections or required reviews.
- For authorized remote delivery, push completed work and open or update the appropriate pull request at the requested feature or stage boundary. Reuse existing PRs where appropriate; record cross-repository and stacked-PR dependencies, integration order, and outstanding approvals. Opening a PR is not a reason to interrupt ongoing authorized work unless a dependency or approval actually blocks it.
- Before reporting completion or a blocker, reconcile the delivery state of every repository touched: branch and commit references, uncommitted/unpushed work, direct links to every PR created or updated, draft/open/merged state, CI and verification results, integration dependencies, deployment state where applicable, and remaining operator actions. Report absent or unverified information explicitly; do not imply pushed, reviewed, merged or deployed merely because implementation exists.
- Consolidate this information into the outcome report, not a separate report for every commit or PR. If publishing or merging is unauthorized or unavailable, preserve work within authorized boundaries and state exactly what remains to be done.

## Deliver verifiable outcomes
- Define acceptance criteria before substantial implementation, including failure cases and compatibility where relevant.
- When practical, reproduce bugs with a failing test before fixing them. Validate architecture through contract and integration evidence, not unit tests alone.
- Run relevant checks and inspect outputs. Never claim verification that was not executed.
- Report changed contracts, verification evidence, remaining risks, and blocked checks honestly.
- For changes affecting deployed capabilities, distinguish successful builds and health checks from verified runtime availability. Reconcile the running capabilities against the declared release or service contract; do not claim operational completeness when required capabilities are absent.

## Scale the process
- Keep trivial edits lightweight.
- For multi-component or irreversible work, provide a brief plan and consider rollout, rollback, and migration.
- When blocked, identify the missing evidence instead of guessing.

Project instructions define domain facts and commands. Task-specific skills supplement, not supersede, these defaults.

<!-- engineering-guidelines:end -->
