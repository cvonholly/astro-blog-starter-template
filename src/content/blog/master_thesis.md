---
title: "Stabilizing the Power Grid: How I Used Game Theory to ensure cooperation in Dynamic Virtual Power Plants"
description: "My Master Thesis explored how dynamic ancillary services will be provided in future power grids."
pubDate: "Mar 27 2026"
heroImage: "/cover_MA.png"
---
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">

*This blog post is a shortened and more blog-like version of my Master Thesis. The full text Master Thesis with all the formulas and proofs is available [here](https://drive.google.com/file/d/1pCPoJt1C8YLEp0EDTyXuijjh6lYNsOHB/view?usp=sharing).*

The global transition toward renewable energy is moving forward for good economic and environmental reasons, but during my master's research, I focused on two significant technical and economic challenges it presents. First, as synchronous power plants get replaced with distributed energy resources, power grids have a lower inertia, meaning faults lead to larger instantaneous frequency deviations. Second, the market value of renewables (e.g., wind and solar PV) is decreasing as their share of production grows.

![Figure from Ahmadyar et al., 2018, shows decreasing inertia.](../../assets/latex_praesentation/pics/ahmad11-2773091-large.png)
*Figure from Ahmadyar et al., 2018, shows decreasing inertia.*
![Figure from Hirth, 2013, displays decreasing market value of Solar PV.](../../assets/latex_praesentation/pics/solar_decreasing_val.jpg)
*Figure from Hirth, 2013, displays decreasing market value of Solar PV.*

The proposed technical solution lies in cooperative aggregation to provide services that boost the system inertia. These services are called *Dynamic Ancillary Services (DAS)*. By networking heterogeneous Distributed Energy Resources (DERs) together, we can create a Dynamic Virtual Power Plant (DVPP) which can provide exactly these services. Together, the DERs leverage their complementary technical capabilities.

For this proposal to work, there are two main requirements:
1. Technical feasibility (a collection of
DERs is able to provide the DAS)
2. Economic & Cooperative feasibility (collective DAS provision is beneficial
for every DER)

The main challenge is to ensure that the DERs are capable of DAS provision but also ensure economic cooperation.

### Dynamic Ancillary Service (DAS)

DAS are characterized by *fast* ancillary services, such as Fast Frequency Reserve (FFR), Frequency Containment Reserve for Disturbances (FCR-D), and voltage control. These operate in multiple countries with high penetrations of renewables, such as Australia, Ireland and the Nordic Countries in Europe. Individual DERs are often ineffective in providing DAS. However, a specific aggregation of DERs named a Dynamic Virtual Power Plant (DVPP) can provide DAS [as shown by previous research.](https://ieeexplore.ieee.org/abstract/document/9667320/) Below are the requirement curves for Fingrid's FFR service, employed in this thesis.

![blank](../../assets/latex_praesentation/pics/ffr_fingrid_bigger_font.png)

## Dynamic Virtual Power Plant (DVPP) Control Design

The goal is to provide DAS with a set of DERs. Given an activation signal $\Delta f(s)$, we formulate the desired power deviation $T_\mathrm{des}(s)$ and split the desired transfer function into local transfer functions $T_i(s)$:
$$\Delta p_{\mathrm{des}}=T_\mathrm{des}(s)\Delta f(s)$$
and
$$\Delta p_{i} (s)= T_i(s) \Delta f(s).$$
We use a DER-specific Adaptive Dynamic Participation Factor (ADPF) $m_i(s)$ to encode each DER's role in the DVPP:
$$T_i(s) = m_i(s) \cdot T_\mathrm{des}(s), \quad\forall i\in \mathcal{D}$$
The ADPF roles may for example include Low-pass, Band-pass, and High-pass filters. It is important to match the desired transfer function:
$$\sum_{i\in\mathcal{D}}m_i(s) \overset{!}{=}1$$

![Low Pass Filter](../../assets/latex_praesentation/pics/Low%20Pass%20Filter%20$m_i$.png)
![Band Pass Filter](../../assets/latex_praesentation/pics/Band%20Pass%20Filter%20$m_i$.png)
![High Pass Filter](../../assets/latex_praesentation/pics/High%20Pass%20Filter%20$m_i$.png)

## DVPP Control Design in Action

The DVPP studied in this Thesis has the following control design:

![blank](../../assets/latex_praesentation/pics/threeblocks.png)

<!-- <video width="640" height="480" controls>
  <source src="/videos/ANI_simulation_data_FFR_+_FCR-D_PV+Wind+BESS_on_12_4_2025_at_11_.mp4" type="video/mp4"> -->
<!-- </video> -->
<iframe width="100%" height="400" src="https://youtu.be/p9uhmy2yA6w" title="Simulation Video" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Timeline

Now we move to a market setting where the previously presented control design is integrated. First, we have to quantify the worth of a DVPP in the market. Consider a set of DERs $\mathcal{N}$ and a DVPP $\mathcal{D}$ operating in the Dynamic Ancillary Service Market:

![blank](../../assets/latex_praesentation/pics/timeline_r3.png)

where $R_\gamma(\mathcal{D},b) = b(\mathcal{D})\cdot[\pi \gamma_{\mathcal{D}}(b) - q (1-\gamma_{\mathcal{D}}(b))]$ is the (forecasted) probabilistic reward (the reward $\mathcal{D}$ can expect when placing a bid $b$) with the pass probability $\gamma_{\mathcal{D}}(b)$, $b^*(\mathcal{D})\leftarrow \arg\max_{b} R_\gamma(\mathcal{D},b)$ is the optimal bid placed in the market, $h(\mathcal{D},b) \xrightarrow{\text{simulate bid activation}} \{\text{fail, pass}\}$ evaluates whether the DVPP is successful delivering the bid and
$$R(\mathcal{D}, b) = \begin{cases} \pi\cdot b(\mathcal{D}), & \text{if } h(\mathcal{D},b) = \text{pass}\\ -q\cdot b(\mathcal{D}) & \text{if } h(\mathcal{D},b) = \text{fail} \end{cases}$$
is the realized reward, where $\pi$ is the price of the DAS and $q$ the penalty of the DAS.

### Quantify Value of DERs

Now we move to the Game Theory part of the cooperation by asking: How much is each set of DERs (coalitions $S$) worth? I introduce the **Forecasted** and **Realized Value** to quantify this:
$$v_\mathbb{F}(\mathcal{S}) = \sum_{\mathcal{D}\in P^*(\mathcal{S})} R_\gamma(\mathcal{D}, b^*(\mathcal{D})$$
$$v_\mathcal{R}(\mathcal{S})= \sum_{\mathcal{D}\in P^*(\mathcal{S})} R(\mathcal{D}, b^*(\mathcal{D}))$$
where $P^*(\mathcal{S})$ is the optimal partition of coalition $\mathcal{S}$.

Now we have all the ingredients to formulate a reward allocation based on the following timeline:

![blank](../../assets/latex_praesentation/pics/timeline_r4.png)

### Cooperation Criteria

To define a suitable Reward Allocation
$$A^*(\mathcal{N}):v_\mathcal{R}(\mathcal{N})\rightarrow[x_i]_{i\in\mathcal{N}}$$
we introduce a crucial set of cooperation criteria:
* **C1: Rationality & Coalitional Stability**
* **C2: Bayesian Incentive Compatibility**
* **C3: Optimality & Feasibility**
* **C4: Fairness**
* **C5: Ex-Post Consistency**

C1 is the most important criteria: it gurantees that every DER is best of in the grand coalition (all DERs togehter), thus has no incentive to form sub-coalitions or bid alone. C2 means that every DER reveals their private information truthfully, assuming that the other DERs also act truthfully. Optimiality and Feasibility (C3) simply indicate that the operation of the DVPP is physically implementable and that it bids optimally. 

Next up, Although Fairness (C4) is sometimes perceived as a subjective and debated concept, there are attempts to define Fairness objectively. Oftentimes, fair outcomes lie between equality and optimality (Hall et al., 2025). There exists a rich literature on Fairness concepts and measures (Soares et al., 2024; Ostmann & Meinhardt, 2008; de Clippel & Rozen, 2022). In this work, we adopt the first principles presented in the GT foundations in van den Brink (2002) and the application of Fairness concepts for DERs in Cuenca et al. (2023). Cuenca et al. (2023) compares various outcomes of DERs installation in the grid, depending on the deployment policy of DERs. They define a fair outcome as an equity criterion: a fair allocation ensures that individual effort is proportionally rewarded, given an equal opportunity to participate (a level playing ground). Translating this definition to Cooperative Game Theory, an allocation is fair if it fulfills the balanced contributions axiom.

Finally, Ex-Post Consistency is the relation of Forecasted and Realized Reward: if they align, up to a bias factor of the forecast, I call the game **Ex-Post Consistent**.

### Proposed Reward Allocation

Now that both the cooperation criteria and the market operation of the DVPP are established, the proposed reward allocation can be presented:

![blank](../../assets/latex_praesentation/pics/proposed_solution.png)

To satisfy the largest set of criteria possible, two methods were employed in my Thesis.

* **The Shapley Value:** In the thesis, I show that the Shapley Value leads to conditions C1 to C5 being satisfied, if the value function is "convex". Convexity implies that the DERs have a positive network effect, meaning the marginal returns increase as the set of DERs grows. The Shapley Value distributes the reward based on the average marginal contribution of each DER. It ensures that individual effort is proportionally rewarded on a level playing field, satisfying the condition of Fairness. 
* **The Nucleolus:** Sometimes, the convexity property is not fulfilled, for example when the risk preferences of the DERs is significantly different. In this case, the Shapley Value cannot guarantee the most important critera, Colaitional Stability (C1), and the Nucleolus is applied. The Nucleolus always fulfills criteria C1 to C3, but not nesceccarily C4 and C5. It minimizes the maximum dissatisfaction for all DERs, such that no DER wants to leave the cooperation. It calculates the payout by lexicographically maximizing the excess of the most dissatisfied coalition. While it sacrifices some linearity (C5) and Fairness (C4) compared to the Shapley Value, it ensures coalitional stability by proimising that no DER is incentivized to leave the grand coalition.

### The Real-World Test: The Finnish Grid

To prove this framework practically, I simulated its operation on the Finnish power grid, utilizing historical data for their fast frequency reserve markets. 

My simulated DVPP included:
* A 21.6 MW Wind power plant.
* A 15 MW Solar PV plant.
* A 15 MW Battery Energy Storage System (BESS).

Operating a DVPP in this market is high-stakes; failing to deliver the promised capacity results in a severe penalty that is three times the potential reward.

### The Results: Better Together

The simulation proved my hypothesis: cooperation is highly effective. Operating together as a DVPP yielded 16% higher revenue compared to the DERs operating completely on their own:

![blank](../../assets/latex_praesentation/pics/average_forecasted_and_realized_rewards.png)

The BESS emerged as the most significant beneficiary of this teamwork. Because of its extremely fast response times, it earned a reward that exceeded its standalone value by 40%. Meanwhile, the solar and wind plants benefited considerably from the battery compensating for production shortfalls when the wind died down or clouds rolled in, saving them from massive financial penalties.