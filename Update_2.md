Adaptive Patch-Aware Haze Removal
Update 2
Overview

Update v3 extends the adaptive multi-sample dehazing (v2) by introducing:

=> Frobenius norm–based quantitative validation

=> Synthetic patch-wise haze generation

=> Patch-only selective dehazing

=> Controlled spatial blending

=> Stability comparison between global and localized restoration

This version improves robustness by preventing unnecessary modification of non-hazy regions.

🔬 Methodology
1) Frobenius Norm Validation (Real Haze Case)

After global restoration, haze removal effectiveness is quantified using energy-based analysis:

norm_input  = sqrt(sum(I(:).^2));
norm_output = sqrt(sum(Ffused(:).^2));
norm_removed = sqrt(sum((I - Ffused)(:).^2));

hazeReductionPercent = (norm_removed / norm_input) * 100;

Metrics Computed are:

Input Energy

Output Energy

Haze Removed Energy

Percentage Haze Reduction

![Metrics Calculated](doc/images/metrics.png)

This ensures physical consistency of restoration.

2️) Synthetic Patch-Wise Haze Creation

To test spatial robustness, haze is manually introduced into selected patches of a clean image.

Haze Formation Model
𝐼
(
𝑥
)
=
𝐽
𝑐
𝑙
𝑒
𝑎
𝑛
(
𝑥
)
⋅
𝑡
(
𝑥
)
+
𝐴
(
1
−
𝑡
(
𝑥
)
)
I(x)=J
clean
	​

(x)⋅t(x)+A(1−t(x))

Two radial haze patches are generated with smooth Gaussian blending:

t_true = imgaussfilt(t_true,4);

![Input](doc/images/patchwise.png)
![Synthetic Haze patches](doc/images/patchwise.png)

This creates localized haze regions while preserving the rest of the image.

3️) Global Dehazing

Applied same multi-sample adaptive pipeline:

Atmospheric light estimation

Dark channel transmission

Regularized pseudo-inverse recovery

Multi-sample restoration

Frequency-domain fusion

Result:

Haze removed

But non-hazy regions slightly altered

![Input](doc/images/global.png)

4) Patch-Only Selective Blending (Key Contribution)

To avoid artificial enhancement of clean areas:

Haze Mask Creation
hazeMask = t_true < 0.98;
hazeMask = imgaussfilt(double(hazeMask),5);
Selective Blending
J_final(:,:,c) = ...
    hazeMask .* Ffused(:,:,c) + ...
    (1-hazeMask).*I(:,:,c);

![Input](doc/images/patchout.png)
    
What we Achieved:

* Only hazy regions are corrected
* Clean regions remain untouched
* No artificial amplification
* Reduced edge artifacts
