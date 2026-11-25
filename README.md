# Linkpoint Viewer for Apple platforms

Linkpoint — a viewer for Second Life and OpenSimulator — for **iOS and
macOS** first, with **visionOS in view** (the broader "Apple" scope is
deliberate: the project's VR direction is OpenXR/XR, and Apple's headset
platform is part of that future).

This is a **founding repo** (created 2026-11-25). It contains no app code
yet, on purpose: the first task here is a design decision, not code.

## Status: founding — platform stack decision is OPEN

The first design task for this repo is choosing the platform stack.
Neither option is decided; both are real candidates:

**Option A — native Swift.** SwiftUI app, rendering via Metal directly or
via Filament's iOS builds (Filament ships iOS binaries, and the Android
app's materials are compiled `.filamat` — the same material set could be
shared). Maximum platform fidelity (Apple XR frameworks on visionOS are
Swift-first), at the cost of re-implementing the protocol core in Swift.

**Option B — Kotlin Multiplatform.** The Android app's protocol core
(`core/` in `Kaleaon/Linkpoint-Android`) is pure JVM Kotlin with no
Android dependencies — it was kept that way deliberately — so sharing it
via Kotlin Multiplatform, with a native Swift/SwiftUI shell and renderer,
is a genuine candidate. One protocol core to maintain across both apps;
the cost is KMP/native toolchain complexity and a less-trodden path for
the rendering layer.

Considerations on the table (from the new-version design study,
2026-11-25, and the Android/Lumiya programmes):

- Filament ships iOS builds and is the renderer on Android; material
  sharing argues for keeping it in the frame on either option.
- Voice is WebRTC on the modern grid (caps signalling); vendor WebRTC
  builds exist for iOS/macOS either way.
- visionOS has no OpenXR runtime — Apple's own frameworks (ARKit,
  RealityKit) are the XR path there, which favours a native shell
  whichever core is chosen.
- Linden Lab's Third-Party Viewer policy applies on every platform:
  honest viewer identity/channel strings, encrypted credential storage.

## Shared assets this repo builds on

- **Design authority:** `Kaleaon/linkpoint-design` — the layout packs and
  the Ktheme palette set (app-neutral JSON schema), the logo and its
  geometry/colour-role spec. Apple screens consume the same tokens; no
  per-platform design fork.
- **Reference implementation:** `Kaleaon/Linkpoint-Android` — the new
  version's trunk (Kotlin/Compose/Filament), proven against OpenSim; its
  behaviour specs and test vectors are this repo's starting reference.
- **Protocol truth:** the `secondlife_*` skills library
  (`Kaleaon/secondlife-skills`) and, behind it, the official viewer's
  `message_template.msg` and capability behaviour — features are taken
  from the official viewer / Firestorm / Alchemy / Kokua as specs and
  implemented fresh, never ported from C++.

## Sibling projects

- `Kaleaon/Linkpoint-Android` — Linkpoint Viewer for Android (the trunk)
- `Kaleaon/linkpoint-design` — design authority
- `Kaleaon/React-Linkpoint` — React/TypeScript web companion
- `Kaleaon/Linkpoint` — the earlier umbrella repo

## Licence

MIT — see `LICENSE`. Second Life is a trademark of Linden Lab; this
project is not affiliated with or endorsed by Linden Lab.
