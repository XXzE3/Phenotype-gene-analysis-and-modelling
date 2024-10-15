# Phenotype gene analysis and modelling
Dataset - [[link]](https://www.kaggle.com/datasets/pritam1202/phenotype-gene-embeddings) <br>
Video link for project walkthrough - [[link]](https://drive.google.com/file/d/1DLlnf9h2_6L1W4fdAVzU5pXguzyS6JN8/view?usp=drive_link) <br>
Unique Hash Value : 
'4d69fbe946a2991088aa9242f15452b9'<br>
<br>
The goal of this project is to analyze phenotypes and the associated genes using their embeddings, identifying any pattern between genes that are causal and their corresponding phenotypes. Phenotype refers to a particular trait such as blonde hair or blue eyes and causal gene refers to gene that is responsible for the phenotype. 
<br> Quality check, cleaning and structuring of raw data were implemented for analysis. Dimensionality reduction and clustering techniques were applied to uncover patterns and similarities between phenotypes and genes.

## Datasets Used in the Analysis
- **Phenotypes and genes**: A trait or condition, with associated genes. Out of these, only one gene is causal.
- **Ground Truth**: The gene considered causal for the phenotype. The ordering in this file is consistent with the above file.
- **Phenotype embeddings**: The embeddings for the phenotype present.
- **Gene embeddings**: The embeddings for the genes present.

## Dataset Preparation
The above-mentioned datasets were loaded and gone through. The following steps were taken to create the final dataset from these datasets:

### Base Unique Dataset
- A unique set of 500 rows was extracted from the **Phenotype and Genes** dataset using the unique hash value. Duplicates from this dataset were removed after cross-checking.
- Using the same hash value, the corresponding causal genes were extracted from the **Ground Truth** dataset and merged with the previously derived dataset.
- The gene string for each phenotype was expanded into rows and labeled as 0 for non-causal and 1 for causal.

### Extracting the Phenotype and Gene Embeddings
- The embeddings of the phenotypes present in the base unique dataset were extracted from the **Phenotype Embedding** dataset. Embeddings for 10 phenotypes were missing, and these phenotypes were dropped from the base unique dataset.
- Embeddings of the genes present in the base unique dataset were extracted from the **Gene Embedding** dataset.

### Dimensionality Reduction
- The 3072-dimensional embeddings were reduced to 2 dimensions using **PCA** and **t-SNE**.
- The **t-SNE** reduced dimensions were able to capture 97% of the local relations present in the higher dimensions compared to 67% of the **PCA** components. Hence the **t-SNE** dimensions were taken forward.

### Final Unique Dataset
- The original embeddings and the reduced dimensions were merged with their corresponding phenotype and genes in the base unique dataset to create the final dataset.<br>
![image](https://github.com/user-attachments/assets/b3aacc3f-fa2e-4e61-8506-92fa4d26695b)

## Analysis
The final dataset was analyzed, and the following insights were gathered:

### Base Analysis
- 46 phenotypes were observed to have multiple causal genes. Shown below are the top 5 phenotypes among them:<br>
![image](https://github.com/user-attachments/assets/6476e08d-1062-420d-83a7-699207c97b4d)

- 48 of the genes were observed to be the causal gene for multiple phenotypes. Shown below are the top 5 genes among them:<br>
![image](https://github.com/user-attachments/assets/8dec9043-6fac-46bb-831c-13ec3623830d)

### Similarity Analysis
- The **cosine distance** to check direction similarity and the **euclidean distance** to check magnitude similarity were calculated between each gene and its corresponding phenotype. The parameters were calculated using the original embeddings.

- For multi-causal gene phenotypes, a considerable majority of the causal genes had a lower cosine and euclidean distance from their phenotypes when compared to the non-causal ones. Shown below is for the **Type 2 diabetes** phenotype:<br>
![image](https://github.com/user-attachments/assets/ac5c6fab-46ca-4f47-af58-588be8d60a7e)<br>
Please use this link to view for all multi-causal gene phenotype - [[link]](https://drive.google.com/file/d/1XqPXZ9kuQiTp67YBN-c_Le8I3D7h7tZK/view)
  
- For mono-causal gene phenotypes, a similar trend was observed where the causal genes were more similar to their phenotypes when compared to non-causal genes. Shown below is for the **Wrist fracture** phenotype:<br>
![image](https://github.com/user-attachments/assets/3e0acf9d-a274-4e09-82cc-4f9f6af74e3b)<br>
Please use this link to view for all mono-causal gene phenotype - [[link]](https://drive.google.com/file/d/1bEIRpK35R85-d2FnmPbu7LbBqI3qKGq1/view)

- An overall pattern of higher similarity between the phenotype and its causal genes compared to non-causal genes is observed in this case.

### Clustering
- The clustering of a particular phenotype was observed to check whether the phenotype and its causal genes end up in the same cluster. The number of clusters was limited to 2 as the target was to check whether the causal gene was in the same cluster as its phenotype. The reduced dimensions were used for clustering visualization.
- The clustering for the **Type 2 diabetes** phenotype is shown below.<br>
![image](https://github.com/user-attachments/assets/38086622-d52c-4b6f-9634-cd699928ab7c)<br>
Majority of the causal genes are clustered together with the phenotype, indicating greater similarity between the phenotype and its causal gene compared to the non-causal gene.

## Modelling
A **Support Vector Classifier (SVC)** was used to classify the genes as causal or non-causal.
- The **cosine** and **euclidean distances** were used as the parameters to classify, as the causality of a gene depends on the gene as well as the phenotype.
- An accuracy of 93.8% was observed on the test dataset, but on calculating the **f1-score**, a very poor f1-score was evaluated for the causal category. This is probably because of the fewer causal genes present in the dataset, leading to a bias toward the non-causal category by the model.

