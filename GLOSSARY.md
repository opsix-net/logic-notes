# Glossary

New concepts are appended as research notes are published.


<!-- publication:2026-10-02 -->

## 2026-10-02

Source note: [2026-10-02](articles/2026-10-02-trading-applications-data-protocols-safe-boundaries.md).

### FIX

Financial Information eXchange, a messaging protocol used to communicate financial information and instructions. It provides conventions for encoding messages, but implementations can differ by protocol version and venue-specific requirements, so support for FIX alone does not guarantee drop-in compatibility.

### Tag

A numeric field identifier in a FIX message, paired with a value in `tag=value` form. Tags identify items such as message type or instrument. Applications should use a clear internal mapping and validate required fields against the relevant protocol and venue rules.

### SOH

Start of Header, the ASCII control byte `0x01` used to separate FIX fields. A printable character such as a vertical bar may represent SOH in documentation, but must not replace the actual byte in a transmitted message.

### BodyLength

The value in FIX tag 9, giving the byte length of the message body: the bytes after the delimiter ending tag 9 and before the start of tag 10. It must be calculated from the serialized byte sequence, including field delimiters.

### Checksum

The value in FIX tag 10, calculated from the message bytes preceding that tag according to the protocol’s checksum rule. It is a transmission-format check, not proof that the message is valid for a venue or that an order is safe or accepted.

### Session

An established exchange of messages between identified sender and target systems. A session includes connection and lifecycle behavior such as logon and logout; exact authentication, recovery, and required message fields depend on the counterparty’s specification.

### Venue adapter

A software boundary that translates internal application concepts into a particular broker’s or trading venue’s interface. It isolates differences in fields, supported order types, and connection conventions from strategy and domain logic.

### Syntactic validity

Conformance to a message’s structural rules, such as field encoding and required formatting. A syntactically valid request may still be semantically inappropriate, unauthorized, unsupported, or outside risk limits.

### Ragged row

A CSV data record whose number of fields differs from the number of header columns. In this utility, such rows are reported by file line number and excluded from per-column statistics because their values cannot be safely aligned to the header.

### Finite numeric value

A value successfully parsed as a number that is neither positive infinity, negative infinity, nor NaN. The CSV profiler includes finite numeric values in counts and summaries, while excluding nonfinite values from those calculations.
