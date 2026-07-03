# DWave Benchmarks

Let's follow this chat: https://claude.ai/chat/4480d201-c5e6-49a1-95eb-2ea68605bb82

Here we replicate the key benchmarks and error metrics suggested in the [docs](https://docs.dwavequantum.com/en/latest/quantum_research/errors.html).

## Integrated Control Errors

These relate to the difference between the $h$ and $J$ we request, and those that are actually implemented. That is, we send $h_i\to h_i+\delta h_i$ and $J_{ij}\to J_{ij}+\delta J_{ij}$. There is a lot of interconnection: next-nearest-neighbours and their biases and couplings all matter. The overall $\delta h, \delta J$ are Gaussian distributions across all qubits, pairs, and bias and coupling values. The means and variances of these distribution vary with the anneal parameter, and DWave calibrate so that there is a point during the anneal at which $\mu=0$, to balance out.

Sources of ICE include
- NNN interactions (background susceptibility)
- $1/f$ flux noise (time- and s-varying)
- DAC quantisation
- I/O system effects
- inter-qubit differences

Details of all of these are on the website. 

We will first measure $1/f$ noise, then the $\delta h, \delta J$ distributions.

### $1/f$ noise

We set all $h=J=0$ and look at how the final qubit distributions drift over time. This lets us measure the effective $1/f$ noise when $s=s_q^*$, i.e. the qubit freezeout point. To probe at earlier times, we need to create "clusters of strongly coupled qubits and measure the time-dependent behavior of the net magnetization of system". That is, we pick two sets of physical qubits and give them all the same $h$ within each cluster. We then add an inter-cluster coupling $J$. This gives us an effective two-spin instance we can use. 