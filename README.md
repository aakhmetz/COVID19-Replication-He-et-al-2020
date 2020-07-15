# Replication of the results of He et al NatMed 2020

This repository is to infer infectiousness profile using distribution of serial intervals and to replicate the results of He et al NatMed 2020.

**Dataset of He et al.** compromises 77 infector-infectee pairs
<p align="center">
  <img src="data/data_He_NatMedicine.png" title="Dataset of He et al 2020">
</p>

## Framework

Each infector is characterized by the infectiousness profile – the probability of transmitting the disease over time. If the time zero is the illness onset of the infector, which we denote as $$o_1$$, the profile is defined by a unimodal distribution $$\beta(t_I-o_1; c)$$, where $$t_I$$ is the time of infection of the infectee by the infector, $$c$$ is the shift of the distribution to account for pre-symptomatic transmissions. Because the timepoint $$t_I$$ is not observable, but the serial interval $$(o_2-o_1)$$ does. Here, $$o_2$$ is the time of illness onset of the infector. If the incubation period is similarly defined by the distribution $$g(o_2-t_I)$$, the serial interval $$h$$ is a sum of two $$h(o_2-o_1 )=g(o_2-t_I) + \beta(t_I-o_1; c)$$ (in this case we imply the convolution when we say "the sum of two").

## He et al study

He et al. obtained point-wise estimates of the infectiousness profile using maximum likelihood estimation (MLE).

The infectiousness profile was fitted to the shifted gamma distribution with the shift *c* = –2.3 days (95% CI: –3.0, –0.8), the maximum of the distribution (=the mode) was at –0.7 days (95% CI: –2.0, 0.2).The estimated fraction of presymptomatic transmissions was 44% (95% CI: 25%, 69%).

## Replication of He et al results

We perform our simulation with implementation in Stan. Comparing four distribution by WAIC values, we identify the best-fit distribution to be a shifted Weibull distribution:
<p align="center">
  <img src="results/WAIC.png" title="Comparing data fits with WAIC values">
</p>
where the fits:
<p align="center">
  <img src="results/model_fit.png" title="Data fitting with different distributions">
</p>

The best-fit Weibull distribution was shifted by *c* = –3.7 days (95% CI: –4.3, –3.3) with the mode at –0.8 days (95% CI: –2.5, 0.5). The presymptomatic transmission compromised 47% (95% CI: 34%, 61%).

**These estimates are almost exactly the same as the ones obtained by He et al.**

We did another double-check by extending the dataset using by He et al. After including additional infector-infectee pairs collected by us and not shown here (literature search + Japanese data resulted in total 265 pairs), we arrived at quite similar results with only difference that the Weibull distribution became more determined:
<p align="center">
  <img src="results/weibull2.png" title="Best-fit Weibull distribution">
</p>
In this case the shift was *c* = –3.2 days (95% CI: –3.5, –3.2), the mode was at –0.8 days (95% CI: –1.4, –0.2). The fraction of presymptomatic transmissions was a bit higher than before: 58% (95% CI: 49%, 67%).

### Code scripts
* [A1. Stan simulations.ipynb](https://nbviewer.jupyter.org/github/aakhmetz/) Code to run MCMC simulations in cmdStan

* [A2. Processing the traces.ipynb](https://nbviewer.jupyter.org/github/aakhmetz/) Python script to analyse posterior distirubions generated by Stan

### A note about incubation period

The incubation period period used in He et al was a lognormal distribution of the mean 5.2 days (95% CI: 4.1, 7.0) from [Li et al NEJM 2020]. In this case, the parameters of the lognormal distribution are 1.434 and 0.661.

In our Stan simulations we used the estimates of Linton et al 2020 which were based on a larger dataset. In that case the mean is 5.1 days and parameters of the lognormal distribution are 1.519 and 0.615. Linton et al estimates were consistent to two other studies of Baker et al 2020 and Lauer et al 2020.

## Conclusions

The results of simulations in Stan show high consistency with the MLE estimates obtained in He et al 2020. Unintentional exclusion of two extreme datapoints by He et al has not affected the resulting estimates.

Moreover, fitting data with different distributions showed that shifted distributions with positive support (Weibull/gamma/lognormal) were much better than skewed normal distribution in terms of WAIC values.

Accounting for more data points (extended dataset available for Japan and not included here) showed even a better robustness of He et al results.

Our replication exercise does not support the claims of a recent preprint of Ashcroft et al, who argued that the infectiousness profile would be similar to a skewed normal distribution and the results of He et al are incorrect. We consider the omission of two data points by He et al as a minor technical issue rather than an error. Our replication demonstrate consistency with He et al after inclusion of two points.

## References

[Ashcroft et al 2020] Ashcroft, P.; Huisman, J. S.; Lehtinen, S.; Bouman, J. A.; Althaus, C. L.; Regoes, R. R.; Bonhoeffer, S. COVID-19 infectivity profile correction. arXiv 2020 ([https://arxiv.org/abs/2007.06602](https://arxiv.org/abs/2007.06602))

[Backer et al 2020] Backer, J.A.; Klinkenberg, D.; Wallinga, J. Incubation period of 2019 novel coronavirus (2019-nCoV) infections among travellers from Wuhan, China, 20–28 January 2020. Euro Surveill. 2020. ([https://doi.org/10.2807/1560-7917.ES.2020.25.5.2000062](doi:10.2807/1560-7917.ES.2020.25.5.2000062))

[He et al 2020] He, X.; Lau, E.H.Y.; Wu, P.; et al. Temporal dynamics in viral shedding and transmissibility of COVID-19. Nat Med 2020 ([http://dx.doi.org/10.1038/s41591-020-0869-5](doi:10.1038/s41591-020-0869-5))

[Lauer et al 2020] Lauer, S.A.; Grantz, K.H.; Bi, Q.; Jones, F.K.; Zheng, Q.; Meredith, H. R.; Azman, A.S.; Reich, N.G.; Lessler, J. The Incubation Period of Coronavirus Disease 2019 (COVID-19) From Publicly Reported Confirmed Cases: Estimation and Application. Annals of Internal Medicine 2020 ([http://dx.doi.org/10.7326/M20-0504](doi:10.7326/M20-0504))

[Linton et al 2020] Linton, N.M.; Kobayashi, T.; Yang, Y.; Hayashi, K.; Akhmetzhanov, A.R.; Jung, S-m.; Yuan, B.; Kinoshita, R.; Nishiura, H. Incubation period and other epidemiological characteristics of 2019 novel coronavirus Infections with right truncation: A statistical analysis of publicly available case data. Journal of Clinical Medicine 2020, 9(2), 538 ([http://dx.doi.org/10.3390/jcm9020538](doi:10.3390/jcm9020538))

---------
**Thank you for your interest!**

Few words of caution: We would like to note that our code is not supposed to work out of box, because the links used in the notebooks were user-specific, and our main intention was to show the relevance of the methods used in our paper.