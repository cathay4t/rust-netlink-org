# How to Package rust-netlink in Fedora

This document was last updated on April 18, 2024. If a significant amount
of time has passed, please consult the
[Fedora Rust Package Guide][fedora-rust-guide] before following the steps
below.

## Find Consuming Fedora Packages

Use `fedrq whatrequires-src` to identify which Fedora packages depend on
rust-netlink crates:

```bash
fedrq whatrequires-src 'rust-netlink-packet-route*'
fedrq whatrequires-src 'rust-rtnetlink*'
```

For example, the current consumers include:

- `rust-below-tc`
- `nispor`

These packages must be rebuilt when shipping updated rust-netlink crates to
Fedora.

## Update rust-netlink Crates with `rust2rpm`

### Update the crate itself

```bash
cd fedpkg/rust-netlink-packet-route
git checkout rawhide
git clean -fdx
rust2rpm
spectool -g ./*.spec
fedpkg new-sources *.crate
fedpkg commit -m 'upgrade to X.Y.Z'
```

### Update rust-below-tc

Use `rust2rpm --patch` to adjust the dependency version:

```bash
# Fork and clone https://src.fedoraproject.org/rpms/rust-below-tc
cd fedpkg/rust-below-tc
git checkout rawhide
git clean -fdx
rust2rpm --patch
# In the editor, update the `netlink-packet-route` version constraint
fedpkg commit -m 'Bump netlink-packet-route dependency to X.Y.Z'
git push

# Open a merge request at
# https://src.fedoraproject.org/rpms/rust-below-tc/pull-requests
# and request @decathorpe to review and build the package in your sidetag.
# Example: https://src.fedoraproject.org/rpms/rust-below-tc/pull-request/2
```

### Update consumer package

Update rpm SPEC file for lines like:

```
BuildRequires:  (crate(rtnetlink/default) >= 0.18.0 with crate(rtnetlink/default) < 0.19)
```


## Build with Fedora Side Tag

```bash
cd fedpkg/rust-netlink-packet-route
git checkout rawhide
git pull
fedpkg request-side-tag

# Note the side tag, e.g. f41-build-side-109296
SIDE_TAG="f41-build-side-109296"

# Build in the side tag
fedpkg build --target=$SIDE_TAG

# Wait for the side tag repository to be ready before building the next package
koji wait-repo --request $SIDE_TAG -v

# Repeat the same process for `rtnetlink`, other rust-netlink crates,
# and all impacted consumer packages.
```

## Ship via Bodhi

Even updates to rawhide require a Bodhi update to ship side tag RPMs.

Once all builds are complete, visit
https://bodhi.fedoraproject.org/updates/new, select the side tag (instead of
manually entering package NVRs), and create the update.

[fedora-rust-guide]: https://docs.fedoraproject.org/en-US/packaging-guidelines/Rust/
