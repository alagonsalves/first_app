# Beam Designer

## Purpose
A beam design tool for engineers and students. The user sets up a beam,
places loads on it, and picks a steel or lumber section. The app shows the
shear, moment, and deflection diagrams and checks whether the beam is
strong and stiff enough, updating instantly as anything changes.

## Users
A civil engineering student checking a homework beam, or an engineer
sizing a floor beam or deck joist.

## Inputs
- Support type: Simply supported or Cantilever (fixed at the left end).
- Span: 4 to 40 ft, set with a slider and a number box that stay in sync.
- Loads, listed in a panel where each can be edited or removed:
  - Point loads: magnitude (kips, downward positive) and position (ft).
    A new point load starts at midspan (at the free end of a cantilever).
    Each point load has editable magnitude and position fields.
  - Uniform loads: magnitude (kips/ft) and start and end positions (ft).
    Uniform-load magnitude, start, and end positions are separate editable
    numeric fields.
  - "Add point load" and "Add uniform load" buttons.
  - Point loads can also be dragged along the beam in the drawing,
    snapping to 0.25 ft.
  - Point-load positions are restricted to the beam span from 0 to L.
  - Uniform-load endpoints are clamped to 0 and L. If their start position
    is greater than their end position, swap the two positions.
  - Allow downward positive load magnitudes only. Reject negative point-load
    and uniform-load magnitudes.
- Material and section, from these tables:

  Steel W-shapes (E = 29,000 ksi, allowable bending stress Fb = 30 ksi):

  | Section | Ix (in^4) | Sx (in^3) | Weight (lb/ft) |
  |---------|-----------|-----------|----------------|
  | W8x10   | 30.8      | 7.81      | 10 |
  | W10x12  | 53.8      | 10.9      | 12 |
  | W12x26  | 204       | 33.4      | 26 |
  | W14x22  | 199       | 29.0      | 22 |
  | W16x26  | 301       | 38.4      | 26 |
  | W18x35  | 510       | 57.6      | 35 |
  | W21x44  | 843       | 81.6      | 44 |
  | W24x55  | 1350      | 114       | 55 |

  Douglas Fir-Larch No. 2 lumber (E = 1,600 ksi, Fb = 0.900 ksi):

  | Section | Ix (in^4) | Sx (in^3) |
  |---------|-----------|-----------|
  | 2x8     | 47.63     | 13.14     |
  | 2x10    | 98.93     | 21.39     |
  | 2x12    | 177.98    | 31.64     |

- Deflection limit: L/240 or L/360 (default L/360).
- Steel beam self-weight is optional and off by default. When enabled,
  include the selected steel section's tabulated weight as a downward
  uniform load across the full span.
- Lumber has no self-weight option. Switching to lumber turns the
  self-weight toggle off.
- The material control is a dropdown; the section control shows only the
  sections available for the selected material.
- Below the section dropdown, show a checkbox labeled "Include selected
  steel beam self-weight" and a derived weight label when steel is selected.
  The label uses kips/ft, for example "W12x26: 0.026 kips/ft".
- The toggle is disabled and hidden, or clearly unavailable, while lumber
  is selected.

## Calculations
- Reactions from statics. Upward reactions are positive. The cantilever is
  fixed at x = 0 and free at x = L. Report the fixed-end reaction moment
  as a positive magnitude while preserving the negative hogging moment in
  the moment diagram.
- Positive shear is upward reactions minus downward loads to the left of the
  section. Sagging moment is positive and hogging moment is negative.
- Shear V(x) and moment M(x) along the beam, sampled at 0.25 ft or finer,
  plus exactly at every support, load point, and load start/end. Include
  both sides of shear discontinuities.
- Calculate deflection y(x) exactly by integrating M/EI twice with the
  appropriate support boundary conditions: y = 0 at both supports for a
  simply supported beam, or y = 0 and slope = 0 at the fixed end for a
  cantilever. Downward deflection is positive.
- Use exact continuous extrema rather than sampled maxima. Evaluate exact
  zero-slope locations and endpoints for maximum deflection. Report
  |y max| over the whole beam. If multiple equal maxima occur, label the
  leftmost location.
- Bending stress fb = |M max| x 12 / Sx (ksi). Bending check passes if
  fb <= Fb.
- Deflection check passes if |y max| <= L/240 or L/360 (L in inches).
- When steel self-weight is enabled, add the selected section weight to
  the general load model as one uniform load over 0 to L. Convert lb/ft to
  kips/ft by dividing by 1000. Do not add self-weight to the editable
  uniform-load list or replace general reaction, shear, moment, or
  deflection calculations with full-span uniform-load formulas.
- The beam drawing and uniform-load display include the synthetic steel
  self-weight load only when enabled.
- Lightest passing section: of the sections in the chosen material, the
  lightest steel section that passes both checks (if two weigh the same,
  the one with the larger Ix), or the first lumber size in the table
  that passes.
  With steel self-weight enabled, evaluate every steel candidate using that
  candidate's own weight, Ix, and Sx. The candidate's weight and section
  properties must be applied together. Use the lightest candidate that
  passes both checks, with the same weight tie rule.
  The "Use lightest passing" button changes only the section selection,
  never the selected material. If no section passes, display a clear message.
- Units: kips, ft, and kip-ft on screen; inches for deflection.
  For calculations, use lengths in inches and loads in kips.
  Use the tabulated modulus of elasticity directly in kip/in^2: steel
  E = 29,000 ksi = 29,000 kip/in^2, and lumber E = 1,600
  ksi = 1,600 kip/in^2. Do not multiply E by 144.
- Display reactions, shear, moment, and locations to two decimal places.
  Display bending stress to two decimal places and deflection to three
  decimal places in inches. Keep full precision internally and use
  unrounded values for comparisons and PASS/FAIL decisions.
- Validation tolerances are 0.001 in for deflection, 0.01 ksi for bending
  stress, and 0.01 ft for locations. Flag any discrepancy between a
  calculated result and the provided validation cases rather than forcing
  it to match.

## Outputs
- A beam drawing with the supports, load arrows (point loads as single
  arrows, uniform loads as a row of small arrows), and the reactions.
- Three diagrams stacked under the beam and lined up with it on the same
  x scale: shear (kips), moment (kip-ft), and deflected shape (in). Each
  diagram is filled and shows its exact maximum value labeled at its
  location. The shear diagram may show negative and positive signed values.
- Moving the mouse over any diagram shows a vertical line across all three
  with the values of V, M, and y at that x.
- Two check cards: Bending and Deflection. Each shows the actual value, the
  allowable value, a bar, and PASS or FAIL. The bending ratio is actual
  bending stress divided by allowable bending stress. The deflection ratio
  is maximum absolute deflection divided by the selected allowable
  deflection (L/240 or L/360). Color the bar green below 70%, amber from
  70% through 100%, and red above 100%. Exactly 100% passes.
- A line suggesting the lightest passing section, with a button to use it.
- Everything updates immediately whenever any input changes.
- A "Print / Save PDF" button uses the browser native print dialog.
- Printing produces a clean US Letter portrait report containing:
  - report title and generated date
  - support type and span
  - deflection limit
  - material and section properties
  - point and uniform loads
  - steel self-weight status and derived kips/ft
  - support reactions
  - maximum shear, moment, and deflection with locations
  - beam, shear, moment, and deflection diagrams
  - bending and deflection checks
  - recommended lightest passing section
  - educational-use disclaimer
- Printed data must come from the current app inputs and unrounded
  calculation state. Interactive controls, hover readouts, buttons, and
  screen-only navigation must be hidden in the print report.
- The report must remain single-file and use no external dependencies.

## Look and feel
- Opens with an example already loaded (20 ft simply supported W12x26
  with a 1 kip/ft uniform load over the full span), so it is useful the
  moment it opens.
- Clean and professional, like a commercial engineering web app: light
  background, white cards with soft shadows, one accent color, a modern
  system font, generous spacing. No clutter.
- Inputs in a panel on the left; drawing, diagrams, and checks on the
  right. On a phone, the panels stack.
- Every input has a short label with units. Hovering over a check card
  explains the formula used.

## Validation
1. Simply supported, 20 ft, W12x26, uniform 1 kip/ft over the full span:
   reactions 10 k and 10 k; max shear 10 k; max moment 50.0 kip-ft at
   10 ft; fb = 17.96 ksi; max deflection 0.609 in at 10 ft; L/360 =
   0.667 in; both checks pass.
2. Simply supported, 20 ft, W12x26, point load 10 k at 5 ft: reactions
   7.5 k (left) and 2.5 k (right); max moment 37.5 kip-ft at 5 ft;
   fb = 13.47 ksi; max deflection 0.340 in at 8.82 ft.
   Verified with consistent kip-inch units. No discrepancy found.
3. Cantilever, 10 ft, W12x26, point load 5 k at the free end: reaction
   5 k and fixed-end moment 50 kip-ft; max deflection 0.487 in at the free
   end (PL^3/3EI); L/360 = 0.333 in, so deflection fails.
4. Simply supported 12 ft 2x10 joist, uniform 0.05 kip/ft: max moment
   0.90 kip-ft; fb = 0.505 ksi; max deflection 0.147 in; both pass.
5. Simply supported, 20 ft, W12x26, no external load, self-weight on:
   0.026 kips/ft self-weight; reactions 0.26 k each; max moment 1.30 kip-ft
   at 10 ft; fb = 0.467 ksi; max deflection 0.016 in.
6. Simply supported, 20 ft, W12x26, 1 kip/ft external uniform load over
   the full span, self-weight on: combined load 1.026 kips/ft; reactions
   10.26 k each; max moment 51.30 kip-ft at 10 ft; deflection and stress
   are recalculated from the combined loading: fb = 18.43 ksi and
   max deflection 0.624 in.
7. Cantilever, 10 ft, W12x26, no external load, self-weight on:
   0.026 kips/ft self-weight; reaction 0.26 k; fixed-end moment 1.30 kip-ft;
   deflection uses the general cantilever uniform-load integration and is
   0.009 in at the free end.
8. Section-change with self-weight on: 20 ft simply supported, no external
   load; W12x26 uses 0.026 kips/ft and W10x12 uses 0.012 kips/ft. Candidate
   checks and Ix/Sx properties must change with each section. W12x26 gives
   1.30 kip-ft moment, 0.467 ksi stress, and 0.016 in deflection; W10x12
   gives 0.60 kip-ft moment, 0.661 ksi stress, and 0.028 in deflection.
9. Printable report: Print Preview and Save-to-PDF tests for the default
   example, cantilever case, off-center point load, lumber case, steel
   self-weight on/off, and combined point/uniform loads. Verify every report
   section, disclaimer, diagram, and result matches the live app.
9. Steel-to-lumber switch: lumber selection turns self-weight off and excludes
   all steel self-weight. 12 ft simply supported 2x10 with 0.05 kip/ft
   external uniform load retains the original no-self-weight result.

## Out of scope
- Lumber self-weight, shear stress, lateral-torsional buckling, load
  combinations, and continuous beams.
- Saving designs.