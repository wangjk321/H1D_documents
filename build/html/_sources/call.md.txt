# 6. Extract secondary information from metrics (dTAD, stripeTAD, etc.)

## 6.1 Quick start

```shell
h1d call stripe ./test_data/Control/observed.KR.chr21.matrix.gz \
	50000 chr21 -o testname
```

The output will be: `testname_stripe.csv`

## 6.2 Parameters

```
h1d call -h
usage: __main__.py call [-h] [-o OUTNAME] [-c CONTROLMATRIX]
                        [--datatype DATATYPE] [--gt GT] [-p PARAMETER]
                        mode matrix resolution chromosome
```

- Required parameters:

  - `mode`, Running mode,,should be one of {dTAD,stripe,TAD,hubs}

  - `data`, Path of matrix file or raw .hic file.
  - `resolution`, resolution (50000, i.e.) of given contact matrix, or choosed resolution for analyzing `.hic` file.
  - `chromosome`, selected chromosome to be analyzed.

- Optional parameters:

  - `-o`,  output name, default: defaultname
  - `-c`, contact matrix or .hic file of control sample, which is required when using "dTAD" mode.
  - `--datatype`, type of input data: "matrix" (default) or "rawhic".
  - `--gt`, [genome table file](https://h1d.readthedocs.io/en/latest/overview.html#input-format) when using raw .hic data.
  - `-p`, parameters  for particular mode:

  | mode   | Which parameter | Default        |
  | ------ | --------------- | -------------- |
  | dTAD   | for DRF         | 200000-5000000 |
  | stripe | for IAS         | 300000         |
  | TAD    | for IS          | 300000         |
  | Hubs   | for IF          | 0.05           |



## 6.3 dTAD

h1d provide the function to call dTAD as 

```shell
h1d call dTAD ./test_data/Treat/observed.KR.chr21.matrix.gz \
	50000 chr21 -c ./test_data/Control/observed.KR.chr21.matrix.gz \
  --datatype matrix -o testname -p 200000-5000000
```

The output will be `testname_leftdTAD.csv` and `testname_rightdTAD.csv`, as:

| chr   | TADstart | TADend   |
| ----- | -------- | -------- |
| chr21 | 17900000 | 18250000 |
| chr21 | 26800000 | 27700000 |
| ...   | ...      | ...      |

## 6.4 stripe-TAD

This function simply divide all TAD into "loop", "left-stripe", "right-stripe" and "other" TAD:

``` shell
h1d call stripe ./test_data/Treat/observed.KR.chr21.matrix.gz 50000 chr21 \
	--datatype matrix -o testname -p 300000
```

The output will be `testname_stripe.csv`, as:

| chr   | TADstart | TADend   | TADtype    |
| ----- | -------- | -------- | ---------- |
| chr21 | 9500000  | 10250000 | loopTAD    |
| chr21 | 14800000 | 15600000 | leftStripe |
| chr21 | 15600000 | 16850000 | otherTAD   |
| ...   | ...      | ...      | ...        |

## 6.5 Hubs

This function extract chromatin Hubs as described in [PMID: 26272203](https://pubmed.ncbi.nlm.nih.gov/26272203/)

<div style="padding: 15px; border: 1px solid transparent; border-color: transparent; margin-bottom: 20px; border-radius: 4px; color: #31708f; background-color: #d9edf7; border-color: #bce8f1;"> Comutation of hubs requires rawhic input </div>

```shell
h1d call hubs ./test_data/inter_30.hic 50000 chr21 \
	--datatype rawhic --gt ./hg19/genome_table -o testname -p 0.05
```

The output will be `testname_hubs.csv` in `.bed` style, as :

| chr21 | 15450000 | 15750000 |
| ----- | -------- | -------- |
| chr21 | 15850000 | 16000000 |
| ...   | ...      | ...      |

## 6.6 TAD

This function will use Insulation Score to simply call TAD:

```shell
h1d call TAD ./test_data/Treat/observed.KR.chr21.matrix.gz 50000 chr21 \
	--datatype matrix -o testname -p 300000
```

