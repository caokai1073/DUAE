# UnionCom

## Paper
[Unsupervised Topological Alignment for Single-Cell Multi-Omics Integration](https://www.biorxiv.org/content/10.1101/2020.02.02.931394v2)

## Enviroment
Ubuntu 18.04.3 LTS  
python >= 3.6

numpy 1.17.3  
torch 1.3.0  
torchvision 0.4.1  
scikit-learn 0.21.3  

## Install
UnionCom software is available on the Python package index (PyPI), latest version 0.2.1. To install it using pip, simply type:
```
pip3 install unioncom
```
## v0.2.1
+ Software optimization.
+ Split function "train" into functions "Match" and "Project".
+ Use Kuhn-Munkres algorithm to find optimal pairs between datasets instead of parbabilistic matrix matching.
+ Add a new parameter "project" to provide options for barycentric projection.
+ Separate "test_label_transfer_accuracy" function from "fit_transform" function
+ fix some bugs

## Examples (jupyter notebook)

+ [Integration of simulations in UnionCom paper](https://github.com/caokai1073/UnionCom/blob/master/Examples/Simulation_example.ipynb)

+ [Integration of simulations in MMD-MA paper](https://github.com/caokai1073/UnionCom/blob/master/Examples/Simulation_data_from_MMD-MA.ipynb)

+ [batch correction](https://github.com/caokai1073/UnionCom/blob/master/Examples/Batch_correction_example.ipynb)

+ [integration multi-omics data](https://github.com/caokai1073/UnionCom/blob/master/Examples/scGEM_and_scNMT_example_example.ipynb)

+ [integration of datasets with specific cells](https://github.com/caokai1073/UnionCom/blob/master/Examples/dataset-specific_example.ipynb)

## Parameters
```
UnionCom.fit_transform(dataset, datatype=None, epoch_pd=30000, epoch_DNN=100, epsilon=0.001, 
lr=0.001, batch_size=100, rho=10, log_DNN=10, manual_seed=666, delay=0, 
beta=1, kmax=20, distance = 'geodesic', project='tsne', output_dim=32, test=False)
```
```
dataset: list of datasets to be integrated. [dataset1, dataset2, ...].
datatype: list of data type. [datatype1, datatype2, ...].
epoch_pd: epoch of Prime-dual algorithm.
epoch_DNN: epoch of training Deep Neural Network.
epsilon: training rate of data matching matrix F.
lr: training rate of DNN.
batch_size: training batch size of DNN.
rho: training damping term.
log_DNN: log step of training DNN.
manual_seed: random seed.
delay: delay steps of alpha. (from 0 to epoch_pd)
beta: trade-off parameter of structure preserving and point matching.
kmax: maximum value of knn when constructing geodesic distance matrix.
distance: mode of distance. [geodesic(suggested for multimodal integration), euclidean(suggested for batch correction)].
project:　mode of project, ['tsne', 'barycentric'], default is tsne.
output_dim: output dimension of integrated data.
test: test the label transfer accuracy, need datatype.
```

## Integrate data
Each row should contain the measured values for a single cell, and each column should contain the values of a feature across cells.
```data_0.txt, ... ,data_N.txt``` to be integrated, use
```
from unioncom import UnionCom
import numpy as np

data_0 = np.loadtxt("data_0.txt")
...
data_N = np.loadtxt("data_N.txt")

data = [data_0, ..., data_N]

integrated_data = UnionCom.fit_transform(data)

matched_data_0 = integrated_data[0]
...
matched_data_N = integrated_data[N]
```

## Test label transfer accuracy
To test the label transfer accuracy, you need to input cell types of ```data_0.txt, ... ,data_N.txt``` as ```type_0.txt, ... ,type_N.txt```
```
type_0 = np.loadtxt("type_0.txt")
...
type_N = np.loadtxt("type_N.txt")
datatype = [type_0,...,type_N]

UnionCom.test(data, datatype, test=True)
```

## Visualization by PCA
```
type_0 = type_0.astype(np.int)
...
type_N = type_N.astype(np.int)
datatype = [type_0,...,type_N]

UnionCom.PCA_visualize(data, integrated_data, datatype)
```


