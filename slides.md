---
theme: seriph
#theme: geist
#theme: academic
highlighter: prism
lineNumbers: false
#themeConfig:
#  primary: "#5d8392"
aspectRatio: "8/5"
---

# Introduction to 3D Computer Vision & Graphics with Neural Fields

## @ ML-Phys 2022/7/31

<br>

#### version: 0.1

---

# Introduction: Novel View Synthesis

Novel view systhesis is a task to synthesize a photorealistic image with a new camera pose from given source images and their camera poses.

<center>
<img class="w-120" src="https://shaohua0116.github.io/Multiview2Novelview/img/illustration.png" />
</center>

---

# Introduction : NeRF

Recently, there have been proposed remarkable methods with machine learning to solve it. One of the most impressive one is "NeRF".
Here are demo results from it:

<table><tr><td>
<video width="250" src="http://cseweb.ucsd.edu/~viscomp/projects/LF/papers/ECCV20/nerf/website_renders/orchid.mp4" loop controls></video>
</td><td>
<video width="250" src="http://cseweb.ucsd.edu/~viscomp/projects/LF/papers/ECCV20/nerf/website_renders/pecanpie_200k_rgb.mp4" loop controls></video>
</td><td>
<video width="250" src="http://cseweb.ucsd.edu/~viscomp/projects/LF/papers/ECCV20/nerf/website_renders/redtoyota.mp4" loop controls></video>
<!-- possible video-attr: controls -->
</td></tr>
</table>

cited from https://www.matthewtancik.com/nerf

<!--
layout: iframe

url: https://www.matthewtancik.com/nerf
-->

<!--

# NeRF: 3D mesh

<iframe title="アボガドのウィキペディアページ" src="https://www.matthewtancik.com/nerf"></iframe>

<iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="300"
    height="200"
    src="https://www.openstreetmap.org/export/embed.html?bbox=-0.004017949104309083%2C51.47612752641776%2C0.00030577182769775396%2C51.478569861898606&layer=mapnik">
</iframe>
-->

---

# View-Dependent Appearance

The brightness of any object surface usually depends on the view angle.
NeRF can realize such a view-dependent appearance effect naturally.

<video width="500" src="http://cseweb.ucsd.edu/~viscomp/projects/LF/papers/ECCV20/nerf/website_renders/viewdirs_website_stove.mp4" loop controls></video>
cited from https://www.matthewtancik.com/nerf

---

# Other Results in recent developments

## Block-NeRF

* 2.8 million images in total
* use multiple NeRF models ( put on each crossing )

<table>
<tr><td>
  <video width="400" src="https://storage.googleapis.com/waymo-uploads/files/site-media/research/waymo_block_nerf_grace_cathedral.webm" loop controls></video>
Grace Cathedral 
</td><td>
  <video width="400" src="https://storage.googleapis.com/waymo-uploads/files/site-media/research/waymo_block_nerf_lombard.webm" loop controls></video>
Lombard Street
</td></tr>
</table>

---

## NeRF in the Wild

<table>
<tr><td>
  <video width="400" src="https://storage.googleapis.com/nerf-w-public/videos/trevi/flythrough-app3.mp4" loop controls></video>
  Trevi Fountain
</td><td>
  <video width="400" src="https://storage.googleapis.com/nerf-w-public/videos/sacre/flythrough_v3.webm" loop controls></video>
  Sacre Coeur
</td></tr>
</table>




<!--
360 scene
<video width="500" src="http://cseweb.ucsd.edu/~viscomp/projects/LF/papers/ECCV20/nerf/website_renders/website_360.mp4"></video>
-->

---

# Observation map

In many settings, data is abstarctly considered as an observation of some state.

Let's say that we can observe data from some state in a given setting and represent this as a map:

$$
\Pi: \underset{\rm state}{\mathscr{S}} \times \underset{\substack{\rm measurement \\ \rm parameters}}{\mathscr{P}} \rightarrow \underset{\rm observables}{\mathscr{O}}
$$

Here we assume the following properties:

- deterministic _i.e._ there happen no noises, fluctuations or unknown/hidden states.

  - It is possible to extend it to the probabilistic map. In this case, the target domain is the set of the probability over observables.

- differentiable w.r.t. $\mathscr{S}$.
  - Here, it implies both $\mathscr{S}$ is equipped with some distance and some differentials are defined.
  - We do not asssume that it is differentiable w.r.t. $\mathscr{P}$ but, in many cases, so it is.

In ref [1], the map, state and observables are called "forward map", "reconstruction domain", "sensor domain" respectively.

<!-- link to ref -->

<footer class="absolute bottom-0 left-0 right-0 p-3">
<hr><br>
[1]: "Neural Fields in Visual Computing and Beyond", https://arxiv.org/abs/2111.11426
</footer>

<!--

## Examples

- Classical mechanics

- Qunatum mechanics

- State Space Model
-->

---

# Inverse problem

Let us assume:
- The observation map $\Pi$ is known and can be computed by ourselves.
- A state $s$ is fixed but unknown.

In this setup, we can obtain a collection of observations:
$$
\mathcal{D} := \{ o_1 = \Pi(s, p_1), o_2 = \Pi(s, p_2), \ldots, o_n = \Pi(s, p_n) | s \in \mathscr{S}, \{p_i\} \in \mathscr{P}^n \}
$$
where each $p_i$ can be either known or unknown here.

Now, to generate a totally new sample for a given measurement parameter $p_{\rm novel}$, it is enough to specify the state $s$ since we can evaluate $o_{\rm novel} = \Pi(s,p_{\rm novel})$. This is the inverse problem in term of the data generation process:
- construct or specify $s \in \mathscr{S}$ from some observabed data $\mathcal{D} = \Pi(s, \{p_i\}_i) \subset \mathscr{O}$
<!-- consider styling of the last line -->

---

# What corresponds in the 3D world vision / graphics ?

- <span><font color="blue">state</font></span> $\color{blue} \mathscr{S}$ : <font color="blue">scene</font>
  - all the information related to the perception / view to model the 3D world vision
  - still abstract but usually corresponds
    - geometrical objects (location, pose, shape, textures) and lighting condition
- <span><font color="orange">measurement parameters</font></span> $\color{orange} \mathscr{P}$ : <font color="orange">viewpoint setup (camera paramters)</font>
  - camera's geometrical setting called extrinsic paramaters: location, pose (free from hardware)
  - camera's features from hardware called intrinsic parameters: focus, field of view, lens distorsion
- <span><font color="green">observables</font></span> $\color{green} \mathscr{O}$ : <font color="green">2D view image</font>
  - RGB pixel images in digital setting
- <span><font color="red">observation map</font></span> $\color{red} \Pi$ : <font color="red">rendering</font>
  - generate the view image from the camera and a given scene

<center>
<img class="w-120" src="/3DCV_observation_process.png" />
</center>

<!--
<div class="grid grid-cols-[65%,35%] gap-4"><div>
</div><div>
</div></div>
-->

---

# Inverse Problem = Novel View Synthesis

The inverse problem of the observation process whose goal is to infer the scene $s$ and predict a new image $o_{\rm novel}$ for any novel parameter $p_{\rm novel}$
reduces to the novel view synthesis itself.

Next, we dive into two concepts:

* scene: how we efficiently represent the 3D world ?
* rendering: how we can generate the 2D view image from the scene above ?

---

# Scene representation with radiance and density

In many cases, scene can be represented as the radiance values and the density associated to each point in the 3D space.

- radiance:
  - How much and what kind of light (color) come from
- density:
  - How much matter exist
  - Higher the value is, less the opacity is

Notive that, this does not mean the actual light emission happens there but just show the final state aafter taking the complex physical light transport process.

---

# Examples

### Eg. Voxel

This is a natural 3D extension of pixel which is a map from a 2D grid to a color value such as RGB.

- As mathematical object: It is a map from a grid/lattice into color & density/opacity.
- As data structure: 4D array with shape (H,W,D,C) where H: # of height grid, W: # of width grid, D: # of depth grid and C: # of color channels.

### Eg. Radiance field of the Lambert reflectance model

- As mathematical object: It is a map from space such as $\mathbb{R}^3$ into color & density/opacity.
- As data structure: There are many ways to represent but the most powerful one is the Neural Network representation.

---

# View dependence

The previous model cannot represent the "reflectance" nor "refraction".

For example, let us consider a small metal with smooth surface and a illumination.
From some viewpoint, it looks so bright because much light is reflected to that direction.
However, if you move around, it becomes darker.

These kind of phenomena are translated as the radiance value depends not only on the space point but also on the direction in the radiance field.


<!--
If we look it from the sun (light source), it looks like a yellow disk with many craters on it.
However, when we go to the opposite side of the sun, it is a dark disk because of the shadow.

Based on our experience or this kind of the observation, we can say the view is dependent on the direction.
-->

---

# State as Field

As we have seen, in many cases, the state space can be specified by some fields which are maps from an underlying manifold to some values.

Let us consider an underlying manifold $\mathcal{M}$ and some value set $\mathcal{V}$. Then, field is an element of the map from $\mathcal{M}$ to $\mathcal{V}$ (satisfying some property called $F$). With the formula expression,

$$
\Phi \in \mathscr{F} = {\rm Map}^F(\mathcal{M}, \mathcal{V}).
$$

In particular, we assume the field $\Phi$ is approximately represented by a neural model (parametric model) $\Psi_\theta$ with a paramters $\theta \in \Theta$.
We call such a neural model "a neural field".

### 3D Computer Vision / Graphics Case (NeRF-family)

Naively speaking, the underlying manifold is the 3D space itself $\mathcal{M} = \mathbb{R}^3$ and the value set includes both the radiance map from the direction to the color and the density.
By introducing the color set $\mathcal{C} \simeq [0,1]^3$ and the density set $\mathcal{D} \simeq \mathbb{R}_{\ge 0}$, the value set is reprsented as $\mathcal{V} = {\rm Map}(S^2, \mathcal{C}) \times \mathcal{D}$ where $S^2$ corresponds to the view direction in the 3D space.

However, for the ease to handle the map with some neural network model, it is not good that the value set includes a map because it is hard to parametrize that.
For that purpose, we uncurry the radiance part, _i.e._ we use

$$
\mathcal{M} = \mathbb{R}^3 \times S^2
\qquad
\mathcal{V} = \mathcal{C} \times \mathcal{D}.
$$

---

# Other representations

The previous voxel or radiance field is called the volume representation.
<!-- This is because the object-related value is assigned to each point of 3D space. -->

## Eg. Sign distance functions

* underlying manifold: $\mathcal{M} = \mathbb{R}^3_{\rm space}$
* value set: $\mathcal{V} = \mathbb{R}$
  * the distance from the objects' surfaces whose sign corresponds to the outer (+) / inner (-) of objects

## Eg. Light Field

* underlying manifold: $\mathcal{M} = Gr(\mathbb{R}^4, 2)$ (dimension is 4)
  * all the rays in $\mathbb{R}^3$
* value set: $\mathcal{V} = \mathcal{C}$
  * the radiance value of the ray enough distant from objects

---

# Positional Encoding

In general, neural networks harder to capture higher frequency modes than to lower modes.
To handle these high frequency modes, one idea is to consider the neural networks on the spectral domain _i.e._ the Fourier modes.

In other words,

- $\Phi$ on the some spectral domain

is equivalent to

- $\Phi \circ \gamma$ on the original 3D space

where $\gamma$ is the Fourier transformation.

$$
\gamma(x) = ( \{ \sin(k \pi x) \}_K, \{ \cos(k \pi x) \}_K )
$$
for some $K \subset \mathbb{R}_{\ge 0}$.

One usual combination recently used is
$
K = \{ 2^0, 2^1, 2^2, \ldots, 2^{L-1} \}
$ for some $L \in \mathbb{N}$.

---

# Beer-Lambert law

Now, a scene $s$ is given by a radiance field $\Phi$ or its neural model $\Psi_\theta$ in the machine learning context.
Next, we need to generate view images once this scene is given as a radiance field whose process is called volume rendering.

At the volume rendering, the physical model behind is the Beer-Lambert law.
This describes the relation between the radiance change $\Delta R$ and the distance $\Delta x$ by

$$
\Delta R = c(x) \sigma(x) \Delta x
$$
where $\sigma(x)$ is a density at $x$ and $c(x)$ is a radiance coefficient depending on the matter at $x$.

Notice that the density and the radiance coefficient is the components of the value of the radiance field.
$$
\Phi(x, \omega) = ( c(x, \omega), \sigma(x) ).
$$

---

# Volume rendering

Let $o_p{u}$ is the pixle value at position $u$ for a camera parameter $p$.
Then, it determines a view line called "ray" denoted as $\gamma_{u;p}$.

By using the one-parameter $t$ along the ray, the rendering is given as
$$
o_p(u) = \int_{\gamma_{u;p}} dt \; T_{\sigma}(t) \; \sigma(r(t)) \; c( r(t), \omega ).
$$
where we introduced the view direction $\omega$ and the decaying factor defined as
$$
T_{\sigma}(t) := \exp \left[ - \int_{\gamma_{u;p}^{\;t}} dt' \; \sigma(r(t')) \right].
$$

<div>
  <table>
  <tr><td>
    <img class="w-130 h-45" src="https://uploads-ssl.webflow.com/51e0d73d83d06baa7a00000f/5e700ef6067b43821ed52768_pipeline_website-01-p-800.png" />
  </td><td>
    <img class="w-60 h-45" src="https://uploads-ssl.webflow.com/51e0d73d83d06baa7a00000f/5e700a02ee168a2a63febc3b_pipeline_website-02.svg" />
  </td></tr>
  </table>
</div>

---

# Literature

- [1]: Neural Fields in Visual Computing and Beyond, https://arxiv.org/abs/2111.11426
  Yiheng Xie, Towaki Takikawa, Shunsuke Saito, Or Litany, Shiqin Yan, Numair Khan, Federico Tombari, James Tompkin, Vincent Sitzmann, Srinath Sridhar
- [2]: Advances in Neural Rendering, https://arxiv.org/abs/2111.05849
  Ayush Tewari, Justus Thies, Ben Mildenhall, Pratul Srinivasan, Edgar Tretschk, Yifan Wang, Christoph Lassner, Vincent Sitzmann, Ricardo Martin-Brualla, Stephen Lombardi, Tomas Simon, Christian Theobalt, Matthias Niessner, Jonathan T. Barron, Gordon Wetzstein, Michael Zollhoefer, Vladislav Golyanik
- [3]: Nerf: Representing scenes as neural radiance fields for view synthesis, ECCV 20202
  Ben Mildenhall, Pratul P. Srinivasan, Matthew Tancik, Jonathan T. Barron, Ravi Ramamoorthi & Ren Ng

