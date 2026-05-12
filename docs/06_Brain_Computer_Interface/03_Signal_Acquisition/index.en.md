# Signal Acquisition Technology

The performance ceiling of a BCI is determined on the acquisition side: different electrodes observe entirely different signal scales, bandwidths, and channel counts. This chapter introduces acquisition technologies in order of decreasing invasiveness, and also covers stimulation (write-in) technology and the preprocessing pipeline.

**The role of this chapter.** It turns the "neurophysiology picture" from Chapter 02 into measurable engineering. Each hardware platform here maps to a company in Chapter 11: Utah array → BrainGate / Pitt team, flexible threads → Neuralink, endovascular → Synchron, thin-film → Precision, EEG → Muse / Emotiv. Understanding the scale ladder of electrodes (single-cell spikes → mm-scale LFP → cm-scale ECoG → scalp EEG) also explains why EEG BCIs can never reach Utah-array decoding accuracy, why Stentrode trades bandwidth for reversibility, and why fMRI cannot drive a real-time BCI.

**Recommended reading order.** Readers oriented toward clinical or hardware engineering should start with *Invasive Electrodes* and *Minimally Invasive Interfaces* to master the platforms that have actually been implanted in patients. Readers oriented toward consumer or algorithmic work can start with *Non-Invasive Recording* and compare the trade-offs among EEG / MEG / fMRI / fNIRS. Either path must end with *Stimulation Technology* and *Signal Preprocessing*: the former underpins the sensory writing in Chapter 09, while the latter (**filtering / spike sorting / artifact removal**) is mandatory groundwork before any decoding pipeline can begin.

**Chapter contents:**

- **[Invasive Electrodes](侵入式电极.md)** — Utah array, Neuropixels, floating microelectrode arrays
- **[Minimally Invasive Interfaces](微创接口.md)** — Stentrode (endovascular), Neuralink N1 (flexible threads), Precision Layer 7 (thin film)
- **[Non-Invasive Recording](非侵入式采集.md)** — EEG, MEG, fMRI, fNIRS comparison
- **[Stimulation Technology](刺激侧技术.md)** — TMS, DBS, ICMS, focused ultrasound (FUS)
- **[Signal Preprocessing](信号预处理.md)** — Filtering, artifact removal, spike sorting; MNE and EEGLAB tools
