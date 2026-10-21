# CanvasWM — Denim fork roadmap

This fork explores a practical macOS infinite-canvas window manager inspired by DriftWM while keeping the implementation native and reversible.

## Baseline

- Keep `main` close to upstream `annieversary/canvaswm`.
- Do active development on `denim-dev`.
- Prefer public macOS APIs first (`ApplicationServices`, Accessibility, Cocoa).
- Avoid SIP changes and private WindowServer/SkyLight APIs unless a future feature truly requires them.
- Keep every experimental step easy to disable or revert.

## Phase 1 — make the prototype safe to test

- Split input handling, canvas state, and AX window operations out of the single-file prototype.
- Add structured logging and a clean shutdown path.
- Preserve original window frames so quitting can restore the desktop.
- Add basic guards for windows that cannot be moved or resized.
- Document permissions and launch/stop behavior.

## Phase 2 — trackpad-first navigation

Target interaction:

- two-finger pan: move the viewport over the canvas;
- pinch: zoom the canvas around the gesture focal point;
- normal clicks and scrolling continue to go to the app under the pointer;
- optional modifier-based fallbacks remain available for debugging.

The first implementation may still use AX window moves/resizes rather than compositor-level transforms. That limitation must remain explicit.

## Phase 3 — coherent world coordinates

- Store every managed window in canvas/world coordinates independent of its current visible frame.
- Maintain a viewport transform (`cameraX`, `cameraY`, `scale`).
- Derive actual macOS window frames from world coordinates instead of repeatedly treating screen coordinates as canonical state.
- Make pan and zoom reversible without accumulating resize/position drift.
- Handle partially visible and off-screen windows without losing their logical position.

## Phase 4 — usability

- Exclude system, transient, modal, PiP, and explicitly ignored windows.
- Add reset-to-100% and home/overview actions.
- Add optional bookmarks / named canvas positions.
- Persist layout and viewport state.
- Improve multi-monitor behavior.

## Explicit non-goals for now

- Replacing WindowServer.
- Running a Wayland compositor on macOS.
- Disabling SIP.
- Requiring virtual displays.
- Building a tiling window manager.

## First milestone

A usable proof of concept where several ordinary macOS windows can be placed spatially, the user can pan around them and zoom with a Mac trackpad, and quitting CanvasWM restores sane native window frames.
