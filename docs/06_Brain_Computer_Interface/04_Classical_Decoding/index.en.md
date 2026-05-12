# Classical Decoding Algorithms

Before deep learning took over BCI, classical linear and probabilistic methods had already built a well-functioning pipeline. These methods remain benchmarks for clinical BCI today: they are computationally cheap, require little training data, and run easily in real time. Understanding them is a prerequisite for understanding modern decoders.

**Why study them still.** Since 2018, the deep-learning decoders of Chapter 05 have largely won on paper benchmarks, yet **the clinical BCIs actually deployed in patients (Pitt arm, BrainGate, Walk Again) almost all still run variants of Kalman + ReFIT**. Three reasons: first, clinical sessions only yield tens of minutes of training data per day, where deep models easily overfit; second, FDA approval demands models that are "interpretable, verifiable, and tunable on-site," which linear-Gaussian models satisfy out of the box; third, closed-loop latency budgets are under 50 ms, beyond the reach of neural foundation-model inference. The methods in Chapter 05 stack *on top of* this foundation, not in place of it.

**Recommended reading order.** Read the four sections as "read → calibrate → compute": start with *Population Vector Algorithm* to absorb the foundational insight of Georgopoulos 1984 (**broadly tuned single neurons + population voting = precise intent**); then move to the linear-Gaussian paradigm of *Wiener and Kalman Filtering*; next take in *ReFIT and Online Calibration* and understand why Gilja 2012 is the cornerstone of closed-loop BCI; finally, *Linear Discriminant Analysis and Feature Selection* extends the same line into the EEG-BCI side via **CSP / FBCSP / Riemannian geometry** — still the algorithmic mainstream of non-invasive BCI today.

**In this chapter:**

- **[Population Vector Algorithm](群体向量算法.md)** — Georgopoulos 1984; population vector coding
- **[Wiener and Kalman Filtering](维纳与卡尔曼滤波.md)** — the classical paradigm of linear-Gaussian decoding
- **[ReFIT and Online Calibration](ReFIT与在线校准.md)** — Gilja 2012 eLife; foundational work on closed-loop calibration
- **[Linear Discriminant Analysis and Feature Selection](线性判别与特征选择.md)** — CSP, FBCSP, and Riemannian geometry in EEG BCI
