# 3. Calculate 1D metrics for comparison of two samples

## 3.1 Quick start

``` shell
h1d two ISC ./test_data/Treat/observed.KR.chr21.matrix.gz \
	./test_data/Control/observed.KR.chr21.matrix.gz \
	50000 chr21 -o treat_vs_control_ISC
```

This command would generate a bedGraph file (`treat_vs_control_ISC.bedGraph`) for ISC:

## 3.2 Usage

The analysis of two-sample metrics cound be run by `h1d two` sub-command :

``` 
$ h1d two -h # type -h for help
usage: __main__.py two [-h] [-p PARAMETER] [-o OUTNAME] [-d] [-s START]
                       [-e END] [--datatype DATATYPE] [--gt GT]
                       type matrix controlmatrix resolution chromosome

1D metrics designed for comparison of two Hi-C samples

positional arguments:
  type                  Type of 1D metrics for two-sample comparison,should be
                        one of {ISC,CIC,SSC,deltaDLR,CD,IESC,IASC,IFC,DRF}
  matrix                Path of treated file (matrix or rawhic).
  controlmatrix         Path of control file (matrix or rawhic).
  resolution            Resolution of input matrix
  chromosome            Chromosome number ('chr21',i.e).

optional arguments:
  -h, --help            show this help message and exit
  -p PARAMETER, --parameter PARAMETER
                        Parameter for indicated metrics
  -o OUTNAME, --outname OUTNAME
                        output name (default: metricsChange)
  -d, --draw            Plot figure for candidate region
  -s START, --start START
                        Start sites for plotting
  -e END, --end END     End sites for plotting
  --datatype DATATYPE   matrix or rawhic
  --gt GT               genome table file
```

- `type` : type of 1D metrics could be one of {ISC,CIC,SSC,deltaDLR,CD,IESC,IASC,IFC,DRF}:

  - Insulation Score Change (**ISC**) ([PMID: 31495782](https://pubmed.ncbi.nlm.nih.gov/31495782/))
  - Contrast Index Change(**CIC**)
  - Separation Score Change(**SSC**)
  - Delta Distal-to-Local Ratio (**deltaDLR**) ([PMID: 30146161](https://pubmed.ncbi.nlm.nih.gov/30146161/))
  - Correlation Difference (**CD**)  ([PMID: 20513432](https://pubmed.ncbi.nlm.nih.gov/20513432/))
  - IntraScore Change(**IASC**) 
  - InterScore Change (**IESC**)
  - Interaction Frequency Change (**IFC**) 
  - Directional Relative Frequency (**DRF**)(Original metric)

  Details is shown in our paper:  *link in the future*

- `-p ` or `--parameter` for each metrics:

  | Type     | Description                               | default value |
  | -------- | ----------------------------------------- | ------------- |
  | ISC      | length of                                 | 1000000       |
  | CIC      | square size                               | 300000        |
  | SSC      |                                           |               |
  | deltaDLR |                                           |               |
  | CD       | Correlation method                        | pearson       |
  | IASC     |                                           |               |
  | IESC     |                                           |               |
  | IFC      | FDR threshold for signficant interactions | 0.05          |
  | DRF      |                                           |               |

  

## 3.3 Calculate 1D metrics (two-sample)

- Use contact matrix:

  ``` shell
  h1d two ISC ./test_data/Treat/observed.KR.chr21.matrix.gz \
  	./test_data/Control/observed.KR.chr21.matrix.gz 50000 chr21 \
  	--datatype matrix -p 300000 -o treat_vs_control_ISC
  ```

- Use raw hic:

  ``` shell
  h1d two ISC ./test_data/Treat/inter_30.hic \
  	./test_data/Control/inter_30.hic 50000 chr21 \
  	--datatype rawhic --gt ./reference/genome_table \
  	-p 300000 -o treat_vs_control_ISC
  ```

  

## 3.4 Visualize 1D metrics (two-sample)

- Use contact matrix:

  ``` shell
  h1d two CIC ./test_data/Rad21KD_1/observed.KR.chr21.matrix.gz ./test_data/Control_1/observed.KR.chr21.matrix.gz 50000 chr21 --datatype matrix -p 300000 -o treat_vs_control_ISC --draw -s 26000000 -e 33000000
  ```

  



<img src="_static/3-4.pdf" alt="RTDimport" style="zoom:60%;" />

