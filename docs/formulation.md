# Formulation

## Phase-field Model of Insulator-Metal Transitions

The total free energy of a system in the most general form is given by,

$$F[\psi(\mathbf{r}, t), \eta(\mathbf{r}, t), n(\mathbf{r}, t), p(\mathbf{r}, t), n_{d}(\mathbf{r}, t), n_{d}'(\mathbf{r}, t), T(\mathbf{r}, t), \phi(\mathbf{r}, t), \mathbf{u}(\mathbf{r}, t)] = \int d^{3}r[f_{GL}(T,\psi,\eta) + f_{eh}(T,\phi,\psi,n,p) + f_{def}(T,\phi,n_{d},n_{d}') + f_{ela}(\textbf{u}, \psi, \eta, n_{d}, n_{d}')$$

where 
Variable       | Description
---------- | -----------
$\psi(\textbf{r}, t)$  | Electronic order parameter
$\eta(\textbf{r}, t)$  | Structural order parameter
$n(\textbf{r}, t)$  | Free-electron density
$p(\textbf{r}, t)$  | Free-hole density
$n_{d}(\textbf{r}, t)$  | Neutral defect density
$n_{d}'(\textbf{r}, t)$  | Ionized defect density
$T(\textbf{r}, t)$  | Temperature
$\phi(\textbf{r}, t) | Electric potential
$\textbf{u}(\textbf{r}, t) | Mechanical displacement

The various free energy density components are functions of the aforementioned variables and can be defined as follows:
- Ginzburg-Landau Potential
	- $f_{GL} = \dfrac{a_{1}}{2}\dfrac{T - T_{1}}{T_{1}}\psi^{2} + \dfrac{b_{1}}{4}\psi^{4} + \dfrac{c_{1}}{6}\psi^{6} + \dfrac{a_{2}}{2}\dfrac{T-T_{2}}{T_{2}}\eta^{2}+\dfrac{b_{2}}{4}\eta^{4}+\dfrac{c_{2}}{6}\eta^{6} + \dfrac{g_{2}}{2}\psi^{2}\eta^{2} + \dfrac{G_{1}}{2}(\nabla\psi)^{2} + \dfrac{G_{2}}{2}(\nabla\eta)^{2} + \ldots $
- Charge-carrier free energy density
	- $f_{eh} = \dfrac{E_{g}(\psi)}{2}(n+p) + \phi e(p - n) + k_{B}T\bigg[\int_{0}^{n}G_{1/2}^{-1}\bigg(\dfrac{x}{N_{C}}\bigg)dx + \int_{0}^{p}G_{1/2}^{-1}\bigg(\dfrac{x}{N_{V}}\bigg)dx\bigg] - F_{eh, 0}(T,\psi)$
- Defect free energy density
	- $f_{def} = \epsilon_{f}(n_{d} + n_{d}') + \epsilon_{d}\nu_{d}n_{d} + e\nu_{d}n_{d}\phi + k_{B}T\bigg[n_{d}ln\dfrac{n_{d}}{N_{d}} + n_{d}'ln\dfrac{n_{d}'}{N_{d}}+(N_{d}-n_{d}-n_{d}')ln\bigg(1-\dfrac{n_{d}+n_{d}'}{N_{d}}\bigg)\bigg]$
- Elastic energy
	- $f_{ela} = \dfrac{1}{2}C_{ijkl}(\varepsilon_{ij} - \varepsilon_{ij}^{0} - n_{d}\Lambda\delta_{ij} - n_{d}'\Lambda'\delta_{ij})(\varepsilon_{kl} - \varepsilon_{kl}^{0} - n_{d}\Lambda\delta_{kl} - n_{d}'\Lambda'\delta_{kl})$

## Evolution Equations
- Time-dependent Ginzburg-Landau-like relaxational equation for order-parameters,

$$\gamma_{\psi}\dfrac{\partial\psi}{\partial t} = - \dfrac{\delta F}{\delta\psi},$$
$$\gamma_{\eta}\dfrac{\partial\eta}{\partial t} = -\dfrac{\delta F}{\delta\eta}$$

- Carrier transport,

$$\dfrac{\partial n}{\partial t} - \nabla\cdot\bigg(\dfrac{nM_{e}}{e}\nabla\dfrac{\delta F}{\delta n}\bigg) = S_{eh} + \Theta(\nu_{d})S,$$
$$\dfrac{\partial p}{\partial t} - \nabla\cdot\bigg(\dfrac{pM_{h}}{e}\nabla\dfrac{\delta F}{\delta p}\bigg) = S_{eh} + \Theta(-\nu_{d})S$$

- Defect transport,

$$\dfrac{\partial n_{d}}{\partial t} - \nabla\cdot\bigg(\dfrac{n_{d}D}{k_{B}T}\nabla\dfrac{\delta F}{\delta n_{d}}\bigg) = -S,$$
$$\dfrac{\partial n_{d}'}{\partial t} - \nabla\cdot\bigg(\dfrac{n_{d}'D'}{k_{B}T}\nabla\dfrac{\delta F}{\delta n_{d}'}\bigg) = S$$

- Gauss' law,

$$-\nabla\cdot(\epsilon_{b}\nabla\phi) = e(\nu_{d}n_{d}' - n + p)$$

- Heat transport

$$\dfrac{\partial H}{\partial t} - \nabla\cdot(\alpha\nabla T) = \textbf{J}\cdot(enM_{e} + epM_{h})^{-1}\cdot\textbf{J}$$
$$H \simeq C_{v}T + f_{GL} - T\dfrac{\partial f_{GL}}{\partial T}$$

- Mechanical force balance

$$\sum_{jkl}\nabla_{j}[C_{ijkl}(\varepsilon_{kl} - \varepsilon_{kl}^{0} - n_{d}\Lambda\delta_{kl} - n_{d}'\Lambda'\delta_{kl})] = 0$$

- Defect-carrier reaction

$$S =
\begin{cases}
\Gamma(Kn_{d} - n_{d}'n^{\nu_{d}}, & \quad \nu_{d} > 0\\ 
\Gamma(Kn_{d} - n_{d}'p^{|\nu_{d}|}, & \quad \nu_{d} < 0
\end{cases}$$

- Carrier recombination

$$ S_{eh} = \Gamma_{eh}(n_{eq}p_{eq} - np) $$

## List of physical parameters
Parameter       | Description
---------- | -----------
$a_{1}, b_{1}, c_{1}, T_{1}, a_{2}, b_{2}, c_{2}, T_{2}, g_{1}, g_{2}, G_{1}, G_{2}$  | Landau coefficients
$\gamma_{1}, \gamma_{2}$  | Kinetic coefficients for relaxational equations
$\epsilon_{b}$ | Background dielectric constant
$\Delta$ | Bandgap coefficient
$E_{C}, N_{c}$ | Conduction band top and effective density of states
$E_{v}, N_{v}$ | Valence band bottom and effective density of states
$M_{e}, M_{h}$ | Electron and hole mobilities
$\Gamma_{eh}$ | Carrier recombination rate
$\epsilon_{f}$ | Ionized defect formation energy
$\Lambda, \Lambda'$ | Neutral and ionized defect volume
$\nu_{d}$ | Defect valence
$\epsilon_{d}$ | Defect electronic level
$N_{d}$ | Defect maximum concentration
$D, D'$ | Ionized and neutral defect diffusion coefficients
$\Gamma$ | Defect-carrier reaction rate
$C_{v}$ | Volumetric heat capacity
$C_{ijkl}$ | Elastic stiffness tensor
