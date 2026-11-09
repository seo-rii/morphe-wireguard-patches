# Unofficial app-local WireGuard patches for Morphe

**Independent, experimental build. Not affiliated with MorpheApp or Proton.**
The [v2.1.5 release](https://github.com/seo-rii/morphe-wireguard-patches/releases/tag/v2.1.5)
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

## Changes in v2.1.5

- Fix the internal WireGuard settings page resolving strings from a global
  context/cache while its entry menu used the localized settings context.
  Titles, preference rows, status text, dialogs and toasts now resolve resources
  through the WireGuard settings Activity itself.
- Keep the existing Morphe language context and restart behavior. All 60 strings
  remain translated for Korean, Japanese, simplified/traditional Chinese,
  Spanish, French, German and Brazilian Portuguese, with English fallback.
- Keep the 19 fixed native diagnostic codes, transport, encrypted profile
  format/key alias, connection default-off policy and patch default selection.
  License and copyright notices remain verbatim.

The translations remain custom-distribution additions requested by the
maintainer, not an official Crowdin export. The upstream contribution retains
English source resources and the Crowdin translation workflow. Existing
[v2.1.3 maintenance fixes](docs/wireguard-maintenance-2.1.3.md) and
[v2.1.4 translations](docs/wireguard-i18n-2.1.4.md) remain included. Earlier
releases are preserved.

## Verification status

- Final source build and regression checks: **64 affected Java tests per source tree plus source bundle/DEX/JNI/resource/native checks passed**.
- Final APK patching/resource inspection: **YouTube 21.30.216 and Music 9.30.53 passed four focused/default Desktop --validate cases; all 2,160 compiled resource values matched**.
- Standalone Android settings Activity runtime checks:
  **four pre-fix failures reproduced; four fixed scenarios passed all 124 comparisons on API35/x86_64 KVM, using real Activity/widgets and explicit backend stubs**.

The v2.1.4 checks matched 2,160 compiled resource values across four APKs. Those
checks established resource inclusion and value equality; they did not execute
the internal screen or prove which context resolved its displayed strings.
See the [v2.1.5 localization fix report](docs/wireguard-i18n-2.1.5.md) for exact
artifact identity, regression evidence and the scope of the runtime harness.

**No Android target-app acceptance test has been executed for v2.1.5.** A
standalone Android harness with backend collaborators stubbed is separate from
running the patched YouTube/Music apps. Target-app text layout, language
selection/restart, profile import, AndroidKeyStore, remote handshake, playback,
DNS/leak behavior, network transitions and concurrent app/process use require
further device testing. The maintainer's successful v2.1.1 device use remains a
separate historical report.

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

The [v2.1.5 release](https://github.com/seo-rii/morphe-wireguard-patches/releases/tag/v2.1.5)
contains:

- `morphe-wireguard-youtube-music-preview-2.1.5.mpp` — unsigned source-built
  bundle; SHA-256 `ec0dc5bce25f795fbcae9c4abfd9a8dc2798e800355f88eb04f308f986998627`.
- `morphe-wireguard-prototype-source.zip` — full corresponding modified source,
  pinned dependency source snapshots, build instructions and license notices.
- `RELEASE-SHA256SUMS.txt` — asset checksums.

The review patch is [wireguard-prototype.patch](wireguard-prototype.patch),
against Morphe v2.1.0. Full source commit: `ac1a922bd896160c896764d7535698295fed487a` on `feat/wireguard-i18n-custom`.
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
