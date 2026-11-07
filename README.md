# Unofficial app-local WireGuard patches for Morphe

**Independent, experimental build. Not affiliated with MorpheApp or Proton.**
Based on Morphe Patches v2.1.0, this preview provides opt-in app-local
WireGuard proxy patches for YouTube and YouTube Music.

## Add this source in Morphe

Open [Add this patch source](https://morphe.software/add-source?github=seo-rii/morphe-wireguard-patches)
on Android, or navigate to **Morphe → Sources → + → Remote** and enter:

`https://github.com/seo-rii/morphe-wireguard-patches`

This release is a **GitHub pre-release**: enable **Pre-release patches** in
Morphe. Select **WireGuard proxy (experimental)** explicitly in Expert mode.
The patch is off by default, and each app keeps its own encrypted profile.
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
- Host compilation, **13 tests**, real Morphe loader (202 patches), DEX, JNI
  and native payload checks passed. **Actual target APK patching, playback,
  real WireGuard handshakes, and traffic-leak tests have not been run.**
- The published bundle is unsigned. Treat it as an experimental preview, not a
  verified VPN product. No live VPN accounts or private profiles are included.

## Source and release files

In the [v2.1.0 pre-release](https://github.com/seo-rii/morphe-wireguard-patches/releases/tag/v2.1.0):

- `morphe-wireguard-youtube-music-preview-2.1.0.mpp` — patch bundle,
  SHA-256 `c9fb32a42ff7d6228c86d3bd113398f2e21b253b0e3b292d741a4d3b5bbbbf19`.
- `morphe-wireguard-prototype-source.zip` — **full corresponding source**,
  including the modified Morphe project, vendored WireGuard/gVisor code,
  built native payloads, and four pinned public build-source dependencies.
- [`wireguard-prototype.patch`](wireguard-prototype.patch) — patch against
  upstream Morphe v2.1.0 for review.

The source ZIP includes `README-WIREGUARD.md`, build instructions,
unit tests, exact dependency revisions and the Music validation report.
See [release SHA-256 checksums](RELEASE-SHA256SUMS.txt).

## License and provenance

This is a modified, unofficial derivative of
[MorpheApp/morphe-patches](https://github.com/MorpheApp/morphe-patches)
at `8fa2c7a1a832498de8f179748998a3ca87e198eb`.
The original [GPL-3.0 license](LICENSE) and mandatory
[NOTICE](NOTICE) are preserved; bundled dependencies have their own
license notices in the complete source ZIP. No proprietary YouTube APK,
credentials, or private signing keys are distributed.
