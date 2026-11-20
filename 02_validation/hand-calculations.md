# Hand Calculations

## 1. Simply supported uniform load

- Span: L = 20 ft = 240 in
- Uniform load: w = 1 kip/ft = 1/12 kip/in
- Reactions: R1 = R2 = wL/2 = 10 kip
- Maximum moment: Mmax = wL^2/8 = 50 kip-ft
- Maximum bending stress for W12x26:
  fb = Mmax × 12 / Sx = 50 × 12 / 33.4 = 17.96 ksi
- Maximum deflection:
  ymax = 5wL^4/(384EI) = 0.6085 in
- Allowable deflection: 240 in/360 = 0.667 in

## 2. Simply supported off-center point load

- P = 10 kip
- a = 5 ft, b = 15 ft
- R1 = Pb/L = 7.5 kip
- R2 = Pa/L = 2.5 kip
- Mmax = R1a = 37.5 kip-ft
- fb = 37.5 × 12/33.4 = 13.47 ksi
- Maximum deflection location: x = 8.82 ft
- Maximum deflection: 0.340 in

## 3. Cantilever point load

- P = 5 kip
- L = 10 ft = 120 in
- Fixed-end moment: M = PL = 50 kip-ft
- ymax = PL^3/(3EI) = 0.487 in
- Allowable deflection: 120/360 = 0.333 in
- Deflection check fails.

## 4. Simply supported lumber uniform load

- L = 12 ft = 144 in
- w = 0.05 kip/ft = 0.05/12 kip/in
- Mmax = wL^2/8 = 0.90 kip-ft
- fb = 0.90 × 12/21.39 = 0.505 ksi
- ymax = 5wL^4/(384EI) = 0.147 in
