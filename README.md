# EarthBound Movement Patches

BPS patches that change normal overworld walking speeds in EarthBound (USA).

## Patches

These patches target the headerless EarthBound (USA) ROM with CRC32 `DC9BB451`.

| Variant | Cardinal Movement | Diagonal Movement | BPS Patch |
|---|:---:|:---:|:---:|
| 1 px cardinal / 1,1 diagonal | 1 px/frame | 1 px/frame per axis | [bps](EarthBound%20%28USA%29%20%281px%20Cardinal%201x1%20Diagonal%29.bps) |
| 1.5 px cardinal / 1,1 diagonal | 1.5 px/frame | 1 px/frame per axis | [bps](EarthBound%20%28USA%29%20%281.5px%20Cardinal%201x1%20Diagonal%29.bps) |
| 2 px cardinal / 1,1 diagonal | 2 px/frame | 1 px/frame per axis | [bps](EarthBound%20%28USA%29%20%282px%20Cardinal%201x1%20Diagonal%29.bps) |
| 2 px cardinal / 2,2 diagonal | 2 px/frame | 2 px/frame per axis | [bps](EarthBound%20%28USA%29%20%282px%20Cardinal%202x2%20Diagonal%29.bps) |

Diagonal values are listed per axis. For example, `1,1` moves 1 pixel horizontally and 1 pixel vertically during each diagonal frame.

### Cardinal Movement Cadence

| Speed | Pixel Cadence |
|---|:---:|
| Original 1.375 px/frame | `1,1,2,1,1,2,1,2` |
| 1 px/frame | `1,1,1,1` |
| 1.5 px/frame | `1,2,1,2` |
| 2 px/frame | `2,2,2,2` |

The original cadence shown above starts from an integer-aligned position; its starting point can vary with the stored fractional position. The 1.5 px variant has a more regular cadence than the original, while the 1 px and 2 px variants move by a constant whole-pixel amount each frame.

Of the included variants, **1.5 px cardinal / 1,1 diagonal** is the closest overall speed compromise to the original game. Its cardinal speed is **9.09% faster** than the original 1.375 px/frame rate. Its diagonal resultant speed is approximately **1.414 px/frame**, which is **2.85% faster than the original diagonal speed** and **5.72% slower than its patched cardinal speed**.

Stuttering caused by the game itself delaying or missing frames is separate from the movement cadence. Reducing that type of stutter may still require an emulator-side speed hack or CPU overclock option.

Changing movement speed also changes the visible distance between the party leader and following party members. Faster variants generally cause followers to trail farther behind, while slower variants keep them closer.

## Technical Details

* **Movement Speed Tables:**
  EarthBound stores cardinal and diagonal overworld movement speeds as 16.16 fixed-point values. These patches replace only the normal-walking entry (entry 0) in each table.

* **ROM File Offsets:**

  | Movement | Entry 0 |
  |---|:---:|
  | Cardinal | `$03E0BC` |
  | Diagonal | `$03E0F4` |

* **Original Values:**
  The original cardinal entry contains **`$00016000`**, which is exactly **1.375 pixels per frame**. The original diagonal entry contains **`$0000F8E6`**, which is approximately **0.97226 pixels per frame on each axis**, or approximately **1.37498 pixels per frame overall**.

* **Replacement Values:**

  | Variant | Cardinal Value | Diagonal Value |
  |---|:---:|:---:|
  | 1 px cardinal / 1,1 diagonal | `$00010000` | `$00010000` |
  | 1.5 px cardinal / 1,1 diagonal | `$00018000` | `$00010000` |
  | 2 px cardinal / 1,1 diagonal | `$00020000` | `$00010000` |
  | 2 px cardinal / 2,2 diagonal | `$00020000` | `$00020000` |

* **Patch Scope:**
  The ROM produced by each patch differs from the clean ROM only within the two normal-walking movement values listed above. Bicycle speed, collision code, map data, camera-pan code, scripted-movement data, and other ROM data are not modified. Depending on the replacement values, 4 or 5 individual bytes differ because some bytes in the four-byte values already match the original.
