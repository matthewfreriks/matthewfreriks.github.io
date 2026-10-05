---
title: Medical imaging and image processing in MATLAB
status: done
timeframe: 2025–2026
order: 2
featured: true
summary: X-ray attenuation analysis, beam hardening simulation and image filtering, measured and compared quantitatively in MATLAB.
skills: [MATLAB, Image Processing Toolbox, X-ray physics, image filtering, data analysis]
---

*Coursework from BME40005 Medical Imaging and EEE40017 Machine Vision, Swinburne University of Technology.*

Two pieces of work that show how I approach images as data: measuring what an X-ray image actually tells you, and testing which filters genuinely improve an image rather than just changing how it looks.

## X-ray attenuation and beam hardening

In the BME40005 X-ray lab, I analysed radiographs of a brass step wedge and an aluminium wedge in MATLAB. My script:

- **Measured transmission through each of the ten brass steps,** sampling three regions per step and comparing against open background, with error bars from the spread of the samples.
- **Checked for saturated pixels,** since clipped values would compress the results.
- **Compared the measurements with theory,** calculating expected transmission at 70 keV from published attenuation coefficients using the Beer–Lambert law, with error propagation from the thickness measurements.
- **Built a thickness-calibrated surface plot** of the aluminium wedge, converting image intensity into optical density and calibrating it against calliper measurements.

I also simulated X-ray spectra in SpekCalc through 0, 5 and 20 mm of aluminium. Analysing them showed **beam hardening**: aluminium removes low-energy photons first, so the beam's mean energy rises and its effective attenuation falls as filtering increases. That's why real X-ray tubes are filtered, and why simple single-energy models don't fully describe them.

<!-- Add one or two of your report figures here if you can export them
     (e.g. the transmission plot or the calibrated surface plot), e.g.
     ![Transmission through the brass step wedge](/images/projects/xray-transmission.png) -->

## Image filtering: what actually removes noise?

In EEE40017, I compared common filters on images with two kinds of noise, measuring each result against a clean reference using PSNR rather than judging by eye.

**Smoothing masks** reduce noise, but blur detail as the mask grows:

![Five smoothing masks applied to an image with salt and pepper noise](/images/projects/filtering-part1-noise2.png)

**Laplacian sharpening** enhances edges, and the 8-neighbour version is stronger. On a noisy image, though, it amplifies the noise as well, a good example of a filter that "works" while making the image worse:

![Laplacian edge detection and sharpening on a clean image](/images/projects/filtering-part2-sharp.png)

**Min, max and median filters** show the clearest lesson. For salt and pepper noise, min and max filters spread the noise into blotches, while a median filter removes it almost completely and keeps the edges sharp:

![Min, max and median filters of three sizes on an image with salt and pepper noise](/images/projects/filtering-part4-noise2.png)

The choice of filter has to match the type of noise. Gaussian noise responds well to smoothing, but impulse noise needs a non-linear filter like the median. In medical imaging that matters, because a filter that blurs edges can hide exactly the detail a clinician needs.

## What I learned

<!-- In your own words: e.g. why measuring (PSNR, error bars) beats judging by eye,
     what surprised you about beam hardening, or how this connects to your
     bacteria-counting project. -->
