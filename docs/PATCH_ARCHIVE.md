# Flycast 2021 experiment archive

`patches/flycast2021-experiments-2026-07-23.zip` preserves the 27 Flycast and
Flycast 2021 patches produced during the RK3326 optimization work. The archive
also contains `PATCHES-SHA256.txt`, with a SHA-256 digest for every patch.

Archive SHA-256:

```text
2a0c8ef2515b251c5561d5cf5088ffcfd049e236da3c1b29ddcb9743a6a905fc
```

## Integrated in this fork

The implementation at commit `aaf6d44c` contains the complete result of
`flycast2021-configurable-render-options.patch`:

- `flycast2021_adjacent_state_elision`, disabled by default;
- `flycast2021_translucent_strip_merge`, disabled by default, with the
  explicitly inaccurate mode selected by the `inaccurate` value;
- exact-state checks including the second texture ID;
- the corrected original translucent depth order before index generation;
- Flycast 2021 option prefix, RGB565 low-end compatibility and core identity.

The configurable patch therefore supersedes these earlier standalone patches:

- `flycast2021-adjacent-state-elision-experimental.patch`;
- `flycast2021-stripmerge-orderfix-experimental.patch`;
- `flycast2021-amberelec-lowend-compat.patch`;
- the earlier translucent merge/batching variants.

The only source difference between applying the configurable patch to base
revision `4c293f30` and the current implementation is the explanatory comment
added by commit `009effe7`; the functional source is identical.

## Preserved as experimental evidence

The remaining patches document profiler instrumentation and candidates tested
during development, including lazy uniforms, GL statistics, state caches,
buffer streaming, AICA/audio experiments, primitive restart and earlier
translucent batching strategies. They are archived for reproducibility and
future investigation, but are not active in the shipping source.

Several candidates measured neutral or worse performance, while some
translucent variants produced visual regressions. Their presence in the
archive does not imply that they should be applied together or enabled by
default.

## Compatibility audit

The fork is based directly on `4c293f30`. None of the accepted renderer
changes modifies `core/serialize.cpp`, the serialization version, AICA state,
Maple state or SH4 state. Commit `009effe7` keeps the historical
`Flycast 2021` libretro identity used by frontends for core-specific save-state
handling.

The two low-end renderer options preserve the original path when disabled.
Save-state compatibility still requires an on-device regression test before a
release, because frontend naming and runtime state cannot be proven solely by
source comparison.

## Build verification

Commit `009effe7` was rebuilt on the ARM64 Ubuntu build VM with the established
Cortex-A35 flags:

```text
-mcpu=cortex-a35+crc+fp+simd+crypto -mabi=lp64
-mno-outline-atomics -fomit-frame-pointer -Ofast
```

The build completed successfully and produced an AArch64 shared object
containing both option keys and the `Flycast 2021` identity:

```text
29199c2456f90b94d2541a86d8b85a138cf3e595dc971903be86db8e964cdd84
```
