# Flycast 2021 Low-End

Flycast 2021 Low-End is an unofficial, performance-oriented libretro fork for
low-power ARM handhelds. It is based on
[`libretro/flycast` commit `4c293f3`](https://github.com/libretro/flycast/tree/4c293f306bc16a265c2d768af5d0cea138426054)
from 6 April 2022.

The fork keeps the original renderer as the default and adds explicitly
optional optimizations for devices such as the RK3326/Cortex-A35 family.
Accuracy-changing options remain disabled unless the user enables them.

For the emulator's original features, BIOS, game compatibility and general
configuration, refer to the
[original Flycast documentation](https://docs.libretro.com/library/flycast/)
and the
[upstream source at the base revision](https://github.com/libretro/flycast/tree/4c293f306bc16a265c2d768af5d0cea138426054).

## New core options

The option prefix remains `flycast2021` so existing Flycast 2021
configurations continue to work.

### Adjacent Render-State Elision

Key:

```text
flycast2021_adjacent_state_elision
```

Values:

- `disabled` (default): preserves the original renderer path.
- `enabled`: skips repeated GLES state setup when two adjacent polygons have
  an exact state match.

This mode is designed to preserve rendering semantics, but the measured
performance difference on the current RK3326 test scenes is small and can be
within normal run-to-run variance. It is retained as an experimental option
for broader per-game testing.

### Translucent Strip Merge

Key:

```text
flycast2021_translucent_strip_merge
```

Values:

- `disabled` (default): preserves the original translucent rendering order.
- `inaccurate`: reorders and merges compatible translucent strips to reduce
  GLES draw submissions.

This is the main low-end performance option. Tests on an AmberELEC RG351V
showed scene-dependent gains around 12–18%, but it can break transparency,
menus or PowerVR ordering. Enable it per game only after visual testing.

See [the compatibility notes](docs/LOW_END_COMPATIBILITY.md) for the current
device and game observations.

## Suggested low-end baseline

Start with the accurate path:

```ini
flycast2021_adjacent_state_elision = disabled
flycast2021_translucent_strip_merge = disabled
```

For a game already verified with the faster translucent path:

```ini
flycast2021_adjacent_state_elision = disabled
flycast2021_translucent_strip_merge = inaccurate
```

Do not make the inaccurate mode a global default. A setting that works well
for one Dreamcast title can damage another title's menus.

## Building the libretro core

Use the same toolchain and platform flags as the target distribution. A
typical ARM libretro build is:

```sh
make -C core -f Makefile platform=armv unix-armv7-hardfloat-neon
```

AmberELEC integration should install this fork as a separate core, for
example:

```text
flycast2021_lowend_libretro.so
```

This allows users to keep the distribution's standard Flycast/Flycast 2021
cores and select the experimental core per game.

## Project status

- Primary target: low-power OpenGL ES handhelds.
- First validated platform: AmberELEC on RK3326/Cortex-A35/Mali-G31.
- Renderer defaults: accurate/original.
- Inaccurate optimizations: opt-in and expected to need a per-game
  compatibility list.
- Save-state compatibility with other Flycast builds is not guaranteed.

Benchmark results are workload-specific. A higher rendered FPS result does not
by itself prove that audio, transparency, menus and save states remain correct.

## License and attribution

The project preserves the original history and is distributed under the
GPL-2.0 license in [LICENSE](LICENSE). Flycast, reicast and libretro
attributions remain with their respective authors.

Code contributed to this fork is not bound by the Individual Contributor
License Agreement of the upstream reicast repository and must not be treated
as an upstream contribution unless its author explicitly submits it there.
