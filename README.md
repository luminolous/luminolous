<p align="center">
  <img src="public/banner.gif" width="900" alt="banner">
</p>

I work in deep learning, but what actually holds my attention isn't the output, it's what's happening underneath: how a network ends up representing whatever you feed it, and whether that representation survives being pushed somewhere it was never trained for. Most of my projects start from adapters, LoRA variants, expert routing, or frozen backbones with a bit of task memory bolted on, since that's usually where you can see what a representation is carrying, and the writeups rarely stay as short as I plan them to.

That same question pulls me toward cross-modal generative work, mainly feeding EEG into a frozen diffusion model's latent space, which is the one that actually worked, and toward JEPA and world models, since both skip reconstruction and just predict something useful about the input instead. Most of it lands back in neuroscience, EEG especially, since biological systems handle perception and memory in ways deep learning keeps rediscovering under different names, and these projects tend to grow into long ablations rather than tidy benchmarks, because I care more about the question than a clean result.

## **「Featured Works」**

Here are some highlights:

- [**Hessian-Guided Feature Injection in U-Net for Nucleus Instance Segmentation**](https://github.com/luminolous/HFI-UNet)
  - Developed HFI-UNet, injecting six analytically-derived Hessian-Morse feature channels (local maxima, saddle point, determinant, mean curvature, SDF approximation, eigenvalue ratio) as explicit geometric priors into a multi-head U-Net for nucleus instance segmentation on H&E histopathology images, with a marker-based watershed post-processing pipeline for final instance separation.
  - Designed a multi-head architecture (semantic mask, SDF, boundary) trained under a combined Dice + SDF + boundary loss, and ran three cross-organ ablations on CryoNuSeg (30 images, 10 organ types) to isolate the contribution of channel injection, loss weighting, and cumulative feature stacking.
  - Found that stacking all six Hessian channels on top of RGB (9-channel input) achieved the best AJI (0.395±0.007) and PQ (0.298±0.006) over RGB-only (AJI 0.381), that Dice+boundary loss outperformed the proposed three-term loss (AJI 0.383, BF1 0.644), and that a rejected multi-scale Hessian extension with magnitude gating collapsed performance to AJI 0.170 — a negative result documented as a design boundary.

- [**Implementation of Mixture of Low-Rank Adapter Experts (X-LoRA) Architecture in English-Indonesian Cross-Lingual Adaptation with Qwen2.5-0.5B**](https://github.com/luminolous/CrossLingual-WaguriAI)
  - Researched parameter-efficient bilingual LLM adaptation by fine-tuning Qwen2.5-0.5B-Instruct with separate Indonesian and English LoRA experts.
  - Designed a data mining workflow for large-scale instruction datasets, including missing-value filtering, token distribution analysis, semantic embedding, clustering, and representative sampling.
  - Explored X-LoRA-based adapter composition to combine language-specific LoRA experts and evaluated multilingual response generation using BLEU and ROUGE metrics.

- [**Frequency-Decomposed Complex-State SSM Encoder for EEG Visual Decoding with Frozen Diffusion Evaluation**](https://github.com/luminolous/EEG-BandMamba)
  - Developed FreqMamba-3, a frequency-decomposed Mamba-3 encoder that splits EEG signals into five neural bands (delta, theta, alpha, beta, gamma) via a Butterworth filter bank before shared-weight SSM encoding and learned softmax band fusion, aligning EEG embeddings to CLIP's 768-dim image space through InfoNCE contrastive training.
  - Benchmarked the proposed encoder against CNN (EEGNet), Transformer (EEG-Conformer), and standard Mamba-3 baselines on EEG-ImageNet (62-channel, 500-timestep signals) using a frozen Stable Diffusion 1.5 generator, isolating all performance differences to the encoder design.
  - Ran three ablation scenarios (architecture comparison, hyperparameter sensitivity, loss function study) and found FreqMamba-3 nearly doubled embedding alignment over CNN/Transformer baselines (cosine similarity 0.068 vs. 0.028–0.038) at identical parameter count (802K), with InfoNCE+Cosine loss further boosting alignment to 0.249.

<!-- - [**NH-LoRA: Future-Aware Structural Expansion of Low-Rank Adapters for Rehearsal-Free Class-Incremental Learning**](https://github.com/luminolous/NH-LoRA)
  - Developed NH-LoRA, a parameter-efficient continual learning architecture for rehearsal-free class-incremental image classification using a frozen Vision Transformer and low-rank adapter expansion.
  - Designed task-aware adaptation mechanisms including a Task-State Encoder, Horizon Planner, Instance Router, and Consolidation-Homeostasis Unit to balance plasticity, stability, and parameter growth.
  - Implemented and evaluated the NH-LoRA training pipeline on continual image classification benchmarks using metrics such as Average Incremental Accuracy, Final Average Accuracy, and Forgetting.

- [**Multi-Task Learning with DNABERT for Joint gRNA On-Target and Off-Target Prediction**](https://github.com/luminolous/CRISPR-MTL)
  - Fine-tuned DNABERT as a shared encoder to jointly predict CRISPR-Cas9 gRNA on-target efficiency (regression) and off-target activity (classification) within a single multi-task network.
  - Designed a two-phase fine-tuning schedule with selective layer unfreezing (layers 9–12) and ran four ablation variants, finding that full-layer fine-tuning caused catastrophic forgetting on small-scale genomic data (Spearman 0.769 vs. 0.812).
  - Applied Integrated Gradients to compare attribution patterns between task heads and evaluated performance via 5-fold cross-validation. MTL-Full achieved Spearman 0.812 on-target and AUROC 0.811 off-target, outperforming single-task DNABERT (AUROC 0.771). -->

> For more of my work, including unfinished or unpublished projects, check my [personal website](https://luminolous.site) or browse my [repository list](https://github.com/luminolous?tab=repositories) directly.

## 「Currently reading」

I don't keep a clean reading list, just whatever's actually feeding into an experiment right now, and lately that's almost entirely JEPA and world models. The starting point is ["Value-guided action planning with JEPA world models"](https://arxiv.org/abs/2601.00844), which pokes at something I keep running into myself: a good representation doesn't automatically buy you good planning, and the gap between the two is bigger than most JEPA papers seem willing to admit. ["Causal-JEPA"](https://arxiv.org/abs/2602.11389) pushes the masking down to the object level instead of patches, and I'm still not sure if that actually generalizes better or just happens to fit their benchmarks well. The one I keep coming back to is ["LeWorldModel"](https://arxiv.org/abs/2603.19312), a roughly 15-million-parameter model trained end-to-end with no frozen foundation encoder underneath it, because if that holds up under more scrutiny it says something uncomfortable about how much of the current scale obsession is actually necessary. Rounding it out is ["Var-JEPA"](https://arxiv.org/abs/2603.20111), which tries to give JEPA a proper generative backbone instead of leaving it as just a prediction loss, and I honestly don't know yet whether that's a meaningful reframing or just a nicer way of writing the same objective.