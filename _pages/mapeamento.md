---
layout: single
title: "Mapeamento de genomas"
permalink: /pages/mapeamento/
---

#Mapeamento
O mapeamento de reads curtas contra um genoma de referência é uma etapa fundamental nas análises genômicas, permitindo identificar a posição genômica exata de cada sequência obtida por tecnologias de sequenciamento de nova geração (NGS). Essa etapa é essencial para investigações como a detecção de variantes, estimativas de cobertura e análises de expressão gênica. Para essa tarefa, uma das ferramentas mais utilizadas é o BWA (Burrows-Wheeler Aligner), que realiza o alinhamento eficiente das sequências ao genoma de referência previamente indexado. Após o alinhamento, os resultados são gerados no formato SAM, que pode ser convertido e manipulado com o uso do SAMtools, uma suíte de programas voltada ao processamento de arquivos SAM/BAM. O SAMtools permite converter o arquivo para o formato binário BAM, ordenar os alinhamentos, indexar o arquivo resultante e extrair estatísticas relevantes. Esse fluxo de trabalho, que combina rapidez, precisão e compatibilidade com outras ferramentas bioinformáticas, é amplamente empregado em projetos de genômica comparativa, genética populacional e estudos de conservação.

##Preparação


```
from google.colab import drive
drive.mount('/content/drive/')
```


```
working_dir = "drive/MyDrive/Congen_ITV/"
```


```

```


```
import os
os.chdir(f"{working_dir}/bams")
```


```
# Install Miniconda (1–2 min)
!wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
!bash Miniconda3-latest-Linux-x86_64.sh -bfp /usr/local

# Add conda to the environment
import sys
sys.path.append('/usr/local/lib/3.7/site-packages')

# Add Bioconda channels
!conda config --add channels defaults
!conda config --add channels bioconda
!conda config --add channels conda-forge
```


```
!conda create -n genomics_env -y
```


```
!conda install -n genomics_env -c bioconda -c conda-forge bwa -y
```


```
!conda install -n genomics_env -c bioconda -c conda-forge samtools -y
```

##Análise

###Mapeamento


```
mtDNA_ref = "/content/drive/MyDrive/Congen_ITV/reference/RheHoff_mtDNA.fasta"
```


```
!conda run -n genomics_env bwa index {mtDNA_ref}
```


```
rawData_dir = "/content/drive/MyDrive/Congen_ITV/raw_data/"
```


```
bams_dir = "/content/drive/MyDrive/Congen_ITV/bams/"
```


```
!conda run -n genomics_env bwa
```


```
!conda run -n genomics_env samtools
```


```
!conda run -n genomics_env bash -c \
"bwa mem -t 2 {mtDNA_ref} {rawData_dir}ITV66053_1.subsampled.fastq.gz {rawData_dir}ITV66053_2.subsampled.fastq.gz | \
samtools view -Sb - > {bams_dir}ITV66053_1.mtDNA.bam"
```


```
!conda run -n genomics_env bash -c \
"bwa mem -t 2 {mtDNA_ref} {rawData_dir}ITV66073_1.subsampled.fastq.gz {rawData_dir}ITV66073_2.subsampled.fastq.gz | \
samtools view -Sb - > {bams_dir}ITV66073_1.mtDNA.bam"
```


```
!conda run -n genomics_env bash -c \
"bwa mem -t 2 {mtDNA_ref} {rawData_dir}ITV66053_1.subsampled.fastq.gz {rawData_dir}ITV66053_2.subsampled.fastq.gz | \
samtools view -Sb - | \
samtools sort -o {bams_dir}ITV66053_1.sorted.bam && \
samtools index {bams_dir}ITV66053_1.sorted.bam"
```

###Estatísticas


```
!conda run -n genomics_env bash -c \
"samtools flagstat {bams_dir}ITV66053_1.mtDNA.bam"
```
