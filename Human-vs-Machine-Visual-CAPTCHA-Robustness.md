## Introduction

This is a draft for a research idea inspired by the [Breaking reCAPTCHAv2 paper](https://arxiv.org/abs/2409.08831).

The paper initially uses YOLOv8 to autonomously bypass reCAPTCHA v2 and claims to have achieved a 100% success rate, which is pretty scary if CAPTCHA relies solely on visuals to protect a page. However, CAPTCHA uses other methods as well, such as IP addresses, mouse movements, and other signals.

This research will focus only on protecting the visual aspect of CAPTCHA by answering the following question:

`Can we generate images that are ambiguous to ML models like YOLO but clear to the naked human eye?`

## Generating Images

For image generation, this research will use publicly available models to generate such ambiguous images by relying on prompt engineering. Examples include adding spikes to bread or changing the color of a crosswalk to red instead of one of its commonly used colors.

The potential models:

- Stable Diffusion XL (SDXL 1.0)
- FLUX.1 Schnell
- Qwen-Image

Each image will have an added effect, such as spikes, color changes, or other alterations. Each image type will be stored in a separate folder to test which effects are most ambiguous to AI models while remaining obvious to humans.

Initially, there will be five groups of images:

- Group 0: Ordinary images, without modifications.
- Group 1: Unusual object shapes, such as spikes or irregular geometry.
- Group 2: Unusual colors.
- Group 3: Texture and surface modifications.
- Group 4: Combinations of modifications.

### Experiment A

Groups with an index greater than 0 are modifications of Group 0.

- Start with ordinary images.
- Use image editing or image-to-image generation to produce modified versions.
- Groups 1–4 correspond to modifications of Group 0.
- Compare original images with their modified counterparts.

### Experiment B

Each group is independent, and none is a modification of another.

- Generate each group independently using prompts.
- Group 0 contains ordinary images.
- Groups 1–4 contain images generated with the corresponding visual characteristics.
- No image is derived from another group.

## Testing YOLO on the Samples

The images will then be passed to YOLO to evaluate its detection performance.

- Images recognized by YOLO will be classified into group \(X_i\).
- Images not recognized by YOLO will be classified into group \(Y_i\).

Here, \(i\) represents the index of the group being tested.

## Testing Humans

A Google Form will be created to test human participants using images from all five groups (0–4) and determine whether they can identify the objects.

- Images identified correctly by at least 75% of human participants will be classified into group \(H_i\).
- Images not identified correctly by at least 75% of human participants will be classified into group \(F_i\).

## Mixing Results Through Intersection and Union

- Images not identified by humans but recognized by YOLO will be classified into group \(\beta_i\) (the intersection of \(F_i\) and \(X_i\)).
- Images identified by most humans but not recognized by YOLO will be classified into group \(\alpha_i\) (the intersection of \(H_i\) and \(Y_i\)).
- Images identified by most humans and recognized by YOLO will be classified into group \(\omega_i\) (the intersection of \(H_i\) and \(X_i\)).
- Images not identified by either humans or YOLO will be classified into group \(\delta_i\) (the intersection of \(F_i\) and \(Y_i\)).

The thresholds for YOLO will be a confidence score of at least 55% and an IoU of at least 50%. The correct class must also be predicted.

Finally, we are going to compare Alpha Rates between each group.

**Alpha Rate:**

$$
\text{Alpha Rate}_i =
\frac{|\alpha_i|}{|G_i|} \times 100
$$

This represents the percentage of images in group \(G_i\) that are correctly recognized by humans but not correctly recognized by YOLO.

There is also the Human Recognition Rate:

**Human Recognition Rate:**

$$
\text{Human Recognition Rate}_i =
\frac{|H_i|}{|G_i|} \times 100
$$

This represents the percentage of images correctly recognized by humans.

Here, we are going to determine which modification is best for making images ambiguous to YOLO while keeping them recognizable to humans.

We also need to calculate the YOLO Detection Rate:

**YOLO Detection Rate:**

$$
\text{YOLO Detection Rate}_i =
\frac{|X_i|}{|G_i|} \times 100
$$

Here, \(G_i\) represents the complete image group being evaluated.

## Generating a New CAPTCHA Using Only Alpha Samples

In this step, we will generate or create a new CAPTCHA using only Alpha samples and test the project from the referenced paper against it to measure its identification rate.

We will have a baseline version of the CAPTCHA generated using ordinary images from Group 0 and experimental versions generated using Alpha samples.

The experimental evaluation will have two parts. In the first part, we will evaluate existing Alpha samples. In the second part, we will generate a CAPTCHA using images from the winning group.

## Conclusion

This isn't a breakthrough, but rather an experiment to determine whether it is possible to build such a CAPTCHA version. If it works, then, with more samples and enough time, a new type of CAPTCHA could be developed.