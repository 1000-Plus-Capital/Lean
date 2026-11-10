# Claude Code | 1000+ Capital LEAN fork

This is a **public Apache-2.0 fork** of [QuantConnect/Lean](https://github.com/QuantConnect/Lean). Preserve upstream license and copyright notices, mark modified files correctly and retain a clean upstream synchronization path.

**Role:** LEAN is a separately running engine launched via its C# Launcher. It is not a generic HTTP OMS or a library used by every system. The proprietary OMS/EMS/PMS code and execution-only client algorithm belong in separate **private repositories**. Never put credentials, customer records, strategy models, private URLs or nonpublic operational details in this public fork.

## Immediate work

1. Inventory the current fork revision, licenses and dependencies. Build and run its documented Linux backtest smoke test without any broker account or paid cloud service. Document the exact .NET/runtime version and successful commands.
2. Inspect Launcher, Algorithm/Framework, Engine/TransactionHandlers, ITransactionHandler, QCAlgorithm order submission and OnOrderEvent callbacks, brokerage interface, datafeed and config-driven extension points.
3. Identify the narrowest supported method for an **external, approved, durable intent** to be consumed by an execution-only QCAlgorithm in a separate process while preserving pre-send records and order-event correlation. Produce a short upstream-path design note and an executable local proof with simulated broker only.
4. Demonstrate normal ACK, immediate fill, partial fill, cancel/fill race, process restart, and broker open-order reconstruction limitations. Track engine order ID versus broker order/execution identifiers. Do not assert exactly-once transmission from local database idempotency alone.
5. Prefer no modifications to LEAN core. If a small compatibility hook is necessary, write minimal isolated code with unit tests, extension-point documentation and an upstream rebase plan. Never modify transaction handling wholesale without evidence.
6. Maintain a public compatibility matrix: fork revision, plugin/connector license, Linux runtime, tested order types, known broker capability gaps, and how the private integration adapter should plug in without source disclosure.

## Restrictions

No production broker accounts, no order-routing changes in upstream without approved tests, no server-side proprietary investment logic, no live credentials, no copying code or internal documents from private repositories. Separate process health from trading readiness. Treat restored LEAN tickets as transient; the firm-specific EMS owns durable attempt history and the PMS independently reconciles broker results.

The default coding task is **engine feasibility**, not building an enterprise trading platform in this public repo. Keep patches small and follow standard upstream project conventions.

## Public fork boundary and operational hooks

This repository is public; never add private on-call contact details, investor requests, adviser/researcher identity, production credentials, SOC reports or nonpublic control architecture. Ensure execution-only algorithm integration remains in private EMS. If engine changes are needed, restrict them to small, upstream-compatible extensions exposing lifecycle health, order-event source identifiers, restart epoch and broker reconciliation support. This fork must not become an alternative OMS, PMS, fund accounting or compliance system.

Use deterministic local simulation for halt, unknown order, gateway loss, process restart and no-blind-resend tests. Any diagnostics must omit trading secrets, investor data and proprietary alpha. Provide stable version/build provenance for the EMS evidence chain, while leaving firm policy and formal assurance in private tech-docs and EMS.
