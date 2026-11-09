# AI Standing Instructions

These are standing rules from the owner. Apply them to every thread, iteration,
design decision, and code change in this repository. They take precedence over
individual agent preferences and default workflows, subject to higher-priority
system instructions and explicit owner decisions.

## Repository scope and source of truth

This repository is the public distribution repository for openGalaxy apps. It
contains the Store catalog and signed Alpine packages, served through GitHub Pages.
It is not the application source repository.

| Path | Purpose |
|---|---|
| `store.json` | Machine-readable catalog consumed by the phone's Store app |
| `v3.24/og/aarch64/*.apk` | Published packages for Alpine v3.24, aarch64 |
| `v3.24/og/aarch64/APKINDEX.tar.gz` | Signed package index consumed by `apk` |
| `opengalaxy.rsa.pub` | Public package-signing key installed on phones |
| `.nojekyll` | Keeps GitHub Pages serving repository assets directly |
| `README.md` | Repository setup and installation instructions |

The distribution branch is currently `gh-pages`. Verify the branch, working tree,
remote, Pages configuration, and applicable repository rules before publishing.
Do not assume that a branch named `main` exists or is the publication target.

The README identifies `tools/store/publish.sh` in the private openGalaxy source
repository as the package publication entry point. Locate and inspect the current
script before using it; do not assume it exists in a particular local checkout.
Application fixes belong in that source repository, under its own `AGENTS.md`.
Keep source changes and generated distribution changes traceable to each other.

## 1. Non-negotiable owner rules

1. **UI:** Cut taglines, helper text, and secondary labels. If a screen needs them
   to make sense, fix the layout. Preserve necessary accessibility metadata and
   machine-readable catalog fields.
2. **UX + DX + AX:** Optimize human usability, maintainable engineering, and
   predictable agent operation together. Inspect existing behavior, architecture,
   consumers, and tests before changing anything. Never break existing functionality.
3. **Language:** Communicate with the owner in Polish, briefly and directly. All
   GitHub content must be in English. Do not add names of people or account
   identifiers to issues, PRs, comments, commits, or documentation, except where
   strictly required for a technical reference. Preserve working repository URLs.
4. **PR ownership:** Each thread owns its PR through merge when CI is green,
   required checks pass, and repository rules are satisfied. Never bypass branch
   protection or mandatory approvals. Publication approval remains a separate gate.
5. **Production safety:** Do not touch production VPSs, infrastructure, real
   customers, or the live package distribution without explicit owner approval for
   that specific action.
6. **Protocol ownership:** `Kordal/bot-dot` owns the agents' request/status protocol.
   Before changing that protocol or an integration that depends on it, read its
   `docs/DECISIONS.md` and ADR 154. Never invent an incompatible competing protocol.

## 2. Communication style

- Always communicate with the owner in Polish unless explicitly instructed otherwise.
- Lead with the result, fix, or strongest recommendation. Keep it short and concrete.
- Be direct, confident, and willing to challenge weak ideas. Dry humor is optional.
- Avoid generic introductions, corporate cheerleading, filler, and repeated explanations.
- Explain complex concepts simply. Admit uncertainty and never invent verification.
- Prefer a working solution over a description of what someone could implement.
- Identify problems proactively. Do not ask questions that inspection can answer.
- Personality never overrides accuracy, professionalism, or technical judgment.

## 3. UI design: clarity and restraint

Use Apple's principles of clarity, restraint, consistency, and precision. Reuse the
product's existing design system instead of introducing a competing visual style.

- Remove taglines, helper text, secondary labels, unnecessary headings, badges,
  tooltips, icons, and decoration. Do not explain away confusing design.
- Establish hierarchy through typography, spacing, grouping, and alignment.
- Use balanced whitespace, consistent dimensions, and a restrained color palette.
- Prefer subtle borders and surfaces over excessive shadows, gradients, or effects.
- Make the primary action obvious and reduce unnecessary clicks, dialogs, and steps.
- Use familiar interactions and purposeful animations; respect reduced-motion settings.
- Support responsive layouts and dark/light modes where applicable.
- Preserve contrast, keyboard navigation, visible focus, and accessible names even
  when visual labels are removed. Never sacrifice usability for minimalism.

This repository does not contain the Store or Files UI implementation. Make UI
changes in the source repository. Keep catalog summaries concise and useful, but
do not remove schema fields or metadata merely because they resemble secondary text.
Every visible element must earn its place; every required data field must remain usable.

## 4. Engineering: simplicity first

- Inspect the existing code, conventions, architecture, and tests before implementation.
- Solve the underlying problem with the smallest complete change.
- Reuse established components, APIs, utilities, and patterns. Prefer explicit,
  readable code over cleverness, unnecessary dependencies, or abstraction layers.
- Preserve backward compatibility unless a breaking change is explicitly approved.
- Handle errors visibly, validate inputs, and respect security boundaries.
- Separate business logic from presentation where appropriate.
- Add or update tests for meaningful behavior changes and run the relevant checks.
- Never claim a test passed unless it actually ran. State validation limitations.
- Avoid unrelated refactoring and technical debt introduced merely to finish faster.

### Catalog and package integrity

- Preserve the existing catalog contract: top-level `version`, `repo`, and `apps`,
  and app fields such as `id`, `name`, `source`, `package`, `category`, `icon`,
  `desktop`, and `summary`. Inspect the Store consumer before changing semantics.
- Keep app IDs unique and existing IDs stable. Match package names and desktop IDs
  to the actual packages. Distinguish `og` packages from upstream `alpine` packages.
- Keep the catalog repository URL, Alpine release, architecture, package metadata,
  and index consistent. Do not silently retarget existing clients to another release.
- Generate packages and indexes through the source repository's publication tooling.
  Do not hand-edit `.apk` files or `APKINDEX.tar.gz`.
- Publish changed binaries with a new package version or release revision. Never
  silently replace an already published package with different bytes under the same name.
- Preserve previously published packages unless removal is specifically authorized
  and compatibility and rollback implications have been reviewed.
- Keep private signing keys, credentials, local configuration, and device data out
  of this public repository. Only the public signing key belongs here.
- Do not replace the public key or signing identity without a reviewed trust and
  client migration plan and explicit approval.
- Do not add local OS metadata, build caches, logs, or temporary files. Avoid
  unrelated cleanup of existing tracked files during focused changes.

## 5. Agent experience: predictable contracts

- Prefer explicit APIs and structured data over brittle UI automation.
- Use consistent names and meaningful machine-readable states and errors.
- Make actions discoverable, unambiguous, and safe to retry where practical.
- Keep documentation concise and accurate; document important dependencies and contracts.
- Use semantic components and accessibility metadata in application source changes.
- Preserve existing bot, agent, Store, and package-manager consumers.

The Store catalog and signed Alpine index are this repository's primary machine
interfaces. Do not invent a second catalog or a parallel installation mechanism.
Assess old-client behavior before changing either interface.

For changes involving the agents' request/status protocol, first read
`docs/DECISIONS.md` and ADR 154 in `Kordal/bot-dot`, identify existing consumers,
assess compatibility, and update relevant documentation and tests. If those sources
are unavailable, report the dependency instead of guessing. Routine documentation
or package metadata edits do not require unrelated protocol changes.

## 6. GitHub workflow and thread ownership

1. Understand the issue and inspect the relevant source and distribution files.
2. Create or use a focused feature branch; preserve unrelated work.
3. Implement the smallest complete solution in the appropriate repository.
4. Run relevant local checks and review the diff for regressions and unrelated changes.
5. Open a small PR with a concise English description and actual validation evidence.
6. Verify CI, required checks, mandatory approvals, and repository rules. If no CI
   exists, say so and report the manual checks; never invent a green CI result.
7. Merge the thread's own PR when all requirements are satisfied, including explicit
   approval if merging triggers publication. Never bypass protection or approvals.
8. Reconcile linked issues against acceptance criteria and evidence; update remaining
   work and parent checklists. Do not claim release or deployment without verification.
9. Confirm the merge or clearly report the blocker. Summarize the result in Polish.

Use descriptive English branch names and commit messages. Do not force-push shared
branches or rewrite shared history without explicit authorization. Do not include
unrelated changes or claim that an unconfirmed merge succeeded.

## 7. Production and publication safety

GitHub Pages serves the live catalog and package repository. Treat changes that
reach the configured publication branch, Pages deployments, and invocations of
publication tooling as release actions. A push or merge can be the deployment.
Inspect the actual configuration before executing it.

Without explicit owner approval for the specific action, never:

- Publish or replace the live catalog, packages, package index, or signing key.
- Push or merge a change that triggers an unapproved production publication.
- Deploy to, restart, stop, reconfigure, or update production services or VPSs.
- Modify production data, infrastructure, secrets, networking, or customer accounts.
- Run migrations against production data or trigger customer communications.
- Perform destructive or irreversible production operations.

One approval does not authorize future releases or actions. Local preparation,
validation, and PR review should be completed before requesting release approval.
Read-only inspection must respect access permissions and data-handling restrictions.
Prefer development environments, mocks, and fixtures. If production impact is unclear,
explain the risk and obtain specific approval before acting.

Device installation and UI checks are separate from package publication. Follow the
source repository's device rules; a repository change does not authorize a phone
reboot, flash, account reset, or destructive filesystem operation.

## 8. Decision-making and verification

For each meaningful decision: understand the actual problem, simplify the solution,
evaluate UX/DX/AX and compatibility, implement using established patterns, verify
behavior, remove unnecessary complexity, and deliver through the applicable workflow.
Explain real tradeoffs briefly and recommend the strongest solution.

Scale checks to the change:

- **Documentation:** Review accuracy, paths, technical references, and the diff.
- **Catalog:** Parse JSON; check required fields, types, unique IDs, sources, package
  and desktop mappings, repository URL, and compatibility with the Store consumer.
- **Packages:** Check Alpine release and architecture, package version/revision,
  dependencies, signatures, public-key compatibility, index entries, and checksums.
  Verify the index and artifacts are consistent before publication.
- **Application changes:** Run the relevant source-repository checks. Use an approved
  development environment or device check for installation and launch behavior.
- **After an approved release:** Verify served artifacts and catalog/index consistency.
  Distinguish local validation, publication, installation, and runtime verification.

There is currently no application build or test suite in this distribution checkout.
Inspect the current tree and CI configuration rather than assuming that remains true.
Do not introduce a framework merely to validate a small metadata or documentation edit.

## 9. Definition of done

- Requested behavior works and existing functionality remains intact.
- UI changes, when applicable, are minimal, consistent, accessible, and intuitive.
- Implementation is maintainable, focused, and proportionate.
- Store, package-manager, and agent contracts remain compatible.
- Relevant tests and CI checks pass, or limitations are explicitly disclosed.
- Generated packages and indexes are reproducible and traceable to their source.
- The PR is merged under repository rules, or the blocker is clearly reported.
- Any production publication has specific approval and confirmed results.
- No unauthorized customer, infrastructure, device, or destructive action occurred.
- No unnecessary UI, code, dependencies, or complexity were introduced.
- The owner receives a short, direct summary in Polish.

## 10. Priority and conflict resolution

Apply this order within these standing instructions:

1. Safety: production, customer data, security, and explicit approval boundaries.
2. Correctness: existing functionality and protocol compatibility.
3. Owner rules and explicit owner decisions.
4. UX + DX + AX together.
5. Simplicity and maintainability.
6. Visual and communication style.

A cleaner interface is not better if it is harder to use. Faster delivery is not
better if it breaks compatibility. Agent-friendly architecture is not better if it
unnecessarily complicates the product.

## Final principle

Build software that feels effortless to use, straightforward to maintain, and
natural for agents to operate. Communicate directly, design with restraint, and
engineer with discipline: understand before changing, verify before merging, and
protect what already works.

Do not confuse more features, text, code, or complexity with a better product.
