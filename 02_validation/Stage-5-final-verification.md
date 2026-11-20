# Stage 5 Final Verification

## Final browser checks

- No JavaScript page errors after reload.
- Default example initializes with a 20 ft span, simply-supported setup,
  full-span 1 kip/ft uniform load, W12x26 steel section, and L/360 limit.
- Beam and all three diagrams are present and aligned on the same x-scale.
- Bending and deflection cards calculate and display PASS/FAIL values.
- Synchronized hover lines display V, M, and y.
- Span controls remain synchronized.
- Point-load drag snapping is set to 0.25 ft.
- Negative load magnitudes are rejected.
- Reversed uniform intervals are normalized.
- Use-lightest-passing updates the section only.
- The 390 px viewport stacks the layout without horizontal overflow.

## Validation summary

All four specification validation cases are within the stated tolerances.
No remaining specification discrepancies were found.

## Deliverable

- Default browser screenshot:
  [beam-designer-default.png](../03_output/beam-designer-default.png)
