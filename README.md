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

As you can see, for each ROI the standard deviation of the absorbance across repeated scans (the noise) increased signficantly when the path length doubled. With all $p$-
