# Consumer & Non-invasive BCI

Implantable BCIs are medical devices aimed at a small number of patients; only non-invasive BCIs have the potential to reach billions of users. This chapter covers three things: the capability boundary of today's consumer EEG devices, neural-sensing patents from mainstream brands like Apple AirPods, and the real-world status in 2024–2026 of the long-standing vision of "thought-to-text."

**Relationship to Chapter 11.** Chapter 11 is the commercial landscape for invasive BCI (the medical-device pathway); this chapter is the consumer landscape for non-invasive BCI (the consumer-electronics pathway). Their regulatory regimes differ entirely: FDA / NMPA demand years of clinical evidence for the former, while the latter mostly needs only FCC EMC and general consumer-electronics compliance. Both paths share a common paradox — **signal quality is inversely correlated with product form** — and that tension runs through the whole chapter: hiding electrodes inside earbuds / glasses / headbands forces signal-to-noise to drop several orders of magnitude below a Utah array, so the decoding side (Chapters 04, 05) must compensate heavily.

**Recommended reading order.** *Consumer EEG Device Panorama* first establishes the side-by-side comparison of **Muse / Emotiv / OpenBCI / Neurable** (research vs consumer, wet vs dry electrodes, sampling rate and channel count). *Apple Neural Sensing Patents* then zooms in on the most commercially significant player — AirPods EEG, granted in 2023, is expected to be the key product to bring consumer BCI mainstream. Finally, *Thought-to-Text Status* delivers a reality check: after twenty years of research, **EEG-based thought typing is still not practical**, the WER gap between invasive and non-invasive is wide, and multimodal fusion (eye-tracking + EMG + EEG) may be the only way to break the bandwidth ceiling.

**This chapter covers:**

- **[Consumer EEG Device Panorama](EEG消费设备全景.md)** — Muse, Emotiv, OpenBCI, Neurable; research vs consumer contexts
- **[Apple Neural Sensing Patents](Apple_神经传感专利.md)** — AirPods EEG (patent granted 2023); expected applications
- **[Thought-to-Text Status](思维打字现状.md)** — non-invasive vs invasive WER comparison; why EEG typing is still not practical
