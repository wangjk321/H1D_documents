# 1. Overview and Installation

## 1.1 Introduction

We presented “HiC1Dmetrics”, a pipeline that are able to calculate and analysis one-dimensional (1D) metrics for Hi-C data. HiC1Dmetrics is a Python3-based program (<https://github.com/wangjk321/HiC1Dmetrics>) and provide command line interface for UNIX system. 

## 1.2 Installation

HiC1Dmetrics were released on PyPI, and could be accessed by:

``` python
pip3 install h1d
```

## 1.3 Requirements

HiC1Dmetrics is based on python3 and it requires:

- Python (>= 3.6) packages:
  - pandas
  - numpy
  - scipy
  - scikit-learn
  - statsmodels
  - matplotlib
  - seaborn
  - fithic==2.0.7
  - multiprocess
- Others: 
  - bedtools >= 2.29.2

All required python packages will be automatically installed when use `pip install h1d`

## 1.4 Input format

HiC1Dmetrics support:

- raw `.hic` defined by [juicer](https://github.com/aidenlab/juicer/wiki) software.

- or dense matrix (raw or zipped) of intra-chromosomal contacts, like:

  |       |  0   | 25000 | 50000 | 75000 | ...  |
  | :---: | :--: | :---: | :---: | :---: | ---- |
  |   0   |  0   |   0   |   0   |   0   | ...  |
  | 25000 |  0   |   8   |   3   |   5   | ...  |
  | 50000 |  0   |   3   |   8   |   4   | ...  |
  | 75000 |  0   |   5   |   4   |   0   | ...  |
  |  ...  | ...  |  ...  |  ...  |  ...  | ...  |

Note: when use `.hic` file, a genome table file (tab-separated) must be prepared, which described the length of each chromosome for your genome reference: 

| chr1 | 248956422 |
| ---- | --------- |
| chr2 | 242193529 |
| ...  | ...       |





# 2. Calculate 1D metrics for one sample



# 3. Calculate 1D metrics for comparison of two samples

