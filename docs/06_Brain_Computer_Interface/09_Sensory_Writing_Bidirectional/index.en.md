# Sensory Writing and Bidirectional BCI

The first eight chapters focused on "reading the brain" — extracting signals from neural activity. This chapter turns to "writing the brain" — stimulating the cortex to write back touch, proprioception, vision, or even memory fragments. This is the critical transition for BCI from "remote-controlled robotic arm" to "embodied prosthesis": without sensory feedback, the user must constantly watch the arm to check whether an action completed; with sensory feedback, the prosthesis truly becomes part of the body.

**Why writing is necessary.** Anything that humans do without looking — picking up an egg, holding a coffee cup, buttoning a shirt — depends on real-time proprioceptive and tactile feedback. The Pitt arm (Chapter 06) can complete visual tasks but cannot reliably grip fragile objects, precisely because it lacks the writing side; once Flesher 2016 added ICMS-based tactile feedback to the Pitt team's setup, grasp time dropped from 20 s to 10 s — direct evidence of the value of closing the write-in loop. This chapter also connects to the visual prosthesis (V1 phosphene writing) at the end of Chapter 08, completing a three-way picture of "writing motor / writing vision / writing memory."

**Recommended reading order.** Read the four sections as "touch → bidirectional → memory → safety": start with *ICMS and Somatosensory Feedback* and the **Flesher 2016 / 2021 Sci Transl Med** lineage, to see how intracortical microstimulation can restore touch; move on to *Bidirectional BCI and Channel Separation* and digest Ganzer 2020 Cell — the algorithmic challenge of **reading movement and writing touch in the same M1 patch simultaneously**; *Memory Prosthesis* turns to the Hampson / Berger hippocampal MIMO model, the most controversial and technically deepest write-in frontier; finally, *Neural Stimulation Safety* imposes engineering bounds on every stimulation scheme (**charge density / long-term stability / tissue damage**).

**Chapter contents:**

- **[ICMS and Somatosensory Feedback](ICMS与体感反馈.md)** — Flesher 2016 / 2021 Sci Transl Med; intracortical microstimulation
- **[Bidirectional BCI and Channel Separation](双向BCI与多路分离.md)** — Ganzer 2020 Cell; simultaneously reading movement and writing touch in the same M1 patch
- **[Memory Prosthesis](记忆假体.md)** — Hampson / Berger hippocampal prosthesis; MIMO model
- **[Neural Stimulation Safety](神经刺激安全性.md)** — charge-density thresholds, long-term stability, tissue damage assessment
