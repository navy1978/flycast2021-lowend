# Low-End Compatibility Notes

These observations describe the experimental renderer options in Flycast 2021
Low-End. They are not a general Dreamcast compatibility list.

Test platform:

- Anbernic RG351V
- RK3326 / Cortex-A35
- Mali-G31 OpenGL ES
- AmberELEC
- RetroRun GO2 video and audio

## Current observations

| Game | Adjacent state elision | Translucent strip merge | Notes |
| --- | --- | --- | --- |
| Soul Calibur | No visible regression in tested scenes | Usable in the corrected-order build | Strong performance and working menus in the latest manual test. Re-test additional stages before treating it as fully compatible. |
| Dead or Alive 2 | No visible regression in tested scenes | Usable in tested USA image | No obvious transparency or menu regression was reported. Audio quality depends on the frontend backend and is not controlled by this core option. |
| Sonic Adventure 2 | Controls and gameplay work | Not recommended | The faster path leaves menu text readable but can render menu backgrounds/elements incorrectly. |
| Crazy Taxi | No visible regression in the tested run | Not yet classified | Gameplay and audio were reported as good, but the inaccurate strip merge path still needs an isolated A/B visual test. |
| Power Stone | No visible regression in the tested run | Not yet classified | Requires an isolated A/B visual test before enabling the inaccurate path. |

## Reporting a result

Record all of the following:

1. device and operating system;
2. game region and image format;
3. internal resolution;
4. alpha sorting mode;
5. both low-end option values;
6. whether menus, transparency, shadows and render-to-texture effects work;
7. measured emulation speed and rendered FPS when available.

Short title screens are not sufficient. Test gameplay, menus and at least one
effect-heavy scene.
