# D6_MFC4_ImageDehazing
This project implements a single-image dehazing method inspired by CVAE ideas, without training. It generates multiple plausible dehazed results by varying haze inversion parameters to model uncertainty, then fuses them into a stable RGB output using a regularized pseudo-inverse model that adapts to thin, moderate, and thick haze.
