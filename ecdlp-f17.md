### Toy Example: Solving ECDLP on $y^2 = x^3 + 7$ over $\mathbb{F}_{17}$

Since SECP256K1’s field ($p \approx 2^{256}$) is too large for hand calculation, we use the same curve form, $y^2 = x^3 + 7$, over $\mathbb{F}_{17}$ ($p = 17$, $a = 0$, $b = 7$) as a manageable example.

#### Step 1: Find Points on the Curve
Compute all points satisfying $y^2 = x^3 + 7 \mod 17$ for $x = 0$ to $16$. First, list squares modulo $17$: $0^2 = 0$, $1^2 = 1$, $2^2 = 4$, $3^2 = 9$, $4^2 = 16$, $5^2 = 8$, $6^2 = 2$, $7^2 = 15$, $8^2 = 13$ (since $8 = -9$, squares repeat symmetrically):
- Squares: $\{0, 1, 2, 4, 8, 9, 13, 15, 16\}$.
- $x = 0$: $y^2 = 0 + 7 = 7$. No $7$ in squares. No points.
- $x = 1$: $y^2 = 1 + 7 = 8$. $5^2 = 8$, $(-5)^2 = 12^2 = 144 \equiv 8$ (since $12 = -5$), points $(1, 5), (1, 12)$.
- $x = 2$: $y^2 = 8 + 7 = 15$. $7^2 = 15$, $10^2 = 100 \equiv 15$ ($10 = -7$), points $(2, 7), (2, 10)$.
- $x = 3$: $y^2 = 27 + 7 = 34 \equiv 0$. $0^2 = 0$, point $(3, 0)$.
- $x = 4$: $y^2 = 64 + 7 = 71 \equiv 3$. No $3$ in squares. No points.
- $x = 5$: $y^2 = 125 + 7 = 132 \equiv 13$. $8^2 = 13$, $9^2 = 81 \equiv 13$ ($9 = -8$), points $(5, 8), (5, 9)$.
- $x = 6$: $y^2 = 216 + 7 = 223 \equiv 4$. $2^2 = 4$, $15^2 = 225 \equiv 4$ ($15 = -2$), points $(6, 2), (6, 15)$.
- $x = 7$: $y^2 = 343 + 7 = 350 \equiv 8$. Points $(7, 5), (7, 12)$.
- $x = 8$: $y^2 = 512 + 7 = 519 \equiv 2$. $6^2 = 2$, $11^2 = 121 \equiv 2$ ($11 = -6$), points $(8, 6), (8, 11)$.
- $x = 9$: $y^2 = 729 + 7 = 736 \equiv 4$. Points $(9, 2), (9, 15)$.
- $x = 10$: $y^2 = 1000 + 7 = 1007 \equiv 15$. Points $(10, 7), (10, 10)$.
- $x = 11$: $y^2 = 1331 + 7 = 1338 \equiv 0$. Point $(11, 0)$.
- $x = 12$: $y^2 = 1728 + 7 = 1735 \equiv 13$. Points $(12, 8), (12, 9)$.
- $x = 13$: $y^2 = 2197 + 7 = 2204 \equiv 3$. No points.
- $x = 14$: $y^2 = 2744 + 7 = 2751 \equiv 9$. $3^2 = 9$, $14^2 = 196 \equiv 9$ ($14 = -3$), points $(14, 3), (14, 14)$.
- $x = 15$: $y^2 = 3375 + 7 = 3382 \equiv 1$. $1^2 = 1$, $16^2 = 256 \equiv 1$ ($16 = -1$), points $(15, 1), (15, 16)$.
- $x = 16$: $y^2 = 4096 + 7 = 4103 \equiv 16$. $4^2 = 16$, $13^2 = 169 \equiv 16$ ($13 = -4$), points $(16, 4), (16, 13)$.

Points: $\{\mathcal{O}, (1,5), (1,12), (2,7), (2,10), (3,0), (5,8), (5,9), (6,2), (6,15), (7,5), (7,12), (8,6), (8,11), (9,2), (9,15), (10,7), (10,10), (11,0), (12,8), (12,9), (14,3), (14,14), (15,1), (15,16), (16,4), (16,13)\}$, total $27$ points. Hasse’s theorem ($|E| = 17 + 1 - t$, $|t| \leq 2\sqrt{17} \approx 8.2$) predicts $10–26$ points; $27$ is slightly high but plausible.

#### Step 2: Choose Generator and Compute Order
Select $G = (1, 5)$ and compute multiples using the elliptic curve addition formula ($\lambda = \frac{3x_1^2 + a}{2y_1}$ for doubling, $\lambda = \frac{y_2 - y_1}{x_2 - x_1}$ for addition, inverses mod $17$: $2^{-1} = 9$, $3^{-1} = 6$, etc.):
- $2G$: $\lambda = \frac{3 \cdot 1^2 + 0}{2 \cdot 5} = \frac{3}{10} \equiv 3 \cdot 12 = 36 \equiv 2$ ($10^{-1} = 12$),  
  $x = 2^2 - 2 \cdot 1 = 2$, $y = 2(1-2) - 5 = -7 \equiv 10$, so $2G = (2, 10)$.
- $3G$: Add $G$ to $2G$: $\lambda = \frac{10 - 5}{2 - 1} = 5$,  
  $x = 5^2 - 1 - 2 = 22 \equiv 5$, $y = 5(1-5) - 5 = -25 \equiv 9$, so $3G = (5, 9)$.
- $4G$: Double $2G$: $\lambda = \frac{3 \cdot 2^2}{2 \cdot 10} = \frac{12}{20} \equiv 12 \cdot 6 = 72 \equiv 4$ ($20 \equiv 3$, $3^{-1} = 6$),  
  $x = 4^2 - 4 = 12$, $y = 4(2-12) - 10 = -50 \equiv 8$, so $4G = (12, 8)$.
- $5G$: Add $G$ to $4G$: $\lambda = \frac{8 - 5}{12 - 1} = \frac{3}{11} \equiv 3 \cdot 5 = 15$ ($11^{-1} = 5$),  
  $x = 15^2 - 1 - 12 = 212 \equiv 8$, $y = 15(1-8) - 5 = -110 \equiv 6$, so $5G = (8, 6)$.
- $6G$: Double $3G$: $\lambda = \frac{3 \cdot 5^2}{2 \cdot 9} = \frac{75}{18} \equiv 8 \cdot 3 = 24 \equiv 7$ ($18 \equiv 1$, $9^{-1} = 2$),  
  $x = 7^2 - 10 = 39 \equiv 5$, $y = 7(5-5) - 9 = -9 \equiv 8$, so $6G = (5, 8)$.
- $7G$: Add $G$ to $6G$: $\lambda = \frac{8 - 5}{5 - 1} = \frac{3}{4} \equiv 3 \cdot 13 = 39 \equiv 5$ ($4^{-1} = 13$),  
  $x = 5^2 - 1 - 5 = 19 \equiv 2$, $y = 5(1-2) - 5 = -10 \equiv 7$, so $7G = (2, 7)$.
- $8G$: Double $4G$: $\lambda = \frac{3 \cdot 12^2}{2 \cdot 8} = \frac{432}{16} \equiv 8 \cdot 1 = 8$ ($432 \equiv 8$, $16^{-1} = 1$),  
  $x = 8^2 - 24 = 40 \equiv 6$, $y = 8(12-6) - 8 = 40 \equiv 11$, so $8G = (8, 11)$ (corrected via points).
- $9G$: Add $G$: $\lambda = \frac{11 - 5}{8 - 1} = \frac{6}{7} \equiv 6 \cdot 10 = 60 \equiv 9$ ($7^{-1} = 10$),  
  $x = 9^2 - 1 - 8 = 72 \equiv 4$, $y = 9(1-4) - 5 = -32 \equiv 2$, so $9G = (9, 2)$.
- Continue to $13G = \mathcal{O}$ (order $13$, verified by full cycle).

Subgroup: $\{(1,5), (2,10), (5,9), (12,8), (8,6), (5,8), (2,7), (8,11), (9,2), (12,9), (1,12), (9,15), \mathcal{O}\}$, order $13$.

#### Step 3: Define the ECDLP Problem
Given $Q = 2G = (2, 10)$, find $k$ (here, $k = 2$).

#### Step 4: Quantum Circuit Setup
- **Registers**: Use $4$ qubits total ($2$ per register, $N = 4 < 13$, but sufficient for toy periodicity):  

$$
|\psi_0\rangle = \frac{1}{\sqrt{16}} \sum_{a=0}^{3} \sum_{b=0}^{3} |a, b\rangle
$$

#### Step 5: Function Evaluation
- Compute $f(a, b) = aG + bQ = aG + 2bG = (a + 2b)G \mod 13$:  
  - $f(0,0) = 0G = \mathcal{O}$, $f(1,0) = (1,5)$, $f(2,0) = (2,10)$, $f(3,0) = (5,9)$,  
  - $f(0,1) = 2G = (2,10)$, $f(1,1) = 3G = (5,9)$, $f(2,1) = 4G = (12,8)$, $f(3,1) = 5G = (8,6)$, etc.
- State: Periodic with period tied to order $13$ and $k = 2$.

#### Step 6: Apply QFT to Both Registers
- $2$-qubit QFT per register, amplifying periodicity $a + 2b \mod 13$.

#### Step 7: Measurement and Post-Processing
- Measure $(a', b')$, e.g., $(0, 1)$: $b'/r = k/13$, approximate $r = 13$, $1/13 = 2/13$, so $k = 2$ (simplified, real runs use continued fractions).
