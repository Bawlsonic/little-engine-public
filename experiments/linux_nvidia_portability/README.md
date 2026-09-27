# LE-NVIDIA-001 - Linux / NVIDIA Portability Test

**Date:** 2026-09-21  
**Status:** COMPLETED  
**Environment:** Local Linux workstation  
**Protocol:** Little Engine Operating Protocol

## Objective

Evaluate whether the Little Engine verified-state and minimum-dependency workflow operates correctly during a local Qwen inference test on an NVIDIA/Linux environment while hardware telemetry is observed externally.

This experiment does not attempt to establish performance superiority, token savings, or general thermal characteristics.

## Environment

- GPU: NVIDIA GeForce RTX 3090
- VRAM: 24 GB
- CPU: Intel Core i9-13900K
- System RAM: 64 GB
- NVIDIA Driver: 595.91.07
- CUDA reported by NVIDIA-SMI: 13.2
- Ollama: 0.34.2
- Model: qwen3:30b-a3b
- Model architecture: qwen3moe
- Quantization: Q4_K_M
- Runtime context: 32768
- Runtime processor: GPU
- Interface: Open WebUI
- Telemetry: NVIDIA-SMI / Mission Center
- Telemetry role: External read-only observation

## Protocol State

The saved Little Engine operating protocol was supplied to Qwen and activated for the test session.

Qwen confirmed protocol activation and subsequently accepted the supplied environment as:

`STATE_0 = VERIFIED`

Telemetry was explicitly defined as external to the Little Engine control loop.

## Phase A - Dependency Gate

Qwen was asked to determine whether GPU thermal behavior remained stable during a 60-second inference workload using only STATE_0.

The available state contained no workload thermal telemetry.

Qwen responded:

`STATE_0 = INSUFFICIENT`

and opened a telemetry dependency.

The initial dependency request contained multiple telemetry dimensions:

- GPU temperature
- fan speed
- power consumption

The protocol's minimum-dependency rule was then reapplied.

Qwen narrowed the OPEN DEPENDENCY to:

`GPU temperature readings during 60-second inference workload`

STATE_0 remained unchanged.

No thermal conclusion was produced.

## Phase B - Requested Dependency Delta

A 60-second external telemetry capture was performed during the inference workload.

GPU temperature was sampled at 5-second intervals.

Recorded temperature samples:

`57, 57, 57, 57, 57, 57, 57, 57, 57, 57, 57, 57 Ã‚Â°C`

12 samples were recorded across the measurement window.

Fan telemetry was also observed externally as 0% during the captured window but was intentionally withheld from Qwen because fan telemetry was not part of the final requested dependency.

Only the requested GPU temperature delta was supplied to Qwen.

## Result

After receiving the requested dependency delta, Qwen responded:

`STATE_0 = UPDATED`

Qwen determined that GPU thermal behavior remained stable during the measured 60-second window based on the supplied temperature readings.

Qwen then returned:

`DEPENDENCY CLOSED`

## State Transition

```text
STATE_0 VERIFIED
        |
        v
REQUEST
        |
        v
INSUFFICIENT
        |
        v
STOP
        |
        v
OPEN DEPENDENCY
        |
        v
MINIMUM DEPENDENCY
GPU TEMPERATURE
        |
        v
EXTERNAL MEASUREMENT
12 x 5-second samples
        |
        v
REQUESTED DELTA
        |
        v
STATE_0 UPDATED
        |
        v
DEPENDENCY CLOSED
` 

```

## Evidence Boundary

### Observed

- Little Engine protocol was activated in the local Qwen session.
- STATE_0 was established before the test.
- Qwen stopped when required thermal telemetry was absent.
- Qwen initially requested multiple telemetry dimensions.
- Reapplication of the minimum-dependency rule reduced the request to GPU temperature only.
- STATE_0 did not advance while the dependency remained open.

- External telemetry recorded 12 temperature samples during the 60-second measurement window.
- All 12 samples were 57Ã‚Â°C.
- NVIDIA-SMI reported fan telemetry as 0% during the captured window.
- Fan telemetry was not supplied to Qwen.
- Qwen accepted the requested temperature delta.
- Qwen produced an updated state.
- Qwen marked the dependency closed.

### Supported

This test supports that, in this NVIDIA/Linux Qwen session, the Little Engine protocol produced the intended sequence:

verified state -> insufficiency -> STOP -> minimum dependency -> requested delta -> resume -> updated state -> dependency closed

The experiment also demonstrates that observed telemetry outside the requested dependency can remain outside the model interaction.

### Not Established

This experiment does NOT establish:

- token savings
- reduced inference cost
- reduced processing time
- performance superiority
- general GPU thermal stability
- thermal behavior outside the measured 60-second window
- physical GPU fan state from NVIDIA-SMI telemetry
- behavior across all NVIDIA hardware
- behavior across all Linux environments
- behavior across all models
- patentability or novelty

Those require separate evidence.

## Experiment Status

**LE-NVIDIA-001: COMPLETE**

This checkpoint should remain frozen as recorded. Future experiments should receive separate experiment identifiers rather than modifying the conclusions of this test.
