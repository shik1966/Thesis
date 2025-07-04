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

## **Slides 10a-d: Raw Intensity Results** (2.5 minutes)
**Script:**
"Let me walk you through our baseline results using raw MRI intensities without any preprocessing.

For Slide 10a - Training Performance:
The training history reveals three distinct phases. We saw rapid improvement in the first 8 epochs, with Dice coefficient rising from 0.20 to 0.67. This was followed by steady refinement, reaching a plateau around 0.80. Most importantly, our validation Dice stabilized at 0.799, with close tracking between training and validation curves indicating good generalization.

For Slide 10b - Test Set Metrics:
Under Full Policy, which includes empty slices, we achieved strong results: Tumor Core Dice of 0.835, Edema 0.634, and Enhancing Tumor 0.873. However, the Simple Policy, excluding empty slices, revealed the true challenge - with Dice scores dropping by 38-34% across all classes.

Let me explain the pixel-level analysis, which gives us deeper insights. Looking at True Positives - the pixels we correctly identified as tumor - Edema had the highest count at 43,447, followed by Enhancing Tumor at 14,422 and Tumor Core at 7,907. These numbers reflect the relative sizes of these tumor regions in our dataset.

For False Positives - where we incorrectly labeled healthy tissue as tumor - Edema again showed the highest count at 18,589. This translates to about 138 false positive pixels per slice on average, indicating the model tends to over-segment edema regions. In contrast, Tumor Core had only 14 false positives per slice, showing more conservative predictions.

False Negatives - actual tumor pixels we missed - were also highest for Edema at 14,965 total, or about 111 per slice. This means we're missing significant portions of edema regions. Tumor Core had 47 false negatives per slice, explaining its low recall of 0.555.

The precision values tell us how reliable our positive predictions are. Tumor Core achieved the highest precision at 0.805, meaning when we predict tumor core, we're correct 80.5% of the time. Edema's lower precision of 0.700 reflects its tendency to over-segment.

Recall, or sensitivity, measures how well we detect actual tumor pixels. Enhancing Tumor performed best with 0.837 recall, detecting 83.7% of true enhancing tumor pixels. Tumor Core's low recall of 0.555 means we're missing almost half of the actual tumor core regions.

Specificity was extremely high across all classes - above 0.997 - which simply reflects that we correctly identify most background pixels. With millions of background pixels, even small error rates translate to the false positive counts we see.

These pixel-level metrics reveal that while our baseline performs reasonably well overall, it struggles with precise boundary delineation, particularly for the diffuse edema regions. This motivated our exploration of edge detection techniques."

For Slide 10c - Qualitative Analysis:
Looking at our prediction examples, we can see both strengths and limitations. Large tumors are generally captured but with over-segmentation. Small lesions are frequently missed or misidentified. The boundary precision is notably fuzzy, with imprecise edges.

For Slide 10d - Data Samples & Next Steps:
Our dataset split of 629 training, 136 validation, and 135 test samples provided good coverage of tumor variations. The fuzzy predictions suggested the model needs help identifying tissue boundaries, leading us to explore edge detection techniques next."

**Key Points:**
- Emphasize the strong baseline but clear limitations
- Highlight the gap between Full and Simple policies
- Use visual examples to illustrate challenges
- Set up the motivation for edge detection

## **Slides 11a-d: Sobel Edge Detection Results** (2.5 minutes)
**Script:**
"Moving to our first feature enhancement approach using Sobel edge detection.

For Slide 11a - Implementation & Training:
We implemented Sobel filtering to enhance boundary information, computing gradient magnitude in both x and y directions. Training showed steady improvement but with more fluctuations than the baseline.

For Slide 11b - Test Performance:
The results were surprising - while precision remained high at 0.806 for tumor core, recall actually decreased to 0.433 compared to raw intensity's 0.555. This suggested that pure edge enhancement might be removing important contextual information.

Looking at the pixel-level analysis reveals why Sobel struggled. For True Positives, we saw fewer correctly identified pixels: Edema at 39,450, Enhancing Tumor at 12,192, and Tumor Core at only 6,166. The edge filtering was making the model more conservative in its predictions.

False Positives decreased for Tumor Core to just 11 per slice, showing the model became very cautious about predicting tumor core. However, Edema still had 117 false positives per slice, indicating persistent over-segmentation issues. The precision improvement for Tumor Core (0.806) came at the cost of missing many actual tumor pixels.

False Negatives increased dramatically - Tumor Core had 60 false negatives per slice compared to 47 with raw intensities. This explains the recall drop from 0.555 to 0.433. We were missing even more actual tumor tissue than before.

The high specificity (>0.998) remained excellent, but the precision-recall trade-off was unfavorable. While we reduced false alarms, we were missing too much actual tumor tissue. This suggested that edge information alone was insufficient for robust segmentation.

For Slide 11c - Qualitative Results:
The visual examples show clearer anatomical boundaries, but this didn't translate to better segmentation. Examples 1, 2, and 4 demonstrate improved edge definition but also reveal a tendency toward under-segmentation.

For Slide 11d - Critical Learning:
This experiment taught us that while edge information was valuable, it wasn't sufficient by itself. We needed to capture more complex tissue characteristics, leading us to explore texture analysis through Gabor filtering."

## **Slides 12a-d: Gabor Results** (2.5 minutes)
**Script:**
"Our next approach leveraged Gabor filters for texture analysis.

For Slide 12a - Implementation & Training:
We designed a multi-scale, multi-orientation Gabor filter bank to capture texture patterns. However, training showed higher fluctuations and lower overall performance, with validation Dice only reaching about 0.70.

For Slide 12b - Performance Decline:
The results were concerning - tumor core Dice plummeted to 0.209 under Simple Policy, a 59% decrease from baseline. We saw severe over-segmentation with 112.4 false positives per slice for enhancing tumor.

The pixel-level metrics revealed the extent of Gabor's problems. True Positives dropped dramatically: Tumor Core to just 4,806, Edema to 26,245, and Enhancing Tumor to 10,033. The complex texture features were confusing the model's ability to make confident predictions.

False Positives skyrocketed across all classes. Enhancing Tumor had 112 false positives per slice - nearly triple the raw intensity baseline. Tumor Core had 54 per slice, and Edema had 92 per slice. The model was seeing tumor patterns everywhere due to the rich texture representations.

False Negatives were equally problematic. Tumor Core had 70 false negatives per slice, meaning we were missing large portions of actual tumor tissue. Edema had a staggering 238 false negatives per slice - more than double the baseline.

Precision collapsed across all classes: Tumor Core fell to 0.398, Enhancing Tumor to 0.398, while only Edema maintained reasonable precision at 0.678. Recall was similarly poor except for Enhancing Tumor at 0.582. The complex Gabor features created too much noise for effective decision-making.

For Slide 12c - Qualitative Analysis:
The predictions show clear problems - complete misses of small lesions, severe under-segmentation, and fragmentary predictions. Only the true negatives remained reliable.

For Slide 12d - Critical Learning:
This experiment revealed a crucial insight: more complex features aren't always better. The rich, multi-dimensional Gabor features may have introduced too much complexity for the model to learn effectively."

## **Slides 13a-d: Laplacian Results** (2.5 minutes)
**Script:**
"Learning from previous attempts, we turned to Laplacian-of-Gaussian filtering.

For Slide 13a - Multi-Scale Implementation:
We implemented LoG filtering at multiple scales to capture both edge and blob-like structures. Training showed more stability than Gabor, with validation Dice reaching 0.71.

For Slide 13b - Performance Recovery:
Results showed significant improvement - tumor core precision reached 0.777, the highest among all methods. However, edema remained challenging with 203.7 false positives per slice.

The pixel-level analysis showed Laplacian's balanced approach. True Positives improved over Gabor: Tumor Core at 5,667, Edema at 38,600, and Enhancing Tumor at 12,780. The second-derivative features provided better structural information than pure texture analysis.

False Positives showed mixed results. Tumor Core achieved excellent control at just 12 per slice - the best of any method. However, Edema spiked to 204 false positives per slice, indicating the blob detection was triggering on normal tissue variations. Enhancing Tumor had 62 per slice, moderate but manageable.

False Negatives remained concerning for Tumor Core at 64 per slice, explaining the low recall of 0.398. Edema had 147 false negatives per slice, and Enhancing Tumor had 33 per slice. The conservative nature of LoG filtering was missing subtle tumor regions.

The precision-recall balance showed Laplacian's strength in avoiding false alarms (highest precision for Tumor Core at 0.777) but weakness in comprehensive detection. This suggested that while LoG features were valuable, they needed to be combined with other approaches for optimal performance.

For Slide 13c - Qualitative Validation:
The predictions show better structural capture and more stable performance than Gabor, though some boundary smoothing and detail loss remained evident.

For Slide 13d - Strategic Integration:
This experiment suggested we needed to combine the strengths of multiple approaches rather than relying on any single feature type."

## **Slides 14a-d: Combined Results** (2.5 minutes)
**Script:**
"Finally, let me present our combined feature detection approach.

For Slide 14a - Integration Strategy:
We integrated Sobel, Laplacian, and Gabor features while expanding our dataset to 11,000 slices. This provided both richer features and more robust training examples.

For Slide 14b - Optimal Performance:
The results validated our approach - enhancing tumor achieved a Dice of 0.843, with better precision-recall balance (0.757/0.716) than any individual method.

The pixel-level analysis demonstrated the power of feature integration. True Positives increased substantially: Tumor Core to 260,079, Edema to 612,185, and Enhancing Tumor to 284,888. The expanded dataset and combined features enabled much more confident tumor detection.

False Positives showed the benefits of feature moderation. Tumor Core had 80 per slice - higher than Laplacian alone but with much better recall. Edema had 149 per slice, improved from Laplacian's 204. Enhancing Tumor achieved excellent control at 55 per slice while maintaining strong detection.

False Negatives demonstrated the improved sensitivity. Tumor Core had 74 per slice, achieving the best precision-recall balance of any method. Edema had 177 per slice, and Enhancing Tumor had 68 per slice. The combined features helped the model detect subtle tumor patterns while avoiding false alarms.

The final precision-recall metrics showed optimal balance: Tumor Core (0.661/0.679), Edema (0.712/0.676), and Enhancing Tumor (0.757/0.716). Each feature type moderated the others' weaknesses - Sobel's edges prevented Gabor's over-sensitivity, Gabor's textures enriched Sobel's simplicity, and Laplacian provided balanced intermediate detection.

For Slide 14c - Qualitative Success:
Examples 2-5 demonstrate accurate segmentation of complex tumor structures with improved boundary definition. However, Example 6 reminds us that small lesion detection remains challenging.

For Slide 14d - Research Journey:
This final experiment proved that thoughtful feature engineering, combined with adequate data scale, can significantly enhance deep learning performance. The 33% improvement in Simple Policy tumor core Dice validates our hybrid approach."

**Key Points:**
- Emphasize the progression of understanding
- Highlight how each method informed the next
- Use specific metrics to demonstrate improvements
- Connect to broader implications for medical imaging

---

## **Slide 15: Computational Efficiency** (1.5 minutes)
**Script:**
"Critically for clinical deployment, we maintained computational efficiency. Our SegNet plus features approach uses only 4-6 GB of GPU memory compared to 8-12 GB for standard U-Net - a 50% reduction. Feature extraction adds only 2-3 seconds per case, which is acceptable for clinical workflow."

**Key Points:**
- Emphasize practical applicability
- Quantify the efficiency gains
- Address real-world constraints

---

## **Slide 16: Key Contributions** (2 minutes)
**Script:**
"My main contributions are: First, a novel preprocessing pipeline that's the first systematic evaluation of classical features with SegNet. Second, comprehensive feature analysis validated on the standard BraTS dataset. Third, a clinically feasible solution that maintains memory efficiency. Fourth, demonstrated performance improvements with better boundary precision."

**Key Points:**
- Clearly state your contributions
- Emphasize novelty and validation
- Connect to practical impact

---

## **Slide 17: Limitations & Future Work** (2 minutes)
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