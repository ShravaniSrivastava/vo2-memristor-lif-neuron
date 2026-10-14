# VO₂ Memristor-Based Leaky Integrate-and-Fire Neuron and Logic Gates

A simulation study of spike-based computing: a vanadium dioxide (VO₂) memristor is used to build a Leaky Integrate-and-Fire (LIF) neuron in LTspice, and that neuron is used to realise **AND, OR and NOT** logic gates.

*BSc (Hons) Electronics dissertation, ARSD College, University of Delhi (2026). Simulation only.*

---

## Overview

Conventional computers move data between separate memory and processing units, which costs energy. Neuromorphic computing takes a cue from the brain, where neurons communicate with short spikes and compute sparsely. This project asks a narrow, practical question:

> Can a VO₂ memristor, which switches abruptly at a threshold and resets itself, serve as the spiking element of a neuron, and can that neuron implement basic Boolean logic?

In simulation, yes, but only inside a narrow range of circuit parameters.

## Key results

- **Reproduced I–V behaviour of 9 published memristor device profiles** using the Yakopcic SPICE model, as a validated starting point for the work.
- **Found that a non-volatile memristor cannot act as the spiking element.** In the LIF circuit it latches into its low-resistance state after the first spike and spiking stops, so a *volatile* device is needed.
- **Built a custom VO₂ threshold-switching subcircuit:** switches on at **Vth = 3.4 V** and resets below **Vhold = 1.45 V** (hysteresis), with a resistance ratio of about 150× and a state time constant of 200 ns.
- **Built an LIF neuron and AND, OR and NOT gates** from it, with every truth-table case simulated and checked.
- **Mapped the failure modes** (latch-up, sub-threshold operation, runaway oscillation) and showed that the gates work only in a narrow stable operating window.
- **Compared against TTL logic** using datasheet values: far lower idle power, but higher energy per operation and slower switching.

## Approach

```mermaid
flowchart LR
    A[Reproduce published memristor I-V with the Yakopcic model] --> B[Test in an LIF circuit: non-volatile device latches]
    B --> C[Build a volatile VO2 threshold-switching subcircuit]
    C --> D[LIF neuron]
    D --> E[AND, OR and NOT gates]
    E --> F[Failure modes and TTL comparison]
```

**How the neuron works.** The input charges a capacitor (*integrate*) while a resistor slowly drains it (*leak*). When the membrane voltage reaches the VO₂ threshold, the device switches to its low-resistance state and dumps the charge as an output spike (*fire*). When the voltage falls below the holding value, the device returns to its insulating state and the cycle repeats (*reset*).

![LIF neuron schematic](figures/01_lif_schematic.png)
*LTspice schematic of the VO₂ memristor LIF neuron.*

![VO₂ I–V threshold switching](figures/02_vo2_iv_switching.png)
*Simulated I–V of the custom VO₂ model: threshold switching with hysteresis.*

![LIF waveforms](figures/03_lif_waveforms.png)
*LIF neuron operation: the membrane voltage integrates, crosses threshold, fires and resets.*

## Logic gates

All gates use the same neuron. Only the input network and bias are changed.

| Gate | Condition | Output |
|------|-----------|--------|
| **AND** | Both inputs inactive | No spikes (0) |
| | Only one input active | No spikes (0) |
| | Both inputs active | Spikes (1) |
| **OR** | Both inputs inactive | No spikes (0) |
| | Either input active | Spikes (1) |
| | Both inputs active | Spikes at a higher frequency (1) |
| **NOT** | Input inactive | Spikes (1) |
| | Input active (inhibitory pulse) | Spikes suppressed (0) |

- **AND:** the coupling resistors and load are chosen so a single input keeps the membrane voltage below threshold; both inputs together push it over.
- **OR:** lower coupling resistance means one input alone crosses the threshold.
- **NOT:** a constant bias keeps the neuron firing; an inhibitory input removes current and suppresses the spikes.

![AND gate](figures/04_and_gate.png)
*AND gate: output spikes only when both inputs are active.*

![OR gate](figures/05_or_gate.png)
*OR gate: either input produces output spikes; both inputs raise the spike rate.*

![NOT gate](figures/06_not_gate.png)
*NOT gate: an inhibitory input suppresses the spikes.*

## Comparison with TTL

TTL figures are taken from datasheets; VO₂ figures come from simulation. The comparison is qualitative and should be read with that caveat.

| Metric | VO₂ neuromorphic gates | TTL logic |
|--------|------------------------|-----------|
| Idle power | Much lower | Higher |
| Energy per operation | Much higher | Lower |
| Switching speed | Slower (hundreds of ns) | Faster (a few ns to tens of ns) |
| Robustness to parameter variation | Narrow stable window | More tolerant |

The VO₂ gates do **not** beat TTL on energy per operation or speed. Their advantage is in idle power and in showing that spike-based computation is possible with a single threshold-switching device.

## Limitations

- **Simulation only.** No hardware was built or measured.
- **Simplified device model.** Thermal effects, device-to-device variability and noise are not modelled.
- **Parameter-sensitive.** The gates fail if component values move outside a narrow window, and some margins are thin.
- **Single gates only.** Cascaded gates and larger networks were not tested.
- **TTL comparison uses datasheet values**, not a like-for-like simulation.

## Future work

- Add thermal and noise effects to the VO₂ model.
- Cascade gates and test small networks.
- Try an adaptive LIF neuron.
- Carry out a proper energy-per-operation analysis.

## Repository structure

```
.
├── README.md
├── figures/          # simulation plots and schematics used in this README
└── ltspice/          # (optional) .asc and .sub files, if shared
```

> The full dissertation report is not published here. It is **available on request**.

## Tools

- LTspice
- Yakopcic generalised memristor SPICE model

## References

1. C. Yakopcic, T. M. Taha, G. Subramanyam and R. E. Pino, "Generalized Memristive Device SPICE Model and its Application in Circuit Design," *IEEE Transactions on Computer-Aided Design of Integrated Circuits and Systems*, vol. 32, no. 8, pp. 1201–1214, 2013.
2. [Add the full citation for the Rziga et al. device-profile reference from your report.]
3. [Add any other references you want to credit, for example the VO₂ neuron and ECG classification work by Yuan et al., 2023.]

## Author

**Shravani Srivastava**
BSc (Hons) Electronics (Minor in Computer Science), ARSD College, University of Delhi
[LinkedIn link] · [GitHub link]

*Supervised by [supervisor name, optional].*
