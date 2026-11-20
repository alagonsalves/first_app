# Steel Beam Self-Weight Validation

The toggle is off by default. When enabled for steel, beam weight is
converted from lb/ft to kips/ft and combined with the existing external
point and uniform loads through the general reaction and integration engine.

| Case | Expected | Browser/engine result |
|---|---|---|
| W12x26, 20 ft simply supported, no external load, weight on | 0.026 k/ft; reactions 0.26 k each; moment 1.30 kip-ft; stress 0.467 ksi; deflection 0.016 in | 0.26 k / 0.26 k; 1.30 kip-ft; 0.467 ksi; 0.015818 in; PASS |
| W12x26, 20 ft simply supported, 1 k/ft external, weight on | 1.026 k/ft combined; reactions 10.26 k each; moment 51.30 kip-ft; stress 18.431 ksi; deflection 0.624 in | 10.26 k / 10.26 k; 51.30 kip-ft; 18.431 ksi; 0.624185 in; PASS |
| W12x26, 10 ft cantilever, no external load, weight on | reaction 0.26 k; fixed moment 1.30 kip-ft; deflection 0.009 in | 0.26 k; 1.30 kip-ft; 0.009493 in; PASS |
| W12x26/W10x12 section change, 20 ft, weight on | W12x26: 0.026 k/ft; W10x12: 0.012 k/ft; candidate properties and checks must use each section | W12x26: 1.30 kip-ft, 0.467 ksi, 0.015818 in. W10x12: 0.60 kip-ft, 0.661 ksi, 0.027682 in |
| Steel-to-lumber, 12 ft 2x10, 0.05 k/ft | steel weight excluded; original 0.147 in, 0.505 ksi result retained | 0.147275 in; 0.504909 ksi; PASS |

The original four validation cases are retained with `includeSelfWeight`
set to false, so they remain unchanged.

## Browser verification

- Reloaded the app with no JavaScript page errors.
- Confirmed the toggle is off in the default steel example.
- Confirmed W12x26 shows `0.026 kips/ft`.
- Confirmed the combined 1 kips/ft plus self-weight case shows 18.43 ksi
  bending stress and 0.624 in deflection.
- Confirmed cantilever self-weight shows a 0.26 k reaction, 1.30 kip-ft fixed
  moment, and 0.009 in maximum deflection.
- Confirmed switching to lumber turns off and disables the toggle.
- Confirmed switching back to steel restores the toggle to the off state.
