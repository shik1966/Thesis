# Defense Presentation Script & Timing Guide

**Total Time Target: 15-20 minutes + 5-10 minutes Q&A**

---

## **Slide 1: Title Slide** (30 seconds)
**Script:**
"Good morning/afternoon, committee members. I'm Marwan Shakib, and I'm here to present my bachelor thesis on 'RGB Images Generation Based on Feature Detection Techniques: The Case of SegNet.' This work combines classical computer vision techniques with modern deep learning for brain tumor segmentation."

**Key Points:**
- State your name clearly
- Mention thesis title
- Thank the committee for their time

---

## **Slide 2: Acknowledgments** (45 seconds)
**Script:**
"Before I begin, I'd like to acknowledge those who made this research possible. My thesis supervisor provided invaluable guidance in medical image analysis and deep learning. The German University in Cairo's Computer Science Department provided essential computational resources and GPU infrastructure. I'm grateful to the BraTS challenge organizers for the comprehensive dataset, and special thanks to Abdullah Hany for his help with SegNet implementation. Finally, thank you to my fellow researchers for technical discussions and my family for their unwavering support throughout this journey."

**Timing Note:** Cover the key supporters efficiently - aim for 45 seconds maximum to show gratitude without losing momentum.

---

## **Slide 3: Motivation & Problem Statement** (1.5 minutes)
**Script:**
"Brain tumor segmentation is critical because accurate tumor boundaries directly guide surgical decisions and radiotherapy targeting. Currently, manual segmentation takes hours per patient and varies between experts - we urgently need automated solutions.

Here's the challenge: U-Net achieves high accuracy but requires 8-12 GB of GPU memory, which isn't available in many hospitals. Classical methods are fast but struggle with complex tumors. Most approaches process raw MRI data, missing valuable structural information that radiologists naturally recognize.

This creates our opportunity: What if we combine classical computer vision with modern deep learning? We can utilize domain knowledge like edges, textures, and blobs while maintaining accuracy and reducing computational requirements. This bridges the gap between efficiency and precision."

**Key Points:**
- Keep it concise and impactful
- Emphasize the practical problem clearly
- Position your research as the logical solution
- Set up the hybrid approach as innovative

---

## **Slide 4: Research Objectives & Questions** (1.5 minutes)
**Script:**
"This led me to four main objectives: developing a feature-enhanced preprocessing pipeline, adapting SegNet for multi-class segmentation, conducting comprehensive evaluation of different methods, and analyzing clinical feasibility.

The key research questions I wanted to answer were: Can classical features improve modern CNN performance? Which feature detection method works best? And does combining multiple features yield better results than individual methods?"

**Key Points:**
- Be clear about your research goals
- These questions guide your entire methodology

---

## **Slide 5: Background - Brain Tumor Segmentation** (2 minutes)
**Script:**
"Let me explain the key components of brain tumor segmentation. We work with three complementary MRI modalities, each providing unique information. T1 with contrast enhancement, which we map to our red channel, highlights active tumor boundaries where the blood-brain barrier is compromised. T2, our green channel, shows the full extent of the tumor and surrounding edema due to its sensitivity to water content. FLAIR, mapped to blue, suppresses cerebrospinal fluid signal, making edema near the ventricles much clearer.

It's worth noting that while the BraTS dataset includes T1 modality, we excluded it based on my supervisor's clinical expertise, focusing on the most diagnostically relevant contrasts.

We segment three distinct tumor regions, each with specific BraTS labels: Tumor Core, labeled as 1, includes both necrotic and non-enhancing tumor mass. Peritumoral Edema, label 2, represents surrounding tissue swelling, particularly visible on T2 and FLAIR. Enhancing Tumor, label 4, indicates active tumor regions that light up with contrast. Everything else is labeled as background, label 0.

This multi-label approach is crucial because each region provides unique diagnostic information, guides treatment decisions, and helps monitor tumor progression over time."

**Key Points:**
- Emphasize how each modality contributes unique information
- Explain the T1 exclusion decision confidently
- Connect labels to clinical significance
- Highlight the importance of multi-label segmentation

---

## **Slide 6: Why SegNet & Our Enhancements** (3 minutes)
**Script:**
"I chose SegNet because it's uniquely memory-efficient. Unlike U-Net which stores full feature maps requiring 8-12 GB of GPU memory, SegNet only stores pooling indices and uses them for unpooling. This reduces memory requirements by about 50% while preserving spatial information. It is also more clinically feasible 

However, SegNet can produce smoother segmentations than desired. My innovation was to enhance it with classical feature detection in the preprocessing stage, creating a hybrid approach that maintains efficiency while improving accuracy.

Let me explain our specific enhancements in detail. We designed the network to take 240x240 three-channel inputs, preserving the native BraTS resolution without interpolation. The output directly produces four-class predictions, mapping to the BraTS labels {0,1,2,4} for background, tumor core, edema, and enhancing tumor.

A critical design choice was the model size. At 8.6 million parameters, it's substantially lighter than VGG-style SegNets with 29 million parameters, yet more capable than simple binary variants. We added skip connections to preserve fine spatial details and implemented learned upsampling through Conv2DTranspose layers, which produces sharper segmentation masks compared to simple unpooling. For robustness, we incorporated modern features like BatchNorm and Dropout at each block.

This hybrid design combines the best of both worlds - SegNet's memory efficiency with modern architectural improvements. It's practical for clinical deployment, handles multi-class prediction natively, and remains easy to modify thanks to standard Keras layers. Most importantly, it creates a foundation for our feature-enhanced preprocessing approach, bridging classical computer vision with deep learning."

**Key Points:**
- Start with original SegNet advantages and limitations
- Explain each enhancement's purpose
- Emphasize practical clinical considerations
- Connect to overall hybrid approach strategy
- Show understanding of model scaling trade-offs

---

## **Slide 7: Classical Feature Detection Techniques** (2 minutes)
**Script:**
"I evaluated three classical methods: Sobel edge detection captures tumor boundaries and structural edges - exactly what we need for precise segmentation. Gabor texture analysis captures texture patterns in different orientations, which is effective for heterogeneous tumor regions. Laplacian-of-Gaussian detects blob-like structures and fine details, useful for identifying tumor cores.

Each method provides complementary information that raw intensities alone cannot capture. Sobel detects edges and boundaries crucial for tumor delineation. Gabor filters capture texture patterns at different orientations, important for heterogeneous tumor regions with varying internal structures. Laplacian-of-Gaussian detects blob-like structures and fine details, perfect for identifying tumor cores.

I chose these three because they represent different categories of feature detection - edge detection, texture analysis, and blob detection - and are well-established in medical image analysis. They complement each other and cover the main visual features radiologists look for.

Finally, I combined all three methods to see what would happen when we integrate their complementary strengths - creating a comprehensive feature representation that leverages the synergistic effects of multiple classical techniques."

---

## **Slide 8: RGB Fusion Approach** (2 minutes)
**Script:**
"Let me explain my preprocessing pipeline. First, I established a baseline by carefully mapping the MRI modalities to specific RGB channels. I assigned T1-contrast enhanced to red because enhancing tumors appear brightest in T1CE, making them naturally stand out. T2 went to the green channel as it provides excellent soft tissue contrast and edema visibility. FLAIR was mapped to blue, which works well for highlighting suppressed CSF signals and peripheral edema. This creates an intuitive visualization where active tumors appear reddish, edema shows in green-blue tones, and normal tissue maintains balanced intensity.

Then, I systematically explored feature enhancement. For each modality, I applied Sobel filtering to enhance edge information, capturing sharp tumor boundaries. With Gabor filters, I extracted texture patterns at multiple scales and orientations, particularly effective for heterogeneous regions. The Laplacian-of-Gaussian operator helped detect both fine edges and blob-like structures characteristic of tumor cores. Finally, I developed a combined approach that integrated all three feature detectors to leverage their complementary strengths.

In total, I evaluated five distinct approaches: raw intensity baseline, Sobel edge detection, Gabor texture analysis, Laplacian blob detection, and the combined feature stack. Each method was carefully tuned for medical imaging characteristics and evaluated using multiple performance metrics."

**Key Points:**
- Explain the rationale behind RGB channel mapping
- Detail how each feature detector enhances specific tumor characteristics
- Emphasize the systematic progression from simple to complex
- Highlight the innovation of combining all three methods

---

## **Slide 8b: Data Preprocessing Details** (1.5 minutes)
**Script:**
"Let me explain the key preprocessing decisions in my pipeline. I chose to preserve the native 240×240 pixel resolution of the BraTS MRI slices, rather than resizing to 256×256. This decision was crucial for maintaining anatomical fidelity and avoiding interpolation artifacts. It also aligns perfectly with SegNet's downsampling structure, keeping our training efficient.

For each patient, I carefully selected a balanced set of slices: 10 with the largest tumor areas to capture complex cases, 10 with the smallest tumors to ensure sensitivity to subtle abnormalities, and 10 background slices to train the model on healthy tissue. The modalities were stacked as RGB channels while maintaining their original intensities, with only minimal normalization to the [0,1] range applied per channel.

These choices reflect a deliberate minimalist approach - avoiding unnecessary preprocessing steps that could introduce artifacts or distortions. This creates a clean baseline for evaluating our feature detection methods and ensures that any improvements we see are genuinely from our enhancement techniques rather than preprocessing artifacts."

**Key Points:**
- Justify the resolution choice
- Explain the balanced sampling strategy
- Emphasize minimal preprocessing philosophy
- Connect to feature detection evaluation

---

## **Slide 9: Experimental Setup** (1.5 minutes)
**Script:**
"Starting with the complete BraTS2020 dataset of 369 patients, I constructed a balanced dataset of 900 total slices using a careful subsampling strategy. For each patient, I selected 10 slices with the largest tumor areas, 10 with the smallest nonzero tumor areas, and 10 background slices to ensure representation of all cases.

For the initial experiments with individual feature detectors, I used a dataset of 900 slices split into 629 training samples (70%), 136 validation samples (15%), and 135 test samples (15%). The model was trained for 20 epochs with a batch size of 16, using the Adam optimizer and model checkpointing to save the best weights based on validation Dice score.

However, for the final combined feature approach, I significantly expanded the dataset to approximately 11,000 slices and extended training to 50 epochs. This expanded dataset provided much richer training examples and allowed the model to better learn the complementary features. For evaluation, I used two complementary types of metrics. Region-based metrics include the Dice coefficient and IoU, which measure segmentation overlap and similarity. Boundary-based metrics include HD95 and ASSD, which specifically assess the accuracy of tumor boundaries - HD95 captures the maximum deviation while ASSD provides the average boundary error.

Importantly, I employed a dual evaluation policy. The Full policy includes all slices and counts empty slices as perfect scores, showing overall system reliability. The Simple policy evaluates only tumor-containing slices, providing a more realistic and stringent assessment of clinical performance."

**Key Points:**
- Explain the balanced subsampling approach
- Detail the precise dataset splits
- Emphasize the importance of dual evaluation
- Connect metrics to clinical relevance

---

## **Slide 10: Results Overview - Combined Feature Approach** (2 minutes)
**Script:**
"Let me present the results achieved by our combined preprocessing approach, which integrates Sobel edge detection, Gabor texture analysis, and Laplacian blob detection. Enhancing Tumor showed the best overall performance, with a Dice coefficient of 0.842 under Full Policy and 0.590 under Simple Policy. The sharp boundary detection, indicated by an HD95 of just 2.77, shows our method's strength in capturing well-defined tumor boundaries.

Tumor Core also demonstrated strong performance, achieving a Dice of 0.817 under Full Policy and 0.526 under Simple Policy. The good boundary precision with an HD95 of 3.66 suggests reliable structural detection. However, Edema proved to be our most challenging region, with Dice scores of 0.590 and 0.381 under Full and Simple policies respectively. The higher HD95 of 6.97 reflects the difficulty in precisely delineating these diffuse boundaries.

These results reveal important insights: while our method performs well overall, the significant drop between Full and Simple policies highlights the real challenges in clinical tumor segmentation. The combined feature approach particularly benefits boundary detection, though further work is needed to improve edema segmentation."

**Key Points:**
- Present results honestly - don't oversell
- Acknowledge complexity
- Frame challenges as insights, not failures

---

## **Slide 11-13: Individual Results** (3 minutes total)
**Script:**
"Let me show you some visual results. The baseline raw intensity approach shows the limitations - smoother boundaries and missed fine details. The combined features approach shows sharper boundaries and better detail preservation.

Each individual method contributed differently: Sobel excelled at boundary delineation, Gabor captured texture patterns effectively, and Laplacian highlighted fine structural details. The synergistic effect of combining all three created the most comprehensive feature representation."

**Key Points:**
- Use visuals to support your claims
- Explain what the audience is seeing
- Connect back to your methodology rationale

---

## **Slide 14: Computational Efficiency** (1.5 minutes)
**Script:**
"Critically for clinical deployment, we maintained computational efficiency. Our SegNet plus features approach uses only 4-6 GB of GPU memory compared to 8-12 GB for standard U-Net - a 50% reduction. Feature extraction adds only 2-3 seconds per case, which is acceptable for clinical workflow."

**Key Points:**
- Emphasize practical applicability
- Quantify the efficiency gains
- Address real-world constraints

---

## **Slide 15: Key Contributions** (2 minutes)
**Script:**
"My main contributions are: First, a novel preprocessing pipeline that's the first systematic evaluation of classical features with SegNet. Second, comprehensive feature analysis validated on the standard BraTS dataset. Third, a clinically feasible solution that maintains memory efficiency. Fourth, demonstrated performance improvements with better boundary precision."

**Key Points:**
- Clearly state your contributions
- Emphasize novelty and validation
- Connect to practical impact

---

## **Slide 16-17: Limitations & Future Work** (2 minutes)
**Script:**
"I acknowledge several limitations: the 2D slice-based approach could benefit from 3D volumetric processing, feature parameters required manual tuning, and I focused only on SegNet architecture.

For future work, I see exciting directions: 3D volumetric implementation, attention mechanisms, advanced fusion strategies, and most importantly, clinical validation with radiologist evaluation."

**Key Points:**
- Show intellectual honesty about limitations
- Demonstrate you understand next steps
- Show vision for impact

---

## **Slide 18: Key Takeaways** (1.5 minutes)
**Script:**
"The key takeaways are: Classical features can enhance deep learning with measurable improvements. Combined approaches work through synergistic effects. Clinical feasibility is maintained through memory efficiency. And most importantly, hybrid approaches combining domain knowledge with modern AI show real promise for medical imaging."

**Key Points:**
- Synthesize your main findings
- Connect to broader implications
- End on a strong note about significance

---

## **Slide 19: Thank You** (30 seconds)
**Script:**
"Thank you for your attention. I'm happy to answer any questions about the methodology, results, or implications of this work."

**Key Points:**
- Express appreciation
- Open the floor confidently
- Be ready for questions

---

## **Q&A Preparation**

### **Expected Questions & Answers:**

**Q: "Why not use 3D approaches?"**
A: "3D would be ideal for spatial consistency, but I wanted to focus on the core question of whether classical features enhance CNNs. The 2D approach allowed comprehensive evaluation of multiple feature types. 3D implementation is definitely the next step."

**Q: "How does this compare to state-of-the-art methods?"**
A: "My focus was on the specific question of classical feature integration rather than achieving state-of-the-art performance. The improvements I showed demonstrate the principle works, and could be applied to other architectures for even better results."

**Q: "What about computational overhead?"**
A: "Feature extraction adds 2-3 seconds per case, but we save 50% GPU memory. For clinical workflow, this trade-off is very favorable - many hospitals can't afford high-end GPUs but can accept slightly longer processing."

**Q: "Statistical significance?"**
A: "I used standard BraTS evaluation protocols. While the improvements are modest, they're consistent and demonstrate the principle. Larger studies would be needed for clinical deployment."

**Q: "Why did you choose these specific three classical methods for Slide 7?"**
A: "I selected Sobel, Gabor, and Laplacian-of-Gaussian because they capture fundamentally different image properties that are clinically relevant. Sobel detects edges and boundaries, which are crucial for tumor delineation. Gabor filters capture texture patterns at different orientations, important for heterogeneous tumor regions that have varying internal structures. Laplacian-of-Gaussian detects blob-like structures and fine details, perfect for identifying tumor cores. These three methods complement each other and cover the main visual features radiologists look for."

**Q: "Did you experiment with other classical feature detection methods?"**
A: "I focused on these three because they represent different categories of feature detection - edge detection, texture analysis, and blob detection. I did consider methods like Harris corner detection and SIFT, but they're more suited for natural images rather than medical imaging. The methods I chose are well-established in medical image analysis and have proven effectiveness for MRI data."

**Q: "How do you handle parameter tuning for the classical methods?"**
A: "That's a great question and actually one of the limitations I acknowledge. For this study, I used standard parameters - Sobel with 3x3 kernels, Gabor with frequency 0.6 and orientation angles from 0° to 135°, and LoG with sigma 1.0. Ideally, these should be optimized for each dataset, which represents an opportunity for future work using automated parameter optimization techniques."

**Q: "How do the classical features integrate with the original MRI intensity values?"**
A: "The features are added to the corresponding intensity values in each channel before normalization. So for example, in the red channel, we have T1-contrast + Sobel edges, in green we have T2 + texture features, and in blue we have FLAIR + LoG features. This preserves the original anatomical information while enhancing structural details the CNN can learn from."

### **Timing Tips:**
- Practice with a timer - aim for 18 minutes max
- If running long, skip backup slides
- If running short, elaborate on key results
- Leave adequate time for Q&A
- Have water ready - talking for 20+ minutes is thirsty work!

### **Delivery Tips:**
- Maintain eye contact with committee
- Use a pointer/laser for highlighting results
- Speak clearly and not too fast
- Show enthusiasm for your work
- Be confident but not arrogant 