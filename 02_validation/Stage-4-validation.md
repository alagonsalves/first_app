# Stage 4 Validation

## Validation cases

| Case | Reactions | Shear / Moment | Bending Stress | Deflection | Result |
|---|---|---|---|---|---|
| 20 ft simply supported, W12x26, 1 k/ft full span | 10.00 k / 10.00 k | 10.00 k / 50.00 kip-ft | 17.964 ksi | 0.608 in at 10.00 ft | PASS |
| 20 ft simply supported, W12x26, 10 k at 5 ft | 7.50 k / 2.50 k | 7.50 k / 37.50 kip-ft | 13.473 ksi | 0.340 in at 8.82 ft | PASS |
| 10 ft cantilever, W12x26, 5 k at free end | 5.00 k, 50.00 kip-ft fixed moment | 5.00 k / 50.00 kip-ft | 17.964 ksi | 0.487 in at 10.00 ft; limit 0.333 in | Bending PASS, Deflection FAIL |
| 12 ft simply supported, 2x10, 0.05 k/ft | 0.30 k / 0.30 k | 0.30 k / 0.90 kip-ft | 0.505 ksi | 0.147 in at 6.00 ft | PASS |

All calculated values are within the specified validation tolerances.

## Browser checks

- No JavaScript page errors were found after refresh.
- The default uniform load and W12x26 section initialize correctly.
- The beam, shear, moment, and deflection SVG drawings render and align.
- Cantilever and simply-supported configurations update calculations and diagrams.
- Negative load magnitudes are reset to zero.
- Reversed uniform-load endpoints are normalized by swapping start and end.
- The span slider and numeric span remain synchronized.
- Point-load dragging was verified to snap to 0.25-ft increments.
- Synchronized hover readouts display position, shear, moment, and deflection.
- The layout stacks for a phone-width viewport.
