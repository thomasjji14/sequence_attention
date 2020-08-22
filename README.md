# *sequence_attention*: Learning, Visualizing and Exploring 16S rRNA Structure Using an Attention-based Deep Neural Network

*sequence_attention* is a deep learning model python package that trains a *Read2Phenotype* model on 16S rRNA data.
Drexel University EESI Lab, 2020        
Maintainer: Zhengqiao Zhao, zz374 at drexel dot edu        
Owner: Gail Rosen, gailr at ece dot drexel dot edu        

## Dependencies
The following are required:    
- python=3.5.4
- matplotlib=2.1.1
- pandas=0.20.3
- scipy=1.0.0
- scikit-learn=0.19.1
- Tensorflow=1.9.0
- Keras=2.2.2

## Data requirement
#### Raw data
A raw data folder is required. It contains a comma separated file (CSV) and FASTA nucleic acid files for different samples. The directory structure of raw data is shown below:
```
raw_data
│   meta_data.csv  
|   SAMPLE_1.fna
|   SAMPLE_2.fna
|   SAMPLE_3.fna
|   ...
```
Examples of the required files is shown below:
1. *meta_data.csv* example:
| sample_id | label  |
|-----------|--------|
| SAMPLE_1  | feces  |
| SAMPLE_2  | tongue |
| SAMPLE_3  | feces  |
| SAMPLE_4  | skin   |
| ...       | ...    |
2. SAMPLE_1.fna
```
>SAMPLE_1_1
GCGAGCGAAGTTCGGAATTACTGGGCGTAAAGGGTGTGTA
>SAMPLE_1_2
GCGAGCGTTGTTCGGAACCACTGGGCGTAAAGGGTGTGTA
>SAMPLE_1_3
GCGAGCGTTGTTCGGAATTACTGGGCGTAGAGGGTGTGTA
```
#### Processed data
Here is an example of processed data directory structure:
```
processed_data
│   meta_data.pkl 
|   train_test_split.pkl
|   label_dict.pkl
│
└───SAMPLE_1
│   │   SAMPLE_1_1.fna
│   │   SAMPLE_1_2.fna
│   │   ...
└───SAMPLE_2
|   │   SAMPLE_2_1.fna
|   │   SAMPLE_2_2.fna
|   │   ...
|   ...
```
