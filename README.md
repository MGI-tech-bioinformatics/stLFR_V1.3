# stLFR_V1.3
Tools of stLFR(Single Tube Long Fragment Reads) Resequencing data analysis.

Introduction
----------------

    Tools of stLFR(Single Tube Long Fragment Reads) data analysis.
    stLFR FAQs is directed to bgi-MGITech_Bioinfor@genomics.cn.
    Download source code package from https://github.com/MGI-tech-bioinformatics/stLFR_V1.3.

Versions
----------------
    V1.3, 17/08/2020
      1. fix some bugs in result representation
      2. introduce smoove for SV detection
      3. publish on Docker only
  
    V1.2, 14/07/2020
      https://github.com/MGI-tech-bioinformatics/stLFR_V1.2

Setup
----------------
    1. Install docker follow the official website
      https://www.docker.com/
    2. Then do the following for the workflow:
      docker pull rjunhua/stlfr_reseq:v1.3
    3. Download and unzip the database
      From BGI Cloud Drive:
        https://pan.genomics.cn/ucdisk/s/Jvmuii
      Or from OneDrive:
        https://dwz.cn/ZPlGA0eJ
    Notes:
      1. Please make sure that you run the docker container with at least 45GB memory and 30 CPU.
      2. The input is sample list and output directory which descripted below (Main progarm arguments).
      3. The log files and temp results will not be removed now, so the memory must be double-checked.
    
  Running
  ----------------
    1. Please set the following variables on your machine:
      (a) $DB_LOCAL: directory on your local machine that has the database files.
      (b) $DATA_LOCAL: directory on your local machine that has the sequence data and "samplelist" file.
          "samplelist" must follow the format descripted bellow,
          and the *PATH* in "samplelist" must be absolute dicrtory of $DATA_LOCAL.
      (c) $RESULT_LOCAL: directory for result.
    2. Run the command:
        docker run -d -P \
        --name $STLFRNAME \
        -v $DB_LOCAL:/stLFR/db \
        -v $DATA_LOCAL:$DATA_LOCAL \
        -v $RESULT_LOCAL:$RESULT_LOCAL \
        rjunhua/stlfr_reseq:v1.3 \
        /bin/bash \
        /stLFR/bin/stLFR_SGE \
        $DATA_LOCAL/samplelist \
        $RESULT_LOCAL
    3. After report is generated:
        docker rm $STLFRNAME

Demo data
----------------
    two demo stLFR libraries are provided for testing, and every library consists two lanes:
      1. T0001-2:
            http://ftp.cngb.org/pub/CNSA/data1/CNP0000387/CNS0057111/
      2. T0001-4:
            http://ftp.cngb.org/pub/CNSA/data1/CNP0000387/CNS0094773/

Input: Sample List
----------------

         Name of input file. This is required.

         Three columns are required:
         1. name    : unique sample ID in this analysis
         2. path    : fastq path(s) for this sample split with colon(:)
         3. barcode : sample-barcode for each path split with colon(:), 0 means all used

         eg:  
            SAM1   /DATA/slide1/L01     1-4,5,7-9
            SAM2   /DATA/slide1/L02:/DATA/slide2/L02     1-5:6-9
         
Result
----------------
    After all analysis processes ending, you will get these files below:
      1.  HTML report:                              *_cn.html, *_en.html
      2.  raw data summary:                         *.fastqtable.xls
      3.  stLFR barcode summary:                    *.fragtable.xls
      4.  alignment summary:                        *.aligntable.xls
      5.  variant summary:                          *.varianttable.xls
      6.  haplotype phasing summary:                *.haplotype.xls, *.haplotype.pdf
      7.  evaluation summary (NA12878):             *.evaluation.xls
      8.  quality distrubution in cleanfq:          *.Cleanfq.qual.png
      9.  base distribution in cleanfq:             *.Cleanfq.base.png
      10. depth distribution in alignment:          *.Sequencing.depth.pdf
      11. accumulated depth distribution:           *.Sequencing.depth.accumulation.pdf
      12. insert size distrubition:                 *.Insertsize.metrics.txt, *.Insertsize.pdf
      13. GC bias distrubution:                     *.GCbias.metrics.txt, *.GCbias.pdf
      14. Fragment coverage figure:                 *.frag_cov.pdf
      15. Fragment length distribution figure:      *.fraglen_distribution_min5000.pdf
      16. Fragment per barcode distribution figure: *.frag_per_barcode.pdf
      17. variant CIRCOS:                           *.circos.svg, *.circos.png, *.legend_circos.pdf

Additional Information
----------------
    We provide stLFR_ReSeq workflow by Docker since stLFR_ReSeq_v1.3. 
    If docker is not aviable, please try stLFR_V1.2 (https://github.com/MGI-tech-bioinformatics/stLFR_V1.2) or contact us for more information.

License
----------------
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions： 
  
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
  
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
