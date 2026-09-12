# Minimal Skeleton (Illustrative)

Annotated Odin snippets showing the shape of the basic game loop. **Illustrative only**—this is not a complete, drop-in project yet. Use it alongside [01-game-loop-plan.md](01-game-loop-plan.md) when you implement `main.odin`.

## Canonical empty loop

Window opens, clears each frame, exits on ESC or window close:

```odin
// illustrative only
package main

import rl "vendor:raylib"

main :: proc() {
    rl.InitWindow(800, 450, "odin-demo")
    defer rl.CloseWindow()
    rl.SetTargetFPS(60)

    for !rl.WindowShouldClose() {
        // input
        // update

        rl.BeginDrawing()
        rl.ClearBackground(rl.RAYWHITE)
        // draw
        rl.EndDrawing()
    }
}
```

### What each piece does

| Piece | Role |
| --- | --- |
| `InitWindow` | Creates the OS window and raylib context |
| `defer CloseWindow()` | Guarantees cleanup when `main` returns |
| `SetTargetFPS(60)` | Caps/pacing so the loop is not a busy-spin |
| `WindowShouldClose()` | True on close button or default quit key (ESC) |
| `BeginDrawing` / `EndDrawing` | Frame render bracket; swap/present happens at End |
| `ClearBackground` | Proof the draw path runs every frame |

## With input + dt update + draw

Slightly richer skeleton once the empty loop works—still illustrative:

```odin
// illustrative only
package main

import rl "vendor:raylib"

main :: proc() {
    rl.InitWindow(800, 450, "odin-demo")
    defer rl.CloseWindow()
    rl.SetTargetFPS(60)

    pos := rl.Vector2{100, 100}
    speed: f32 = 200 // pixels per second

    for !rl.WindowShouldClose() {
        // --- input ---
        move := rl.Vector2{}
        if rl.IsKeyDown(.RIGHT) do move.x += 1
        if rl.IsKeyDown(.LEFT)  do move.x -= 1
        if rl.IsKeyDown(.DOWN)  do move.y += 1
        if rl.IsKeyDown(.UP)    do move.y -= 1

        // --- update ---
        dt := rl.GetFrameTime()
        pos.x += move.x * speed * dt
        pos.y += move.y * speed * dt

        // --- render ---
        rl.BeginDrawing()
        rl.ClearBackground(rl.RAYWHITE)
        rl.DrawRectangleV(pos, {40, 40}, rl.SKYBLUE)
        rl.DrawText("Arrow keys to move", 10, 10, 20, rl.DARKGRAY)
        rl.EndDrawing()
    }
}
```

### Phase boundaries to keep

1. **Input** — read keys/mouse into values; avoid drawing here.
2. **Update** — apply `dt` and mutate state; avoid drawing here.
3. **Render** — only draw; avoid changing simulation state here.

## Suggested first milestone

Ship the **canonical empty loop** first. Confirm open / clear / close. Only then copy patterns from the second snippet.

## Not included here

- Project file layout beyond a single `main.odin`
- Resource loading or unloading
- Fixed timestep accumulator
- Separate packages or build configuration

Those belong after the loop in this file is real and running under `odin run .`.
