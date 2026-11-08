# Unofficial app-local WireGuard patches for Morphe

**Independent, experimental build. Not affiliated with MorpheApp or Proton.**
The [v2.1.3 release](https://github.com/seo-rii/morphe-wireguard-patches/releases/tag/v2.1.3)
provides app-local WireGuard proxy patches for YouTube and YouTube Music,
based on Morphe Patches v2.1.0. It is built from the complete modified source.

## Add this source in Morphe

Open [Add this patch source](https://morphe.software/add-source?github=seo-rii/morphe-wireguard-patches)
on Android, or navigate to **Morphe → Sources → + → Remote** and enter:

`https://github.com/seo-rii/morphe-wireguard-patches`

This is a standard GitHub release, rather than a pre-release; the WireGuard
feature remains experimental. **WireGuard proxy (experimental) is selected by
default for both apps**, including in the Android patch DEX. **WireGuard routing
itself defaults to off**: import a supported `.conf`, explicitly enable it in
the patched app, and restart. Each app keeps its own encrypted profile.

The bundle includes the other patches from its upstream base. Choose **one
patch source per app** instead of combining this source with official Morphe
patches.

## Changes in v2.1.3

- WireGuard settings reuse Morphe themes, toolbar and preference rows, with a
  confirmation and restart step for the enable switch.
- Import errors distinguish document access, UTF-8, file size, unsupported
  configuration, native loading, and secure storage. Native validation reasons
  contain no profile values, document addresses or keys.
- Standard `PersistentKeepalive = off` and numeric `Address`/`AllowedIPs` without
  CIDR masks are accepted. A bare route denotes one host (`/32` or `/128`).
- Maintenance fixes abort truncated plain HTTP responses, stop peers before
  closing the native device, recover committed profile backups, and improve
  cleanup after proxy startup failures.
- Both native ABIs, extensions and Android patch DEX are built through Gradle
  from source. The v2.1.3 build does not modify an older release's bytecode.

The encrypted profile format and key alias are retained. Existing releases and
their historical [default-selection correction](docs/wireguard-default-verification.md)
remain available.

## Verification status

The maintainer reported successful practical use of **v2.1.1** on a physical
Android device on 2026-11-07. That report is separate from v2.1.3 verification.
**No new Android target-app acceptance test was executed for v2.1.3.**

For this release, the full source build passed with **603 tests passed and 5
existing APK-dependent tests skipped**. That total includes the 24 new
import-workflow tests. Separate checks passed: 42 host JNI checks, 33 Python
checks, and Go race/vet checks.
YouTube **21.30.216** and Music **9.30.53** each passed Morphe Desktop
`--validate` with the WireGuard-only and full default selections. These four
cases cover host patching and static APK inspection, not app launch or traffic.

See the [maintenance report](docs/wireguard-maintenance-2.1.3.md) for the exact
scope, source/bundle identity and remaining device checks.

## Supported scope and limitations

- This is an authenticated app-local HTTP proxy using a userspace TCP/IP stack
  and WireGuard. It does not register Android `VpnService`, use root, require a
  separate WireGuard app, or occupy the system VPN slot.
- Only requests intercepted by the patched network paths are tunneled. Other
  app requests, Cast, WebView and separate processes are not guaranteed to be
  covered. This is **not a system-wide VPN or complete kill switch**.
- `arm64-v8a` and `x86_64` are supported. APKs containing 32-bit ABIs and root
  mount installation are unsupported.
- WireGuard requires YouTube 20.47.62 or newer among the bundle's declared
  targets. Music uses its declared target list. Compatibility metadata is
  separate from the tested versions listed above.
- Profiles must be UTF-8 text of at most 64 KiB with one interface and one peer,
  a numeric endpoint IP, and numeric DNS servers covered by `AllowedIPs` and a
  configured address family. Endpoint hostnames, DNS search domains, shell
  hooks and routing-table options remain unsupported.
- Intercepted requests have no direct fallback when WireGuard fails. Local
  proxy readiness does not prove a server handshake or Internet access.
- The reported import failure on another user's setup has not been reproduced
  on that device. The syntax fixes and clearer diagnostics do not establish
  that every reported import problem is resolved.

The bundle is unsigned. Android profile import, AndroidKeyStore integration,
server handshake, playback, DNS/traffic leak behavior, network transitions,
restart behavior and concurrent app use still need device acceptance testing.

## Source and release files

The [v2.1.3 release](https://github.com/seo-rii/morphe-wireguard-patches/releases/tag/v2.1.3)
contains:

- `morphe-wireguard-youtube-music-preview-2.1.3.mpp` — source-built patch bundle,
  SHA-256 `557463a7dd755857d81742f998e34b654010c75dc1de830e713c0b8fc569b25d`.
- `morphe-wireguard-prototype-source.zip` — full corresponding modified source,
  pinned dependency source snapshots, build instructions and license notices.
- `RELEASE-SHA256SUMS.txt` — release asset checksums.

[`wireguard-prototype.patch`](wireguard-prototype.patch) contains the changes
against Morphe v2.1.0. The full project source is also pinned at
[`b13c6c88a`](https://github.com/seo-rii/morphe-patches/tree/b13c6c88a40835ba81f1b44510fd0878fade4b4d).
See [source build instructions](docs/wireguard-source-build.md)
and the source ZIP's `README-WIREGUARD.md`, `docs/`, `build-support/` and
`scripts/`. The historical repair script is retained for v2.1.2 provenance;
v2.1.3 uses its `verify` operation only.

## License and provenance

This is a modified, unofficial derivative of
[MorpheApp/morphe-patches](https://github.com/MorpheApp/morphe-patches)
at `8fa2c7a1a832498de8f179748998a3ca87e198eb`.
The original [GPL-3.0 license](LICENSE) and mandatory [NOTICE](NOTICE) are
preserved. Dependency versions and copyright/license notices are included in
the source ZIP and patch bundle; the patched app exposes them in WireGuard
settings. Proprietary target APKs, personal profiles, private keys, account
credentials and signing secrets are not distributed.

Source archive assembly is reproducible with the tracked
[packaging helper](scripts/package-wireguard-source.py), using the frozen clean
source commit, four pinned dependency checkouts and tested bundle. Its `prepare`
step stages dependency sources and its `package` step records per-file hashes.
The ZIP includes provenance manifests; no target app APKs or credentials are
distributed. Required dependency-owned AOSP framework/test APK resources are
retained with ARSCLib source and licensing.
