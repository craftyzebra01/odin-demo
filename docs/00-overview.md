# Overview: Basic Odin Game Loop

## Purpose

This document set outlines how to set up the **most basic useful game loop** in [Odin](https://odin-lang.org/) for this `odin-demo` project: open a window, keep running while the window is open, process input, update game state, draw a frame, then shut down cleanly.

These files are a **plan and reference**, not a finished game. Source code comes later; the goal here is a clear path from zero to a minimal looping window.

## Scope

**In scope**

- Windowed application using [raylib](https://www.raylib.com/) via Odin’s vendor package
- Init → input → update → render → quit loop
- Frame timing with `SetTargetFPS` and optional `GetFrameTime()`
- Clean shutdown (`CloseWindow`, preferably via `defer`)

**Out of scope (for now)**

- Assets, scenes, ECS, physics
- Audio beyond “not yet”
- Fixed timestep / accumulator loops (noted as a later upgrade only)
- Build scripts, CI, packaging
- SDL2 or GLFW stacks (valid alternatives later; not planned here)

## Prerequisites

1. **Odin toolchain** installed and available on your `PATH` (`odin version` succeeds).
2. **raylib vendor package** shipped with the Odin distribution (`core:vendor/raylib`, or the equivalent vendor path for your install). No separate game engine is required.
3. A platform that raylib supports for desktop windows (the usual local-dev targets: Windows, macOS, Linux).

When you implement the skeleton, the expected one-line run is:

```bash
odin run .
```

(from the package directory that contains `main.odin`). Exact package layout is covered in [01-game-loop-plan.md](01-game-loop-plan.md).

## Definition of done

The basic loop is complete when all of the following are true:

1. A window opens with a title (e.g. `odin-demo`).
2. Each frame clears the background to a solid color (proof that draw is running).
3. The loop exits when the user presses ESC or closes the window (raylib’s default `WindowShouldClose` behavior).
4. The process exits without leaking the window (cleanup via `CloseWindow`).

Anything beyond that—sprites, movement, menus—is a follow-on step after the loop exists.

## Document map

| Doc | Contents |
| --- | --- |
| [00-overview.md](00-overview.md) | Purpose, scope, prerequisites, done criteria |
| [01-game-loop-plan.md](01-game-loop-plan.md) | Package layout, phases, timing, implementation checklist |
| [02-minimal-skeleton.md](02-minimal-skeleton.md) | Annotated illustrative Odin snippets |

## Backend choice

**Default: raylib.** One API covers windowing, input queries, drawing, and frame pacing—ideal for a first loop.

SDL2 and GLFW remain reasonable later if you need lower-level control or a different rendering path. This plan does not cover those stacks.
