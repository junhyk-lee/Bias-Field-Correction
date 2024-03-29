Bias Field Correction with Hampel random noise
============
> Authors: Lee Junhyeok, Kang Junghwa, Nam Yoonho, Lee TaeYoung



Abstract
============

> Non-uniform bias field due to external factors hampers quantitative MR image analysis. For reliable quantitative MR image analysis, appropriate correction for the bias field is necessary. In this study, we propose **Hampel denoising diffusion model** to effectively correct the bias field from MR images. Compared with N4 and Gaussian denoising diffusion models, the proposed model provided higher PSNRs, SSIMs and lower MSEs. Higher efficiency could be achieved compared to N4 when our model takes 9 times faster in inference time.


Introduction
============

> Magnetic Resonance Imaging (MRI) is a widely used medical imaging modality. But bias field obscure subtle details and impede accurate identification. N4 has been a commonly used method for correcting the inhomogeneities, however, this method has limitations in terms of its accuracy, technical factor, and efficiency. **We propose Hampel Denoising Diffusion Model (HDDnet) conceived to model inhomogeneities by Cauchy-Lorentz distribution. We modeled the Hampel mixture distribution to represent the image intensity disrupted by the inhomogeneities**. To assess the fitness of Hampel function to the image intensity, the mean fitting error between the histogram and the probability function was calcuated, shown in Figure 1. The intensity difference between the input image and t-step image of diffusion process was used in both histogram and the probability function. The mean fitting error is 0.012 less in Hampel function showing that it is a much better fit to MRI with the bias field, Figure 1A-D. Proposed method effectively corrects the bias field and gener- ates reduced inhomogeneities MRI with higher accuracy and faster inference time than N4.
<p align="center">
  <img src = "https://github.com/junhyk-lee/Bias-Field-Correction/blob/main/HDD/Picture1.jpg" />
</p>
Figure 1 : (A) Hampel distribution (B) Gaussian distribution (C) Comparing the fit (D) The mean fitting MSE between the histogram and the density function


Method and Experiments
============

> Method :  We modeled the Hampel Mixture distribution to represent the image intensity disrupted by the inhomogeneities. Denote $\mathbb{H}(\alpha, x_{0}, \gamma$) as the Hampel mixture distribution, where $\alpha,x_{0},\gamma$ are weight, location, scale parameter, respectively; We use the term $F_h(x,\alpha), F_n(x;0,1), F_c(x; x_{0}, \gamma)$ as probability distribution function of Hampel, Gaussian, and Cauchy-Lorentz, respectively. Hampel function\footnote{Hampel mixture probability distribution function} could be written as
>
>> $F_h(x,\alpha)=(1-\alpha)F_n(x;0,1)+\alpha F_c(x;x_{0},\gamma)\ \ with\ \ 0\leq\alpha\leq1$
> 
>> $F_n(x;0,1) = {1\over\sqrt{2\pi}}exp({-x^2\over2}),\ F_c(x;x_{0},\gamma)={1\over\pi}\left({\gamma^2\over(x-x_0)^2+\gamma^2}\right)$
>
> Hampel function was optimized with Maximum Likelihood Estimation. Through maximizing the Hampel function, we were able to allocate $(\alpha,x_0,\gamma)$ as $(1e-05,0.6332,0.0274)$. Detail explanation will be described below. $\[\mathbb{H}(\alpha,x_0,\gamma)=\mathbb{H}(1e-05,0.6332,0.0274)\]$
>
> Dataset : This study was approved by the Institutional Review Board. We used $202$ subjects ($126$ male, $76$ female, age $26.27\pm7.84$ years) scanned on a 3T MRI following 3D gradient echo protocol with MT pulse \cite{nam2017imaging,namsimultaneous}. Each of the brain slices is resized to a size of $512\times512$ and normalized the values to range between $[0,1]$. Our dataset is composed of $6,000$ images ($n = 176$) to train and $780$ images ($n= 26$) to test.
>
> Model and Training : HDDnet is trained on Nvidia RTX 3090 GPU 24GB with the batch size of 8 for 512 iterations. HDDnet is trained with $L2$ loss, the sigmoid noise schedule for 1,000 steps, a learning rate of $10^{-6}$ for the Adam optimizer, the first layer is chosen as 64.
>
> Evaluation : Evaluation was took in both quantitative and qualitative. For the quantitative evaluation, **Mean Squared Error, Peak Signal-to-Noise Ratio, Structural Similarity Index Map**, respectively were used. Each was calculated between model output and the N4 label image. **Inference time** was measured in same environment with training setup, with 26 patients. The qualitative assessment was done by comparing N4 and HDDnet **prediction of synthetic bias field**. The synthetic bias field was generated from train image bias field merged to test set, shown in Figure 2.



Results
============
> **As shown in Table Table1(A), Hampel random noise outperformed Gaussian random noise in MSE, PSNR, SSIM. Quantitatively, Hampel mixture distribution can provide clear evidence of convergence. Figure 2 shows N4 and HDDnet follow similar pattern of the bias field in synthetic image. But as in Table1(B), our model outperformed on its MSE and PSNR, while SSIM is small in difference. While N4 takes average of 39.6014 secs to correct the bias field of from its corrupted MRI, HDDnet takes about average of 4 secs, which is 9.75 times faster. While maintaining or improving the bias field correction our model shows high efficiency in time.**

|   | Model    | MSE                 | PSNR              | SSIM             | Inference Time    |
|---|----------|---------------------|-------------------|------------------|-------------------|
| A | Gaussian | 0.0004 &pm; 0.0003  | 32.486 &pm; 2.649 | 0.950 &pm; 0.002 | 4.471 &pm; 0.030  |
|   | Hampel   | 0.0003 &pm; 0.0002  | 35.945 &pm; 1.050 | 0.983 &pm; 0.004 | 4.473 &pm; 0.012  |
| B | N4       | 0.0003 &pm; 0.0003  | 34.766 &pm; 1.815 | 0.979 &pm; 0.003 | 39.601 &pm; 0.128 |
|   | HDDnet   | 0.0001 &pm; 0.0002  | 36.865 &pm; 2.614 | 0.978 &pm; 0.003 | 4.478 &pm; 0.029  |

<p align="center">
  <img src = "https://github.com/junhyk-lee/Bias-Field-Correction/blob/main/HDD/fig_2.png" />
</p>


Conclusion
============

> In this paper, we propose a new bias field correction method by altering the Gaussian noise to Hampel noise, a mixture of Gaussian distribution and Cauchy-Lorentz distribution. Our proposed method is more robust with automatic parameter settings on correcting the bias field than N4. Such automation can give less complexity to the user. We also point out that such deep learning approach is faster in time while still maintaining the accuracy.



Hampel Mixture Distribution
============

> Signal can be divide into two sub-signals, $S_{m}, S_{b}$.  $S_{m}$ represents the signal that encodes the patients information. But $S_{b}$ is the unwanted bias field that corrupts the wanted signal and blurrs the quantitative analysis. We investigated with the prior knowledge that $S_{b}$ can be described with Cauchy-Lorentz distribution. We took the bias field apart from the MRI with using N4 and with R package **"fitdistr"** package to analyze whether it does fit into the Cauchy-Lorentz distribution. Figure 3 describes that the Cauchy-Lorentz distribution possibly describes the bias field with location and scale parameter $x_{0}, \gamma = (0.6332, 0.0274)$. We then established Hampel mixture distribution that collaborates the Gaussian distribution and the Cauchy-Lorentz distribution, each representing $S_{m}, S_{b}$ respectively.
<p align="center">
  <img src = "https://github.com/junhyk-lee/Bias-Field-Correction/blob/main/HDD/hdd_cdf.png" />
  <img src = "https://github.com/junhyk-lee/Bias-Field-Correction/blob/main/HDD/hdd_cdf2.png" />
</p>

> 





