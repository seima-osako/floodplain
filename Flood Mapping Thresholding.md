# Otsu vs. Kittler–Illingworth Thresholding for SAR Water Detection and Flood Mapping: A Technical Comparison

## TL;DR
- **Kittler–Illingworth (KI) minimum-error thresholding is the technically superior automatic thresholder for SAR flood mapping**, because it explicitly models unequal class variances and unequal priors — exactly the conditions (a small, low-variance water class embedded in a large, high-variance land class) that dominate flood scenes — whereas Otsu's implicit equal-variance/equal-prior assumption biases its threshold toward the larger (land) class and systematically overestimates or misplaces the water boundary under class imbalance.
- **Neither global method works on a whole flood scene**, because the scene-wide histogram is usually unimodal; both are therefore embedded in tile/split-based frameworks (Martinis et al. SBAT/tile-based; Chini et al. HSBA) that first select bimodal subimages using metrics such as Ashman's D, the Bhattacharyya coefficient, and a surface/class-balance ratio, then estimate a local threshold, and often use that threshold to seed a two-component Gaussian mixture (GMM/EM) plus spatial refinement (region growing, fuzzy logic, MRF, HAND masking).
- **Operationally, the choice matters less after refinement and ensembling**: Copernicus GFM runs three algorithms (LIST/Chini, DLR/Martinis, TUW/Bauer-Marschallinger) and majority-votes; DLR uses a hierarchical tile-based thresholding + fuzzy-logic chain, and many deep-learning pipelines still use Otsu (for its simplicity and scikit-image/GEE availability) to auto-generate weak labels. Use KI (or a SAR-specific non-Gaussian generalization) when running a single global/local thresholder on imbalanced scenes; Otsu is acceptable inside bimodal, class-balanced tiles or where speed and tooling dominate.

## Key Findings

1. **Mathematical relationship — Otsu is a special case of KI.** Xue & Zhang (2012, *Pattern Recognition Letters* 33(6):793–797) prove that Ridler–Calvard iterative selection is an iterative version of Otsu, and that "Otsu's method can be regarded as a special case of Kittler and Illingworth's MET method." Otsu maximizes between-class variance (equivalently minimizes within-class variance) and implicitly assumes two Gaussians of equal variance and equal prior; KI minimizes a criterion derived from the Gaussian-mixture log-likelihood that retains separate per-class variance and prior terms.

2. **Otsu's class-imbalance bias is real and directionally quantifiable.** Otsu's threshold equals the average of the two class means only when variances are equal; when within-class variances differ, the threshold biases toward the class with the larger variance (Xu et al., "Characteristic analysis of Otsu threshold," *Pattern Recognition Letters* 2011, which proves "Otsu threshold is equal to the average of the mean levels of two classes... [and] biases toward the class with larger variance"). In flood scenes water is a small, low-variance minority class, so Otsu shifts the boundary into the land distribution, overestimating water — the same reason a Google Earth Engine tutorial warns that Otsu "overestimated water areas" because "surface water usually constitutes only a small fraction of the overall land cover."

3. **KI has been extended to SAR-appropriate non-Gaussian distributions.** Moser & Serpico (2006, *IEEE TGRS* 44(10):2972–2982) generalized KI ("Generalized KI thresholding," GKIT) for the non-Gaussian amplitude statistics of SAR ratio images; Bazi, Bruzzone & Melgani (2005, *IEEE TGRS* 43(4):874–887, and their EM/generalized-Gaussian thresholding work) coupled EM with a generalized Gaussian model for changed/unchanged classes.

4. **Bimodality is the binding constraint.** Both methods assume a bimodal histogram. On whole flood scenes the histogram is typically unimodal, so tile/split frameworks select bimodal subimages. Per Chini, Hostache, Giustarini & Matgen (2017) HSBA, the exact eligibility criteria are "AD should be above 2, BC above 0.99 and the surface ratio above 10%."

5. **Operational systems and deep learning still lean on both.** Copernicus GFM/CEMS uses a three-algorithm ensemble whose accuracy is documented as greater than 94%; deep-learning pipelines routinely use Otsu to create weak/auto labels (Sen1Floods11 "OTSU" labels; several CNN/UNet studies).

## Details

### 1. Mathematical formulation

**SAR water physics.** Open, calm water is a specular reflector: it reflects C-band energy away from the sensor, so water appears dark (low σ⁰) — typically −20 to −25 dB for calm water at C-band VV, versus roughly −10 to −18 dB for bare soil and −6 to −10 dB for forest (C-band, 30–45° incidence). Flood mapping therefore reduces to separating a low-backscatter (water) mode from a high-backscatter (land) mode.

**Speckle and scale.** SAR intensity follows a multiplicative-speckle model I = R·S, with multi-look intensity gamma-distributed (L looks, mean 1, variance 1/L per Goodman's model). In linear/intensity space the water and land classes are gamma/non-Gaussian and strongly skewed; a logarithmic (dB) transform converts multiplicative speckle to additive and makes each class much closer to Gaussian ("Taking the log-transform results in an almost Gaussian distribution... in decibels (dB)"). This is central to the Otsu-vs-KI question: the Gaussian assumptions underlying both methods hold better in dB space, which is why most operational tile-based thresholding is performed on log-transformed σ⁰.

**Otsu (1979).** For a threshold T splitting the histogram into foreground (f) and background (b) with weights ω and means μ, the between-class variance is

σ_B²(T) = ω_f · ω_b · (μ_f − μ_b)².

Otsu selects T* = argmax σ_B²(T). Equivalently it minimizes the within-class variance ω_f σ_f² + ω_b σ_b². Its implicit model is two Gaussians of equal variance and equal prior; the optimal threshold reduces to the midpoint of the means when priors are equal.

**Kittler–Illingworth (1986).** KI models the histogram h(g) as a two-component Gaussian mixture. At each candidate threshold T it computes the class priors P_i(T) = Σ h(g), means μ_i(T) = [Σ h(g)·g]/P_i(T), and variances σ_i²(T) = [Σ (g−μ_i)²·h(g)]/P_i(T) over the two partitions (i = 1 for g ≤ T, i = 2 for g > T). It selects T* = argmin J(T), where the compact closed-form criterion function is

**J(T) = 1 + 2·[P₁(T)·log σ₁(T) + P₂(T)·log σ₂(T)] − 2·[P₁(T)·log P₁(T) + P₂(T)·log P₂(T)].**

J(T) is a direct index of the average classification error of assigning gray level g to a class under the Gaussian-mixture model. The presence of separate log σ₁, log σ₂ terms and separate P₁ log P₁, P₂ log P₂ terms is precisely what lets KI accommodate **unequal class variances and unequal priors** — the general case of which Otsu (equal σ, equal P) is a special case. For the equal-variance case, the Bayes/KI optimal threshold takes the closed form

T = (μ₁+μ₂)/2 + [σ²/(μ₁−μ₂)]·ln(P₂/P₁),

which shows explicitly how the prior-imbalance term ln(P₂/P₁) shifts the threshold away from the mean-midpoint — a term Otsu lacks entirely. (Minor notational differences — the additive constant "1," the factor of 2 — appear across textbooks and do not change the argmin.)

### 2. Why Otsu is biased under class imbalance

Because Otsu (equivalently) places the threshold near the average of the two class means and biases toward the larger-variance class, a scene with a small water fraction and a broad land distribution pushes the Otsu threshold up into the land mode, so land pixels are misclassified as water (commission error / overestimation). KI's explicit prior term ln(P₂/P₁) pulls the threshold back toward the small class, reducing this bias. Empirically, when water is only a fraction of a percent of the scene, plain global Otsu or KI both fail: the *Water* 2022 optimal-threshold/reference-mask paper explicitly targets "'water'/'land' class imbalance situations... even when the water surface covers only a fraction of a percent of the area," precisely because standard Otsu/KI cannot obtain the correct σ⁰ threshold there. A complementary empirical note: a 2026 Sentinel-1/LSTM study found "Otsu showed better performance at tributary stations with small water bodies due to reduced class imbalance" — i.e., Otsu's accuracy depends on how balanced the local scene is.

### 3. KI under non-Gaussian SAR statistics

- **Moser & Serpico (2006):** Generalized KI (GKIT) for SAR amplitude change detection, adopting an image-ratioing approach and "a SAR-specific parametric modeling approach for the ratio image" to "take into account the non-Gaussian distribution of the amplitude values of SAR images"; a "relaxed GKIT" was proposed later (IEEE conf. 6351079).
- **Bazi, Bruzzone & Melgani (2005):** Unsupervised SAR change detection using a generalized Gaussian model with a modified double-thresholding KI, plus a separate EM + generalized-Gaussian thresholding method ("Image thresholding based on the EM algorithm and the generalized Gaussian distribution").
- **Distribution options** used in the SAR literature for water/land or changed/unchanged classes include log-normal, gamma, Weibull, Nakagami, and generalized gamma/generalized Gaussian; multi-look intensity is classically gamma (or 𝒢⁰ under a reciprocal-gamma backscatter). The log-normal/Gaussian-in-dB choice is what makes plain KI workable on log-scaled σ⁰.

### 4. Bimodality analysis and tile/split frameworks

- **Martinis, Twele & Voigt (2009), *NHESS* 9:303–314 (SBAT):** Split-based automatic thresholding on TerraSAR-X. The scene is split into tiles; tiles with high backscatter variability (containing both water and land) are selected; a per-tile threshold is computed (KI or Otsu) and combined into a global threshold; multi-scale segmentation/region-growing then refines. Their stated purpose is that the tiling "solves the water detection problem even in large-size SAR data with small a priori probabilities" — i.e., it directly addresses class imbalance.
- **Chini, Hostache, Giustarini & Matgen (2017), *IEEE TGRS* 55(12):6975–6988 (HSBA):** Hierarchical quadtree decomposition searches for tiles of variable size whose histogram is bimodal, Gaussian, and class-balanced, judged by three criteria, with exact thresholds "AD [Ashman's D] should be above 2, BC [Bhattacharyya coefficient] above 0.99 and the surface ratio above 10%." Ashman's D is AD(h) = √2·|μ₁−μ₂|/√(σ₁²+σ₂²) (Ashman et al. 1994), requiring "a value of at least 2" for Gaussian separability; the Bhattacharyya coefficient measures overlap between the empirical histogram and a fitted two-Gaussian curve; the surface ratio is the smaller-class/larger-class pixel-count ratio (each class must exceed ~10%). A sum-of-two-Gaussians is fitted on the log-transformed image to parameterize water and land.
- **Alternative bimodality tests:** the bimodality coefficient (BC) based on skewness/kurtosis (used on σ⁰-dB tiles in large-area studies), Hartigan's dip statistic (used in dual-pol split-based optimal thresholding, *Remote Sensing* 2025), and Otsu's own between-class-variance (BCV) score all serve as tile-selection gates.

### 5. GMM/EM initialization — how Otsu/KI feed the mixture

The natural probabilistic model for a bimodal SAR tile histogram is a two-component GMM,

p(x) = π_w·N(x|μ_w,σ_w²) + π_l·N(x|μ_l,σ_l²),

fitted by EM. EM is sensitive to initialization and can converge to poor local optima; standard practice initializes with k-means or a threshold. In SAR water work the threshold (Otsu or KI) produces an initial hard split whose two partitions' sample means, variances, and relative counts become the initial μ, σ², and mixing proportions π for EM. Concrete examples: a two-level Otsu segmentation initializes an improved GMM refined by graph cut for lake extraction (*Journal of the Chinese Academy of Sciences* 2023 — "the two-level Otsu threshold method is used to obtain the initial segmentation map... the calculated parameter set is used as the initial parameter of the GMM"); and a Gabor + Otsu voting scheme initializes a dual-threshold GMM graph-cut water extractor (*Remote Sensing* 13(17):3465, 2021 — "we calculate the initial segmentation map as GMM parameters for estimating the probability distributions").

Which initializer is better? **KI is the stronger initializer for imbalanced/unequal-variance histograms** because its split already reflects the mixture's unequal σ and π, so EM starts closer to the true solution and is less likely to be dragged toward the dominant land mode. Otsu's equal-variance split places the initial boundary too far into the land class, giving biased initial μ_w, σ_w², and an inflated π_w. That said, KI's criterion can fail to converge or select a spurious threshold on nearly unimodal tiles (Landuyt et al. noted the KI-based algorithm "was non-convergent for a few orbital segments"), so the tile-selection metrics above matter more than the threshold choice. There is also a clean theoretical unification: Barron's Generalized Histogram Thresholding (GHT, 2020) shows Otsu, KI/MET, and weighted-percentile thresholding are all special cases of approximate MAP estimation of a two-Gaussian mixture with conjugate priors — "a kind of inverted expectation-maximization in which the sweep over threshold resembles an M-step and the assignment of latent variables... resembles an E-step."

### 6. Empirical comparisons

- **Dadhich et al. (2026), *Journal of Flood Risk Management*, e70201:** Compared Edge Otsu, Bmax Otsu, and KI, each crossed with Difference-Image, NDFI, and NDSI change detection, on the September 2019 NE-Thailand flood (Sentinel-1 VV+VH, Google Earth Engine). The best-performing combinations were NDSI+KI, Difference+KI, and NDFI+Edge-Otsu, with a reported accuracy of 86.29%; the paper characterizes KI as adjusting thresholds via a Gaussian mixture and favors NDSI+KI for robustness in complex landscapes. (Exact per-combination F1/kappa/CSI values are behind the Wiley paywall.)
- **Liang & Liu (2020), *ISPRS J. Photogramm. Remote Sens.* 159:53–62:** Benchmarked a new local thresholding method against Otsu, gamma-function fitting, and split-based KI on Sentinel-1. Qualitatively, Otsu's commission error was "more noticeable," gamma fitting's omission error was "rather large," and split-based KI performed reasonably; the proposed local method "can achieve higher accuracy with a better balance between omission and commission error." (Exact per-method error percentages are behind the Elsevier paywall.)
- **Landuyt et al. (2019), *IEEE TGRS* — "Flood mapping based on SAR: an assessment of established approaches":** Compared global, tiled, HAND-masked thresholding, active contour, change detection, and HSBA across UK/Ireland flood and lake scenes (ERS-2, ENVISAT, Sentinel-1), scored by Critical Success Index (CSI) and bias. Conclusion: the best method "depends on both the area of interest and its characteristics as well as the intended use of the observation product"; global thresholding "performs good on small image subsets" with clear bimodality but degrades on large/unimodal scenes — the motivation for tiled/HSBA approaches.
- **Travert et al. (2025 preprint, arXiv:2510.11305):** Ensemble study on the Garonne (Sentinel-1) treating Otsu vs KI as a hyperparameter; found global/local thresholding gave "moderate performance and low variability" and were "not very sensitive to their hyperparameters," with local thresholding sometimes matching a supervised CNN when tuned — evidence that method choice matters less than the tiling/refinement pipeline.
- **Comparative reviews** (e.g., an L-/C-band comparison in *Earth Science Informatics* 2024) benchmark Otsu, KI, GMM, Quality Index, and gamma-MLE for water delineation, reinforcing that no single thresholder dominates across bands and land covers.

### 7. Post-threshold refinement

The initial threshold is almost always refined, which compresses the Otsu-vs-KI difference:
- **Region growing** from high-confidence water seeds — Matgen et al. (2011) delineated seeds at the deviation point between empirical and fitted distributions and stopped at the water distribution's 99th percentile; Cao et al. (2018) used the water mode as seed and the KI threshold as the stopping condition.
- **Fuzzy logic** post-classification combining backscatter with topographic indices (DLR/Martinis–Twele chain).
- **Markov Random Field (MRF)** spatial-contextual smoothing (e.g., KI-MRF-SA pipelines; Martinis & Twele's hierarchical spatio-temporal Markov model for X-band).
- **HAND (Height Above Nearest Drainage)** exclusion masks remove hydrologically implausible low-backscatter pixels; GFM's exclusion mask also removes radar shadow/layover, "no sensitivity," permanent low-backscatter surfaces, and permanent/seasonal water bodies.

Once region growing + MRF + HAND are applied, sensitivity to the exact initial threshold (Otsu vs KI) is substantially reduced.

### 8. Known failure modes

- **Wind/rain roughening of water:** raises water backscatter and merges the water mode into land, causing omission (missed flood); both thresholders lose the bimodal separation. (Operational chains add multi-resolution reclassification specifically to recover wind-roughened water.)
- **Flooded vegetation (double-bounce):** water under vegetation/urban produces high, not low, backscatter (double bounce between the water surface and trunks/walls), so low-backscatter thresholding misses it entirely; InSAR coherence (Chini et al. 2019) or fuzzy/multi-threshold approaches are needed.
- **Radar shadow and layover** in mountains: low-backscatter shadow mimics water → commission error; handled by exclusion masks / local-incidence-angle layers, not by the thresholder.
- **Dry, smooth anthropogenic and natural surfaces (tarmac, airport runways, dry sand, saltpans):** "Calm water surfaces, characterized by specular reflection, produce low radar backscatter (σ0) values similar to those of smooth anthropogenic features like roads or airport runways, leading to frequent false positives" (*Journal of Water and Climate Change* 2025, 16(10):2901). Neither Otsu nor KI can separate these on backscatter alone; change detection or ancillary masks are required.

Neither method intrinsically handles these; they are addressed upstream (change detection, polarimetry, coherence) or downstream (masks).

### 9. Recent (2020–2026) work, deep learning, and operational systems

- **Copernicus EMS — Global Flood Monitoring (GFM):** near-real-time global Sentinel-1 flood product that "typically delivers flood maps within five hours of image acquisition" (with an ~8-hour outer bound in the Product User Manual). It is an ensemble of three independently developed algorithms combined by a pixelwise majority-voting rule where "at least two algorithms must classify a pixel as flooded or non-flooded": **LIST** (Chini et al. 2017 — HSBA hierarchical tiling + change detection + Bayesian inference), **DLR** (Martinis et al. 2015 / Twele et al. 2016 — hierarchical tile-based thresholding + region growing + fuzzy logic; the GFM PUM describes it as "non-parametric hierarchical tile-based thresholding"), and **TUW** (Bauer-Marschallinger et al. 2022 — datacube harmonic-model Bayesian classification). Exclusion masks (HAND, shadow/layover, permanent water) are applied. Documented overall accuracy is "greater than 94%."
- **NASA/ASF HydroSAR (SERVIR):** cloud-based Sentinel-1 RTC flood/inundation service (e.g., over the Hindu Kush Himalaya) using adaptive thresholding on RTC σ⁰ with HAND-based water-depth estimation, delivered within ~6 hours of acquisition.
- **Deep learning / weak supervision:** Otsu remains the dominant auto-labeler for training data (Sen1Floods11 "OTSU" labels; multiple CNN/UNet/transformer studies apply Otsu to VH to generate masks). Some use KI (KI-MRF-SA + CNN, *Water* 2023). Self-supervised methods (DeepAqua knowledge distillation) benchmark specifically against Otsu as the unsupervised baseline. DeepSARFlood and DeepSAR Flood Mapper (2025) explicitly compare MLP/transformer models against local Otsu, showing thresholding underestimates flooding in cropland/vegetated areas (e.g., Otsu recall 0.64, F1 0.76 in one flood case).

### 10. Software / open-source implementations

- **scikit-image:** `threshold_otsu` (widely used); `threshold_minimum` is a different histogram-valley method (not KI proper); no first-class KI, though community and MATLAB File Exchange ("Kittler–Illingworth Thresholding") implementations exist. `try_all_threshold` compares Otsu, Li, Yen, IsoData, Minimum, Triangle, etc.
- **OpenCV:** `cv2.threshold(..., THRESH_OTSU)` built in; KI not built in.
- **ESA SNAP / Graph Processing Framework:** used with KI in several operational chains (ESA RSS CloudToolbox flood mapping explicitly uses the KI algorithm via GPF XML).
- **Google Earth Engine:** Otsu is the de facto standard (Donchyts et al. edge-buffered Otsu); Edge-Otsu and Bmax-Otsu variants are common.
- **Operational chains:** DLR flood service, LIST HSBA (SBATool on GitHub, IES-SARLab), GFM processing chain (EODC), ASF HydroSAR — these embed KI/parametric or two-Gaussian tile thresholding rather than plain global Otsu.

### Summary comparison table

| Dimension | Otsu (1979) | Kittler–Illingworth (1986) |
|---|---|---|
| Criterion | Maximize between-class variance σ_B² = ω_f ω_b (μ_f−μ_b)² | Minimize J(T)=1+2[P₁logσ₁+P₂logσ₂]−2[P₁logP₁+P₂logP₂] |
| Underlying model | 2 Gaussians, **equal variance, equal prior** | 2-Gaussian mixture, **unequal variance, unequal prior** |
| Formal relationship | Special case of KI/MET (Xue & Zhang 2012) | General case; reduces to Otsu when σ₁=σ₂, P₁=P₂ |
| Behavior under class imbalance | Biases toward larger/higher-variance (land) class → overestimates water | Prior term ln(P₂/P₁) pulls threshold toward small water class → less biased |
| Non-Gaussian SAR extensions | Rare | GKIT (Moser & Serpico 2006), generalized-Gaussian/EM (Bazi et al. 2005) |
| Best scale | dB (log) to approach Gaussianity; poor in linear/gamma space | dB (log); non-Gaussian variants can work on amplitude/intensity |
| Convergence/robustness | Always returns a threshold (even if unimodal → meaningless) | Can be non-convergent / spurious on unimodal tiles |
| As GMM/EM initializer | Biased μ_w, σ_w², inflated π_w under imbalance | Closer to true unequal-variance/prior mixture → better EM start |
| Tooling | scikit-image, OpenCV, GEE (ubiquitous) | SNAP/GPF, HSBA/SBAT, MATLAB FEX; less built-in |
| Operational use | Weak-label generation, GEE flood workflows | DLR/GFM tile chains, LIST change detection, KI-MRF pipelines |
| Computational cost | Very low (single histogram pass) | Low (per-threshold sweep of P,μ,σ); slightly higher |

## Recommendations

1. **Default for single-image, imbalanced global/local thresholding: use KI (or a SAR-specific generalized KI) on log-transformed (dB) σ⁰.** KI's unequal-variance/unequal-prior terms directly counter the water-minority bias that afflicts Otsu. Switch to Otsu only when operating strictly inside pre-selected bimodal, class-balanced tiles (surface ratio near 50/50), where the two methods nearly coincide and Otsu's stability and tooling win.
2. **Always embed the thresholder in a tile/split framework (HSBA or SBAT) with explicit bimodality gating** — Ashman's D > 2, Bhattacharyya > 0.99, minimum class fraction > ~10% (Chini et al. 2017 defaults), and a minimum tile size large enough for stable statistics (e.g., ≥100×100 px; Travert et al. tested 100×100 and 200×200). If no tile passes these tests, do not threshold globally; fall back to change detection or a reference-water-mask/imbalance-aware method.
3. **Use the threshold to initialize a two-component GMM/EM, preferring KI over Otsu as the initializer** for imbalanced histograms; run EM with multiple restarts and select by log-likelihood/BIC. Then refine with region growing + MRF + HAND/exclusion masking.
4. **Choose polarization and scale deliberately:** VH often separates flooded/dry better than VV for open water; work in dB (Gaussian-in-log) for both methods; apply speckle filtering (Lee/Lee-Sigma/Frost or SAR2SAR), noting the filter and window size measurably shift flood extent (Travert et al. found variations of several square kilometers from filter choice alone).
5. **For operational/large-area products, ensemble** (as GFM does, with documented >94% accuracy) rather than betting on one thresholder; for deep-learning label generation where speed and standard tooling matter, Otsu is acceptable but validate against imbalance-driven overestimation and, where possible, use change-detection-conditioned or reference-mask labels.
6. **Benchmarks that would change these recommendations:** if a scene yields a clearly bimodal, class-balanced histogram (AD ≫ 2, balanced surface ratio), Otsu and KI converge and Otsu's simplicity is fine; if water < ~1% of the scene, abandon plain thresholding for reference-mask/imbalance-aware or change-detection methods; if flooded vegetation or urban double-bounce dominates, switch to InSAR coherence or polarimetric methods regardless of thresholder.

## Caveats

- **Paywalled exact numbers:** Precise per-method accuracy/F1/kappa/CSI/omission-commission values from Dadhich et al. (2026) and Liang & Liu (2020) could not be extracted from behind Wiley/Elsevier paywalls; the reported 86.29% (Dadhich) and directional conclusions come from accessible abstracts and citing literature and should be verified against the full results tables.
- **KI equation form:** The compact J(T) form is reproduced consistently across sources citing Kittler & Illingworth (1986); minor prefactor/notation differences (the factor of 2, the additive constant "1") appear between texts and do not change the argmin.
- **"Which is better" is context-dependent:** Multiple benchmark studies (Landuyt et al. 2019; Travert et al. 2025) conclude the best method depends on scene characteristics and intended use, and that after refinement/ensembling the thresholder choice is second-order. The KI preference stated here applies specifically to the raw single-thresholder step on imbalanced scenes.
- **Non-Gaussian reality:** In linear/intensity space SAR classes are gamma/non-Gaussian; both Gaussian-based methods rely on the dB transform to be approximately valid. Generalized (gamma/Weibull/generalized-Gaussian) KI variants exist but add estimation complexity and are less common in operational chains.
- **DLR characterization:** The GFM Product User Manual describes the DLR branch as "non-parametric hierarchical tile-based thresholding" with fuzzy logic and region growing; earlier literature and this report's KI/parametric-tile lineage framing should be read with that qualification.
- **Some cited works are preprints** (Travert et al. arXiv:2510.11305; GFM ensemble-likelihood arXiv:2304.12488) and were non-peer-reviewed at time of writing.