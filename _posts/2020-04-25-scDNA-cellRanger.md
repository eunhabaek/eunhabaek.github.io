---
title:  "scDNA seq analysis(2) - cellRanger"
excerpt: "scDNA sequencing analysis with cellRanger"

categories:
  - NGS
tags:
  - sequencing

last_modified_at: 2020-09-12T
---
- cellRanger
    
    > **Cell Ranger란?**
    > 
    
     10xgenomics 사에서 만든 chromium을 통해 얻어진 scRNAseq을 분석하는 pipeline이다. reads를 align하고 feature barcode matrix, gene expression matrix등을 만든다.
    
    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab1a7935-4ef0-4384-a16f-82a8a38741a8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab1a7935-4ef0-4384-a16f-82a8a38741a8/Untitled.png)
    
    - **[cellranger mkfastq](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/mkfastq)** raw base call (BCL) 파일을 demultiplexing하여 FASTQ 파일로 만든다. Illumina사의 bcl2fastq와 동일한 역할이라 보면 된다.
    - **[cellranger count](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/count)** cellranger mkfastq를 통해 만들어진 FASTQ 파일을 input으로 하여 alignment, filtering, barcode counting, and UMI counting 과정을 진행하고 feature barcode matrix, gene expression matrix등을 만든다.
    - **[cellranger aggr](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/aggregate)** GEM Well을 여러개 사용한 실험일 경우에 사용. cellranger count의 각 결과를 aggregate하고 같은 sequencing depth로 정규화를 시킨다.
    - **[cellranger reanalyze](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/reanalyze)** input은 feature-barcode matrix로 파라미터 세팅을 새로 하여 dimensionality reduction, clustering, and gene expression algorithms 등을 재분석할 수 있다.
    
    > **Installing Cell Ranger**
    > 
    
     cellranger는 아래와 같은 하드웨어 환경을 요구한다.
    
    - 8-core Intel or AMD processor (16 cores recommended)
    - 64GB RAM (128GB recommended)
    - 1TB free disk space
    - 64-bit CentOS/RedHat 6.0 or Ubuntu 12.04
    
     하드웨어 환경이 요건에 맞는지 확인 후 다운을 받아보자.
    
    ```
    curl -o cellranger-3.1.0.tar.gz "https://cf.10xgenomics.com/releases/cell-exp/cellranger-3.1.0.tar.gz?Expires=1568276695&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cDovL2NmLjEweGdlbm9taWNzLmNvbS9yZWxlYXNlcy9jZWxsLWV4cC9jZWxscmFuZ2VyLTMuMS4wLnRhci5neiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTU2ODI3NjY5NX19fV19&Signature=FLKeP1u95hziLa8ViQywYlipCeuTvdecksuVu~JIVsUcy7pev8VfwSwQ5kdst4hWB641KuD2qSM-5meVgCLOwE1h50oiEqYHlFfZ9XT30bAOb74cT5amOGX6HrX5nBGocLrH~gymixrkEU-z9v22TYGdHMAiqV84qvWZYKOUPZsrLR3umQuT2uF19xxbaXbUCKsVfrCSAY~0AVfEyyFYYiKnmANtEOZ1FNM0Lr6LoSq4a8o9wU9YyMYJCOfYm6JqP5QYnXUHxUMzMM4j3vJ-zASLuXito0RvrxYzgM~zNRRFwGjvr98o0xkKZ6EzGXmunA08aPlgRMG6J9t1b02NMg__&Key-Pair-Id=APKAI7S6A5RYOXBWRPDA"
    wget -O cellranger-3.1.0.tar.gz "https://cf.10xgenomics.com/releases/cell-exp/cellranger-3.1.0.tar.gz?Expires=1568198894&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cDovL2NmLjEweGdlbm9taWNzLmNvbS9yZWxlYXNlcy9jZWxsLWV4cC9jZWxscmFuZ2VyLTMuMS4wLnRhci5neiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTU2ODE5ODg5NH19fV19&Signature=MAsku-zG5j7hV9svst0pxw9WTJk3ilq3DmLb1obNHmjsOcIL8CF2T0UzYpiGQ2lTQraiPNQ2nzhLgH1liGP~7JjoZ~HousFx1w7POggIWYkhDu7ktlGQCVvjxBqZvfoWpgVvi1U4lpy7Ud2c0Hq1r3HphUH-ZiHqiGSKfTarUZEQOLLx~5CXz~moIM~TwAF0KDLwbAPkceylZUOTcdzBAH~IsD-LBXK-7WbBA5ZOFOXhAtrLWQnzCRU0HjnkXRUNtKGAJO7ApXkg8PcNQaEWBGiJ6hrnCsbPdjaVFhOcRd-6mtV-zg2WTt1kVcd8UjA4GAQ42mm1SIw5xgTkWc-~cw__&Key-Pair-Id=APKAI7S6A5RYOXBWRPDA"
    
    tar -zxvf cellranger-3.1.0.tar.gz
    ```
    
     curl 또는 wget을 이용하여 파일을 받고 파일의 압축을 푼다.
    
    ```
    export PATH=/home/eunha/CellRanger/cellranger-3.1.0:$PATH
    ```
    
    cellranger가 있는 디렉토리의 경로를 불러오거나 .bashrc 파일에 경로를 붙여넣는다. 이제 cellranger command를 사용할 수 있다.
    
    ```
    cellranger sitecheck > sitecheck.txt
    ```
    
    cellranger sitecheck 명령어를 통해 만들어진 sitecheck.txt 파일을 읽어보면 내 하드웨어가 cellranger를 돌리기에 알맞은지 확인할 수 있다. 
    
    ```
    cellranger testrun --id=check_install
    ```
    
    cellranger testrun 명령어를 돌려서 cellranger가 제대로 설치되었는지를 확인할 수 있다.
    
    > **cellranger mkfastq**
    > 
    
    id = output name
    
    run = bcl file path
    
    csv = sample sheet path
    
    ```
    cellranger mkfastq --id=tutorial_walk_through \
    --run=/home/eunha/yard/run_cellrnager_mkfastq/cellranger-tiny-bcl-1.2.0 \
    --csv=/home/eunha/yard/run_cellrnager_mkfastq/cellranger-tiny-bcl-simple-1.2.0.csv
    ```
    
    > **cellranger count**
    > 
    
    id = output name
    
    fastqs = fastq file path
    
    sample = beginning of the FASTQ file name
    
    transcriptome = reference path
    
    ```
    cellranger count --id=run_count_1kpbmcs \
    --fastqs=/home/eunha/yard/run_cellranger_count/pbmc_1k_v3_fastqs \
    --sample=pbmc_1k_v3 \
    --transcriptome=/home/eunha/yard/run_cellranger_count/refdata-cellranger-GRCh38-3.0.0
    ```
    
    > **cellranger aggr**
    > 
    
    id = output name
    
    csv = output csv from cellranger count
    
    ```
    cellranger aggr --id=1k_10K_pbmc_aggr --csv=pbmc_aggr.csv
    ```
    
    > **cellranger reanalyze**
    > 
    
    id = output name
    
    matrix = input matrix file name
    
    params = parameter csv file name
    
    ```
    cellranger reanalyze --id=10k_pbmc_reanalyze_pc_clust --matrix=pbmc_10k_v3_filtered_feature_bc_matrix.h5 --params=reanalyze_10k_pbmcs.csv
    ```