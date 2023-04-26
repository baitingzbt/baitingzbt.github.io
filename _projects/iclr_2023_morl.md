---
layout: page
title: Scaling Pareto-Efficient Decision Making via Offline Multi-Objective RL
description: Proposing offline MORL dataset and benchmark and offline MORL agents. Published in ICLR 2023.
img: assets/img/project_peda/peda.png
importance: 1
category: research
---

### Introduction

Reinforcement Learning (RL) agents are usually trained and evaluated on a fixed reward function. In this paper, however, we want to train agents with multiple objectives that cannot be simultaneously optimized, which is referred to as Multi-Objective Reinforcement Learning (MORL).

For instance, the two objectives for a MuJoCo Hopper is running faster and jumping higher, which cannot be achieved all at once under a physics simulation engine. More interestingly, notice the objectives are entangled: the Hopper cannot run without leaving the floor (jump), but jumping too high also makes running slower.


<!-- <div class="row">
    <div class="col-sm mt-3 mt-md-0">
    </div>
    <div class="col-sm mt-3 mt-md-0">
    </div>
    <div class="col-sm mt-3 mt-md-0">
    </div>
</div> -->

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/EWicckohj04" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</center>
<div class="caption">
    MORL example: Hopper needs to jump higher and run faster, which are two competing objectives cannot optimized simutaneously.
</div>

### D4MORL

In this paper, we aim solving MORL through a purely offline approach while there are no existing open-sourced benchmark or dataset. As a result, we first propose our own Dataset and Benchmark: **Data for MORL (D4MORL)**. For each environment, we have 6 variants as measured by any of 3 preference distributions 2 return distributions.

The preference distribution is measured by the empirical distribution of the samples. Specifically, we use 3 underlying distributions: **Uniform**, **Dirichlet-Wide**, and **Dirichlet-Narrow** which correspond to 3 levels of entropy: High, Medium, and Low. Through this design, we are able to examine whether agents can adapt to unseen and out-of-distribution preferences.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_peda/pref_dist.png" title="example image" class="img-fluid rounded z-depth-0.3" style="width:80%" %}
    </div>
</div>


On the other hand, we also design the Expert vs. Amateur distributions measured by trajectories’ return. In short, Experts are sampled from fully-trained behavioral policy while Amateurs are sampled from noisy behavioral policy.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_peda/return_dist.png" title="example image" class="img-fluid rounded z-depth-0.3" %}
    </div>
</div>

Leveraging the D4MORL dataset and benchmark, we move on to train the **Pareto-Efficient Decision Agents (PEDA)**, which are a family of offline MORL algorithms. Specifically, we build upon return-conditioned offline models including Decision Transformer (Chen et al, 2021) and RvS (Emmons et al, 2021) by adapting three simple yet very effective changes:

1) Concatenating preference as part of the input tokens; 2) Using preference-weighted return-to-go or avg-return-to-go; 3) Use a linear-regression model to find desired rtg from preference. Notice that 2) and 3) together help us find the correct return-to-go for each objective conditioned on the preference. In short, we should avoid naively setting the initial return-to-go as the highest seen value in the dataset, as this value wouldn't be achievable by our agent under a different preference.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
    </div>
    <div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/project_peda/lr.png" title="example image" class="img-fluid rounded z-depth-0.3" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
    </div>
</div>

### Experiments

In the chart below, PEDA trained on Amateur dataset outperforms the Amateur behavioral policy mesaured by Hypervolume, marking the success of learning.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_peda/exps.png" title="example image" class="img-fluid rounded z-depth-0.3" %}
    </div>
</div>

On the other hand, PEDA variants closely follow the conditioned return-to-go for each objective under all preferences.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project_peda/rtg_vs_achieved.png" title="example image" class="img-fluid rounded z-depth-0.3" %}
    </div>
</div>

### Demos

The Cheetah on the left prioritizes **running fast** while the Cheetah on the right prioritizes **saving energy**.

<iframe width="380" height="280" src="https://www.youtube.com/embed/kcL0zmteKeA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<iframe width="380" height="280" src="https://www.youtube.com/embed/XI16nrk5wDk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

--

The Ant on the left prioritizes **running horizontally** while the Ant on the right prioritizes **running vertically**.

<iframe width="380" height="240" src="https://www.youtube.com/embed/ywxyXmYXRfo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<iframe width="380" height="240" src="https://www.youtube.com/embed/ValIp8PG3cY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


### External Links

Paper: [https://openreview.net/forum?id=Ki4ocDm364](https://openreview.net/forum?id=Ki4ocDm364)

Github: [https://github.com/baitingzbt/PEDA](https://github.com/baitingzbt/PEDA)

Poster: [https://drive.google.com/file/d/1kiUYbYcfAdd8wLLK7x26NSYCqfWk6mGr/view](https://drive.google.com/file/d/1kiUYbYcfAdd8wLLK7x26NSYCqfWk6mGr/view)

Cite:
```
@inproceedings{
    zhu2023paretoefficient,
    title     = {Pareto-Efficient Decision Agents for Offline Multi-Objective Reinforcement Learning},
    author    = {Baiting Zhu and Meihua Dang and Aditya Grover},
    booktitle = {International Conference on Learning Representations},
    year      = {2023},
    url       = {https://openreview.net/forum?id=Ki4ocDm364}
}
```