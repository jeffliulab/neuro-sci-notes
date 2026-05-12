# Neurophysiology Foundations

The physical feasibility of BCI rests on understanding how the nervous system encodes information. This chapter makes four things clear: how motor cortex produces decodable intent signals, what level of signal each electrode type can see, why population activity lives on a low-dimensional manifold, and how sensory cortex can be written into.

**The role of this chapter.** It is the middleware between signal physics and algorithms. The electrode choices in Chapter 03, the decoders in Chapters 04–05, and the sensory writing in Chapter 09 all depend on the neurophysiology picture established here: which area encodes motor preparation, what scale (spike / LFP / ECoG / EEG) can see what, why population coding lets you infer full intent from a small set of neurons, and why somatotopy determines the spatial resolution of ICMS prostheses. Without this chapter it is hard to truly understand why Utah arrays go into M1 rather than V1 in Chapter 03, or why Flesher 2016 (Chapter 09) could let a paralyzed patient "feel" objects again.

**Recommended reading order.** Read the four sections as "region → signal → population → writing": start with *Motor Cortex Hierarchy* to nail down the functional division of **M1 / PMd / PMv / SMA / posterior parietal**; then move to *Origins of Neural Signals* to understand the spike → LFP → ECoG → EEG scale ladder; next take in the **rotational dynamics** and preparatory subspace of Churchland-Shenoy in *Neural Manifolds and Dynamics*; finally, *Sensory Cortex and Somatotopy* turns to S1 / V1 and lays the groundwork for the writing-side chapters later.

**Contents:**

- **[Motor Cortex Hierarchy](运动皮层层级.md)** — M1, PMd, PMv, SMA, posterior parietal; population coding
- **[Origins of Neural Signals](神经信号起源.md)** — Physical origins and scales of spike, LFP, ECoG, EEG
- **[Neural Manifolds and Dynamics](神经流形与动力学.md)** — Churchland-Shenoy rotational dynamics, preparatory subspace
- **[Sensory Cortex and Somatotopy](感觉皮层与躯体图谱.md)** — S1 somatotopy, visual cortex V1, phosphene mapping
