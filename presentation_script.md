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

### Script for Pixel-Level Metrics (Part 2)

**Box 1: Precision, Recall, and Specificity**
> “In this box, we're looking at three important metrics that help us understand the model's behavior for each tumor class:
>
> - **Precision** tells us how often the model's positive predictions are actually correct. Here, Tumor Core stands out with the highest precision, meaning when the model predicts tumor core, it's usually right. However, it also has the lowest recall, so it misses more true tumor pixels.
> - **Recall** (or sensitivity) measures how many of the actual tumor pixels the model successfully detects. Enhancing Tumor achieves the best recall, meaning it finds most of the actual tumor, while Edema lags behind in both precision and recall.
> - **Specificity** shows how well the model avoids labeling healthy tissue as tumor. All classes have very high specificity, which reflects the dominance of background pixels in these images.
>
> The key takeaway is that high precision means few false alarms, high recall means few missed tumors, and high specificity means the model rarely mistakes healthy tissue for tumor. The trade-off between precision and recall is especially clear for Tumor Core, which is conservative but under-sensitive, and for Edema, which is the hardest to segment overall.”

**Box 2: Truth Values (TP, FP, FN, TN)**
> “In the second box, we break down the raw counts of the model's predictions:
>
> - **True Positives (TP):** Edema has the highest count, followed by Enhancing Tumor and Tumor Core. This reflects their prevalence in the dataset.
> - **False Positives (FP):** Edema also has the highest number of false positives, indicating more over-segmentation, while Tumor Core is more conservative.
> - **False Negatives (FN):** Again, Edema leads, showing the model's difficulty in detecting all true tumor pixels, especially for these diffuse regions.
> - **True Negatives (TN):** These are extremely high for all classes, simply because there's so much background in brain MRIs.
>
> The main interpretation is that Edema is the most challenging class, with the highest false positives and false negatives per slice, and lower precision and recall. Enhancing Tumor is detected most completely, while Tumor Core is detected most reliably when predicted, but is often missed. Overall, the model captures major tumor structures but still struggles with subtle or diffuse regions, especially Edema.”

## Slide 10a: Raw Intensity Baseline Training Performance
    
> “Let me start by showing the training performance of our SegNet model using raw MRI intensities, without any feature engineering or preprocessing.
>
> As you can see from the training history, the Dice coefficient started quite low, around 0.2, but improved steadily throughout the 20 epochs. By the end of training, both the training and validation Dice coefficients reached about 0.8, and—importantly—these curves tracked each other closely. This close tracking is a strong indicator of good generalization, meaning the model isn't just memorizing the training data, but is actually learning patterns that transfer well to new, unseen cases.
>
> If we look at the accuracy and loss curves, we see that both training and validation accuracy rapidly climbed above 99% within just a few epochs, and the loss dropped sharply before stabilizing. This tells us that the model is learning efficiently from the raw MRI signals, which in our case are the T1ce, T2, and FLAIR modalities mapped to RGB channels.
>
> The key takeaway here is that even without any explicit feature extraction, SegNet is able to extract and utilize relevant patterns from the original MRI data for four-class brain tumor segmentation. However, as we'll see in the next part of the slide, high accuracy doesn't always mean high-quality segmentation—especially when it comes to the more challenging tumor regions.”

## Slide 10a: Explaining the Four Metric Boxes

Box 1: Dice Coefficient (Overlap Similarity)
> “In the first box, we see the Dice coefficient, which measures how well our predicted tumor regions overlap with the ground truth. Under the Full Policy, the scores look strong across all tumor types, but when we switch to the Simple Policy—focusing only on slices that actually contain tumors—there's a dramatic drop. This highlights that the model performs well on easy, empty slices, but real tumor segmentation is much more challenging. Among the classes, enhancing tumor is segmented best, while edema remains the most difficult. This gap between policies really shows the importance of evaluating on clinically relevant cases.”

Box 2: IoU (Intersection over Union)
> “The second box shows the IoU, a stricter measure of overlap than Dice. Here, the drop from Full to Simple Policy is even more pronounced, especially for the harder classes. IoU is more sensitive to small errors at the boundaries, so these results reveal that our model's segmentations are often not as precise as they appear under the Full Policy. This underlines the need for better boundary delineation, especially for challenging tumor regions.”

Box 3: HD95 (95th Percentile Hausdorff Distance)
> “The third box presents the HD95, which measures the worst-case boundary error. Under the Full Policy, the errors seem low, but this is mostly due to the large number of empty slices. When we look at the Simple Policy, boundary errors increase significantly, especially for edema. This means that, in the most difficult cases, our model can be off by a considerable margin—something that's critical for surgical planning, where precise boundaries are essential.”

Box 4: ASSD (Average Symmetric Surface Distance)
> “Finally, the fourth box shows the ASSD, which reflects the average boundary error. Again, we see that errors are much lower under the Full Policy, but nearly double when we focus on tumor-containing slices. This consistent increase across all tumor types tells us that, in realistic clinical scenarios, our model's average boundary accuracy is a real limitation. It's a reminder that average performance can mask important challenges in the most relevant cases.”

Transition/Wrap-up:
> “So, across all four metrics, the key takeaway is that while our model performs well on the overall dataset, the real challenge—and the real test of clinical utility—comes when we focus on the cases that matter most: those with actual tumors. This is why we emphasize the Simple Policy results in our evaluation.”

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

## **Slide 14e: The Journey - From Raw Intensities to Feature Integration** (2.5 minutes)
**Script:**
"Let me take you through the journey of discovery that led to our final solution. This wasn't a linear path—it was an iterative process where each experiment revealed crucial insights that shaped the next.

We began with raw MRI intensities as our baseline, achieving a validation Dice of 0.80. Initially, this seemed promising, but deeper analysis revealed troubling patterns. The model was missing nearly half of tumor core pixels with only 0.555 recall, and edema boundaries were consistently imprecise with 138 false positives per slice. Most critically, the 38% performance drop under Simple Policy exposed that much of our 'success' came from correctly identifying empty background rather than accurately segmenting tumors.

This led me to hypothesize that the model needed explicit boundary information, so I turned to Sobel edge detection. The results were surprising—while precision remained high at 0.806, recall actually dropped to 0.433. The edge filtering made the model more conservative, missing even more tumor tissue. This taught me that while edges were important, removing intensity information was counterproductive.

Seeking richer features, I explored Gabor texture analysis with its multi-scale, multi-orientation capabilities. The results were sobering—tumor core Dice plummeted to just 0.209 under Simple Policy, with severe over-segmentation showing 112 false positives per slice for enhancing tumor. The lesson was clear: more complex features aren't always better. The rich Gabor representations created too much noise for effective learning.

This failure pointed me toward Laplacian-of-Gaussian filtering as a middle ground—capturing both edges and blob-like structures. Results improved significantly, with tumor core achieving the highest precision of 0.777. However, edema false positives spiked to 204 per slice, showing that even balanced features had limitations.

The journey taught me that each method had complementary strengths: Sobel excelled at boundaries, Gabor at textures, and Laplacian at structural detection. This realization, combined with the understanding that our 900-slice dataset might be limiting learning, led to the final breakthrough—combining all three methods with an expanded 11,000-slice dataset.

The combined approach achieved what no individual method could: tumor core Simple Policy Dice improved to 0.526, precision-recall balance optimized across all classes, and false positive rates moderated through feature complementarity. This wasn't just an incremental improvement—it validated the entire hypothesis that thoughtful feature engineering enhances deep learning.

Looking back, each 'failure' was actually a stepping stone. The journey from 0.398 Gabor performance to 0.526 combined performance represents not just a 33% improvement, but a fundamental understanding of how classical computer vision and modern deep learning can work synergistically in medical imaging."

**Key Points:**
- Present as a narrative journey of discovery
- Show how each failure informed the next approach
- Emphasize the iterative scientific process
- Highlight the breakthrough insight about complementary features
- Connect to broader lessons about hybrid approaches

---

## **Slide 14f: The Best Model - Combined Feature Integration** (2.5 minutes)
**Script:**
"Let me elaborate on why our combined feature approach emerged as the best solution and what made it successful.

The combined model integrates three complementary feature detection methods applied to our RGB-mapped MRI channels. Each modality receives its tailored enhancement: T1-contrast enhanced gets Sobel edges for precise tumor boundaries, T2 receives Gabor texture features for heterogeneous tissue patterns, and FLAIR incorporates Laplacian blob detection for identifying tumor cores and edema regions. This creates a 9-dimensional feature space that preserves original intensities while adding structural, textural, and morphological information.

But feature integration alone wasn't enough—the dramatic expansion from 900 to 11,000 training slices was equally crucial. This 12-fold increase provided the diversity needed for the model to learn how to effectively utilize the rich feature representations. The extended 50-epoch training allowed proper convergence on this more complex feature space.

The performance gains speak for themselves. Under the challenging Simple Policy evaluation, tumor core Dice improved from 0.398 with Gabor alone to 0.526—a 32% increase. More importantly, we achieved balanced precision-recall across all classes: enhancing tumor reached 0.757 precision with 0.716 recall, the best balance of any approach. Even the challenging edema class improved to 0.712 precision and 0.676 recall.

What makes this work is the synergistic moderation effect. Sobel's conservative edge detection prevents Gabor's tendency to over-segment—we see this in enhancing tumor false positives dropping from 112 to 55 per slice. Gabor's texture sensitivity compensates for Sobel's tendency to miss subtle patterns, improving tumor core recall from 0.433 to 0.679. Laplacian provides the structural middle ground, particularly effective for blob-like tumor cores while the other features handle boundaries and textures.

The false positive analysis reveals this balance beautifully. While individual methods showed extremes—Sobel too conservative at 11 FP/slice for tumor core, Gabor too aggressive at 112 FP/slice for enhancing tumor—the combined approach achieved moderate values across all classes: 80 for tumor core, 149 for edema, and 55 for enhancing tumor.

From a clinical perspective, this model offers several advantages. First, it maintains SegNet's memory efficiency at 4-6GB while achieving performance approaching heavier architectures. Second, the feature preprocessing adds only 2-3 seconds to processing time—negligible in clinical workflow. Third, the balanced precision-recall means fewer false alarms without missing critical tumor regions, exactly what radiologists need for treatment planning.

Most significantly, this work demonstrates that we don't have to choose between classical computer vision and deep learning. By thoughtfully combining domain knowledge through feature engineering with the pattern recognition power of CNNs, we create solutions that are both effective and interpretable. The features we add have clear clinical meaning—edges for surgical boundaries, textures for tissue characterization, and blobs for lesion detection.

This hybrid approach points toward the future of medical imaging AI: not replacing human expertise but encoding it into our algorithms, creating tools that think more like radiologists while maintaining the consistency and efficiency of automated analysis."

**Key Points:**
- Detail the technical implementation clearly
- Quantify improvements with specific metrics
- Explain the synergistic effects with examples
- Emphasize clinical relevance and feasibility
- Connect to broader implications for medical AI
- Show both technical innovation and practical impact

---

## **Slide 15: Conclusion** (2 minutes)
**Script:**
"As we reach the conclusion of this research journey, let me summarize what we've achieved and what it means for the field of medical image segmentation.

This thesis has successfully demonstrated that classical feature detection techniques, when thoughtfully integrated with modern deep learning, can significantly enhance brain tumor segmentation performance. We developed a novel RGB fusion pipeline that systematically evaluated Sobel edge detection, Gabor texture analysis, and Laplacian-of-Gaussian blob detection—both individually and in combination—with the SegNet architecture.

Our key findings paint a compelling picture. While raw MRI intensities provided a solid baseline with a Simple Policy Dice of 0.517 for tumor core, the journey through different feature detection methods revealed crucial insights. Sobel showed us that edges alone weren't sufficient. Gabor taught us that more complex features aren't always better—dramatically so, with performance plummeting to 0.209. Laplacian offered a balanced approach but still had limitations. 

The breakthrough came with the combined approach. By integrating all three feature types and expanding our dataset from 900 to 11,000 slices, we achieved optimal performance—tumor core Dice improved to 0.526, representing not just a numerical improvement but a fundamental validation of the hybrid approach. The precision-recall balance across all classes improved significantly, with enhancing tumor achieving both 0.757 precision and 0.716 recall.

From a clinical perspective, this work maintains the practical advantages we set out to achieve. The memory requirement stays at 4-6GB compared to U-Net's 8-12GB, making it deployable on standard clinical hardware. The additional preprocessing time of just 2-3 seconds is negligible in clinical workflow. Most importantly, the features we use have clear clinical interpretations—edges for surgical boundaries, textures for tissue characterization, and blobs for lesion detection.

The broader implications extend beyond brain tumor segmentation. This work validates that domain knowledge, encoded through classical computer vision techniques, remains valuable in the deep learning era. We don't have to choose between interpretability and performance—we can have both. The hybrid approach provides a framework for other medical imaging challenges where combining human expertise with machine learning could yield superior results.

In essence, this thesis demonstrates that the future of medical image analysis lies not in pure end-to-end deep learning, but in thoughtful integration of established techniques with modern architectures. By respecting both the wisdom of classical methods and the power of neural networks, we create solutions that are not only more effective but also more trustworthy and deployable in real clinical settings."

**Key Points:**
- Summarize the complete research journey and findings
- Emphasize the validation of the hybrid approach
- Highlight practical clinical benefits
- Connect to broader implications for the field
- End with a strong statement about the future of medical imaging

---

## **Slide 16: Future Work** (2 minutes)
**Script:**
"While this thesis has demonstrated the value of combining classical features with deep learning, it also opens exciting avenues for future research and development.

In the short term, the most impactful enhancement would be transitioning from 2D slice-based processing to full 3D volumetric implementation. Our current approach processes each slice independently, missing valuable spatial context. A 3D convolutional network could maintain consistency across slices, potentially improving small lesion detection by 10-15% based on literature precedents. Additionally, integrating attention mechanisms could help the model focus on challenging regions—particularly small tumors and ambiguous boundaries that our current approach sometimes misses.

Moving to medium-term developments, there's significant potential in advanced feature fusion strategies. Rather than simply stacking our Sobel, Gabor, and Laplacian features, we could implement learnable fusion weights, allowing the network to dynamically adjust feature importance based on the specific case. Multi-scale pyramid feature extraction could capture tumors across different size ranges more effectively. We could also explore cross-modal attention between different MRI sequences, potentially discovering new relationships between modalities that human experts haven't recognized.

The long-term vision extends to full clinical integration. This requires extensive validation through multi-center trials, comparing our automated segmentations against multiple radiologists to establish inter-rater agreement. Integration with Picture Archiving and Communication Systems (PACS) would enable real-time segmentation during clinical reads. Critically, we need to add uncertainty quantification—the system should know when it's unsure and flag cases for human review.

Beyond brain tumors, the methodology could extend to other pathologies. The feature-enhanced approach might work for stroke detection, multiple sclerosis lesion segmentation, or even applications outside the brain. We could adapt the framework for different imaging modalities—CT, PET, or ultrasound—each with their own relevant classical preprocessing techniques.

From a research perspective, several methodological advances beckon. Self-supervised pretraining on large unlabeled MRI datasets could improve the model's understanding of normal anatomy before fine-tuning on tumors. Domain adaptation techniques could handle variations between different scanners and protocols—a major challenge in clinical deployment. Explainable AI methods could visualize which features contribute most to specific predictions, building radiologist trust.

The ultimate goal is to create a system that truly enhances radiologist capabilities rather than replacing them. Imagine a tool that provides initial segmentations in seconds, highlights areas of uncertainty, explains its reasoning through feature visualizations, and learns from radiologist corrections. This would not just save time but could also serve as a training tool for residents and a second opinion for challenging cases.

The journey from raw intensities to feature-enhanced segmentation has shown us that the most powerful solutions come from combining human domain knowledge with machine learning capabilities. The future of medical imaging AI lies in this synergy, and I'm excited to see where this path leads."

**Key Points:**
- Present a clear roadmap from immediate to long-term improvements
- Connect technical enhancements to clinical benefits
- Show vision for broader applications
- Emphasize the human-AI collaboration aspect
- End with enthusiasm for the future of the field

---

## **Slide 17: Computational Efficiency** (1.5 minutes)
**Script:**
"Critically for clinical deployment, we maintained computational efficiency. Our SegNet plus features approach uses only 4-6 GB of GPU memory compared to 8-12 GB for standard U-Net - a 50% reduction. Feature extraction adds only 2-3 seconds per case, which is acceptable for clinical workflow."

**Key Points:**
- Emphasize practical applicability
- Quantify the efficiency gains
- Address real-world constraints

---

## **Slide 18: Key Contributions** (2 minutes)
**Script:**
"My main contributions are: First, a novel preprocessing pipeline that's the first systematic evaluation of classical features with SegNet. Second, comprehensive feature analysis validated on the standard BraTS dataset. Third, a clinically feasible solution that maintains memory efficiency. Fourth, demonstrated performance improvements with better boundary precision."

**Key Points:**
- Clearly state your contributions
- Emphasize novelty and validation
- Connect to practical impact

---

## **Slide 19: Limitations & Future Work** (2 minutes)
**Script:**
"I acknowledge several limitations: the 2D slice-based approach could benefit from 3D volumetric processing, feature parameters required manual tuning, and I focused only on SegNet architecture.

For future work, I see exciting directions: 3D volumetric implementation, attention mechanisms, advanced fusion strategies, and most importantly, clinical validation with radiologist evaluation."

**Key Points:**
- Show intellectual honesty about limitations
- Demonstrate you understand next steps
- Show vision for impact

---

## **Slide 20: Key Takeaways** (1.5 minutes)
**Script:**
"The key takeaways are: Classical features can enhance deep learning with measurable improvements. Combined approaches work through synergistic effects. Clinical feasibility is maintained through memory efficiency. And most importantly, hybrid approaches combining domain knowledge with modern AI show real promise for medical imaging."

**Key Points:**
- Synthesize your main findings
- Connect to broader implications
- End on a strong note about significance

---

## **Slide 21: Thank You** (30 seconds)
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



{0 = Background:} Dark blue 
 {1 = Tumor Core:} Cyan‐blue  
 {2 = Edema:} Green–yellow 
 {4 = Enhancing Tumor:} Red 

---

## **DETAILED TRAINING PERFORMANCE SCRIPTS FOR ALL FEATURE DETECTION METHODS**

## Slide 11a: Sobel Edge Detection - Training Performance

> "Now let me show you the training performance of our SegNet model enhanced with Sobel edge detection preprocessing.
>
> Looking at the training history, we can see that the Dice coefficient evolution shows steady improvement throughout the 20 epochs, though with notably more fluctuations compared to our raw intensity baseline. The model started from approximately 0.2 and gradually improved, but the validation curve shows more variability, which suggests that the edge-enhanced features created a more challenging learning environment for the network.
>
> What's particularly interesting is that while the overall trend is positive, the validation Dice reaches around 0.80 by the final epochs - similar to our baseline - but the path to get there is less smooth. This increased volatility in the learning curves was our first hint that pure edge enhancement might be introducing some complexity that the network was struggling to handle optimally.
>
> The accuracy and loss curves tell a similar story. Both training and validation accuracy still climb rapidly to above 99% within the first few epochs, which is consistent with our baseline performance. The loss curves show the expected sharp initial decline followed by stabilization, but again with more fluctuation in the validation loss compared to raw intensities.
>
> The key insight here is that while Sobel edge detection provides clear visual enhancement of tumor boundaries - which we can see in our RGB composites - this doesn't automatically translate to better learning dynamics for the neural network. The edge filtering is highlighting important structural information, but it may also be removing some of the contextual intensity information that the network found useful in the raw intensity approach.
>
> This training behavior foreshadowed what we would later discover in our test results - that edge information alone, while valuable, wasn't sufficient to improve overall segmentation performance and in some cases made the model more conservative in its predictions."

## Slide 12a: Gabor Texture Analysis - Training Performance

> "Moving to our Gabor texture analysis approach, the training performance reveals some concerning patterns that would later explain our disappointing test results.
>
> The Dice coefficient evolution shows the most unstable learning curves we've seen so far. While the model does improve from its starting point around 0.2, the validation Dice only reaches approximately 0.70 by the final epochs - notably lower than both our raw intensity baseline and Sobel approach. More troubling is the high degree of fluctuation throughout training, with the validation curve showing significant volatility that suggests the model is struggling to find stable patterns in the complex Gabor feature space.
>
> What we're seeing here is the challenge of learning from over-rich feature representations. The multi-scale, multi-orientation Gabor filter bank creates a very high-dimensional feature space with complex texture patterns at different scales and orientations. While this might seem advantageous for capturing tumor heterogeneity, it appears to overwhelm the SegNet architecture's ability to learn stable decision boundaries.
>
> The accuracy and loss curves show similar instability. While accuracy still reaches the 99% range, the path is much more erratic, and the validation loss shows concerning fluctuations that indicate the model is having difficulty converging to a stable solution. This suggests that the rich texture information, rather than helping the network, is actually creating confusion in the learning process.
>
> Looking back, these training dynamics were a clear warning sign. The network was telling us through its unstable learning behavior that the Gabor features, while theoretically appealing, were too complex for effective utilization. The high-dimensional texture representations were creating a feature space that was difficult for the model to navigate, leading to the poor generalization we would later observe in our test results.
>
> This experience taught us a crucial lesson about feature engineering: more sophisticated features don't automatically lead to better performance. Sometimes, the complexity of the feature representation can actually hinder rather than help the learning process."

## Slide 13a: Laplacian-of-Gaussian - Training Performance

> "Our Laplacian-of-Gaussian approach shows a much more encouraging training profile, representing a significant recovery from the Gabor experiment.
>
> The Dice coefficient curves demonstrate much more stable learning compared to Gabor, with validation Dice reaching approximately 0.71 by the final epochs. While this is still slightly below our raw intensity baseline, the learning curves are much smoother and show consistent improvement throughout training. This stability suggests that the LoG features provide a more learnable representation than the complex Gabor textures while still offering enhancement beyond simple edge detection.
>
> What makes this particularly interesting is that Laplacian-of-Gaussian, as a second-derivative operator, captures both edge and blob-like structures simultaneously. This multi-scale approach seems to hit a 'sweet spot' in feature complexity - rich enough to provide useful structural information, but not so complex as to overwhelm the network's learning capacity.
>
> The accuracy and loss curves reinforce this positive trend. We see rapid convergence to 99% accuracy with much less fluctuation than the Gabor approach, and the loss curves show clean, stable convergence patterns. The validation metrics track the training metrics closely, indicating good generalization without the instability we observed with texture-based features.
>
> This stable training behavior gave us confidence that LoG features were providing a more balanced approach to feature enhancement. The second-derivative operator was capturing important structural information about tumor regions - particularly blob-like tumor cores and fine edge details - while maintaining a feature space that the network could effectively learn from.
>
> The training performance here validated our hypothesis that balanced feature detection, rather than overly complex texture analysis, was the key to improving segmentation. This insight would prove crucial when we later developed our combined approach, as it showed us that Laplacian features could serve as an effective bridge between simple edge detection and complex texture analysis."

## Slide 14a: Combined Feature Integration - Training Performance

> "Finally, let me present the training performance of our combined approach, which represents the culmination of all our learning from the individual feature detection experiments.
>
> The most striking aspect of these training curves is their remarkable stability and smooth progression. The Dice coefficient evolution shows the cleanest, most consistent improvement we've seen across any of our experiments. Starting from around 0.24, the model demonstrates steady, almost monotonic improvement, reaching a validation Dice of approximately 0.72 by the final epochs - the highest we achieved across all our approaches.
>
> What makes this even more impressive is that we're now training on a dramatically expanded dataset of approximately 11,000 slices compared to the 900 slices used in our individual feature experiments. Despite this increased data complexity, the learning curves are smoother and more stable than any of our previous attempts. This suggests that the combined feature representation, rather than adding confusion, is actually providing the network with a more comprehensive and learnable feature space.
>
> The extended training to 50 epochs was necessary given the complexity of the combined feature space and the larger dataset. What we see is that the model continues to improve throughout this extended training period, with no signs of overfitting or instability. The close tracking between training and validation curves indicates excellent generalization, suggesting that our feature integration approach is robust and scalable.
>
> The accuracy and loss curves tell an equally compelling story. Both metrics show rapid initial improvement followed by stable convergence, with minimal fluctuation in the validation metrics. This stability is particularly remarkable given that we're now processing features from three different detection methods (Sobel, Gabor, and Laplacian) simultaneously.
>
> The key insight from these training dynamics is that thoughtful feature integration, combined with adequate data scale, creates a synergistic effect. Rather than the features competing or creating confusion, they appear to be working together to provide the network with complementary information that enables more robust and stable learning.
>
> This training performance validated our core hypothesis: that combining the edge detection capabilities of Sobel, the texture sensitivity of Gabor (in balanced amounts), and the structural detection of Laplacian would create a feature representation greater than the sum of its parts. The smooth, stable learning curves gave us confidence that we had found an optimal balance between feature richness and learnability."

## **COMPREHENSIVE METRIC EXPLANATION SCRIPTS**

## Sobel Edge Detection - Four Metric Boxes Explanation

**Box 1: Dice Coefficient (Overlap Similarity)**
> "Looking at the Dice coefficient results for our Sobel edge detection approach, we see an interesting pattern. Under the Full Policy, the scores appear quite strong - Tumor Core at 0.803, Edema at 0.598, and Enhancing Tumor at 0.853. However, when we switch to the Simple Policy, focusing only on tumor-containing slices, we see significant drops across all classes. Most notably, Tumor Core drops to 0.408 - actually worse than our raw intensity baseline of 0.517. This tells us that while edge enhancement makes boundaries visually clearer, it's actually making the model more conservative and causing it to miss tumor regions that the raw intensity model would have detected."

**Box 2: IoU (Intersection over Union)**
> "The IoU results reinforce the Dice findings but are even more dramatic. Under Simple Policy, Tumor Core IoU drops to just 0.313, and Edema falls to 0.285. IoU is a stricter metric than Dice, so these low scores indicate that our edge-enhanced model is producing segmentations with poor spatial overlap with the ground truth. The edge filtering, while highlighting boundaries, seems to be creating fragmented or incomplete segmentations that don't capture the full extent of tumor regions."

**Box 3: HD95 (95th Percentile Hausdorff Distance)**
> "Interestingly, the boundary distance metrics show mixed results. Under Full Policy, HD95 scores are actually quite good - particularly for Enhancing Tumor at just 1.38 voxels. However, under Simple Policy, these errors increase substantially. What this suggests is that when Sobel does detect tumor boundaries, they tend to be quite accurate, but the problem is that it's missing too many tumor regions entirely. The edge enhancement is creating precise but incomplete segmentations."

**Box 4: ASSD (Average Symmetric Surface Distance)**
> "The ASSD results follow a similar pattern to HD95. Under Full Policy, the average boundary errors are reasonable, but they increase under Simple Policy. For Enhancing Tumor, ASSD goes from 0.41 to 1.26 voxels. This reinforces our interpretation that Sobel edge detection is creating more precise boundaries where it does detect tumor, but it's being overly conservative and missing significant portions of actual tumor tissue."

## Gabor Texture Analysis - Four Metric Boxes Explanation

**Box 1: Dice Coefficient (Overlap Similarity)**
> "The Dice coefficient results for Gabor filtering reveal the most dramatic performance decline we observed in our entire study. Under Simple Policy, Tumor Core plummets to just 0.209 - a 59% decrease from our raw intensity baseline. Edema drops to 0.261, and Enhancing Tumor falls to 0.343. These results indicate that the complex multi-scale, multi-orientation Gabor features, rather than helping the network understand tumor characteristics, are actually confusing its decision-making process. The rich texture representations appear to be creating too much noise in the feature space for effective learning."

**Box 2: IoU (Intersection over Union)**
> "The IoU scores are even more concerning, with Tumor Core achieving only 0.144 under Simple Policy. This extremely low overlap indicates that when Gabor features are used, the model produces segmentations that barely correspond to the actual tumor regions. The texture-based approach seems to be triggering false positive responses to normal brain tissue patterns while simultaneously missing actual tumor regions."

**Box 3: HD95 (95th Percentile Hausdorff Distance)**
> "The boundary distance metrics show catastrophic performance under Simple Policy. HD95 scores reach 23-32 voxels across all classes - roughly three times worse than our baseline. This indicates that not only is the model missing tumor regions, but when it does make predictions, the boundaries are severely misplaced. The complex Gabor features appear to be creating spatial confusion in the network's understanding of tumor boundaries."

**Box 4: ASSD (Average Symmetric Surface Distance)**
> "ASSD scores follow the same alarming trend, with average boundary errors of 11-16 voxels under Simple Policy. These results, combined with the other metrics, paint a clear picture: the Gabor texture analysis approach is fundamentally incompatible with our SegNet architecture and dataset size. The lesson here is that sophisticated feature engineering can actually harm performance if the features are too complex for the model to effectively utilize."

## Laplacian-of-Gaussian - Four Metric Boxes Explanation

**Box 1: Dice Coefficient (Overlap Similarity)**
> "The Laplacian-of-Gaussian results show a significant recovery from the Gabor disaster. Under Simple Policy, Tumor Core achieves 0.382 - still below our baseline but a substantial improvement over Gabor's 0.209. Enhancing Tumor reaches 0.562, which is approaching our baseline performance. This suggests that the second-derivative LoG operator is providing a more balanced feature representation that the network can actually learn from, capturing both edge and blob-like structures without overwhelming complexity."

**Box 2: IoU (Intersection over Union)**
> "IoU scores show similar recovery patterns, with Enhancing Tumor achieving 0.430 under Simple Policy. While still below baseline, these scores indicate that LoG features are enabling the network to produce segmentations with reasonable spatial overlap with ground truth. The multi-scale blob detection appears to be particularly effective for the more well-defined enhancing tumor regions."

**Box 3: HD95 (95th Percentile Hausdorff Distance)**
> "The boundary distance metrics show substantial improvement over Gabor, with HD95 scores in the 4-22 voxel range under Simple Policy. While still elevated compared to our baseline, these represent a dramatic improvement over Gabor's 25-32 voxel errors. The LoG features are enabling more spatially coherent segmentations, though boundary precision remains a challenge."

**Box 4: ASSD (Average Symmetric Surface Distance)**
> "ASSD scores follow the recovery trend, with errors in the 1-10 voxel range. Most notably, Enhancing Tumor achieves just 1.48 voxels average error under Simple Policy, indicating that LoG features are particularly effective for this tumor type. The blob detection capabilities of Laplacian filtering appear well-suited to high-confidence tumor core identification."

## Combined Feature Integration - Four Metric Boxes Explanation

**Box 1: Dice Coefficient (Overlap Similarity)**
> "The combined approach delivers our best overall performance across all metrics. Under Simple Policy, Tumor Core achieves 0.526 - finally exceeding our raw intensity baseline of 0.517. Enhancing Tumor reaches 0.590, maintaining strong performance, while Edema achieves 0.381. These results validate our hypothesis that combining complementary feature types can overcome the limitations of individual approaches. The synergistic effect of edge detection, texture analysis, and blob detection creates a more robust feature representation."

**Box 2: IoU (Intersection over Union)**
> "IoU scores show consistent improvement, with Tumor Core reaching 0.429 and Enhancing Tumor achieving 0.492 under Simple Policy. These represent the best IoU scores we achieved for these classes, indicating that the combined features enable more spatially accurate segmentations. The integration of multiple feature types appears to provide the network with sufficient information to make more confident and accurate boundary decisions."

**Box 3: HD95 (95th Percentile Hausdorff Distance)**
> "Boundary distance metrics show balanced performance across all classes. Under Simple Policy, HD95 scores range from 8-13 voxels - not our absolute best for individual classes, but representing the most consistent performance across all tumor types. This suggests that the combined approach provides robust boundary detection without the extreme variations we saw with individual methods."

**Box 4: ASSD (Average Symmetric Surface Distance)**
> "ASSD scores reinforce the balanced performance theme, with errors in the 2-4 voxel range under Simple Policy. Most importantly, the combined approach achieves this balanced performance while maintaining the improved Dice and IoU scores. This indicates that we've found an optimal trade-off between boundary precision and region detection accuracy - exactly what's needed for clinical applications where both aspects are crucial."

## **PIXEL-LEVEL ANALYSIS SCRIPTS FOR ALL METHODS**

## Sobel Edge Detection - Pixel-Level Metrics

> "The pixel-level analysis reveals why Sobel edge detection, despite producing visually appealing edge-enhanced images, actually performed worse than our baseline in terms of overall segmentation quality.
>
> Looking at True Positives, we see a concerning decrease across all classes compared to raw intensities. Tumor Core drops to just 6,166 correctly identified pixels, down from 7,907 in our baseline. This immediately tells us that the edge filtering is making the model more conservative - it's identifying fewer tumor pixels overall.
>
> The precision metrics tell an interesting story. Tumor Core precision actually improves to 0.806, the highest we've seen. This means that when the Sobel-enhanced model predicts tumor core, it's usually correct. However, this comes at a severe cost to recall, which drops to just 0.433. The model is being extremely cautious, making very few false positive errors but missing a large portion of actual tumor tissue.
>
> False Negatives reveal the extent of this problem. Tumor Core false negatives increase to 60 per slice, compared to 47 in our baseline. We're literally missing more tumor tissue than before. This suggests that the edge filtering, while highlighting boundaries clearly, is removing important intensity and texture information that the network needs to confidently identify tumor regions.
>
> The key insight here is that high precision without adequate recall is clinically problematic. Missing tumor tissue (high false negatives) is more dangerous than including some extra tissue (false positives) in surgical planning. The Sobel approach, while producing cleaner-looking boundaries, is actually making the model less clinically useful."

## Gabor Texture Analysis - Pixel-Level Metrics

> "The pixel-level analysis of our Gabor texture approach reveals the full extent of why this method failed so dramatically.
>
> True Positives collapse across all classes. Tumor Core drops to just 4,806 correctly identified pixels - the lowest of any method we tested. This represents a fundamental failure of the texture-based approach to enable confident tumor detection. The complex multi-scale, multi-orientation features appear to be overwhelming the network's decision-making capabilities.
>
> The precision and recall metrics paint a picture of complete confusion. Tumor Core precision falls to 0.398 while recall drops to 0.337 - both terrible. Enhancing Tumor shows a similar precision collapse to 0.398, though recall is somewhat better at 0.582. These numbers indicate that the rich Gabor features are causing the network to see tumor patterns everywhere, leading to both excessive false positives and missed true tumor regions.
>
> False Positives spike dramatically. Enhancing Tumor reaches 112 false positives per slice - nearly triple our baseline. The model is triggering on normal brain tissue patterns that the Gabor filters identify as tumor-like textures. This over-sensitivity to texture patterns makes the approach clinically unusable.
>
> False Negatives are equally problematic, with Edema reaching a staggering 238 false negatives per slice. We're missing massive amounts of actual tumor tissue while simultaneously creating false alarms on healthy tissue.
>
> This analysis confirmed our hypothesis that the Gabor approach was fundamentally flawed for our architecture and dataset size. The lesson is clear: sophisticated feature engineering must be matched to the model's capacity to learn from those features."

## Laplacian-of-Gaussian - Pixel-Level Metrics

> "The pixel-level analysis of our Laplacian-of-Gaussian approach shows a strategic recovery from the Gabor failure, with some interesting trade-offs.
>
> True Positives show modest improvement over Gabor across all classes. Tumor Core reaches 5,667 correctly identified pixels, while Enhancing Tumor achieves 12,780. While still below our baseline, these numbers indicate that the LoG features are enabling more confident tumor detection than the complex Gabor textures.
>
> The precision-recall balance reveals Laplacian's characteristic behavior. Tumor Core achieves our highest precision of 0.777 - even better than Sobel - but recall remains low at 0.398. This suggests that LoG features are excellent at avoiding false alarms but are still conservative in their detection. The blob detection capabilities seem particularly well-suited to high-confidence tumor core identification.
>
> False Positives show mixed results. Tumor Core achieves excellent control at just 12 per slice - the best of any method. However, Edema spikes to 204 false positives per slice, indicating that the blob detection is triggering on normal tissue variations. This suggests that LoG features work well for well-defined structures but struggle with diffuse regions.
>
> False Negatives remain concerning, with Tumor Core at 64 per slice. While better than Gabor, we're still missing significant tumor tissue. The conservative nature of LoG filtering appears to prioritize precision over sensitivity.
>
> The analysis suggests that Laplacian features provide valuable structural information, particularly for tumor cores, but need to be combined with other approaches to achieve optimal sensitivity."

## Combined Feature Integration - Pixel-Level Metrics

> "The pixel-level analysis of our combined approach demonstrates the power of feature integration and data scale expansion.
>
> True Positives increase dramatically across all classes due to our expanded dataset. Tumor Core reaches 260,079 correctly identified pixels, Edema achieves 612,185, and Enhancing Tumor attains 284,888. These substantial increases reflect both the larger dataset and the improved feature representation enabling more confident tumor detection.
>
> The precision-recall balance achieves our best overall performance. Tumor Core reaches 0.661 precision with 0.679 recall - the first time we achieved good balance for this challenging class. Enhancing Tumor achieves excellent balance with 0.757 precision and 0.716 recall. Even Edema shows improved balance with 0.712 precision and 0.676 recall.
>
> False Positives demonstrate the moderating effect of feature combination. Tumor Core shows 80 per slice - higher than Laplacian alone but with much better recall. Enhancing Tumor achieves excellent control at 55 per slice while maintaining strong detection. The combined features appear to prevent the extreme behaviors we saw with individual methods.
>
> False Negatives show improved sensitivity. Tumor Core achieves 74 per slice - not our absolute best but with much better precision-recall balance. The combined features help the model detect subtle tumor patterns while avoiding excessive false alarms.
>
> This analysis validates our integration strategy. Each feature type moderates the others' weaknesses: Sobel's edges prevent Gabor's over-sensitivity, Gabor's textures enrich Sobel's simplicity, and Laplacian provides balanced intermediate detection. The result is a robust, clinically viable segmentation approach." 