# Runa's SalmonEgg fork

This fork of [salmonloop/salmon-egg](https://github.com/salmonloop/salmon-egg) lives at `toddams/salmon-egg` on the
branch `runa/compaction-capability`, and is Runa's submodule `libs/salmon-egg`. It branches from upstream `71400f2`
(main on 2026-11-13). Its `src/SalmonEgg.Acp` is the same as at `96cfe86`, the commit the SalmonEgg.Acp 1.2.0 package
Runa used before was built from. Runa references `src/SalmonEgg.Acp/SalmonEgg.Acp.csproj` as a project reference;
nothing else in this repository is built by Runa.

The entries run oldest first.

## At a glance

| Date | Change |
|---|---|
| 2026-11-13 | A v1 client can declare `clientCapabilities.session.compaction` |

---

## A v1 client can declare `clientCapabilities.session.compaction`

- **Used by:** Runa's chat, which declares it in every `initialize`
- **Tests:** `AcpJsonContextTests.InitializeParams_V1DeclaresSessionCompactionCapability` and
  `AcpJsonContextTests.ClientSessionCapabilities_OmitsCompactionWhenNotDeclared`

**Why.** ACP's [Session Compaction RFD](https://agentclientprotocol.com/rfds/session-compaction.md) adds the
`compaction_update` and `compaction_summary_chunk` session updates, and in v1 an agent may send them only to a client
that sets `clientCapabilities.session.compaction` to an object. `ClientSessionCapabilities` had only `configOptions`
and no room for other fields, so a client could not declare it. Without it, claude-agent-acp reports a compaction as a
"Compact conversation" tool call with no summary.

**What changed.** `ClientSessionCapabilities.Compaction` (`compaction`), of the new empty `CompactionCapabilities`
record, registered in `AcpJsonContext` and listed in `PublicSurface.Types.txt` as stable. The two updates themselves
are not modelled: a client reads them as unknown update kinds (`SessionUpdate.UnknownUpdateKind` with their fields in
`ExtensionData`).
