# 3. Calculate 1D metrics for comparison of two samples

## 3.1 Quick start

``` shell
h1d two ISC ./test_data/GSE104334_Rad21KD.chr21.matrix.gz \
	./test_data/GSE104334_Ctrl.chr21.matrix.gz \
	50000 chr21 -o treat_vs_control_ISC
```

This command would generate a bedGraph file (`treat_vs_control_ISC.bedGraph`) for ISC.  An example result can be checked from [here](https://www.dropbox.com/s/l7n8bkqnmsmtdt6/treat_vs_control_ISC.bedGraph)

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

  | Type     | Description              | default value  |
  | -------- | ------------------------ | -------------- |
  | ISC      | square size              | 300000         |
  | CIC      | length of bins           | 300000         |
  | SSC      | length of bins           | 300000         |
  | deltaDLR | Local distance           | 3000000        |
  | CD       | Correlation method       | pearson        |
  | IASC     | Paramter for TAD calling | 300000         |
  | IESC     | Paramter for TAD calling | 300000         |
  | IFC      | FDR threshold            | 0.05           |
  | DRF      | length of bins           | 200000-5000000 |

  

## 3.3 Calculate 1D metrics (two-sample)

- Use contact matrix:

  ``` shell
  h1d two ISC ./test_data/GSE104334_Rad21KD.chr21.matrix.gz \
  	./test_data/GSE104334_Ctrl.chr21.matrix.gz 50000 chr21 \
  	--datatype matrix -p 300000 -o treat_vs_control_ISC
  ```

- Use raw hic:

  ``` shell
  h1d two ISC ./test_data/GSE104334_Rad21KD.hic \
  	./test_data/GSE104334_Ctrl.hic 50000 chr21 \
  	--datatype rawhic --gt ./test_data/hg19_genome_table.txt \
  	-p 300000 -o treat_vs_control_ISC
  ```

- Use cool file

  ``` shell
  h1d two ISC ./test_data/GSE104334_Rad21KD.50000.cool \
  	./test_data/GSE104334_Ctrl.50000.cool 50000 chr21 \
  	--datatype cool --gt ./test_data/hg19_genome_table.txt \
  	-p 300000 -o treat_vs_control_ISC
  ```

  


### 3.3.1 Multiprocessing for all chromomes:

- `chromosome` , set chromosome to "all" will compute metrics for all chromosomes.

- `data`, if calculating for all chromosomes, the input file should be absolute folder of contact matrix.

- `-maxchr`, Maximum index of chromosome (human genome is 22,i.e.). It will compute chromosome 1~maxchr plus chromosome X.

- `--prefix`, the prefix of matrix file, please modify the name of zipped matrix to `${prefix}chr1.matrix.gz`. If you used our [dump function](https://h1d.readthedocs.io/en/latest/basic.html#dump-all-chromosomes), the file should be:

  ```
  ├── observed.KR.chr1.matrix.gz
  ├── observed.KR.chr10.matrix.gz
  ├── observed.KR.chr11.matrix.gz
  ├── observed.KR.chr12.matrix.gz
  ├── observed.KR.chr13.matrix.gz
  ├── observed.KR.chr14.matrix.gz
  ```

  so the prefix is `observed.KR.`

- `-n`, Number of processors

To run all chromosomes parallel (treat vs control), do:

```shell
h1d one ISC ./test/Treat/ ./test/Control/  50000 all 
	--maxchr 22 --prefix observed.KR. -n 30 -o treat_vs_control
```

Output would be `treat_vs_control_IS_allchr.csv`.



## 3.4 Visualize 1D metrics (two-sample)

- Use contact matrix:

  ``` shell
  h1d two CIC ./test_data/GSE104334_Rad21KD.chr21.matrix.gz \
  	./test_data/GSE104334_Ctrl.chr21.matrix.gz 50000 chr21 \
	  --datatype matrix -p 300000 -o treat_vs_control_ISC \
  	--draw -s 26000000 -e 33000000
  ```
  
- Use raw `.hic` file

  ```shell
  h1d two CIC ./test_data/GSE104334_Rad21KD.hic \
  	./test_data/GSE104334_Ctrl.hic 50000 chr21 \
  	--datatype rawhic --gt ./test_data/hg19_genome_table.txt \
  	-p 300000 -o treat_vs_control_ISC \
  	--draw -s 26000000 -e 33000000
  ```


- Use `.cool` file

  ``` shell
  h1d two CIC ./test_data/GSE104334_Rad21KD.50000.cool \
  	./test_data/GSE104334_Ctrl.50000.cool 50000 chr21 \
  	--datatype cool --gt ./test_data/hg19_genome_table.txt \
  	-p 300000 -o treat_vs_control_ISC \
  	--draw -s 26000000 -e 33000000
  ```

<img src="_static/3-4.png" alt="RTDimport" style="zoom:60%;" />

