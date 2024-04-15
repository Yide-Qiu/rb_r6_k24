A1 for W1: The Anisotropic Propagation Module (APM) is our original work. Anisotropic Propagation (AP) aims to anisotropically propagate semantic features to generate multi-hop propagation features in the feature propagation process. It balances the allocation of GPU and CPU resources, enhancing the cross-type edge awareness capability on large-scale graphs and effectively capturing multi-attribute structural information on large-scale heterogeneous graphs, which significantly outperforms existing methods. Hence, it is not an adaptation of existing work. For more details, please refer to Section 4.3.

A2 for W2&W3: We have expressed the details of the individual processes as much as possible. In this manuscript, we construct a universal large-scale heterogeneous graph, and we have provided a figure of the specific construction process of this large-scale heterogeneous graph. We have also proposed efficient representation learning methods on graphs of this scale, and correspondingly, we have provided algorithmic framework diagrams for the HGD framework and the APM module. To illustrate the inapplicability of metapath-like approaches on large-scale heterogeneous graphs, we also provide a metapath example diagram. Finally, to show the superiority of our proposed method with existing methods (226 times speedup and 37.8% accuracy improvement compared to HGT), we also plotted the statistics of the temporal overhead with baseline. If you think something needs to be corrected or there are other illustrations missing, please point them out specifically.

A3 for W4: We have attempted the two baselines you mentioned:
B1: HINormer: Representation Learning On Heterogeneous Information Networks with Graph Transformer
We have made efforts to reproduce HINormer on UniKG but were unsuccessful. The analysis of the issue is as follows:
1.The method was not developed for large-scale graphs, hence it is not the focus of our research.
2.HINormer's design for large-scale graphs is contradictory. The reason is that HINormer uses breadth-first search to generate node sequences, which serve as inputs to the transformer. In large-scale graphs, the average degree is high, leading to the discovery of nodes that are predominantly within the 1-hop neighborhood of the target node. This results in oversmoothed node features after message passing. When attempting to alleviate oversmoothing by extending the random walk sequence length, training the transformer with excessively long sequences introduces significant spatiotemporal overhead and convergence difficulties.

B2: Exploiting Latent Attribute Interaction with Transformer on Heterogeneous Information Networks
We have made efforts to reproduce this method on UniKG but were unsuccessful due to an Out of Memory (OOM) error. The reasons are as follows:
1.The method does not have open-source code available.
2.The method was not developed for large-scale graphs.
3.We further analyzed the temporal and spatial complexity of this method:
\alpha_{i j}=\frac{\exp \left(\sigma\left(\mathbf{a}^T\left[\mathbf{W} \tilde{\mathbf{h}}_i\left\|\mathbf{W} \tilde{\mathbf{h}}_j\right\| \mathbf{W}_{\mathbf{r}} \mathbf{r}_{\psi(i, j)}\right]\right)\right)}{\sum_{k \in \mathcal{N}_i} \exp \left(\sigma\left(\mathbf{a}^T\left[\mathbf{W} \tilde{\mathbf{h}}_i\left\|\mathbf{W} \tilde{\mathbf{h}}_k\right\| \mathbf{W}_{\mathbf{r}} \mathbf{r}_{\psi(i, k)}\right]\right)\right)}
The method concatenates relationship embeddings and node-pair representations and learns edge weights in a manner similar to GAT aggregation, implicitly capturing heterogeneous graph information.
Here, \mathbf{r}_{\psi(i, j)} is initialized as a one-hot vector of size [1, R] (similar to label embedding [A.9] initialization [1, c]). Therefore, the space complexity required for computing edge weights is O(RE). In UniKG, taking R=2082 as the number of relationships and E= 564,425,621, the minimum memory overhead required for computing edge weights is approximately (2082*77,312,474)*2B ≈ 2.14TB (assuming float16 type). We cannot afford such high space overhead.
In comparison, the space overhead of our proposed method, HGD, during training on UniKG is approximately 270GB, which is over 8.1 times smaller than this method (actually more, as we are only comparing its edge weight computation overhead).

[A.9] Tianlong Chen, Kaixiong Zhou, Keyu Duan, Wenqing Zheng, Peihao Wang, Xia Hu, and Zhangyang Wang. 2022. Bag of tricks for training deeper graph neural networks: A comprehensive benchmark study. IEEE Transactions on Pattern Analysis and Machine Intelligence 45, 3 (2022), 2769-2781.

A4 for W5: Thank you very much for your review, we will revise these minor issues in the manuscript.

A5 for Q1: On UniKG-1M and UniKG-10M, HGD achieves significantly higher values of accuracy, recall, and F1 score compared to the baselines. The reason for the high precision and low recall of the baseline methods: the model fails to capture all the features of the samples, which may lead to errors in predictions, resulting in the inability to cover all true positive instances.

A6 for Q2: In the ablation experiments section, we report improvements in four metrics: accuracy, recall, precision, and F1 score, compared to HGD-SGC, HGD-SIGN, and HGD-GAMLP. You can find these details in Table 7 and Section 5.3.


