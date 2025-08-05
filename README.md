# FTIR-Noise-Analysis

The purpose of these experiments is to elucidate the path length dependence of noise values in various 'clean' regions in an FTIR Spectrometer. This was important as theory predicts a relative doubling of signal in a doubling of path length (including noise), we have observed deviations from this prediction, particularly in different wavenumber regions. The second analysis builds off the first, rectifying some mistakes in experimental setup and providing more conclusive results. Unfortunately, due to time constraints only $2$ variations on the path length were available to use in each experiment. The experiment is ongoing and at the end of the second analysis I detail, very specifically, how the analysis can be broadened and deepend, and it is my intention on pursuing this evolved experimental design as time and resources permit. Due to the time/spatial variance of noise (particularly thermal drift and mechanical vibrations), coupled with the introduction of atmosphere necessitating repurging the instrument with each path length swap, the evolved experimental design will need to be started and concluded continuously, without interruption. Once the time and resources are set aside to conduct this experiment, I will update this repository with Part $3$.

## Experimental Setup

The experimental setup for both parts of the experiment involves a MIDAC I-Series (gas cell) FTIR Spectrometer purged with blowoff nitrogen gas from a liqud nitrogen dewer ($H_2O \leq 10 \ ppm$). In Part $1$, due to circumstances, only a $10 \ m$ gas cell was able to be utilized, with the shortened path being simulated by bypassing the cell entirely through removal of the nose mirrors (though this was fraught with the introduction of many confounding variables, as well as a lack of true quantification, as the interal path length of the house had yet to be determined - see Ethylene-Saturation-Analysis). In Part $2$ both a $10 \ m$ and $20 \ m$ cell were available for use. In both cases, $5$ wavenumber regions were selected for analysis: $1800-2000$, $2000-2200$, $2400-2600$, $2600-2800$ and $3800-4000$. These regions were selected due to their relative scarcity of absorption features in atomsphere; thus making it easier to directly observe the behavior of the noise, without confounding due to absorption signals. Additionally, a noise trace running the entire spectral range (~$500 - 5000 \ cm^{-1}$) enabling both local and global inspection and analysis of the behavior of the noise. AutoQuant was used to control the instruments and take the spectra ($256$ co-added scans per sample at $0.5 \ cm^{-1}$ resolution). Essential FTIR was used to inspect and batch export the spectra to .csv, which were then loaded into a Python script for analysis.

### Python Environment

**Python Version** $\geq 3.8$

**Libraries**
- Numpy
- Pandas
- Matplotlib
- OS
- Scipy
- Glob

## Results

**As always, all figures, tables, graphs and code are in the relevant notebooks**

**Results for part $1$**: Were invalidated by the necessity to use the cell bypass in lieu of a true shortened cell relative to the $10 \ m$. The thinking, at the time, was that the edge mirrors were configured just so, to allow for continuous transmission even with the nose mirrors (which direct the IR beam into and out of the cell) removed. This turned out to be false, as the cell bypass exhibited much greater noise than the much longer $10 \ m$ cell. It is my belief that the removal of the nose mirrors introduced numerous confounding variables to the transmission of the beam. Namely, optical instability in the beam due to the lack of focusing mirrors normally present in the cell and window elements that help maintain collimation. Their removal may have resulted in beam divergence, optical clipping and increased scattering, thereby degrading the uniformity of the IR beam and thereby lowering photon flux on the detector and increasing random fluctuations on the detector. This interpretation is supported, quantitatively, by increased coefficients of variation in the bypass configuration, as well as a significantly higher baseline noise profile. These findings, while invalidated, were not useless. Speaking personally, the experiment underscored the importance of the various optical elements in the instrument, something which can be overlooked when focusing primarily on theory and abstracting away the practical elements of operation; it was very clarifying for me.

**Results for part $2$**: With the availability of a second cell of varying length the provenance of the results for part $2$ are much more robust. The table below summarizes the results:

<img width="884" height="184" alt="image" src="https://github.com/user-attachments/assets/6168cb89-804d-49a4-9b08-833c964f7288" />

As you can see, for each ROI the standard deviation of the absorbance across repeated scans (the noise) increased signficantly when the path length doubled. With all $p$-values $\lt \lt 10^{-3}$ this indicates the observed noise increases are highly unlikely to result from random variation.

We also note that the coefficient of variation drops at $20 \ m$ in every ROI, despite the higher absolute noise levels, indicating the baseline absorbance grows faster (Via Beer-Lambert law) than its fluctuation amplitude

## Conclusions

__Path Length vs Noise Trade-Off__: As expected from Beers law, doubling the optical path length enhances absorbance signals but also amplifies baseline fluctuations (e.g., residual gas absorption, drift, thermal/mechanical fluctuations) resulting in a net noise increase of $36-79\%$ in "quiet" spectral windows

__Optimal ROI Selection__: Of all the tested regions, $1800-2000$ and $2000-2200$ record the smallest absolute noise levels as well as the smallest relative increases in noise when increasing path length. Suggesting these windows are ideal for quantifying weak features in long-path length measurements
- In addition, looking at the $CV\%$ values for each region we note that the $CV\%$ values are relatively high in the low-mid frequency regions, suggesting residual gas absorption and thus, artifacts due to incomplete purge remain. The relatively smaller $CV\%$ value of the high wavenumber region suggest this window is dominated by instrument/detector noise rather than residual gas absorption
- This notion is corroborated by looking at the spectra

$\therefore$ Given a more complete purge we might see even lower absolute noise values in the low-mid IR regions

__Summary__: These results validate, and quantify, the noise/pathlength relationship and pinpoint the best regions for true instrument noise characterization in both $10$ and $20$ meter configurations. The statistical analysis, along with inspection of the spectra, confirm that while our results are not the product of inherent random variation and, as a result, are reproducible, they are also degraded by an incomplete purge protocol

## Next Steps

In order to more robustly quantify the noise/pathlength relationship, including decoupling pure signal enhancement from baseline drift effects and determining the optimal path length the experimental setup will need to be modified in the following ways:

1. Take $n \geq m \in \mathbb{Z}$ replicate spectra for a number of path lengths $\ell$, randomizing and alternating $k \lt n$ replicates for each path length until $k \geq m$, to reduce systematic drift bias
    - Ensure the purge is complete and robust, ideally by monitoring a "clean" water band until its absorbance drift $\lt 1 \times 10^{-4} AU$ before starting true acquisition
    - Before and after each block of $k$ replicate spectra, run a background scan to track baseline drift. Subtract interpolated background from your sample spectra to remove low-frequency noise
        1. Choose a clean ROI and track the mean absorbance over consecutive background scans
        2. Graph mean absorbance vs scan number; a sloped line signifies drift, random variance around zero indicates stability
        3. Ensure drift is below an agreed-upon threshold (e.g., $10^{-4}$)
2. Fit $A(\ell)$ to a linear model: This verifies Beers law is upheld and our data provenance is robust
    - Since a plot of $A$ vs. $\ell$ should be a straight line through the origin with slope $\epsilon c$
        - The slope then gives the product of the molar absorptivity $\epsilon$ and the analyte concentration $c$
    - Deviations from linearity (e.g., curvature or intercept-offset) flag departures from ideality - such as stray scattering, cell-window reflections or saturation at high absorbance
    - Quantifying the goodness of fit with $R^2$: A high $R^2$ value, ideally $R^2 \approx 1.00$, verifies the absorbance scales linearly with path length and validates our optical alignment assumptions
3. Fit $\sigma(A)$ vs $\ell$ to a polynomial model: This determines the scaling factor for our noise and elucidates the nature of the dominant noise mechanism
    - _Empirical Scaling Laws_:
        1. White-noise limited regime: $\sigma \propto \sqrt{(\ell)}$ if detector shot noise or thermal/mechanical fluctuations dominate
        2. Drift limited regime: $\sigma \propto \ell^{\alpha}$ (With $\alpha \approx 1$) if baseline fluctuations, purge variability or thermal drift dominate
    - _Fitting Procedure_:
        1. Plot $\log(\sigma)$ vs. $\log(\ell)$
        2. Fit a straight line: $$\log(\sigma) = \alpha \log(\ell) + \beta \rightarrow \sigma = 10^{\beta}\ell^{\alpha} \tag{3}$$
        3. The exponent $\alpha$ reveals the dominant noise mechanism
    - _Variations_: There are $2$ variations to this procedure depending on requirements for interpretation:
        1. ROI Specific Curves: For each ROI, plot it's measured $\sigma_{ROI}$ at each path length $\ell$ to end up with one curve per ROI on the same $\log-\log$ axes. This reveals how each ROI's noise scales with the environmental factors
        2. Aggregate Noise Curve: Compute a single noise metric as the mean of all ROI specific noise values, as: $$\langle \sigma \rangle(\ell) = \sum\limits_{i=1}^N \sigma_i(\ell) \tag{4}$$ Then plot $(\ell, \langle \sigma \rangle)$. This provides a simple overall view of how the system behaves with path length.
        3. Both Methods can be used in concert to give a holistic, yet granular understanding of the systematic noise behavior
4. Plot $SNR(\ell) = \frac{\langle A \rangle}{\sigma}$ to find the optimal path length for maximal sensitivity.
    - Recall: $$SNR(\ell) = \frac{\mu(\ell)}{\sigma(\ell)} \propto \frac{\ell}{\ell^{\alpha}} = \ell^{1-\alpha} \tag{5}$$
        - If $\alpha \lt 1$, SNR _increases_ with path length (signal grows faster than noise) - longer cells improve sensitivity
        - If $\alpha \approx 1$, SNR is roughly constant - path length choice is a matter of convenience
        - If $\alpha \gt 1$, SNR _decreases_ with path length (noise outpaces signal) - shorter cells are better
    - Finding the optimum:
        1. Compute $SNR(\ell)$ for each path length from your data
        2. Plot $(\ell, SNR)$ and look for the maximum

By carrying out these $3$ fits - Linear Absorbance, Power-Law Noise and SNR Profiling - we will fully characterize how both the signal and baseline fluctuations scale with optical path length, and thus choose the path that maximizes our systems analytical performance.
