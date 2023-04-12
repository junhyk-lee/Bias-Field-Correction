Bias Field Correction with Hampel random noise
============
by Lee,Junhyeok   Kang,Junghwa    Nam,Yoonho    Lee,TaeYoung



Abstract
============

> Non-uniform bias field due to external factors hampers quantitative MR image analysis. For reliable quantitative MR image analysis, appropriate correction for the bias field is necessary. In this study, we propose Hampel denoising diffusion model to effectively correct the bias field from MR images. Compared with N4 and Gaussian denoising diffusion models, the proposed model provided higher PSNRs, SSIMs and lower MSEs. Higher efficiency could be achieved compared to N4 when our model takes 9 times faster in inference time.


Introduction
============

> Magnetic Resonance Imaging (MRI) is a widely used medical imaging modality. But bias field obscure subtle details and impede accurate identification. N4 has been a commonly used method for correcting the inhomogeneities, however, this method has limitations in terms of its accuracy, technical factor, and efficiency. We propose Hampel Denoising Diffusion Model (HDDnet) conceived to model inhomogeneities by Cauchy-Lorentz distribution. We modeled the Hampel mixture distribution to represent the image intensity disrupted by the inhomo- geneities. To assess the fitness of Hampel function to the image intensity, the mean fitting error between the histogram and the probability function was calcuated, shown in Figure 1. The intensity difference between the input image and t-step image of diffusion process was used in both histogram and the probability function. The mean fitting error is 0.012 less in Hampel function showing that it is a much better fit to MRI with the bias field, Figure 1A-D. Proposed method effectively corrects the bias field and gener- ates reduced inhomogeneities MRI with higher accuracy and faster inference time than N4. 


Method and Experiments
============

> Method : We modeled the Hampel Mixture Distribution to represent the image intensity disrupted by the inhomogeneities. Denote H(\alpha)
> Dataset
> Model and Training
> Evaluation




Results
============


Conclusion
============

> In this paper, we propose a new bias field correction method by altering the Gaussian noise to Hampel noise, a mixture of Gaussian distribution and Cauchy-Lorentz distribution. Our proposed method is more robust with automatic parameter settings on correcting the bias field than N4. Such automation can give less complexity to the user. We also point out that such deep learning approach is faster in time while still maintaining the accuracy.

