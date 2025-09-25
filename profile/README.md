# Netlink

This project aims at providing building blocks for [netlink][man-netlink] (see
`man 7 netlink`).

If you seeking crates to communication with linux netlink, please try:
- The [`rtnetlink`][rtnetlink] crate provides higher level abstraction for the
  [route protocol][man-rtnetlink]
- The [`audit`][audit] crate provides higher level abstractions for the audit
  protocol.
- The [`genetlink`][genetlink] crate provides higher level abstraction for the
  [generic netlink protocol][man-genl]
- The [`ethtool`][ethtool] crate provides higher level abstraction for
  [ethtool netlink protocol][ethtool-kernel-doc]
- The [`mptcp-pm`][mptcp-pm] crate provides MPTCP path manager netlink protocol
- The [`wl-nl80211`][wl-nl80211] crate provides wireless nl80211 netlink
  protocol

If you seeking crates to parsing or emitting netlink packet, please try:

- Each netlink protocol has a `netlink-packet-<protocol_name>` crate that
  provides the packets for this protocol:
    - The [`netlink-packet-wireguard`][netlink-packet-wireguard] crate provide
      netlink message for wireguard.
    - [`netlink-packet-route`][netlink-packet-route] provides messages for the
      [route protocol][man-rtnetlink]
    - [`netlink-packet-audit`][netlink-packet-audit] provides messages for the
      [audit][man-audit] protocol
    - [`netlink-packet-sock-diag`][netlink-packet-sock-diag] provides messages
      for the [sock-diag][man-sock-diag] protocol
    - [`netlink-packet-generic`][netlink-packet-generic] provides message for
      the [generic netlink][man-genl] protocol
    - [`netlink-packet-netfilter`][netlink-packet-netfilter] provides message
      for the `NETLINK_NETFILTER` protocol
    - [`netlink-packet-xfrm`][netlink-packet-xfrm] provides message for IPsec

Shared crates but designed for internal usage:
- The [`netlink_sys`][netlink-sys] crate provides netlink sockets. Integration
  with [`mio`][mio] and [`tokio`][tokio] is optional.
- The [`netlink-packet-core`][netlink-packet-core] is the glue for all the
  other `netlink-packet-*` crates. It provides a `NetlinkMessage<T>` type that
  represent any netlink message for any sub-protocol.
- The [`netlink-proto`][netlink-proto] crate is an asynchronous implementation
  of the netlink protocol. It only depends on `netlink-packet-core` for the
  `NetlinkMessage` type and `netlink-sys` for the socket.

If you want to add more netlink related crates into this organization, please
open pull request to [rust-netlink/new-crate-review][new-crate-review].

## Altnernatives

- https://github.com/jbaublitz/neli: the main alternative to these crates, as
  it is actively developed.
- https://github.com/one-d-wide/netlink-bindings: Rust bindings for
  encoding/decoding Netlink messages generated from Linux kernel YAML netlink
  API
- Other but less actively developed alternatives:
  - https://github.com/achanda/netlink
  - https://github.com/polachok/pnetlink
  - https://github.com/crhino/netlink-rs
  - https://github.com/carrotsrc/rsnl
  - https://github.com/TaborKelly/nl-utils

## Architecture Decision Records

The architecture desion records are stored in
https://github.com/rust-netlink/.github/tree/main/architecture_decisions

## Credits

My main resource so far has been the source code of [`pyroute2`][pyroute2]
(python) and [`netlink`][netlink-go] (golang) a lot. These two projects are
great, and very nicely written. As someone who does not read C fluently, and
that does not know much about netlink, they have been invaluable.

I'd also like to praise [`libnl`][libnl] for its documentation. It helped me a
lot in understanding the protocol basics.

The whole packet parsing logic is inspired by @whitequark excellent blog posts
([part 1][whitequark-1], [part 2][whitequark-2] and [part 3][whitequark-3],
although I've only really used the concepts described in the first blog post).

Thanks also to the people behind [tokio](tokio.rs) for the amazing tool they
are building, and the support they provide.

## Contact

 * Crate github issue
 * Use Matrix room: [`#rust-netlink:fedora.im`][matrix_room_url]

[man-netlink]: https://www.man7.org/linux/man-pages/man7/netlink.7.html
[man-audit]: https://man7.org/linux/man-pages/man3/audit_open.3.html
[man-sock-diag]: https://www.man7.org/linux/man-pages/man7/sock_diag.7.html
[man-rtnetlink]: https://www.man7.org/linux/man-pages/man7/rtnetlink.7.html
[man-genl]: https://www.man7.org/linux/man-pages/man8/genl.8.html
[generic-netlink-lwn]: https://lwn.net/Articles/208755/
[mio]: https://github.com/tokio-rs/mio
[tokio]: https://github.com/tokio-rs/tokio
[route-proto-doc]: https://www.infradead.org/~tgr/libnl/doc/route.html
[netlink-go]: https://github.com/vishvananda/netlink
[pyroute2]: https://github.com/svinota/pyroute2/tree/master/pyroute2/netlink
[libnl]: https://www.infradead.org/~tgr/libnl
[whitequark-1]: https://lab.whitequark.org/notes/2016-12-13/abstracting-over-mutability-in-rust
[whitequark-2]: https://lab.whitequark.org/notes/2016-12-17/owning-collections-in-heap-less-rust
[whitequark-3]: https://lab.whitequark.org/notes/2017-01-16/abstracting-over-mutability-in-rust-macros
[ethtool-kernel-doc]: https://www.kernel.org/doc/html/latest/networking/ethtool-netlink.html
[netlink-sys]: https://github.com/rust-netlink/netlink-sys
[netlink-packet-route]: https://github.com/rust-netlink/netlink-packet-route
[netlink-packet-audit]: https://github.com/rust-netlink/netlink-packet-audit
[netlink-packet-sock-diag]: https://github.com/rust-netlink/netlink-packet-sock-diag
[netlink-packet-generic]: https://github.com/rust-netlink/netlink-packet-generic
[netlink-packet-netfilter]: https://github.com/rust-netlink/netlink-packet-netfilter
[netlink-packet-core]: https://github.com/rust-netlink/netlink-packet-core
[netlink-proto]: https://github.com/rust-netlink/netlink-proto
[rtnetlink]: https://github.com/rust-netlink/rtnetlink
[audit]: https://github.com/rust-netlink/audit
[genetlink]: https://github.com/rust-netlink/genetlink
[ethtool]: https://github.com/rust-netlink/ethtool
[mptcp-pm]: https://github.com/rust-netlink/mptcp-pm
[wl-nl80211]: https://github.com/rust-netlink/wl-nl80211
[new-crate-review]: https://github.com/rust-netlink/new-crate-review
[netlink-packet-xfrm]: https://github.com/rust-netlink/netlink-packet-xfrm
[netlink-packet-wireguard]: https://github.com/rust-netlink/netlink-packet-wireguard
[matrix_room_url]: https://app.element.io/#/room/#rust-netlink:fedora.im
