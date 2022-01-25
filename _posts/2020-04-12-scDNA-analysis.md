---
title:  "scDNA seq analysis (1)"
excerpt: "about scDNA sequencing analysis"

categories:
  - NGS
tags:
  - sequencing

last_modified_at: 2020-08-05T
---


- 1. Computational Alnalysis
    
     scRNA-seq 실험에서 얻은 데이터의 computational 분석의 첫 번째 단계 (노란색)는 일반적인 시퀀싱입니다. 이후 단계 (오렌지)는 scRNASeq의 기술적 차이를 해결하기 위해 기존 RNASeq 분석 방법과 새로운 방법이 혼합되어 있습니다. 마지막으로 생물학적 해석 (파란색) 은 scRNASeq를 위해 특별히 개발 된 방법으로 분석 합니다.
    
    ![https://scrnaseq-course.cog.sanger.ac.uk/website/figures/flowchart.png](https://scrnaseq-course.cog.sanger.ac.uk/website/figures/flowchart.png)
    
    그림 2.2 : scRNA-seq 분석 순서도
    
    위 순서도의 단계를 수행 할 수있는 여러 가지 플랫폼
    
    - [Falco](https://github.com/VCCRI/Falco/) 는 클라우드에서 단일 세포 RNA-seq 처리 프레임 워크입니다.
    - [SCONE](https://github.com/YosefLab/scone)(Single-Cell Overview of Normalized Expression), 단일 세포 RNA-seq 데이터 품질 관리 및 표준화를위한 패키지.
    - [Seurat](http://satijalab.org/seurat/)은 단일 세포 RNA-seq 데이터의 QC, 분석 및 탐색을 위해 설계된 R 패키지입니다.
    - [ASAP](https://asap.epfl.ch/)(Automated Single-cell Analysis Pipeline)는 단일 셀 분석을위한 대화식 웹 기반 플랫폼입니다.
    - [Bioconductor](https://master.bioconductor.org/packages/release/workflows/html/simpleSingleCell.html)는 단일 세포 데이터 분석을위한 패키지를 포함하여 처리량이 많은 게놈 데이터 분석을위한 오픈 소스, 공개 개발 소프트웨어 프로젝트입니다
- 2. Alnalysis Platform
    
    
    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d1fe9cb1-8b91-4324-a864-16f9bca446f7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d1fe9cb1-8b91-4324-a864-16f9bca446f7/Untitled.png)
    
    - CEL-seq (Hashimshony et al. [2012](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Hashimshony2012-kd))
    - CEL-seq2 (Hashimshony et al. [2016](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Hashimshony2016-lx))
    - Drop-seq (Macosko et al. [2015](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Macosko2015-ix))
    - InDrop-seq (Klein et al. [2015](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Klein2015-kz))
    - MARS-seq (Jaitin et al. [2014](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Jaitin2014-ko))
    - SCRB-seq (Soumillon et al. [2014](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Soumillon2014-eu))
    - Seq-well (Gierahn et al. [2017](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Gierahn2017-es))
    - Smart-seq (Picelli et al. [2014](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Picelli2014-ic))
    - [https://satijalab.org/seurat/v3.2/pbmc3k_tutorial.html](https://satijalab.org/seurat/v3.2/pbmc3k_tutorial.html)
    - [SMARTer](http://www.clontech.com/US/Products/cDNA_Synthesis_and_Library_Construction/Next_Gen_Sequencing_Kits/Total_RNA-Seq/Universal_RNA_Seq_Random_Primed)
    - STRT-seq (Islam et al. [2013](https://scrnaseq-course.cog.sanger.ac.uk/website/introduction-to-single-cell-rna-seq.html#ref-Islam2014-cn))
    
    drop-seq와 Smart-seq2 사이에는 거의 두 가지 차이가 있습니다. 이는 프로토콜 선택이 연구에 큰 영향을 줄 수 있음을 나타냅니다.
    
    ![https://scrnaseq-course.cog.sanger.ac.uk/website/figures/ziegenhainEnardFig1.png](https://scrnaseq-course.cog.sanger.ac.uk/website/figures/ziegenhainEnardFig1.png)
    
    그림 2.7 : Enard 그룹 연구
    
    스벤손 (Svensson) 등은 다른 프로토콜의 정확성과 감도를 측정하기 위해 알려진 농도로 합성 전 사체 (스파이크 인, 나중에 더 자세히 설명)를 사용하여 다른 접근 방식을 취합니다. 광범위한 연구를 비교 한 결과, 프로토콜 간 상당한 차이도보고했습니다.
    
    ![https://scrnaseq-course.cog.sanger.ac.uk/website/figures/svenssonTeichmannFig2.png](https://scrnaseq-course.cog.sanger.ac.uk/website/figures/svenssonTeichmannFig2.png)
    
    그림 2.8 : Teichmann 그룹 연구
    
    프로토콜이 개발되고 기술적 소음을 정량화하기위한 계산 방법이 개선됨에 따라, 향후 연구가 다른 방법의 강점에 대한 추가 통찰력을 얻는 데 도움이 될 것입니다. 이러한 비교 연구는 연구자들이 사용할 프로토콜을 결정하는 데 도움이 될뿐만 아니라 벤치마킹을 통해 가장 유용한 전략을 결정할 수 있으므로 새로운 방법을 개발하는 데 도움이됩니다.
    
- 3. scRNA raw data processing
    
    **1) Quality check (Fast QC)**
    
       -대량 및 단일 세포 RNA-seq 데이터 모두에 사용할 수있는 quality check tool
    
    **2) Trimming Reads(Trim Galore!)**
    
       -bad quality reads를 트리밍 하여 data 정제
    
    **3) Demultiplexing**
    
       -노이즈를 제거하고 바코드에 따라 셀 구분
    
       -zUMI pipe-line을 따름
    
       -Smartseq2 나 paired-end full transcript protocols은 이미 demultiplexing 되어있음
    
    **4) Using STAR to Align Reads**
    
       -reference genome에 reads를 mapping
    
    **5) Kallisto and Pseudo-Alignment**
    
       -pseudo-aligners(kallisto) map k-mers to a reference
    
       -기본 정렬보다 빠르고 정확함
    
- 4. Construction of expression matrix
    
    **1) Mapping QC**
    
    -RSeQC
    
    **2) Reads quantification**
    
    -각 세포의 유전자 발현 정도 측정
    
    -e.g. HT-seq or FeatureCounts
    
    **3) Unique Molecular Identifiers (UMIs)**
    
    Unique Molecular Identifiers are short (4-10bp) random barcodes added to transcripts during reverse-transcription → noise와 biase를 제거 할 수 있다.
    
    3-1) Mapping Barcodes
    
    3-2) Counting Barcodes