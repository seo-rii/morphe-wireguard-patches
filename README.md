# Unofficial app-local WireGuard patches for Morphe

**Independent, experimental build. Not affiliated with MorpheApp or Proton.**
The [v2.1.4 release](https://github.com/seo-rii/morphe-wireguard-patches/releases/tag/v2.1.4)
provides app-local WireGuard proxy patches for YouTube and YouTube Music,
retaining the Morphe Patches v2.1.0 base and its other patches.

## Add this source in Morphe

Open [Add this patch source](https://morphe.software/add-source?github=seo-rii/morphe-wireguard-patches)
on Android, or navigate to **Morphe → Sources → + → Remote** and enter:

`https://github.com/seo-rii/morphe-wireguard-patches`

This is a standard GitHub release; the WireGuard feature remains experimental.
**WireGuard proxy (experimental) is selected by default for both apps. Routing
itself defaults to off.** Import a supported `.conf`, explicitly enable it in
each patched app, and restart. Each app keeps its own encrypted profile.
The bundle includes the other patches from its upstream base, so choose **one
patch source per app** instead of combining it with official Morphe patches.
After updating this source, patch and install the app again to receive changes.

## Changes in v2.1.4

- Settings and validation explanations use the established Android XML resources,
  `StringRef` and Morphe language context. The native parser returns 19 fixed
  diagnostic codes; only known codes select translated resources. Unknown
  payloads use a generic localized failure without exposing backend text.
- All 60 WireGuard settings strings have custom translations for Korean,
  Japanese, simplified Chinese, traditional Chinese, Spanish, French, German
  and Brazilian Portuguese. Missing translations use the English source text.
- These translations were added to the custom distribution at the maintainer's
  request. They are not an official Crowdin export. The upstream contribution
  retains the English-source/Crowdin translation workflow.
- The source-built transport, encrypted profile format, key alias, default-off
  routing policy and supported configuration subset are retained. License and
  copyright notices remain verbatim.

The existing [v2.1.3 maintenance fixes](docs/wireguard-maintenance-2.1.3.md),
including Morphe-style preferences, safer import diagnostics and transport
cleanup, remain included. Earlier releases are preserved.

## Verification status

The i18n preparation cohort passed full source builds, 30 workflow tests per
source tree, 80 host JNI checks, 37 Python checks and Go race/vet checks. It also
passed focused APK checks and isolated synthetic Korean resource fixtures.
Those preparation results do not establish final release artifact verification.

**Final v2.1.4 build, APK and eight-locale checks: 609 Java/Kotlin tests passed, 5 existing APK-dependent skips; 80 host JNI checks and 41 Python tests passed.**
The four final APK cases and all 2,160 compiled resource value comparisons also passed.
See the [v2.1.4 localization report](docs/wireguard-i18n-2.1.4.md) for the exact
artifact identity, test scope and remaining checks.

**No Android target-app acceptance test has been executed for v2.1.4.** The
maintainer's successful v2.1.1 device use is a separate historical report.
Actual text layout, language selection/restart, profile import, AndroidKeyStore,
remote handshake, playback, DNS/leak behavior, network transitions and concurrent
app/process use require device testing. APK resource inspection does not prove
that translated text renders correctly on a device.

## Supported scope and limitations

- The authenticated app-local HTTP proxy uses a userspace TCP/IP stack and
  WireGuard. It does not register `VpnService`, use root, require a separate
  WireGuard app, or occupy the system VPN slot.
- Only intercepted network paths are tunneled. Other app requests, WebView,
  Cast and separate processes are not guaranteed to be covered. This is **not
  a system-wide VPN or complete kill switch**.
- Supported APK ABIs: `arm64-v8a` and `x86_64`. APKs containing 32-bit ABIs and
  root-mount installation are unsupported.
- WireGuard requires YouTube 20.47.62 or newer among the declared targets.
  Compatibility metadata is separate from executed APK/device tests.
- Profiles require one interface/peer, UTF-8 text of at most 64 KiB, a numeric
  endpoint and numeric DNS covered by `AllowedIPs` and an interface address
  family. Hostname endpoints, DNS search domains, shell hooks and routing-table
  options remain unsupported.
- Intercepted requests have no direct fallback on tunnel failure. Local proxy
  readiness does not prove a remote handshake or Internet access.
- The previously reported import failure has not been reproduced on the
  reporting user's device; localized diagnostics do not prove every such
  failure is resolved.

## Source and release files

The [v2.1.4 release](https://github.com/seo-rii/morphe-wireguard-patches/releases/tag/v2.1.4)
contains:

- `morphe-wireguard-youtube-music-preview-2.1.4.mpp` — unsigned source-built
  bundle; SHA-256 `8010b938869104ab8a80f1ca381f624d43d9bee64e434c84287b4648b52816ae`.
- `morphe-wireguard-prototype-source.zip` — full corresponding modified source,
  pinned dependency source snapshots, build instructions and license notices.
- `RELEASE-SHA256SUMS.txt` — asset checksums.

The review patch is [wireguard-prototype.patch](wireguard-prototype.patch),
against Morphe v2.1.0. Full source: [`8a87d95`](https://github.com/seo-rii/morphe-patches/commit/8a87d95bf51a90b2206c5fa1b414316b06c539cb) on `feat/wireguard-i18n-custom`.
See [source build instructions](docs/wireguard-source-build.md), the source
ZIP's `README-WIREGUARD.md`, `docs/`, `build-support/` and `scripts/`, and the
[archive packaging helper](scripts/package-wireguard-source.py).
The historical v2.1.2 `repair` operation is not part of this source build.

## License and provenance

This is a modified, unofficial derivative of
[MorpheApp/morphe-patches](https://github.com/MorpheApp/morphe-patches)
at `8fa2c7a1a832498de8f179748998a3ca87e198eb`.
Original [GPL-3.0 LICENSE](LICENSE), [NOTICE](NOTICE), dependency licenses and
copyright notices are preserved. The app exposes bundled notices in WireGuard
settings. No proprietary target APKs, personal profiles, private keys, account
credentials or signing secrets are distributed. Dependency-owned AOSP framework
and test APK resources required by ARSCLib are retained with its source/license.
