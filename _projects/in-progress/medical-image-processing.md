---
title: Medical imaging and image processing in MATLAB
timeframe: 2025–2026
order: 2
featured: true
summary: X-ray attenuation analysis, beam hardening simulation and image filtering, measured and compared quantitatively in MATLAB.
skills: [MATLAB, Image Processing Toolbox, X-ray physics, image filtering, data analysis]
tags: [medical-imaging, image-processing, matlab]
---

*Coursework from BME40005 Medical Imaging and EEE40017 Machine Vision, Swinburne University of Technology.*

Two pieces of work that show how I approach images as data: measuring what an X-ray image actually tells you, and testing which filters genuinely improve an image rather than just changing how it looks.

These are assessed labs that run every year, so this page explains the approach with short excerpts rather than publishing the full code.

## X-ray attenuation and beam hardening

In the BME40005 X-ray lab, I analysed radiographs of a brass step wedge and an aluminium wedge in MATLAB, compared the measurements with theory, and simulated how filtering changes an X-ray beam.

### Measuring transmission, and checking the measurement first

Transmission through each of the ten brass steps is the step's brightness relative to open background. I sampled three regions on every step rather than one, so the spread between them gives an honest error bar.

Before trusting any numbers, the script checks two things that would silently corrupt the results: sample regions that fall outside the image, and saturated pixels, which flatten the brightest values and compress the transmission range.

```matlab
% Fail loudly if any region falls outside the crop
assert(max([xSteps(:); xBG(:)]) <= size(img,2) && ...
       max(yBands(:))           <= size(img,1), ...
       'Region coordinates fall outside the cropped image.');

% Saturation check - clipped pixels would compress the transmission values
nSat = nnz(img >= 255);

T           = 100 * meanI / I_background;            % % transmitted, 3 samples x 10 steps
T_mean      = mean(T, 1);                            % best estimate per step
T_halfRange = (max(T, [], 1) - min(T, [], 1)) / 2;   % error bar from the spread
```

### Comparing with theory

To predict transmission at 70 keV, I needed attenuation coefficients from NIST tables, which only list 60 and 80 keV. Attenuation varies roughly as a power law between absorption edges, so the script interpolates on log-log axes instead of drawing a straight line, which would overestimate attenuation. The Beer–Lambert law then gives the expected transmission, with uncertainty carried through from the calliper measurements.

```matlab
% ln(mu/rho) is close to linear in ln(E), so interpolate in log space
frac      = log(70/60) / log(80/60);
logInterp = @(m60, m80) exp(log(m60) + frac*(log(m80) - log(m60)));

T  = 100 * exp(-mu .* x);   % Beer-Lambert transmission (%)
dT = T .* mu .* dx;         % error propagation: dT/dx = -mu*T
```

### Beam hardening

Real X-ray tubes produce a spread of energies, not one. I simulated spectra in SpekCalc through 0, 5 and 20 mm of aluminium and measured the effective attenuation over each thickness range. For a single-energy beam the two values would match. Instead, the effective attenuation fell as filtering increased: aluminium removes the low-energy photons first, so the beam that's left is "harder" and harder to stop. That's why X-ray tubes are filtered, and why simple single-energy models don't fully describe them.

```matlab
% Effective attenuation over each thickness interval (cm^-1).
% Equal for a monoenergetic beam; a falling value shows beam hardening.
muEff_0to5  = -log(Ntotal(2)/Ntotal(1)) / 0.5;   % first 5 mm of Al
muEff_5to20 = -log(Ntotal(3)/Ntotal(2)) / 1.5;   % next 15 mm of Al
```

### Turning brightness into thickness

For the aluminium wedge, I converted brightness into optical density and fitted it against the calliper-measured thickness of each step. The fitted line turns any pixel's brightness into a thickness, producing a calibrated 3D surface of the wedge. I also plotted the residuals, since curvature there would show the film leaving the linear part of its response.

```matlab
% Calibration: OD = OD_0 - k*x
p    = polyfit(t_al_mm, OD_steps, 1);
k    = -p(1);
OD_0 =  p(2);
R2   = 1 - sum((OD_steps - polyval(p,t_al_mm)).^2) / ...
           sum((OD_steps - mean(OD_steps)).^2);
```

<!-- Add one or two of your report figures here if you can export them
     (e.g. the transmission plot or the calibrated surface plot), e.g.
     ![Transmission through the brass step wedge](/images/projects/xray-transmission.png) -->

## Image filtering: what actually removes noise?

In EEE40017, I compared common filters on images with two kinds of noise. Rather than judging by eye, I measured each result against a clean reference using peak signal-to-noise ratio (PSNR), where higher means closer to the original.

```matlab
% PSNR in dB against a clean reference (higher = closer to the reference)
psnrdB = @(A, ref) 10*log10(255^2 / mean((double(A(:)) - double(ref(:))).^2));
```

**Smoothing masks** reduce noise, but blur detail as the mask grows:

![Five smoothing masks applied to an image with salt and pepper noise](/images/projects/filtering-part1-noise2.png)

**Laplacian sharpening** enhances edges. One detail matters here: the Laplacian produces negative values, so it has to be calculated in floating point. In 8-bit integers, every negative response would be clipped to zero and half the edge information lost.

```matlab
I  = double(img);                          % signed maths, not uint8
LA = imfilter(I, [0 1 0; 1 -4 1; 0 1 0], 'replicate');
sharpened = uint8(I - LA);                 % centre weight is negative, so subtract
```

![Laplacian edge detection and sharpening on a clean image](/images/projects/filtering-part2-sharp.png)

**Min, max and median filters** show the clearest lesson. For salt and pepper noise, min and max filters spread the noise into blotches, while a median filter removes it almost completely and keeps edges sharp:

![Min, max and median filters of three sizes on an image with salt and pepper noise](/images/projects/filtering-part4-noise2.png)

The filter has to match the noise. Gaussian noise responds well to smoothing, but impulse noise needs a non-linear filter like the median. In medical imaging that matters, because a filter that blurs edges can hide exactly the detail a clinician needs.

## What I learned

<!-- In your own words: e.g. why measuring (PSNR, error bars) beats judging by eye,
     what surprised you about beam hardening, or how this connects to your
     bacteria-counting project. -->
