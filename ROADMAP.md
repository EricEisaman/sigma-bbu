A strong, free path into baseband firmware is to (1) build solid wireless/DSP and RTOS fundamentals, (2) work directly with open-source cellular/BLE baseband stacks, and (3) culminate in an original research-style project (e.g., security, performance, or reliability study) that you can present for Research Science Institute. [blog.collegevine](https://blog.collegevine.com/research-opportunities-high-school)
Below is a concrete 12‑month plan assuming you already write good C/C++/Rust firmware, have only free/open-source tools, and limited hardware budget.
***
## Big-picture roadmap
In baseband work you mostly need:
- **Cellular/BLE theory**: GSM/LTE/5G or BLE link layers, modulation, coding, timing.
- **DSP + SDR**: IQ samples, filters, demodulation, frame sync, symbol timing.
- **RTOS + concurrency**: event-driven, hard/soft real-time, interrupt latency.
- **Tooling**: GNU Radio, SDR stacks, emulation frameworks, protocol analyzers.
We’ll stage the year in four phases:
1. Foundations (months 1–3): wireless/DSP and RTOS basics.
2. Baseband exposure (months 3–6): get hands-on with open baseband stacks.
3. Research project build-out (months 6–11): narrow to one question and develop a system.
4. RSI application packaging (month 12): paper-style write-up, code, and demo.
Time assumptions: ~6–10 hrs/week during school, 15–20 hrs/week in summer.
***
## Phase 1 (Months 1–3): Theory + RTOS fundamentals
Goal: Be able to comfortably read baseband code and papers and talk in the language of RF/DSP and real-time firmware.
### Wireless and DSP basics (all free/open)
- Read the free “GNU Radio Documentation” and tutorials to get intuition for blocks, filters, sampling, and mod/demod. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Site: GNU Radio.
- Work through an SDR/BLE intro like the open BTLE SDR project that the open BTLE baseband chip uses as a reference. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
Concrete tasks:
- Install GNU Radio (or use a Linux box / VM + package manager).
- Implement in GNU Radio Companion:
- BPSK and GFSK transmit/receive chains (no hardware yet).
- Add noise, measure bit error rate (BER) by comparing transmitted vs received bits.
This directly parallels what the open BTLE baseband chip project does with Python for BER evaluation. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
### RTOS, scheduling, and concurrency
Free RTOS resources:
- **FreeRTOS kernel** + docs and labs are free and widely used. [reddit](https://www.reddit.com/r/embedded/comments/yrd5p8/im_an_ee_student_what_is_the_learning_path_the/)
- Site: FreeRTOS.
Project:
- On any cheap MCU board you have (or QEMU if no board), port a minimal FreeRTOS app:
- One task handles a simulated “radio RX” ring buffer.
- One task does “MAC layer” parsing of frames pushed into a queue.
- Use timers and interrupts to simulate frame arrival deadlines.
Outcome after Phase 1:
- You can explain sample rate, symbol rate, modulation, and BER.
- You’ve written at least one small RTOS-based firmware demonstrating queues, ISRs, and timing constraints.
***
## Phase 2 (Months 3–6): Hands-on baseband systems
Goal: Touch actual baseband stacks and tools that real researchers use; see their structure.
### 1. Explore open GSM baseband firmware (OsmocomBB)
- OsmocomBB is an open GSM baseband firmware for certain old feature phones. [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
- It gives you a full protocol stack: PHY, L1, L2, L3 for GSM. [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
Tasks:
- Read the OsmocomBB project rationale to understand motivations and architecture. [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
- Follow their build and run instructions, at least to the point of building firmware images and running the host tools, even if you don’t have a supported phone. [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
- Map the directory structure and write notes:
- Where is Layer 1 (radio control, bursts)?
- Where is Layer 2/3 (RLC/MAC, RR, MM, CC)?
Deliverable: A short internal “developer overview” document summarizing OsmocomBB architecture (you’ll later re-use this style for your RSI write-up).
### 2. BLE baseband: open BTLE baseband chip project
The open BTLE baseband chip project gives you:
- A Python bit-true algorithm model of the BLE baseband.
- A Verilog implementation of the controller and PHY.
- Scripts and testbenches to generate vectors and simulate the design. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
Key features from the README: [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Python algorithms aligned with an earlier SDR BTLE project (BTLE SDR).
- Python script to evaluate BER under different clock error.
- Verilog testbench plus iverilog simulation flows.
- Optional FPGA and even OpenLane2 + SKY130 PDK flow.
Tasks:
- Clone the BTLE SDR project and build it using CMake/make as indicated. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Run the `test_vector_for_btle_verilog.py` with iverilog to see how Python and Verilog interact. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Inspect Verilog modules `btle_controller.v`, `btle_rx_core.v`, `gfsk_demodulation.v`, etc., to understand how packet handling, scrambling, CRC, and modulation are structured. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
This gives you a concrete BLE baseband example that’s fully open and scriptable.
***
## Phase 3 (Months 6–11): Research-style project for RSI
Goal: Move from “learning” to “doing research.” RSI wants an original, well-motivated question with technical depth and clear methodology. [blog.collegevine](https://blog.collegevine.com/research-opportunities-high-school)
You already know C/C++/Rust; your differentiator should be:
- Combining **open baseband stacks** (OsmocomBB / BTLE SDR / BTLE chip)
- With **measurement, correctness, or security**.
Below are three realistic project directions that are free, feasible for a strong high-schooler, and clearly anchored in baseband firmware.
### Option A: Robustness of an open BLE baseband under clock error
Anchor: the BTLE baseband chip’s BER evaluation vs clock error. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
Research question (example):
> How do different baseband design parameters (e.g., filter bandwidth, demodulator implementation, symbol timing strategy) influence BLE link robustness under clock drift and noise in a low-cost baseband implementation?
Plan:
1. Use the existing Python model + Verilog baseband to generate BER vs SNR and BER vs clock error curves. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
2. Implement one or two modified versions:
- For instance, change GFSK demodulation parameters or timing recovery.
3. Compare:
- Baseline vs modified cores using the same test vectors and noise/clock profiles.
4. Analyze:
- Identify how much improvement or degradation you see, and justify it using wireless/DSP theory.
Deliverables (RSI-friendly):
- A full experimental section: methodology, parameters, scripts.
- Graphs of BER vs SNR/clock error for different designs.
- A discussion that connects to BLE spec constraints.
Why this is good:
- It is rigorous but doesn’t require expensive hardware.
- It’s based entirely on open code (Python/Verilog, iverilog, GNU Radio). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- It clearly relates to practical baseband engineering (robustness to real-world imperfections).
### Option B: Static analysis / fuzzing harness for baseband firmware
Anchor: baseband fuzzing and emulation work such as full-system emulation platforms for smartphone basebands, like FirmWire. [ringzer0](https://ringzer0.training/countermeasure25-emulation-and-fuzzing-for-baseband-firmware/)
Research question (example):
> How effective are targeted fuzzing harnesses at uncovering memory safety and logic bugs in open-source baseband control-plane code?
Plan (lightweight, student-appropriate variant):
1. Pick an open stack module with clear message-parsing logic (e.g., OsmocomBB Layer 3 messages or BLE link-layer control PDUs). [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
2. Extract that module to run on desktop (Linux) as a C/C++ library.
3. Write a harness using a free fuzzer (AFL++, libFuzzer, or Honggfuzz – all open source).
4. Run fuzzing campaigns on your laptop and record:
- Discovered crashes, coverage statistics, interesting edge cases.
5. Manually triage a few crashes and propose minimal fixes.
Deliverables:
- Technical write-up: architecture of the target, fuzzing strategy, coverage metrics.
- Patches or bug reports sent upstream (if real bugs are found).
- Reproducers demonstrating discovered issues.
Why this is good:
- Directly matches modern baseband security practice, as seen in professional trainings on reverse engineering and fuzzing baseband firmware. [github](https://github.com/FirmWire/FirmWire)
- Doesn’t require any RF hardware, only CPU time and open-source tools.
### Option C: Latency and scheduling analysis in baseband RTOS tasks
Anchor: baseband RTOS structure in modern firmware, which uses many concurrent tasks with strict timing, as described in mobile baseband training materials. [ringzer0](https://ringzer0.training/countermeasure25-emulation-and-fuzzing-for-baseband-firmware/)
Research question (example):
> What scheduling and priority assignment strategies yield the most stable timing for time-critical baseband tasks (e.g., symbol timing updates, frame decoding) on a constrained RTOS?
Plan:
1. Pick a small open-source RTOS (FreeRTOS) and create a realistic task graph:
- Radio ISR, RX processing, TX scheduling, control-plane signaling, housekeeping. [reddit](https://www.reddit.com/r/embedded/comments/yrd5p8/im_an_ee_student_what_is_the_learning_path_the/)
2. Instrument tasks to record completion time and jitter (use GPIO toggling + logic analyzer if you have hardware; otherwise, high-resolution timers on a desktop).
3. Compare different configurations:
- Priority schemes, preemption settings, queue sizes.
4. Evaluate how often deadlines are missed, how latency distributions look, etc.
Deliverables:
- Analytical model (simple queuing / scheduling analysis) + empirical measurements.
- Discussion of implications for real baseband firmware.
***
## Choosing and scoping your project
Given you are already strong in C/C++/Rust, and we are resource-constrained, I’d recommend:
- **Primary choice**: Option A (BLE baseband robustness) using BTLE baseband chip + SDR reference project. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- **Secondary choice**: Option B (fuzzing) if you are more excited about security. [github](https://github.com/FirmWire/FirmWire)
You can even combine them: e.g., fuzz the host control-plane software of the BTLE SDR project while also doing BER robustness experiments. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
***
## Detailed month-by-month sketch
### Months 1–2
- Wireless/DSP:
- Read a basic SDR / wireless primer series (like GNU Radio docs and community tutorials). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Implement and test BPSK and GFSK chains in GNU Radio.
- RTOS:
- Build a FreeRTOS-based firmware on a board or QEMU with multiple tasks and queues. [reddit](https://www.reddit.com/r/embedded/comments/yrd5p8/im_an_ee_student_what_is_the_learning_path_the/)
- Document task interactions and timing.
### Month 3
- Explore OsmocomBB’s rationale and architecture. [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
- Build the project, generate docs from the code, and map the layering.
- Identify modules that interest you (e.g., paging, random access, RR).
### Months 4–5
- Clone and run the BTLE open SDR project and then the open BTLE baseband chip project. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Build the C code with CMake and run the host application. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Run `test_vector_for_btle_verilog.py`, then the Verilog simulation with iverilog and vvp. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Take detailed notes on:
- How Python generates test vectors.
- What each Verilog module does at a high level.
### Month 6
- Decide your project question (Option A/B/C).
- Read at least 2–3 related open-access papers/blog posts on baseband robustness or fuzzing (e.g., FirmWire or baseband security research blogs). [ringzer0](https://ringzer0.training/countermeasure25-emulation-and-fuzzing-for-baseband-firmware/)
- Write a one-page proposal including:
- Problem statement.
- Background.
- Experimental plan.
This one-pager becomes the seed for your RSI research proposal.
### Months 7–9
Focus on implementation:
- Option A (robustness):
- Add parameterization hooks to the Python/Verilog models (e.g., different filter coefficients or timing strategies). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Script multiple experiment runs sweeping SNR and clock error ranges. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Store results in CSVs; plot with matplotlib.
- Option B (fuzzing):
- Extract and isolate a message parsing module from OsmocomBB or BTLE SDR host. [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
- Build fuzzing harness, integrate with AFL++ or libFuzzer.
- Run long fuzzing sessions; keep logs and crash reports.
Throughout:
- Keep a lab notebook (can be a private GitHub repo with markdown).
- Push code to a public GitHub repo (sanitized of any private data), making regular commits.
### Months 10–11
Analysis and writing:
- Thoroughly analyze your data:
- Graphs, error bars, timelines, coverage curves, etc.
- Draft a 8–15 page research-style report:
- Abstract, introduction, background, methods, results, discussion, conclusion.
- Aim for clarity over length; diagrams of your architecture help.
Also:
- Prepare a ~10–15 minute talk with slides:
- Motivation (why baseband matters: always-connected, security, reliability). [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
- Your approach and results.
- Practice explaining concepts in simple terms (RSI reviewers span many fields).
***
## Free/open tools you should standardize on
All are free and widely used by professionals:
- OS: Linux (Ubuntu) on laptop / old PC or in a VM (needed for most SDR/baseband tools). [ringzer0](https://ringzer0.training/countermeasure25-emulation-and-fuzzing-for-baseband-firmware/)
- SDR / DSP:
- GNU Radio for block-based DSP.
- BLE baseband:
- BTLE SDR + BTLE baseband chip (Python, Verilog, iverilog, OpenLane2 optional). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- GSM baseband:
- OsmocomBB for open GSM firmware. [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
- Emulation / security (if going security route):
- FirmWire to study how full baseband emulation is structured. [github](https://github.com/FirmWire/FirmWire)
- RTOS:
- FreeRTOS for RTOS concepts and code. [reddit](https://www.reddit.com/r/embedded/comments/yrd5p8/im_an_ee_student_what_is_the_learning_path_the/)
- Fuzzing:
- AFL++, libFuzzer (via Clang), or Honggfuzz.
- Languages:
- C/C++ for existing stacks.
- Python for scripting and analysis.
- Rust for experimental tooling or harnesses (optional but distinctive).
***
## Positioning for RSI
RSI is extremely selective; they like students who:
- Demonstrate depth in a focused area rather than surface-level in many. [blog.collegevine](https://blog.collegevine.com/research-opportunities-high-school)
- Show independent initiative and familiarity with real research tools. [blog.collegevine](https://blog.collegevine.com/research-opportunities-high-school)
You can emphasize:
- You started from free, open tools and built a research-grade project in a niche area (baseband firmware).
- You engaged with real open-source projects like OsmocomBB and BTLE baseband chip, which are used or referenced by researchers. [osmocom](https://osmocom.org/projects/baseband/wiki/ProjectRationale)
- You produced data, code, and a write-up that could plausibly be extended into a paper or upstream contributions.
--------
Focusing on robustness is a very strong choice, and it maps nicely onto a clean, simulation-only project around BLE/baseband robustness that fits Research Science Institute expectations. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
Below is an RSI-style proposal outline plus an experiment plan with milestones, designed to run on a Linux laptop with free, open-source tools only.
***
## Working title and 1–2 sentence pitch
**Working title**
Robustness of an Open-Source Bluetooth Low Energy Baseband Under Clock Drift and Noise
**Pitch**
This project will experimentally evaluate how implementation choices in an open-source BLE baseband (filtering, timing, and demodulation parameters) affect robustness to clock drift and channel noise, using a fully open Python/Verilog simulation flow for bit‑error‑rate (BER) and packet‑error‑rate (PER) analysis. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
***
## Proposal outline (RSI-style sections)
### 1. Background and motivation
- 5G/Open RAN/cloudified baseband units in the field must maintain robust links under harsh conditions (timing offsets, interference, hardware variation), especially for URLLC and massive IoT traffic. [sciencedirect](https://www.sciencedirect.com/science/article/pii/S0957417424021791)
- Robustness is now a key selling point in market reports and operator roadmaps: vendors highlight resilience, uptime, interference handling, and multi‑RAT coexistence in their positioning for multi‑mode baseband units. [sciencedirect](https://www.sciencedirect.com/science/article/pii/S0957417424021791)
- BLE is widely used for IoT; its baseband layer faces similar robustness challenges at lower power and cost points, making it an ideal sandbox for student‑scale experimentation. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
You can explicitly connect this to “edge baseband” trends: as RAN functions move to the edge and become more virtualized, link robustness is a core KPI, and low‑power links like BLE are often involved in local sensing/control loops. [pmc.ncbi.nlm.nih](https://pmc.ncbi.nlm.nih.gov/articles/PMC10857264/)
### 2. Prior work and open tools
- The **open BTLE baseband chip** project provides a bit‑true Python model of the BLE baseband, a Verilog implementation, and a testbench and scripts for BER evaluation under clock error and noise, all with open‑source tooling (iverilog, OpenLane2, etc.). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- The project already includes:
- Python algorithms aligned with an earlier BTLE SDR project.
- A Python script to evaluate BER for different clock errors.
- A Verilog testbench that drives the baseband with noise/clock profiles. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- In the broader baseband world, research platforms like **FirmWire** and BASECOMP demonstrate how open tools can be used to study robustness/security, but they mainly focus on security and integrity protection rather than physical‑layer robustness to drift and noise. [ndss-symposium](https://www.ndss-symposium.org/wp-content/uploads/2022-136-paper.pdf)
You position your work as: “extending and systematizing the BER/robustness evaluation for an open BLE baseband design, exploring more parameters and trade‑offs than the original example script, and framing it in terms of baseband robustness KPIs.”
### 3. Research question and hypotheses
**Research question (example wording)**
How do specific baseband implementation parameters—such as GFSK demodulator configuration, symbol timing recovery strategy, and filter bandwidth—affect the robustness of a BLE link under combined clock drift and additive noise in an open-source baseband design? [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
**Hypotheses (examples)**
- H1: Narrower receiver filters improve robustness to adjacent‑channel interference but increase sensitivity to clock drift, degrading BER when drift exceeds a threshold. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- H2: A timing recovery algorithm that tracks drift adaptively will yield lower PER at moderate drift compared to a simpler fixed‑timing design, for the same SNR. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- H3: There exist parameter regions where small changes significantly improve robustness without noticeably increasing implementation complexity or resource usage (relevant to low‑cost IoT BBUs). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
***
### 4. Methods and experimental design
All experiments are simulation‑only, running on a Linux laptop/desktop.
#### 4.1 Tools and environment (all free/open-source)
- OS: Linux (e.g., Ubuntu) installed natively or in a VM.
- Baseband project: **open BTLE baseband chip** (Python + Verilog). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Verilog simulator: **iverilog** and **vvp**. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Python stack: Python 3 + NumPy + matplotlib for scripting and plotting. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Version control: Git + GitHub for code and lab notebook (free public repo).
No SDR hardware is required; the channel, noise, and clock drift are all modeled in software. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
#### 4.2 Baseline system
- Clone the BTLE baseband chip repository and associated BTLE SDR reference if needed. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Build and run the existing BER vs clock error script:
- `test_vector_for_btle_verilog.py` (or equivalent) generates test vectors and drives the Verilog core. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Confirm you can reproduce the provided BER curves vs clock error / SNR. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Document:
- Default modulation parameters (Gaussian filter BT, frequency deviation, symbol rate).
- Default demodulation structure (e.g., quadrature discriminator, filter, slicer). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
This becomes the “baseline design.”
#### 4.3 Parameter variations (design space)
You choose a small but meaningful subset of baseband parameters to vary:
- **Filter bandwidth / BT product** in the GFSK modulation/demodulation chain. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- **Timing recovery** (e.g., sample‑per‑symbol, early‑late gate, loop bandwidth if implemented in the model).
- **Clock drift profile**:
- Static offset (ppm range).
- Slowly varying drift (to emulate cheap oscillators). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- **SNR / noise level**: AWGN noise injected at the channel.
Each configuration is a point in this parameter space.
#### 4.4 Experimental procedure
For each parameter configuration:
1. Configure Python script to generate a long random BLE packet stream (e.g., 10⁵–10⁶ bits). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
2. Apply a specified SNR and clock drift profile. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
3. Run the Verilog baseband through iverilog/vvp to decode the stream. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
4. Collect:
- **Bit Error Rate (BER)**: fraction of bits decoded incorrectly.
- **Packet Error Rate (PER)**: fraction of BLE packets failing CRC.
- Optional: log sync failures vs correctly acquired packets.
Sweep:
- SNR from “good” to “poor” (e.g., 0–20 dB).
- Clock drift from 0 ppm to a few hundred ppm. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- A few discrete design variants (e.g., 3–5 configurations).
#### 4.5 Analysis
- Plot BER and PER as functions of:
- SNR for fixed drift and design variant.
- Clock drift for fixed SNR and design variant. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Compare curves across baseband configurations:
- Identify parameter choices that optimize robustness in specific regions (e.g., low SNR, high drift).
- Interpret results in terms of:
- BLE link budget and oscillator tolerances.
- Implications for low‑cost IoT baseband units and, by analogy, 5G/Open RAN robustness considerations. [pmc.ncbi.nlm.nih](https://pmc.ncbi.nlm.nih.gov/articles/PMC10857264/)
***
### 5. Expected contributions
- A systematic, reproducible evaluation of robustness for an open BLE baseband core under combined noise and clock drift. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Identification of specific implementation parameter ranges that significantly affect BER/PER, with quantitative plots. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Open‑sourced scripts and configuration files to allow others to reproduce and extend the experiments. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- A conceptual mapping from BLE baseband robustness to broader RAN/baseband robustness narratives (e.g., edge deployment, URLLC, massive IoT). [sciencedirect](https://www.sciencedirect.com/science/article/pii/S0957417424021791)
For RSI, this shows: clear question, experimental rigor, and relevance to an active research/industry topic.
***
### 6. Minimal hardware and cost
- One Linux‑capable computer (can be low‑end; simulations are not extremely heavy).
- No SDR boards or lab equipment required.
- All tools (Python, iverilog, Git, BTLE baseband project) are free and open source. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
***
## Concrete experiment plan with milestones
Assume ~12 months, but this can compress if they start now and work more intensely in summer.
### Milestone 1 (Weeks 1–4): Setup and baseline reproduction
- Install Linux and toolchain (Python, iverilog, Git).
- Clone and build the open BTLE baseband chip project. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Run provided tests and the BER/clock error example script to generate at least one BER curve. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Deliverables:
- Short internal log: installation steps, any issues, screenshots of successful runs.
- One baseline BER vs clock drift plot (even if rough). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
### Milestone 2 (Weeks 5–8): Code understanding and design of experiments
- Read through the Python model and Verilog modules relevant to modulation/demodulation, timing, and filters. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Write a 2–3 page “mini design doc” summarizing:
- Signal flow from input bits to output bits.
- Where clock drift/noise is modeled.
- Which parameters are easiest/most meaningful to vary. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Finalize the list of 3–5 design variants and the SNR/clock‑drift sweep ranges.
### Milestone 3 (Weeks 9–14): Implementation of parameterizable experiments
- Modify or wrap the Python scripts to accept parameters for:
- SNR, clock drift profile, filter bandwidth, timing settings. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Implement a script that:
- Iterates over a grid of configurations (e.g., 3 SNR levels × 3 drift levels × 4 designs).
- Runs the Verilog simulation for each configuration. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Logs BER and PER to CSV files.
- Verify correctness on a small subset first (sanity checks: BER ~ 0 at high SNR/low drift, increases as conditions worsen). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
### Milestone 4 (Weeks 15–22): Large experiment sweeps and data collection
- Run full sweeps (may take many hours; can run overnight). [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Back up raw outputs to GitHub.
- Generate first round of plots:
- BER vs SNR for each design (separate curves).
- BER vs clock drift for each design. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Start annotating interesting behaviors (e.g., where curves diverge).
### Milestone 5 (Weeks 23–28): Deep analysis and interpretation
- Clean the analysis scripts and produce publication‑quality plots.
- Compute any aggregated metrics:
- E.g., “robustness score”: area under the BER curve below a threshold, across a range of SNR/drift. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
- Write 4–6 pages of results and discussion:
- What trade‑offs did you see?
- How do the results map to practical baseband robustness concerns (e.g., cheap crystals, interference at edge deployments)? [pmc.ncbi.nlm.nih](https://pmc.ncbi.nlm.nih.gov/articles/PMC10857264/)
### Milestone 6 (Weeks 29–36): Draft RSI‑ready paper and talk
- Expand into a full research‑style paper (8–15 pages):
- Introduction with 5G/Open RAN/baseband robustness motivation. [sciencedirect](https://www.sciencedirect.com/science/article/pii/S0957417424021791)
- Related work referencing the open BTLE baseband chip and any baseband robustness literature you find. [usenix](https://www.usenix.org/system/files/usenixsecurity23-kim-eunsoo.pdf)
- Methods, results, discussion, conclusion.
- Create a 10–15 minute slide deck:
- Explain BLE baseband, robustness, and your experiment in accessible terms.
- Rehearse explaining:
- What robustness means in baseband units.
- Why your open BLE baseband study is a relevant and tractable slice of that problem.
***
## How to phrase the “robustness” connection for reviewers
When describing the project in your RSI application:
- Explicitly connect BLE robustness to broader BBU robustness:
- “Although my experiments focus on an open BLE baseband core, the same issues of timing drift, noise, and implementation trade‑offs affect 5G/Open RAN baseband units that must robustly carry URLLC and massive IoT traffic at the edge.” [pmc.ncbi.nlm.nih](https://pmc.ncbi.nlm.nih.gov/articles/PMC10857264/)
- Mention that robustness is a highlighted selling point in commercial multi‑mode baseband unit market reports, emphasizing resilience and uptime, which your metrics (BER/PER under challenging conditions) directly relate to. [sciencedirect](https://www.sciencedirect.com/science/article/pii/S0957417424021791)
- Emphasize that you used fully open source tools and code from an open BLE baseband chip project to do a systematic, reproducible robustness evaluation. [sdr-x.github](http://sdr-x.github.io/open_btle_baseband_chip/)
