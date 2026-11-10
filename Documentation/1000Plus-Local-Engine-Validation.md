# 1000+ Capital LEAN fork | OSS-first local engine validation

**Status: proposed test protocol; no local build, broker certification or production readiness is claimed by this document.**

This is a public Apache-2.0 upstream fork. Keep it suitable for rebasing. Proprietary execution-only algorithms, OMS/EMS/PMS, account configuration, trading controls, broker credentials, evidence archives and strategy code belong in private repositories, not here.

## Public engine baseline

On a Linux workstation with the SDK version specified by the current upstream README (currently .NET 10), record the fork commit and runtime before attempting a local build:

```bash
git rev-parse HEAD
dotnet --info
dotnet restore Launcher/QuantConnect.Lean.Launcher.csproj
dotnet build Launcher/QuantConnect.Lean.Launcher.csproj -c Release --no-restore
```

Do not label these commands as successfully tested until a CI/local run records the result. The full upstream test suite may need additional dependencies or market-data fixtures; scope and run those tests separately. Do not use `lean live` or real broker accounts for this baseline.

## Integration contract for the private EMS

- LEAN runs as a **separate Launcher worker**. The private EMS supervisor owns durable intent/attempt history, fencing, broker IDs and account trading-readiness; a healthy Launcher is not authorization to trade.
- The private execution-only QCAlgorithm/bridge consumes approved instructions through a controlled local interface. LEAN order tickets are not a durable cross-restart order journal.
- Demonstrate deterministic MockBroker cases for ACK, partial fill, cancel/fill race, unknown ACK, gateway loss, worker restart and broker read-back. After restart, the private supervisor must remain HALTED until independent reconciliation and authorized resume.
- Record exact LEAN fork SHA, engine/plugin version, license/entitlement review, Linux runtime, broker order-type support, known limitations and upstream rebase result. Avoid core engine patches unless a measured feasibility spike demonstrates necessity.
- Use the same pinned engine image digest in local Linux and eventual AWS EC2 paper/staging. AWS secrets and telemetry are environment adapters, not changes to the engine's order semantics.

**Acceptance:** a private EMS integration test, not this public fork's build, must prove pre-send durability and no duplicate broker transmission after an ambiguous ACK. Local mock testing does not prove live PB compatibility, adviser regulatory compliance or institutional ODD operating effectiveness.

This file intentionally excludes nonpublic internal documentation and contact details.

## External-engine integration failure matrix (private execution tests required)

This public file defines a **build/test protocol**, not broker safety implementation. Capture `git rev-parse HEAD`, `dotnet --info`, restored NuGet dependency manifest and Release build output; retain CI logs outside the public repository when they contain nonpublic references. Do not guess SDK requirements from unrelated sample workflows.

The private EMS integration must inject, then verify, all of these conditions before live use: worker killed between broker send and ACK; stale worker resuming after a new execution generation; partial fill arriving after a cancel request; engine unable to enumerate all broker open orders; gateway reauthentication; identical broker execution delivered twice; and a restored journal older than PB order history.

A LEAN process restart is **not** authority to start orders. LEAN may be healthy while a broker-resting order remains active and the private EMS state is UNKNOWN. If the broker/adapter cannot enforce sender fencing, no automatic active-worker failover is allowed. Proven old-session termination, PB order/fill read-back, independent PMS verification and authorized resume are prerequisites. Keep all account identifiers, proprietary algorithms, broker captures, investor records and internal control evidence out of this public fork.

**Verification status:** scenarios are obligations in the private EMS; this document does not demonstrate that they were run.
