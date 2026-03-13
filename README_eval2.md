
# Uncertainty-Aware Image Dehazing (Evaluation 2)

## Mathematics for Computing 4 – Project
Amrita School of Artificial Intelligence  
Amrita Vishwa Vidyapeetham – Coimbatore

### Team D6

| Name | Roll Number |
|-----|-----|
| Sai Jagruth | CB.SC.U4AIE24310 |
| Baby Sree | CB.SC.U4AIE24318 |
| Vardhan | CB.SC.U4AIE24320 |
| Likitha Reddy | CB.SC.U4AIE24361 |

---

# Introduction

Outdoor images often suffer from visibility degradation due to atmospheric haze. 
Haze is caused by scattering of light from particles such as dust, fog, smoke, and water droplets.

This results in:

- reduced contrast
- color distortion
- blurred edges
- loss of distant details

Image dehazing attempts to recover the clean scene radiance from such degraded images.

---

# Difference Between Haze, Blur and Noise

| Degradation | Cause | Effect |
|-------------|------|--------|
| Haze | Atmospheric scattering | Reduced contrast |
| Blur | Motion or defocus | Loss of sharpness |
| Noise | Sensor disturbance | Random pixel variations |

Haze is **depth dependent**, unlike blur or noise.

---

# Atmospheric Scattering Model

The formation of hazy images follows:

I(x) = J(x)t(x) + A(1 − t(x))

Where:

I(x) → observed hazy image  
J(x) → clean scene radiance  
t(x) → transmission map  
A → atmospheric light  

Transmission is defined as:

t(x) = exp(-β d(x))

where:

d(x) → scene depth  
β → scattering coefficient

---

# Scene Radiance Recovery

Rearranging the haze model:

J(x) = (I(x) − A(1 − t(x))) / t(x)

However, direct inversion becomes unstable when transmission is small.

---

# Regularized Pseudo-Inverse Reconstruction

To stabilize the inversion:

H = t(x)

Direct inverse:

H⁻¹ = 1/t

Using Tikhonov regularization:

H† = H / (H² + λ)

Substituting H = t:

H† = t / (t² + λ)

Reconstruction:

J = H†(I − A) + A

---

# Uncertainty-Aware Reconstruction

Instead of producing a single solution, multiple reconstructions are generated:

J1, J2, J3, J4

Each uses different:

- blur sizes
- regularization parameters

This models uncertainty similar to CVAE approaches.

---

# Frequency Domain Fusion

Each reconstructed image is transformed using Fourier transform.

F(u,v) = Σ Σ f(x,y)e^{-j2π(ux/M + vy/N)}

The spectra are averaged:

F_fused = (1/N) Σ F(Jk)

Final image is recovered using inverse Fourier transform.

---

# Patch-wise Haze Modeling

Real haze varies spatially. To simulate this:

Two rectangular patches are selected.

Distance from patch center:

d(x,y) = sqrt((x-xc)^2 + (y-yc)^2)

Normalized distance:

dn(x,y) = d(x,y)/max(d(x,y))

Transmission inside patch:

tp(x,y) = 0.4 + 0.3 dn(x,y)

---

# Patch-Selective Dehazing

A haze mask identifies haze regions.

M(x,y) = 1 if t(x,y) < τ else 0

Final reconstruction:

J_final = M * J_global + (1-M) * I

This prevents modifying already clear regions.

---

# ADMM Formulation

The dehazing problem can be expressed as:

min_J || I − (Jt + A(1 − t)) ||² + λ R(J)

Using ADMM:

J^(k+1) = argmin ||I − (Jt + A(1 − t))||² + (ρ/2)||J − Z + U||²

ADMM improves numerical stability and prevents over-amplification.

---

# Frobenius Norm Evaluation

Image energy is measured using:

||I||_F = sqrt(Σ I²)

||J||_F = sqrt(Σ J²)

Haze removed:

||I − J||_F

Haze reduction percentage:

(||I − J||_F / ||I||_F) × 100

---

# Frobenius Norm and Singular Values

If I = UΣVᵀ then

||I||_F = sqrt(Σ σ_i²)

Thus Frobenius norm equals the energy of singular values.

---

# Results

The proposed framework improves:

- visibility
- edge clarity
- contrast
- color consistency

The method works effectively across different scenes such as mountains, urban environments, vegetation, and water bodies.

---

# Conclusion

This project proposes an uncertainty-aware image dehazing framework combining:

- physics-based haze modeling
- regularized pseudo-inverse reconstruction
- uncertainty sampling
- frequency-domain fusion
- patch-selective correction
- ADMM optimization

The framework produces stable and visually improved dehazing results.

---

# Repository Tag

eval_2
