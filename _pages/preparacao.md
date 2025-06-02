---
layout: single
title: "Formatos de Arquivos de Sequenciamento: FASTQ e BAM"
permalink: /pages/preparacao/
---

#Formato de dados

Um dos principais desafios em bioinformática é compreender como cada etapa analítica funciona e como transpor os resultados de uma análise para a seguinte. Isso ocorre pois, geralmente, cada programa possui um formato de arquivo distinto. Para ser capaz de interpretar os resultados e lidar com problemas que possam surgir é fundamental a compreensão de como cada etapa lida com seus arquivos de entrada e qual seu arquivo de saída.

O tutorial de hoje tem como objetivo mostrar os principais formatos de arquivo utilizados em genômica populacional e também como manipulá-los utilizando comandos em linha de comando e alguns programas úteis.

##Acessando os dados

Os dados se encontram na pasta do Google Drive que foi compartilhada. Para acessar esses dados precisamos montar o diretório no notebook.


```
from google.colab import drive
drive.mount('/content/drive/')
```

    Drive already mounted at /content/drive/; to attempt to forcibly remount, call drive.mount("/content/drive/", force_remount=True).


Agora vamos criar variáveis com os caminhos de alguns diretórios e arquivos que vamos utilizar com frequência.


```

##Pastas
working_dir = "drive/MyDrive/Congen_ITV
fastq_folder = "/content/drive/MyDrive/Congen_ITV/raw_data"
bam_folder = "drive/MyDrive/Congen_ITV/bams"
##Arquivos
reference_fasta = "drive/MyDrive/Congen_ITV/reference/bWildVid.reduced.fasta"
vcf_file = "drive/MyDrive/Congen_ITV/vcf/willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz"


```

##Conda
O Conda é um gerenciador de pacotes e ambientes muito utilizado em bioinformática por permitir a instalação, atualização e organização de softwares de forma controlada e reprodutível. Ele é especialmente útil quando se trabalha com ferramentas que possuem diferentes dependências ou versões conflitantes, pois permite a criação de ambientes isolados para cada projeto. Isso garante maior estabilidade e facilita a reprodutibilidade das análises, um aspecto fundamental na ciência. Através do canal Bioconda, milhares de pacotes específicos para bioinformática — como BWA, SAMtools, FastQC e GATK — estão disponíveis e podem ser facilmente instalados. Além disso, o Conda é amplamente usado em pipelines de análise, notebooks Jupyter e ambientes de computação de alto desempenho, onde diferentes projetos e ferramentas precisam coexistir de forma organizada.

Um dos pontos fortes desse gerenciador de pacotes é a capacidade de criar ambientes distintos. Isso permite que versões diferentes do mesmo programa ou linguagem de programação funcionem de forma simultânea. É uma boa prática sempre organizar suas análises em diferentes ambientes evitando a instalação de muitos pacotes e programas em um só.

Para instalar alguns programas precisamos utilizar o conda. Como o Google collab não mantém os arquivos instalados precisamos repetir esse passo antes de todas as aulas.


```
# Install Miniconda (1–2 min)
!wget -c https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
!chmod +x Miniconda3-latest-Linux-x86_64.sh
! {working_dir}/Miniconda3-latest-Linux-x86_64.sh -bfp /usr/local

# Add conda to the environment
import sys
sys.path.append('/usr/local/lib/3.7/site-packages')

# Create the conda environment
!conda create -n genomics_env -y

# Instalação de pacotes no ambiente "genomics_env"
!conda install -n genomics_env -y \
  -c conda-forge -c bioconda \
  samtools \
  bwa \
  fastqc \
  bioawk \
  seqtk \
  bcftools \
  bedtools \
  vcftools \
  gffread

```

    PREFIX=/usr/local
    Unpacking payload ...
    entry_point.py:256: DeprecationWarning:  3.14 will, by default, filter extracted tar archives and reject files or modify their metadata. Use the filter argument to control this behavior.
    entry_point.py:256: DeprecationWarning:  3.14 will, by default, filter extracted tar archives and reject files or modify their metadata. Use the filter argument to control this behavior.
    
    Installing base environment...
    
    Preparing transaction: ...working... done
    Executing transaction: ...working... done
    entry_point.py:256: DeprecationWarning:  3.14 will, by default, filter extracted tar archives and reject files or modify their metadata. Use the filter argument to control this behavior.
    installation finished.
    WARNING:
        You currently have a PATH environment variable set. This may cause
        unexpected behavior when running the  interpreter in Miniconda3.
        For best results, please verify that your PATH only points to
        directories of packages that are compatible with the  interpreter
        in Miniconda3: /usr/local
    Channels:
     - defaults
    Platform: linux-64
    Collecting package metadata (repodata.json): - \ | / - \ | / - \ | / done
    Solving environment: \ done
    
    
    ==> WARNING: A newer version of conda exists. <==
        current version: 25.3.1
        latest version: 25.5.0
    
    Please update conda by running
    
        $ conda update -n base -c defaults conda
    
    
    
    ## Package Plan ##
    
      environment location: /usr/local/envs/genomics_env
    
    
    
    
    Downloading and Extracting Packages:
    
    Preparing transaction: / done
    Verifying transaction: \ done
    Executing transaction: / done
    #
    # To activate this environment, use
    #
    #     $ conda activate genomics_env
    #
    # To deactivate an active environment, use
    #
    #     $ conda deactivate
    
    Channels:
     - conda-forge
     - bioconda
     - defaults
    Platform: linux-64
    Collecting package metadata (repodata.json): - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / done
    Solving environment: \ | done
    
    
    ==> WARNING: A newer version of conda exists. <==
        current version: 25.3.1
        latest version: 25.5.0
    
    Please update conda by running
    
        $ conda update -n base -c defaults conda
    
    
    
    ## Package Plan ##
    
      environment location: /usr/local/envs/genomics_env
    
      added / updated specs:
        - bcftools
        - bedtools
        - bioawk
        - bwa
        - fastqc
        - gffread
        - samtools
        - seqtk
        - vcftools
    
    
    The following NEW packages will be INSTALLED:
    
      _libgcc_mutex      conda-forge/linux-64::_libgcc_mutex-0.1-conda_forge 
      _openmp_mutex      conda-forge/linux-64::_openmp_mutex-4.5-2_gnu 
      alsa-lib           conda-forge/linux-64::alsa-lib-1.2.14-hb9d3cd8_0 
      bcftools           bioconda/linux-64::bcftools-1.21-h3a4d415_1 
      bedtools           bioconda/linux-64::bedtools-2.31.1-h13024bc_3 
      bioawk             bioconda/linux-64::bioawk-1.0-h577a1d6_13 
      bwa                bioconda/linux-64::bwa-0.7.19-h577a1d6_1 
      bzip2              conda-forge/linux-64::bzip2-1.0.8-h4bc722e_7 
      c-ares             conda-forge/linux-64::c-ares-1.34.5-hb9d3cd8_0 
      ca-certificates    conda-forge/noarch::ca-certificates-2025.4.26-hbd8a1cb_0 
      cairo              conda-forge/linux-64::cairo-1.18.4-h3394656_0 
      fastqc             bioconda/noarch::fastqc-0.12.1-hdfd78af_0 
      font-ttf-dejavu-s~ conda-forge/noarch::font-ttf-dejavu-sans-mono-2.37-hab24e00_0 
      font-ttf-inconsol~ conda-forge/noarch::font-ttf-inconsolata-3.000-h77eed37_0 
      font-ttf-source-c~ conda-forge/noarch::font-ttf-source-code-pro-2.038-h77eed37_0 
      font-ttf-ubuntu    conda-forge/noarch::font-ttf-ubuntu-0.83-h77eed37_3 
      fontconfig         conda-forge/linux-64::fontconfig-2.15.0-h7e30c49_1 
      fonts-conda-ecosy~ conda-forge/noarch::fonts-conda-ecosystem-1-0 
      fonts-conda-forge  conda-forge/noarch::fonts-conda-forge-1-0 
      freetype           conda-forge/linux-64::freetype-2.13.3-ha770c72_1 
      gffread            bioconda/linux-64::gffread-0.12.7-h077b44d_6 
      giflib             conda-forge/linux-64::giflib-5.2.2-hd590300_0 
      graphite2          conda-forge/linux-64::graphite2-1.3.13-h59595ed_1003 
      gsl                conda-forge/linux-64::gsl-2.7-he838d99_0 
      harfbuzz           conda-forge/linux-64::harfbuzz-11.2.1-h3beb420_0 
      htslib             bioconda/linux-64::htslib-1.21-h566b1c6_1 
      icu                conda-forge/linux-64::icu-75.1-he02047a_0 
      keyutils           conda-forge/linux-64::keyutils-1.6.1-h166bdaf_0 
      krb5               conda-forge/linux-64::krb5-1.21.3-h659f571_0 
      lcms2              conda-forge/linux-64::lcms2-2.17-h717163a_0 
      lerc               conda-forge/linux-64::lerc-4.0.0-h0aef613_1 
      libblas            conda-forge/linux-64::libblas-3.9.0-31_h59b9bed_openblas 
      libcblas           conda-forge/linux-64::libcblas-3.9.0-31_he106b2a_openblas 
      libcups            conda-forge/linux-64::libcups-2.3.3-h4637d8d_4 
      libcurl            conda-forge/linux-64::libcurl-8.14.0-h332b0f4_0 
      libdeflate         conda-forge/linux-64::libdeflate-1.24-h86f0d12_0 
      libedit            conda-forge/linux-64::libedit-3.1.20250104-pl5321h7949ede_0 
      libev              conda-forge/linux-64::libev-4.33-hd590300_2 
      libexpat           conda-forge/linux-64::libexpat-2.7.0-h5888daf_0 
      libffi             conda-forge/linux-64::libffi-3.4.6-h2dba641_1 
      libfreetype        conda-forge/linux-64::libfreetype-2.13.3-ha770c72_1 
      libfreetype6       conda-forge/linux-64::libfreetype6-2.13.3-h48d6fc4_1 
      libgcc             conda-forge/linux-64::libgcc-15.1.0-h767d61c_2 
      libgcc-ng          conda-forge/linux-64::libgcc-ng-15.1.0-h69a702a_2 
      libgfortran        conda-forge/linux-64::libgfortran-15.1.0-h69a702a_2 
      libgfortran5       conda-forge/linux-64::libgfortran5-15.1.0-hcea5267_2 
      libglib            conda-forge/linux-64::libglib-2.84.2-h3618099_0 
      libgomp            conda-forge/linux-64::libgomp-15.1.0-h767d61c_2 
      libiconv           conda-forge/linux-64::libiconv-1.18-h4ce23a2_1 
      libjpeg-turbo      conda-forge/linux-64::libjpeg-turbo-3.1.0-hb9d3cd8_0 
      liblzma            conda-forge/linux-64::liblzma-5.8.1-hb9d3cd8_1 
      libnghttp2         conda-forge/linux-64::libnghttp2-1.64.0-h161d5f1_0 
      libopenblas        conda-forge/linux-64::libopenblas-0.3.29-pthreads_h94d23a6_0 
      libpng             conda-forge/linux-64::libpng-1.6.47-h943b412_0 
      libssh2            conda-forge/linux-64::libssh2-1.11.1-hcf80075_0 
      libstdcxx          conda-forge/linux-64::libstdcxx-15.1.0-h8f9b012_2 
      libstdcxx-ng       conda-forge/linux-64::libstdcxx-ng-15.1.0-h4852527_2 
      libtiff            conda-forge/linux-64::libtiff-4.7.0-hf01ce69_5 
      libuuid            conda-forge/linux-64::libuuid-2.38.1-h0b41bf4_0 
      libwebp-base       conda-forge/linux-64::libwebp-base-1.5.0-h851e524_0 
      libxcb             conda-forge/linux-64::libxcb-1.17.0-h8a09558_0 
      libxcrypt          conda-forge/linux-64::libxcrypt-4.4.36-hd590300_1 
      libzlib            conda-forge/linux-64::libzlib-1.3.1-hb9d3cd8_2 
      ncurses            conda-forge/linux-64::ncurses-6.5-h2d0b736_3 
      openjdk            conda-forge/linux-64::openjdk-23.0.2-h53dfc1b_2 
      openssl            conda-forge/linux-64::openssl-3.5.0-h7b32b05_1 
      pcre2              conda-forge/linux-64::pcre2-10.45-hc749103_0 
      perl               conda-forge/linux-64::perl-5.32.1-7_hd590300_perl5 
      pixman             conda-forge/linux-64::pixman-0.46.0-h29eaf8c_0 
      pthread-stubs      conda-forge/linux-64::pthread-stubs-0.4-hb9d3cd8_1002 
      samtools           bioconda/linux-64::samtools-1.21-h96c455f_1 
      seqtk              bioconda/linux-64::seqtk-1.5-h577a1d6_0 
      vcftools           bioconda/linux-64::vcftools-0.1.17-pl5321h077b44d_0 
      xorg-libice        conda-forge/linux-64::xorg-libice-1.1.2-hb9d3cd8_0 
      xorg-libsm         conda-forge/linux-64::xorg-libsm-1.2.6-he73a12e_0 
      xorg-libx11        conda-forge/linux-64::xorg-libx11-1.8.12-h4f16b4b_0 
      xorg-libxau        conda-forge/linux-64::xorg-libxau-1.0.12-hb9d3cd8_0 
      xorg-libxdmcp      conda-forge/linux-64::xorg-libxdmcp-1.1.5-hb9d3cd8_0 
      xorg-libxext       conda-forge/linux-64::xorg-libxext-1.3.6-hb9d3cd8_0 
      xorg-libxfixes     conda-forge/linux-64::xorg-libxfixes-6.0.1-hb9d3cd8_0 
      xorg-libxi         conda-forge/linux-64::xorg-libxi-1.8.2-hb9d3cd8_0 
      xorg-libxrandr     conda-forge/linux-64::xorg-libxrandr-1.5.4-hb9d3cd8_0 
      xorg-libxrender    conda-forge/linux-64::xorg-libxrender-0.9.12-hb9d3cd8_0 
      xorg-libxt         conda-forge/linux-64::xorg-libxt-1.3.1-hb9d3cd8_0 
      xorg-libxtst       conda-forge/linux-64::xorg-libxtst-1.2.5-hb9d3cd8_3 
      zlib               conda-forge/linux-64::zlib-1.3.1-hb9d3cd8_2 
      zstd               conda-forge/linux-64::zstd-1.5.7-hb8e6e7a_2 
    
    
    
    Downloading and Extracting Packages:
    
    Preparing transaction: - \ | done
    Verifying transaction: - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ done
    Executing transaction: / - \ | / - \ | / - \ | / done


___

##Arquivo FASTQ
Um arquivo FASTQ é um formato padrão utilizado para armazenar sequências de DNA ou RNA obtidas por tecnologias de sequenciamento de nova geração (NGS). Cada entrada no arquivo contém quatro linhas: a primeira é um identificador da sequência, precedido por “@”; a segunda apresenta a própria sequência nucleotídica; a terceira linha começa com “+” e pode repetir o identificador ou ser deixada em branco; e a quarta linha armazena a qualidade de cada base da sequência, representada por caracteres ASCII que codificam os valores de qualidade Phred. Esse formato permite combinar informações de sequência e qualidade em um único arquivo, sendo amplamente utilizado em análises genômicas.

O comando `head` é útil para visualizar arquivos FASTQ porque permite inspecionar rapidamente as primeiras linhas do arquivo, facilitando a verificação do formato, da presença de sequências e da codificação da qualidade.


```
!ls {fastq_folder}
```

    ITV66053_1.subsampled_fastqc.html  ITV66073_1.subsampled_fastqc.html
    ITV66053_1.subsampled_fastqc.zip   ITV66073_1.subsampled_fastqc.zip
    ITV66053_1.subsampled.fastq.gz	   ITV66073_1.subsampled.fastq.gz
    ITV66053_2.subsampled_fastqc.html  ITV66073_2.subsampled_fastqc.html
    ITV66053_2.subsampled_fastqc.zip   ITV66073_2.subsampled_fastqc.zip
    ITV66053_2.subsampled.fastq.gz	   ITV66073_2.subsampled.fastq.gz



```
!zcat {fastq_folder}/ITV66053_1.subsampled.fastq.gz | head
```

    @VH01766:6:AAC7L7KHV:1:1101:29971:1000 1:N:0:AAGCACTG+AAGGCGTA
    GATGTTATGTATCTATGATATCTTTTTACTCCCCACTACAGAGCCTTCCTTTTTGCTACAGTCATTTGCAATAAGACCATCAAAAGC
    +
    ;CCCCCCCC-CCCCCCCCC;CCCC-CCCCCCCCCC;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC;CC-CCCCCCCCCCC
    @VH01766:6:AAC7L7KHV:1:1101:36750:1000 1:N:0:AAGCACTG+AAGGCGTA
    NTGTAATTAAACACCTTCTAAATGCCTCAGTAAATAAAACAGATTGATGCAATTGTTCTGCAGGTGGACCAGTATAACTGCTCTGCACACAGACTTGGTGATATTTCCTGTGACTGCTTGGGTTTCTGGCCTGATTAGCACACATGGATTG
    +
    #;CCCCCCC;;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC
    @VH01766:6:AAC7L7KHV:1:1101:46653:1057 1:N:0:AAGCACTG+AAGGCGTA
    NTTTTTAGTATTGCACAGAAATGACACGCAAATGCATTGTATTTTTGGTGCTTATTTATTAGTTGCCCTAGATGCAAAGCATTCAGTTATAAAAGAAAAAAAAAAAGAAACCATGTGCTTAACTTAATCCCAAGAGTTTTCCAATTGCCCT


O comando `wc` (word count) serve para contar linhas, palavras e caracteres em arquivos, e no contexto de arquivos FASTQ, ele é útil principalmente para contar o número total de linhas. Como cada leitura em um arquivo FASTQ ocupa exatamente quatro linhas, dividir o número total de linhas por quatro permite estimar rapidamente o número de sequências presentes no arquivo, o que é essencial para o controle de qualidade e o planejamento das análises.


```
!zcat {fastq_folder}/ITV66053_1.subsampled.fastq.gz | wc -l
```

    3166636


O comando `grep` é utilizado para buscar padrões de texto dentro de arquivos. No contexto de arquivos FASTQ, `grep` é útil, por exemplo, para localizar sequências específicas, identificadores de leituras, verificar a presença de determinados adaptadores ou detectar caracteres incomuns. Ele permite filtrar rapidamente informações relevantes em grandes volumes de dados, sendo uma ferramenta essencial para inspeções rápidas e diagnósticos em bioinformática.


```
!zgrep "@VH" {fastq_folder}/ITV66053_1.subsampled.fastq.gz | head
```

    @VH01766:6:AAC7L7KHV:1:1101:29971:1000 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:36750:1000 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:46653:1057 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:27358:1114 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:42810:1151 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:20750:1208 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:34099:1227 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:23685:1265 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:45006:1265 1:N:0:AAGCACTG+AAGGCGTA
    @VH01766:6:AAC7L7KHV:1:1101:37053:1303 1:N:0:AAGCACTG+AAGGCGTA


Adicionando o parâmetro `-c` podemos contar o número de sequências no arquivo


```
!zgrep -c "@VH" {fastq_folder}/ITV66053_1.subsampled.fastq.gz
```

    791659


Além de `head`, `wc` e `grep`, vários outros comandos  são extremamente úteis para manipular e inspecionar arquivos FASTQ. Aqui estão alguns dos principais:

1. `zcat`, `gzip`, `gunzip`
	•	Uso: Muitos arquivos FASTQ são comprimidos como .fastq.gz.
	•	Exemplo: `zcat arquivo.fastq.gz | head -n 8` – visualiza as primeiras duas leituras.
	•	Alternativa: `less <(zcat arquivo.fastq.gz)` para navegação.

2. `cut`
	•	Uso: Extrai colunas específicas, útil para trabalhar com nomes ou qualidade.
	•	Exemplo: `cut -c1-50 arquivo.fastq` – mostra os 50 primeiros caracteres de cada linha.

3. `awk`
	•	Uso: Manipulação mais avançada por padrão de linhas (ex: filtrar apenas sequências).
	•	Exemplo: `awk 'NR%4==2' arquivo.fastq` – extrai apenas as linhas de sequência.


4. `sed`
	•	Uso: Substituições e edições rápidas no conteúdo.
	•	Exemplo: `sed -n '2~4p'` arquivo.fastq – mesma função que o `awk` acima.


5. `sort` e `uniq`
	•	Uso: Verifica repetições ou padrões.
	•	Exemplo: `awk 'NR%4==1' arquivo.fastq | sort | uniq -d` – detecta identificadores duplicados.


6. `grep -A`, `grep -B`
	•	Uso: Busca e mostra linhas antes ou depois do padrão.
	•	Exemplo: `grep -A3 '@SEQ_ID' arquivo.fastq` – mostra a leitura completa que começa com @SEQ_ID.


7. `paste`, `split`, `tee`
	•	Uso: Para dividir, mesclar ou redirecionar FASTQs em múltiplas partes ou saídas.


Esses comandos podem ser combinados em pipes (|) para criar fluxos de processamento poderosos e eficientes para arquivos FASTQ, sem a necessidade de abrir programas pesados.

##Controle de qualidade com o FASTQC
O FastQC é uma ferramenta amplamente utilizada na bioinformática para realizar uma avaliação rápida da qualidade de dados de sequenciamento em arquivos FASTQ. Ele gera relatórios gráficos e estatísticos que ajudam a identificar possíveis problemas nos dados, como baixa qualidade de leitura, contaminação por adaptadores, viés na composição de bases ou duplicações excessivas. Essa checagem é essencial antes de qualquer análise genômica, garantindo que os dados utilizados sejam confiáveis e adequados para os objetivos do estudo.

###Instalação
Para poder utilizar o programa, primeiro, precisamos intalá-lo em nosso ambiente. Para isso utilizamos o comando `apt-get install` e após a instalação podemos checar se o programa está funcionando chamando ele.


```
!conda run -n genomics_env fastqc --version
```

    FastQC v0.12.1
    



```
!ls -lh {fastq_folder}
```

    total 26G
    -rw------- 1 root root 5.4G May 22 13:09 ITV66053_1.fastq.gz
    -rw------- 1 root root  53M May 26 12:55 ITV66053_1.subsampled.fastq.gz
    -rw------- 1 root root 5.7G May 22 13:10 ITV66053_2.fastq.gz
    -rw------- 1 root root  56M May 26 12:57 ITV66053_2.subsampled.fastq.gz
    -rw------- 1 root root 6.9G May 22 13:16 ITV66073_1.fastq.gz
    -rw------- 1 root root  68M May 26 13:01 ITV66073_1.subsampled.fastq.gz
    -rw------- 1 root root 7.3G May 22 13:15 ITV66073_2.fastq.gz
    -rw------- 1 root root  72M May 26 13:06 ITV66073_2.subsampled.fastq.gz


###Como rodar o programa


```
!conda run -n genomics_env fastqc {fastq_folder}/ITV66053_1.subsampled.fastq.gz -o {fastq_folder}
```

    application/gzip
    Analysis complete for ITV66053_1.subsampled.fastq.gz
    
    Started analysis of ITV66053_1.subsampled.fastq.gz
    Approx 5% complete for ITV66053_1.subsampled.fastq.gz
    Approx 10% complete for ITV66053_1.subsampled.fastq.gz
    Approx 15% complete for ITV66053_1.subsampled.fastq.gz
    Approx 20% complete for ITV66053_1.subsampled.fastq.gz
    Approx 25% complete for ITV66053_1.subsampled.fastq.gz
    Approx 30% complete for ITV66053_1.subsampled.fastq.gz
    Approx 35% complete for ITV66053_1.subsampled.fastq.gz
    Approx 40% complete for ITV66053_1.subsampled.fastq.gz
    Approx 45% complete for ITV66053_1.subsampled.fastq.gz
    Approx 50% complete for ITV66053_1.subsampled.fastq.gz
    Approx 55% complete for ITV66053_1.subsampled.fastq.gz
    Approx 60% complete for ITV66053_1.subsampled.fastq.gz
    Approx 65% complete for ITV66053_1.subsampled.fastq.gz
    Approx 70% complete for ITV66053_1.subsampled.fastq.gz
    Approx 75% complete for ITV66053_1.subsampled.fastq.gz
    Approx 80% complete for ITV66053_1.subsampled.fastq.gz
    Approx 85% complete for ITV66053_1.subsampled.fastq.gz
    Approx 90% complete for ITV66053_1.subsampled.fastq.gz
    Approx 95% complete for ITV66053_1.subsampled.fastq.gz
    


Agora podemos repetir para os demais arquivos.


```
!conda run -n genomics_env fastqc {fastq_folder}/ITV66053_2.subsampled.fastq.gz -o {fastq_folder}
!conda run -n genomics_env fastqc {fastq_folder}/ITV66073_1.subsampled.fastq.gz -o {fastq_folder}
!conda run -n genomics_env fastqc {fastq_folder}/ITV66073_2.subsampled.fastq.gz -o {fastq_folder}
```

    application/gzip
    Analysis complete for ITV66053_2.subsampled.fastq.gz
    
    Started analysis of ITV66053_2.subsampled.fastq.gz
    Approx 5% complete for ITV66053_2.subsampled.fastq.gz
    Approx 10% complete for ITV66053_2.subsampled.fastq.gz
    Approx 15% complete for ITV66053_2.subsampled.fastq.gz
    Approx 20% complete for ITV66053_2.subsampled.fastq.gz
    Approx 25% complete for ITV66053_2.subsampled.fastq.gz
    Approx 30% complete for ITV66053_2.subsampled.fastq.gz
    Approx 35% complete for ITV66053_2.subsampled.fastq.gz
    Approx 40% complete for ITV66053_2.subsampled.fastq.gz
    Approx 45% complete for ITV66053_2.subsampled.fastq.gz
    Approx 50% complete for ITV66053_2.subsampled.fastq.gz
    Approx 55% complete for ITV66053_2.subsampled.fastq.gz
    Approx 60% complete for ITV66053_2.subsampled.fastq.gz
    Approx 65% complete for ITV66053_2.subsampled.fastq.gz
    Approx 70% complete for ITV66053_2.subsampled.fastq.gz
    Approx 75% complete for ITV66053_2.subsampled.fastq.gz
    Approx 80% complete for ITV66053_2.subsampled.fastq.gz
    Approx 85% complete for ITV66053_2.subsampled.fastq.gz
    Approx 90% complete for ITV66053_2.subsampled.fastq.gz
    Approx 95% complete for ITV66053_2.subsampled.fastq.gz
    
    application/gzip
    Analysis complete for ITV66073_1.subsampled.fastq.gz
    
    Started analysis of ITV66073_1.subsampled.fastq.gz
    Approx 5% complete for ITV66073_1.subsampled.fastq.gz
    Approx 10% complete for ITV66073_1.subsampled.fastq.gz
    Approx 15% complete for ITV66073_1.subsampled.fastq.gz
    Approx 20% complete for ITV66073_1.subsampled.fastq.gz
    Approx 25% complete for ITV66073_1.subsampled.fastq.gz
    Approx 30% complete for ITV66073_1.subsampled.fastq.gz
    Approx 35% complete for ITV66073_1.subsampled.fastq.gz
    Approx 40% complete for ITV66073_1.subsampled.fastq.gz
    Approx 45% complete for ITV66073_1.subsampled.fastq.gz
    Approx 50% complete for ITV66073_1.subsampled.fastq.gz
    Approx 55% complete for ITV66073_1.subsampled.fastq.gz
    Approx 60% complete for ITV66073_1.subsampled.fastq.gz
    Approx 65% complete for ITV66073_1.subsampled.fastq.gz
    Approx 70% complete for ITV66073_1.subsampled.fastq.gz
    Approx 75% complete for ITV66073_1.subsampled.fastq.gz
    Approx 80% complete for ITV66073_1.subsampled.fastq.gz
    Approx 85% complete for ITV66073_1.subsampled.fastq.gz
    Approx 90% complete for ITV66073_1.subsampled.fastq.gz
    Approx 95% complete for ITV66073_1.subsampled.fastq.gz
    
    application/gzip
    Analysis complete for ITV66073_2.subsampled.fastq.gz
    
    Started analysis of ITV66073_2.subsampled.fastq.gz
    Approx 5% complete for ITV66073_2.subsampled.fastq.gz
    Approx 10% complete for ITV66073_2.subsampled.fastq.gz
    Approx 15% complete for ITV66073_2.subsampled.fastq.gz
    Approx 20% complete for ITV66073_2.subsampled.fastq.gz
    Approx 25% complete for ITV66073_2.subsampled.fastq.gz
    Approx 30% complete for ITV66073_2.subsampled.fastq.gz
    Approx 35% complete for ITV66073_2.subsampled.fastq.gz
    Approx 40% complete for ITV66073_2.subsampled.fastq.gz
    Approx 45% complete for ITV66073_2.subsampled.fastq.gz
    Approx 50% complete for ITV66073_2.subsampled.fastq.gz
    Approx 55% complete for ITV66073_2.subsampled.fastq.gz
    Approx 60% complete for ITV66073_2.subsampled.fastq.gz
    Approx 65% complete for ITV66073_2.subsampled.fastq.gz
    Approx 70% complete for ITV66073_2.subsampled.fastq.gz
    Approx 75% complete for ITV66073_2.subsampled.fastq.gz
    Approx 80% complete for ITV66073_2.subsampled.fastq.gz
    Approx 85% complete for ITV66073_2.subsampled.fastq.gz
    Approx 90% complete for ITV66073_2.subsampled.fastq.gz
    Approx 95% complete for ITV66073_2.subsampled.fastq.gz
    



```
from google.colab import files
files.download('ITV66053_1.subsampled_fastqc.html')
files.download('ITV66053_2.subsampled_fastqc.html')
files.download('ITV66073_1.subsampled_fastqc.html')
files.download('ITV66073_2.subsampled_fastqc.html')
```


    <I.core.display.Javascript object>



    <I.core.display.Javascript object>


---

##Arquivos FASTA
O formato de arquivo FASTA é amplamente utilizado em genômica para armazenar sequências biológicas, como DNA, RNA ou proteínas, de forma simples e padronizada. Cada entrada em um arquivo FASTA começa com uma linha de descrição precedida pelo símbolo “>”, seguida por uma ou mais linhas contendo a sequência propriamente dita. Esse formato é essencial em diversas etapas da análise genômica, como o armazenamento de genomas de referência, resultados de montagem, sequências de genes ou proteínas, além de servir como entrada para programas de alinhamento, anotação e comparação de sequências. Sua estrutura leve e compatível com uma ampla variedade de ferramentas bioinformáticas faz do FASTA um dos formatos mais fundamentais e versáteis na área de genômica.


```
>h1.Chr1
CTGGGGGGGCAATAATGGGGGTAAGGGGGCAGTTATGGGGGTCCTGAACCCCCTCCAGGG
GGCAAACCTGCACCTGGGGGGCAATAATGGGGGTAAGGGGGGCAGTTATGGGGGTCCTGA
ACCCCCTCCAGGGGGCAAACCTGCACCTGGGGGGGGCAATAATGGGGGTAAGGGGGCAAT
TCGGGGGGTCCTGAACCCCCCCCGGGGGGCAAACCAGCACCTGGGGGGGCAATAATGGGG
GTAAGGGGGCAGTTATGGGGGTCCTGAACCCCCCCCCCGGGGGGCAAACCAGCACCTGGG
GGGGCAATAATGGGGGTAAGGGGGCAGTTATGGGGGTCCTGAACCCCCCCGGGGGGCAAA
CCAGCACCTGGGGGGCAATAATGGGGGTAAGGGGGCAATTCGGGGGGTCCTGAACCCCCC
CGGGGGGCAAACCAGCACCTGGGGGGGCAATAATGGGGGTAAGGGGGCAGTTATGGGGGT
CCTGAACCCCCCCGGGGGGCAAACCAGCACCTGGGGGGGCAATAACGGGGGTAAGGGGGC
```


```
!grep -c ">" {reference_fasta}
```

    1510



```
!grep ">" {reference_fasta} | wc -l
```

    1510



```
!grep ">" {reference_fasta} | head -n 10
```

    >h1.Chr1
    >h1.Chr2
    >h1.Chr3
    >h1.Chr4
    >h1.Chr5
    >h1.Chr6
    >h1.Chr7
    >h1.Chr8
    >h1.Chr9
    >h1.Chr10


###Bioawk


```
cmd = f"conda run -n genomics_env bioawk -c fastx '{{ print $name, length($seq) }}' {reference_fasta} | head -n 40"
!{cmd}
```

    h1.Chr1	125632909
    h1.Chr2	121304820
    h1.Chr3	101814693
    h1.Chr4	79714593
    h1.Chr5	78812073
    h1.Chr6	68492996
    h1.Chr7	60408821
    h1.Chr8	58337926
    h1.Chr9	54088992
    h1.Chr10	33110309
    h1.Chr11	27612072
    h1.Chr12	22557200
    h1.Chr13	22347957
    h1.Chr14	22200625
    h1.Chr15	20710275
    h1.Chr16	17540783
    h1.Chr17	16785635
    h1.Chr18	15570789
    h1.Chr19	12988303
    h1.Chr20	12533273
    h1.Chr21	8452080
    h1.Chr22	8339360
    h1.Chr23	7869433
    h1.Chr24	7271986
    h1.Chr25	7195970
    h1.Chr26	6754819
    h1.Chr27	6302267
    h1.Chr28	4350216
    h1.Chr29	3708650
    h1.Chr30	495171
    h1.Chr31	3154482
    h1.Chr32	2776152
    h1.Chr33	2951698
    h1.Chr34	1060145
    h1.Chr35	632745
    h1.Chr36	502227
    h1.Chr37	431232
    h1.Chr38	781457
    h1.Chr39	661359
    h1.ChrW	16264068
    Exception ignored on flushing sys.stdout:
    BrokenPipeError: [Errno 32] Broken pipe



```
cmd = f"conda run -n genomics_env bioawk -c fastx '{{ if (length($seq) > 1000) print \">\"$name\"\\n\"$seq }}' {reference_fasta} > {working_dir}/reference/filtradas.fasta"
!{cmd}
```

**Pergunta**: Quantas sequências permaneceram no arquivo filtradas.fasta?
**Dica:** Você pode utilizar uma variável ou escrever o caminho completo do arquvo gerado no passo anterior.


```
##Escreva seu comando aqui
```

###Índices
Os índices são estruturas fundamentais nas análises genômicas, pois permitem acesso rápido e eficiente às informações contidas em arquivos de grandes dimensões, como genomas de referência. Eles são indispensáveis para tarefas como alinhamento de sequências, extração de regiões específicas, visualização genômica e chamadas de variantes. Sem esses índices, seria necessário percorrer o arquivo inteiro a cada operação, o que tornaria as análises inviáveis em termos de tempo e recursos computacionais. Para diferentes ferramentas, há tipos específicos de indexação. Por exemplo, para arquivos FASTA, o samtools faidx gera um índice .fai que permite extrair regiões com rapidez:


```
!conda run -n genomics_env samtools faidx {reference_fasta}
```


```
!cat {reference_fasta}.fai
```

    h1.Chr24	7271985	10	7271985	7271986
    h1.Chr25	7195969	7272006	7195969	7195970



```
!conda run -n genomics_env bwa index {reference_fasta}
#O BWA gera arquivos binários, que não é possível visualizar
```

    [bwa_index] Pack FASTA... 0.17 sec
    [bwa_index] Construct BWT for the packed sequence...
    [bwa_index] 8.79 seconds elapse.
    [bwa_index] Update BWT... 0.08 sec
    [bwa_index] Pack forward-only FASTA... 0.07 sec
    [bwa_index] Construct SA from BWT and Occ... 4.67 sec
    [main] Version: 0.7.19-r1273
    [main] CMD: bwa index drive/MyDrive/Congen_ITV/reference/bWildVid.reduced.fasta
    [main] Real time: 14.287 sec; CPU: 13.791 sec
    


Esses índices são pré-requisitos para muitas ferramentas bioinformáticas e garantem desempenho e reprodutibilidade nas análises de larga escala com dados genômicos.

---

##Arquivos SAM/BAM

Descrição dos formatos SAM e BAM

O SAM (Sequence Alignment/Map) é um formato de texto utilizado para armazenar alinhamentos de leituras de sequenciamento contra um genoma de referência. Já o BAM é a versão binária do SAM — mais compacta e eficiente para leitura e manipulação por programas computacionais.


Estrutura do arquivo SAM

O arquivo SAM é composto por duas seções principais:

1. Cabeçalho (opcional, mas recomendado)
	•	Linhas iniciadas com @, que contêm metadados sobre o alinhamento, como:

* @HD: versão e tipo de ordenação
* @SQ: nome e tamanho de cada cromossomo do genoma de referência
* @RG: informações sobre grupos de leitura
* @PG: informações sobre os programas usados

2. Corpo do arquivo (dados dos alinhamentos)

Cada linha representa uma leitura alinhada e contém 11 campos obrigatórios, seguidos de campos opcionais.

|Campo	|Nome	|Descrição|
|-------|-----|---------|
|1	|QNAME	|Identificador da leitura
|2	|FLAG	|Valor numérico com informações codificadas (ex: se está pareada, mapeada)
|3	|RNAME	|Nome da referência (cromossomo)
|4	|POS	|Posição inicial do alinhamento
|5	|MAPQ	|Qualidade do mapeamento
|6	|CIGAR	|Descreve como a leitura se alinha (ex: casamentos, deleções, inserções)
|7	|RNEXT	|Referência do par (caso pareada)
|8	|PNEXT	|Posição do par
|9	|TLEN	|Tamanho do fragmento
|10	|SEQ	|Sequência da leitura
|11	|QUAL	|Qualidade de cada base (ASCII)

Campos opcionais trazem informações adicionais como número de mismatches, tags de alinhamento, etc.





```
!conda run -n genomics_env samtools view -H {bam_folder}/ITV66053_1.mtDNA.bam
```

    @HD	VN:1.5	SO:unsorted	GO:query
    @SQ	SN:NC_051466.1	LN:16891
    @PG	ID:bwa	PN:bwa	VN:0.7.19-r1273	CL:bwa mem -t 2 /content/drive/MyDrive/Congen_ITV/reference/RheHoff_mtDNA.fasta /content/drive/MyDrive/Congen_ITV/raw_data/ITV66053_1.subsampled.fastq.gz /content/drive/MyDrive/Congen_ITV/raw_data/ITV66053_2.subsampled.fastq.gz
    @PG	ID:samtools	PN:samtools	PP:bwa	VN:1.21	CL:samtools view -Sb -
    @PG	ID:samtools.1	PN:samtools	PP:samtools	VN:1.21	CL:samtools view -H drive/MyDrive/Congen_ITV/bams/ITV66053_1.mtDNA.bam
    



```
!source activate genomics_env && samtools view -h drive/MyDrive/Congen_ITV/bams/ITV66053_1.mtDNA.bam | head
```

    @HD	VN:1.5	SO:unsorted	GO:query
    @SQ	SN:NC_051466.1	LN:16891
    @PG	ID:bwa	PN:bwa	VN:0.7.19-r1273	CL:bwa mem -t 2 /content/drive/MyDrive/Congen_ITV/reference/RheHoff_mtDNA.fasta /content/drive/MyDrive/Congen_ITV/raw_data/ITV66053_1.subsampled.fastq.gz /content/drive/MyDrive/Congen_ITV/raw_data/ITV66053_2.subsampled.fastq.gz
    @PG	ID:samtools	PN:samtools	PP:bwa	VN:1.21	CL:samtools view -Sb -
    @PG	ID:samtools.1	PN:samtools	PP:samtools	VN:1.21	CL:samtools view -h drive/MyDrive/Congen_ITV/bams/ITV66053_1.mtDNA.bam
    VH01766:6:AAC7L7KHV:1:1101:29971:1000	77	*	0	0	*	*	0	0	GATGTTATGTATCTATGATATCTTTTTACTCCCCACTACAGAGCCTTCCTTTTTGCTACAGTCATTTGCAATAAGACCATCAAAAGC	;CCCCCCCC-CCCCCCCCC;CCCC-CCCCCCCCCC;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC;CC-CCCCCCCCCCC	MQ:i:0	AS:i:0	XS:i:0
    VH01766:6:AAC7L7KHV:1:1101:29971:1000	141	*	0	0	*	*	0	0	GCTTTTGATGGTCTTATTGCAAATGACTGTAGCAAAAAGGAAGGCTCTGTAGTGGGGAGTAAAAAGATATCATAGATACATAACATC	;C-CCCCCCCCCCCCCCCCCCCCCCCCC;CCCCCCCC-CCCCCCCCCCCCCC;CCCCCCCCCCCCCCC-CCC;CCCCCCCC-CCCCC	MQ:i:0	AS:i:0	XS:i:0
    VH01766:6:AAC7L7KHV:1:1101:36750:1000	77	*	0	0	*	*	0	0	NTGTAATTAAACACCTTCTAAATGCCTCAGTAAATAAAACAGATTGATGCAATTGTTCTGCAGGTGGACCAGTATAACTGCTCTGCACACAGACTTGGTGATATTTCCTGTGACTGCTTGGGTTTCTGGCCTGATTAGCACACATGGATTG	#;CCCCCCC;;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC	MQ:i:0	AS:i:0	XS:i:0
    VH01766:6:AAC7L7KHV:1:1101:36750:1000	141	*	0	0	*	*	0	0	GTTGTAACACTTTCATCTTGGTACCACAAAAGATGATCAATCCATGTGTGCTAATCAGGCCAGAAACCCAAGCAGTCACAGGAAATATCACCAAGTCTGTGTGCAGAGCAGTTATACTGGTCCACCTGCAGAACAATTGCATCAATCTGTT	CCCCCCCCCCCCCCCCCCCCCCCCCCCCC-CC;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC-CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC;CCCCCCCC;CC	MQ:i:0	AS:i:0	XS:i:0
    VH01766:6:AAC7L7KHV:1:1101:46653:1057	77	*	0	0	*	*	0	0	NTTTTTAGTATTGCACAGAAATGACACGCAAATGCATTGTATTTTTGGTGCTTATTTATTAGTTGCCCTAGATGCAAAGCATTCAGTTATAAAAGAAAAAAAAAAAGAAACCATGTGCTTAACTTAATCCCAAGAGTTTTCCAATTGCCCT	#;CCCCCCCCCCCCCC;CCCCCCCCCCCCCC;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC;CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC;CCCCCCCCCCCCCCCCCCCCCCCCCC-CCCCCCCCCCCCCCCC	MQ:i:0	AS:i:0	XS:i:0


Manipulação de arquivos SAM/BAM com SAMtools

1. Converter SAM para BAM

```
samtools view -Sb input.sam > output.bam
```

2. Ordenar BAM por posição genômica

```
samtools sort -o sorted.bam input.bam
```

3. Indexar o BAM

```
samtools index sorted.bam
```

Cria um arquivo .bai, necessário para acesso rápido e visualização (ex: IGV).


4. Gerar estatísticas de alinhamento

```
samtools flagstat sorted.bam
samtools stats sorted.bam > alignment_stats.txt
```

5. Filtrar leituras específicas
	•	Apenas mapeadas:

```
samtools view -b -F 4 input.bam > mapped_reads.bam
```

  •	Leituras com qualidade de mapeamento ≥ 30:


```
samtools view -b -q 30 input.bam > high_qual.bam
```


6. Extrair leituras de uma região específica

```
samtools view -b input.bam chr1:10000-20000 > region.bam
```


7. Converter BAM para FASTQ

```
samtools fastq input.bam > output.fastq
```

8. Remover duplicatas (ex: PCR duplicates)

```
samtools markdup -r sorted.bam dedup.bam
```

Resumo

* SAM: formato de texto, útil para inspeção manual.

* BAM: versão compacta e eficiente para grandes análises.

* SAMtools: principal ferramenta para conversão, ordenação, filtragem, estatísticas e extração de dados de arquivos SAM/BAM.


Esses arquivos são centrais em pipelines genômicos, conectando os resultados do mapeamento (ex: via BWA) com etapas posteriores como chamada de variantes, visualização genômica e análises de cobertura.

---

##Arquivos VCF

O VCF (Variant Call Format) é um formato de arquivo amplamente utilizado em bioinformática para armazenar variantes genéticas, como SNPs, inserções, deleções e outras mutações detectadas em dados de sequenciamento. Cada linha do VCF representa uma variante, indicando informações como a posição no genoma, os alelos de referência e variantes, a qualidade da chamada, anotações funcionais e os genótipos observados em cada amostra. O cabeçalho do arquivo (linhas iniciadas com ##) descreve o conteúdo das colunas e os parâmetros usados na análise. O VCF é um formato essencial para estudos de diversidade genética, genômica populacional e medicina personalizada.


```
%cd /content/drive/MyDrive/Congen_ITV/vcf
```

    /content/drive/MyDrive/Congen_ITV/vcf


A seção de cabeçalho de um arquivo VCF (Variant Call Format) contém informações essenciais sobre o conteúdo e a estrutura dos dados que serão apresentados. Ela é composta por linhas iniciadas com o símbolo ##, seguidas por uma última linha iniciada por # que define os nomes das colunas da tabela de variantes.

⸻

1. Linhas de metadados (##)

Essas linhas fornecem informações descritivas sobre:
* O formato do arquivo,
* A versão do VCF (##fileformat=VCFv4.2),
* O histórico da análise (##source=),
* Referência genômica usada (##reference=),
* Campos da coluna INFO (como ##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Depth">),
* Campos do FORMAT (como genótipos, profundidade por amostra etc.),
* Filtros aplicados (##FILTER=<ID=PASS,Description="All filters passed">).

Esses metadados são fundamentais para interpretar corretamente os dados nas colunas INFO e FORMAT.

⸻

2. Linha de cabeçalho principal (#CHROM…)

A última linha do cabeçalho começa com #CHROM e define as colunas principais do VCF:

|Coluna|	Descrição|
|------|-----------|
|CHROM|	Cromossomo onde a variante foi detectada
|POS	|Posição da variante no genoma
|ID	|Identificador da variante (ex: rsID), ou . se ausente
|REF	|Alelo de referência
|ALT	|Alelo(s) alternativo(s)
|QUAL	|Qualidade da chamada da variante
|FILTER	|Filtro(s) aplicados à variante (ex: PASS ou motivo da falha)
|INFO	|Informações adicionais (ex: profundidade, frequência, impacto)
|FORMAT	|Formato dos dados genotípicos (ex: GT:DP:AD)
|sample1 …	|Colunas para cada amostra, com informações conforme FORMAT


⸻

Essa estrutura torna o VCF um formato altamente flexível e padronizado para armazenar e compartilhar dados de variantes genéticas.


```
!zcat {vcf_file} | head -n 10
```

    ##fileformat=VCFv4.2
    ##FILTER=<ID=PASS,Description="All filters passed">
    ##ALT=<ID=NON_REF,Description="Represents any possible alternative allele not already represented at this location by REF and ALT">
    ##FILTER=<ID=FS60,Description="FS > 60.0">
    ##FILTER=<ID=LowQual,Description="Low quality">
    ##FILTER=<ID=MQ40,Description="MQ < 40.0">
    ##FILTER=<ID=MQRankSum-12.5,Description="MQRankSum < -12.5">
    ##FILTER=<ID=QD2,Description="QD < 2.0">
    ##FILTER=<ID=QUAL30,Description="QUAL < 30.0">
    ##FILTER=<ID=ReadPosRankSum-8,Description="ReadPosRankSum < -8.0">


A seção dos genótipos em um arquivo VCF começa após a linha de cabeçalho principal (a que inicia com #CHROM) e contém as informações de variantes para cada amostra sequenciada. Essa seção é organizada em colunas, onde cada linha representa uma variante genética (por exemplo, um SNP ou uma indel), e cada coluna adicional contém os dados genotípicos de uma amostra.

⸻

📊 Estrutura das colunas a partir da nona posição

Coluna	Descrição
CHROM	Cromossomo onde a variante está localizada
POS	Posição genômica
ID	Identificador da variante (ex: rsID ou . se não anotado)
REF	Alelo de referência
ALT	Alelo(s) alternativo(s)
QUAL	Qualidade da chamada
FILTER	Resultado dos filtros aplicados
INFO	Informações adicionais sobre a variante
FORMAT	Especifica os campos de dados que serão apresentados nas amostras seguintes
sample1…	Valores genotípicos para cada amostra, no formato definido pela coluna FORMAT


⸻

🧬 Coluna FORMAT e os campos genotípicos

A coluna FORMAT define quais informações estão presentes nos campos das amostras. Por exemplo:

```
FORMAT      GT:AD:DP:GQ:PL
sample1     0/1:12,8:20:99:120,0,200
```

Exemplos de campos comuns:
	•	GT (Genotype): genótipo da amostra, como 0/0, 0/1, 1/1
	•	DP (Depth): profundidade de cobertura (número de leituras)
	•	GQ (Genotype Quality): qualidade do genótipo
	•	AD (Allele Depth): número de leituras que suportam os alelos REF e ALT
	•	PL (Phred-scaled Likelihoods): probabilidades do genótipo

⸻

📌 Exemplo real:

```
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT          sample1         sample2
chr1    10583   .       G       A       29.7    PASS    .       GT:DP:AD        0/1:23:11,12    0/0:18:18,0
```

Nesse exemplo:
	•	sample1 é heterozigota (0/1), com 23 leituras (11 REF e 12 ALT).
	•	sample2 é homozigota referência (0/0), com 18 leituras REF e nenhuma ALT.



```
!zgrep -v "^#" {vcf_file} | head -n 10
```

    gzip: willisornis.gz: No such file or directory
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	307	.	A	G	222.78	PASS	AC=2;AF=0.018;AN=40;DP=391;ExcessHet=0;FS=0;InbreedingCoeff=0.3395;MLEAC=35;MLEAF=0.155;MQ=45.23;QD=33.11;SOR=0.693	GT:AD:DP:GQ:PL	0/0:5,0:5:0:0,0,0	1/1:0,1:1:3:39,3,0	0/0:6,0:6:0:0,0,0	0/0:3,0:3:0:0,0,0	0/0:0,0:0:0:0,0,0	0/0:6,0:6:0:0,0,0	0/0:3,0:3:0:0,0,0	0/0:7,0:7:0:0,0,0	0/0:0,0:0:0:0,0,0	0/0:4,0:4:0:0,0,0	0/0:11,0:11:0:0,0,0	0/0:3,0:3:0:0,0,0	0/0:4,0:4:0:0,0,0	0/0:2,0:2:0:0,0,0	0/0:0,0:0:0:0,0,0	0/0:0,0:0:0:0,0,0	0/0:0,0:0:0:0,0,0	0/0:1,0:1:0:0,0,0	0/0:7,0:7:0:0,0,0	0/0:1,0:1:0:0,0,0
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	368	.	A	G	97.35	PASS	AC=1;AF=0.004425;AN=40;BaseQRankSum=0;DP=288;ExcessHet=0;FS=0;InbreedingCoeff=0.3064;MLEAC=8;MLEAF=0.035;MQ=41.82;MQRankSum=-1.383;QD=24.34;ReadPosRankSum=1.38;SOR=0.693	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:4,0:4:0:.:.:0,0,0:.	0/0:5,0:5:0:.:.:0,0,0:.	0/0:5,0:5:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:3,0:3:0:.:.:0,0,0:.	0/0:7,0:7:6:.:.:0,6,90:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:13,0:13:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:11,0:11:0:.:.:0,0,0:.	0/0:2,0:2:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:1,0:1:3:.:.:0,3,16:.	0|1:2,2:4:78:0|1:362_G_T:78,0,78:362	0/0:1,0:1:3:.:.:0,3,39:.	0/0:5,0:5:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	380	.	G	T	75.88	PASS	AC=1;AF=0.004425;AN=40;BaseQRankSum=0;DP=197;ExcessHet=0;FS=0;InbreedingCoeff=0.3135;MLEAC=4;MLEAF=0.018;MQ=41.82;MQRankSum=1.38;QD=18.97;ReadPosRankSum=-1.383;SOR=1.022	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:0,0:0:0:.:.:0,0,0:.	0/0:5,0:5:6:.:.:0,6,90:.	0/0:1,0:1:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:2,0:2:6:.:.:0,6,41:.	0/0:7,0:7:3:.:.:0,3,45:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:14,0:14:1:.:.:0,1,410:.	0/0:2,0:2:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:1,0:1:3:.:.:0,3,16:.	0|1:2,2:4:78:0|1:361_G_A:78,0,78:361	0/0:1,0:1:3:.:.:0,3,39:.	0/0:1,0:1:3:.:.:0,3,39:.	0/0:0,0:0:0:.:.:0,0,0:.
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	389	.	G	A	86.56	PASS	AC=1;AF=0.004425;AN=40;BaseQRankSum=0;DP=123;ExcessHet=0;FS=0;InbreedingCoeff=0.3059;MLEAC=6;MLEAF=0.027;MQ=41.82;MQRankSum=-1.383;QD=21.64;ReadPosRankSum=1.38;SOR=1.022	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:1,0:1:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:2,0:2:3:.:.:0,3,45:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:3,0:3:0:.:.:0,0,0:.	0/0:2,0:2:3:.:.:0,3,45:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0|1:2,2:4:78:0|1:362_G_T:78,0,78:362	0/0:1,0:1:3:.:.:0,3,39:.	0/0:1,0:1:3:.:.:0,3,39:.	0/0:0,0:0:0:.:.:0,0,0:.
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	390	.	A	G	101.23	PASS	AC=1;AF=0.004425;AN=40;BaseQRankSum=0;DP=122;ExcessHet=0;FS=0;InbreedingCoeff=0.2912;MLEAC=9;MLEAF=0.04;MQ=41.82;MQRankSum=1.38;QD=25.31;ReadPosRankSum=-1.383;SOR=1.022	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:1,0:1:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:2,0:2:3:.:.:0,3,45:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:3,0:3:0:.:.:0,0,0:.	0/0:2,0:2:3:.:.:0,3,45:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0|1:2,2:4:78:0|1:361_G_A:78,0,78:361	0/0:1,0:1:0:.:.:0,0,0:.	0/0:1,0:1:3:.:.:0,3,39:.	0/0:0,0:0:0:.:.:0,0,0:.
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	399	.	G	A	259.5	PASS	AC=2;AF=0.018;AN=40;DP=72;ExcessHet=0;FS=0;InbreedingCoeff=0.3338;MLEAC=25;MLEAF=0.111;MQ=41.82;QD=31.25;SOR=0.693	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:1,0:1:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:3,0:3:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	1|1:0,2:3:6:1|1:362_G_T:90,6,0:362	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	409	.	C	T	147.09	PASS	AC=0;AF=0.00885;AN=40;DP=64;ExcessHet=0;FS=0;InbreedingCoeff=0.3354;MLEAC=17;MLEAF=0.075;MQ=60;QD=31.12;SOR=0.693	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:1,0:1:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:3,0:3:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	1181	.	C	T	253.04	PASS	AC=0;AF=0.018;AN=40;DP=23;ExcessHet=0;FS=0;InbreedingCoeff=0.3379;MLEAC=32;MLEAF=0.142;MQ=46;QD=25.83;SOR=2.833	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:10,0:10:3:.:.:0,3,45:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	1187	.	A	G	319.55	PASS	AC=0;AF=0.018;AN=40;DP=12;ExcessHet=0;FS=0;InbreedingCoeff=0.3356;MLEAC=44;MLEAF=0.195;MQ=46;QD=29.88;SOR=2.833	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz:h1.Chr24	1234	.	T	C	128.88	PASS	AC=0;AF=0.00885;AN=40;DP=8;ExcessHet=0;FS=0;InbreedingCoeff=0.3333;MLEAC=22;MLEAF=0.097;MQ=50.22;QD=28.29;SOR=2.303	GT:AD:DP:GQ:PGT:PID:PL:PS	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:0,0:0:0:.:.:0,0,0:.	0/0:1,0:1:0:.:.:0,0,0:.	0/0:1,0:1:3:.:.:0,3,31:.	0/0:0,0:0:0:.:.:0,0,0:.


##Arquivos BED

O formato BED (Browser Extensible Data) é amplamente utilizado em bioinformática para representar intervalos genômicos, como genes, exons, regiões repetitivas, ou regiões de interesse para análises. É um formato simples, baseado em texto, onde cada linha representa uma região no genoma, geralmente com pelo menos três colunas obrigatórias:


Estrutura básica do formato BED:

|Coluna	|Nome	|Descrição|
|-------|------|--------|
|1|	chrom	|Nome do cromossomo (ex: chr1, chrX)
|2|	start	|Posição inicial (base 0, inclusive)
|3|	end	|Posição final (base 0, exclusiva)

```
Exemplo de linha BED:

chr1    999    1500
```

Isso representa a região do cromossomo 1 entre as posições 1000 e 1500 (base 0-indexed)

Campos opcionais (até 12 no total):

Além dos 3 obrigatórios, o BED pode conter campos adicionais como nome da região, score, strand (+/-), entre outros.

Exemplos de manipulação com bedtools:

bedtools é um conjunto de ferramentas poderoso para operar com arquivos BED (e outros formatos relacionados a regiões genômicas).


1. Interseção entre regiões (overlaps)

```
bedtools intersect -a genes.bed -b snps.bed > genes_with_snps.bed
```

Encontra genes que possuem sobreposição com SNPs.

⸻

2. Complemento de regiões

```
bedtools complement -i exons.bed -g genome_file.txt > non_coding.bed
```

Obtém regiões do genoma que não estão cobertas pelos exons.

⸻

3. Cobertura de regiões

```
bedtools coverage -a regions.bed -b reads.bam > coverage.txt
```

Calcula quantas leituras (de um BAM) cobrem cada região BED.

⸻

4. Subtração de regiões

```
bedtools subtract -a promoters.bed -b repeats.bed > clean_promoters.bed
```

Remove trechos de repeats.bed que sobrepõem promoters.bed.

⸻

5. Ordenar um arquivo BED (pré-requisito para algumas operações)

```
sort -k1,1 -k2,2n unsorted.bed > sorted.bed
```

⸻

6. Extração de sequências com bedtools getfasta

```
bedtools getfasta -fi reference.fasta -bed regions.bed -fo sequences.fasta
```

Extrai as sequências das regiões BED com base no genoma de referência.

⸻

7. Conversão de GFF/GTF para BED

```
bedtools gff2bed < annotation.gff > annotation.bed
```

⸻

O formato BED, por sua simplicidade e versatilidade, é essencial em pipelines genômicos para delimitar, comparar, e manipular regiões genômicas. Ferramentas como bedtools tornam esse processo rápido, eficiente e altamente flexível.

---

##Arquivos GFF

O formato GFF (General Feature Format) é amplamente utilizado em bioinformática para descrever anotações funcionais em genomas, como genes, exons, promotores, regiões regulatórias, entre outros elementos genômicos. Ele permite representar com precisão a estrutura de genes e suas subunidades ao longo de um genoma de referência.


###Estrutura do formato GFF (versão 3 – GFF3):

O GFF3 possui 9 colunas obrigatórias, separadas por tabulações:

|Coluna|	Nome	|Descrição|
|------|--------|---------|
|1	|seqid	|Nome do cromossomo ou scaffold|
|2	|source	|Origem da anotação (ex: Ensembl, Augustus, manual)|
|3	|type	|Tipo da feição (ex: gene, mRNA, exon, CDS)|
|4	|start	|Posição inicial da feição (base 1)|
|5	|end	|Posição final da feição (base 1)|
|6	|score	|Valor numérico de qualidade (ou . se não disponível)|
|7	|strand	|Fita de DNA: + ou -|
|8	|phase	|Leitura de quadros (0, 1, 2) para feições tipo CDS|
|9	|attributes	|Informações adicionais no formato tag=value, separadas por ;|


Exemplo de linha GFF3:

```
chr1	Ensembl	gene	1000	5000	.	+	.	ID=gene00001;Name=ATP_synthase
```

Esta linha descreve um gene na fita positiva do cromossomo 1, entre as posições 1000 e 5000.



###Arquivo de anotação


```
gff_file = f'{working_dir}gff/gff_chr24_chr25.gff'
```

1. Conversão para BED


```
cmd = f"""cut -f1,4,5,9 "{gff_file}" | awk '{{print $1"\t"($2-1)"\t"$3"\t"$4}}' | head -n 10"""
!{cmd}
```

    h1.Chr24	94365	94619	ID=gene-egapx-itv_003536;Name=egapx-itv_003536;gbkey=Gene;gene_biotype=lncRNA;locus_tag=egapx-itv_003536
    h1.Chr24	94365	94619	ID=rna-egapx-itv_003536;Parent=gene-egapx-itv_003536;gbkey=ncRNA;locus_tag=egapx-itv_003536;product=uncharacterized
    h1.Chr24	94524	94619	ID=exon-egapx-itv_003536-1;Parent=rna-egapx-itv_003536;gbkey=ncRNA;locus_tag=egapx-itv_003536;product=uncharacterized
    h1.Chr24	94365	94438	ID=exon-egapx-itv_003536-2;Parent=rna-egapx-itv_003536;gbkey=ncRNA;locus_tag=egapx-itv_003536;product=uncharacterized
    h1.Chr24	98662	99067	ID=gene-egapx-itv_003412;Name=egapx-itv_003412;gbkey=Gene;gene_biotype=lncRNA;locus_tag=egapx-itv_003412
    h1.Chr24	98662	99067	ID=rna-egapx-itv_003412;Parent=gene-egapx-itv_003412;gbkey=ncRNA;locus_tag=egapx-itv_003412;product=uncharacterized
    h1.Chr24	98929	99067	ID=exon-egapx-itv_003412-1;Parent=rna-egapx-itv_003412;gbkey=ncRNA;locus_tag=egapx-itv_003412;product=uncharacterized
    h1.Chr24	98662	98717	ID=exon-egapx-itv_003412-2;Parent=rna-egapx-itv_003412;gbkey=ncRNA;locus_tag=egapx-itv_003412;product=uncharacterized
    h1.Chr24	106137	121877	ID=gene-egapx-itv_003331;Dbxref=NCBIOrtholog:55633;Name=TBC1D22B;description=TBC1
    h1.Chr24	106137	121877	ID=rna-gnl|WGS:ZZZZ|egapx-itv_003331-R1;Parent=gene-egapx-itv_003331;Dbxref=NCBIOrtholog:55633;gbkey=mRNA;gene=TBC1D22B;locus_tag=egapx-itv_003331;model_evidence=Supporting


Você pode gerar um arquivo bed simplificado apenas com o cromossomo e intervalo. **Como você faria isso?***


```
##Sua resposta aqui:
```

2. Extração de elementos específicos


```
cmd = f"""grep -P "\texon\t" "{gff_file}" | cut -f1,4,5 > "{working_dir}gff/gff_exons.bed" """
!{cmd}
```

3. Extração de sequenências com base na anotação


```
reference_fasta = f"{working_dir}reference/bWildVid.reduced.fasta"
exons_bed = f"{working_dir}gff/gff_exons.bed"
```


```
!conda run -n genomics_env bedtools getfasta -fi {reference_fasta} -bed {exons_bed} -fo {working_dir}gff/exon_sequences.fasta
```


```
exons_fasta = f"{working_dir}gff/exon_sequences.fasta"

!grep -c ">" {exons_fasta}
```

    9806


O formato GFF é crucial para análises funcionais e estruturais de genomas, permitindo integrar informações de anotação em pipelines de expressão gênica, previsão de variantes, e muito mais. Embora mais detalhado que o BED, ele é altamente padronizado e compatível com diversas ferramentas de bioinformática.
