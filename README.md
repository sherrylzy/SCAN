# SCAN: Selective Contrastive Learning Against Noisy Data for Acoustic Anomaly Detection

# 1. Robustness with Varying Noise Ratios

## 1.1 Comparison of Performance Trends with Varying Noise Ratios

The figure presents a comparison of performance trends on source and target domains for the proposed SCAN, baselines, and advanced methods with increasing noise ratios (0% to 8%).

![Comparison of Performance Trends](/results/Comparison%20of%20Performance%20Trends%20with%20Varying%20Noise%20Ratios.png)

## 1.2 Detailed Per-Machine Results under Highest 8% Noise

The figure below presents detailed AUC performance for each machine type under the highest noise ratio (8%), comparing baseline methods, advanced CL-based approaches, and our proposed SCAN.

![Detailed Per-Machine Results under Highest 8% Noise](/results/Detailed%20Per-Machine%20Results%20under%20Highest%208%25%20Noise.png)

## 1.3 Visualisation

### 1.3.1 Over different approaches

t-SNE visualisations of extracted features from the bearing test data under 8% label noise are presented to compare the representation quality of AE-AAD, MobileNetV2, AADCL, CLF-AIAD, and our proposed SCAN, as shown in the figures below.  The proposed SCAN produces more distinct features, indicating its superior ability to capture latent representations under noisy data conditions.

![t-SNE Over different approach-1](/results/t-SNE%20Over%20different%20approach-1.png)

![t-SNE Over different approach-2](/results/t-SNE%20Over%20different%20approach-2.png)


### 1.3.2 Over different Epochs

t-SNE visualizations of extracted features from the bearing test data under 8% label noise are presented for the proposed SCAN at 100, 200, 300, and 400 training epochs. As the number of training epochs increases, the feature clusters become progressively more structured and separated. At 400 epochs, SCAN shows the most distinct feature distribution, corresponding to its best performance.

![t-SNE Over different Epochs](/results/t-SNE%20Over%20different%20Epochs.png)


# 2. Comparison to Top-performing Systems

In this Table, SCAN consistently outperforms both the baseline and top-performing systems across various noise levels, achieving higher AUC scores in DCASE2022. In DCASE2024, when the noise ratio exceeds 8%, the performance gap between SCAN and the top-1 system remains small, demonstrating its strong robustness even when trained on noisy data.

| Method | AE | top-1 | top-2 | top-3 | top-4 | top-5 | Scan-0% | Scan-1% | Scan-2% | Scan-3% | Scan-4% |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DCASE 2023 | 61.5 | 77.13 | 74.87 | 73.72 | 69.7 | 68.22 | 79.78 | 78.56 | 78.22 | 77.86 | 77.02 |
| DCASE2024 | 60.6 | 73.98 | 72.81 | 69.04 | 61.35 | 61.09 | 74.53 | 74.11 | 73.52 | 72.98 | 71.63 |

# 3. Ablation Study

To evaluate the effectiveness of the proposed method, ablation experiments were conducted on three SCAN variants:

(1)**SCAN w/o selecting**, which removes the confident pair selection module;

(2)**SCAN w/o balance factor**, which applies the final threshold value from Eq. (5) at the beginning of training; and

(3)**SCAN w/ mean**, which uses the mean vector instead of the geometric median for Mahalanobis distance calculation.

First, SCAN w/o selecting experiences a sharp decline as noise increases, underscoring the importance of noise mitigation strategies. Second, SCAN without a balance factor further deteriorates under high noise conditions and across different datasets, emphasising the need for progressively increasing threshold confidence to enhance acoustic representation learning and model stability. Lastly, SCAN with mean underperforms the geometric median, which offers better robustness in real-world scenarios.

## 3.1 Comparison of Performance Trends with Varying Noise Ratios with SCAN’s variants

Comparison of performance trends on source and target domains for the proposed SCAN and its variations for increasing noise ratios (0% to 8%).

![Comparison of Performance Trends with SCAN’S variants](/results/Comparison%20of%20Performance%20Trends%20with%20Varying%20Noise%20Ratios%20with%20SCAN%E2%80%99S%20variants.png)


## 3.2 Detailed Per-Machine Results on DCASE2022/DCASE2024 under Highest 8% Noise

![DCASE2022 Per-Machine 8% Noise](/results/dcase2022_per_machine_8percent_noise.png)

![DCASE2024 Detailed Per-Machine 8% Noise](/results/dcase2024_detailed_per_machine_8percent_noise.png)


# 4. Other Evaluation

## 4.1 Comparison with different metrics (F1, Recall, Precision)

To provide a more comprehensive evaluation, we assessed model performance using additional metrics beyond AUC. Below figure presents the F1, Recall, and Precision scores on the DCASE 2022 dataset with 8% noise, comparing SCAN, its variants, and baseline methods. The results demonstrate that our proposed SCAN achieves state-of-the-art performance across these different metrics, confirming its robustness under noisy conditions.

![other-metric.png](/results/other-metric.png)

## 4.2 Geometric Median for Robust Centre Estimation

The reliability of Mahalanobis distance-based anomaly detection hinges on a stable reference center. The arithmetic mean is highly sensitive to outliers—even a few anomalous samples can skew the center and compromise detection accuracy.

To overcome this vulnerability, we use the geometric median to establish a robust reference point. Unlike the mean, the geometric median minimizes the L1 distance, making it significantly more resilient to outliers. This ensures the reference center accurately represents the core distribution of normal data. As the t-SNE visualization below demonstrates, this method yields more compact clusters of normal embeddings and achieves a clearer separation from anomalies.

The reliability of Mahalanobis distance-based anomaly detection hinges on an exceptionally stable reference center. Many previous methods in anomalous sound detection use the arithmetic mean as this center. However, the mean minimizes squared deviations, a property that makes it highly sensitive to outliers; even a few extreme embeddings from label errors or sensor faults can pull the center away from the true distribution of normal data.

To overcome this, we employ the **geometric median**, which establishes a more robust reference point due to two fundamental properties:

1. **Outlier-Resistant Centering:** Unlike the mean, the geometric median minimizes the sum of absolute distances (L1). This ensures that each outlier’s influence is proportional to its distance, not its squared distance. As a result, extreme points have a limited and controlled impact on the estimated center.
2. **High Breakdown-Point Guarantee:** An estimator’s robustness can be measured by its breakdown point—the smallest fraction of bad data needed to corrupt its estimate. The arithmetic mean has a breakdown point of 0%, as a single outlier can shift it arbitrarily far. In contrast, the geometric median possesses a breakdown point of 50%, the highest possible value. This guarantees that the center remains anchored by the majority of genuine normal embeddings, even under heavy data contamination.

The practical benefit of this stability is evident in our below t-SNE visualizations, which show that using the geometric median produces more compact clusters for normal embeddings and achieves a clearer separation from anomalies.

![image.png](image%207.png)

## 4.3 Analysis of Core Hyperparameter

The key hyperparameter in our work is the confidence level $\alpha$ in Equation 5. As shown in below, across all noise ratios (2%, 4%, 6%, and 8%), a value of $\alpha = 0.05$ consistently yields the highest AUC.

Lower thresholds ($\alpha \le 0.03$) tend to under-filter noise, while higher thresholds ($\alpha \ge 0.08$) risk over-filtering and discarding informative normal variations. Both scenarios degrade performance. Therefore, we adopt $\alpha = 0.05$ in our experiments, as it strikes the best balance between noise rejection and data diversity under varied conditions.

![image.png](image%208.png)

# 5. Implementation Details

The input feature is the mel spectrogram, computed using a Hann window of length 2048 with 50% overlap and 128 mel filter banks. The encoder E(⋅) is based on the ResNet-18 architecture [1], generating a 512-dimensional linear output vector. To further refine the latent space, a projection head, implemented as a Multi-Layer Perceptron (MLP), consists of a 512-unit hidden layer followed by a 128-unit output layer, producing the final representation vectors.

The confidence level is set to α=0.05, while the temperature parameter t is empirically chosen as 0.007. Optimization is performed using the AdamW optimizer with a batch size of 128. The initial learning rate is selected from four logarithmically spaced values between 0.0005 and 0.01 and is adjusted using cosine annealing over 100 training epochs. The model is trained for a total of 400 epochs. The random seed is fixed as 42.

# 6. Conclusion & Discussion

To address the pervasive challenge of noisy data in real-world anomaly detection, we introduced SCAN, a selective contrastive learning framework. By employing the Mahalanobis distance with the highly robust geometric median, SCAN effectively identifies potential noise and progressively constructs a set of confident positive pairs. This allows it to learn discriminative latent representations within a contrastive learning framework, even when the training data is corrupted. Our extensive experiments have demonstrated the state-of-the-art (SOTA) performance of SCAN, benchmarking it against top-ranked AUC methods from both the DCASE 2022 and 2024 Task 2 challenges to ensure a fair and direct comparison.

While SCAN has demonstrated state-of-the-art performance, it represents a foundational step. We believe there are several exciting and critical directions for future research and development to make the framework more robust, scalable, and adaptable. We welcome discussion, ideas, and contributions in these areas.

### 6.1. Enhancing Scalability and Computational Efficiency

A key practical limitation of the current implementation is its computational overhead on very large datasets. The need to compute the geometric median and the full covariance matrix for all samples at each epoch is a bottleneck.

- **The Challenge:** These calculations do not scale linearly with the number of samples (`N`) and can be prohibitive for datasets with millions of entries, limiting SCAN's applicability in large-scale industrial settings.
- **Proposed Solutions:**
    - **Stochastic/Mini-Batch Approximations:** Instead of using the entire dataset, we plan to investigate methods for stably approximating the geometric median and covariance matrix from mini-batches. This involves exploring how to reduce the variance of these estimates to ensure the Mahalanobis distance calculation remains reliable.
    - **Online/Streaming Algorithms:** We will explore online algorithms that can update the center and covariance matrix incrementally with each new batch of data, rather than recomputing them from scratch. This would drastically reduce per-epoch computation time.
    - **Low-Rank Covariance Approximations:** For high-dimensional embeddings, the full covariance matrix is costly to store and invert. We will research the use of low-rank approximations to make these operations more tractable.
    - **Efficient Geometric Median Solvers:** We can explore faster, GPU-accelerated solvers for the geometric median to speed up the core centering step.

### 6.2 Developing Adaptive and Dynamic Selection Mechanisms

The current framework relies on a fixed confidence threshold `$\alpha$`, which may not be optimal for all datasets or throughout the entire training process.

- **The Challenge:** A single threshold creates a rigid trade-off. It may be too lenient for data with subtle noise or too aggressive for data with diverse but normal variations, potentially discarding useful samples.
- **Proposed Solutions:**
    - **Dynamic Threshold Schedules:** We propose investigating "confidence annealing" schedules, where the threshold `$\alpha$` is dynamically adjusted during training. For instance, the threshold could start high (more lenient) in early epochs when the model is still learning basic features and gradually become stricter as the latent space becomes more stable.
    - **Multi-Metric Filtering:** Relying solely on Mahalanobis distance might not capture all aspects of "abnormality." We plan to create a more sophisticated filtering mechanism by combining it with other metrics. A promising direction is to fuse it with **cosine distance**, which could help differentiate samples that are semantically distinct even if they are not statistical outliers in the Mahalanobis sense.
    - **Per-Cluster Adaptation:** For complex datasets where normal data consists of multiple operating modes (e.g., a machine at different speeds), we could first perform a lightweight clustering of the embeddings and then apply a separate SCAN selection mechanism within each cluster.

### 6.3 Leveraging Pre-Trained Foundation Models

SCAN currently trains its encoder (ResNet-18) from scratch. The initial training phase, where the model learns basic features, can be unstable, making the confident-pair selection less reliable at the start.

- **The Opportunity:** Using powerful, pre-trained audio foundation models (e.g., PANNs, BEATs, AST) can provide a much stronger and more stable feature representation from epoch one.
- **Proposed Solutions:**
    - **Transfer Learning Strategies:** We will experiment with using pre-trained audio encoders as the backbone for SCAN. This includes exploring different strategies like freezing the backbone and only training the projection head versus fine-tuning the entire model with a small learning rate.
    - **Impact on Convergence:** We hypothesize that this will not only improve the final AUC but also significantly speed up convergence and make the selection of confident pairs more robust from the very beginning.

### 6.4. Broader and More Diverse Benchmarking

To fully understand SCAN's capabilities and limitations, we need to test it against a wider array of challenges.

- **The Goal:** Move beyond the current datasets to establish more comprehensive and generalized performance claims.
- **Proposed Solutions:**
    - **Comparison with Diverse Robust Methods:** Benchmark SCAN against robust learning methods from other fields (e.g., noisy label learning in computer vision) that have been adapted for the audio domain.
    - **Controlled Noise Studies:** Conduct experiments on datasets with synthetically injected noise of various types (e.g., Gaussian, environmental, impulsive) and at precisely controlled signal-to-noise or corruption ratios.
    - **Cross-Dataset Generalization Tests:** Evaluate the model's ability to generalize by training it on one type of machine/environment and testing it on another, which is a critical test for real-world deployment.

