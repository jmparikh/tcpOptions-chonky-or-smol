# tcpOptions-chonky-or-smol

Companion Internet-Draft to
[draft-bonica-tcpm-extended-options](https://datatracker.ietf.org/doc/draft-bonica-tcpm-extended-options/):
a selection algorithm for TCP stacks that support **both** SEG-U (up to
1016 octets of options) and SEG-O (classic TCP, 40-octet cap) — a.k.a.
"chonky or smol?"

**Draft:** `draft-parikh-large-tcp-options-selection-algo-00`
**Status:** work in progress. Several sections still carry `!.ADD INFO.!`
placeholders — see the open issues on this repo.

## What's the problem?

Once a host supports both SEG-U and SEG-O, either can turn up at
connection setup. If both arrive for what would otherwise be the same
session, the host must commit to one format. Absent a defined
procedure, the outcome is implementation-dependent — which breaks the
interoperability promise of the SEG-U draft.

This document defines that procedure: selection inputs, tie-breaking,
fallback behaviour, and how the selection event slots into the
existing TCP state machine.

## SEG-U options region (quick reference)

Standard 20-byte TCP header with `Data Offset = 0` marks a SEG-U.
What would normally be the payload starts with:

    +--------+-----------------------+
    | Length |       Reserved        |   <- 4-byte prefix
    +--------+-----------------------+
    |        Individual Options      |   <- up to 1016 bytes
    +--------------------------------+
    |             Data               |
    +--------------------------------+

`Length` is in 4-octet units and covers the prefix itself. Segment
data begins at byte `20 + Length × 4` from the start of the TCP
header.

## Files

- `draft-parikh-large-tcp-options-selection-algo-00.xml` — draft source (xml2rfc v3).
- `draft-parikh-large-tcp-options-selection-algo-00.txt` — rendered text.
- `revisions/` — historical versions, kept for diffing.
- `attic/` — older drafts and scratch material.

## Related

- [draft-bonica-tcpm-extended-options](https://datatracker.ietf.org/doc/draft-bonica-tcpm-extended-options/) — defines SEG-U itself.
- [tcp-u-poc](https://github.com/jmparikh/tcp-u-poc) — scapy-based SEG-U reference implementation and inter-op test suite.

## Building the draft

```bash
pip install xml2rfc
xml2rfc --text draft-parikh-large-tcp-options-selection-algo-00.xml
```

---

*Authors: Jainam Parikh (Arista Networks), Ron Bonica (HPE Juniper).*
