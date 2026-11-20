# LILLIPUP Driver Setup — releases

One-click Windows 11 driver and configuration installer for the **Lenovo IdeaPad Flex 5i Chromebook ("LILLIPUP")**
running **MrChromebox UEFI Full ROM** firmware.

> ⚠️ **Untested test build.** The installer has not yet been run on a real LILLIPUP. Only try it with a full
> backup / recovery image and external recovery media (the Windows Recovery screen has shown up black on this
> firmware). Please report what happens and attach `C:\ProgramData\LILLIPUP-Setup\Logs`.

Download `LILLIPUP_Driver_Setup.exe` from the [Releases](../../releases) page.

## What it does
1. Checks this is Windows 11 x64 on LILLIPUP hardware (it refuses to run on anything else).
2. Switches **Windows Test Mode** on and (optional) applies the **SignGuiPatcher** watermark patch, then asks for one restart.
3. After you sign in again it continues by itself and installs the known-working drivers in stages:
   Intel platform, then Chromebook EC/keyboard/touchpad/sensors, then USB-C (Type-C + Tiger Lake PMC mux), then audio
   (SOF 1.0.4.2, RT5682, RT1011). It checks which driver each device is actually using.
4. Sets lid close to **Hibernate** and installs the automatic **touchpad-off-in-tablet-mode** helper.
5. Shows a summary and the things to check by hand: speakers/headphones, USB-C, folding to tablet mode.

Running it again repairs only missing or failing parts. Extra switches: `--preflight-only`, `--repair`,
`--restore-watermark`, `--remove-trackpad-task`.

## Security — please read
- Three drivers (SOF audio, RT5682 codec control, USB-C mux) are **self-signed**, not Microsoft-signed, and need
  **Test Mode**. Test Mode lowers driver-signing protection for the **whole PC** and stays on.
- Setup trusts only the two approved LILLIPUP test certificates (by thumbprint).
- The watermark patch (SignGuiPatcher v2.0.0, third party, MIT) only **hides** the "Test Mode" text. It does not
  sign anything, is not Microsoft-supported, restarts Explorer, and Windows updates may undo it.
- The installer EXE is not code-signed, so Windows SmartScreen will warn about an unknown publisher.

## Verify your download
Compare the SHA-256 of `LILLIPUP_Driver_Setup.exe` with `SHA256SUMS.txt` in the release
(PowerShell: `Get-FileHash .\LILLIPUP_Driver_Setup.exe`). The installer also verifies its own embedded payload
before doing anything.

## Third-party components
The installer redistributes unmodified driver packages exported from a working LILLIPUP (Intel, CoolStar, Realtek,
BayHub, Dell, Microsoft and the custom LILLIPUP audio/USB-C drivers) with their original catalogs, plus
SignGuiPatcher (© Marek Wesołowski, MIT). See `THIRD_PARTY_DRIVERS.txt` and the licence files attached to the
release. All trademarks belong to their owners. This project is not affiliated with Lenovo, Google, Intel,
Microsoft or MrChromebox. Use at your own risk.
