# LM386 Audio Amplifier — Mono Channel, 0.25W Speaker

**IC:** LM386 | **Speaker:** 8Ω, 0.25W | **Supply:** 9V DC | **Status:** Breadboard prototype done— soldering to perfboard pending

A breadboard prototype of a variable-gain audio amplifier built around the LM386. Input is sourced from a repurposed stereo earphone converted to mono. Gain was tuned experimentally from ×50 to ×200, with output capacitance adjusted to correct bass-boost distortion.

---

## Circuit overview

The reference schematic (×50 gain configuration) is in `hardware/schematic-x50-reference.png`.

| Parameter | Value |
|-----------|-------|
| IC | LM386N |
| Supply voltage | 5V (USB) during testing |
| Gain | ×200 (final, pins 1–8 bridged via C2, R2 removed) |
| Output coupling cap (C1) | 20µF (2× 10µF in parallel) |
| Output resistor (R1) | 10Ω |
| Zobel cap (C3) | 0.1µF |
| Bypass cap (C2) | 10µF |
| Volume pot | 100kΩ (substituted for schematic's 10kΩ) |
| Speaker | 8Ω, 0.25W |

---

## Input — earphone to mono conversion

The audio source is a standard 3.5mm stereo earphone repurposed as a mono input channel.

**Process:**
1. Cut the earphone cable and exposed the three wires — blue (left channel), green (right channel), yellow (ground/sleeve)
2. Burned off the enamel insulation on blue and green wires using a lighter
3. Added 1kΩ resistors on each channel (blue and green) before combining — this prevents the left and right channels from directly shorting each other
4. Soldered the combined signal wire to the left (input) terminal of the 100kΩ pot
5. Soldered the yellow ground wire to the right terminal of the pot
6. Middle wiper pin of pot goes to Pin 3 (non-inverting input) of LM386

The 1kΩ summing resistors also act as a basic voltage divider with the pot, giving a slight signal attenuation that helps avoid clipping at high pot positions.

---

## Design iterations

### Iteration 1 — ×50 gain (reference circuit)
**Configuration:** R2 = 1.2kΩ between pins 1 and 8, C2 = 10µF, C1 = 220µF  
**Observation:** Sound was audible but very low volume through the 0.25W speaker. The ×50 gain was insufficient for the signal level from the earphone source.

### Iteration 2 — ×200 gain (removed R2)
**Change:** Removed R2 (1.2kΩ). With only C2 (10µF) bridging pins 1 and 8, gain increases to ×200.  
**Observation:** Noticeably louder. However, bass frequencies were heavily boosted — sound was muddy and boomy. Root cause: the 220µF output coupling cap (C1) has a very low cutoff frequency (~90Hz with 8Ω load), letting through excessive low-frequency content.

Lower cutoff frequency formula:
```
f_c = 1 / (2π × R × C)
f_c = 1 / (2π × 8 × 220µF) ≈ 90 Hz
```

### Iteration 3 — Reduced C1 to 20µF (current version)
**Change:** Replaced 220µF C1 with 20µF (two 10µF electrolytic caps in parallel — 47µF or 100µF not available).  
**New cutoff frequency:**
```
f_c = 1 / (2π × 8 × 20µF) ≈ 995 Hz
```
**Observation:** Bass boost significantly reduced. Sound is cleaner and more balanced. Some muddiness remains — likely due to the cutoff being too high now (cutting into lower midrange). A 47µF or 100µF cap would give ~420Hz or ~200Hz cutoff, which is the target range.

### Summary table

| Iteration | R2 | C1 | Gain | Result |
|-----------|----|----|------|--------|
| 1 | 1.2kΩ | 220µF | ×50 | Too quiet |
| 2 | Removed | 220µF | ×200 | Loud but bass-heavy |
| 3 | Removed | 20µF (2×10µF ∥) | ×200 | Cleaner — minor muddiness remains |

---

## What's next

- [ ] Replace 2×10µF parallel with a single 47µF cap → lower the cutoff to ~420Hz for better bass balance
- [ ] Solder final circuit onto perfboard
- [ ] Measure output voltage with multimeter (RMS across speaker at moderate volume)
- [ ] Add a small heatsink pad or check IC temperature at sustained volume
- [ ] Try decoupling the 5V supply with a 100µF bulk cap + 0.1µF ceramic closer to VCC pin

---

## Key learnings

- The output coupling capacitor directly controls the low-frequency cutoff. Larger cap = more bass. At 8Ω load impedance, even 220µF barely reaches 90Hz.
- LM386 gain is set by what's between pins 1 and 8: nothing = ×20, capacitor only = ×200, resistor + cap = anything in between.
- Earphone wire enamel must be fully removed before soldering — partial removal causes intermittent contact. Burning with a lighter followed by light sanding works well.
- Breadboarding first saved time: all three iterations were tested in under an hour before committing to solder.

---

## Files

```
lm386-audio-amplifier/
├── README.md
├── hardware/
│   └── schematic-x50-reference.png   ← reference circuit (base design)
├── docs/
│   ├── photos/                        ← add breadboard photos here
│   └── iterations/                    ← add oscilloscope/audio screenshots here
└── simulation/                        ← LTspice .asc file (to be added)
```

---

## References

- [LM386 Datasheet — Texas Instruments](https://www.ti.com/lit/ds/symlink/lm386.pdf)
- Reference schematic source: ElecCircuit.com (×50 gain configuration)
