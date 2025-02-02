# NetRNApan
## Deciphering RNA modification and post-transcriptional regulation by deep learning framework 

Cellular immunity is orchestrated by T cells through their immense T-cell receptors (TCRs) repertoire, which interact with antigenic peptides presented by major histocompatibility complex (pMHC) molecules, but the specificities of the T cell response is largely undetermined because of the huge variety of TCRs. Here, we present DeepTR, a one-stop collection of unsupervised and supervised deep learning approaches for pan peptide-MHC class I binding prediction, TCR featurization, and accurate T cell response prediction. DeepTR yields higher predictive performance and more efficient feature representation for peptide binding to MHC and enables superior antigen-specific TCR featurization than current state-of-the-art approaches. Through a transfer learning strategy, DeepTR provides accurate prediction of T cell activation achieved by mimicking crucial steps of the antigen presentation pathway. DeepTR also enables the discovery of specific TCR groups with a new regulatory mechanism and characterizes important contact residues that mediate TCR-antigen binding specificity. DeepTR may advance our understanding of the mechanisms of T cell-mediated immunity and yield new insight in both personalized immune treatment and development of targeted vaccines. DeepTR is freely available at https://bioinfo.uth.edu/DeepTR..

<div align=center><img src="https://bioinfo.uth.edu/iapp/github/NetRNApan/Figure 1.jpg" width="800px"></div>

# Installation
Download NetRNApan by
```
git clone https://github.com/bsml320/NetRNApan
```
Installation has been tested in Linux server, CentOS Linux release 7.8.2003 (Core), with Python 3.7. Since the package is written in python 3x, python3x with the pip tool must be installed. NetRNApan uses the following dependencies: numpy, scipy, pandas, h5py, keras version=2.3.1, tensorflow=1.15 shutil, and pathlib. We highly recommend that users leave a message under the NetRNApan issue interface (https://github.com/bsml320/NetRNApan/issue) when encountering any installation and running problems. We will deal with it in time. You can install these packages by the following commands:
```
conda create -n NetRNApan python=3.7
conda activate NetRNApan
pip install pandas
pip install numpy
pip install scipy
pip install -v keras==2.3.1
pip install -v tensorflow==1.15
pip install seaborn
pip install shutil
pip install protobuf==3.20
pip install h5py==2.10.0
```
# Performance
First, the 10-fold CV was executed to assess NetRNApan's prediction performance on the training dataset. The ROC curves were drawn, and the corresponding AUC values were calculated. NetRNApan performed well, with an average AUC value of 0.9733 by 10-fold CV, ranging from 0.9697 to 0.9809, suggesting its good and reliable predictive power. NetRNApan prediction robustness was evaluated as well using the area under the precision-recall (AUPR) curve. The PR curve represents the trade-off between the number of false positive predictions and the number of false negative predictions. NetRNApan achieved AUPR values ranging from 0.9741 to 0.9819 during the balanced sample with 10-fold CV training, indicating that our model had great potential in predicting true positive sites with the high precision. Furthermore, we examined the generalizability of our models using an independent dataset that was not part of the training set. NetRNApan had the AUC value and AUPR of 0.9872 and 0.9877, respectively. These good and consistent metrics between 10-fold CV and independent testing indicated the promising accuracy and robustness. In addition, in our comparison of NetRNApan with other existing tools using an independent dataset, NetRNApan had AUC value improvement by over 3.5% (0.987 vs 0.954) for the modification sites prediction when compared to m5UPred. For performance comparison and benchmarking, we encoded 12 widely used features in RNA modification predictions, including nine sequence-based features (ANF, Binary, CKSNAP, DNC, ENAC, Kmer, NAC, TNC and RCKmer) and three types of physicochemical properties-based features (EIIP, NCP and PseDNC). 84 conventional machine-learning predictors were developed based on 12 types of features with seven algorithms (SVM, RF, LR, AB, SGD, DT, KNN and GB). The average AUC value of individual predictor was calculated based on 10-fold CV. In comparison, we found that the performance of NetRNApan was better than other 84 conventional machine-learning predictors, resulting in the AUC value improvements from 3.66-26.94%.

<div align=center><img src="https://bioinfo.uth.edu/iapp/github/NetRNApan/performance.jpg" width="800px"></div>

# Interpretability
The accuracy and robustness of the NetRNApan might be partly attributed to its deep neural network architecture, which is easily interpretable compared to the traditional machine-learning algorithms. The inputs can be projected via the hidden layers of NetRNApan to a representation space with lower dimensions. We used the UMAP approach to visually display the m5U sites and non-m5U sites in the training dataset based on the feature learnt at various network layers to demonstrate the capabilities of hierarchical representation using NetRNApan. We found that the feature representation became more discriminative along the network layer hierarchy. More specifically, the feature representations for m5U sites and non-m5U sites were mixed at the input layer. As the model continued to train, all nucleotides were grouped into two distinct clusters by the low-dimensional projection, reflecting binding specificities between m5U sites and non-m5U sites. 

<div align=center><img src="https://bioinfo.uth.edu/iapp/github/NetRNApan/Figure 3.jpg" width="800px"></div>

# Motif discovery
To interpret our deep learning model, we also decoded and analyzed the sequence features captured by our model from input nucleotides. Briefly, we first obtained the local segments captured by 256 convolution filters in the first convolution layer of our model. Each filter with a length of 10 nucleotides that was maximally activated at different regions of input. These activated features were then overlaid together to create position weight matrices (PWMs), which were considered as local motifs. To determine the representative motifs, motif score was calculated to measure the difference in the mean maximum activation scores between positive class and negative class. In total, 135 informative motifs were identified. Some strong motifs with higher scores were significantly enriched in positive samples such as “xCxGGG[A/U]x[C/U]U” (Kernel 101, score = 0.446, p-value < 0.01), “[C/U]xGxxxxGCG” (Kernel 138, score = 0.379, p-value < 0.01), and “UUCGAxxCxG” (Kernel 195, score = 0.365, p-value < 0.01). All motifs and the corresponding PWMs were graphically illustrated. Due to the significance of some motifs in modification, they could be found more than once. Then, the top 50 motifs were subjected to clustering analysis for further pattern mining. The pairwise Spearman's correlations between PWM of each motif were evaluated and hierarchical clustering was performed to the correlation matrix, yielding five representative motif clusters, such as UUCGAx[U/C], [C/G]GGUU[C/U]xAA, GGxCCCGG. We further analyzed one of the top-scoring motifs (Kernel 101, score = 0.446), and displayed the activation positions and distribution of the activation scores between m5U and non-m5U nucleotides. It was found that the motif had significantly greater activation scores and was enriched in modification sites and surrounding locations. Taken together, NetRNApan could identify particular patterns of recognition and revealed consensus motifs that may be significant for RNA modifications

<div align=center><img src="https://bioinfo.uth.edu/iapp/github/NetRNApan/Figure 5.jpg" width="800px"></div>

# Post-transcriptional regulation
Compared with the well characterized m6A modification and its increasingly prominent regulatory mechanism, there are still many unknown types of RNA modification. NetRNApan can provide a protein-binding perspective for understanding the functions of RNA modifications. In this study, we discovered 21 RBPs whose binding sites were significantly linked to the extracted motifs among the top 50 top-scoring motifs. For example, among all the identified RBPs, we found that the motif-43 identified by NetRNApan could be matched to the binding motif of ANKHD1 (p-value: 4.68E-03), which was found to interact with the major m5U methyltransferase TRMT2A. ANKHD1 is a large protein characterized by the presence of multiple ankyrin repeats and a K-homology (KH) domain, and its KH domain binds to RNA or ssDNA and is associated with transcriptional and translational regulation. In addition, functional enrichment analysis of these identified RBPs was also performed to further explore the potential regulatory roles of m5U. We found that several terms, including G-quadruplex RNA binding, regulation of (alternative) mRNA splicing, RNA transport and gene expression were enriched, which implied for their potential critical roles in transcriptional regulation.

<div align=center><img src="https://bioinfo.uth.edu/iapp/github/NetRNApan/Figure 6.jpg" width="800px"></div>

# Usage
Please cd to the NetBCE/prediction/ folder which contains predict.py.
Example: 
```
cd NetBCE/prediction/
python NetBCE_prediction.py -f ../testdata/test.fasta -o ../result/test_result
```
For details of other parameters, run:
```
python NetBCE_prediction.py --help
```
# Citation
Please cite the following paper for using: Xu H, Zhao Z. Deciphering RNA modification and post-transcriptional regulation by deep learning framework. In submission.
