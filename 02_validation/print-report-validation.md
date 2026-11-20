# Print / Save PDF Report Validation

## Implementation

The report uses the browser native `window.print()` dialog and a print-only
US Letter portrait layout. It contains no external dependencies.

## Print validation matrix

| Case | Required verification |
|---|---|
| Default simply supported example | 20 ft span, W12x26, 1 k/ft uniform load, 10 k reactions, 50 kip-ft moment, 17.96 ksi, 0.608 in, and PASS checks |
| Cantilever 5 k at 10 ft | Fixed support, 5 k reaction, 50 kip-ft fixed moment, 0.487 in deflection, and deflection FAIL |
| Off-center 10 k point load | 7.5 k / 2.5 k reactions, 37.5 kip-ft moment, 13.47 ksi, and 0.340 in at 8.82 ft |
| Lumber 2x10 | Lumber properties, 0.505 ksi, 0.147 in, and PASS checks |
| Steel self-weight on | Selected section weight, derived kips/ft, synthetic full-span load, combined reactions and results |
| Steel self-weight off | No synthetic load, exact original toggle-off results |
| Combined point and uniform loads | Editable load table and diagrams match the live app |
| 40 ft span | Full-span inputs, reactions, diagrams, and labels remain within Letter portrait margins |
| No passing section | Report shows “No passing section” for the recommendation |

## Print layout checks

- Interactive inputs, buttons, hover readouts, and navigation are absent.
- The report uses US Letter portrait with 0.45-inch margins.
- Beam and diagram SVGs remain aligned and readable.
- Page sections avoid splitting where practical.
- SVG fills and PASS/FAIL colors are preserved in print preview.
- The educational-use disclaimer appears.
- Generated date reflects the print action.
- Save-as-PDF and Chrome Print Preview can create the report without
  external libraries or network assets.

## Browser and calculation verification

- Reloaded the app with no JavaScript page errors.
- The Print / Save PDF button populated the report and invoked the native
  `window.print()` entry point.
- Print-media emulation hid the app header, input panel, results screen,
  hover readout, and print button.
- Print-media emulation showed the report with one beam SVG and four
  report diagram SVG elements in total.
- Verified the default report contains W12x26, 20 ft span, 10 k reactions,
  50 kip-ft moment, 17.96 ksi bending stress, 0.608 in deflection, and PASS.
- Verified cantilever report content for 5 k at 10 ft, including the fixed
  reaction, 50 kip-ft fixed moment, 0.487 in deflection, and deflection FAIL.
- Verified off-center point-load report values of 7.5 k / 2.5 k reactions,
  37.5 kip-ft moment, 13.47 ksi, and 0.340 in at 8.82 ft.
- Verified lumber report material/section status and self-weight lockout.
- Verified steel self-weight on/off status and synthetic load text.
- Verified a 40 ft span report and a no-passing-section report.
- Re-ran all nine existing calculation validation cases. All results matched
  the expected values and PASS/FAIL status.

## Remaining automation limitation

This browser integration exposes print-media emulation but could not invoke
Chromium's `Page.printToPDF` protocol. Chrome Print Preview and the OS/PDF
destination remain the native browser validation path.
