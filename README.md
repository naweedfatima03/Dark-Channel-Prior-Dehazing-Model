# 🌫️ Single Image Dehazing using Dark Channel Prior (DCP)

## Overview

This project is a **self-implementation** of the **Dark Channel Prior (DCP)** algorithm for single-image haze removal, originally proposed by **Kaiming He, Jian Sun, and Xiaoou Tang** in *“Single Image Haze Removal Using Dark Channel Prior”* (International Journal of Computer Vision, 2011).

The algorithm restores visibility in hazy images by estimating the spatial distribution of haze (transmission map) and the global atmospheric light component, enabling accurate recovery of scene radiance.

This implementation has been evaluated using the **Dense-Haze (NTIRE 2019)** dataset — a benchmark introduced by **C. O. Ancuti, C. Ancuti, M. Sbert, and R. Timofte** in their IEEE ICIP 2019 paper *“Dense Haze: A Benchmark for Image Dehazing with Dense-Haze and Haze-Free Images”*, and utilized in the *NTIRE 2019 Image Dehazing Challenge* (CVPR Workshop, 2019).

---

## Methodology

The **Dark Channel Prior (DCP)** is a statistical observation derived from natural haze-free images, asserting that within most local patches (excluding sky regions), at least one color channel tends to exhibit a very low pixel intensity.

Leveraging this prior, the method operates under the **atmospheric scattering model**, which describes the formation of hazy images as a combination of attenuated scene radiance and additive airlight. The algorithm estimates two key components:

* **Global Atmospheric Light (A):** Represents the overall illumination caused by scattering.
* **Transmission Map (t(x)):** Encodes the fraction of scene radiance that reaches the observer without being scattered.

By estimating these quantities, the scene radiance **J(x)** is reconstructed, effectively recovering the haze-free image.
This approach is **model-driven** and **non-learning-based**, relying solely on image statistics rather than training data or deep networks.

---

## Pipeline

1. **Dark Channel Extraction** – Compute local minima across RGB channels within fixed-size patches to generate the dark channel map.
2. **Atmospheric Light Estimation** – Select candidate pixels with the highest intensity values in the dark channel to infer the global airlight vector.
3. **Transmission Estimation** – Estimate transmission via normalized dark channel computation relative to atmospheric light intensity.
4. **Transmission Refinement** – Apply edge-preserving filtering (e.g., guided or bilateral) to refine the estimated transmission and maintain structural continuity.
5. **Scene Radiance Recovery** – Reconstruct the haze-free image by inverting the atmospheric scattering model using the refined transmission and airlight estimates.

---

## Dataset

This project employs the **Dense-Haze (NTIRE 2019)** dataset — a benchmark comprising dense haze images and their corresponding ground-truth clear counterparts for quantitative and qualitative evaluation of dehazing algorithms.

If you use this dataset, please cite the associated ICIP 2019 and CVPR 2019 papers (see References below).

---

## References

### Core Algorithm

> **He, K., Sun, J., & Tang, X. (2011).**
> *Single Image Haze Removal Using Dark Channel Prior.*
> *International Journal of Computer Vision (IJCV)*, 98(2), 139–169.
> [https://people.csail.mit.edu/kaiming/publications/pami10dehaze.pdf](https://people.csail.mit.edu/kaiming/publications/pami10dehaze.pdf)

### Review Work

> **Lee, S., Yun, S., Nam, J.-H., Won, C. S., & Jung, S.-W. (2016).**
> *A Review on Dark Channel Prior Based Image Dehazing Algorithms.*
> *EURASIP Journal on Image and Video Processing*, 2016(4).
> [https://doi.org/10.1186/s13640-016-0104-y](https://doi.org/10.1186/s13640-016-0104-y)

### Dataset Papers

> **[1]** C.O. Ancuti, C. Ancuti, M. Sbert, R. Timofte.
> *Dense Haze: A Benchmark for Image Dehazing with Dense-Haze and Haze-Free Images.*
> IEEE International Conference on Image Processing (ICIP), 2019.
>
> **[2]** C.O. Ancuti, C. Ancuti, R. Timofte, L. Van Gool, L. Zhang, and M.-H. Yang.
> *NTIRE 2019 Image Dehazing Challenge Report.*
> IEEE CVPR Workshop, 2019.
>
> **Dataset link:** [https://data.vision.ee.ethz.ch/cvl/ntire19/dense-haze/](https://data.vision.ee.ethz.ch/cvl/ntire19/dense-haze/)
