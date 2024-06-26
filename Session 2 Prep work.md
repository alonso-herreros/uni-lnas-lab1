---
title: LNAS - Lab 1 S1 Preparatory Homework
---

<style>
:root {
    --markdown-font-family: "Times New Roman", Times, serif;
    --markdown-font-size: 10.5pt;
    --vscode-textBlockQuote-border: #9599e1;
}
</style>

<p class="supt1 center">Linear Networks Analysis and Synthesis</p>

# Lab 1 Session 2 - Relay activation/deactivation transient analysis

<p class="subt2 center">
Academic year 2023-2024
</p>
<p class="subt2 center">
Alonso Herreros Copete</br>
NIA: 100493990
</p>

---

<h2 class="center">
Preparatory Homework
</h2>

In this exercise we study the transient effect of the activation and deactivation of the coil present
in a relay. Let’s think of a microcontroller that activates/deactivates this relay by means of one
of its output pins, as shown in Figure 3.

![Microcontroller activates the relay at t=0](img/fig3.png)

<p class="caption center">
Figure 3: Microcontroller activates the relay at t = 0.
</p>

### Question 1

Consider that the switch in figure 3 has been open for a long time, and that the micro-controller closes it at
an instant we define as $t = 0$. Determine

* (a) $I_L(s)$: current through the coil in the Laplace domain for $t > 0$.

    > As the intial state is in reset, the equivalent cirucit in the Laplace domain doesn't require any
    > additional sources.
    >
    > ![Equivalent circuit in Laplace domain](img/fig_2.1.1.drawio.svg)
    >
    > The equation describing the circuit in Laplace domain for $t>0$ can be found using mesh analysis, where
    > $V_L(s) = sL I_L(s)$ and $V_g(s) = \mathcal{L}\left\{V_g\right\} = \frac{V_g}{s}$
    >
    > $$
    > \begin{aligned}
    >     I_L(s) &= \frac{V_g(s)}{R + R_g + sL} = \frac{V_g}{s(R + R_g + sL)} \\
    >     &= \frac{12}{100s + s^2} \text{ [A]}
    > \end{aligned}
    > $$

* (b) $I_s$: stationary value of the current reached after a long time $(t → ∞)$.

    > Taking a constant input voltage $V_g(s) = V_g$ and applying the final value theorem:
    >
    > $$
    > \begin{aligned}
    >     I_s &= \lim_{t → ∞} i_L(t) = \lim_{s → 0} sI_L(s) \\
    >     &= \lim_{s → 0} s \frac{V_g}{s(R + R_g + sL)} \\
    >     &= \frac{V_g}{R + R_g} \\
    >     &= 120 \text{ mA}
    > \end{aligned}
    > $$
    >
    > This means after a long enough time, the current is stabilized and the inductor behaves as a short
    > circuit, leaving only the two resistors contributing with their impedance.

* (c) $i_L(t)$: current through the coil in the time domain for $t > 0$.

    > We can find the time domain expression for the current through the coil by taking the inverse Laplace of
    > the expression found in the first question. First, we'll use Partial Fraction Decomposition to simplify
    > the expression.
    >
    > $$
    > I_L(s) = \frac{V_g}{s(R + R_g + sL)} = \frac{A}{s} + \frac{B}{(R + R_g + sL)}\\
    > $$
    >
    > Where
    >
    > $$
    > \begin{aligned}
    >     A &= \lim_{s→ 0} s \frac{V_g}{s(R + R_g + sL)} &&= \frac{V_g}{R + R_g} \\
    >     B &= \lim_{s→ -\frac{R+R_g}{L}} \frac{(R + R_g + sL) V_g}{s(R + R_g + sL)} &&= -\frac{LV_g}{R + R_g}
    > \end{aligned}
    > $$
    >
    > Leaving us with this expression.
    >
    > $$
    > I_L(s) = \frac{V_g}{s(R + R_g)} - \frac{L V_g}{(R + R_g + sL)(R + R_g)} \\
    > $$
    >
    > Then, we can adjust the expression to a suitable form and use the inverse Laplace transform of these
    > terms to find the time domain expression for the current.
    >
    > $$
    > \begin{aligned}
    >     i_L(t) &= \mathcal{L}^{-1}\left\{I_L(s)\right\} \\
    >     &= \mathcal{L}^{-1}\left\{\frac{V_g}{s(R + R_g)} - \frac{L V_g}{(R + R_g + sL)(R + R_g)}\right\} \\
    >     &= \frac{V_g}{R + R_g} \left( \mathcal{L}^{-1} \left\{\frac{1}{s}\right\}
    >         - \mathcal{L}^{-1} \left\{\frac{1}{(\frac{R + R_g}{L} + s)}\right\} \right) \\
    >     &= \frac{V_g}{R + R_g} \left( 1 - e^{-\frac{R + R_g}{L} t} \right) \\
    >     &= 120 \left( 1 - e^{-100 t} \right) \text{ [mA]}
    > \end{aligned}
    > $$

* (d) If the relay is triggered when the current flowing through the coil is 80 % of the final value $I_s$,
  how long does it take for the relay to trigger from the time the microcontroller activates it (switch is
  closed)?

    > This is a matter of finding the time $t_{80}$ that it takes for the current $i_L$ to reach $0.8 I_s$ in a the
    > studied scenario (switch is closed at $t = 0$). We can use the equations found in the preivous questions for
    > this.
    >
    > $$
    > \begin{aligned}
    >     && i_L(t_{80}) &= 0.8 I_s &⟹ \\
    >     &⟹& \frac{V_g}{R + R_g} \left( 1 - e^{-\frac{R + R_g}{L} t_{80}} \right) &= 0.8 \frac{V_g}{R + R_g} &⟹ \\
    >     &⟹& e^{-\frac{R+R_g}{L} t_{80}} &= 1 - 0.8 &⟹ \\
    >     &⟹& -\frac{R + R_g}{L} t_{80} &= \ln(0.2) &⟹ \\
    >     &⟹& t_{80} &= -\frac{L}{R + R_g} \ln(0.2) \\
    >     &&&= - \frac{1}{50+50} \ln(0.2) \\
    >     &&&= 16.09 ⋅ 10^{-3} \text{ [s]} \\
    > \end{aligned}
    > $$
    >
    > It would take the relay 16.09 ms to trigger in this scenario.

### Question 2

Consider now that the switch in figure 3 has been closed for a long time, and that the
microcontroller opens it (puts the output pin in the high-impedance state) at an instant
we redefine as $t = 0$.

Under these circumstances, calculate the voltage on the output pin in the Laplace domain,
$V_o(s)$, and in the time domain $v_o(t)$. Interpret the result and comment on whether you
foresee any problems in the microcontroller.

> By using the stabilized current $I_s$ obtained in the previous question, we can find an equivalent circuit
> in the Laplace domain, adding a current source in parallel with the inductor to account for the initial
> current flowing trough it, $i_L(0^-) = I_s$.
>
> ![Equivalent circuit in Laplace domain](img/fig_2.1.2.1.drawio.svg)
>
> Due to the high impedance state of the microcontroller pin acting as an open circuit, this can be further
> simplified:
>
> ![Simplified equivalent circuit in Laplace domain](img/fig_2.1.2.2.drawio.svg)
>
> Using node analysis, we can find the voltage on the output pin, $v_o(t)$
>
> $$
> \begin{aligned}
>     && \frac{I_s}{s} &= \frac{0-v_o(s)}{sL} &⟹ \\
>     &⟹& v_o(s) &= -I_s L \\
>     &&& = -120 ⋅ 10^{-3} \text{ [V]}
> \end{aligned}
> $$
>
> From here, using the inverse Laplace transform, we can find the time domain expression for the voltage on
> the output pin, $v_o(t)$
>
> $$
> \begin{aligned}
>     v_o(t) &= \mathcal{L}^{-1} \{-I_s L\} \\
>     &= -I_s L δ(t) \\
>     &= -120 ⋅ 10^{-3} δ(t) \text{ [V]}
> \end{aligned}
> $$
>
> This delta function in our time domain expression tells us that there will be an unbound voltage spike in
> the opposite direction as soon as the switch is opened. The initial value theorem proves this.
>
> $$
> \begin{aligned}
>     v_o(0^+) &= \lim_{t → 0^+} v_o(t) = \lim_{s → ∞} sV_o(s) \\
>     &= \lim_{s → ∞} -s I_s L \\
>     &= -∞ \text{ [V]}
> \end{aligned}
> $$
>
> This unbound
> voltage could potentially damage the controller. A capacitor may be added in parallel with the relay in
> order to limit the effect of this sudden (high-frequency change).

### Question 3

To avoid the possible problems mentioned in the previous point, a capacitor is connected
in parallel with the relay. $C_p = 200 μF$, as shown in figure 4. Assuming that the switch
has been closed for a long time before opening it at instant $t = 0$, determine:

![alt text](img/fig4.png)

<p class="caption center">
Figure 4: Microcontroller deactivates the relay at t = 0.
</p>

* (a) $I_L(s)$: current through the coil in the Laplace domain for $t > 0$.

    > The first step is to find our new stable *on* state taking the capacitor into account.
    >
    > Since the circuit is charged with DC for a long time ($ω = 0$) the **capacitor behaves as an open
    > circuit.**
    >
    > $$
    > i_C(0^-) = 0
    > $$
    >
    > Therefore, the current through the coil after a long time activated **is the same** as $I_s$ in the
    > previous sections.
    >
    > $$
    > i_L(0^-) = \frac{V_g}{R + R_g} = 120 \text{ mA}
    > $$
    >
    > Now, we'll find the condition of the capacitor after a long time, $v_C(0^-)$. This can be found by using
    > the previous stable currents.
    >
    > $$
    > v_C(0^-) = V_g - I_g R_g = V_g - (i_L(0^-)-i_C(0^-)) R_g = 6 \text{ V}
    > $$
    >
    > With this, we can create a new equivalent circuit in the Laplace domain, taking into account the
    > capacitor and using voltage sources as initial conditions in order to make mesh analysis easier.
    >
    > ![Equivalent circuit in Laplace domain](img/fig_2.1.3.1.drawio.svg)
    >
    > With this, we can analyze the circuit using KVL and obtain the expression for $I_L(s)$ for $t > 0$.
    >
    > $$
    > \begin{aligned}
    >     && \frac{v_C(0^-)}{s} + Li_L(0^-) &= I_L(s) \left(\frac{1}{sC_p} + sL + R\right) &⟹\\
    >     &⟹& I_L(s) &= \frac{\frac{v_C(0^-)}{s} + Li_L(0^-)}{\frac{1}{sC_p} + sL + R} \\
    >     &&&= \frac{v_C(0^-) + sLi_L(0^-)}{C_p^{-1} + sR + s^2L} \\
    >     &&&= \frac{6 + 0.12s}{5000 + 50s + s^2} \text{ [A]} \\
    > \end{aligned}
    > $$

* (b) $V_o(s)$: voltage on the microcontroller output pin in the Laplace domain for $t > 0$.

    > We can find the voltage in the Laplace domain by using the current found in the previous question and
    > the same equivalent circuit.
    >
    > $$
    > \begin{aligned}
    >     V_o(s) &= 0 + \frac{v_C(0^-)}{s} + \frac{I_L(s)}{sC_p} \\
    >     &= \frac{v_C(0^-)}{s} + \frac{v_C(0^-) + sLi_L(0^-)}{s\left(1 + sRC_p + s^2LC_p\right)} \\
    >     &= \frac{6}{s} + \frac{6 + 0.12 s}{s\left(1 + 0.01 s + 0.0002 s^2\right)} \\
    >     % &= \frac{12 + 0.18 s + 0.0012 s^2}{s\left(1 + 0.01 s + 0.0002 s^2\right)} \\
    > \end{aligned}
    > $$

* (c) $v_o(0+)$: voltage to be supported by the output pin of the microcontroller at $t = 0^+$. Has the
  problem discussed in section 2 been solved?

    > As the time point we care the most about is $t = 0^+$, we can find the voltage to be supported by the
    > output pin by using the initial value theorem.
    >
    > $$
    > \begin{aligned}
    >     v_o(0^+) &= \lim_{t → 0^+} v_o(t) = \lim_{s → ∞} sV_o(s) \\
    >     &= \lim_{s→ ∞} s \left(
    >         \frac{v_C(0^-)}{s}
    >         + \frac{v_C(0^-) + sLi_L(0^-)}{s\left(1 + sRC_p + s^2LC_p\right)}
    >     \right) \\
    >     &= v_C(0^-)
    > \end{aligned}
    > $$
    >
    > In this case, the voltage to be supported by the output pin is the same as the initial voltage on the
    > capacitor, unlike the previous scenario where the voltage was unbounded. This means that the power spike
    > problem has been solved by adding the capacitor in parallel with the relay. Instead of an unbounded
    > spike, the voltage will decrease and oscillate as it approaches 0.

* (d) In view of the equation for the current flowing in the coil, what can be concluded about its behavior?
  What would you change in the circuit to avoid this effect?

    > After analyzing the poles and zeros of the transient response (second-order), the system seems to be
    > underdamped, as both poles fall in the left half-plane, with negative real parts and non-zero imaginary
    > parts.
    >
    > The response is faster than that of a critically damped system, and the overshoot is not too dramatic on
    > the turning-on curve, so there is no risk of damaging the relay. The turn-off curve has a more
    > significant overshoot, but it may not be a problem either.
    >
    > In order to change this to a critically damped system with no overshoot at all, both poles must be
    > equal, real, and negative. This can be achieved without modifying the maximum current by adjusting the
    > capacitor value, which is present in the polynomial of the denominator of the transient reponse.
    >
    > $$
    > I_L(s) = \frac{v_C(0^-) + sLi_L(0^-)}{C_p^{-1} + sR + s^2L} \\
    > $$
    >
    > In the second-degree polynomial of the denominator to make the discriminant ($B^2 - 4AC$) in the quadratic equation equal to
    > zero.
    >
    > $$
    > R^2 - 4 L C_p^{-1} = 0 ⟺ C_p = \frac{4L}{R^2} = 1.6 \text{ mF}
    > $$
    >
    > However, this would require a big capacitor, and the response speed would be much lower than the one studied in these previous questions.
