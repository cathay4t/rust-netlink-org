# Title: ADR-003: Will not use linux kernel YAML netlink API

## Status: Accepted

## Context

Linux kernel introduced YAML netlink API at
https://docs.kernel.org/userspace-api/netlink/specs.html

It allows usespace tool to generate netlink API binding by parsing YAML file
for parsing and emitting netlink packets.

## Decision

The `rust-netlink` project will not use Linux kernel YAML netlink API because:

 1. Using codegen tool to generate rust crate API is prone to API breakage
    caused by trivial changes to codegen tool.

 2. The `rust-netlink` crates are also used for FreeBSD, Fuchsia which do not
    have this YAML netlink API. So rust-netlink should do packet parsing and
    emitting manually for them. With that done manually, the remaining socket
    communication is trivial.

## Consequences

### Better

* More user, more test, less bug.
* Precise and elegant code.

### Worse

More human efforts on reading kernel C code.
