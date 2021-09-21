# 4. Extract secondary information from metrics (dTAD, stripeTAD, etc.)

## 4.1 Quick start

```shell
h1d call stripe ./test_data/Control/observed.KR.chr21.matrix.gz \
	50000 chr21 -o testname
```

The output will be: `testname_stripe.csv`

## 4.2 Parameters

```
h1d call -h
usage: __main__.py call [-h] [-o OUTNAME] [-c CONTROLMATRIX]
                        [--datatype DATATYPE] [--gt GT] [-p PARAMETER]
                        mode matrix resolution chromosome
```

- Required parameters:

  - `mode`, Running mode,,should be one of {dTAD,stripe,stripeTAD, TAD,hubs}

  - `data`, Path of matrix file or raw .hic file.
  - `resolution`, resolution (50000, i.e.) of given contact matrix, or choosed resolution for analyzing `.hic` file.
  - `chromosome`, selected chromosome to be analyzed.

- Optional parameters:

  - `-o`,  output name, default: defaultname
  - `-c`, contact matrix or .hic file of control sample, which is required when using "dTAD" mode.
  - `--datatype`, type of input data: "matrix" (default) or "rawhic".
  - `--gt`, [genome table file](https://h1d.readthedocs.io/en/latest/overview.html#input-format) when using raw .hic data.
  - `-p`, parameters  for particular mode:

  | mode      | Which parameter       | Default        |
  | --------- | --------------------- | -------------- |
  | dTAD      | for DRF               | 200000-5000000 |
  | stripe    | for 'strong IAS peak' | 0.02           |
  | stripeTAD | for IAS               | 300000         |
  | TAD       | for IS                | 300000         |
  | Hubs      | for IF                | 0.05           |



!! Please note that "stripe" is different from "stripeTAD": Stripe is the regions with "stripe" structure, whereas stripe-TAD is asymmetric TAD (may contain many stripes). Thus stripe-TAD is the classfication from all TAD.



## 4.3 dTAD

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



## 4.4 stripe

This function automatically identify all regions with stripe 'structure'. The key idea is to find the sharp, strong IAS peaks. We used the similar strategy which use insulation score to identify TAD boundaries. For the stripe calling, after extracting the local maximum positions of IAS, only positions with IAS > IASmean is retained. Then, similar to Crane et.al, Nature 2015, we calculate a delta vector of IAS for each bin to extract only strong IAS peaks. To avoid clustered small peaks, the IAS value of a ‘stripe’ position should be higher than any position around 100kb. 

``` shell
h1d call stripe ./test_data/Treat/observed.KR.chr21.matrix.gz 50000 chr21 \
	--datatype matrix -o testname
```

This will output the summit of IAS signal, i.e. the stripes:

| chr   | start    | end      | IAS      |
| ----- | -------- | -------- | -------- |
| chr21 | 15450000 | 15500000 | 2.333717 |
| chr21 | 15900000 | 15950000 | 2.561192 |
| chr21 | 16300000 | 16350000 | 1.892257 |
| ...   | ...      | ...      | ...      |



## 4.5 stripe-TAD

This function simply divide all TAD into "loop", "left-stripe", "right-stripe" and "other" TAD:

``` shell
h1d call stripeTAD ./test_data/Treat/observed.KR.chr21.matrix.gz 50000 chr21 \
	--datatype matrix -o testname -p 300000
```

The output will be `testname_stripe.csv`, as:

| chr   | TADstart | TADend   | TADtype    |
| ----- | -------- | -------- | ---------- |
| chr21 | 9500000  | 10250000 | loopTAD    |
| chr21 | 14800000 | 15600000 | leftStripe |
| chr21 | 15600000 | 16850000 | otherTAD   |
| ...   | ...      | ...      | ...        |

## 4.6 Hubs

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

## 4.7 TAD

This function will use Insulation Score to simply call TAD:

```shell
h1d call TAD ./test_data/Treat/observed.KR.chr21.matrix.gz 50000 chr21 \
	--datatype matrix -o testname -p 300000
```

