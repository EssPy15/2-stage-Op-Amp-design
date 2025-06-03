# 2 stage Op-Amp 

# Objective
Design a 2-stage OTA in TSMC180nm technology.
TSMC 0.18μm technology parameters (taken from model file):
$V_{Tn} = 0.37V$; $V_{Tp} = 0.39V$; $μ_nC_{ox} = 230 μA/V^2$; $μ_pC_{ox} = 100 μA/V^2$; $V_{dd} = 1.8V$; $L_{min} = 0.18μm$; $W_{min} = 0.27μm$;

The designed opamp, should be used to make a non-inverting amplifier of gain $2$ The DC gain of the opamp should be at least $40dB$.

The circuit design for DC biasing is as follows:

![image](https://github.com/user-attachments/assets/5335cc00-c2f4-4e38-8b98-ba9a5cdc6914)

We now need to find the W/L parameters so that it will satisy the requirements. We take a $2\mu A$ current source for biasing the NMOS tail transistor to $20\mu A$. Thus the current through the NMOS and PMOS in the branch will recieve $10\mu A$. Assume overdrive of every transistor is $V_{ov}=100mV=V_{GS}-V_T$ and in saturation otherwise the transistors won't behave in the desired manner. Also assume a current of $10\mu A$ going through the 2nd stage. We use the equation $I_D = \frac{1}{2}\mu_{n/p}C_{ox}\frac{W}{L} (V_{ov})^2$ to calculate the $W/L$ of each transistor. 

The $W/L$ for each transistor turns out to be:
- $(W/L)_0 = 17.4 \implies L=900nm ,W=16000nm$
- $(W/L)_1 = 8.7 \implies L=1700nm ,W=15000nm$
- $(W/L)_2 = 8.7 \implies L=1700nm ,W=15000nm$
- $(W/L)_3 = 20 \implies L=1700nm ,W=34000nm$
- $(W/L)_4 = 20 \implies L=1700nm ,W=34000nm$
- $(W/L)_5 = 20 \implies L=900nm ,W=18000nm$
- $(W/L)_6 = 8.7 \implies L=900nm ,W=8000nm$
- $(W/L)_7 = 1.74 \implies L=900nm ,W=1600nm$

  We can calculate the $g_m$ of each transistor with $g_m=\frac{2I}{V_ov}$.

The $L$ for some transistors are more to increase the $r_o$ and gain of the system. These values was taken from simulation in LtSpice. With these parameters we get first stage gain $40db$ and overall gain after second stage $75db$. Power consumption $P = (\text{total I though all branhes}) \times V_{dd} = 53\mu W$. 

We now check for miller compensation for the design.
For non inverting part gain we take $R_f = R_1 = 100k\Omega$ for gain $= 2$.

![image](https://github.com/user-attachments/assets/07d637ed-e9e2-4fff-9590-bea3304c6119)

![image](https://github.com/user-attachments/assets/88f5fd49-e701-43cf-b0d1-513e199bfa91)

We assume $C = 0.3C_L$. Location of pole of the circuit $p_1=\frac{1}{(r_{o_1}\parallel r_{o_3})C(g_{m_5} (r_{o_5}\parallel r_{o_6}))} = \frac{1}{C} \times 2.862 \times 10^{-8}$. (values taken from prev calcs). As there is a zero in the transfer function. Location of zero = $\frac{-g_{m_6}}{C(Rg_{m_6}-1)}$. To make the contibution of zero less we send its location to infinity means $R=\frac{1}{g_{m_6}}=10k\Omega$. We want a single pole system for stability and so $p_2>>p_1$ which is true from the assumption. We change the value of capcitance and simulating in LtSpice we get $C=150pF, C_L=450pF$ for a stable op-amp with phase margin $\approx 58\deg$. 

