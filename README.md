# Idle Cinematics

## [**Showcase 1**](https://youtu.be/Fm_YjI3Huwc)

## [**Showcase 2**](https://youtu.be/7e2g-ZryE6w?si=pBmDQYdxyDIQBUET)

A lightweight cinematic idle-camera mod for **Helldivers 2** inspired from **Forza Horizon and NFS**

When you remain idle in a supported vehicle driver seat, the mod automatically transitions into a cinematic camera sequence with smooth gliding shots, optional cinematic tilt compositions, HUD hiding, and configurable shot behavior. Moving again immediately returns control and restores the camera and HUD state.

**Current release:** v1.0.0  
**BSL module:** `mods/OnlyTanks/idle_cinematic`

---

## Features

### Cinematic Idle Camera
- Automatically starts after the configured idle delay.
- Smooth vehicle-focused cinematic camera shots.
- Continuous camera glide instead of stopping at the end of each shot.
- Camera movement continues through fade-to-black transitions.
- The next shot is already moving when it fades back in.
- Supports **5 to 15 shots per cycle**.
- Shot variety includes orbit, profile, rear-quarter, front-quarter, wide, close, crane, push/pull, and low cinematic compositions.
- Camera framing is kept far enough away to avoid excessive close-up zooming.

### Cinematic Tilt Shots
- Optional **Cinematic Tilt Shots** toggle.
- Tilt is used only by dedicated authored cinematic shots.
- Normal shots remain level.
- The director automatically mixes a small number of tilt compositions into each cycle instead of tilting every shot.

### Optional End-of-Shot Slowdown
- **Slow Down Near Shot End** can be enabled or disabled.
- OFF keeps the camera gliding continuously at normal speed.
- ON gently reduces motion near the end of a shot without bringing the camera to a full stop.

### HUD Control
- Optional **Hide HUD during Cinematic** support for the native Helldivers 2 HUD.
- Restores the exact previous HUD state when the cinematic ends.
- HUD state is not written every frame.

### Driver HUD 1.5.1 Compatibility
- Optional **Hide Driver HUD during Cinematic** toggle.
- Works with the original, unmodified **Driver HUD 1.5.1**.
- Driver HUD does not need to be patched, replaced, or bundled with this mod.
- Driver HUD telemetry continues running while its drawing is temporarily suppressed.
- The compatibility hook is active only while the cinematic is running and is removed afterward.

### Photo Mode
- Optional Photo Mode support.
- Photo Mode processing stays effectively dormant while disabled.

### Safe Cancellation
The cinematic is released immediately when the player resumes control.

The mod also keeps a lightweight driver-seat safety check active while the cinematic is running. If the driver seat or vehicle relationship is lost, camera and HUD state are restored safely.

### Configuration
- Full local configuration support.
- Optional in-game controls through CowboyBingus Mod Options Menu.
- Existing configuration values are preserved during updates.
- Missing settings are added automatically.
- Old configuration headers/schema markers migrate forward without resetting user settings.
- Existing comments and unknown/custom lines are preserved where possible.

### Clean Failure-Only Logging
Normal successful operation is intentionally silent.

A log is created only for protected failures such as:
- HUD failure
- camera failure
- config migration/write failure
- vehicle/runtime safety failure

Release logs avoid exposing reverse-engineering addresses, offsets, signatures, entity IDs, or other internal research details.

---

## Requirements

### Required

**Bingus Shared Loader**  
https://github.com/CowboyBingus/BingusSharedLoader/releases

Install the latest supported release separately from Idle Animation.

### Optional

**CowboyBingus Mod Options Menu**  
https://github.com/CowboyBingus/ModOptionsMenu

Mod Options Menu provides the in-game settings UI. Idle Animation can still use its local configuration file without it.

### Optional Compatibility

**Driver HUD 1.5.1**

Driver HUD is not bundled with Idle Animation. If installed, the **Hide Driver HUD during Cinematic** option can temporarily suppress its rendering during the cinematic while leaving its telemetry active.

Compatibility is specifically built around stock Driver HUD 1.5.1 behavior. Other Driver HUD versions are not guaranteed to expose the same integration path.

---

## Installation

1. Close Helldivers 2.
2. Install the latest supported **Bingus Shared Loader**.
3. Import `Idle-Animation-v1.0.0.zip` into Arsenal or HD2MM.
4. Enable Idle Animation.
5. Optional: install **Mod Options Menu** for in-game configuration.
6. Optional: keep your normal stock **Driver HUD 1.5.1** installation if you use it.
7. Deploy your enabled mods.
8. Launch the game.

With the default Arsenal priority behavior, Bingus Shared Loader should follow the loader placement guidance from its own release documentation.

The expected loader entry for this mod is:

```text
mods/OnlyTanks/idle_cinematic: loaded
```

---

## Recommended First Setup

For a cinematic-focused setup:

- **Shots Per Cycle:** 12-15
- **Cinematic Tilt Shots:** ON
- **Slow Down Near Shot End:** OFF
- **Hide HUD during Cinematic:** ON
- **Hide Driver HUD during Cinematic:** ON if using Driver HUD 1.5.1

This gives the most continuous camera flow while keeping the HUD out of the shot.

---

## Performance

Idle Animation is designed to stay lightweight, including on lower-end systems.

Only one cinematic shot is active at a time. Selecting 15 shots does **not** create or update 15 cameras simultaneously.

The active cinematic path performs one current-shot calculation and one camera pose update per frame.

The release avoids:
- broad repeated `game.dll` scans
- per-frame HUD writes
- per-frame Driver HUD discovery
- filesystem polling for compatibility
- repeated Driver HUD rebinding
- heavy diagnostic snapshots
- continuous success logging

Driver HUD compatibility is also performance-focused:
- compatibility is attached only when the cinematic needs it
- Driver HUD drawing is skipped while hidden
- telemetry can continue running
- the compatibility wrapper is removed when the cinematic ends

The remaining active safety checks are intentionally retained for fast cancellation and reliable camera/HUD restoration.

---

## v1.0.0 Highlights

- Production module identity: `mods/OnlyTanks/idle_cinematic`
- Continuous camera glide
- Flowing fade transitions with no stationary blackout pose
- 5-15 shots per cycle
- Dedicated cinematic tilt-shot system
- Optional end-of-shot slowdown
- Native HUD hide/restore
- Driver HUD 1.5.1 cinematic hud suppression (Toggleable)
- Photo Mode support
- Safe input/driver-seat cancellation
- Automatic config migration
- Failure-only sanitized logging
- Performance-focused release runtime

---

## Notes

This is a client-side cinematic presentation mod for Helldivers 2.
For game updates, keep the last known-good release available until the current Helldivers 2 build has been confirmed compatible.
