# D6_MFC4_ImageDehazing
This Repo has the details of the MFC-4 Project.


# UNCERTAINTY-AWARE IMAGE DEHAZING USING PSEUDO-INVERSE MODELING INSPIRED BY CVAE

## Team Details

**Team D – 6**

- Sai Jagruth – CB.SC.U4AIE24310  
- Baby Sree – CB.SC.U4AIE24318  
- Vardhan – CB.SC.U4AIE24320  
- Likitha Reddy – CB.SC.U4AIE24361  

---

## Project Description

This project focuses on **single image dehazing** using an **uncertainty-aware approach** inspired by the idea of Conditional Variational Autoencoders (CVAE).  
Unlike deep learning based methods, this approach **does not involve training or datasets**.

Instead of producing only one dehazed image, the method generates **multiple plausible dehazed outputs** by slightly varying the parameters of the physical haze inversion model. These multiple outputs represent uncertainty in the dehazing process. The results are then **fused** to obtain a stable and visually balanced final RGB image.

The method is built using a **physics-based haze imaging model** and a **regularized pseudo-inverse formulation**

---

## Base / Reference Papers

The concepts used in this project are inspired by the following research works:

1. **H. Ding et al., “Robust Haze and Thin Cloud Removal via Conditional Variational Autoencoders,”  
   IEEE Transactions on Geoscience and Remote Sensing, 2024.**  
   https://ieeexplore.ieee.org/document/10394023  

2. **S. G. Narasimhan and S. K. Nayar, “Vision and the Atmosphere,”  
   International Journal of Computer Vision, 2002.**  
   https://link.springer.com/article/10.1023/A:1016328200723  

3. **A. N. Tikhonov and V. Y. Arsenin, “Solutions of Ill-Posed Problems,”  
   Wiley, 1977.**  
   https://onlinelibrary.wiley.com/doi/book/10.1002/9780470172799  

These papers provide the theoretical foundation for uncertainty modeling, the physical haze imaging equation, and regularized inverse reconstruction.

---

## Project Outline

- Read a single RGB hazy image as input.
- Estimate airlight and transmission using the atmospheric scattering model.
- Generate multiple dehazed images by varying transmission, blur scale, and regularization parameters.
- Apply a regularized pseudo-inverse to avoid instability and brightness amplification.
- Fuse the multiple dehazed results in the frequency domain - We fuse in the frequency domain because the multiple dehazed results mainly differ in their high-frequency details, while the low-frequency structure remains consistent. Frequency-domain fusion preserves stable structures and suppresses inconsistent artifacts better than direct spatial averaging.
- Perform RGB color correction to obtain a stable and natural-looking output.
- Adapt parameters automatically based on haze density.

---

## Updates

- Implemented the complete dehazing pipeline in MATLAB.
- Added uncertainty handling inspired by CVAE concepts.
- Improved stability using pseudo-inverse regularization.
- Ensured final output is always in RGB format.
- The project runs without the need for training (usually CVAE requires training).

---

## Challenges / Issues Faced

- Direct inversion caused over-brightness and color distortion.
- Thick haze leads to significant information loss, limiting recovery quality.
- A single set of parameters did not work well for all haze conditions.
- Balancing contrast enhancement without introducing artifacts.
- Maintaining consistent color appearance across different images.

---

## Future Plans

- Improve transmission estimation using multi-scale techniques.
- Adaptive handling for **thin, moderate, and thick haze** conditions.

---

## Notes

This project emphasizes **physical interpretability and explainability** rather than black-box learning methods.  
It demonstrates how uncertainty modeling ideas can be applied using classical inverse problem techniques.
