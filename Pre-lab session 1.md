---
title: Lab 1 Report
---

<style>
:root {
    --markdown-font-family: "Times New Roman", Times, serif;
    --markdown-font-size: 10.5pt;
}
</style>

<p class="supt1 center">Linear Networks Analysis and Synthesis</p>

# Lab 1

<p class="subt2 center">
Academic year 2023-2024
</p>
<p class="subt2 center">
Alonso Herreros Copete
</p>

---

## 1.1 Preparatory Homework

The circuit in the figure 2 shows a well-known and widely used circuit often referred to as
*Sallen-Key*

<figure> <!-- Sallen-Key Circuit -->
    <img src="./img/fig2.png" alt="Sallen-Key Circuit">
    <figcaption>
        Figure 2: Sallen-Key Circuit
    </figcaption>
</figure>

1. In view of the circuit, estimate without calculating the transfer function the value of $V_o(ω)/V_g(ω)$ for
   $ω = 0$ and $ω = ∞$. Recall that the impedance of a capacitor depends on the angular frequency $ω$ of the
   signal.

   > We can use the fact that capacitors act as open circuits at $\omega=0$ and as short circuits at $\omega =
   > \infty$ to estimate the behavior of the circuit at these frequencies. The Operational Amplifier is in a
   > *voltage buffer* (also known as *voltage follower*) configuration, which has a gain of 1.
   >
   > At $\omega=0$, both capacitors act as **open cirucits**, resulting in the following equivalent circuit:
   >
   > ![alt text](img/fig_1.1.1.1.png)
   >
   > Having an ideal OpAmp, the currents at both input terminals are zero.
   >
   > $$
   > i_+ = i_- = 0 \implies v_+ = v_g
   > $$
   >
   > From the voltage follower behavior, we can conclude that $v_o(t) = v_g(t)$ when $\omega = 0$
   >
   > At $\omega=\infty$, both capacitors act as **short circuits**, resulting in the following equivalent
   > circuit.
   >
   > ![alt text](img/fig_1.1.1.2.png)
   >
   > Due to the shorting to ground at the positive OpAmp terminal, the input to the *voltage buffer* will
   > always be $0$.
   >
   > $$
   > v_+ = 0 \implies v_o = 0
   > $$
   >
   > This might seem contradicting with the same output node being connnected to the point between both $R$
   > resistors, but it's actually compatible: the voltage at this point is also 0, and no current flows
   > through the second $R$ resistors. All current going through the first $R$ resistor is drained through the
   > input of the ideal Operational Amplifier, which has 0 output impedance, keeping the voltage at $v_o$
   > equal to $0$. Therefore, the overall circuit gain is $0$ at $\omega = \infty$.

2. Assume that the circuit works in sinusoidal steady state and obtain the transfer function defined by the
   following ratio:

    $$
    H(ω) = \frac{V_o(ω)}{V_g(ω)}
    $$

3. Find the ratio between the square of the amplitudes of the input $v_g(t)$ and the output $v_o(t)$ for each
   angular frequency $ω$. That is:

    $$
    |H(ω)|^2 = \frac{|V_o(ω)|^2}{|V_g(ω)|^2}
    $$

4. Determine, as a function of $R$ and $C$, the angular frequency $ω$ in which the amplitude of the output
   signal $v_o(t)$ is 3 dB lower than the amplitude of the input signal $v_g(t)$. That is, to find the angular
   frequency $ω$ that verify

    $$
    |H(ω)|^2 = \frac{1}{2}
    $$

5. Calculate the current provided by the operational amplifier as a function of $V_g$, $R$, $C$, $R_L$ and
   $ω$.

6. The datasheet of the operational amplifier to be used specifies that the amplitude of the output current
   must always be less than 25 mA. If $R_L = 50 \Omega$, what is the maximum value that the input signal
   amplitude can take in order not to exceed this margin when $ω \approx 0$?
  
**NOTE:** It is recommended to check with the simulator the results obtained for a particular election of
$R$ and $C$
