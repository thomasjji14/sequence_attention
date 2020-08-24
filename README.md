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
## Tutorial
#### Data Preprocessing
First, please review the settings in *config.py* file especially the data path. Then, run the following to preprocessing the data.
```python
import keras
import numpy as np
from sequence_attention import SeqAttModel, preprocess_data, DataGenerator, DataGeneratorUnlabeled
from config import Config

opt = Config()

preprocess_data(opt)
```
#### Model Initialization
Once the data is preprocessed, run the following to load metadata, initialize the model and the data generator.
```python
import pickle
label_dict = pickle.load(open('{}/label_dict.pkl'.format(opt.out_dir), 'rb')) 
sample_to_label, read_meta_data = pickle.load(open('{}/meta_data.pkl'.format(opt.out_dir), 'rb'))
partition = pickle.load(open('{}/train_test_split.pkl'.format(opt.out_dir), 'rb')) 
seq_att_model = SeqAttModel(opt)

training_generator = DataGenerator(partition['train'], sample_to_label, label_dict, 
                                   dim=(opt.SEQLEN,opt.BASENUM), batch_size=opt.batch_size, shuffle=opt.shuffle)
testing_generator = DataGenerator(partition['test'], sample_to_label, label_dict, 
                                   dim=(opt.SEQLEN,opt.BASENUM), batch_size=opt.batch_size, shuffle=opt.shuffle)

```
#### Model training and evaluation
Once the model is trained, you can evaluate the performance of the model using the training data and testing data. The following command will return accuracy.
```python
seq_att_model.train_generator(training_generator, n_workers=opt.n_workers)
seq_att_model.evaluate_generator(testing_generator, n_workers=opt.n_workers)

```
#### Model interpretation and sequence visualization
Prepare the X_visual (N by SEQ_LEN by NUMBASE) in *numpy array*, y_visual (phenotypic labels in integers) in *numpy array* and a list of taxonomic labels of those sequences (e.g., genus level labels). We also need a list of phenotypic labels of those sequences. Then run the following commands to plot embedding and attention weights visualization figures.
```python
prediction, attention_weights, sequence_embedding = seq_att_model.extract_weigths(X_visual)
from sequence_attention import SeqVisualUnit
idx_to_label = {label_dict[label]: label for label in label_dict}
seq_visual_unit = SeqVisualUnit(X_visual, y_visual, idx_to_label, taxa_label_list, 
                                prediction, attention_weights, sequence_embedding, 'Figures')
seq_visual_unit.plot_embedding()
seq_visual_unit.plot_attention('Prevotella')
```
In the code snippet above, we also need , *label_dict* (phenotypic labels to integer dictionary saved in `$opt.out_dir/label_dict.pkl` by the previous steps.