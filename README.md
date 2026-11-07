# Unofficial app-local WireGuard patches for Morphe

**Independent, experimental build. Not affiliated with MorpheApp or Proton.**
Based on Morphe Patches v2.1.0, the v2.1.1 preview provides app-local
WireGuard proxy patches for YouTube and YouTube Music.

## Add this source in Morphe

Open [Add this patch source](https://morphe.software/add-source?github=seo-rii/morphe-wireguard-patches)
on Android, or navigate to **Morphe → Sources → + → Remote** and enter:

`https://github.com/seo-rii/morphe-wireguard-patches`

This is a **standard GitHub release** (not a pre-release). The WireGuard
implementation is nevertheless experimental and not yet validated on a real
device. **WireGuard proxy (experimental) is selected by default** for both
YouTube and YouTube Music; Expert mode is not required to select it.
**Traffic tunneling remains disabled** until a valid WireGuard `.conf` is
imported and the in-app enable switch is explicitly turned on. Each app
keeps its own encrypted profile.
Both `arm64-v8a` and `x86_64` are supported; 32-bit and root-mount modes are
not supported.

**The bundle includes the upstream patches**, so choose **one** patch source
per app instead of combining this source with official Morphe patches.

## Important limitations

- This is a user-space app-local proxy, **not a system VPN**. It does not
  register `VpnService`, a system TUN, or a system-wide kill switch.
- It accepts a bounded, single-peer WireGuard `.conf` with numeric endpoint
  and DNS settings. Only hooked requests are routed through it. Other app
  network paths, Cast, and separate processes are not guaranteed to be covered.
- The **v2.1.0 base** passed host compilation, **13 tests**, real Morphe
  loader (202 patches), DEX, JNI and native-payload checks. The v2.1.1
  default-selection update uses exact one-byte class-file changes for each
  app, checked with JVM bytecode inspection and ZIP integrity checks.
  **It was not fully recompiled or retested through Gradle.** Actual target
  APK patching, playback, WireGuard handshakes, and leak tests remain unverified.
- The published bundle is unsigned. Treat it as an experimental preview, not a
  verified VPN product. No live VPN accounts or private profiles are included.

## Source and release files

In the [v2.1.1 release](https://github.com/seo-rii/morphe-wireguard-patches/releases/tag/v2.1.1):

- `morphe-wireguard-youtube-music-preview-2.1.1.mpp` — patch bundle,
  SHA-256 `9905a6a9a57d281532c94a9b53215a5414ccc4185b880031c0f67d0361fd69ca`.
- `morphe-wireguard-prototype-source.zip` — **full corresponding source**,
  including the modified Morphe project, vendored WireGuard/gVisor code,
  built native payloads, and four pinned public build-source dependencies.
- [`wireguard-prototype.patch`](wireguard-prototype.patch) — patch against
  upstream Morphe v2.1.0 for review.

The source ZIP includes `README-WIREGUARD.md`, build instructions,
unit tests, exact dependency revisions, the Music validation report, and
the bytecode-default update script used to produce v2.1.1 from v2.1.0.
See [release SHA-256 checksums](RELEASE-SHA256SUMS.txt).

## License and provenance

This is a modified, unofficial derivative of
[MorpheApp/morphe-patches](https://github.com/MorpheApp/morphe-patches)
at `8fa2c7a1a832498de8f179748998a3ca87e198eb`.
The original [GPL-3.0 license](LICENSE) and mandatory
[NOTICE](NOTICE) are preserved; bundled dependencies have their own
license notices in the complete source ZIP. No proprietary YouTube APK,
credentials, or private signing keys are distributed.
