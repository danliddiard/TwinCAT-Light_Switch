# TwinCAT Light Switch

My first TwinCAT 3 program on real Beckhoff EtherCAT hardware. A push button on an EL1008 input turns on a light on an EL2008 output.

```iecst
PROGRAM MAIN
VAR
    Switch AT %I* : BOOL;
    LIGHT  AT %Q* : BOOL;
END_VAR

LIGHT := Switch;
```

Small on purpose. The goal was to prove the full path end to end: engineering, I/O linking, licensing, runtime, and Git version control.

## Branches

| Branch | Runs on | TwinCAT | Platform |
|---|---|---|---|
| `main` | Dell laptop as the EtherCAT master | 3.1.4026.27 | Windows x64 |
| `Target-4024` | Beckhoff CP6606 panel PC (Windows CE 7) | 3.1.4024.79 | TwinCAT CE7 (ARMV7) |

The CP6606 can't run 4026, so the panel version lives on its own branch. Keep the two separate. Don't merge them.

## Hardware

- EK1100 EtherCAT coupler
- EL1008 8-channel digital input (button on channel 1)
- EL2008 8-channel digital output (light on channel 1)
- EL7342 2-channel DC motor terminal (not used here)
- EL9011 end cap

## I/O Map

| Variable | Terminal | Channel |
|---|---|---|
| `MAIN.Switch` | EL1008 | 1 |
| `MAIN.LIGHT` | EL2008 | 1 |

## Running it

1. Open the `.sln` in TcXaeShell with the TwinCAT version for your branch selected *before* opening the project.
2. Check the EtherCAT adapter under I/O > Devices > Device 1 > Adapter.
   - Laptop: the Intel port with the TwinCAT real-time driver
   - CP6606: CCAT DPRAM Slot 0 (the port labeled EtherCAT, X103)
3. Activate Configuration, then PLC > Login > Start.
4. A 7-day trial license works for testing.

## Lessons learned

- EtherCAT cable goes from the master into the EK1100's **top port (X1, IN)**.
- If frames go out and little or nothing comes back, check for a bad cable/intermittent connection before the settings.
- An old CE panel with its clock reset makes a new trial license look like it's from the future. Fix the clock first.
- Changing TwinCAT versions or rescanning I/O drops variable links, so relink afterward.

Full notes and daily logs: [twincat-damper-trainer](https://github.com/danliddiard/twincat-damper-trainer)

## Author

Daniel Liddiard, Davis Technical College, TECE 1420 (Beckhoff / TwinCAT independent study)
