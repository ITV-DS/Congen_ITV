---
layout: single
title: "Estatísticas Sumárias de Diversidade (Heterozigosidade, π, dXY, Parentesco)"
permalink: /pages/estatisticas_diversidade/
---

#Estatísticas sumárias
As **estatísticas sumárias de diversidade genética** são ferramentas fundamentais na **genômica de populações** e na **genômica da conservação**, pois permitem quantificar e interpretar os padrões de variação genética dentro e entre populações. Essas estatísticas resumem grandes volumes de dados genômicos em métricas informativas, como a **heterozigosidade**, a **divergência genética (FST)**, a **proporção de variantes raras**, e o número de **polimorfismos segregantes**. Em genômica da conservação, essas métricas são especialmente importantes para avaliar o estado genético de populações ameaçadas, identificar sinais de perda de variabilidade, gargalos populacionais e endogamia, além de subsidiar decisões sobre manejo genético, definição de unidades evolutivamente significativas e priorização de áreas para conservação. A incorporação dessas estatísticas em estudos com dados de genoma inteiro permite uma abordagem mais precisa e robusta para entender os processos evolutivos e ecológicos que moldam a diversidade genética natural.

##Preparação


```
from google.colab import drive
drive.mount('/content/drive/')

%cd /content/drive/MyDrive/Congen_ITV/vcf
```

    Mounted at /content/drive/



```
%ls
```

    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz
    willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz.csi



```
# Install Miniconda (1–2 min)
!wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
!bash Miniconda3-latest-Linux-x86_64.sh -bfp /usr/local

# Add conda to the environment
import sys
sys.path.append('/usr/local/lib/3.7/site-packages')
```

    --2025-05-29 13:29:33--  https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    Resolving repo.anaconda.com (repo.anaconda.com)... 104.16.191.158, 104.16.32.241, 2606:4700::6810:bf9e, ...
    Connecting to repo.anaconda.com (repo.anaconda.com)|104.16.191.158|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 155472915 (148M) [application/octet-stream]
    Saving to: ‘Miniconda3-latest-Linux-x86_64.sh’
    
    Miniconda3-latest-L 100%[===================>] 148.27M  28.4MB/s    in 4.9s    
    
    2025-05-29 13:29:38 (30.1 MB/s) - ‘Miniconda3-latest-Linux-x86_64.sh’ saved [155472915/155472915]
    
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



```
!conda create -n genomics_env -c bioconda -c conda-forge -y vcftools plink bcftools
```

    Channels:
     - bioconda
     - conda-forge
     - defaults
    Platform: linux-64
    Collecting package metadata (repodata.json): - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ done
    Solving environment: / done
    
    
    ==> WARNING: A newer version of conda exists. <==
        current version: 25.3.1
        latest version: 25.5.0
    
    Please update conda by running
    
        $ conda update -n base -c defaults conda
    
    
    
    ## Package Plan ##
    
      environment location: /usr/local/envs/genomics_env
    
      added / updated specs:
        - bcftools
        - plink
        - vcftools
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        bcftools-1.21              |       h3a4d415_1         987 KB  bioconda
        bzip2-1.0.8                |       h4bc722e_7         247 KB  conda-forge
        c-ares-1.34.5              |       hb9d3cd8_0         202 KB  conda-forge
        ca-certificates-2025.4.26  |       hbd8a1cb_0         149 KB  conda-forge
        gsl-2.7                    |       he838d99_0         3.2 MB  conda-forge
        htslib-1.21                |       h566b1c6_1         3.0 MB  bioconda
        keyutils-1.6.1             |       h166bdaf_0         115 KB  conda-forge
        krb5-1.21.3                |       h659f571_0         1.3 MB  conda-forge
        libblas-3.9.0              |31_h59b9bed_openblas          16 KB  conda-forge
        libcblas-3.9.0             |31_he106b2a_openblas          16 KB  conda-forge
        libcurl-8.14.0             |       h332b0f4_0         438 KB  conda-forge
        libdeflate-1.24            |       h86f0d12_0          71 KB  conda-forge
        libedit-3.1.20250104       | pl5321h7949ede_0         132 KB  conda-forge
        libev-4.33                 |       hd590300_2         110 KB  conda-forge
        liblzma-5.8.1              |       hb9d3cd8_1         110 KB  conda-forge
        libnghttp2-1.64.0          |       h161d5f1_0         632 KB  conda-forge
        libssh2-1.11.1             |       hcf80075_0         298 KB  conda-forge
        libstdcxx-ng-15.1.0        |       h4852527_2          34 KB  conda-forge
        ncurses-6.5                |       h2d0b736_3         871 KB  conda-forge
        openssl-3.5.0              |       h7b32b05_1         3.0 MB  conda-forge
        zstd-1.5.7                 |       hb8e6e7a_2         554 KB  conda-forge
        ------------------------------------------------------------
                                               Total:        15.4 MB
    
    The following NEW packages will be INSTALLED:
    
      _libgcc_mutex      conda-forge/linux-64::_libgcc_mutex-0.1-conda_forge 
      _openmp_mutex      conda-forge/linux-64::_openmp_mutex-4.5-2_gnu 
      bcftools           bioconda/linux-64::bcftools-1.21-h3a4d415_1 
      bzip2              conda-forge/linux-64::bzip2-1.0.8-h4bc722e_7 
      c-ares             conda-forge/linux-64::c-ares-1.34.5-hb9d3cd8_0 
      ca-certificates    conda-forge/noarch::ca-certificates-2025.4.26-hbd8a1cb_0 
      gsl                conda-forge/linux-64::gsl-2.7-he838d99_0 
      htslib             bioconda/linux-64::htslib-1.21-h566b1c6_1 
      keyutils           conda-forge/linux-64::keyutils-1.6.1-h166bdaf_0 
      krb5               conda-forge/linux-64::krb5-1.21.3-h659f571_0 
      libblas            conda-forge/linux-64::libblas-3.9.0-31_h59b9bed_openblas 
      libcblas           conda-forge/linux-64::libcblas-3.9.0-31_he106b2a_openblas 
      libcurl            conda-forge/linux-64::libcurl-8.14.0-h332b0f4_0 
      libdeflate         conda-forge/linux-64::libdeflate-1.24-h86f0d12_0 
      libedit            conda-forge/linux-64::libedit-3.1.20250104-pl5321h7949ede_0 
      libev              conda-forge/linux-64::libev-4.33-hd590300_2 
      libgcc             conda-forge/linux-64::libgcc-15.1.0-h767d61c_2 
      libgcc-ng          conda-forge/linux-64::libgcc-ng-15.1.0-h69a702a_2 
      libgfortran        conda-forge/linux-64::libgfortran-15.1.0-h69a702a_2 
      libgfortran5       conda-forge/linux-64::libgfortran5-15.1.0-hcea5267_2 
      libgomp            conda-forge/linux-64::libgomp-15.1.0-h767d61c_2 
      liblzma            conda-forge/linux-64::liblzma-5.8.1-hb9d3cd8_1 
      libnghttp2         conda-forge/linux-64::libnghttp2-1.64.0-h161d5f1_0 
      libopenblas        conda-forge/linux-64::libopenblas-0.3.29-pthreads_h94d23a6_0 
      libssh2            conda-forge/linux-64::libssh2-1.11.1-hcf80075_0 
      libstdcxx          conda-forge/linux-64::libstdcxx-15.1.0-h8f9b012_2 
      libstdcxx-ng       conda-forge/linux-64::libstdcxx-ng-15.1.0-h4852527_2 
      libxcrypt          conda-forge/linux-64::libxcrypt-4.4.36-hd590300_1 
      libzlib            conda-forge/linux-64::libzlib-1.3.1-hb9d3cd8_2 
      ncurses            conda-forge/linux-64::ncurses-6.5-h2d0b736_3 
      openssl            conda-forge/linux-64::openssl-3.5.0-h7b32b05_1 
      perl               conda-forge/linux-64::perl-5.32.1-7_hd590300_perl5 
      plink              bioconda/linux-64::plink-1.90b7.7-h18e278d_1 
      vcftools           bioconda/linux-64::vcftools-0.1.17-pl5321h077b44d_0 
      zstd               conda-forge/linux-64::zstd-1.5.7-hb8e6e7a_2 
    
    
    
    Downloading and Extracting Packages:
    gsl-2.7              | 3.2 MB    | :   0% 0/1 [00:00<?, ?it/s]
    htslib-1.21          | 3.0 MB    | :   0% 0/1 [00:00<?, ?it/s][A
    
    openssl-3.5.0        | 3.0 MB    | :   0% 0/1 [00:00<?, ?it/s][A[A
    
    
    krb5-1.21.3          | 1.3 MB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A
    
    
    
    bcftools-1.21        | 987 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A
    
    
    
    
    ncurses-6.5          | 871 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A
    
    
    
    
    
    
    
    libcurl-8.14.0       | 438 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    libedit-3.1.20250104 | 132 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    keyutils-1.6.1       | 115 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    liblzma-5.8.1        | 110 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libev-4.33           | 110 KB    | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libdeflate-1.24      | 71 KB     | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libstdcxx-ng-15.1.0  | 34 KB     | :   0% 0/1 [00:00<?, ?it/s][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    openssl-3.5.0        | 3.0 MB    | :   1% 0.005255644910358278/1 [00:00<00:25, 25.28s/it][A[A
    
    
    
    gsl-2.7              | 3.2 MB    | :   0% 0.00485247257230507/1 [00:00<00:36, 36.97s/it]
    htslib-1.21          | 3.0 MB    | :   1% 0.005149561702649271/1 [00:00<00:35, 35.37s/it][A
    
    
    krb5-1.21.3          | 1.3 MB    | :   1% 0.011958923317345767/1 [00:00<00:15, 16.06s/it][A[A[A
    
    openssl-3.5.0        | 3.0 MB    | :  39% 0.3889177233665126/1 [00:00<00:00,  2.01it/s]  [A[A
    
    
    
    bcftools-1.21        | 987 KB    | :  83% 0.8267019146292236/1 [00:00<00:00,  4.01it/s] [A[A[A[A
    
    
    
    gsl-2.7              | 3.2 MB    | :  32% 0.3202631897721346/1 [00:00<00:00,  1.41it/s] 
    htslib-1.21          | 3.0 MB    | :  21% 0.20598246810597085/1 [00:00<00:00,  1.12s/it] [A
    
    
    krb5-1.21.3          | 1.3 MB    | :  61% 0.6099050891846342/1 [00:00<00:00,  2.58it/s]  [A[A[A
    
    openssl-3.5.0        | 3.0 MB    | : 100% 1.0/1 [00:00<00:00,  3.53it/s]               [A[A
    
    
    krb5-1.21.3          | 1.3 MB    | : 100% 1.0/1 [00:00<00:00,  2.58it/s]               [A[A[A
    
    
    
    
    ncurses-6.5          | 871 KB    | :   2% 0.018375108367605347/1 [00:00<00:18, 18.36s/it][A[A[A[A[A
    
    openssl-3.5.0        | 3.0 MB    | : 100% 1.0/1 [00:00<00:00,  3.53it/s][A[A
    
    
    
    
    ncurses-6.5          | 871 KB    | : 100% 1.0/1 [00:00<00:00, 18.36s/it]                 [A[A[A[A[A
    
    
    
    
    
    
    gsl-2.7              | 3.2 MB    | : 100% 1.0/1 [00:00<00:00,  2.63it/s]
    htslib-1.21          | 3.0 MB    | : 100% 1.0/1 [00:00<00:00,  2.73it/s]                [A
    htslib-1.21          | 3.0 MB    | : 100% 1.0/1 [00:00<00:00,  2.73it/s][A
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | :   3% 0.02529960670106038/1 [00:00<00:17, 18.13s/it][A[A[A[A[A[A
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    | : 100% 1.0/1 [00:00<00:00, 15.34s/it]                [A[A[A[A[A[A[A
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | : 100% 1.0/1 [00:00<00:00, 18.13s/it]                [A[A[A[A[A[A
    
    
    
    
    
    
    
    libcurl-8.14.0       | 438 KB    | :   4% 0.03655119487426603/1 [00:00<00:12, 13.43s/it][A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    libcurl-8.14.0       | 438 KB    | : 100% 1.0/1 [00:00<00:00, 13.43s/it]                [A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | :   5% 0.05375504445683914/1 [00:00<00:09, 10.05s/it][A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | :   6% 0.06481448515129577/1 [00:00<00:07,  8.40s/it][A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | : 100% 1.0/1 [00:00<00:00, 10.05s/it]                [A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | :   8% 0.07919413777769185/1 [00:00<00:06,  7.02s/it][A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | : 100% 1.0/1 [00:00<00:00,  7.02s/it]                [A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | : 100% 1.0/1 [00:00<00:00,  8.40s/it]                [A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | :  11% 0.10758915965669182/1 [00:00<00:04,  5.29s/it][A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | : 100% 1.0/1 [00:00<00:00,  5.29s/it]                [A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    libedit-3.1.20250104 | 132 KB    | :  12% 0.12165493480649855/1 [00:00<00:04,  4.74s/it][A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    libedit-3.1.20250104 | 132 KB    | : 100% 1.0/1 [00:00<00:00,  4.74s/it]                [A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    keyutils-1.6.1       | 115 KB    | :  14% 0.13904660063989951/1 [00:00<00:03,  4.46s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    keyutils-1.6.1       | 115 KB    | : 100% 1.0/1 [00:00<00:00,  4.46s/it]                [A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    liblzma-5.8.1        | 110 KB    | :  15% 0.1451903052860118/1 [00:00<00:03,  4.32s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    liblzma-5.8.1        | 110 KB    | : 100% 1.0/1 [00:00<00:00,  4.32s/it]               [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libev-4.33           | 110 KB    | :  15% 0.14529202064452051/1 [00:00<00:03,  4.41s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libev-4.33           | 110 KB    | : 100% 1.0/1 [00:00<00:00,  4.41s/it]                [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libdeflate-1.24      | 71 KB     | :  23% 0.2257588910476348/1 [00:00<00:02,  2.94s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libdeflate-1.24      | 71 KB     | : 100% 1.0/1 [00:00<00:00,  2.94s/it]               [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libstdcxx-ng-15.1.0  | 34 KB     | :  47% 0.47288365515051806/1 [00:00<00:00,  1.49s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libstdcxx-ng-15.1.0  | 34 KB     | : 100% 1.0/1 [00:00<00:00,  1.49s/it]                [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    openssl-3.5.0        | 3.0 MB    | : 100% 1.0/1 [00:00<00:00,  3.53it/s][A[A
    
    
    krb5-1.21.3          | 1.3 MB    | : 100% 1.0/1 [00:01<00:00,  1.20s/it][A[A[A
    
    
    krb5-1.21.3          | 1.3 MB    | : 100% 1.0/1 [00:01<00:00,  1.20s/it][A[A[A
    
    
    
    bcftools-1.21        | 987 KB    | : 100% 1.0/1 [00:01<00:00,  4.01it/s][A[A[A[A
    
    
    
    
    ncurses-6.5          | 871 KB    | : 100% 1.0/1 [00:03<00:00,  3.32s/it][A[A[A[A[A
    
    
    
    
    gsl-2.7              | 3.2 MB    | : 100% 1.0/1 [00:05<00:00,  2.63it/s]
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    | : 100% 1.0/1 [00:05<00:00,  4.96s/it][A[A[A[A[A[A[A
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    | : 100% 1.0/1 [00:05<00:00,  4.96s/it][A[A[A[A[A[A[A
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | : 100% 1.0/1 [00:05<00:00,  4.98s/it][A[A[A[A[A[A
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | : 100% 1.0/1 [00:05<00:00,  4.98s/it][A[A[A[A[A[A
    
    
    
    
    
    
    
    libcurl-8.14.0       | 438 KB    | : 100% 1.0/1 [00:05<00:00,  5.01s/it][A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    libcurl-8.14.0       | 438 KB    | : 100% 1.0/1 [00:05<00:00,  5.01s/it][A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | : 100% 1.0/1 [00:05<00:00,  5.12s/it][A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | : 100% 1.0/1 [00:05<00:00,  5.12s/it][A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | : 100% 1.0/1 [00:05<00:00,  5.23s/it][A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | : 100% 1.0/1 [00:05<00:00,  5.23s/it][A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | : 100% 1.0/1 [00:05<00:00,  5.24s/it][A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | : 100% 1.0/1 [00:05<00:00,  5.24s/it][A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | : 100% 1.0/1 [00:05<00:00,  5.32s/it][A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | : 100% 1.0/1 [00:05<00:00,  5.32s/it][A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    libedit-3.1.20250104 | 132 KB    | : 100% 1.0/1 [00:05<00:00,  5.36s/it][A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    libedit-3.1.20250104 | 132 KB    | : 100% 1.0/1 [00:05<00:00,  5.36s/it][A[A[A[A[A[A[A[A[A[A[A[A[A
    htslib-1.21          | 3.0 MB    | : 100% 1.0/1 [00:05<00:00,  2.73it/s][A
    
    
    
    
    
    
    
    
    
    
    
    
    
    keyutils-1.6.1       | 115 KB    | : 100% 1.0/1 [00:05<00:00,  5.47s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    keyutils-1.6.1       | 115 KB    | : 100% 1.0/1 [00:05<00:00,  5.47s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    liblzma-5.8.1        | 110 KB    | : 100% 1.0/1 [00:05<00:00,  5.49s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    liblzma-5.8.1        | 110 KB    | : 100% 1.0/1 [00:05<00:00,  5.49s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libev-4.33           | 110 KB    | : 100% 1.0/1 [00:05<00:00,  5.49s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libev-4.33           | 110 KB    | : 100% 1.0/1 [00:05<00:00,  5.49s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libdeflate-1.24      | 71 KB     | : 100% 1.0/1 [00:05<00:00,  5.65s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libdeflate-1.24      | 71 KB     | : 100% 1.0/1 [00:05<00:00,  5.65s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libstdcxx-ng-15.1.0  | 34 KB     | : 100% 1.0/1 [00:05<00:00,  6.17s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libstdcxx-ng-15.1.0  | 34 KB     | : 100% 1.0/1 [00:05<00:00,  6.17s/it][A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
                                                                            
                                                                            [A
    
                                                                            [A[A
    
    
                                                                            [A[A[A
    
    
    
                                                                            [A[A[A[A
    
    
    
    
                                                                            [A[A[A[A[A
    
    
    
    
    
                                                                            [A[A[A[A[A[A
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
                                                                            [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    [A
    Preparing transaction: \ | done
    Verifying transaction: - \ | / - \ | / - \ | / - done
    Executing transaction: | / - \ | done
    #
    # To activate this environment, use
    #
    #     $ conda activate genomics_env
    #
    # To deactivate an active environment, use
    #
    #     $ conda deactivate
    



```
!pip install -q scikit-allel
```


```
import allel
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from io import StringIO
import seaborn as snsaa
```


```
vcf_path = 'willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz'
metadata_path = 'content/drive/MyDrive/Congen_ITV/willisornis_tutorial_metadata.csv'
```

---

##Estatísticas populacionais
As estatísticas sumárias populacionais descrevem os padrões de diversidade genética dentro de uma única população, permitindo avaliar sua variabilidade genética, estrutura interna e histórico evolutivo. Essas estatísticas são fundamentais para entender o estado genético de uma população e identificar possíveis riscos à sua viabilidade a longo prazo.

As métricas mais comuns incluem:
* Heterozigosidade observada e esperada: indicam o nível de variação genética nos indivíduos;
* π (pi, diversidade nucleotídica): estima o número médio de diferenças por par de sequências dentro da população;
* θW (theta de Watterson): baseada no número de sítios polimórficos, útil para comparações com π;
* Tajima’s D: avalia desvios do equilíbrio neutro, podendo indicar seleção ou eventos demográficos como expansão ou gargalo;
* FIS: mede o grau de endogamia, comparando a heterozigosidade observada com a esperada;
* FROH: estima o nível de homozigose baseado em trechos contínuos do genoma.

Essas estatísticas são amplamente aplicadas em estudos de genômica da conservação, onde o foco está em monitorar populações pequenas ou isoladas, identificar perda de diversidade, e subsidiar estratégias de manejo genético. Ao se concentrar em apenas uma população, essas análises ajudam a entender seus padrões internos de variação e possíveis ameaças evolutivas, mesmo na ausência de dados comparativos com outras populações.


```
# Paste your metadata (sample<TAB>population)
metadata_str = """
ITV66049	Willisornis_poecilinotus_griseiventris_E
ITV66050	Willisornis_poecilinotus_griseiventris_E
ITV66051	Willisornis_poecilinotus_griseiventris_E
ITV66052	Willisornis_poecilinotus_griseiventris_E
ITV66053	Willisornis_poecilinotus_griseiventris_E
ITV66038	Willisornis_poecilinotus_griseiventris_W
ITV66039	Willisornis_poecilinotus_griseiventris_W
ITV66041	Willisornis_poecilinotus_griseiventris_W
ITV66042	Willisornis_poecilinotus_griseiventris_W
ITV66043	Willisornis_poecilinotus_griseiventris_W
ITV66028	Willisornis_poecilinotus_lepidonota
ITV66066	Willisornis_poecilinotus_lepidonota
ITV66081	Willisornis_poecilinotus_lepidonota
ITV66083	Willisornis_poecilinotus_lepidonota
ITV66084	Willisornis_poecilinotus_lepidonota
ITV66071	Willisornis_vidua_nigrigula
ITV66072	Willisornis_vidua_nigrigula
ITV66073	Willisornis_vidua_nigrigula
ITV66075	Willisornis_vidua_nigrigula
ITV66076	Willisornis_vidua_nigrigula
"""

# Load metadata
metadata = pd.read_csv(StringIO(metadata_str), sep="\t", names=["sample", "population"])

# Show populations
print("Populations found:")
print(metadata['population'].value_counts())
```

    Populations found:
    population
    Willisornis_poecilinotus_griseiventris_E    5
    Willisornis_poecilinotus_griseiventris_W    5
    Willisornis_poecilinotus_lepidonota         5
    Willisornis_vidua_nigrigula                 5
    Name: count, dtype: int64



```
vcf_path = 'willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz'  # Change to your actual path

callset = allel.read_vcf(vcf_path, fields=['samples', 'calldata/GT', 'variants/POS'], alt_number=1)
samples = callset['samples']
positions = callset['variants/POS']
gt = allel.GenotypeArray(callset['calldata/GT'])
```


```
# Prepare results
results = []

for pop_name, group in metadata.groupby('population'):
    pop_samples = group['sample'].tolist()
    pop_idx = [np.where(samples == s)[0][0] for s in pop_samples if s in samples]

    if len(pop_idx) < 2:
        print(f"Skipping {pop_name}: not enough samples.")
        continue

    gt_pop = gt[:, pop_idx]
    ac = gt_pop.count_alleles()
    gn = gt_pop.to_n_alt()

    # Compute stats
    # Sort positions and associated allele counts
    sort_idx = np.argsort(positions)
    positions_sorted = positions[sort_idx]
    ac_sorted = ac[sort_idx]
    gn_sorted = gn[sort_idx]

# Now compute the statistics
    pi = allel.sequence_diversity(positions_sorted, ac_sorted)
    S = ac_sorted.count_segregating()
    theta_w = allel.watterson_theta(positions_sorted, ac_sorted)
    tajima_d = allel.tajima_d(gn_sorted)

    results.append({
        "population": pop_name,
        "n_samples": len(pop_idx),
        "pi": pi,
        "S": S,
        "theta_w": theta_w,
        "tajima_d": tajima_d
    })
```


```
df_stats = pd.DataFrame(results)
df_stats = df_stats.sort_values("population")
df_stats
```





  <div id="df-6fe74c76-0564-4f81-8c3c-9f1cc6bb32d7" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>population</th>
      <th>n_samples</th>
      <th>pi</th>
      <th>S</th>
      <th>theta_w</th>
      <th>tajima_d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Willisornis_poecilinotus_griseiventris_E</td>
      <td>5</td>
      <td>0.008089</td>
      <td>179823</td>
      <td>0.008741</td>
      <td>7.731940</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Willisornis_poecilinotus_griseiventris_W</td>
      <td>5</td>
      <td>0.009755</td>
      <td>238010</td>
      <td>0.011570</td>
      <td>7.820080</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Willisornis_poecilinotus_lepidonota</td>
      <td>5</td>
      <td>0.010694</td>
      <td>249668</td>
      <td>0.012136</td>
      <td>7.723001</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Willisornis_vidua_nigrigula</td>
      <td>5</td>
      <td>0.006333</td>
      <td>149226</td>
      <td>0.007254</td>
      <td>7.940702</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-6fe74c76-0564-4f81-8c3c-9f1cc6bb32d7')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-6fe74c76-0564-4f81-8c3c-9f1cc6bb32d7 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-6fe74c76-0564-4f81-8c3c-9f1cc6bb32d7');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-a323261c-366f-4659-8d48-2d0e14c6aebb">
      <button class="colab-df-quickchart" onclick="quickchart('df-a323261c-366f-4659-8d48-2d0e14c6aebb')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-a323261c-366f-4659-8d48-2d0e14c6aebb button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

  <div id="id_bec7f7d6-cfc2-4f2d-9c27-59c110e35241">
    <style>
      .colab-df-generate {
        background-color: #E8F0FE;
        border: none;
        border-radius: 50%;
        cursor: pointer;
        display: none;
        fill: #1967D2;
        height: 32px;
        padding: 0 0 0 0;
        width: 32px;
      }

      .colab-df-generate:hover {
        background-color: #E2EBFA;
        box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
        fill: #174EA6;
      }

      [theme=dark] .colab-df-generate {
        background-color: #3B4455;
        fill: #D2E3FC;
      }

      [theme=dark] .colab-df-generate:hover {
        background-color: #434B5C;
        box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
        filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
        fill: #FFFFFF;
      }
    </style>
    <button class="colab-df-generate" onclick="generateWithVariable('df_stats')"
            title="Generate code using this dataframe."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M7,19H8.4L18.45,9,17,7.55,7,17.6ZM5,21V16.75L18.45,3.32a2,2,0,0,1,2.83,0l1.4,1.43a1.91,1.91,0,0,1,.58,1.4,1.91,1.91,0,0,1-.58,1.4L9.25,21ZM18.45,9,17,7.55Zm-12,3A5.31,5.31,0,0,0,4.9,8.1,5.31,5.31,0,0,0,1,6.5,5.31,5.31,0,0,0,4.9,4.9,5.31,5.31,0,0,0,6.5,1,5.31,5.31,0,0,0,8.1,4.9,5.31,5.31,0,0,0,12,6.5,5.46,5.46,0,0,0,6.5,12Z"/>
  </svg>
    </button>
    <script>
      (() => {
      const buttonEl =
        document.querySelector('#id_bec7f7d6-cfc2-4f2d-9c27-59c110e35241 button.colab-df-generate');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      buttonEl.onclick = () => {
        google.colab.notebook.generateWithVariable('df_stats');
      }
      })();
    </script>
  </div>

    </div>
  </div>





<h4 class="colab-quickchart-section-title">Distributions</h4>
<style>
  .colab-quickchart-section-title {
      clear: both;
  }
</style>



      <div class="colab-quickchart-chart-with-code" id="chart-1d0acdf2-0580-4f5d-bb2a-5b114444f463">
        <img style="width: 180px;" src="data:image/png;base64,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
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-1d0acdf2-0580-4f5d-bb2a-5b114444f463");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-1d0acdf2-0580-4f5d-bb2a-5b114444f463"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-6797a0ec-5436-4026-908a-aa4180f58885">
        <img style="width: 180px;" src="data:image/png;base64,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
eggAgLfiI/QBgGQJFQAgWUIFAEiWUAEAkiVUEnPTTTfF6NGjo1AoxOOPP96xvbW1Nb7whS/EuHHj
4sMf/nDHPZQi3v4u1Xns6ws6W+fvfve7MXXq1JgyZUpMnDgxvvGNb3Tsa25ujiuvvDLGjRsXEydO
jP/6r//KdV9v99prr8V1110XVVVVMXny5Jg5c2bHvb5O91qeqet8ww03dGy/5JJLYtu2bR3f98or
r8T1118flZWVUVVVFXV1dbnu6+3ebp3fsGnTpigpKYm77rqrY5t17gYZSXn00UezxsbGbNSoUdnO
nTs7tn/xi1/MvvCFL2THjh3LsizLmpqaOvZdccUV2Zo1a7Isy7J/+7d/yy666KJc9/UFb7XOx44d
y84555xs165dWZZl2YEDB7KysrKspaUly7Isu+GGG7Jbb701y7Is27p1azZixIjs9ddfz21fb/fq
q69mDz30UMfP7Ne+9rXsIx/5SJZlp38tz9R1fuCBB7KjR49mWZZl//mf/5mNGjWq4/uWL1+ezZs3
L8uyLHv66aezIUOGZL/97W9z29fbvd06Z1mWvfjii9n06dOza665JvvqV7/asd06v3tCJVEn/g/0
5Zdfzvr3758dOXLkpOOeffbZrH///h3/MTp27Fj2gQ98INu/f38u+/qaN4fKueeemz366KNZlmXZ
rl27svPPPz9rbW3NsizL3v/+9xcF4vTp07Mf/OAHue3ra7Zt29bxP8rTvZZn6jqf6LnnnstKS0s7
/l1PmDAh27JlS8f+T37yk9m9996b276+5s3rPHfu3OyBBx7I5s2bVxQq1vnd89ZPL/DUU0/Fueee
G3feeWdcdNFF8Xu/93vxyCPH72b5dnepzmNfX1YoFGLdunUxe/bsGDVqVFx66aXxjW98I84666x4
/vnn4+jRozFs2LCO49+403ce+/qiu+++O6699trTvpZn6jq/1farr76649/12925Po99fc2J61xX
Vxf9+vWLWbNmnXScdX73et3dk89EbW1tcejQoZgwYUKsXLkydu7cGTNnzuxz1430tLa2trjjjjti
w4YNcdlll8W2bdti1qxZsXv37igUCj09Xq925513Rn19fTzyyCPx6quv9vQ4fdaJ63yitWvXxne+
850+dW1OTzpxnQ8fPhx33HFH/OhHP+rpsfosr6j0AhUVFdGvX7/44z/+44iIuPDCC+N3f/d3Y/fu
3UV3qY6IortU57GvL3v88cfjmWeeicsuuywiIqZPnx7l5eWxc+fOGDx4cJSWlsbhw4c7jn/jTt95
7OtL/u7v/i42bNgQDz/8cLzvfe877Wt5pq7zG9atWxfLly+PH/zgB/GBD3ygY3tFRUUcOnSo4+sT
1ySPfX3Fm9d5x44d0dTUFFOmTInRo0dHXV1d3H777bFs2bKIsM7domffeaIzb76YdubMmdlDDz2U
Zdnxi6cGDx6c/frXv86yLMs+8pGPFF34Om3atI7vy2NfX3LiOh8+fDg7++yzs71792ZZlmX79+/P
zjnnnOzQoUNZlmXZvHnzii7IPP/88zsuyMxjX1/w93//99nUqVOzF154oWj76V7LM3Wd161bl1VW
VmYHDx486XtuvfXWky7IfO6553Lb1xd0ts4nevM1Ktb53RMqiVm0aFE2YsSIrKSkJBs6dGg2duzY
LMuy7Kmnnsouv/zybOLEidmkSZOyurq6ju/55S9/mV188cXZuHHjsmnTpmVPPPFErvv6gs7W+Zvf
/GbHGk+cODH713/9147vOXz4cDZz5syssrIymzBhQrZp06Zc9/V2jY2NWURkY8aMySZPnpxNnjw5
q66uzrLs9K/lmbrOpaWlWXl5ecf2yZMnd/x2yMsvv5x96lOfysaMGZONGzcuW7duXcc589jX273d
Op/ozaFind89NyUEAJLlGhUAIFlCBQBIllABAJIlVACAZAkVACBZQgUASJZQAQCSJVQAgGQJFQAg
WUIFAEjW/wfQP7FJOQR0RwAAAABJRU5ErkJggg==
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-6797a0ec-5436-4026-908a-aa4180f58885");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-6797a0ec-5436-4026-908a-aa4180f58885"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-848b2ca5-c0a4-4b81-8919-8bed7ded15e0">
        <img style="width: 180px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAioAAAGrCAYAAADuNLxTAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90
bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAP
YQAAD2EBqD+naQAAH19JREFUeJzt3X1QlXX+//HXAexspUApBiMcWURQUyFN1gG3NFejvt6U1m7b
0somN7rO1C7tomZN6VqYMztbW9N6TNfGte1OrBjdLStcx8pCysowA1Q8bKG0ZRzdFOFw/f5wOr/O
Euqhc8EHeT5mzkznnA+X7+vKmudc5+JcDsuyLAEAABgorLsHAAAA6AihAgAAjEWoAAAAYxEqAADA
WIQKAAAwFqECAACMRagAAABjESoAAMBYhAoAADAWoQIAAIxFqAAIiSeffFLx8fHdPQaA8wyhAqBT
Jk6cqHvuuSek28zNzVVOTk5ItwmgZyNUAACAsQgVAEGbN2+eduzYoZUrV6pv377q27ev/z23263E
xERFRUXp5ptvltfr9b/31Vdfaf78+Ro8eLD69++v66+/XgcOHJAkPfjgg3rqqaf07LPP+rfp8XjU
0NCgadOm6bLLLlO/fv00evRoPf/88+c059ixY+V2u/3Pf/jDH+qKK67wP3/ooYc0efLk73s4ANiI
UAEQtFWrVunHP/6xiouLdfz4cR0/flySdPjwYe3bt08ff/yx9u3bp/fff19//OMfJUmWZenGG2+U
1+vV7t279dlnn2nUqFGaNm2aWlpadPfdd+sXv/iFfvazn/m36XK55PP5dPvtt2v//v368ssvdeed
d+rWW29VVVXVWeecMmWKtm7dKkmqrq7WyZMndeDAATU2NkqSXn31VU2dOtWmowQgFAgVACETERGh
lStX6sILL1RcXJxuuOEGVVRUSJJ2796tN998U263W5deeqmcTqcefPBBHTx4UO+8806H24yPj9es
WbPUt29f9enTR3PnztWIESNUXl5+1nmmTp2q8vJy+Xw+bd26Vddee60mTpyorVu36sSJE3rjjTcI
FcBwEd09AIDzx4ABA9SnTx//84svvljHjh2TJNXU1Ki1tfU7fzOovr6+w20ePXpUxcXFeu211/TF
F18oLCxMx48f958VOZOsrCydOnVKFRUV2rp1q2655RYdPXpUr7zyimJiYhQZGan09PTgdxRAlyFU
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
y7KsP/zhD9bNN9/cNTtmMDuP+erVq61JkyZZzc3NVltbm5WXl2etXLmyy/bNRN/3eH/xxRfWjh07
rM2bN1tpaWnn/HO9lZ3H+0zv9WZ2HvP33nvP2r9/v2VZlnXixAkrKyvL/2d1NUKlFzpy5IjVr18/
q6WlxbIsy2pra7Muu+wyq6amJmDdypUrrcLCQv/zLVu2WFlZWWd978UXX7TS0tIsr9drtbW1Wb//
/e+t3/72t3bvltHsPuYLFiywHnjgAf97paWl1qhRo2zbH9OF4nh/Y9u2be3+J34uP9eb2H28z+W9
3qarjvk3FixYYN13330hmT1YfPTTC53pztbfdqY7W5/pvenTp2vixImKjY1VXFycXn/9dS1btszu
3TKa3cd87NixKisrk9frVUtLi5577jnV1dXZvFfmCsXxPpPO/tz5yu7jjfa68pgfPnxYGzdu1LRp
077/4J1AqCDkKisr9dFHH+nTTz/VZ599psmTJ2vevHndPdZ5LTc3V9nZ2br66qt19dVXKyUlxf8/
MADoLK/Xq+nTp6u4uFhXXnllt8xAqPRC376ztaQz3tn60KFD/uffvrP1md5bv369rrnmGkVHRyss
LExz5szRtm3b7N4to9l9zB0Oh+6//37t3r1bb731lv8CuN4qFMf7TDr7c+cru4832uuKY37s2DFl
Z2dr5syZKioqCt3wQSJUeqFQ3Nn6TO8lJSWpvLzc/1snmzdv1siRI7twD81j9zE/efKkjh49Kkn6
z3/+oxUrVqi4uLgL99Asdt+9nbu+B7L7eKM9u4/58ePHlZ2drezsbN1zzz227MM565YrY9Dt9u3b
Z40fP94aOnSoNXbsWOvDDz+0LMuy5s6da7300kv+datXr7aSkpKspKQk6/bbb/f/Js+Z3jt58qSV
l5dnDRs2zBo1apQ1ZcoU/9XjvZmdx/zw4cPWsGHDrBEjRljDhg2z/vKXv3Ttzhno+x7v//73v9ag
QYOsAQMGWH369LEGDRpkLVq06Kw/11vZebzP9u+it7LzmC9fvtyKiIiw0tLS/I/ly5d3/U5alsVN
CQEAgLH46AcAABiLUAEAAMYiVAAAgLEIFQAAYCxCBQAAGItQAQAAxiJUAACAsQgVAABgLEIFAAAY
i1ABAADG+n8yhPG6FRz+iQAAAABJRU5ErkJggg==
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-848b2ca5-c0a4-4b81-8919-8bed7ded15e0");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-848b2ca5-c0a4-4b81-8919-8bed7ded15e0"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-37c74e13-7685-4218-af50-0d718e8b54f4">
        <img style="width: 180px;" src="data:image/png;base64,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
Br9J9OT8oHd0dm4OHjyoyZMnKzIyUpJ033336a233pLEa6c39eT8hINRXwSKrtu1a5fOnj2r6dOn
X3W/N998U/fdd59iY2ND1t9zzz1qa2vTPffco5dfflmDBg2yc9ybytXOTXp6up555hl9//vf19Ch
Q+VyufSf//mfkiS/36/Ro0cH942MjFRUVJR+//vfKzExsdfmN11Pz8+3eO3Y52rn5o477tCaNWt0
6tQp/dmf/Zl+85vfqLm5WWfOnOG100t6en6GDh0q6fpeO7yzc5MqLS3V448/roiIznvXsiz96le/
uqzCT5w4of3792vXrl36wx/+oL//+7+3e9ybytXOzfHjx1VWVqbq6mrV19frmWee0UMPPdQHU968
ruf88Nqx19XOzd13362/+7u/0/Tp05WTk6MRI0ZI0lV/BiK8ruf8XPdrp8cfgOGG1dzcbA0ePNj6
3//936vu98knn1i333671dbW1uk+u3btssaPHx/uEW9a1zo3r732mpWfnx+8fe7cOUuS1dLSYlVW
VlqpqanBbYFAwLr11lutCxcu2D73zeJ6zs938doJr67+XPvW7t27LbfbbVmWxWunF1zP+fmunrx2
eGfnJvT2228rPT1dY8eOvep+paWlmjdvnpxOZ3Dd2bNn9fXXX0uSOjo69PbbbyszM9PWeW8m1zo3
iYmJ+t3vfqdz585JkrZt26aUlBTdeuutuuOOO9Ta2qpPPvlE0qUv4n3ggQd022239dr8prue88Nr
x15d+bnW0NAgSfr666/14osvqrCwUJJ47fSC6zk/4Xjt8P7dTai0tFT5+fkh61588UWNHDlSixYt
kiQ1NTWprKxMn332Wch+X3zxhXw+nxwOh9ra2uT1erV69epem9101zo3Dz74oPbu3as777xTLpdL
gwYN0r/8y79Ikm655RZt3LhRPp9PFy9e1MiRI8N6gR+u7/zw2rFXV36u/fSnP1VHR4e++eYbPfbY
Y3r66acl8drpDddzfsLx2uGLQAEAgNH4GAsAABiN2AEAAEYjdgAAgNGIHQAAYDRiBwAAGI3YAQAA
RiN2AACA0YgdAABgNGIHAAAYjdgBAABG+z8nJDDLFNfdJAAAAABJRU5ErkJggg==
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-37c74e13-7685-4218-af50-0d718e8b54f4");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-37c74e13-7685-4218-af50-0d718e8b54f4"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



<h4 class="colab-quickchart-section-title">Categorical distributions</h4>
<style>
  .colab-quickchart-section-title {
      clear: both;
  }
</style>



      <div class="colab-quickchart-chart-with-code" id="chart-296b3780-4c2d-47b5-a4f8-207a408ba89b">
        <img style="width: 180px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAvUAAAGZCAYAAAAJnM4xAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90
bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAP
YQAAD2EBqD+naQAAPURJREFUeJzt3XtYVWX+///XFlRKQ51CK50iRi3ZgFtBBEMRT6mV4xSpHzFI
ZUqdyUP5yexg5jjTAT9ZV14zUE2iiVrWZI5ZXaPu3VEh1A14nFTwEJpNIjIqCLh/f/Bl/SQO7k0c
WvR8XNe+Lvc63Ou97kXx2ve+18LicrlcAgAAAGBarZq7AAAAAAA/DaEeAAAAMDlCPQAAAGByhHoA
AADA5Aj1AAAAgMkR6gEAAACTI9QDAAAAJkeoBwAAAEyOUA8AAACYHKEe+IV45ZVXmrsEAADQSAj1
wC/EkSNHmrsEAADQSAj1AAAAgMkR6gEAAACTI9QDAAAAJkeoBwAAAEyOUA8AAACYHKEeAAAAMDlC
PQAAAGByhHoAAADA5Aj1AAAAgMkR6gEAAACTI9QDAAAAJkeoBwAAAEyOUA8AAACYHKEeAAAAMDlC
PQAAAGByhHoAAADA5Aj1AAAAgMkR6gEAAACTI9QDAAAAJkeoBwAAAEyOUA8AAACYnHdzFwCgaZxb
kamTHyc1dxkAALQ41+/93+YugZF6AAAAwOwI9QAAAIDJEeoBAAAAkyPUAwAAACZHqAcAAABMjlAP
AAAAmByhHgAAADA5Qj0AAABgcoR6AAAAwOQI9QAAAIDJEeoBAAAAkyPUAwAAACZHqAcAAABMjlAP
AAAAmByhHgAAADA5Qj0AAABgcoR6AAAAwOQI9QAAAIDJEeoBAAAAkyPUN6Nnn31WiYmJxvsvvvhC
FotFDofDWDZt2jSNGjVK48ePlyTl5eWpY8eOxnqLxaIzZ85IkkaPHq0DBw40RelKTEyU3W5v0DYX
LFigtLS0GtctW7ZMDzzwQIMezx111XQlDdFHqampGjt27E9qAwAAtHzezV3AL1lMTIymTJlivLfb
7erfv78cDocGDx5sLEtOTlZMTMwV29u0aVOD1VZWViZv79p/PN54440GO1alRYsWNXibP1V9ayov
L2+UPgIAAKgJI/XNKCIiQvn5+Tp+/LgkyeFwaMGCBcZI/YkTJ3T06FGVlJTIZrNdsT1/f385nU5J
0uLFi9WrVy/ZbDbZbDYdOXJEkvTJJ5+ob9++CgkJUXR0tPbu3Wsc22q1aurUqbLZbHr//ffl7++v
BQsWKDIyUrfccosWL15sHGvw4MFav369pIqAHxgYKJvNpuDgYKWnp9da4/Dhw/Xuu+8a7x0Oh/r0
6SNJeuCBB/Tyyy9LkoqKijR+/HjdeuutioqKUk5OjrHPj0evN27caHwIOnnypGJiYhQaGiqr1ao/
/vGPunTpUp39NnjwYM2dO1cDBw7Ub37zG02bNs1YV1NNt912mwYOHKiHHnrI+PYgNTVVMTExuvfe
exUcHKyMjIwqfXTixAmNGDFCgYGBGjFihCZMmKCFCxdKkhYuXKjZs2cbx6ztWwlPzq2kpERnz56t
8ip31d0PAADAvAj1zahNmzYaMGCA7Ha7SkpKlJubq9GjR+v48eMqLi6W3W5XZGSkfHx8PGq3oKBA
S5Ys0c6dO+V0OvXVV1+pS5cuOnXqlCZOnKgVK1YoOztbDz74oGJjY+VyuSRJ+/btU3x8vJxOp+67
7z5J0pkzZ7Rt2zZ9/fXXSkpK0rffflvteI8++qi2bNkip9OpnTt3ymq11lrb5MmTlZqaarxfvnx5
lW8rKi1atEht27bV/v379eGHH+qzzz5z69w7duyof/7zn9qxY4eys7OVl5end95554r7HTp0SHa7
Xbt379Ynn3yibdu21VjTVVddpX379mnTpk366quvqqxPT0/XX/7yF+Xk5CgyMrLKupkzZyoyMlJ7
9+7VypUrq0yxcpcn5/bcc8+pQ4cOVV7OC8c8PiYAADAHQn0zi4mJkcPhUHp6usLDwyVVjOBv27ZN
DofDrWk3P+br66sePXpo0qRJSklJ0enTp+Xj46P09HQFBwcrODhYkhQXF6f8/HwjqAcEBCg6OrpK
WxMnTpQkXXfddQoICFBubm614w0dOlT333+/XnnlFeXm5qp9+/a11va73/1O27dv14kTJ/Tf//5X
GzduNI5xuS1btmjq1KmyWCzq0KFDjdvU5NKlS5o3b5569+6tPn36KDMz0/j2oi7jx4+Xt7e3rrrq
KtlsNh06dKjGmiZPniyLxaJrrrnGuM+h0oABA3TrrbfW2P6WLVuMDy/XX3+97rrrLrfOp77nNn/+
fBUWFlZ52a76tcfHBAAA5kCob2YxMTGy2+2y2+3GFJLo6Ghj2ZAhQzxu08vLS9u3b9fs2bN16tQp
RURE6PPPP7/ifjWF8cu/JfDy8lJZWVm1bd577z09//zzKi0t1ejRo7V27dpaj3HVVVfpvvvu01tv
vaV169ZpyJAhuvbaa69Ym8ViMf7t7e2t8vJy431xcbHx75deekmnTp1Senq6srOzNXHixCrra+PO
edZVk1Rz/7mzb13nczlPzq1t27by9fWt8vKy8J87AAAtFb/lm1m/fv106tQppaWlVQn1a9eu1YkT
J4zRe08UFRXpu+++08CBA/X0008rKipKu3btUkREhHJycrR7925J0tq1a9W1a1d17dq13vWXlZXp
0KFDCgsL09y5cxUbG6uMjIw695k8ebKWL1+u1NTUGqfeSNKwYcO0fPlyuVwunT17VmvWrDHWde/e
XdnZ2bpw4YLKysq0evVqY11BQYGuv/56+fj46OTJk1q3bl29z+3HhgwZohUrVsjlcum///2vW9N6
Lt+3ctrRd999p40bN1Y5n8zMTJWXl+v8+fN67733amyjMc8NAACYG0+/aWatW7dWVFSUsrKydNtt
t0mSevbsqaKiIkVFRal169Yet1lYWKjY2FidO3dOFotFPXr0UEJCgjp06KC0tDTFx8errKxMnTp1
0rp166qNOHuivLxcU6ZM0enTp+Xt7S0/Pz8tX768zn3Cw8Pl5eWlgwcPasSIETVu8/TTTysxMVG3
3Xab/Pz8FBUVpZKSEkkV05NGjx6toKAg3XDDDbr99tuNm3NnzZql2NhYWa1W3XjjjRo2bFi9z+3H
FixYoKlTp6pXr1667rrr1Lt37yqPF63LK6+8ooSEBAUGBurGG29U//79jX3vuecerVu3Tr169VK3
bt3Up08fnT9/vlobjXluAADA3CyuyrskAdSptLRU5eXl8vHx0blz53THHXfo4Ycfrja3viYXLlxQ
69at5e3trR9++EERERFatWqV+vfv3wSVV3jo2kF6tsvdTXY8AAB+Ka7f+7/NXQIj9YC7CgoKNGrU
KJWXl6u4uFi//e1vNW7cOLf2/eabbxQfHy+Xy6WLFy9qxowZTRroAQBAy0aoR6MICwurdrOp1Wqt
919n/aneeOMNLVu2rNryV199VQMHDnSrjc6dO2vHjh31On5ISIhbT+EBAACoD6bfAL8QTL8BAKBx
/Bym3/D0GwAAAMDkCPUAAACAyRHqAQAAAJMj1AMAAAAmR6gHAAAATI5QDwAAAJgcoR4AAAAwOUI9
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
LS3VjBkz1LNnT0VEROjRRx81PuTVdJ0eeOABvfzyy5Kkf/7znwoJCZHNZlNQUJA++OADSdLJkyc1
btw4hYeHKzg4WE899ZRxvMq6v/zySwUHB1epZfDgwUYbn3zyiaKiohQaGqrw8HDjm5rKazpjxgz1
7t1bVqtVmZmZRj8VFRXJZrMpLCzMaHPmzJmKjIzUiBEjqnz7dOHCBY0fP16BgYHq3bu3RowYUWs/
SRUfjCv72OFw6I477lDnzp2Vl5dnLKst1JeUlOjs2bNVXuWX6jwcAAAwsUYL9W3atNGAAQNkt9tV
UlKi3NxcjR49WsePH1dxcbHsdrsiIyPl4+PjUbsFBQVasmSJdu7cKafTqa+++kpdunTRqVOnNHHi
RK1YsULZ2dl68MEHFRsbK5fLJUnat2+f4uPj5XQ6dd9990mqGGndtm2bvv76ayUlJenbb7+tdrxH
H31UW7ZskdPp1M6dO2W1Wuusb8+ePUpISNDu3bs1b948TZgwQS6XSx999JHefPNNffnll8rJyVG7
du2MEd6kpCS1a9dOGRkZcjqdVYLp7Nmz1aZNG2VnZysrK0svvPCCR/1VWFiokJAQ7dixQ8uWLdOc
OXMkqc7+Sk5O1q233iqn06kNGzbU63rU5rXXXtM333yjPXv26PPPP1d2dnaV9TVdp0pPPfWUUlJS
5HQ6lZ2drejoaElSQkKC/vCHPygjI0O7du1SZmam1q1bV2Xf22+/XSUlJUYgP3z4sA4cOKA777xT
hw8f1sKFC7Vp0ybt2LFDq1ev1sSJE1VSUiJJ2r9/vxISEpSVlaWHH35YTz75pKSKKVDXXHONnE6n
0a4k/fvf/9Znn32mrVu3Vqnh448/1pkzZ7R3715lZWVp7dq1dfZtTEyM8eHCbrdr8ODBio6Olt1u
V2lpqb788ksNGTKkxn2fe+45dejQocor+z91Hg4AAJhYo94oWznSmJ6ervDwcEkVI/jbtm2rc5Sx
Lr6+vurRo4cmTZqklJQUnT59Wj4+PkpPT1dwcLAxGhsXF6f8/HwjqAcEBBghsNLEiRMlSdddd50C
AgKUm5tb7XhDhw7V/fffr1deeUW5ublq3759nfX5+/tr6NChkqRx48bp5MmTOnbsmDZv3qzx48cb
o7bTp0/Xv/71L0nS+vXrtWrVKmOke82aNUYtGzdu1Ny5c9WqVcWl8vPz86i/fHx8dM8990iSIiMj
dejQIUm6Yn+5q7brUZstW7Zo0qRJat26tVq3bq2EhIQq62u6TpWGDh2qWbNm6cUXX1R2drY6duyo
c+fOacuWLZo1a5YxYn7w4MEa59FPnjxZy5cvlyStWLFCcXFx8vb21scff6yDBw9q0KBBstlsio2N
VatWrXT06FFJFXPZ+/fvX60Pa1N5fj/Wu3dv7du3TzNmzNDbb79d4zaXi4mJ0bZt23Tx4kV98cUX
ioqKUnR0tBwOh77++mt16dKl1jn18+fPV2FhYZVXyHV1Hg4AAJhYo82plypCyd///nfddNNNxhSL
ypFGu92u1NRUlZaWetSml5eXtm/frq+++koOh0MRERFas2bNFferKYxfHj69vLxUVlZWbZv33ntP
O3bskMPh0OjRo7V48WJNmDDB7XotFossFkuNyyu5XC69+uqrV5yOUR9t27Y1juXl5aXy8vJ6tePt
7V1l3+LiYqPNmq6HuzeH/rhv6vrQ9NJLL2nPnj2y2+1KSEhQXFycZsyYIaniptErfeuTkJCg3r17
a8mSJVq5cqU2btwoqaL/hw8frtWrV1fb59tvv3Xr58SdcwgICNDevXu1detWbd68WY899picTqc6
depU4/Zdu3ZVt27d9Pbbb+vaa69V+/btNWDAAE2bNk09e/asdZReqrjubdu2rbLMi2ddAQDQYjXq
r/l+/frp1KlTSktLqxLq165dqxMnThij954oKirSd999p4EDB+rpp59WVFSUdu3apYiICOXk5Gj3
7t2SpLVr16pr167q2rVrvesvKyvToUOHFBYWprlz5yo2NlYZGRl17pOXl2dMmXj33XfVpUsXdevW
TcOGDdM777yjs2fPSpJSUlKMED927FgtXbpU58+flySdP39ee/bskSSNGTNGS5Ys0aVLFROiv//+
+3qfz+Xq6i9fX19jvn+l7t27G/cTZGRkGCPhtV2P2gwZMkSrV69WaWmpSktLtXLlSrdr3r9/v6xW
q/74xz9q+vTp2r59u9q3b6+YmJgqT6G5/F6Oy914443q16+f5syZo86dOxtTqe644w5t3ry5ylSg
K11nqeJbigsXLlR5Qk1djh8/LovFYlxTl8ulY8eO1blPTEyM/vSnPxnfXlx99dXq3LmzVqxYwU2y
AADA0Kgj9a1bt1ZUVJSysrJ02223SZJ69uypoqIiRUVFXXH6QU0KCwsVGxurc+fOyWKxqEePHkpI
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
PDzc+Kam8prOmDFDvXv3ltVqVWZmptFPRUVFstlsCgsLM9qcOXOmIiMjNWLEiCrfPl24cEHjx49X
YGCgevfurREjRtTaT54qKSnR2bNnq7xU3iSf3wEAQDNoslDfpk0bDRgwQHa7XSUlJcrNzdXo0aN1
/PhxFRcXy263KzIyUj4+Ph61W1BQoCVLlmjnzp1yOp366quv1KVLF506dUoTJ07UihUrlJ2drQcf
fFCxsbGq/GJi3759io+Pl9Pp1H333SepYvrEtm3b9PXXXyspKUnffvttteM9+uij2rJli5xOp3bu
3Cmr1VpnfXv27FFCQoJ2796tefPmacKECXK5XProo4/05ptv6ssvv1ROTo7atWunxx9/XFJF2G/X
rp0yMjLkdDqrBNPZs2erTZs2ys7OVlZWll544QWP+quwsFAhISHasWOHli1bpjlz5khSnf2VnJys
W2+9VU6nUxs2bKjX9ajNa6+9pm+++UZ79uzR559/ruzs7Crra7pOlZ566imlpKTI6XQqOztb0dHR
kqSEhAT94Q9/UEZGhnbt2qXMzEytW7euyr633367SkpKjEB++PBhHThwQHfeeacOHz6shQsXatOm
TdqxY4dWr16tiRMnqqSkRJK0f/9+JSQkKCsrSw8//LCefPJJSRVToK655ho5nU6jXUn697//rc8+
+0xbt26tUsPHH3+sM2fOaO/evcrKytLatWvr7FtJSktLqzL95u23365xu+eee04dOnSo8rqYe+KK
7QMAAHNq0htlY2Ji5HA4lJ6ervDwcEkVI/jbtm2Tw+Ewpt14wtfXVz169NCkSZOUkpKi06dPy8fH
R+np6QoODjZGY+Pi4pSfn28E9YCAACMEVpo4caIk6brrrlNAQIByc3OrHW/o0KG6//779corryg3
N1ft27evsz5/f38NHVox5WHcuHE6efKkjh07ps2bN2v8+PHGqO306dP1r3/9S5K0fv16rVq1yghu
a9asMWrZuHGj5s6dq1atKi6dn5+fR/3l4+Oje+65R5IUGRmpQ4cOSdIV+8tdtV2P2mzZskWTJk1S
69at1bp1ayUkJFRZX9N1qjR06FDNmjVLL774orKzs9WxY0edO3dOW7Zs0axZs4wR84MHD9Y4j37y
5Mlavny5JGnFihWKi4uTt7e3Pv74Yx08eFCDBg2SzWZTbGysWrVqpaNHj0qSunfvrv79+1frw9pU
nt+P9e7dW/v27dOMGTP09ttv17jNj8XFxcnpdBqv2u6XmD9/vgoLC6u82txywxXbBwAA5tSkE5Rj
YmL097//XTfddJMxxSI6Olp2u112u12pqakqLS31qE0vLy9t375dX331lRwOhyIiIrRmzZor7ldT
GL88fHp5eamsrKzaNu+995527Nghh8Oh0aNHa/HixZowYYLb9VosFlkslhqXV3K5XHr11VcbdDpG
pbZt2xrH8vLyUnl5eb3a8fb2rrJvcXGx0WZN18Pdm0N/3Dd1fWh66aWXtGfPHtntdiUkJCguLk4z
ZsyQJG3fvv2K3/okJCSod+/eWrJkiVauXKmNGzdKquj/4cOHa/Xq1dX2+fbbb936OXHnHAICArR3
715t3bpVmzdv1mOPPSan06lOnTrV2Z472rZtq7Zt21Zd6FX95w4AALQMTTpS369fP506dUppaWlV
Qv3atWt14sQJY/TeE0VFRfruu+80cOBAPf3004qKitKuXbsUERGhnJwc7d69W5K0du1ade3aVV27
dq13/WVlZTp06JDCwsI0d+5cxcbGKiMjo8598vLyjPnY7777rrp06aJu3bpp2LBheueddyrmOktK
SUkxQvzYsWO1dOlSnT9/XpJ0/vx57dmzR5I0ZswYLVmyRJcuXZIkff/99/U+n8vV1V++vr7GfP9K
3bt3N+4nyMjIMEbCa7setRkyZIhWr16t0tJSlZaWauXKlW7XvH//flmtVv3xj3/U9OnTtX37drVv
314xMTFVnkJz+b0cl7vxxhvVr18/zZkzR507dzamUt1xxx3avHlzlalAV7rOUsW3FBcuXNDFixfd
qv/48eOyWCzGNXW5XDp27Jhb+wIAAFyuSUfqW7duraioKGVlZem2226TJPXs2VNFRUWKiopya/rB
jxUWFio2Nlbnzp2TxWJRjx49lJCQoA4dOigtLU3x8fEqKytTp06dtG7duhpHyd1VXl6uKVOm6PTp
0/L29pafn58xfaM2VqtVqampmjlzptq0aaM1a9bIYrFo1KhR2r17tyIjI9WqVSuFhITor3/9qyRp
3rx5KikpUf/+/Y16582bJ6vVqqVLl2rOnDkKDg5W69at1a9fP73++uv1PqdKfn5+tfZXSEiIrFar
goKCFBAQoA0bNmjx4sVKSEhQSkqKIiMjjUBc2/WozUMPPaScnBwFBgaqU6dOCgsLU35+vls1P/HE
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
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-296b3780-4c2d-47b5-a4f8-207a408ba89b");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-296b3780-4c2d-47b5-a4f8-207a408ba89b"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



<h4 class="colab-quickchart-section-title">2-d distributions</h4>
<style>
  .colab-quickchart-section-title {
      clear: both;
  }
</style>



      <div class="colab-quickchart-chart-with-code" id="chart-02fb2ac6-52d7-4e19-99f5-6f14b73e324b">
        <img style="width: 180px;" src="data:image/png;base64,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
RK5CYII=
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-02fb2ac6-52d7-4e19-99f5-6f14b73e324b");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-02fb2ac6-52d7-4e19-99f5-6f14b73e324b"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-9f0daab9-a1c5-4665-b168-70baa979b5d2">
        <img style="width: 180px;" src="data:image/png;base64,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
AABJRU5ErkJggg==
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-9f0daab9-a1c5-4665-b168-70baa979b5d2");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-9f0daab9-a1c5-4665-b168-70baa979b5d2"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-8c1167a0-72bd-4887-84b0-9ca5da5deb57">
        <img style="width: 180px;" src="data:image/png;base64,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
KyuKe9j2RPqY19fXy+12S5K+/fZb/e53v9Ps2bOjuIdtSziO96m09vfOVpE+3mgs0se8trZWI0eO
1MiRI/XrX/86IvvQbDG5Mwdt3o4dO+zBgwfbl156qZ2bm2t/9NFHtm3b9sSJE+0333wzuN2zzz5r
9+nTx+7Tp4/9s5/9LPiXSKdaV19fb9955512RkaG3a9fP/vaa68N3v1+LovkMa+oqLAzMjLszMxM
OyMjw37qqaeiu3Nt0Jke77q6OvvCCy+0O3fubMfHx9sXXnihfd999532985VkTzep/tnca6K5DGf
P3++7XK57Ozs7OBr/vz50d9J27b5riUAAGAsLi0BAABjETIAAMBYhAwAADAWIQMgah588EHV19dL
OvFsm8WLF7f4Pd544w1t3LgxzJMBMBUhAyBq5s2bFwyZ1iJkAPwvQgZAVPz36c1XX321cnJyVFlZ
qc8++0z5+flKT0/XDTfcEHy2kNfr1X333adBgwYpJydHP/7xj+V2u/WPf/xDK1as0KOPPqqcnBw9
99xzqqioUF5ennJzc9W3b19NmzbtlN/Cu3PnTqWnp0s68ZCwbt266Ze//KUk6b333tM111wT4SMB
IJwIGQBR8fTTT0uS1q9fr7KyMnXt2lVlZWVauXKlPvvsMx08eFDLli2TJD366KNKSkrS5s2bVVZW
pn79+unXv/61Ro8ere9///uaNWuWysrKdOedd6pjx45auXKltm7dqo8++kh79+7Vq6++etI50tPT
5fF4tG/fPn300Ufq06eP1qxZI0l65513NGLEiMgfDABhw5etAIiZsWPH6rzzzpMkDRo0SF988YWk
E5ePjhw5EgybhoYG9e7du8n3CAQCuvfee/Xvf/9btm2rsrJSWVlZp3w6aX5+vlavXi23263x48fr
2WefVXV1tVavXt2q+3YAxA4hAyBmEhMTg//Z6XSGfC/M448/ruuuu+6077Fo0SJVVlZq06ZNSkxM
VFFR0WnvwxkxYoRWrVolt9utJUuWaNeuXVq+fLl27doVsy++A9A6XFoCEDUpKSk6cuTIabf74Q9/
qMcee0zHjh2TJB07dkzbt2+XJLVv3z7kPdxut7p3767ExERVVFTotddeO+375+fna82aNdq7d6/S
09M1YsQIzZs3T0OHDpXT6Wzl3gGIBc7IAIiamTNn6tprr9V5552n1NTUk2537733yuPx6Morr5TD
4Qgu69u3r8aPH6877rhDb7zxhqZOnarp06frxhtvVN++fZWamtqse1y6deumbt26Bc++DBs2TPv3
79fMmTPDs6MAoobvWgIAAMbi0hIAADAWl5YAnJWee+45PfHEE42WP/7447r66qtjMBGASODSEgAA
MBaXlgAAgLEIGQAAYCxCBgAAGIuQAQAAxiJkAACAsQgZAABgLEIGAAAYi5ABAADG+j91ORb/Nb9W
mgAAAABJRU5ErkJggg==
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-8c1167a0-72bd-4887-84b0-9ca5da5deb57");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-8c1167a0-72bd-4887-84b0-9ca5da5deb57"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



<h4 class="colab-quickchart-section-title">Time series</h4>
<style>
  .colab-quickchart-section-title {
      clear: both;
  }
</style>



      <div class="colab-quickchart-chart-with-code" id="chart-8e6cac5e-91ba-4dfb-b1ae-332b39a7d74f">
        <img style="width: 180px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABQEAAAITCAYAAAC3w9pyAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90
bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAP
YQAAD2EBqD+naQAAVsxJREFUeJzt3Xv81/P9P/7b692JTA7NkJTy7qC0TkpOleMIWUJmmRDFPsbC
fGzYbH5mp/ZxYeTQ8pkz2cfMZ3MWxhxizfFdSokpNZFI7w7v9+8P314frbPkvZ6u18vldVmv1/Px
fDzvz8OlXbp5PJ6PUm1tbW0AAAAAgMKqqOsCAAAAAID1SwgIAAAAAAUnBAQAAACAghMCAgAAAEDB
CQEBAAAAoOCEgAAAAABQcEJAAAAAACg4ISAAAAAAFNwXOgS89NJL67oEAAAAAFjv6td1AXXp9ddf
r+sSAAAA4FNZvHhx5s2bl48++qiuSwE+RxUVFalXr175+5e+9KVsvPHGq93vCx0CAgAAwIbon//8
Z6ZPn56KioqUSqW6LgeoQzU1Ndlqq63SvHnzVbYTAgIAAMAGZMGCBZk+fXoaN26cr3zlK2nYsKEg
EL6gamtrM3v27MyePTtNmzZd5YhAISAAAABsQObMmZN69eplu+22y6abblrX5QCfs9ra2vKfl/4H
gPfffz8ffPDBKkPAL/TCIAAAALChqqjwT3ogazwS2N8YAAAAAFBwQkAAAACADURtbW2WLFmSiRMn
rnI6eG1tbWpqajJ79uyce+65ST5eQKK2tjZHHXVU/vjHP35eJfNvQggIAAAAUEC1tbWZM2dOLr/8
8iQpryZ922235dBDD63j6vi8CQEBAAAA1sGSJUtSU1OTUqmU0047LTvttFNatmyZK664ojwib+zY
senQoUPatm2bHj16ZPz48ampqcndd9+dysrKDBgwIJWVlenQoUOeeOKJ8rb27duXR/89/fTT2W67
7ZY5dk1NTWpqatK/f/907Ngxbdu2Td++ffP6668nSYYOHZr58+enffv26dixY2pra9OzZ89cf/31
qampyRtvvJEDDjggbdq0SWVlZX7+85+Xz6dZs2Y5/fTT06VLl2y33XY5++yz6+Ly8hkRAgIAAAB8
RkqlUl588cX8+c9/zrnnnpuqqqq89dZbGTp0aMaMGZOqqqocf/zxGTRoUGpqapIkU6ZMyXHHHZdJ
kyZlxIgR+eY3v1netibHq6ioyJVXXpkXXnghVVVV2W233fL9738/SXLttdemcePGqaqqygsvvFDe
b+kKs8OGDUubNm1SVVWVcePGZeTIkXn44YfL7ebOnZtnn302zzzzTK688sq89tprn9Wl4nMmBAQA
AABYR0tXaP32t7+dUqmU9u3bp0ePHnnggQfyyCOPpG3bttl1111TW1ubU045JbNmzcrUqVOTJM2a
NcvXv/711NbW5sQTT8w///nPNQ7blo40HD16dDp16pS2bdvmxhtvzEsvvbRGNT/xxBP5zne+k1Kp
lGbNmuWggw7KvffeWz6fY489NqVSKdtuu2223377TJo06VNeIeqaEBAAAADgM7Y0RFv6v2u7b6lU
Sv369bNkyZLy7x999NFybWtra3PfffflmmuuyT333JNJkybl4osvTnV19TrVvdTGG29c/nNFRUUW
L178qfql7gkBAQAAANbR0um1o0aNSm1tbSZNmpTx48dn3333Td++fTNp0qQ8/fTTKZVKueaaa7L1
1lunVatWSZK33norf/zjH1MqlTJmzJg0bdo0rVq1Srt27TJjxoz84x//SJJcd911yx23VCrlnXfe
ySabbJKtt946CxYsyLXXXlvevvnmm6e6ujoLFixYYc277757LrvsstTW1mbGjBn585//nAMPPHA9
XCHqWv26LgAAAACgKJYsWZKdd9458+fPz8UXX1xe2OOaa67JkCFDsnjx4my22Wa55ZZbUlHx8dis
HXfcMWPGjMmIESPSoEGD3HDDDamoqEiLFi1yyimnZNddd82Xv/zl7Lvvvis85uGHH54bb7wxlZWV
2XzzzdO7d+/MnDkzpVIpW221VQYMGJCddtopjRs3zosvvpjk/0b8XXnllTnppJPKdX73u99N3759
P5drxeerVLs0qv4CGjFiREaOHFnXZQAAAMAae+uttzJ79uxUVlZmk002qetyyMfBX0VFRSoqKjJr
1qxstdVWq92ntrY2tbW1+dOf/pSzzjorVVVVn0OlFMEno7xSqZQPP/wwkydPTrNmzVb57JkODAAA
AAAFJwQEAAAAWAf16tVLqVRKbW3tGo0CTD4ewVVRUZFDDjnEKEA+F0JAAAAAACg4ISAAAAAAFJwQ
EAAAAAAKTggIAAAAAAUnBAQAAACAghMCAgAAAOtkxIgROeqoo8rf77nnnpRKpdx9993l34455pj0
7t07/fr1S5JMnDgxm266aXl7qVTK7NmzkyS9e/fOhAkTPpfajzrqqPzxj3/8XI71af3sZz/Leeed
lyS59NJLs99++yVJHnnkkfL1/LQmTpyYn/3sZ+tc42fh9NNPzxVXXPGp91+fz83s2bNz7rnnrrLN
1KlT071790/V/3e/+91sscUWad++ffnzve9971P1tTL1P9PeAAAAgM9VTU1NPly0cL0eY5MGDVNR
sfJxRPvtt1+GDx9e/v7AAw/kq1/9ah588MEccsghSZInnngil112WQ499NDVHu/RRx9d96L/n4UL
F6Zhw4Yr3X7bbbd9ZsdaX84555wV/t6nT5/06dNnnfp+9dVXM2bMmJUe4/OycOHCXHrppevUx2f5
3PyrOXPm5PLLL89Pf/rTFW5fuHBhWrVqlWefffZTH2PAgAH57W9/+6n3Xx0jAQEAAGAD9uGihdnp
ph+t18/qQsZ99tkns2bNyuTJk5Mkf/nLX/L9738/jz/+eJJk2rRpmTlzZhYsWJD27duv9pyaNWuW
J554Ikly9tlnp3Xr1uXRURMnTkySjB07Nh06dEjbtm3To0ePjB8/Pkly9913p7KyMkceeWTat2+f
66+/Ps2aNcvpp5+eLl26ZLvttsvZZ59dPlbPnj1z/fXXJ0l+9atfZccdd0z79u3Tpk2bPPTQQyut
celxBgwYkMrKynTo0KFcc5Kcd955qaysTJs2bdK/f/+88847SZLq6uqccsop6dSpU9q3b59+/fpl
1qxZSZJ33nknRx11VCorK9OuXbsceeSRST4eJXbCCSessIal13PpyMozzjgjHTp0SIsWLXLrrbeW
267sep122mmZNm1a2rdvn3322We5658kHTt2LI/qXNn9WJHq6uoMHjw4LVu2TOfOnTN06ND07Nlz
pffp8MMPz4UXXpgkufHGG9O2bdu0b98+lZWV5Xv0+uuvp1+/funUqVPatGmT0047rXy8pXXfe++9
adOmzTK1fPI+jx07Nt26dUuHDh3SqVOn8kjQpTUNHjw47dq1S2VlZR555JEkydChQzN//vy0b98+
HTt2LPc5ZMiQdOnSJb17915mdOsHH3yQfv36Zccdd0y7du2yxx57rPQ6fV6EgAAAAMA62WijjdK1
a9fce++9+eijj/Lmm29m0KBBmTlzZj788MP8+c9/TpcuXbLxxhuvVb+zZs3KqFGj8vzzz6eqqirP
PPNMmjdvnjfffDNDhw7NmDFjMmnSpBx//PEZNGhQampqkiSvvfZahgwZkqqqqpx44olJkrlz52bC
hAl55plncuWVV+a1115b7ng//OEP8+CDD6aqqiovvvhiunXrtsr6pkyZkiFDhmTy5MkZMWJEvvnN
b6ampia33XZbbrrppvz1r3/Nq6++msaNG+c73/lOkuSCCy7IJptskhdeeCFVVVXp0KFDzjzzzCTJ
sGHD0rBhw0ycODETJ07Mf/3Xf63V9frggw/SuXPnvPzyy/nVr35VHt23qut12WWXZYcddkhVVdUq
Q89V3Y+V+dWvfpXXXnstkyZNytNPP52XX355me0ruk9L/fjHP84VV1yRqqqqTJw4MQceeGCS5Jvf
/Ga+/e1v54UXXshLL72UCRMmZPTo0cvs+7WvfS0LFy4sB3gvv/xypk6dmkGDBuXll1/ORRddlAce
eCAvv/xybr755pxwwgn56KOPknwcWJ9wwgmZOHFiTjrppPzgBz9Iklx77bVp3Lhxqqqq8tJLL5WP
NWXKlDz11FN58sknl6nhjjvuyPvvv58pU6Zk4sSJ+f3vf7/Ka5sk//M//7PMdOBrrrlmtfusDdOB
AQAAYAO2SYOGeeWYH633Y6zOXnvtlXHjxqVVq1bp3LlzkqRr16556KGH8sgjj2TPPfdc6+NuueWW
admyZQYOHJh99903hx9+eCorK3PnnXembdu22XXXXZMkp556as4555xysNe8efMcfPDBy/R17LHH
Jvl4tNj222+fSZMmpXXr1su02X333fONb3wjBx10UA477LDyeaxMs2bNcthhhyX5eKTYd7/73Uye
PDn33XdfDjvssGy11VZJPh5t941vfCNJ8qc//Snz5s0rj6xbtGhRtttuuyTJgw8+mMcffzz16tVL
kvLva6pRo0Y57rjjknw8OvPNN99M8vG7A1d1vdbUyu7HyowbNy5HH310GjVqlCQZPHhwrrvuuvL2
Fd2npfbaa69897vfTf/+/XPwwQdn9913z/vvv58nn3wyZ555Zjk4nT9/fqqqqpbb/xvf+Eauvfba
9OnTJ1dffXW+/vWvp2HDhvnDH/6Q119/Pbvvvnu5balUyquvvpok2X777csjInv37p3f/OY3q7wm
3/jGN8rn90k9evTIf/7nf2bw4MHp06dPjjjiiFX2k6z/6cBCQAAAANiAVVRUZNNGG9V1Gdl///1z
/PHHZ/vtt0/v3r2TfBzkPPDAA/nrX/+aq6++OosWLVqrPuvXr58JEybk/vvvz4MPPpg99thjmRBp
ZRo3brzcb58chVhRUZHFixcv1+aee+7JY489lgceeCCHHnpozjvvvJx88slrXG+pVEqpVFrh70vV
1tbmV7/6VQYOHLjG/a6pBg0alN/dWK9evSxZsuRT9VO/fv1lrk91dXX59xXdj4MOOuhTHWdF92mp
a6+9Ns8880zuu+++HH/88TniiCPK07ifffbZbLLJJqvse9iwYenWrVs++OCD3H777bnzzjuTfHz9
99xzzxUuBjN9+vRlAr169eqt8Dn5pE8ubvNJHTp0yCuvvJK77747DzzwQH74wx9mwoQJ+cpXvrLK
/tYn04EBAACAdda7d+/MmTMnd9xxRw444IAkHweDd955Z2bPnp2+ffuudZ/vvvtu3njjjRx00EH5
5S9/mV122SXjx49P3759y1NMk+Tqq6/O1ltvvdzIvrWxcOHCvPzyy+nTp09+8pOf5OCDDy73vzJv
vfVWOUz67W9/m6ZNm2bHHXfMAQcckLvuuitz5sxJkvzmN78pB6P9+vXLpZdemnnz5iVJ5s2bl2ee
eSbJxwusXHzxxeXw7h//+MenPp9PWtX12myzzcq1LNWyZcvy+xwffvjhTJs2LcnK78fK9OnTJ7fd
dluqq6tTXV2dm266aY1r/tvf/pYePXrkBz/4QU444YQ888wz2XzzzdOzZ8/ySsnJxyvyLn0X5Se1
atUqnTp1ysknn5ymTZumR48eSZL+/fvn8ccfX2b67sMPP7zaejbffPNUV1dnwYIFa1T/5MmTU1FR
kcGDB2fUqFGpra1d65GXnzUjAQEAAIB11qhRo3Tv3j1VVVXp2rVrkqRz586ZP39+unfvvsIpk6vz
zjvvZODAgZk/f35KpVJatWqVU089NU2bNs0111yTIUOGZPHixdlss81yyy23rHIF49VZsmRJhgwZ
kvfeey/169fPlltumd/97ner3GfHHXfMmDFjMmLEiDRo0CA33HBDKioqctRRR+X5559Pz549UyqV
stNOO2XMmDFJkosuuihnn312unXrVh4heMYZZ6RHjx656qqrMmzYsLRr1y7169dP586dl1nc49Pa
brvtVnq9dt1117Rp0yaVlZVp0aJFHnrooVx00UU58cQTc91116V79+7lKb8rux8rc9ZZZ+XFF19M
27Zt06RJk3Tu3DkzZ85co5qXTldu0KBBNtpoo1x55ZVJPl7N+dvf/nYqKytTKpXSuHHjjBo1aoXT
ko877rgMHTo0l1xySfm3nXfeOaNHj87w4cPz0UcfZdGiRenYsWP23nvvVdaz9dZbZ8CAAdlpp53S
uHHjZd4LuCLPPfdcLrjggtTW1mbJkiUZOHBgevXqtcp9/ud//meZBVn22GOP5d53uC5KtbW1tZ9Z
bxuYESNGZOTIkXVdBgAAAKyxt956K7Nnz05lZeVqp0Sy/tx9990566yzVvg+Ov7Pu+++my222CLV
1dUZMGBAunTpkosvvriuy9qgfTLKK5VK+fDDDzN58uQ0a9as/B7KFTESEAAAAID1ok+fPlm4cGGq
q6vTo0ePfP/736/rkr6whIAAAAAAK9GxY8flFtho27Zt7rrrrhxyyCF1VNW/jzfffDP77bffcr/3
6dMnV111VZ5//vk6qOrf06233pof/vCHy/1+5pln5qSTTlrvxxcCAgAAAKzE6t799kXXvHlzU6LX
0KBBgzJo0KA6O77VgQEAAACg4IwEBAAAgA1IvXr1yn/+Aq/1CawlIwEBAABgA1JR4Z/ywMcrA68N
f3MAAAAAwAZkbQPAxHRgAAAA2GB9miAA+GIyEhAAAAA2YDU1NVny0fvr9VNTU7PKGkaMGJGjjjqq
/P2ee+5JqVTK3XffXf7tmGOOSe/evdOvX78kycSJE7PpppuWt5dKpcyePTtJ0rt370yYMOEzvEor
d9RRR+WPf/zj53KsT+tnP/tZzjvvvCTJpZdemv322y9J8sgjj5Sv56c1ceLE/OxnP1vnGj8Lp59+
eq644opPvf/6fG5mz56dc889d5Vtpk6dmu7du6913xMnTszGG2+cBQsWlH9r0aJFBg4cWP7+4IMP
Ztttt13rvj/JSEAAAADYgNVWf5App2yxXo+x45XvJhs3Wen2/fbbL8OHDy9/f+CBB/LVr341Dz74
YA455JAkyRNPPJHLLrsshx566GqP9+ijj6570f/PwoUL07Bhw5Vuv+222z6zY60v55xzzgp/79On
T/r06bNOfb/66qsZM2bMSo/xeVm4cGEuvfTSderjs3xu/tWcOXNy+eWX56c//ekKty9cuDCtWrXK
s88+u9Z9t2vXLk2bNs24ceNy4IEH5tVXX80mm2yS5557rtzm/vvvz2677fap60+MBAQAAADW0T77
7JNZs2ZlypQpSZK//OUv+f73v5/HH388STJt2rTMnDkzCxYsSPv27VfbX7NmzfLEE08kSc4+++y0
bt067du3T/v27TNx4sQkydixY9OhQ4e0bds2PXr0yPjx45Mkd999dyorK3PkkUemffv2uf7669Os
WbOcfvrp6dKlS7bbbrucffbZ5WP17Nkz119/fZJk5MiR2XHHHdO+ffu0adMmDz300EprXHqcAQMG
pLKyMh06dCjXnCTnnXdeKisr06ZNm/Tv3z/vvPNOkqS6ujqnnHJKOnXqlPbt26dfv36ZNWtWkuSd
d97JUUcdlcrKyrRr1y5HHnlkkuS73/1uTjjhhBXWsPR6Lh1ZecYZZ6RDhw5p0aJFbr311nLblV2v
0047LdOmTUv79u2zzz77LHf9k6Rjx47lUZ0rux8rUl1dncGDB6dly5bp3Llzhg4dmp49e670Pg0c
ODAXXnhhkuTGG29M27Zt0759+1RWVpbv0fTp09OvX7906tQpbdq0yWmnnVY+3tK677vvvrRp02aZ
Wj55n8eOHZtu3bqlQ4cO6dSpU3kk6NKaBg8enHbt2qWysjKPPPJIkmTo0KGZP39+2rdvn44dO5b7
HDJkSLp06ZLevXsvM7r1gw8+SL9+/bLjjjumXbt22WOPPVZ6nZJkt912y4MPPpgkue+++9K3b980
bdq0fH3/8pe/rHPgayQgAAAAbMBKjb708Ui99XyMVdloo43StWvX3HPPPTnhhBPy5ptvZtCgQTn7
7LPz4Ycf5p577kmXLl2y8cYbr9VxZ82alVGjRmXGjBn50pe+lHnz5qWioiJvvvlmhg4dmnvvvTe7
7rprrrjiigwaNCivvvpqkuS1117LpZdemoMPPjhJcv7552fu3LmZMGFC3nrrrbRt2zannHJKWrdu
vczxLrjggrz44ovZYYcdUl1dnY8++miV9U2ZMiW/+tWvcthhh+Xaa6/NN7/5zUyZMiVjx47NTTfd
lKeeeipbbbVVjj766HznO9/JjTfemAsuuCCbbLJJXnjhhSTJWWedlTPPPDPXX399hg0blo022igT
J05MvXr18o9//GOtrtcHH3yQzp0757/+679y++235+yzz86gQYNWeb0uu+yynHXWWamqqvrU92Nl
fvWrX+W1117LpEmTkiR77733Mtv/9T796U9/Km/78Y9/nCuuuCL77bdflixZkjlz5iT5eFr5ueee
m4MPPjgLFy7Mvvvum9GjR+fEE08s73vAAQdk4cKFeeSRR9KnT5+8/PLLmTp1agYNGpSXX345F110
UR566KFsueWWefHFF7P33ntn+vTpST4OrK+++urccMMN+dnPfpYf/OAH+ctf/pJrr702u+yyy3LX
acqUKXnqqafSqFGjZQLR3//+93n//ffLwfjbb7+9ymu7995753e/+12SZNy4cTnqqKPSoEGD/PnP
f84OO+yQ5557Lr/97W9X2cfqGAkIAAAAG7CKiorU27jJev2sKuhZaq+99sq4cePy8MMPp3PnzkmS
rl275qGHHsq4ceOy5557rvW5bbnllmnZsmUGDhyYn//855k1a1Y22WSTPPLII2nbtm123XXXJMmp
p56aWbNmZerUqUmS5s2bl4OlpY499tgkH48W23777cvB1Cftvvvu+cY3vpEf//jHqaqqyuabb77K
+po1a5bDDjssyccjxf75z39m8uTJue+++3LYYYdlq622SvLxaLvHHnssycdB19ixY8sj6f7nf/4n
r7/+epKP3/v2/e9/P/Xq1UuSbLfddmt1vRo1apTjjjsuycejM998880kWe31WlMrux8rM27cuHzj
G99Io0aN0qhRowwePHiZ7Su6T0vttdde+e53v5sf/OAH5TD1/fffz5NPPpkzzzwz7du3z1e/+tW8
/vrrKwwwv/GNb+Taa69Nklx99dUZMGBAGjZsmD/84Q95/fXXs/vuu6d9+/Y54ogjUiqVMnny5CTJ
9ttvXx4R2bt373I4uDJLz+9f7bLLLpkyZUoGDx6ca665ZpVT0pPkwAMPzIQJE7JgwYI8/fTT2X//
/bPPPvvk0UcfzaOPPpqmTZumbdu2q+xjdYSAAAAAwDrbf//989e//jUPPPBAevfuneTjIOeBBx7I
X//61xxwwAFr3Wf9+vUzYcKEnHHGGZk1a1Z23333/PnPf17tfo0bN17ut0+OQqyoqMjixYuXa3PP
PffkkksuyaJFi3LooYfm6quvXqt6S6XSClds/uRvtbW1+dWvfpWqqqpUVVVlypQpn9m77Bo0aFAO
bOvVq5clS5Z8qn7q16+/zPWprq4u//5p7sfKrOg+LXXttdfmt7/9bRo3bpzjjz8+P/jBD8oL1Dz7
7LPl6zd9+vT84he/WG7/YcOG5e67784HH3yQ22+/PSeddFKSj6//nnvuWd6/qqoqs2bNSqdOnZJk
mUCvXr16K3xOPumTi9t8UocOHfLKK6/kwAMPzOOPP56OHTuWp32vSOvWrbP11ltn9OjR2WKLLbL5
5ptn3333zfjx43P//fdn9913X2Uda0IICAAAAKyz3r17Z86cObnjjjvKgd/++++fO++8M7Nnz/5U
7zN7991388Ybb+Sggw7KL3/5y+yyyy4ZP358+vbtm0mTJuXpp59O8vFIr6233jqtWrX61PUvXLgw
L7/8cvr06ZOf/OQnOfjgg8v9r8xbb71Vfp/cb3/72zRt2jQ77rhjDjjggNx1113lKay/+c1vysFo
v379cumll2bevHlJknnz5uWZZ55J8vECKxdffHE5vFvb6cArs6rrtfnmm5drWaply5bl9zk+/PDD
mTZtWpKV34+V6dOnT2699dZUV1enuro6N9100xrX/Le//S09evTID37wg5xwwgl55plnsvnmm6dn
z57llZKTj1fkXTrl9pNatWqVTp065eSTT07Tpk3To0ePJEn//v3z+OOP58knnyy3ffjhh1dbz+ab
b57q6uplVvBdlSlTpqSioiKDBw/OqFGjUltbm9dee22V++y+++75+c9/Xl4AZNNNN03Tpk1z6623
LjeV+tPwTkAAAABgnTVq1Cjdu3dPVVVVunbtmiTp3Llz5s+fn+7du69wyuTqzJkzJ4cffnjmz5+f
UqmUVq1a5dRTT03Tpk1zzTXXZMiQIVm8eHE222yz3HLLLWs0bXlllixZkiFDhuS9995L/fr1s+WW
W5YXkliZHXfcMWPGjMmIESPSoEGD3HDDDamoqMhRRx2V559/Pj179kypVMpOO+2UMWPGJEkuuuii
nH322enWrVt5hOAZZ5yRHj165KqrrsqwYcPSrl271K9fP507d15mcY9Pa7vttlvp9erZs2fatGmT
ysrKtGjRIg899FAuuuiinHjiibnuuuvSvXv3VFZWJln5/ViZs846Ky+++GLatm2bJk2apHPnzpk5
c+Ya1XzOOefktddeS4MGDbLRRhvlyiuvTPLxas7f/va3U1lZmVKplMaNG2fUqFHZcccdl+vjuOOO
y9ChQ3PJJZeUf9t5553z29/+NsOHD89HH32URYsWpWPHjqsN2bbeeusMGDAgO+20Uxo3bpyXXnpp
le2fffbZXHDBBamtrc2SJUsycODA9OrVa5X77L333rnlllvK05GTj4PBK664IgceeOAq910Tpdra
2tp17mUDNWLEiIwcObKuywAAAIA1Nnv27Lz11luprKxc5fvYWL/uvvvuNV5Q44vs3XffzRZbbJHq
6uoMGDAgXbp0ycUXX1zXZRXKhx9+mMmTJ6dZs2bl91CuiJGAAAAAAKwXffr0ycKFC1NdXZ0ePXrk
+9//fl2X9IUlBAQAAABYiY4dOy63wEbbtm1z11135ZBDDqmjqv59vPnmm9lvv/2W+71Pnz656qqr
8vzzz9dBVf+ebr311vzwhz9c7vczzzyzvHDJ+iQEBAAAAFiJ1b377YuuefPmpkSvoUGDBmXQoEF1
dnyrAwMAAMAGpF69enVdAvBvaHUL4xgJCAAAABuQpSFgbW1tvsBrfQL/z9K/B1b3HwiEgAAAALCB
KZVKKZVKQkAgyf/9nbAqQkAAAADYAK3JP/qB4lvdNOByu/VcBwAAALCeLA0C6/pz9tln5+ijjy5/
v//++1NRUZE///nP5d8GDx6cPn365JBDDkmpVMqkSZPSpEmT8vaKioq88847KZVK6du3b1544YXP
pfajjz46//u//1vn13BVn1/+8pf54Q9/mFKplMsvvzwHHHBASqVS/vKXv5Sv56f9TJo0Kb/85S/X
S92frPXTfm6++eacdNJJK9z27LPPpnnz5uvtuv/5z3/O73//+zq//6v7rCkjAQEAAGADVlNTk4XV
S9brMRo2qrfK0Ub77bdfTj755PL3+++/P1/96lfz4IMPpl+/fkmSJ554IpdddlkOOeSQ1R7vkUce
Wfei/59FixalQYMGK91+6623fmbHWl/OPvvsFf6+1157Za+99lqnvqdMmZLRo0ev9Bh17Zhjjskx
xxxTJ8d+8MEH895772XgwIF1cvzPmhAQAAAANmALq5fkgu/fu16P8eOLv5aNNl55CNi3b9/MmjUr
r732Wlq3bp2//OUvOe+88/KLX/wiSfL6669nxowZqa6uTvv27VNVVbXK42233XYZO3Zsdtttt3zv
e9/L2LFj07BhwyTJXXfdlbZt2+b3v/99zjvvvCxevDibbbZZrrrqqnTr1i1/+tOf8p3vfCfdu3fP
3//+95xzzjk577zzcuSRR2bcuHGZPXt2vvnNb+bnP/95kqRnz575zne+k8GDB+fXv/51Lr/88jRo
0CA1NTW56qqrsvfee6+wxj/96U85/fTT06lTp7zwwgtp2LBhrr322uy2225JkgsuuCA33XRTKioq
stNOO+W3v/1tmjZtmurq6owYMSKPPfZYFi5cmNatW+e///u/s9VWW+Wdd97Jqaeemueeey4VFRXp
3Llzbrvttpx55pl57733Mnr06OVqGDFiRKqqqjJx4sTssssuGTp0aO67777Mmzcvv/rVr3LkkUcm
yUqv17e//e3MmDEj7du3z3bbbZcHH3xwmeufJDvvvHN+/vOfp1+/fiu9H2viyiuvzJVXXpnFixen
cePGufzyy9OrV69cdtllufHGG7PJJptk2rRp2WKLLXLjjTemXbt2ueyyy3LXXXfl/vvvT5KceeaZ
GTt2bL70pS9lv/32W67/X//610mSZs2aZcyYMWnVqlUuu+yy3HzzzWnatGkmTpyYhg0b5vbbb89O
O+200ns1adKk/Pd//3eWLFmSxx9/PIceemguvvji7LPPPnn33XezYMGCdOjQITfccEOaNGmyRudf
10wHBgAAANbJRhttlG7duuXee+/NRx99lDfeeCNHHnlkZs6cmfnz5+eee+5Jly5dsvHGG69Vv7Nn
z86VV16ZF154IVVVVXnmmWfSvHnz/OMf/8gJJ5yQ6667LpMmTcqJJ56YI488MjU1NUmS1157LUOG
DElVVVWOP/74JMncuXMzYcKEjB8/PldeeWWmTp263PEuuOCCPPTQQ6mqqsoLL7yQbt26rbK+yZMn
Z8iQIXn11Vdz5pln5phjjklNTU3Gjh2bG264IU8++WQmTZqUxo0b5/TTT0+S/OhHP0rjxo3z/PPP
p6qqKh07dsyZZ56ZJBk2bFgaNmxYDvX+67/+a62u1wcffJAuXbrkpZdeyq9//et873vfS5JVXq/f
/OY32WGHHVJVVZUHH3zwU92PNXH//ffnlltuyVNPPZWXX345/9//9/9l8ODB5e3PPfdcfvnLX2bK
lCk56KCDcuKJJy7Xx6233po//OEPmTBhQl544YW8/vrr5W3jx4/P+eefn3vvvTeTJk1Kr169ctxx
x5W3v/DCC/nFL36RSZMmpU+fPvnJT36SJCu9V7vttluOO+64HH744amqqsovfvGL1KtXL2PHjs2L
L75Yns5+ySWXrNH5/zswEhAAAAA2YA0b1cuPL/7aej/G6vTu3Tvjxo1Lq1at0qVLlyRJt27d8tBD
D2XcuHGfatrqFltskZYtW+bwww/P/vvvn8MPPzytW7fOH/7wh7Rt2zY9e/ZMkgwfPjxnn312pk2b
liRp3rx5DjrooGX6OvbYY5Mk2267bZo3b55XX301rVq1WqbNbrvtlm984xvp169fDjvssHTq1GmV
9TVr1iz9+/dPkpxwwgk5/fTTM2XKlNx3330ZMGBAvvzlLydJTjvttBx99NFJkv/93//NvHnz8sc/
/jHJx9OVlwZpDz74YJ544onUq1ev3P/aaNSoUfk8+/btmzfeeCNJ8uijj67yeq2pld2PNfH73/8+
r7zySrp27Vr+be7cufnwww+TJF27di1vO/3003PJJZdk8eLFy/TxwAMP5Otf/3q22GKLJMkpp5yS
Z555Jkly7733pm/fvuV7euaZZ2bkyJHlPrp06ZL27dsnSfbYY49cfvnlSbLKe/Wvamtrc/HFF+e+
++7LkiVLMm/evHTv3n2Nzv/fgZGAAAAAsAGrqKjIRhs3WK+fNVl9dL/99svjjz+eBx54IH369Eny
cTD4wAMP5IknnsgBBxyw1udWv379TJgwISNGjMjbb7+d3XbbLffeu/qpz40bN17ut0+OQqxXr95y
AVOS3HPPPfnZz36WRYsW5eCDD8611167VvWWSqUVXqtPLt5QU1OTkSNHpqqqKlVVVZkyZcpn9g7E
Bg3+717Vr18/S5Z8undF1qtXb5l9q6ury31+mvuRfBygHXXUUeXzrqqqyuzZs7PJJpt8qhqTrHJR
jH/dttFGG5X/XFFRscL7v7o+r7766jz66KN54oknMmnSpPzHf/xH+dpsCISAAAAAwDrba6+9MmfO
nIwdOzb7779/kmT//ffPnXfemdmzZ6d3795r3ed7772XN998M1/72tfyi1/8Ij169Mj48ePTp0+f
TJo0KePHj0+SXHvttdl6662zww47fOr6Fy1alFdeeSV77bVXLrzwwhx66KF56qmnVrnPW2+9lbvv
vjtJct1116Vp06Zp1apVDjjggNx555159913kyS/+c1vysHoIYcckl//+teZN29ekmTevHl59tln
kyQHHHBALr744nIA99Zbb33q8/mkVV2vzTbbrFzLUi1btszjjz+e5ONFWpZOnV7Z/VgTAwYMyB13
3JFXX301SbJkyZI89thj5e0TJkzIhAkTkiSXXXZZevXqlfr1l53AesABB+QPf/hD3nvvvdTU1GTU
qFHlbV/72tcybty48ujGkSNHZrfddluuj3+1qnu16aabZu7cueW2c+bMyZZbbpktttgi7733Xm68
8cY1Ovd/F6YDAwAAAOusUaNG6d69e6qqqsrTgTt16pQPP/ww3bt3T6NGjda6zzlz5mTAgAH56KOP
kiStWrXK8OHD07Rp04wePTrf+ta3ygtd3HbbbWs0YnFlFi9enOOOOy5z585NvXr10rRp01x//fWr
3KeysjJjxozJiBEj0qBBg9x4442pqKjIEUcckeeffz49evRYZrGJJPnJT36Ss88+e5lppCNGjEj3
7t0zatSoDB8+PO3atUv9+vXTpUuX3HLLLZ/6nJZq1qzZSq9Xz54907Zt27Rp0yYtWrTIgw8+mIsv
vjjHH398fvvb32aXXXZJZWVlkpXfjzXxta99LT/5yU/y9a9/PYsXL86iRYuy//77l6eJd+3aNWed
dVamTZuWzTfffIUB25FHHpknn3wynTt3Li8MsjSo3WWXXfKTn/ykPOK0WbNmue6661Zb16ru1dFH
H50BAwakffv2OfTQQ/Of//mf+d///d+0atUqW265ZXr16lWecr0hKNXW1tbWdRF1ZcSIERk5cmRd
lwEAAABrbO7cuXn99ddTWVm5wmmvfD4+uTIv6+ZfVwBm7cyfPz+TJ09Oy5Yts9lmm620nenAAAAA
AFBwpgMDAAAArMTOO++83CIS7dq1yx/+8If069evjqr69/GPf/wj++6773K/9+3bd5l39q3Kaaed
ltNOO+2zLo1/IQQEAAAAWIkXX3yxrkv4t7bddtuZEr2BMB0YAAAANkBf4Ff8A5+CkYAAAACwAWnQ
oEGSj1ezXbx48TqtiAts+JZOV69ff9UxnxAQAAAANiAVFRWpX79+SqVSFi1aVNflAHWspqYm9erV
S7169VbZTggIAAAAG5hSqZQGDRqkUaNGdV0K8G9gTUYECwEBAABgA1QqlUwFBlIqldaonb8tAAAA
YANWW1ubmvkL1+tndYuQXHjhhRk6dGj5+1/+8peUSqWMGzeu/Nvw4cNz0EEHZdCgQUmSadOmZfPN
Ny9vL5VKee+995Ik/fr1y8SJEz+za7QqQ4cOzcMPP/yZ9nnBBRfkxhtvXOG2yy+/PEOGDPlMj7cm
VlXT6nwW1+i6667L17/+9XXqg3VjJCAAAABswGo/WpRZu1y6Xo/xlfGnp9S44Uq377333jnhhBPK
3x9++OHsuuuuGTduXPr27Vv+bdSoUdl7771Xe7w//elP61zzUosXL17lggnXXnvtZ3aspX784x9/
5n2uq09b05IlS9bLNeLzZyQgAAAAsE569eqVt956K2+++WaSZNy4cbngggvKIwFnzJiR6dOnp7q6
Ol26dFltfzvssEMmTJiQJLnooouy0047pUuXLunSpUtef/31JMm9996bbt265atf/Wr69OmTl19+
uXzsjh075sQTT0yXLl3yP//zP9lhhx1ywQUXZLfddkurVq1y0UUXlY/Vt2/f3HnnnUk+DgQ7dOiQ
Ll26pFOnTnnqqadWWuP++++fsWPHlr+PGzcuXbt2TZIMGTIk//Vf/5UkmTdvXgYNGpR27dplzz33
zAsvvFDe519Hx919993l0HTmzJnZe++9071793Ts2DH/8R//kZqamlVet759++ass87KXnvtlR13
3DHDhw8vb1tRTe3bt89ee+2VYcOGlUcnXnfdddl7770zcODAdOrUKU8//fQy12jGjBk54IAD0qFD
hxxwwAE5+uij86Mf/ShJ8qMf/ShnnHFG+ZgrG/X4ac6NdWckIAAAAGzAShs3yFfGn77ej7EqDRs2
zO67756HH344Rx11VKZOnZp+/frlO9/5ThYsWJCHH344u+22WzbaaKO1Ou67776bX/7yl5kxY0Y2
3njjzJ8/PxUVFZk1a1aOOeaYjBs3Lp06dcqNN96YI444Ii+99FKS5JVXXskVV1yR0aNHJ0nOPvvs
vPfee/nrX/+af/7zn9lxxx1z/PHHZ7vttlvmeGeeeWaqqqqy7bbbZtGiRamurl5pbccff3yuu+66
HHHEEUmSMWPGLDMacqkf//jHadSoUaqqqvL++++nV69e2XXXXVd77ptvvnn++Mc/5ktf+lKWLFmS
ww47LLfddluOPvroVe43ZcqUPPzww1m0aFE6dOiQv/71r9ltt92Wq2njjTfOK6+8kg8++CC77757
unfvXt7+1FNP5W9/+1vatWu3XP/f+c53sttuu+XCCy/MzJkz06VLl7Rv33615/NZnBvrxkhAAAAA
2ICVSqVUNG64Xj9rsvDA3nvvnXHjxuWpp55Kz549k3w8QvCvf/1rxo0bt0bTgP9VkyZN0qZNmwwe
PDhXXXVV5syZk4022ihPPfVUOnXqlE6dOiVJvvnNb+att97KP/7xjyRJ69at06dPn2X6OuaYY5Ik
X/7yl9O6detMnTp1uePtu+++OfbYY3PppZdm6tSp+dKXvrTS2gYMGJAnn3wyM2bMyAcffJC77767
fIxPevDBB3PiiSemVCpls802W2GbFampqck555yTzp07p2vXrhk/fnx5dOSqDBo0KPXr18/GG2+c
Ll26ZMqUKSus6fjjj0+pVMqmm25afk/jUrvvvvsKA8Cl+y4NO7fZZpsccsgha3Q+n8W5sW6EgAAA
AMA623vvvfPwww/n4YcfLk9p7dOnT/m3ffbZZ637rFevXp588smcccYZmTVrVnr16pXHHntstfut
KLz75CjEevXqZfHixcu1ueOOO3LJJZdk0aJF6devX2655ZaVHmPjjTfOkUcemeuvvz6333579tln
nzRt2nS1tX0yUK1fv36WLFlS/r5gwYLyn0eOHJlZs2blqaeeyvPPP59jjjlmme0rsybnuaqakhVf
vzXZd1Xn80mf9txYN0JAAAAAYJ316NEjs2bNyo033rhMCHjLLbdkxowZ5dGBa2PevHl5++23s9de
e+X888/Pnnvumb/97W/p1atXXnjhhbz44otJkltuuSXbbbfdctN718bixYszZcqU7LLLLjnrrLNy
xBFH5Omnn17lPscff3zGjBmT6667boVTgZNkv/32y5gxY1JbW5v3338/N998c3lbZWVlnn/++Xz0
0UdZvHhxbrrppvK2d999N9tss0022mijzJw5M7fffvunPrd/tc8+++S///u/U1tbmw8++CC33Xbb
Wu173XXXJUnefvvt3H333cucz/jx47NkyZLMnz8/d9xxxwr7WJ/nxsp5JyAAAACwzho0aJA999wz
f//738vviGvbtm3mzZuXPffcMw0arPq9gisyd+7cHHHEEfnwww9TKpXSpk2bHHfccdlss81y4403
5lvf+lYWL16cLbbYIrfffvsaTVtemSVLluSEE07InDlzUr9+/Wy11VYZM2bMKvfp2bNn6tWrl8mT
J+eAAw5YYZvzzz8/Q4cOTfv27bPVVltlzz33LL9rsFevXunXr1923nnnbLvtttljjz3Ki5Gcfvrp
OeKII9KxY8c0a9Ys++2336c+t391wQUX5MQTT8xOO+2UL3/5y+ncuXM233zzNdr30ksvzXHHHZcO
HTqkWbNm2XXXXcv7Hn744bn99tuz0047pXnz5unatWvmz5+/XB/r89xYuVJtbW1tXRdRV0aMGJGR
I0fWdRkAAACwxhYsWJCpU6emVatWa73QBiTJokWLsmTJkmy00Ub58MMP87WvfS2nnXbacu8GXJGP
PvooDRo0SP369fPOO++kV69eueGGG9ZosRPWjzX9O8FIQAAAAIAvkHfffTcHHXRQlixZkgULFuSw
ww7LUUcdtUb7vvrqq/nWt76V2traLFy4MKeeeqoAcAMhBAQAAABYiV122WW5xTU6duyYG2+8sU7q
ufbaa3P55Zcv9/tll12Wvfbaa436+MpXvpJnn332Ux3/q1/9qpV8N1BCQAAAANgA1dTU1HUJXwjj
x4+v6xKWMXTo0AwdOrSuy+DfyJq+6U8ICAAAABuQhg0bpqKiIm+99Va22mqrNGzYcJ0WxAA2XLW1
tZk9e3ZKpdJqF98RAgIAAMAGpKKiIq1atcqMGTPy1ltv1XU5QB0rlUpp3rx56tWrt8p2QkAAAADY
wDRs2DAtWrTI4sWLs2TJkrouB6hDDRo0WG0AmKznEPDVV1/Ncccdl3/+85/ZbLPNct1116Vjx47L
tRs9enQuueSS1NTUZJ999skVV1yRBg0aZNq0aRkyZEj+9re/pVWrVsu9eHJl+wEAAEDRLZ3+59/B
wJqoWJ+dDxs2LCeffHImTZqUc845J0OGDFmuzdSpU3P++efnsccey+TJk/P222/n6quvTpI0adIk
F110UW666aa12g8AAAAA+D/rLQScNWtWxo8fn8GDBydJBg4cmDfeeCOTJ09ept3YsWPTv3//bLPN
NimVShk+fHhuvvnmJMmWW26ZPffcM5tsssly/a9qPwAAAADg/6y36cBvvPFGtt1229Sv//EhSqVS
WrRokenTp6eysrLcbvr06WnZsmX5+w477JDp06evtv+13a+6ujrV1dXL/Oa9CQAAAAB8EazX6cD/
Tn76059ms802W+bz9NNP13VZAAAAALDerbcQcPvtt8+MGTOyePHiJEltbW2mT5+eFi1aLNOuRYsW
ef3118vfp02btlybFVnb/c4999zMnTt3mU/Pnj3X9rQAAAAAYIOz3kLAr3zlK+nWrVtuuOGGJMkd
d9yR5s2bLzMVOPn4XYF33XVXZs6cmdra2owaNSpHH330avtf2/0aNWqUJk2aLPNZk+WTAQAAAGBD
t16nA1911VW56qqr0rZt21xyySUZM2ZMkmTo0KG56667kiStW7fOhRdemD322COVlZXZaqutMmzY
sCTJ/Pnz07x58xx55JF5+eWX07x585x77rmr3Q8AAAAA+D+l2tra2rouoq6MGDEiI0eOrOsyAAAA
AGC9+sIsDAIAAAAAX1RCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIA
AABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAA
gIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAF
JwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4I
CAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAA
AAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAA
ABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAo
OCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBC
QAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAA
AAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAA
AKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABA
wQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIIT
AgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABTceg0BX3311ey+++5p27ZtevTo
kZdeemmF7UaPHp02bdpkxx13zEknnZRFixatdltNTU3OOuus7Lzzzmnfvn1OPPHELFy4cH2eDgAA
AABskNZrCDhs2LCcfPLJmTRpUs4555wMGTJkuTZTp07N+eefn8ceeyyTJ0/O22+/nauvvnq120aP
Hp3nnnsuzz33XF555ZVUVFTk0ksvXZ+nAwAAAAAbpPUWAs6aNSvjx4/P4MGDkyQDBw7MG2+8kcmT
Jy/TbuzYsenfv3+22WablEqlDB8+PDfffPNqt/3973/Pfvvtl4YNG6ZUKuWggw7K9ddfv75OBwAA
AAA2WOstBHzjjTey7bbbpn79+kmSUqmUFi1aZPr06cu0mz59elq2bFn+vsMOO5TbrGpb9+7dc9dd
d+X999/PokWLctttt2XatGkrrae6ujrvv//+Mp8lS5Z8VqcLAAAAAP+2NtiFQYYMGZIDDzwwffr0
SZ8+fdK2bdty4LgiP/3pT7PZZpst83n66ac/x4oBAAAAoG6stxBw++23z4wZM7J48eIkSW1tbaZP
n54WLVos065FixZ5/fXXy9+nTZtWbrOqbaVSKT/60Y/yt7/9LU888UQ6dOiQjh07rrSec889N3Pn
zl3m07Nnz8/sfAEAAADg39V6CwG/8pWvpFu3brnhhhuSJHfccUeaN2+eysrKZdoNHDgwd911V2bO
nJna2tqMGjUqRx999Gq3LViwIO+++26S5J///GcuueSSfO9731tpPY0aNUqTJk2W+dSrV299nDoA
AAAA/FtZ+fzZz8BVV12VIUOG5OKLL06TJk0yZsyYJMnQoUPTv3//9O/fP61bt86FF16YPfbYI0nS
t2/fDBs2LElWuW3u3Lnp27dvKioqUlNTk9NPPz2HHnro+jwdAAAAANgglWpra2vruoi6MmLEiIwc
ObKuywAAAACA9WqDXRgEAAAAAFgzQkAAAAAAKDghIAAAAAAUnBAQAAAAAApOCAgAAAAABScEBAAA
AICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAAAAAUnBAQAAAAAApOCAgAAAAA
BScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAAAAAUnBAQAAAAAApO
CAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAAAAAUnBAQ
AAAAAApOCAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAA
AAAUnBAQAAAAAApOCAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBw9de04Ycf
fphNNtkk77///gq3N2nS5DMrCgAAAAD47KxxCLjXXnvlueeey+abb55SqZTa2trytlKplCVLlqyX
AgEAAACAdbPGIeBzzz2XJKmpqVlvxQAAAAAAn701DgE/afr06Xn00UdTKpXSu3fvbL/99p91XQAA
AADAZ2StFwa56aab0rVr19xxxx0ZO3ZsunXrlltuuWV91AYAAAAAfAbWeiTgj3/844wfPz6tWrVK
kkybNi0HHnhgjj766M+8OAAAAABg3a31SMDGjRuXA8Ak2WGHHdK4cePPtCgAAAAA4LOz1iHgwQcf
nB/96Ed5880388Ybb+THP/5xDj300Lz//vt5//3310eNAAAAAMA6KNXW1tauzQ4VFSvPDUulUpYs
WbLORX1eRowYkZEjR9Z1GQAAAACwXq31OwFramrWRx0AAAAAwHqy1tOBAQAAAIANixAQAAAAAApO
CAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAAAAAUnBAQ
AAAAAApOCAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAA
AAAUnBAQAAAAAApOCAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAA
KDghIAAAAAAUnBAQAAAAAApOCAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBw
QkAAAAAAKDghIAAAAAAUnBAQAAAAAApOCAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISA
AAAAAFBwQkAAAAAAKLj1GgK++uqr2X333dO2bdv06NEjL7300grbjR49Om3atMmOO+6Yk046KYsW
LVrttpqamowYMSIdOnTIV7/61ey9996ZPHny+jwdAAAAANggrdcQcNiwYTn55JMzadKknHPOORky
ZMhybaZOnZrzzz8/jz32WCZPnpy33347V1999Wq33XXXXXn88cfz97//Pc8//3z23XfffP/731+f
pwMAAAAAG6T1FgLOmjUr48ePz+DBg5MkAwcOzBtvvLHcaL2xY8emf//+2WabbVIqlTJ8+PDcfPPN
q91WKpVSXV2dBQsWpLa2Nu+//36aN2++vk4HAAAAADZY9ddXx2+88Ua23Xbb1K//8SFKpVJatGiR
6dOnp7Kystxu+vTpadmyZfn7DjvskOnTp69226GHHpqHH34422yzTTbddNNst912eeSRR1ZaT3V1
daqrq5f5bcmSJet+ogAAAADwb26DXRhk/PjxefHFF/OPf/wjb731Vvbdd98MHz58pe1/+tOfZrPN
Nlvm8/TTT3+OFQMAAABA3VhvIeD222+fGTNmZPHixUmS2traTJ8+PS1atFimXYsWLfL666+Xv0+b
Nq3cZlXbfve732WfffbJ5ptvnoqKihx33HF5+OGHV1rPueeem7lz5y7z6dmz52d2vgAAAADw72q9
hYBf+cpX0q1bt9xwww1JkjvuuCPNmzdfZipw8vG7Au+6667MnDkztbW1GTVqVI4++ujVbmvdunUe
euihLFy4MEly9913Z+edd15pPY0aNUqTJk2W+dSrV299nDoAAAAA/FtZb+8ETJKrrroqQ4YMycUX
X5wmTZpkzJgxSZKhQ4emf//+6d+/f1q3bp0LL7wwe+yxR5Kkb9++GTZsWJKsctu3v/3tvPLKK+nc
uXMaNGiQbbbZJqNGjVqfpwMAAAAAG6RSbW1tbV0XUVdGjBiRkSNH1nUZAAAAALBebbALgwAAAAAA
a0YICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABSc
EBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEg
AAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAA
AAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAA
UHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDg
hIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkB
AQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIA
AABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAFJwQEAAAA
gIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4ICAAAAAAF
JwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAAAAAACk4I
CAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAAABScEBAA
AAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAoOCEgAAAA
ABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIJbryHgq6++mt133z1t27ZNjx498tJLL62w3ejRo9Om
TZvsuOOOOemkk7Jo0aLVbhszZky6dOlS/nz5y1/O4Ycfvj5PBwAAAAA2SOs1BBw2bFhOPvnkTJo0
Keecc06GDBmyXJupU6fm/PPPz2OPPZbJkyfn7bffztVXX73abccff3wmTJhQ/myzzTb55je/uT5P
BwAAAAA2SOstBJw1a1bGjx+fwYMHJ0kGDhyYN954I5MnT16m3dixY9O/f/9ss802KZVKGT58eG6+
+ebVbvukp556KrNmzUr//v1XWk91dXXef//9ZT5Lliz5DM8YAAAAAP49rbcQ8I033si2226b+vXr
J0lKpVJatGiR6dOnL9Nu+vTpadmyZfn7DjvsUG6zqm2fNHr06Bx77LFp0KDBSuv56U9/ms0222yZ
z9NPP71O5wgAAAAAG4INfmGQDz/8MLfccktOPPHEVbY799xzM3fu3GU+PXv2/JyqBAAAAIC6U399
dbz99ttnxowZWbx4cerXr5/a2tpMnz49LVq0WKZdixYtMmXKlPL3adOmldusattSt99+ezp27JgO
HTqssp5GjRqlUaNGy/xWr169T3VuAAAAALAhWW8jAb/yla+kW7duueGGG5Ikd9xxR5o3b57Kyspl
2g0cODB33XVXZs6cmdra2owaNSpHH330arctNXr06NWOAgQAAACAL7L1Oh34qquuylVXXZW2bdvm
kksuyZgxY5IkQ4cOzV133ZUkad26dS688MLsscceqayszFZbbZVhw4atdluSTJw4MRMmTMigQYPW
52kAAAAAwAatVFtbW1vXRdSVESNGZOTIkXVdBgAAAACsVxv8wiAAAAAAwKoJAQEAAACg4ISAAAAA
AFBwQkAAAAAAKDghIAAAAAAUnBAQAAAAAApOCAgAAAAABScEBAAAAICCEwICAAAAQMEJAQEAAACg
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
gvv/AV6nnxkRwbJ4AAAAAElFTkSuQmCC
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-8e6cac5e-91ba-4dfb-b1ae-332b39a7d74f");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-8e6cac5e-91ba-4dfb-b1ae-332b39a7d74f"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-857ea751-be20-44f3-84de-99f358d7993b">
        <img style="width: 180px;" src="data:image/png;base64,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
f5Jk4sSJ2WCDDcrtpVIps2bNSpL07Nkz48ePXyO1H3744fnLX/6yRvpaXRdddFHOPffcJMmIESPS
t2/fJMkDDzxQHs/VNXHixFx00UWfuMZPwxlnnJHLLrtstY+vy/tm1qxZOeecc1a4z9SpU9O1a9fV
Ov83v/nNbLLJJmnXrl35861vfWu1zrU89T/VswEAAABr1OLFi/Pegvl12sf6DRqmomL584j69u2b
k08+ufx9zJgx2X777TN27Njsv//+SZJHHnkkl1xySb7yla+stL8HH3zwkxf9f82fPz8NGzZcbvst
t9zyqfVVV7797W8vc3uvXr3Sq1evT3TuF154ISNHjlxuH2vK/PnzM2LEiE90jk/zvvlfs2fPzqWX
XpoLL7xwme3z589Pq1at8vTTT692HwcddFB+85vfrPbxK2MmIAAAAKzD3lswP9ve8MM6/awsZNxj
jz0yc+bMTJ48OUny0EMP5bvf/W4efvjhJMm0adMyY8aMzJs3L+3atVvpb2rWrFkeeeSRJMmwYcPS
unXr8uyoiRMnJklGjx6d9u3bp6qqKt26dctTTz2VJLnjjjtSWVmZww47LO3atcvvf//7NGvWLGec
cUY6d+6cLbfcMsOGDSv31b179/z+979PkvziF7/INttsk3bt2qVNmza57777llvjh/0cdNBBqays
TPv27cs1J8m5556bysrKtGnTJgcccEDeeOONJElNTU1OOeWUdOzYMe3atUv//v0zc+bMJMkbb7yR
ww8/PJWVlWnbtm0OO+ywJB/MEjv++OOXWcOH4/nhzMozzzwz7du3z9Zbb52bb765vO/yxuu0007L
tGnT0q5du+yxxx5LjX+SdOjQoTyrc3nXY1lqamoyaNCgtGjRIp06dcqQIUPSvXv35V6ngw8+OD/6
0Y+SJKNGjUpVVVXatWuXysrK8jV66aWX0r9//3Ts2DFt2rTJaaedVu7vw7rvvvvutGnTZolaPnqd
R48enR122CHt27dPx44dyzNBP6xp0KBBadu2bSorK/PAAw8kSYYMGZK5c+emXbt26dChQ/mcgwcP
TufOndOzZ88lZre+++676d+/f7bZZpu0bds2u+6663LHaU0RAgIAAACfSOPGjdOlS5fcfffdef/9
9/PKK69k4MCBmTFjRt57773ceeed6dy5c9Zbb72Pdd6ZM2fmiiuuyDPPPJPq6uo8+eSTad68eV55
5ZUMGTIkI0eOzKRJk3Lcccdl4MCBWbx4cZLkxRdfzODBg1NdXZ0TTjghSfL2229n/PjxefLJJ3P5
5ZfnxRdfXKq/H/zgBxk7dmyqq6vz7LPPZocddlhhfVOmTMngwYMzefLkDB06NEcffXQWL16cW265
JTfccEMeffTRvPDCC2nSpElOP/30JMl5552X9ddfPxMmTEh1dXXat2+fs846K0ly0kknpWHDhpk4
cWImTpyYX/3qVx9rvN5999106tQpzz//fH7xi1+UZ/etaLwuueSStGzZMtXV1SsMPVd0PZbnF7/4
RV588cVMmjQpTzzxRJ5//vkl2pd1nT704x//OJdddlmqq6szceLE7LPPPkmSo48+Ol//+tczYcKE
PPfccxk/fnyuvfbaJY7de++9M3/+/HKA9/zzz2fq1KkZOHBgnn/++Zx//vkZM2ZMnn/++dx44405
/vjj8/777yf5ILA+/vjjM3HixHzta1/L9773vSTJNddckyZNmqS6ujrPPfdcua8pU6bk8ccfz2OP
PbZEDbfeemveeeedTJkyJRMnTsxtt922wrFNkj/+8Y9LPA589dVXr/SYj8PjwAAAALAOW79Bw/z7
qB/WeR8rs/vuu2fcuHFp1apVOnXqlCTp0qVL7rvvvjzwwAPZbbfdPna/m266aVq0aJFDDjkke+65
Zw4++OBUVlbm9ttvT1VVVXbaaackyamnnppvf/vb5WCvefPm2W+//ZY41zHHHJPkg9liW221VSZN
mpTWrVsvsc8uu+ySI488Mvvuu28GDBhQ/h3L06xZswwYMCDJBzPFvvnNb2by5Mm55557MmDAgGy2
2WZJPphtd+SRRyZJ/va3v2XOnDnlmXULFizIlltumSQZO3ZsHn744dSrVy9JyttXVaNGjXLssccm
+WB25iuvvJLkg3cHrmi8VtXyrsfyjBs3LkcccUQaNWqUJBk0aFCuu+66cvuyrtOHdt9993zzm9/M
AQcckP322y+77LJL3nnnnTz22GM566yzysHp3LlzU11dvdTxRx55ZK655pr06tUrV111VQ488MA0
bNgwf/rTn/LSSy9ll112Ke9bKpXywgsvJEm22mqr8ozInj175te//vUKx+TII48s/76P6tatW77z
ne9k0KBB6dWrVw499NAVniep+8eBhYAAAACwDquoqMgGjRqv7TKy11575bjjjstWW22Vnj17Jvkg
yBkzZkweffTRXHXVVVmwYMHHOmf9+vUzfvz43HvvvRk7dmx23XXXJUKk5WnSpMlS2z46C7GioiIL
Fy5cap+77rorf//73zNmzJh85StfybnnnpsTTzxxlestlUoplUrL3P6h2tra/OIXv8ghhxyyyudd
VQ0aNCi/u7FevXpZtGjRap2nfv36S4xPTU1Nefuyrse+++67Wv0s6zp96JprrsmTTz6Ze+65J8cd
d1wOPfTQ8mPcTz/9dNZff/0Vnvukk07KDjvskHfffTd/+MMfcvvttyf5YPx32223ZS4G8/LLLy8R
6NWrV2+Z98lHfXRxm49q3759/v3vf+eOO+7ImDFj8oMf/CDjx4/Pl770pRWery55HBgAAAD4xHr2
7JnZs2fn1ltvTb9+/ZJ8EAzefvvtmTVrVnr37v2xz/nmm2/mP//5T/bdd99cfPHF2XHHHfPUU0+l
d+/e5UdMk+Sqq67K5ptvvtTMvo9j/vz5ef7559OrV6/85Cc/yX777Vc+//K89tpr5TDpN7/5TZo2
bZptttkm/fr1y5///OfMnj07SfLrX/+6HIz2798/I0aMyJw5c5Ikc+bMyZNPPpnkgwVWLrjggnJ4
9+qrr6727/moFY3XRhttVK7lQy1atCi/z/H+++/PtGnTkiz/eixPr169csstt6SmpiY1NTW54YYb
Vrnmf/7zn+nWrVu+973v5fjjj8+TTz6ZjTfeON27dy+vlJx8sCLvh++i/KhWrVqlY8eOOfHEE9O0
adN069YtSXLAAQfk4YcfXuLx3fvvv3+l9Wy88capqanJvHnzVqn+yZMnp6KiIoMGDcoVV1yR2tra
jz3z8tNmJiAAAADwiTVq1Chdu3ZNdXV1unTpkiTp1KlT5s6dm65duy7zkcmVeeONN3LIIYdk7ty5
KZVKadWqVU499dQ0bdo0V199dQYPHpyFCxdmo402yk033bTCFYxXZtGiRRk8eHDeeuut1K9fP5tu
uml+97vfrfCYbbbZJiNHjszQoUPToEGDXH/99amoqMjhhx+eZ555Jt27d0+pVMq2226bkSNHJknO
P//8DBs2LDvssEN5huCZZ56Zbt265corr8xJJ52Utm3bpn79+unUqdMSi3usri233HK547XTTjul
TZs2qayszNZbb5377rsv559/fk444YRcd9116dq1a/mR3+Vdj+U5++yz8+yzz6aqqiobbrhhOnXq
lBkzZqxSzR8+rtygQYM0btw4l19+eZIPVnP++te/nsrKypRKpTRp0iRXXHHFMh9LPvbYYzNkyJD8
9Kc/LW/bbrvtcu211+bkk0/O+++/nwULFqRDhw7p06fPCuvZfPPNc9BBB2XbbbdNkyZNlngv4LL8
4x//yHnnnZfa2tosWrQohxxySHr06LHCY/74xz8usSDLrrvuutT7Dj+JUm1tbe2ndraCGjp0aIYP
H762ywAAAIC89tprmTVrViorK1f6SCR154477sjZZ5+9zPfR8f+8+eab2WSTTVJTU5ODDjoonTt3
zgUXXLC2y1qnfTTKK5VKee+99zJ58uQ0a9as/B7KZTETEAAAAIA60atXr8yfPz81NTXp1q1bvvvd
767tkj63hIAAAAAAy9GhQ4elFtioqqrKn//85+y///5rqarPjldeeSV9+/ZdanuvXr1y5ZVX5pln
nlkLVX023XzzzfnBD36w1PazzjorX/va1+q8fyEgAAAAwHKs7N1vn3fNmzf3SPQqGjhwYAYOHLjW
+rc6MAAAAAAUnJmAAAAAsA6pV69e+c/W+gRWlZmAAAAAsA6pqPCf8sAHKwN/HP7mAAAAAIB1yMcN
ABOPAwMAAMA6a3WCAODzyUxAAAAAWIctXrw4i95/p04/ixcvXmENQ4cOzeGHH17+ftddd6VUKuWO
O+4obzvqqKPSs2fP9O/fP0kyceLEbLDBBuX2UqmUWbNmJUl69uyZ8ePHf4qjtHyHH354/vKXv6yR
vlbXRRddlHPPPTdJMmLEiPTt2zdJ8sADD5THc3VNnDgxF1100Seu8dNwxhln5LLLLlvt4+vyvpk1
a1bOOeecFe4zderUdO3a9WOfe+LEiVlvvfUyb9688ratt946hxxySPn72LFjs8UWW3zsc3+UmYAA
AACwDquteTdTTtmkTvvY5vI3k/U2XG573759c/LJJ5e/jxkzJttvv33Gjh2b/fffP0nyyCOP5JJL
LslXvvKVlfb34IMPfvKi/6/58+enYcOGy22/5ZZbPrW+6sq3v/3tZW7v1atXevXq9YnO/cILL2Tk
yJHL7WNNmT9/fkaMGPGJzvFp3jf/a/bs2bn00ktz4YUXLrN9/vz5adWqVZ5++umPfe62bdumadOm
GTduXPbZZ5+88MILWX/99fOPf/yjvM+9996bnXfeebXrT8wEBAAAAD6hPfbYIzNnzsyUKVOSJA89
9FC++93v5uGHH06STJs2LTNmzMi8efPSrl27lZ6vWbNmeeSRR5Ikw4YNS+vWrdOuXbu0a9cuEydO
TJKMHj067du3T1VVVbp165annnoqSXLHHXeksrIyhx12WNq1a5ff//73adasWc4444x07tw5W265
ZYYNG1buq3v37vn973+fJBk+fHi22WabtGvXLm3atMl999233Bo/7Oeggw5KZWVl2rdvX645Sc49
99xUVlamTZs2OeCAA/LGG28kSWpqanLKKaekY8eOadeuXfr375+ZM2cmSd54440cfvjhqaysTNu2
bXPYYYclSb75zW/m+OOPX2YNH47nhzMrzzzzzLRv3z5bb711br755vK+yxuv0047LdOmTUu7du2y
xx57LDX+SdKhQ4fyrM7lXY9lqampyaBBg9KiRYt06tQpQ4YMSffu3Zd7nQ455JD86Ec/SpKMGjUq
VVVVadeuXSorK8vX6OWXX07//v3TsWPHtGnTJqeddlq5vw/rvueee9KmTZslavnodR49enR22GGH
tG/fPh07dizPBP2wpkGDBqVt27aprKzMAw88kCQZMmRI5s6dm3bt2qVDhw7lcw4ePDidO3dOz549
l5jd+u6776Z///7ZZptt0rZt2+y6667LHack2XnnnTN27NgkyT333JPevXunadOm5fF96KGHPnHg
ayYgAAAArMNKjb7wwUy9Ou5jRRo3bpwuXbrkrrvuyvHHH59XXnklAwcOzLBhw/Lee+/lrrvuSufO
nbPeeut9rH5nzpyZK664ItOnT88XvvCFzJkzJxUVFXnllVcyZMiQ3H333dlpp51y2WWXZeDAgXnh
hReSJC+++GJGjBiR/fbbL0ny/e9/P2+//XbGjx+f1157LVVVVTnllFPSunXrJfo777zz8uyzz6Zl
y5apqanJ+++/v8L6pkyZkl/84hcZMGBArrnmmhx99NGZMmVKRo8enRtuuCGPP/54NttssxxxxBE5
/fTTM2rUqJx33nlZf/31M2HChCTJ2WefnbPOOiu///3vc9JJJ6Vx48aZOHFi6tWrl1dfffVjjde7
776bTp065Ve/+lX+8Ic/ZNiwYRk4cOAKx+uSSy7J2Wefnerq6tW+Hsvzi1/8Ii+++GImTZqUJOnT
p88S7f97nf72t7+V23784x/nsssuS9++fbNo0aLMnj07yQePlZ9zzjnZb7/9Mn/+/Oy555659tpr
c8IJJ5SP7devX+bPn58HHnggvXr1yvPPP5+pU6dm4MCBef7553P++efnvvvuy6abbppnn302ffr0
ycsvv5zkg8D6qquuyvXXX5+LLroo3/ve9/LQQw/lmmuuyY477rjUOE2ZMiWPP/54GjVqtEQgettt
t+Wdd94pB+Ovv/76Cse2T58++d3vfpckGTduXA4//PA0aNAgd955Z1q2bJl//OMf+c1vfrPCc6yM
mYAAAACwDquoqEi99Tas08+Kgp4P7b777hk3blzuv//+dOrUKUnSpUuX3HfffRk3blx22223j/3b
Nt1007Ro0SKHHHJIfvazn2XmzJlZf/3188ADD6Sqqio77bRTkuTUU0/NzJkzM3Xq1CRJ8+bNy8HS
h4455pgkH8wW22qrrcrB1EftsssuOfLII/PjH/841dXV2XjjjVdYX7NmzTJgwIAkH8wU++9//5vJ
kyfnnnvuyYABA7LZZpsl+WC23d///vckHwRdo0ePLs+k++Mf/5iXXnopyQfvffvud7+bevXqJUm2
3HLLjzVejRo1yrHHHpvkg9mZr7zySpKsdLxW1fKux/KMGzcuRx55ZBo1apRGjRpl0KBBS7Qv6zp9
aPfdd883v/nNfO973yuHqe+8804ee+yxnHXWWWnXrl223377vPTSS8sMMI888shcc801SZKrrroq
Bx10UBo2bJg//elPeemll7LLLrukXbt2OfTQQ1MqlTJ58uQkyVZbbVWeEdmzZ89yOLg8H/6+/7Xj
jjtmypQpGTRoUK6++uoVPpKeJPvss0/Gjx+fefPm5Yknnshee+2VPfbYIw8++GAefPDBNG3aNFVV
VSs8x8oIAQEAAIBPbK+99sqjjz6aMWPGpGfPnkk+CHLGjBmTRx99NP369fvY56xfv37Gjx+fM888
MzNnzswuu+ySO++8c6XHNWnSZKltH52FWFFRkYULFy61z1133ZWf/vSnWbBgQb7yla/kqquu+lj1
lkqlZa7Y/NFttbW1+cUvfpHq6upUV1dnypQpn9q77Bo0aFAObOvVq5dFixat1nnq16+/xPjU1NSU
t6/O9VieZV2nD11zzTX5zW9+kyZNmuS4447L9773vfICNU8//XR5/F5++eX8/Oc/X+r4k046KXfc
cUfefffd/OEPf8jXvva1JB+M/2677VY+vrq6OjNnzkzHjh2TZIlAr169esu8Tz7qo4vbfFT79u3z
73//O/vss08efvjhdOjQofzY97K0bt06m2++ea699tpssskm2XjjjbPnnnvmqaeeyr333ptddtll
hXWsCiEgAAAA8In17Nkzs2fPzq233loO/Pbaa6/cfvvtmTVr1mq9z+zNN9/Mf/7zn+y77765+OKL
s+OOO+app55K7969M2nSpDzxxBNJPpjptfnmm6dVq1arXf/8+fPz/PPPp1evXvnJT36S/fbbr3z+
5XnttdfK75P7zW9+k6ZNm2abbbZJv3798uc//7n8COuvf/3rcjDav3//jBgxInPmzEmSzJkzJ08+
+WSSDxZYueCCC8rh3cd9HHh5VjReG2+8cbmWD7Vo0aL8Psf7778/06ZNS7L867E8vXr1ys0335ya
mprU1NTkhhtuWOWa//nPf6Zbt2753ve+l+OPPz5PPvlkNt5443Tv3r28UnLywYq8Hz5y+1GtWrVK
x44dc+KJJ6Zp06bp1q1bkuSAAw7Iww8/nMcee6y87/3337/SejbeeOPU1NQssYLvikyZMiUVFRUZ
NGhQrrjiitTW1ubFF19c4TG77LJLfvazn5UXANlggw3StGnT3HzzzUs9Sr06vBMQAAAA+MQaNWqU
rl27prq6Ol26dEmSdOrUKXPnzk3Xrl2X+cjkysyePTsHH3xw5s6dm1KplFatWuXUU09N06ZNc/XV
V2fw4MFZuHBhNtpoo9x0002r9Njy8ixatCiDBw/OW2+9lfr162fTTTctLySxPNtss01GjhyZoUOH
pkGDBrn++utTUVGRww8/PM8880y6d++eUqmUbbfdNiNHjkySnH/++Rk2bFh22GGH8gzBM888M926
dcuVV16Zk046KW3btk39+vXTqVOnJRb3WF1bbrnlcsere/fuadOmTSorK7P11lvnvvvuy/nnn58T
Tjgh1113Xbp27ZrKysoky78ey3P22Wfn2WefTVVVVTbccMN06tQpM2bMWKWav/3tb+fFF19MgwYN
0rhx41x++eVJPljN+etf/3oqKytTKpXSpEmTXHHFFdlmm22WOsexxx6bIUOG5Kc//Wl523bbbZff
/OY3Ofnkk/P+++9nwYIF6dChw0pDts033zwHHXRQtt122zRp0iTPPffcCvd/+umnc95556W2tjaL
Fi3KIYcckh49eqzwmD59+uSmm24qP46cfBAMXnbZZdlnn31WeOyqKNXW1tZ+4rMU3NChQzN8+PC1
XQYAAABk1qxZee2111JZWbnC97FRt+64445VXlDj8+zNN9/MJptskpqamhx00EHp3LlzLrjggrVd
VqG89957mTx5cpo1a1Z+D+WymAkIAAAAQJ3o1atX5s+fn5qamnTr1i3f/e5313ZJn1tCQAAAAIDl
6NChw1ILbFRVVeXPf/5z9t9//7VU1WfHK6+8kr59+y61vVevXrnyyivzzDPPrIWqPptuvvnm/OAH
P1hq+1lnnVVeuKQuCQEBAAAAlmNl7377vGvevLlHolfRwIEDM3DgwLXWv9WBAQAAYB1Sr169tV0C
8Bm0soVxzAQEAACAdciHIWBtbW2s9Ql8+PfAyv6BQAgIAAAA65hSqZRSqSQEBJL8v78TVkQICAAA
AOugVfmPfqD4VvYYcHm/Oq4DAAAAqCMfBoFr+zNs2LAcccQR5e/33ntvKioqcuedd5a3DRo0KL16
9cr++++fUqmUSZMmZcMNNyy3V1RU5I033kipVErv3r0zYcKENVL7EUcckb/+9a9rfQxX9Ln44ovz
gx/8IKVSKZdeemn69euXUqmUhx56qDyeq/uZNGlSLr744jqp+6O1ru7nxhtvzNe+9rVltj399NNp
3rx5nY37nXfemdtuu22tX/+VfVaVmYAAAACwDlu8eHHm1yyq0z4aNqq3wtlGffv2zYknnlj+fu+9
92b77bfP2LFj079//yTJI488kksuuST777//Svt74IEHPnnR/9eCBQvSoEGD5bbffPPNn1pfdWXY
sGHL3L777rtn9913/0TnnjJlSq699trl9rG2HXXUUTnqqKPWSt9jx47NW2+9lUMOOWSt9P9pEwIC
AADAOmx+zaKc992767SPH1+wdxqvt/wQsHfv3pk5c2ZefPHFtG7dOg899FDOPffc/PznP0+SvPTS
S5k+fXpqamrSrl27VFdXr7C/LbfcMqNHj87OO++cb33rWxk9enQaNmyYJPnzn/+cqqqq3HbbbTn3
3HOzcOHCbLTRRrnyyiuzww475G9/+1tOP/30dO3aNf/617/y7W9/O+eee24OO+ywjBs3LrNmzcrR
Rx+dn/3sZ0mS7t275/TTT8+gQYPyy1/+MpdeemkaNGiQxYsX58orr0yfPn2WWePf/va3nHHGGenY
sWMmTJiQhg0b5pprrsnOO++cJDnvvPNyww03pKKiIttuu21+85vfpGnTpqmpqcnQoUPz97//PfPn
z0/r1q3z29/+NptttlneeOONnHrqqfnHP/6RioqKdOrUKbfcckvOOuusvPXWW7n22muXqmHo0KGp
rq7OxIkTs+OOO2bIkCG55557MmfOnPziF7/IYYcdliTLHa+vf/3rmT59etq1a5ctt9wyY8eOXWL8
k2S77bbLz372s/Tv33+512NVXH755bn88suzcOHCNGnSJJdeeml69OiRSy65JKNGjcr666+fadOm
ZZNNNsmoUaPStm3bXHLJJfnzn/+ce++9N0ly1llnZfTo0fnCF76Qvn37LnX+X/7yl0mSZs2aZeTI
kWnVqlUuueSS3HjjjWnatGkmTpyYhg0b5g9/+EO23Xbb5V6rSZMm5be//W0WLVqUhx9+OF/5yldy
wQUXZI899sibb76ZefPmpX379rn++uuz4YYbrtLvX9s8DgwAAAB8Io0bN84OO+yQu+++O++//37+
85//5LDDDsuMGTMyd+7c3HXXXencuXPWW2+9j3XeWbNm5fLLL8+ECRNSXV2dJ598Ms2bN8+rr76a
448/Ptddd10mTZqUE044IYcddlgWL16cJHnxxRczePDgVFdX57jjjkuSvP322xk/fnyeeuqpXH75
5Zk6depS/Z133nm57777Ul1dnQkTJmSHHXZYYX2TJ0/O4MGD88ILL+Sss87KUUcdlcWLF2f06NG5
/vrr89hjj2XSpElp0qRJzjjjjCTJD3/4wzRp0iTPPPNMqqur06FDh5x11llJkpNOOikNGzYsh3q/
+tWvPtZ4vfvuu+ncuXOee+65/PKXv8y3vvWtJFnheP36179Oy5YtU11dnbFjx67W9VgV9957b266
6aY8/vjjef755/N//s//yaBBg8rt//jHP3LxxRdnypQp2XfffXPCCScsdY6bb745f/rTnzJ+/PhM
mDAhL730Urntqaeeyve///3cfffdmTRpUnr06JFjjz223D5hwoT8/Oc/z6RJk9KrV6/85Cc/SZLl
Xqudd945xx57bA4++OBUV1fn5z//eerVq5fRo0fn2WefLT/O/tOf/nSVfv9ngZmAAAAAsA5r2Khe
fnzB3nXex8r07Nkz48aNS6tWrdK5c+ckyQ477JD77rsv48aNW63HVjfZZJO0aNEiBx98cPbaa68c
fPDBad26df70pz+lqqoq3bt3T5KcfPLJGTZsWKZNm5Ykad68efbdd98lznXMMcckSbbYYos0b948
L7zwQlq1arXEPjvvvHOOPPLI9O/fPwMGDEjHjh1XWF+zZs1ywAEHJEmOP/74nHHGGZkyZUruueee
HHTQQfniF7+YJDnttNNyxBFHJEn++te/Zs6cOfnLX/6S5IPHlT8M0saOHZtHHnkk9erVK5//42jU
qFH5d/bu3Tv/+c9/kiQPPvjgCsdrVS3veqyK2267Lf/+97/TpUuX8ra333477733XpKkS5cu5bYz
zjgjP/3pT7Nw4cIlzjFmzJgceOCB2WSTTZIkp5xySp588skkyd13353evXuXr+lZZ52V4cOHl8/R
uXPntGvXLkmy66675tJLL02SFV6r/1VbW5sLLrgg99xzTxYtWpQ5c+aka9euq/T7PwvqbCbgvHnz
cuCBB6aqqiqdOnXKXnvtlcmTJy+xz3333Zd69eotkWzPnTs3Rx55ZCorK1NVVZXRo0fXaRsAAACs
yyoqKtJ4vQZ1+lmV1Uf79u2bhx9+OGPGjEmvXr2SfBAMjhkzJo888kj69ev3sX9b/fr1M378+Awd
OjSvv/56dt5559x998offW7SpMlS2z46C7FevXpLBUxJctddd+Wiiy7KggULst9+++Waa675WPWW
SqVljtVHF29YvHhxhg8fnurq6lRXV2fKlCmf2jsQGzT4f9eqfv36WbRo9d4VWa9evSWOrampKZ9z
da5H8kGAdvjhh5d/d3V1dWbNmpX1119/tWpMssJFMf63rXHjxuU/V1RULPP6r+ycV111VR588ME8
8sgjmTRpUr7xjW+Ux2ZdUKePA5944omZOHFi/vWvf2XAgAEZMmRIue3tt9/Od77znfILQj908cUX
p1GjRpk8eXLuvvvunHrqqXnjjTfqrA0AAAD45HbffffMnj07o0ePzl577ZUk2WuvvXL77bdn1qxZ
6dmz58c+51tvvZVXXnkle++9d37+85+nW7dueeqpp9KrV69MmjQpTz31VJLkmmuuyeabb56WLVuu
dv0LFizIv//97+y+++750Y9+lK985St5/PHHV3jMa6+9ljvuuCNJct1116Vp06Zp1apV+vXrl9tv
vz1vvvlmkuTXv/51ORjdf//988tf/jJz5sxJksyZMydPP/10kqRfv3654IILygHca6+9ttq/56NW
NF4bbbRRuZYPtWjRIg8//HCSDxZp+fDR6eVdj1Vx0EEH5dZbb80LL7yQJFm0aFH+/ve/l9vHjx+f
8ePHJ0kuueSS9OjRI/XrL/kAa79+/fKnP/0pb731VhYvXpwrrrii3Lb33ntn3Lhx5dmNw4cPz847
77zUOf7Xiq7VBhtskLfffru87+zZs7Pppptmk002yVtvvZVRo0at0m//rKizELBx48bp379/OUHt
0aPHEtNMv/GNb+Tcc89N06ZNlzju5ptvzsknn5wkadWqVXr37p0//vGPddb2v2pqavLOO+8s8Vnd
5BwAAAA+Lxo1apSuXbvmvffeKz8O3LFjx7z33nvp2rVrGjVq9LHPOXv27AwYMCBVVVWpqqrKggUL
cvLJJ6dZs2a59tpr89WvfjVVVVW58sorc8stt6zSjMXlWbhwYY499ti0adMm7dq1y/jx43POOees
8JjKysqMHDkyVVVV+fnPf55Ro0aloqIihx56aI4++uh069YtVVVVmTNnTvkpyJ/85CfZYYcd0rVr
11RVVaVr167lR1qvuOKKzJ8/P23btk27du0ydOjQ1f49H7Wi8erevXuqqqrSpk2b7LnnnkmSCy64
IFdffXXatm2bq666KpWVlUmWfz1Wxd57752f/OQnOfDAA9O2bdu0adMm119/fbm9S5cuOfvss1NZ
WZm//vWvy5yFedhhh+WAAw5Ip06d0rFjx2y11Vblth133DE/+clP0q9fv1RVVeWRRx7Jddddt9K6
VnStjjjiiDz33HNp165dhg0blpNPPjnvv/9+WrVqlT333DM9evRYpd/+WVGqra2tXRMdHXPMMdl0
000zYsSIjB49On/5y1/y29/+NoMHD07nzp1z5plnJvkgZZ00aVK22GKLJMm3vvWtNG7cOD/+8Y/r
pO1//fCHP8yPfvSjJbb16NEjjz76aF0NDQAAAKyyt99+Oy+99FIqKyuX+dgra8ZHV+blk/nfFYD5
eObOnZvJkyenRYsW2WijjZa73xpZHfiCCy7I5MmTc+GFF2bGjBk5//zzM2LEiDXR9cd2zjnn5O23
317i8+GLMwEAAABgXVTnqwNffPHFue222zJmzJg0adIk999/f6ZPn16eGvzf//43f/7znzNr1qz8
n//zf7L11lvnpZdeKs/amzZtWvnloXXR9r8aNWq01BTlD1flAQAAAD5ftttuu6UWkWjbtm3+9Kc/
LbXOwefRq6++Wn6M+KN69+69xDv7VuS0007Laaed9mmXxv+o0xBw+PDhufHGGzNmzJhsvPHGSZL9
9tsvr7/+enmf/30c+LDDDssVV1yRHj16ZOrUqRk3blwuu+yyOmsDAAAAWJ5nn312bZfwmbblllt6
JHodUWePA7/yyis566yz8tZbb6VPnz7p3Llzdtppp5UeN2zYsLz//vvZZpttsvfee+fSSy/NF7/4
xTprAwAAgHXRGnrFP1AQa2xhkHXZ0KFDM3z48LVdBgAAACyxCMD666//iVbEBdZ97777bqZNm5Zt
ttkm66+//nL3q/N3AgIAAACfnoqKitSvXz+lUikLFixY2+UAa9nixYtTr169la5pIQQEAACAdUyp
VEqDBg2WWtgS+HxalRnBQkAAAABYB5VKJY8CAymVSqu0n78tAAAAYB1WW1ubxXPn1+lnZcsJ/OhH
P8qQIUPK3x966KGUSqWMGzeuvO3kk0/Ovvvum4EDByZJpk2blo033rjcXiqV8tZbbyVJ+vfvn4kT
J35qY7QiQ4YMyf333/+pnvO8887LqFGjltl26aWXZvDgwZ9qf6tiRTWtzKcxRtddd10OPPDAT3QO
PhkzAQEAAGAdVvv+gszccUSd9vGlp85IqUnD5bb36dMnxx9/fPn7/fffn5122injxo1L7969y9uu
uOKK9OnTZ6X9/e1vf/vENX9o4cKFqV9/+fHHNddc86n19aEf//jHn/o5P6nVrWnRokV1MkaseWYC
AgAAAJ9Ijx498tprr+WVV15JkowbNy7nnXdeeSbg9OnT8/LLL6empiadO3de6flatmyZ8ePHJ0nO
P//8bLvttuncuXM6d+6cl156KUly9913Z4cddsj222+fXr165fnnny/33aFDh5xwwgnp3Llz/vjH
P6Zly5Y577zzsvPOO6dVq1Y5//zzy3317t07t99+e5IPAsH27dunc+fO6dixYx5//PHl1rjXXntl
9OjR5e/jxo1Lly5dkiSDBw/Or371qyTJnDlzMnDgwLRt2za77bZbJkyYUD7mf2fH3XHHHeXQdMaM
GenTp0+6du2aDh065Bvf+EYWL168wnHr3bt3zj777Oy+++7ZZpttcvLJJ5fbllVTu3btsvvuu+ek
k04qz0687rrr0qdPnxxyyCHp2LFjnnjiiSXGaPr06enXr1/at2+ffv365YgjjsgPf/jDJMkPf/jD
nHnmmeU+lzfrcXV+G5+cmYAAAACwDiut1yBfeuqMOu9jRRo2bJhddtkl999/fw4//PBMnTo1/fv3
z+mnn5558+bl/vvvz84775zGjRt/rH7ffPPNXHzxxZk+fXrWW2+9zJ07NxUVFZk5c2aOOuqojBs3
Lh07dsyoUaNy6KGH5rnnnkuS/Pvf/85ll12Wa6+9NkkybNiwvPXWW3n00Ufz3//+N9tss02OO+64
bLnllkv0d9ZZZ6W6ujpbbLFFFixYkJqamuXWdtxxx+W6667LoYcemiQZOXLkErMhP/TjH/84jRo1
SnV1dd5555306NEjO+2000p/+8Ybb5y//OUv+cIXvpBFixZlwIABueWWW3LEEUes8LgpU6bk/vvv
z4IFC9K+ffs8+uij2XnnnZeqab311su///3vvPvuu9lll13StWvXcvvjjz+ef/7zn2nbtu1S5z/9
9NOz884750c/+lFmzJiRzp07p127div9PZ/Gb+OTMRMQAAAA1mGlUikVTRrW6WdVFh7o06dPxo0b
l8cffzzdu3dP8sEMwUcffTTjxo1bpceA/9eGG26YNm3aZNCgQbnyyisze/bsNG7cOI8//ng6duyY
jh07JkmOPvrovPbaa3n11VeTJK1bt06vXr2WONdRRx2VJPniF7+Y1q1bZ+rUqUv1t+eee+aYY47J
iBEjMnXq1HzhC19Ybm0HHXRQHnvssUyfPj3vvvtu7rjjjnIfHzV27NiccMIJKZVK2WijjZa5z7Is
Xrw43/72t9OpU6d06dIlTz31VHl25IoMHDgw9evXz3rrrZfOnTtnypQpy6zpuOOOS6lUygYbbFB+
T+OHdtlll2UGgB8e+2HY+eUvfzn777//Kv2eT+O38ckIAQEAAIBPrE+fPrn//vtz//33lx9p7dWr
V3nbHnvs8bHPWa9evTz22GM588wzM3PmzPTo0SN///vfV3rcssK7j85CrFevXhYuXLjUPrfeemt+
+tOfZsGCBenfv39uuumm5fax3nrr5bDDDsvvf//7/OEPf8gee+yRpk2brrS2jwaq9evXz6JFi8rf
582bV/7z8OHDM3PmzDz++ON55plnctRRRy3Rvjyr8jtXVFOy7PFblWNX9Hs+anV/G5+MEBAAAAD4
xLp165aZM2dm1KhRS4SAN910U6ZPn16eHfhxzJkzJ6+//np23333fP/7389uu+2Wf/7zn+nRo0cm
TJiQZ599Nkly0003Zcstt1zq8d6PY+HChZkyZUp23HHHnH322Tn00EPzxBNPrPCY4447LiNHjsx1
1123zEeBk6Rv374ZOXJkamtr88477+TGG28st1VWVuaZZ57J+++/n4ULF+aGG24ot7355pv58pe/
nMaNG2fGjBn5wx/+sNq/7X/tscce+e1vf5va2tq8++67ueWWWz7Wsdddd12S5PXXX88dd9yxxO95
6qmnsmjRosydOze33nrrMs9Rl7+N5fNOQAAAAOATa9CgQXbbbbf861//Kr8jrqqqKnPmzMluu+2W
Bg1W/F7BZXn77bdz6KGH5r333kupVEqbNm1y7LHHZqONNsqoUaPy1a9+NQsXLswmm2ySP/zhD6v0
2PLyLFq0KMcff3xmz56d+vXrZ7PNNsvIkSNXeEz37t1Tr169TJ48Of369VvmPt///vczZMiQtGvX
Lptttll222238rsGe/Tokf79+2e77bbLFltskV133bW8GMkZZ5yRQw89NB06dEizZs3St2/f1f5t
/+u8887LCSeckG233TZf/OIX06lTp2y88cardOyIESNy7LHHpn379mnWrFl22mmn8rEHH3xw/vCH
P2TbbbdN8+bN06VLl8ydO3epc9Tlb2P5SrW1tbVru4jPuqFDh2b48OFruwwAAADIvHnzMnXq1LRq
1epjL7QBSbJgwYIsWrQojRs3znvvvZe99947p5122lLvBlyW999/Pw0aNEj9+vXzxhtvpEePHrn+
+utXabET6saq/p1gJiAAAADA58ibb76ZfffdN4sWLcq8efMyYMCAHH744at07AsvvJCvfvWrqa2t
zfz583PqqacKANcRQkAAAACA5dhxxx2XWlyjQ4cOGTVq1Fqp55prrsmll1661PZLLrkku++++yqd
40tf+lKefvrp1ep/++23t5LvOkoICAAAAOugxYsXr+0SPheeeuqptV3CEoYMGZIhQ4as7TL4DFnV
N/0JAQEAAGAd0rBhw1RUVOS1117LZpttloYNG36iBTGAdVdtbW1mzZqVUqm00sV3hIAAAACwDqmo
qEirVq0yffr0vPbaa2u7HGAtK5VKad68eerVq7fC/YSAAAAAsI5p2LBhtt566yxcuDCLFi1a2+UA
a1GDBg1WGgAmQkAAAABYJ334+N/KHgEESJKKtV0AAAAAAFC3hIAAAAAAUHBCQAAAAAAoOCEgAAAA
ABScEBAAAAAACk4ICAAAAAAFJwQEAAAAgIITAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQAAAAAAo
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
AQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAAAAAUnBAQAAAAAApOCAgAAAAABScEBAAAAICCEwIC
AAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAAAAAUnBAQAAAAAApOCAgAAAAABScEBAAA
AICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwQkAAAAAAKDghIAAAAAAUnBAQAAAAAApOCAgAAAAA
BScEBAAAAICCEwICAAAAQMEJAQEAAACg4ISAAAAAAFBwdRoCnn766WnZsmVKpVLGjx9f3v63v/0t
O+ywQzp37pztttsuv/3tb8ttM2fOzD777JM2bdpku+22y4MPPlinbQAAAABQdHUaAh566KF56KGH
0qJFi/K22traDBo0KNddd13Gjx+fO+64IyeddFLmzJmTJPnOd76THj165IUXXsjIkSNz1FFHZcGC
BXXWBgAAAABFV78uT96zZ89lbi+VSnnrrbeSJO+8806aNm2aRo0aJUluueWWTJ48OUnSrVu3NGvW
LA888ED69u1bJ20AAAAAUHR1GgIuS6lUys0335yDDz4466+/ft58883cdtttadiwYd54440sWLAg
X/7yl8v7t2zZMi+//HKdtC1LTU1Nampqlti2aNGiT+vnAwAAAMAat8YXBlm4cGHOP//83HbbbXnp
pZcyduzYHHPMMfnvf/+7pktZpgsvvDAbbbTREp8nnnhibZcFAAAAAKttjYeA48ePz2uvvVZ+VLhb
t25p3rx5/vnPf6Zp06apX79+ZsyYUd5/2rRp2XrrreukbVnOOeecvP3220t8unfv/mkPAwAAAACs
MWs8BNxqq60yffr0/Pvf/06STJ48OVOmTEnbtm2TJIcddliuuOKKJMmTTz6ZV199Nb169aqztv/V
qFGjbLjhhkt86tWrVxdDAQAAAABrRJ2+E/Ckk07KX//618yYMSN77713Nthgg0yePDlXXXVVDj/8
8FRUVGTx4sW59NJLyzPzLrroohxzzDFp06ZNGjZsmOuvvz4NGjSoszYAAAAAKLpSbW1t7dou4rNu
6NChGT58+NouAwAAAABWyxp/HBgAAAAAWLOEgAAAAABQcEJAAAAAACg4ISAAAAAAFJwQEAAAAAAK
TggIAAAAAAUnBAQAAACAghMCAgAAAEDBCQEBAAAAoOCEgAAAAABQcEJAAAAAACg4ISAAAAAAFJwQ
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
wiAAAAAAUHBmAgIAAABAwQkBAQAAAKDghIAAAAAAUHBCQPi/Ro4cmVKplNtvv32Z7RdddFHat2+f
zp07p0ePHnniiSfWbIF8rq3s/vz5z3+e7bbbLu3bt89BBx2Ut956a43Wx+dby5Yt07Zt23Tu3Dmd
O3fOzTffvMz9rr322rRp0ybbbLNNvva1r2XBggVruFI+j1bl/pw2bVp69+6djTbaKJ07d17zRfK5
tir36H333Zfu3bunffv26dChQ771rW9l8eLFa6FaANZl9dd2AfBZMG3atFx99dXp0aPHMtvHjx+f
yy67LM8991y+8IUv5Prrr883vvENQSBrxMruz3vvvTcjR47M448/ng022CDnn39+vve97+XXv/71
Gq6Uz7Obb755heHJ1KlT8/3vfz//+Mc/svnmm2fAgAG56qqr8vWvf33NFcnn1sruzw033DDnn39+
3n777Xzve99bc4XB/7Wye3STTTbJTTfdlNatW2fevHnp27dvfve732Xw4MFrrEYA1n1mAvK5t3jx
4gwZMiSXXHJJGjVqtMx9SqVSFixYkPfeey9J8tZbb6V58+Zrskw+p1bl/vzXv/6V3XbbLRtssEGS
pH///vn973+/JsuElRo9enQOOOCAfPnLX06pVMrJJ5+cG2+8cW2XBUmSTTfdNLvttlvWX3/9tV0K
LFOXLl3SunXrJEnjxo3TuXPnTJs2be0WBcA6RwjI597w4cOz6667pmvXrsvdp1OnTvnmN7+ZVq1a
pXnz5vnlL3+ZSy65ZA1WyefVqtyfXbt2zZgxYzJjxozU1tZm1KhRmTNnTmbPnr0GK+Xz7qtf/Wo6
duyYE044IbNmzVqq/eWXX06LFi3K31u2bJmXX355TZbI59jK7k9Y2z7OPTpjxoyMHj06+++//xqq
DoCiEALyufbss8/m1ltvzbnnnrvC/aZOnZrbbrstkydPziuvvJJvfvObGThw4Bqqks+rVb0/+/Tp
k7PPPjv7779/evTokc022yxJUr++Nz6wZjz44IN55pln8o9//CNf/OIXc+yxx67tkqDM/cln3ce5
R99555185Stfybe+9a3suOOOa7BKAIrAfyHyufb3v/8906ZNS5s2bZJ88C+rJ554YqZPn55TTjml
vN+tt96ajh07plmzZkmS4447Lqeddlrmz5+fhg0brpXaKb5VvT+T5NRTT82pp56aJHnsscfSvHnz
bLjhhmu8Zj6ftt566yRJgwYNcuaZZ6aqqmqZ+0yZMqX8fdq0aeXjoC6tyv0Ja9Oq3qNz5szJPvvs
kwEDBmTo0KFrskQACsJMQD7XTjnllEyfPj3Tpk3LtGnT0qNHj1x11VVLBSytW7fOww8/nHfffTdJ
cscdd6SqqkoASJ1a1fszSaZPn54kmTt3bs4777x861vfWtPl8jn13nvvLbEa9Y033pguXbostd8h
hxySP//5z+XH1q+44oocccQRa7BSPo9W9f6EtWVV79F33303++yzT/bZZ5+VPiEAAMtjJiAsx3nn
nZdmzZrl5JNPzkEHHZQnn3wyO+64Yxo1apT1118/N9xww9oukc+xj96fSdKvX78sXrw48+fPzzHH
HJNvfOMba7lCPi9ef/31HHLIIVm0aFFqa2vTunXr/O53v0uSDBkyJAcccEAOOOCAtG7dOj/60Y+y
6667Jkl69+6dk046aW2WzufAqt6fc+fOTVVVVWpqavL222+nefPmOeaYY3LhhReu5V9A0a3qPTpi
xIg88cQTee+993LbbbclSQ477DCrWQPwsZRqa2tr13YRAAAAAEDd8TgwAAAAABScEBAAAAAACk4I
CAAAAAAFJwQEAAAAgIITAgIAnzm9e/fO7bffvrbLAACAwhACAgAAAEDBCQEB4HOiVCrlggsuSPfu
3dOqVauMHDlyhfu/8MIL2XXXXdOpU6d07Ngx5557bpJk7Nix2XnnndOlS5d06NAh1157bfmYwYMH
58QTT0zfvn3TqlWrHH/88XniiSfSu3fvtG7dOkOHDi3v27t375x22mnp1q1bKisrc9ZZZ6W2tnap
OubMmZOvfe1r6d69e7bffvuceOKJmT9/fpLk/PPPz7bbbpvOnTunc+fOeemllz6NoQIAgMKpv7YL
AADWnEaNGuWJJ55IdXV1unXrlmOOOSb16y/7/w5ceuml2X///XPOOeckSWbPnp0k2WGHHfLQQw+l
Xr16mT17drp06ZK99947zZs3T5JMmDAh999/fyoqKtK+ffu8+eabuffeezN//vy0bt06J5xwQjp0
6JAkef755/PII49kwYIF6dmzZ2688cYcddRRS9Rx1llnZffdd8/VV1+d2trafO1rX8uIESMyZMiQ
XHzxxZk+fXrWW2+9zJ07NxUV/n0TAACWRQgIAJ8jRx99dJKkXbt2qV+/fmbMmFEO7/5Xz549M2zY
sLz77rvp1atX+vbtmyR54403csIJJ2TSpEmpX79+3njjjTz77LPl8wwYMCCNGzdOknTs2DF77713
GjRokAYNGqR9+/Z54YUXyiHgV7/61XLboEGDMmbMmKVCwNtvvz2PPvpohg8fniR5//33U69evWy4
4YZp06ZNBg0alH79+mW//fZb7m8BAIDPO/9cDgCfIx+Gc0lSr169LFy4cLn7HnLIIXn44YfTtm3b
8qzAJDn55JOz2267ZcKECRk/fnyqqqoyb9685fbxcfoslUpLbautrc2tt96a8ePHZ/z48Zk4cWKu
vPLK1KtXL4899ljOPPPMzJw5Mz169Mjf//73VRsIAAD4nBECAgDL9MILL2TzzTfPV7/61fzsZz/L
Y489liR5880306JFi5RKpTz44IP517/+tdp9XH/99VmwYEHef//93HDDDeXZhh914IEH5qKLLiqH
h2+++WYmT56cOXPm5PXXX8/uu++e73//+9ltt93yz3/+c7VrAQCAIvM4MACwTKNHj87111+fhg0b
ZvHixbniiiuSJD/96U9z6qmn5ic/+Uk6d+6cnXbaabX72HbbbbPrrrtm9uzZGTBgQI444oil9vnl
L3+Z73znO+ncuXMqKipSv379/OxnP0vjxo1z6KGH5r333kupVEqbNm1y7LHHrnYtAABQZKXaZS3D
BwBQx3r37p0zzzwzBx544NouBQAACs/jwAAAAABQcGYCAsDn3I477rjUYh0dOnTIqFGj1lJFAADA
p00ICAAAAAAF53FgAAAAACg4ISAAAAAAFJwQEAAAAAAKTggIAAAAAAUnBAQAAACAghMCAgAAAEDB
CQEBAAAAoOD+f+fqTPAQ2B7XAAAAAElFTkSuQmCC
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-857ea751-be20-44f3-84de-99f358d7993b");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-857ea751-be20-44f3-84de-99f358d7993b"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-9f2e8411-921d-467a-8252-0d93b632bda8">
        <img style="width: 180px;" src="data:image/png;base64,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
8eOqUaOGZbvJZFJKSookqUuXLoqKirojtQ8dOlRffPHFHTnWrXrrrbc0ffp0SdL8+fPVo0cPSdJ3
331nuZ636vjx43rrrbduu8byMHHiRC1atOiW97fmc5OSkqJp06YV2+bkyZNq06bNLfX/4osvqk6d
OvLy8rJ8XnrppVvqqyh25dobAAAAAAC4o/Ly8nQ1O8uqx6hu7yAbm6LHEfXo0UPjx4+3fN+6dase
eOABbdu2TX379pUk7dmzRwsWLFC/fv1KPN73339/+0X/n6ysLDk4OBS5fcOGDeV2LGuZOnVqob8H
BgYqMDDwtvo+ceKEIiIiijzGnZKVlaX58+ffVh/l+dz8twsXLmjhwoV68803C92elZUlV1dX/fTT
T7d8jIEDB+qjjz665f1LwkhAAAAAAAAqsavZWWqx9jWrfkoKGbt166bk5GTFxsZKkn744Qe9/PLL
2r17tyQpPj5eSUlJysjIkJeXV4nn5OTkpD179kiSpkyZIjc3N8voqOPHj0uSIiMj5e3tLU9PT/n7
++vgwYOSpM2bN8vd3V1DhgyRl5eXVq1aJScnJ02cOFF+fn5q1KiRpkyZYjlWQECAVq1aJUl67733
1KxZM3l5ecnDw0Pbt28vssYbxxk4cKDc3d3l7e1tqVmSpk+fLnd3d3l4eKh///46f/68JCkzM1NP
P/20fH195eXlpT59+ig5OVmSdP78eQ0dOlTu7u5q3ry5hgwZIun6KLFRo0YVWsON63ljZOULL7wg
b29vubi46OOPP7a0Lep6TZgwQfHx8fLy8lK3bt0KXH9J8vHxsYzqLOp+FCYzM1MhISFq0qSJWrZs
qbCwMAUEBBR5nwYNGqRZs2ZJktasWSNPT095eXnJ3d3dco9OnTqlPn36yNfXVx4eHpowYYLleDfq
/uabb+Th4ZGvlpvvc2RkpFq3bi1vb2/5+vpaRoLeqCkkJETNmzeXu7u7vvvuO0lSWFiY0tPT5eXl
JR8fH0ufoaGh8vPzU5cuXfKNbr1y5Yr69OmjZs2aqXnz5urYsWOR1+lOIQQEAAAAAAC3pUqVKmrV
qpW++eYbXbt2TWfOnNGwYcOUlJSkq1ev6quvvpKfn5+qVq1apn6Tk5O1ZMkSHTp0SNHR0Tpw4ICc
nZ115swZhYWFKSIiQjExMRo5cqSGDRumvLw8SdLvv/+u0NBQRUdHa/To0ZKk1NRURUVF6cCBA1q8
eLF+//33Asd79dVXtW3bNkVHR+vIkSNq3bp1sfXFxcUpNDRUsbGxCg8P1xNPPKG8vDxt2LBBa9eu
1Y8//qgTJ06oWrVqev755yVJM2fOVPXq1XX48GFFR0fL29tbkyZNkiSNGzdODg4OOn78uI4fP65/
/vOfZbpeV65cUcuWLXXs2DG99957ltF9xV2vBQsWqGnTpoqOji429CzufhTlvffe0++//66YmBjt
379fx44dy7e9sPt0w+uvv65FixYpOjpax48fV69evSRJTzzxhJ599lkdPnxYR48eVVRUlFasWJFv
34cfflhZWVmWAO/YsWM6efKkhg0bpmPHjmn27NnaunWrjh07pnXr1mnUqFG6du2apOuB9ahRo3T8
+HGNGTNGr7zyiiRp+fLlqlatmqKjo3X06FHLseLi4rRv3z7t3bs3Xw0bN27U5cuXFRcXp+PHj+vT
Tz8t9tpK0r///e9804E//PDDEvcpC6YDAwAAAABQiVW3d9Bvj79m9WOUpHPnztq5c6dcXV3VsmVL
SVKrVq20fft2fffdd+rUqVOZj1u3bl01adJEwcHB6t69uwYNGiR3d3d99tln8vT01IMPPihJeuaZ
ZzR16lRLsOfs7KxHHnkkX19PPvmkpOujxRo3bqyYmBi5ubnla9OhQwc99thj6t27twYMGGA5j6I4
OTlpwIABkq6PFHvxxRcVGxurLVu2aMCAAapfv76k66PtHnvsMUnSl19+qbS0NMvIuuzsbDVq1EiS
tG3bNu3evVu2traSZPm9tBwdHTVixAhJ10dnnjlzRtL1dwcWd71Kq6j7UZSdO3dq+PDhcnR0lCSF
hIRo5cqVlu2F3acbOnfurBdffFH9+/fXI488og4dOujy5cvau3evJk2aZAlO09PTFR0dXWD/xx57
TMuXL1dgYKCWLVumRx99VA4ODvr888916tQpdejQwdLWZDLpxIkTkqTGjRtbRkR26dJFH3zwQbHX
5LHHHrOc3838/f3197//XSEhIQoMDNTgwYOL7Uey/nRgQkAAAAAAACoxGxsb1XCsUtFl6KGHHtLI
kSPVuHFjdenSRdL1IGfr1q368ccftWzZMmVnZ5epTzs7O0VFRenbb7/Vtm3b1LFjx3whUlGqVatW
4LebRyHa2NgoJyenQJuvv/5au3bt0tatW9WvXz9Nnz5dY8eOLXW9JpNJJpOp0N9vMJvNeu+99xQc
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
Z8/K3d1d1atXr+hyAFSwq1evKjY2Vk5OTqpfv36R7RhDDAAAAABAJZaXl6fca5et+snLyyu2hvDw
cA0dOtTy/euvv5bJZNLmzZstvz3++OPq0qWL+vTpI0k6fvy4atSoYdluMpmUkpIiSerSpYuioqLK
8SoVbejQofriiy/uyLFu1VtvvWV5Ddr8+fPVo0cPSdJ3331nuZ636vjx43rrrbduu8byMHHiRC1a
tOiW97fmc5OSkmKZmVqUkydPqk2bNmXu+/jx46pataoyMjIsv7m4uCg4ONjyfdu2bbrvvvvK3PfN
rLowCAAAAAAAsC5z5hXFPV3HqsdotviiVLVmkdt79Oih8ePHW75v3bpVDzzwgLZt26a+fftKkvbs
2aMFCxaoX79+JR7v+++/v/2i/09WVpYcHByK3L5hw4ZyO5a1TJ06tdDfAwMDFRgYeFt9nzhxQhER
EUUe407JysrS/Pnzb6uP8nxu/tuFCxe0cOFCvfnmm4Vuz8rKkqurq3766acy9928eXPVq1dPO3fu
VK9evXTixAlVr15dP//8s6XNt99+q/bt299y/RIjAQEAAAAAwG3q1q2bkpOTFRcXJ0n64Ycf9PLL
L2v37t2SpPj4eCUlJSkjI0NeXl4l9ufk5KQ9e/ZIkqZMmSI3Nzd5eXnJy8tLx48flyRFRkbK29tb
np6e8vf318GDByVJmzdvlru7u4YMGSIvLy+tWrVKTk5Omjhxovz8/NSoUSNNmTLFcqyAgACtWrVK
kjRv3jw1a9ZMXl5e8vDw0Pbt24us8cZxBg4cKHd3d3l7e1tqlqTp06fL3d1dHh4e6t+/v86fPy9J
yszM1NNPPy1fX195eXmpT58+Sk5OliSdP39eQ4cOlbu7u5o3b64hQ4ZIkl588UWNGjWq0BpuXM8b
IytfeOEFeXt7y8XFRR9//LGlbVHXa8KECYqPj5eXl5e6detW4PpLko+Pj2VUZ1H3ozCZmZkKCQlR
kyZN1LJlS4WFhSkgIKDI+xQcHKxZs2ZJktasWSNPT095eXnJ3d3dco8SEhLUp08f+fr6ysPDQxMm
TLAc70bdW7ZskYeHR75abr7PkZGRat26tby9veXr62sZCXqjppCQEDVv3lzu7u767rvvJF1f2yI9
PV1eXl7y8fGx9BkaGio/Pz916dIl3+jWK1euqE+fPmrWrJmaN2+ujh07FnmdJKl9+/batm2bJGnL
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
WKdOnbJsO3jwoGbMmKFvvvlGMTExateunUaMGGHZfvjwYb3zzjuKiYlRYGCg3njjDUkq8l61b99e
I0aM0KBBgxQdHa133nlHtra2ioyM1JEjRyzT2efOnVuq8/8rYCQgAAAAAACVmIOjrV6f87DVj1GS
Ll26aOfOnXJ1dZWfn58kqXXr1tq+fbt27tx5S9NW69SpoyZNmmjQoEF66KGHNGjQILm5uenzzz+X
p6enAgICJEnjx4/XlClTFB8fL0lydnZW79698/X15JNPSpLuu+8+OTs768SJE3J1dc3Xpn379nrs
scfUp08fDRgwQL6+vsXW5+TkpP79+0uSRo0apYkTJyouLk5btmzRwIED9be//U2SNGHCBA0fPlyS
9J///EdpaWn64osvJF2frnwjSNu2bZv27NkjW1tbS/9l4ejoaDnPoKAgnT59WpL0/fffF3u9Squo
+1Ean376qX777Te1atXK8ltqaqquXr0qSWrVqpVl28SJEzV37lzl5OTk62Pr1q169NFHVadOHUnS
008/rQMHDkiSvvnmGwUFBVnu6aRJkzRv3jxLH35+fvLy8pIkdezYUQsXLpSkYu/VfzObzZozZ462
bNmi3NxcpaWlqU2bNqU6/78CRgICAAAAAFCJ2djYqEpVe6t+SrP6aI8ePbR7925t3bpVgYGBkq4H
g1u3btWePXvUs2fPMp+bnZ2doqKiFB4ernPnzql9+/b65puSpz5Xq1atwG83j0K0tbUtEDBJ0tdf
f6233npL2dnZeuSRR7R8+fIy1WsymQq9Vjcv3pCXl6d58+YpOjpa0dHRiouLK7d3INrb//97ZWdn
p9zcW3tXpK2tbb59MzMzLX3eyv2QrgdoQ4cOtZx3dHS0UlJSVL169VuqUVKxi2L897YqVapY/mxj
Y1Po/S+pz2XLlun777/Xnj17FBMTo+eee85ybSoDQkAAAAAAAHDbOnfurAsXLigyMlIPPfSQJOmh
hx7SZ599ppSUFHXp0qXMfV66dElnzpzRww8/rHfeeUf+/v46ePCgAgMDFRMTo4MHD0qSli9frgYN
Gqhp06a3XH92drZ+++03de7cWbNmzVK/fv20b9++Yvc5e/asNm/eLElauXKl6tWrJ1dXV/Xs2VOf
ffaZLl68KEn64IMPLMFo37599f777ystLU2SlJaWpp9++kmS1LNnT82ZM8cSwJ09e/aWz+dmxV2v
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
AAAwuP8HYCNBMwmSqr4AAAAASUVORK5CYII=
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-9f2e8411-921d-467a-8252-0d93b632bda8");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-9f2e8411-921d-467a-8252-0d93b632bda8"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-da42c070-eac5-4a59-b63b-000e44be9494">
        <img style="width: 180px;" src="data:image/png;base64,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
ny+z2azIyEgdOnRIbdq0UWBgoCIjI3XgwAGZTCYtXLhQVapUkYuLiyTp8uXL+u6772QymRQWFiZH
R0e5uLioTp06unLliv78809J0pIlSzL1azKZdO3aNTk4OKhKlSpKTEzUokWLLPsrVKigpKQkJSYm
Zllz06ZNNXv2bJnNZl25ckU//PCDnn322UK4QyhuNsVdAAAAAAAAgFGkpaWpXr16SkhI0NSpUy0L
eyxcuFADBgxQamqqypcvr5UrV8rK6s7YrFq1aiksLEzBwcGytbXVsmXLZGVlpWrVqunll19W48aN
9fjjj6tNmzZZ9vnCCy9o+fLlcnV1VYUKFdSyZUtdvXpVJpNJlStXVpcuXVS3bl2VKVNGx44dk/R/
I/7mzZunoUOHWuocO3asAgMDi+ReoWiZzHejagMLDg7WzJkzi7sMAAAAAADy7fLly4qOjparq6sc
HByKuxzoTvBnZWUlKysrRUVFqXLlyvc9x2w2y2w26/vvv9f48eMVERFRBJXCCO6N8kwmk27duqXT
p0/Lyckpx2eP6cAAAAAAAACAwRECAgAAAAAA5IO1tbVMJpPMZnOuRgFKd0ZwWVlZqUOHDowCRJEg
BAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAA
AORLcHCwevToYfm+ceNGmUwmbdiwwbLtxRdfVMuWLRUUFCRJOnnypMqWLWvZbzKZFB0dLUlq2bKl
wsPDi6T2Hj166LvvviuSvh7U9OnT9dZbb0mSZs2apbZt20qSdu7cabmfD+rkyZOaPn16vmssCKNH
j9bcuXMf+PzCfG6io6M1ceLEHI85d+6cfH19H6j9sWPHqmLFinJ3d7d8Xn/99QdqKzs2BdoaAAAA
AAAoUunp6bqVklyofTjY2snKKvtxRG3bttWIESMs37ds2aKnnnpKW7duVYcOHSRJe/fu1ezZs9Wx
Y8f79rdr1678F/2/kpOTZWdnl+3+1atXF1hfheWNN97IcntAQIACAgLy1fapU6cUFhaWbR9FJTk5
WbNmzcpXGwX53PxdTEyM5syZo2nTpmW5Pzk5WS4uLjp8+PAD99GlSxctXrz4gc+/H0YCAgAAAABQ
gt1KSVbdFf8u1M/9QsbWrVsrKipKp0+fliT99NNPevPNN7Vnzx5J0vnz53X16lUlJibK3d39vtfk
5OSkvXv3SpImTJigmjVrWkZHnTx5UpK0Zs0aeXh4yM3NTY0aNdKhQ4ckSRs2bJCrq6u6d+8ud3d3
LV26VE5OTho9erR8fHxUtWpVTZgwwdKXn5+fli5dKkn66KOPVKtWLbm7u6t27dratm1btjXe7adL
ly5ydXWVh4eHpWZJeuutt+Tq6qratWurU6dOunbtmiQpKSlJL7/8sry8vOTu7q6goCBFRUVJkq5d
u6YePXrI1dVVderUUffu3SXdGSU2aNCgLGu4ez/vjqwcM2aMPDw8VK1aNa1atcpybHb3a9SoUTp/
/rzc3d3VunXrTPdfkjw9PS2jOrP7PbKSlJSkvn37qnr16vL29taQIUPk5+eX7e/0wgsvaPLkyZKk
5cuXy83NTe7u7nJ1dbX8RhcuXFBQUJC8vLxUu3ZtjRo1ytLf3bo3bdqk2rVrZ6jl3t95zZo1atCg
gTw8POTl5WUZCXq3pr59+6pOnTpydXXVzp07JUlDhgxRQkKC3N3d5enpaWlzwIAB8vHxUcuWLTOM
bo2Pj1dQUJBq1aqlOnXqqFmzZtnep6JCCAgAAAAAAPKlVKlSql+/vjZt2qTbt2/r0qVL6tmzp65e
vapbt27phx9+kI+Pj0qXLp2ndqOiojR//nwdOXJEEREROnjwoJydnXXp0iUNGTJEYWFhioyM1MCB
A9WzZ0+lp6dLks6ePasBAwYoIiJCgwcPliTduHFD4eHhOnjwoObNm6ezZ89m6u+dd97R1q1bFRER
oWPHjqlBgwY51nfmzBkNGDBAp0+fVnBwsPr06aP09HStXr1aK1as0M8//6xTp06pTJkyeu211yRJ
ISEhcnBw0NGjRxURESEPDw+NGzdOkjR8+HDZ2dnp5MmTOnnypD755JM83a/4+Hh5e3vrxIkT+uij
jyyj+3K6X7Nnz1aNGjUUERGRY+iZ0++RnY8++khnz55VZGSkDhw4oBMnTmTYn9XvdNe7776ruXPn
KiIiQidPntSzzz4rSerTp49eeeUVHT16VMePH1d4eLhCQ0MznPvMM88oOTnZEuCdOHFC586dU8+e
PXXixAlNmTJFW7Zs0YkTJ/Tll19q0KBBun37tqQ7gfWgQYN08uRJDR06VJMmTZIkLVq0SGXKlFFE
RISOHz9u6evMmTPav3+/9u3bl6GGtWvX6ubNmzpz5oxOnjypdevW5XhvJenrr7/OMB144cKF9z0n
L5gODAAAAABACeZga6ffX/x3ofdxPy1atNCOHTvk4uIib29vSVL9+vW1bds27dy5U82bN89zv5Uq
VVL16tXVtWtXtWnTRi+88IJcXV21fv16ubm5qXHjxpKkkSNH6o033rAEe87OznruuecytPXSSy9J
ujNa7Mknn1RkZKRq1qyZ4ZimTZuqd+/eat++vTp37my5juw4OTmpc+fOku6MFBs7dqxOnz6tH3/8
UZ07d1blypUl3Rlt17t3b0nS999/r7i4OMvIupSUFFWtWlWStHXrVu3Zs0fW1taSZNmeW/b29urf
v7+kO6MzL126JOnOuwNzul+5ld3vkZ0dO3aoV69esre3lyT17dtXS5YssezP6ne6q0WLFho7dqw6
deqk5557Tk2bNtXNmze1b98+jRs3zhKcJiQkKCIiItP5vXv31qJFixQQEKDPPvtMzz//vOzs7PTN
N9/owoULatq0qeVYk8mkU6dOSZKefPJJy4jIli1b6tNPP83xnvTu3dtyffdq1KiR/vWvf6lv374K
CAhQt27dcmxHKvzpwISAAAAAAACUYFZWViprX6q4y9DTTz+tgQMH6sknn1TLli0l3QlytmzZop9/
/lmfffaZUlJS8tSmjY2NwsPDtXnzZm3dulXNmjXLECJlp0yZMpm23TsK0crKSqmpqZmO2bhxo3bv
3q0tW7aoY8eOeuuttzRs2LBc12symWQymbLcfpfZbNZHH32krl275rrd3LK1tbW8u9Ha2lppaWkP
1I6NjU2G+5OUlGTZntXv0b59+wfqJ6vf6a5Fixbp4MGD+vHHHzVw4EB169bNMo378OHDcnBwyLHt
4cOHq0GDBoqPj9dXX32l9evXS7pz/5s3b57lYjAXL17MEOhZW1tn+Zzc697Fbe7l4eGh33//XRs2
bNCWLVv0zjvvKDw8XP/4xz9ybK8wMR0YAAAAAADkW8uWLRUTE6O1a9eqXbt2ku4Eg+vXr1d0dLQC
AwPz3GZsbKz++OMPtW/fXjNmzFDDhg116NAhBQYGWqaYStJnn32mKlWqZBrZlxfJyck6ceKEAgIC
9N577+m5556ztJ+dy5cvW8KkxYsXy9HRUbVq1VK7du307bffKiYmRpL06aefWoLRoKAgzZo1S3Fx
cZKkuLg4HTx4UNKdBVamTp1qCe/+/PPPB76ee+V0v8qXL2+p5a7q1atb3ue4fft2nT9/XlL2v0d2
AgICtHr1aiUlJSkpKUkrVqzIdc2//vqrGjVqpEmTJmnQoEE6ePCgKlSoID8/P8tKydKdFXnvvovy
Xi4uLvLy8tKwYcPk6OioRo0aSZI6deqkPXv2ZJi+u3379vvWU6FCBSUlJSkxMTFX9Z8+fVpWVlbq
27ev5s+fL7PZnOeRlwWNkYAAAAAAACDf7O3t5evrq4iICNWvX1+S5O3trYSEBPn6+mY5ZfJ+rl27
pq5duyohIUEmk0kuLi4aOXKkHB0dtXDhQg0YMECpqakqX768Vq5cmeMKxveTlpamAQMG6Pr167Kx
sVGlSpX0xRdf5HhOrVq1FBYWpuDgYNna2mrZsmWysrJSjx49dOTIEfn5+clkMqlu3boKCwuTJE2Z
MkUTJkxQgwYNLCMEx4wZo0aNGmnBggUaPny46tSpIxsbG3l7e2dY3ONBVa1aNdv71bhxY9WuXVuu
rq6qVq2atm3bpilTpmjw4MFasmSJfH19LVN+s/s9sjN+/HgdO3ZMbm5uKleunLy9vXX16tVc1Xx3
urKtra1KlSqlefPmSbqzmvMrr7wiV1dXmUwmlSlTRvPnz89yWnL//v01ZMgQvf/++5Zt9erVU2ho
qEaMGKHbt28rJSVFnp6eatWqVY71VKlSRV26dFHdunVVpkyZDO8FzMovv/yikJAQmc1mpaWlqWvX
rvL398/xnK+//jrDgizNmjXL9L7D/DCZzWZzgbX2kAoODtbMmTOLuwwAAAAAAPLt8uXLio6Olqur
632nRKLwbNiwQePHj8/yfXT4P7GxsapYsaKSkpLUpUsX+fj4aOrUqcVdVol2b5RnMpl069YtnT59
Wk5OTpb3UGaFkYAAAAAAAAAoFAEBAUpOTlZSUpIaNWqkN998s7hLemQRAgIAAAAAAGTD09Mz0wIb
bm5u+vbbb9WhQ4diqurhcenSJbVt2zbT9oCAAC1YsEBHjhwphqoeTqtWrdI777yTafu4ceM0dOjQ
Qu+fEBAAAAAAACAb93v326PO2dmZKdG51LNnT/Xs2bPY+md1YAAAAAAAAMDgGAkIAAAAAEAJYm1t
bfnzI7DWJ4ACwkhAAAAAAABKECsr/lMewJ2VgfOCvzkAAAAAAACAEiSvAaDEdGAAAAAAAEqsBwkC
ADyaGAkIAAAAAEAJlp6errTbNwv1k56enmMNwcHB6tGjh+X7xo0bZTKZtGHDBsu2F198US1btlRQ
UJAk6eTJkypbtqxlv8lkUnR0tCSpZcuWCg8PL8C7lL0ePXrou+++K5K+HtT06dP11ltvSZJmzZql
tm3bSpJ27txpuZ8P6uTJk5o+fXq+aywIo0eP1ty5cx/4/MJ8bqKjozVx4sQcjzl37px8fX3z3PbJ
kydVunRpJSYmWrZVq1ZNXbt2tXzfunWrnnjiiTy3fS9GAgIAAAAAUIKZk+J15uWKhdpHrXmxUuly
2e5v27atRowYYfm+ZcsWPfXUU9q6das6dOggSdq7d69mz56tjh073re/Xbt25b/o/5WcnCw7O7ts
969evbrA+iosb7zxRpbbAwICFBAQkK+2T506pbCwsGz7KCrJycmaNWtWvtooyOfm72JiYjRnzhxN
mzYty/3JyclycXHR4cOH89x2nTp15OjoqB07dujZZ5/VqVOn5ODgoF9++cVyzObNm9WkSZMHrl9i
JCAAAAAAAMin1q1bKyoqSmfOnJEk/fTTT3rzzTe1Z88eSdL58+d19epVJSYmyt3d/b7tOTk5ae/e
vZKkCRMmqGbNmnJ3d5e7u7tOnjwpSVqzZo08PDzk5uamRo0a6dChQ5KkDRs2yNXVVd27d5e7u7uW
Ll0qJycnjR49Wj4+PqpataomTJhg6cvPz09Lly6VJM2cOVO1atWSu7u7ateurW3btmVb491+unTp
IldXV3l4eFhqlqS33npLrq6uql27tjp16qRr165JkpKSkvTyyy/Ly8tL7u7uCgoKUlRUlCTp2rVr
6tGjh1xdXVWnTh11795dkjR27FgNGjQoyxru3s+7IyvHjBkjDw8PVatWTatWrbIcm939GjVqlM6f
Py93d3e1bt060/2XJE9PT8uozux+j6wkJSWpb9++ql69ury9vTVkyBD5+fll+zt17dpVkydPliQt
X75cbm5ucnd3l6urq+U3unjxooKCguTl5aXatWtr1KhRlv7u1v3jjz+qdu3aGWq593des2aNGjRo
IA8PD3l5eVlGgt6tqW/fvqpTp45cXV21c+dOSdKQIUOUkJAgd3d3eXp6WtocMGCAfHx81LJlywyj
W+Pj4xUUFKRatWqpTp06atasWbb3SZKaNGmirVu3SpJ+/PFHBQYGytHR0XJ/f/rpp3wHvowEBAAA
AACgBDPZP3ZnpF4h95GTUqVKqX79+tq4caMGDRqkS5cuqWfPnpowYYJu3bqljRs3ysfHR6VLl85T
v1FRUZo/f76uXLmixx57THFxcbKystKlS5c0ZMgQbdq0SY0bN9bcuXPVs2dPnTp1SpJ09uxZzZo1
S88995wk6e2339aNGzcUHh6uy5cvy83NTS+//LJq1qyZob+QkBAdO3ZMNWrUUFJSkm7fvp1jfWfO
nNFHH32kzp07a9GiRerTp4/OnDmjNWvWaMWKFdq/f78qV66sXr166bXXXtPy5csVEhIiBwcHHT16
VJI0fvx4jRs3TkuXLtXw4cNVqlQpnTx5UtbW1vrzzz/zdL/i4+Pl7e2tTz75RF999ZUmTJignj17
5ni/Zs+erfHjxysiIuKBf4/sfPTRRzp79qwiIyMlSa1atcqw/++/0/fff2/Z9+6772ru3Llq27at
0tLSFBMTI+nOtPKJEyfqueeeU3Jystq0aaPQ0FANHjzYcm67du2UnJysnTt3KiAgQCdOnNC5c+fU
s2dPnThxQlOmTNG2bdtUqVIlHTt2TK1atdLFixcl3QmsP/vsMy1btkzTp0/XpEmT9NNPP2nRokVq
2LBhpvt05swZ7d+/X/b29hkC0XXr1unmzZuWYPyvv/7K8d62atVKX3zxhSRpx44d6tGjh2xtbfXD
Dz+oRo0a+uWXX7R48eIc27gfRgICAAAAAFCCWVlZybp0uUL95BT03NWiRQvt2LFD27dvl7e3tySp
fv362rZtm3bs2KHmzZvn+doqVaqk6tWrq2vXrvrggw8UFRUlBwcH7dy5U25ubmrcuLEkaeTIkYqK
itK5c+ckSc7OzpZg6a6XXnpJ0p3RYk8++aQlmLpX06ZN1bt3b7377ruKiIhQhQoVcqzPyclJnTt3
lnRnpNh///tfnT59Wj/++KM6d+6sypUrS7oz2m737t2S7gRda9assYyk+/rrr3XhwgVJd9779uab
b8ra2lqSVLVq1TzdL3t7e/Xv31/SndGZly5dkqT73q/cyu73yM6OHTvUu3dv2dvby97eXn379s2w
P6vf6a4WLVpo7NixmjRpkiVMvXnzpvbt26dx48bJ3d1dTz31lC5cuJBlgNm7d28tWrRIkvTZZ5+p
S5cusrOz0zfffKMLFy6oadOmcnd3V7du3WQymXT69GlJ0pNPPmkZEdmyZUtLOJidu9f3dw0bNtSZ
M2fUt29fLVy4MMcp6ZL07LPPKjw8XImJiTpw4ICefvpptW7dWrt27dKuXbvk6OgoNze3HNu4H0JA
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
/rrWrFkjOzs7SdK3334rNzc3rVu3Tm+99ZZSU1NVvnx5LViwQA0aNND333+v1157Tb6+vvrtt9/0
xhtv6K233lL37t21Y8cORUdHq0+fPvrggw8kSX5+fnrttdfUt29fffzxx5ozZ45sbW2Vnp6uBQsW
qFWrVlnW+P3332v06NHy8vLS0aNHZWdnp0WLFqlJkyaSpJCQEK1YsUJWVlaqW7euFi9eLEdHRyUl
JSk4OFi7d+9WcnKyatasqc8//1yVK1fWtWvXNHLkSP3yyy+ysrKSt7e3Vq9erXHjxun69esKDQ3N
VENwcLAiIiJ08uRJNWzYUEOGDNGPP/6ouLg4ffTRR+revbskZXu/XnnlFV25ckXu7u6qWrWqtm7d
muH+S1K9evX0wQcfKCgoKNvfIzfmzZunefPmKTU1VWXKlNGcOXPk7++v2bNna/ny5XJwcND58+dV
sWJFLV++XHXq1NHs2bP17bffavPmzZKkcePGac2aNXrsscfUtm3bTO1//PHHkiQnJyeFhYXJxcVF
s2fP1pdffilHR0edPHlSdnZ2+uqrr1S3bt1sf6vIyEh9/vnnSktL0549e9SxY0dNnTpVrVu3Vmxs
rBITE+Xh4aFly5apXLlyubr+4sZ0YAAAAAAAkC+lSpVSgwYNtGnTJt2+fVt//PGHunfvrqtXryoh
IUEbN26Uj4+PSpcunad2o6OjNW/ePB09elQRERE6ePCgnJ2d9eeff2rQoEFasmSJIiMjNXjwYHXv
3l3p6emSpLNnz2rAgAGKiIjQwIEDJUk3btxQeHi4Dh06pHnz5uncuXOZ+gsJCdG2bdsUERGho0eP
qkGDBjnWd/r0aQ0YMECnTp3SuHHj9OKLLyo9PV1r1qzRsmXLtG/fPkVGRqpMmTIaPXq0JOnf//63
ypQpoyNHjigiIkKenp4aN26cJGn48OGys7OzhHqffPJJnu5XfHy8fHx8dPz4cX388cd6/fXXJSnH
+/Xpp5+qRo0aioiI0NatWx/o98iNzZs3a+XKldq/f79OnDih//mf/1Hfvn0t+3/55RfNmDFDZ86c
Ufv27TV48OBMbaxatUrffPONwsPDdfToUV24cMGy79ChQ3r77be1adMmRUZGyt/fX/3797fsP3r0
qD788ENFRkYqICBA7733niRl+1s1adJE/fv31wsvvKCIiAh9+OGHsra21po1a3Ts2DHLdPb3338/
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
fNd8V2pqao4LJixatKjA+rrr3XffLfA28+tBa0pLSyuUe4Six0hAAAAAAACQL/7+/rp8+bIuXbok
SdqxY4dCQkIsIwGvXLmiixcvKikpST4+Pvdtr0aNGgoPD5ckTZkyRXXr1pWPj498fHx04cIFSdKm
TZvUoEEDPfXUUwoICNCJEycsfXt6emrw4MHy8fHR119/rRo1aigkJERNmjSRi4uLpkyZYukrMDBQ
69evl3QnEPTw8JCPj4+8vLy0f//+bGt8+umntWbNGsv3HTt2qH79+pKkAQMG6JNPPpEkxcXFqWfP
nqpTp46aN2+uo0ePWs75++i4DRs2WELTq1evqlWrVvL19ZWnp6deffVVpaen53jfAgMDNX78eLVo
0UK1atXSiBEjLPuyqsnd3V0tWrTQ8OHDLaMTlyxZolatWqlr167y8vLSgQMHMtyjK1euqF27dvLw
8FC7du3Uq1cv/fvf/5Yk/fvf/9aYMWMsfWY36vFBrg35x0hAAAAAAABKMFNpW/3j0OhC7yMndnZ2
atq0qbZv364ePXro3LlzCgoK0muvvabExERt375dTZo0UalSpfLUb2xsrGbMmKErV66odOnSSkhI
kJWVlaKiovTiiy9qx44d8vLy0vLly9WtWzcdP35ckvT7779r7ty5Cg0NlSRNmDBB169f188//6z/
/ve/qlWrlgYOHKiqVatm6G/cuHGKiIjQE088oZSUFCUlJWVb28CBA7VkyRJ169ZNkhQWFpZhNORd
7777ruzt7RUREaGbN2/K399fjRs3vu+1V6hQQd99950ee+wxpaWlqXPnzlq9erV69eqV43lnzpzR
9u3blZKSIg8PD/38889q0qRJpppKly6t33//XfHx8WratKl8fX0t+/fv369ff/1VderUydT+a6+9
piZNmmjy5Mm6evWqfHx85O7uft/rKYhrQ/4wEhAAAAAAgBLMZDLJqoxdoX5ys/BAq1attGPHDu3f
v19+fn6S7owQ/Pnnn7Vjx45cTQP+u3Llyql27drq27evFixYoJiYGJUqVUr79++Xl5eXvLy8JEl9
+vTR5cuX9eeff0qSatasqYCAgAxtvfjii5Kkxx9/XDVr1tS5c+cy9demTRu99NJLmjVrls6dO6fH
Hnss29q6dOmiffv26cqVK4qPj9eGDRssfdxr69atGjx4sEwmk8qXL5/lMVlJT0/XG2+8IW9vb9Wv
X1+HDh2yjI7MSc+ePWVjY6PSpUvLx8dHZ86cybKmgQMHymQyqWzZspb3NN7VtGnTLAPAu+feDTv/
+c9/qkOHDrm6noK4NuQPISAAAAAAAMi3Vq1aafv27dq+fbtlSmtAQIBlW+vWrfPcprW1tfbt26cx
Y8YoKipK/v7+2r17933Pyyq8u3cUorW1tVJTUzMds3btWr3//vtKSUlRUFCQVq5cmW0fpUuXVvfu
3bV06VJ99dVXat26tRwdHe9b272Bqo2NjdLS0izfExMTLX+eOXOmoqKitH//fh05ckQvvvhihv3Z
yc115lSTlPX9y825OV3PvR702pA/hIAAAAAAACDfGjVqpKioKC1fvjxDCLhy5UpduXLFMjowL+Li
4vTXX3+pRYsWevvtt9W8eXP9+uuv8vf319GjR3Xs2DFJ0sqVK1W1atVM03vzIjU1VWfOnFHDhg01
fvx4devWTQcOHMjxnIEDByosLExLlizJciqwJLVt21ZhYWEym826efOmvvzyS8s+V1dXHTlyRLdv
31ZqaqpWrFhh2RcbG6t//vOfKlWqlK5evaqvvvrqga/t71q3bq3PP/9cZrNZ8fHxWr16dZ7OXbJk
iSTpr7/+0oYNGzJcz6FDh5SWlqaEhAStXbs2yzYK89qQPd4JCAAAAAAA8s3W1lbNmzfXb7/9ZnlH
nJubm+Li4tS8eXPZ2ub8XsGs3LhxQ926ddOtW7dkMplUu3Zt9e/fX+XLl9fy5cvVr18/paamqmLF
ivrqq69yNW05O2lpaRo0aJBiYmJkY2OjypUrKywsLMdz/Pz8ZG1trdOnT6tdu3ZZHvP2229ryJAh
cnd3V+XKldW8eXPLuwb9/f0VFBSkevXq6YknnlCzZs0si5GMHj1a3bp1k6enp5ycnNS2bdsHvra/
CwkJ0eDBg1W3bl09/vjj8vb2VoUKFXJ17qxZs9S/f395eHjIyclJjRs3tpz7wgsv6KuvvlLdunXl
7Oys+vXrKyEhIVMbhXltyJ7JbDabi7uIwhYcHKyZM2cWdxkAAAAAAORbYmKizp07JxcXlzwvtAFI
UkpKitLS0lSqVCndunVLzzzzjEaNGpXp3YBZuX37tmxtbWVjY6Nr167J399fy5Yty9ViJygcuf07
gZGAAAAAAAAAj5DY2Fi1b99eaWlpSkxMVOfOndWjR49cnXvq1Cn169dPZrNZycnJGjlyJAFgCUEI
CAAAAAAAkI2GDRtmWlzD09NTy5cvL5Z6Fi1apDlz5mTaPnv2bLVo0SJXbfzjH//Q4cOHH6j/p556
ipV8SyhCQAAAAAAASqD09PTiLuGRcOjQoeIuIYMhQ4ZoyJAhxV0GHiK5fdMfISAAAAAAACWInZ2d
rKysdPnyZVWuXFl2dnb5WhADQMllNpsVHR0tk8l038V3CAEBAAAAAChBrKys5OLioitXrujy5cvF
XQ6AYmYymeTs7Cxra+scjyMEBAAAAACghLGzs1O1atWUmpqqtLS04i4HQDGytbW9bwAoEQICAAAA
AFAi3Z3+d78pgAAgSVbFXQAAAAAAAACAwkUICAAAAAAAABgcISAAAAAAAABgcISAAAAAAAAAgMER
AgIAAAAAAAAGRwgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAAAACAwRECAgAAAAAAAAZHCAgAAAAA
AAAYHCEgAAAAAAAAYHCEgAAAAAAAAIDBFWoIeO3aNfn4+Fg+bm5usrGxUUxMTKZjP/zwQ9WrV08e
Hh7q0qWLrl+/btm3f/9+eXt7y83NTa1bt9aff/5ZmGUDAAAAAAAAhlKoIaCjo6PCw8Mtn2HDhql9
+/aqVKlShuM2b96ssLAw/fzzzzpx4oR8fX01adIkSVJ6err69OmjTz75RJGRkQoKCtKYMWMKs2wA
AAAAAADAUIp0OnBoaKgGDx6caftvv/2m5s2bq2zZspKkoKAgLV26VJJ0+PBh2djYqFWrVpKk4cOH
67vvvlNiYmKWfSQlJenmzZsZPmlpaYV0RQAAAAAAAMDDr8hCwL179yo2NlYdOnTItM/X11dbtmzR
1atXZTabtXz5csXFxSkmJkYXL15U9erVLceWLVtW5cqV0+XLl7PsZ9q0aSpfvnyGz4EDBwrtugAA
AAAAAICHXZGFgKGhoerXr59sbGwy7WvVqpXGjx+vDh06yN/fX5UrV5akLI+9n4kTJ+rGjRsZPn5+
fvmuHwAAAAAAACip8p6yPYD4+HitXr1aBw8ezPaYkSNHauTIkZKkffv2ydnZWeXKlVO1atV04cIF
y3FxcXG6ceOGnJycsmzH3t5e9vb2GbZZW1sXwFUAAAAAAAAAJVORjARctWqVvL295e7unu0xV65c
kSQlJCQoJCREr7/+uqQ7U4VTUlK0fft2SdKCBQvUsWNHlSpVqvALBwAAAAAAAAygSEYChoaGaujQ
oRm2hYSEyMnJSSNGjJAktWvXTunp6UpOTtZLL72kV199VZJkZWWlZcuWafjw4UpMTJSTk5Nl0RAA
AAAAAAAA92cym83m4i6isAUHB2vmzJnFXQYAAAAAAABQLIpsYRAAAAAAAAAAxYMQEAAAAAAAADA4
QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAA
AAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4Q
EAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAA
AMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQE
AAAAAAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAA
MDhCQAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEA
AAAAAACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAM
jhAQAAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAA
AAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMj
BAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAA
AAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgB
AQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAA
AAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAA
AAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAAAAAA
gyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAA
AAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDg
CjUEvHbtmnx8fCwfNzc32djYKCYmJtOx06dPl4eHh3x8fOTv768DBw5Y9plMJnl5eVna2b17d2GW
DQAAAAAAABiKTWE27ujoqPDwcMv3GTNmaOfOnapUqVKG48LDwzV37lwdP35cjz32mJYtW6ZXX301
QxC4e/duVahQoTDLBQAAAAAAAAypUEPAvwsNDdW0adMybTeZTEpJSdGtW7f02GOP6fr163J2dn6g
PpKSkpSUlJRhW1pa2gO1BQAAAAAAABhBkYWAe/fuVWxsrDp06JBpn7e3t8aOHSsXFxdVqlRJ9vb2
2rVrV4Zj2rRpo9TUVLVp00bvvfeeHBwcsuxn2rRpmjx5coZt/v7+BXchAAAAAAAAQAlTZAuDhIaG
ql+/frKxyZw7njt3TuvWrdPp06d16dIljR07Vj179rTsv3Dhgg4fPqy9e/cqOjpaEyZMyLafiRMn
6saNGxk+fn5+hXJNAAAAAAAAQElQJCFgfHy8Vq9erUGDBmW5f+3atfLy8pKTk5MkaeDAgdqzZ4+S
k5MlSdWqVZMkOTg4aOTIkTkuDGJvb69y5cpl+FhbWxfwFQEAAAAAAAAlR5GEgKtWrZK3t7fc3d2z
3F+zZk3t2bNH8fHxkqQNGzbIzc1NdnZ2io2NVUJCgiQpPT1dq1atUv369YuibAAAAAAAAMAQiuSd
gKGhoRo6dGiGbSEhIXJyctKIESPUpUsXHTx4UA0bNpS9vb0cHBy0YsUKSVJERISGDx8uk8mk1NRU
NWjQQLNmzSqKsgEAAAAAAABDMJnNZnNxF1HYgoODNXPmzOIuAwAAAAAAACgWRbYwCAAAAAAAAIDi
QQgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAAAACAwRECAgAAAAAAAAZHCAgAAAAAAAAYHCEgAAAA
AAAAYHCEgAAAAAAAAIDBEQICAAAAAAAABkcICAAAAAAAABicTW4PtLKykslkynZ/WlpagRQEAAAA
AAAAoGDlOgSMi4uT2WzWJ598otu3b+vll1+WJM2fP1+lS5cutAIBAAAAAAAA5I/JbDab83KCr6+v
Dh8+fN9tD5Pg4GDNnDmzuMsAAAAAAAAAikWe3wkYFxenqKgoy/eoqCjFxcUVaFEAAAAAAAAACk6u
pwPfNW7cOHl7eysoKEiStHHjRv373/8u6LoAAAAAAAAAFJA8h4DDhw9X8+bNtW3bNkl3ptp6enoW
eGEAAAAAAAAACkaeQ0BJ8vT0zDb4a9OmjbZu3ZqvogAAAAAAAAAUnDy/E/B+YmJiCrpJAAAAAAAA
APlQ4CGgyWQq6CYBAAAAAAAA5EOBh4AAAAAAAAAAHi6EgAAAAAAAAIDBFXgI+OSTTxZ0kwAAAAAA
AADy4YFWB05JSdG5c+eUmJho2fbUU09Jkr755puCqQwAAAAAAABAgchzCLhhwwYNHTpUsbGxcnBw
0PXr11WtWjWdO3euMOoDAAAAAAAAkE95ng789ttva9++fapbt66uXbumzz//XN26dSuM2gAAAAAA
AAAUgDyHgFZWVqpevbpSU1MlSX379tW2bdsKvDAAAAAAAAAABSPP04FtbW0lSc7Ozvr6669Vo0YN
xcbGFnhhAAAAAAAAAApGnkPA0aNHKzY2VlOmTFGvXr10/fp1ffLJJ4VQGgAAAAAAAICCkOcQsHfv
3pIkX19fnTp1qsALAgAAAAAAAFCw8hwCStIPP/ygU6dOWd4LKEnBwcEFVhQAAAAAAACAgpPnELBP
nz46ceKE6tevL2tra0mSyWQq8MIAAAAAAAAAFIw8h4CHDx/W8ePHLQEgAAAAAAAAgIebVV5PqFGj
hpKSkgqjFgAAAAAAAACFIM8jAT/66CO1bdtWgYGBKlWqlGV7SEhIgRYGAAAAAAAAoGDkOQScOHGi
7OzslJiYqJSUlMKoCQAAAAAAAEABynMIePLkSZ08ebIwagEAAAAAAABQCPL8TsA6dero5s2bhVEL
AAAAAAAAgEKQ55GApUuXVoMGDdSuXbsM7wScOXNmgRYGAAAAAAAAoGDkOQT08PCQh4dHYdQCAAAA
AAAAoBDkOQR85513CqMOAAAAAAAAAIUk1yHgl19+qd69e+v//b//l+X+1157rcCKAgAAAAAAAFBw
ch0CRkRESJJ+/fXXTPtMJlPBVQQAAAAAAACgQOU6BJw8ebIkKSwsrNCKAQAAAAAAAFDwch0C7ty5
UwEBAfr2228z7TOZTHr88cfl7+/PqEAAAAAAAADgIZPrEHDZsmUKCAjQxx9/nOX+6Oho1alTR2vX
ri2w4gAAAAAAAADkX65DwIULF0qStm/fnu0x7u7u+a8IAAAAAAAAQIHKdQh4r5SUFJ07d06JiYmW
bU899ZRl8RAAAAAAAAAAD488h4AbNmzQ0KFDFRsbKwcHB8XGxqp69eo6d+5cYdQHAAAAAAAAIJ+s
8nrC22+/rX379qlu3bq6du2avvjiC3Xr1q0wagMAAAAAAABQAPIcAlpZWal69epKTU2VJPXt21fb
tm0r8MIAAAAAAAAAFIw8Twe2tbWVJDk7O+vrr79WjRo1FBsbW+CFAQAAAAAAACgYeQ4BX3nlFcXG
xmrKlCnq1auXrl+/ro8//rgwagMAAAAAAABQAPI8Hfjjjz9WxYoV5evrq1OnTik6OlozZ84sjNoA
AAAAAAAAFIBcjwRMTk5WYmKi0tLSFBcXJ7PZLJPJpOvXr+vWrVuFWSMAAAAAAACAfMj1SMBp06ap
QoUKOnbsmMqXL68KFSqofPny8vLyUt++fQuzRgAAAAAAAAD5kOsQ8J133lF6erqGDRum9PR0y+f6
9et6++23C7NGAAAAAAAAAPmQ53cCzps3rzDqAAAAAAAAAFBI8hwCAgAAAAAAAChZCAEBAAAAAAAA
gyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAA
AAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDg
CAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAA
AAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhC
QAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAA
AACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMr1BDw2rVr8vHxsXzc
3NxkY2OjmJiYTMdOnz5dHh4e8vHxkb+/vw4cOGDZt3//fnl7e8vNzU2tW7fWn3/+WZhlAwAAAAAA
AIZSqCGgo6OjwsPDLZ9hw4apffv2qlSpUobjwsPDNXfuXB04cEDh4eF69dVX9eqrr0qS0tPT1adP
H33yySeKjIxUUFCQxowZU5hlAwAAAAAAAIZSpNOBQ0NDNXjw4EzbTSaTUlJSdOvWLUnS9evX5ezs
LEk6fPiwbGxs1KpVK0nS8OHD9d133ykxMTHLPpKSknTz5s0Mn7S0tEK6IgAAAAAAAODhZ1NUHe3d
u1exsbHq0KFDpn3e3t4aO3asXFxcVKlSJdnb22vXrl2SpIsXL6p69eqWY8uWLaty5crp8uXLqlmz
Zqa2pk2bpsmTJ2fY5u/vX8BXAwAAAAAAAJQcRTYSMDQ0VP369ZONTebc8dy5c1q3bp1Onz6tS5cu
aezYserZs+cD9TNx4kTduHEjw8fPzy+/5QMAAAAAAAAlVpGMBIyPj9fq1at18ODBLPevXbtWXl5e
cnJykiQNHDhQo0aNUnJysqpVq6YLFy5Yjo2Li9ONGzcsx/6dvb297O3tM2yztrYuoCsBAAAAAAAA
Sp4iGQm4atUqeXt7y93dPcv9NWvW1J49exQfHy9J2rBhg9zc3GRnZydfX1+lpKRo+/btkqQFCxao
Y8eOKlWqVFGUDgAAAAAAAJR4RTISMDQ0VEOHDs2wLSQkRE5OThoxYoS6dOmigwcPqmHDhrK3t5eD
g4NWrFghSbKystKyZcs0fPhwJSYmysnJSUuXLi2KsgEAAAAAAABDMJnNZnNxF1HYgoODNXPmzOIu
AwAAAAAAACgWRbYwCAAAAAAAAIDiQQgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAAAACAwRECAgAA
AAAAAAZHCAgAAAAAAAAYHCEgAAAAAAAAYHCEgAAAAAAAAIDBEQICAAAAAAAABkcICAAAAAAAABgc
ISAAAAAAAABgcISAAAAAAAAAgMERAgIAAAAAAAAGRwgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAA
AACAwRECAgAAAAAAAAZHCAgAAAAAAAAYHCEgAAAAAAAAYHCEgAAAAAAAAIDBEQICAAAAAAAABkcI
CAAAAAAAABgcISAAAAAAAABgcISAAAAAAAAAgMERAgIAAAAAAAAGRwgIAAAAAAAAGBwhIAAAAAAA
AGBwhIAAAAAAAACAwRECAgAAAAAAAAZHCAgAAAAAAAAYHCEgAAAAAAAAYHCEgAAAAAAAAIDBEQIC
AAAAAAAABkcICAAAAAAAABgcISAAAAAAAABgcISAAAAAAAAAgMERAgIAAAAAAAAGRwgIAAAAAAAA
GBwhIAAAAAAAAGBwhIAAAAAAAACAwRECAgAAAAAAAAZHCAgAAAAAAAAYHCEgAAAAAAAAYHCEgAAA
AAAAAIDBEQICAAAAAAAABkcICAAAAAAAABgcISAAAAAAAABgcISAAAAAAAAAgMERAgIAAAAAAAAG
RwgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAAAACAwRECAgAAAAAAAAZHCAgAAAAAAAAYHCEgAAAA
AAAAYHCEgAAAAAAAAIDBEQICAAAAAAAABkcICAAAAAAAABgcISAAAAAAAABgcISAAAAAAAAAgMER
AgIAAAAAAAAGRwgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAAAACAwRECAgAAAAAAAAZHCAgAAAAA
AAAYHCEgAAAAAAAAYHCEgAAAAAAAAIDBEQICAAAAAAAABkcICAAAAAAAABgcISAAAAAAAABgcISA
AAAAAAAAgMERAgIAAAAAAAAGRwgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAAAACAwRECAgAAAAAA
AAZHCAgAAAAAAAAYHCEgAAAAAAAAYHCEgAAAAAAAAIDBEQICAAAAAAAABkcICAAAAAAAABgcISAA
AAAAAABgcISAAAAAAAAAgMERAgIAAAAAAAAGRwgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAAAACA
wRECAgAAAAAAAAZHCAgAAAAAAAAYHCEgAAAAAAAAYHA2hdn4tWvX1KZNG8v3hIQEnT17VlFRUapU
qZJl+6ZNm/TGG29YvkdFRemf//ynfvnlF0mSyWRSvXr1ZG1tLUmaPXu2WrRoUZilAwAAAAAAAIZR
qCGgo6OjwsPDLd9nzJihnTt3ZggAJemZZ57RM888Y/neoUMHtWrVKsMxu3fvVoUKFQqzXAAAAAAA
AMCQCjUE/LvQ0FBNmzYtx2MuX76srVu3avHixQ/UR1JSkpKSkjJsS0tLe6C2AAAAAAAAACMosncC
7t27V7GxserQoUOOxy1ZskRBQUH6xz/+kWF7mzZt5O3treDgYN26dSvb86dNm6by5ctn+Bw4cKBA
rgEAAAAAAAAoiYosBAwNDVW/fv1kY5P94EOz2azFixdr8ODBGbZfuHBBhw8f1t69exUdHa0JEyZk
28bEiRN148aNDB8/P78Cuw4AAAAAAACgpCmS6cDx8fFavXq1Dh48mONxO3fuVGJiYob3A0pStWrV
JEkODg4aOXKkhg0blm0b9vb2sre3z7Dt7oIiAAAAAAAAwKOoSEYCrlq1St7e3nJ3d8/xuNDQUA0Y
MCBDaBcbG6uEhARJUnp6ulatWqX69esXar0AAAAAAACAkRTJSMDQ0FANHTo0w7aQkBA5OTlpxIgR
kqQbN25o3bp1Onr0aIbjIiIiNHz4cJlMJqWmpqpBgwaaNWtWUZQNAAAAAAAAGILJbDabi7uIwhYc
HKyZM2cWdxkAAAAAAABAsSiyhUEAAAAAAAAAFA9CQAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAA
AAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAA
AAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAAAAAA
gyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAA
AAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDg
CAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAA
AAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQAAAAAAAAMDhC
QAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAAwOAIAQEAAAAA
AACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQ
AAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAwOEJAAAAAAAAA
wOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAAAAAAAADA4AgBAQAAAAAAAIMjBAQA
AAAAAAAMjhAQAAAAAAAAMDhCQAAAAAAAAMDgCAEBAAAAAAAAgyMEBAAAAAAAAAyOEBAAAAAAAAAw
OEJAAAAAAAAAwOAIAQEAAAAAAACDIwQEAAAAAAAADI4QEAAAAAAAADA4QkAAAAAAAADA4Exms9lc
3EUUthdeeEE1atQo7jJQTNLS0nTgwAH5+fnJ2tq6uMsBcsTzipKCZxUlCc8rShKeV0hS9erVNXr0
6OIuA4DBPBIhIB5tN2/eVPny5XXjxg2VK1euuMsBcsTzipKCZxUlCc8rShKeVwBAYWE6MAAAAAAA
AGBwhIAAAAAAAACAwRECAgAAAAAAAAZHCAjDs7e31zvvvCN7e/viLgW4L55XlBQ8qyhJeF5RkvC8
AgAKCwuDAAAAAAAAAAbHSEAAAAAAAADA4AgBAQAAAAAAAIMjBAQAAAAAAAAMjhAQhhIWFiaTyaT1
69dnuX/69Ony8PCQj4+P/P39deDAgaItELjH/Z7XDz/8UPXq1ZOHh4e6dOmi69evF2l9gCTVqFFD
derUkY+Pj3x8fLRq1aosjwsNDVXt2rVVq1YtDR06VCkpKUVcKZC75/X8+fMKDAxU+fLl5ePjU/RF
Av8rN8/rtm3b5OfnJw8PD3l6eur1119Xenp6MVQLADACm+IuACgo58+f18KFC+Xv75/l/vDwcM2d
O1fHjx/XY489pmXLlunVV18lCESxuN/zunnzZoWFhWn//v0qW7aspkyZokmTJunTTz8t4koBadWq
VTmGJefOndPbb7+tX375RVWqVFHnzp312Wef6ZVXXim6IoH/db/ntVy5cpoyZYpu3LihSZMmFV1h
QBbu97xWrFhRK1euVM2aNZWYmKi2bdvqiy++0IABA4qsRgCAcTASEIaQnp6uIUOGaPbs2bK3t8/y
GJPJpJSUFN26dUuSdP36dTk7OxdlmYCk3D2vv/32m5o3b66yZctKkoKCgrR06dKiLBPItTVr1qhT
p0765z//KZPJpBEjRujLL78s7rKALFWqVEnNmzeXg4NDcZcC3Ff9+vVVs2ZNSVKpUqXk4+Oj8+fP
F29RAIASixAQhjBz5kw1a9ZMvr6+2R7j7e2tsWPHysXFRc7Ozvr44481e/bsIqwSuCM3z6uvr6+2
bNmiq1evymw2a/ny5YqLi1NMTEwRVgrc0a9fP3l5eWnw4MGKjo7OtP/ixYuqXr265XuNGjV08eLF
oiwRsLjf8wo8TPLyvF69elVr1qxRhw4diqg6AIDREAKixDt27JjWrl2rt956K8fjzp07p3Xr1un0
6dO6dOmSxo4dq549exZRlcAduX1eW7VqpfHjx6tDhw7y9/dX5cqVJUk2NrzFAUVr165dOnLkiH75
5Rc9/vjj6t+/f3GXBGSL5xUlSV6e15s3b6pjx456/fXX1bBhwyKsEgBgJPzXJEq83bt36/z586pd
u7akO/9KOmzYMF25ckUvv/yy5bi1a9fKy8tLTk5OkqSBAwdq1KhRSk5Olp2dXbHUjkdPbp9XSRo5
cqRGjhwpSdq3b5+cnZ1Vrly5Iq8Zj7Zq1apJkmxtbTVmzBi5ubllecyZM2cs38+fP285DyhKuXle
gYdFbp/XuLg4Pfvss+rcubOCg4OLskQAgMEwEhAl3ssvv6wrV67o/PnzOn/+vPz9/fXZZ59lClRq
1qypPXv2KD4+XpK0YcMGubm5EQCiSOX2eZWkK1euSJISEhIUEhKi119/vajLxSPu1q1bGVal/vLL
L1W/fv1Mx3Xt2lXffvutZfr6/Pnz1atXryKsFMj98wo8DHL7vMbHx+vZZ5/Vs88+e99ZBAAA3A8j
AWFoISEhcnJy0ogRI9SlSxcdPHhQDRs2lL29vRwcHLRixYriLhGwuPd5laR27dopPT1dycnJeuml
l/Tqq68Wc4V41Pz111/q2rWr0tLSZDabVbNmTX3xxReSpCFDhqhTp07q1KmTatasqcmTJ6tZs2aS
pMDAQA0fPrw4S8cjKLfPa0JCgtzc3JSUlKQbN27I2dlZL730kqZNm1bMV4BHSW6f11mzZunAgQO6
deuW1q1bJ0nq3r07K1sDAB6IyWw2m4u7CAAAAAAAAACFh+nAAAAAAAAAgMERAgIAAAAAAAAGRwgI
AAAAAAAAGBwhIAAAAAAAAGBwhIAAAKDIBQYGav369cVdBgAAAPDIIAQEAAAAAAAADI4QEAAAgzCZ
TJo6dar8/Pzk4uKisLCwHI8/deqUmjVrJm9vb3l5eemtt96SJG3dulVNmjRR/fr15enpqdDQUMs5
AwYM0LBhw9S2bVu5uLho0KBBOnDggAIDA1WzZk0FBwdbjg0MDNSoUaPUqFEjubq6aty4cTKbzZnq
iIuL09ChQ+Xn56ennnpKw4YNU3JysiRpypQpqlu3rnx8fOTj46MLFy4UxK0CAAAAHjk2xV0AAAAo
OPb29jpw4IAiIiLUqFEjvfTSS7Kxyfr/7ufMmaMOHTpo4sSJkqSYmBhJUoMGDfTTTz/J2tpaMTEx
ql+/vp555hk5OztLko4ePart27fLyspKHh4eio2N1ebNm5WcnKyaNWtq8ODB8vT0lCSdOHFCe/fu
VUpKilq2bKkvv/xSL774YoY6xo0bpxYtWmjhwoUym80aOnSoZs2apSFDhmjGjBm6cuWKSpcurYSE
BFlZ8e+XAAAAwIMgBAQAwED69OkjSXJ3d5eNjY2uXr1qCe/+rmXLlpowYYLi4+MVEBCgtm3bSpKu
XbumwYMHKzIyUjY2Nrp27ZqOHTtmaadz584qVaqUJMnLy0vPPPOMbG1tZWtrKw8PD506dcoSAvbr
18+yr2/fvtqyZUumEHD9+vX6+eefNXPmTEnS7du3ZW1trXLlyql27drq27ev2rVrp+eeey7bawEA
AACQM/45HQAAA7kbzkmStbW1UlNTsz22a9eu2rNnj+rUqWMZFShJI0aMUPPmzXX06FGFh4fLzc1N
iYmJ2faRlz5NJlOmbWazWWvXrlV4eLjCw8N18uRJLViwQNbW1tq3b5/GjBmjqKgo+fv7a/fu3bm7
EQAAAAAyIAQEAOARderUKVWpUkX9+vXTBx98oH379kmSYmNjVb16dZlMJu3atUu//fbbA/exbNky
paSk6Pbt21qxYoVltOG9nn/+eU2fPt0SHsbGxur06dOKi4vTX3/9pRYtWujtt99W8+bN9euvvz5w
LQAAAMCjjOnAAAA8otasWaNly5bJzs5O6enpmj9/viTp/fff18iRI/Xee+/Jx8dHjRs3fuA+6tat
q2bNmikmJkadO3dWr169Mh3z8ccf61//+pd8fHxkZWUlGxsbffDBBypVqpS6deumW7duyWQyqXbt
2urfv/8D1wIAAAA8ykzmrJbpAwAAyKfAwECNGTNGzz//fHGXAgAAADzymA4MAAAAAAAAGBwjAQEA
MLiGDRtmWqzD09NTy5cvL6aKAAAAABQ1QkAAAAAAAADA4JgODAAAAAAAABgcISAAAAAAAABgcISA
AAAAAAAAgMERAgIAAAAAAAAGRwgIAAAAAAAAGBwhIAAAAAAAAGBwhIAAAAAAAACAwf1/3wdeW3vP
RvIAAAAASUVORK5CYII=
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-da42c070-eac5-4a59-b63b-000e44be9494");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-da42c070-eac5-4a59-b63b-000e44be9494"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



<h4 class="colab-quickchart-section-title">Values</h4>
<style>
  .colab-quickchart-section-title {
      clear: both;
  }
</style>



      <div class="colab-quickchart-chart-with-code" id="chart-462bec92-833f-4bda-85f6-bc9e5ed66c56">
        <img style="width: 180px;" src="data:image/png;base64,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
qV69eikqKkrdu3dXSkrKWfebPXu2IiMj1bFjR91zzz2qqqpq4qS25XyO+/r169WiRQt17dq1/uPk
yZMmpLUNDz30kMLCwmSxWLR9+/Zf3Y/3euM6n+POe71xlZeX67rrrlNUVJQSEhJ09dVXKysr66z7
Ll68WNHR0YqMjNSoUaNUUlLSxGltx/ke9+zsbDk6Op72ft+9e7cJiW3H4MGDFR8fr65du6pPnz7a
tm3bWfezqu/vBs6pf//+xpw5cwzDMIx58+YZl1122Rn77NmzxwgKCjLy8vKM2tpa49prrzXefffd
Jk5qW87nuK9bt85ISEho2mA27OuvvzZyc3ON0NBQY9u2bWfdh/d64zuf4857vXGdPHnSWLJkiVFb
W2sYhmG88847Rt++fc/Yr7S01PD39zfS0tIMwzCM+++/35g4cWJTRrUp53vc9+7da3h7ezdtOBt3
9OjR+v9euHChER8ff8Y+1vb9nTOp51BYWKgtW7bo1ltvlSSNHj1aubm5Z/zkN3/+fI0YMUKBgYGy
WCwaP3685s6da0Zkm3C+xx2N66qrrlJwcPBv7sN7vfGdz3FH43Jzc9OwYcNksVgkST179lR2dvYZ
+y1btkyXXHKJoqOjJUn33Xcf7/cLcL7HHY2vVatW9f9dXFxc///gl6zt+zsl9Rxyc3MVFBQkJycn
SZLFYlFISIhycnJO2y8nJ0ehoaH1n4eFhZ2xD87f+R53Sdq9e7e6deum7t276/3332/qqHaH97p5
eK9fPG+99ZZGjhx5xq+f7f2el5en6urqpoxns37tuEvSiRMn1L17d3Xr1k0vvviiampqmjid7bn9
9tvVvn17TZkyRZ988skZ263t+7uTaX8y0Ai6deum/fv3y9vbW/v379ewYcPk6+urG2+80exoQKPi
vX7xTJ06VVlZWVqzZo3ZUezKbx33oKAgHThwQP7+/ioqKtJNN92kN954Q48//rgJSW3Hxx9/LEn6
6KOP9MQTT2jp0qUmJ/ptnEk9h/bt25/2U7NhGMrJyVFISMhp+4WEhGjfvn31n2dnZ5+xD87f+R53
Ly8veXt7S5KCg4P1hz/8QRs2bGjyvPaE97o5eK9fHNOnT9fChQu1bNkyubu7n7H9bO/3X0558Puc
67i7urrK399fkuTj46M777yT93sj+tOf/qR169bpyJEjp/26tX1/p6Seg7+/v7p166ZPP/1UkrRg
wQIFBwcrIiLitP1Gjx6tRYsWKT8/X4ZhaObMmbr55pvNiGwTzve45+Xlqba2VpJUWlqqxYsX65JL
LmnyvPaE97o5eK83vhkzZmju3LlatWrVadfr/VJiYqK2bt2q9PR0SdL777/P+/0Cnc9xLywsrL+r
vKKiQgsXLuT9fgGOHTumgwcP1n/+xRdfqE2bNvLx8TltP6v7/m7aLVvNSHp6utGzZ08jMjLSuPTS
S40dO3YYhmEYd911l/Hll1/W7/fhhx8a4eHhRnh4uHHnnXcalZWVZkW2Cedz3N955x0jJibGiI+P
N2JiYoznnnuu/q5RNNy9995rtGvXznB0dDT8/f2Njh07GobBe/1iO5/jznu9ceXm5hqSjPDwcCMh
IcFISEgwevToYRiGYUyZMsX44IMP6vf98ssvjU6dOhkdO3Y0Ro4caRw7dsys2M3e+R73BQsWGLGx
sfXv9wceeMAoLy83M3qzlp2dbXTv3t2Ii4sz4uPjjYEDB9avJGLN398thmEY5lVkAAAA4EyM+wEA
AGB1KKkAAACwOpRUAAAAWB1KKgAAAKwOJRUAAABWh5IKAAAAq0NJBQAAgNWhpAIAAMDqUFIBAABg
dSipAAAAsDqUVAAAAFid/weM0d2Mw8ffxQAAAABJRU5ErkJggg==
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-462bec92-833f-4bda-85f6-bc9e5ed66c56");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-462bec92-833f-4bda-85f6-bc9e5ed66c56"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-4131e862-5774-4488-ad07-a57f03e0f1a7">
        <img style="width: 180px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAArMAAAFuCAYAAACSr5B3AAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90
bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAP
YQAAD2EBqD+naQAARNtJREFUeJzt3Xd8FHX+x/H37qZXSEIKkEJJQk2QXhTFLhY6VqwICv48zwp3
9gIW5I5TwXKeXY+OiqKHiA2QJr0HEkJLQgKkkrY7vz+C0ShoAklmy+v5eOzjZGdyvHcckzfDZ75j
MQzDEAAAAOCCrGYHAAAAAE4XZRYAAAAuizILAAAAl0WZBQAAgMuizAIAAMBlUWYBAADgsiizAAAA
cFmUWQAAALgsyiwAAABcFmUWAAAALosyCwAuLj09Xddee62aN2+uoKAgNW/eXAMHDtShQ4fMjgYA
DY4yCwAubuDAgQoODtbmzZtVVFSkdevW6eqrr5bFYjE7GgA0OIthGIbZIQAApycvL08RERFau3at
unbtanYcAGh0lFkAcHEpKSny9fXVuHHj1K1bN3Xq1ElWK3/xBsAz8N0OAFzc0qVLddlll2nGjBnq
2bOnIiIidP/996usrMzsaADQ4LgyCwBupKysTIsWLdJNN92kv/71r3r88cfNjgQADYoyCwBuaNiw
YaqoqNAnn3xidhQAaFCMGQCACzt69KgmTJigjRs3qqysTHa7XUuWLNHSpUvVv39/s+MBQIPzMjsA
AOD0+fj4KDc3VyNGjNDBgwdls9nUsmVLPfTQQ7rvvvvMjgcADY4xAwAAALgsxgwAAADgsiizAAAA
cFmUWQAAALgsyiwAAABcFmUWAAAALosyCwAAAJdFmQUAAIDL8tgyO23aNLMjAAAA4Ax5bJndu3ev
2REAAABwhjy2zAIAAMD1UWYBAADgsiizAAAAcFmUWQAAALgsyiwAAABcFmUWAAAALosyCwAAAJdF
mQUAAIDLoswCAADAZVFmAQAA4LIoswAAAHBZXmYHAADAVZRW2JWWU6QdWYXamV2oXTlF6hrXRHed
n2h2NMBjUWYBAPiNSrtDGXkl2pldqB1ZhdXlNSOvWA6j5r5fb89R28hgXdop2pywgIejzAIAPJZh
GDqYX6qdWYXafqKw7sgqVNrhIpVXOk76NU0CvJUcFax20cHKLS7XZxsP6eEFm9SzVZjCAn0a+RMA
oMwCADzCkeJybc8q0M6sQu3ILtLO7ELtzCpUYVnlSff397YpKTpYyVFBSooKVrvoECVFB6lZkK8s
FoskqazSrl3ZhdqZXaRHP96sl6/r2pgfCYAoswAAN1NcVllVVLN/fbW1SLlFZSfd38tqUZtmQUqK
rrramhQVrOSoYLVs6i+r1fKHv5evl01TRqRqyPTlWrjxkAZ2PqSBnWMa4mMBOAXKLADAJZVXOrQn
t6jGTOuO7ELtO3L8lF8TFxZw4ipr8ImrrsFqFREoH6/TX9wnpWUTjTuvjV76Ok0PL9isnq3CFBHk
e9r/fwDqhjILAHBqDoehzCMl2nFiLGDHibnW9NxiVf72bqwTmgX71rjKmhwdrLaRQQr0bZgfe/93
fqIWb83W9qxCPbJgs6Zf37V6FAFAw6LMAgCcgmEYyiksq77K+vOIwK7sIh2vsJ/0a4L9vJQcFVxj
RCApKrjRb8Ty8bJqyohUDX5lmRZtztLCjYd0ZWrzRs0AeCrKLACg0eWXVGhnzonCeuJq687sQh0r
qTjp/j5eViVGBin5xGjAzyMCMaF+TnMFtFOLUI0f0FbTluzSox9vVu/W4WoWzLgB0NAoswCABnO8
/MRDBn59Q1ZWobIKSk+6v9UitYoIVPKJq6w/X22NDw+U7U9uxnIG4we01eKt2dp6qEAPL9ikV2/o
5jRlG3BXlFkAwBmreshAsXZkFWlHVsGJ8lqkjLxiGScfa1WLJv5KigpScnSIkqOrlr9q0yxIft62
xg1fj34eNxj0yg/6cku2PtlwUIO6tDA7FuDWKLMAgFozDEMHjh2vWkHgxA1Z27MKtedwscrtJ3/I
QFigT/VNWElRwUqODlJiVLBC/LwbOX3j6NA8RP93fqKmLt6pRz/eoj6twxUZ4md2LMBtUWYBACeV
W1RWY551e1bVzVhFp3jIQICPrXr1gF/fkBUR5ONxf9V+53lt9L+tWdp8oEB/m79Jb9zY3eOOAdBY
KLMA4OGKfn7IwK8e6bozu1C5ReUn3d/bduIhAyeutv581bVFkz9/yICn8LZZ9eKILrripe/11bYc
zV93QEO7tjQ7FuCWKLMA4CHKKu3ac7j4dyMCB46d/CEDFkvVQwZqjghUPWTA23b6DxnwFMnRwbrn
wiS98OUOPf7JFvVtE6HoUMYNgPpGmQUAN2P/+SED1Y9yrSqv6bnFsp/iIQNRIb6/GxFoGxmkAB9+
TJyJsf1b639bsrRhf74mztuo/9zcg3EDoJ7xXQoAXJRhGMouKDvxRKwC7cgqqnrIQE6hSitOfjNW
iJ9X1WjAz+u1nng1beSHDHgKL1vV6gaX/+sHLd1xWLPX7tfI7rFmxwLcCmUWAFzAsZLyX660/ny1
NatQBaUnvxnL18taXVR/XvaqXXSIokJ8uTLYyBKjgnXvxUl6dtF2PfXpVp3dNkLNm/ibHQtwG5RZ
AHAix8vt2pXzS1n9eSWB7IKyk+5vs1qqHjIQ9ctMa3J0sOLCAlziIQOe4vZzWuvLLVlal3lME+Zt
0ju3MG4A1BfKLACYoMLuUEZucfXqAT9fdd17pOQPHzLw2xGBNpGB8vVy3YcMeAqb1aIpI1I1cNr3
+m7nYc1cvU/X9IwzOxbgFiizANCAHI7fPGTgRHHdfbhIFfaTt9bwQJ8aqwckRwcrMTJIwW76kAFP
0aZZkO6/OFnPfL5NT3+2TWcnRqhl0wCzYwEujzILAPXkcGHZL6sHnCivu7ILVVxuP+n+gT42Jf3q
Kmu76KqVBCKCfBs5ORrLrWe30hdbsrR271FNmLtJ793Wk3ED4AxRZgGgjgpLK7Qzu6jG0lc7swuV
V/zHDxn4uaz+XF55yIDnsVktemF4igb+63v9kJarD1dl6vpe8WbHAlwaZRYATqG0wq7dh4tOFNYi
7cgq0M7soj98yEBCeKCSooJqrNcaH85DBvCL1s2C9OAl7fTkwq165rNt6p/YTLFhjBsAp6tOZba0
tFTXXHONtm7dKn9/f0VGRmrGjBlq27Zt9T5ff/21LrroIr344ou65557JEklJSW67bbbtHr1almt
Vk2aNEnDhw9vsG0AUBd2h6G9ecXamV1Y44asjLySUz5kIDrE78RV1iAlR4coOarqIQP+PtyMhT93
c98EfbE5S6syjujBORv1weheXKUHTlOdr8yOGTNGl112mSwWi15++WWNHj1a33zzjSQpPz9fEyZM
0MCBA2t8zZQpU+Tr66u0tDSlp6erV69eGjBggMLDwxtkGwCcjGEYyioorSqsv7oha1d2kcoqT/6Q
gVB/718e53piRCA5KlihAdyMhdNntVr0wogUXfrP77ViT57eX7lXN/ZJMDsW4JLqVGb9/PxqFNXe
vXtrypQp1b++66679PDDD2vevHk1vm7mzJl68803JUmtWrXSeeedp/nz52v06NENsg0AjhaX11g9
4OcbsgpP8ZABP+9fPWTgV6sIRAbzkAE0jPjwQE24rJ0e+2SLJn++XecmNVN8eKDZsQCXc0Yzs9Om
TdOgQYMkSXPmzJHVatVVV131uzKbmZmp+PhfBtwTEhKUmZnZYNt+q6ysTGVlNRcct9tPfncxANdS
Ul6pXSduxvp1ec0pPPVDBlpHBFbNs0b9crU1locMwASjesdr0eZD+nHPET0wZ6P+e3tvxg2AOjrt
Mjtp0iSlpaVpyZIlysrK0tNPP109buBsJk+erCeeeKLGe7179zYpDYDTUWF3aM/h4qrCmvXLI133
HT31QwZaNvWvWkHg5zGBqGC1bsZDBuA8rFaLXhieqkv++Z1WpR/ROysydEu/VmbHAlzKaZXZKVOm
aN68efrqq68UEBCgpUuX6tChQ+rSpYskKTc3V5988okOHz6sZ555RnFxcdq7d69iYmIkSRkZGbr4
4oslqUG2/dbEiRN177331njvkUceOZ2PDqCBORyG9h89Xn2V9ef51j25p37IQESQr5Kjg2qMCCRG
BSvIlwVb4PxiwwI0cWB7PbJgs577YrvOS45UqwjGDYDashjGqa5pnNzUqVP1wQcf6KuvvlLTpk1P
us/NN9+sLl26VK9m8PjjjysjI0Nvv/129c1aW7duVURERINsq417771XU6dOrctHB1CPDMPQ4aIy
7cwq0vasgqrxgOwi7couVMkpHjIQ5OtVtezVz2u1nvjfcB4yABfncBga9Z+VWpaWp+7xTTVzbB/G
XoBaqtNli/379+u+++5T69atNWDAAEmSr6+vVq5c+Ydf98ADD+jWW29VmzZtZLPZ9PLLL1eXzobY
BsC5FJRW/LJ6QNYvy18dLak46f4+NqvaRAb9akSgavmr5qF+3IwFt2S1WvTcsBRd8o/vtGbvUb21
LF2jz2ltdizAJdT5yqy74Mos0DCy8ku1fHfuLzdkZRXqYH7pSfe1Vj9k4JcHDCRFBSshPEBePGQA
HuijVZmaOG+TfL2s+vwv56hNsyCzIwFOj4EyAPXm6+3ZuuvDdScdE4gJ9VNSVHCNG7LaRgbJz5ub
sYCfXdMjVp9vOqTvd+Xq/tkbNOeOvowbAH+CMgugXry7IkOPf7JFDkNqFx2snq3CaqwiEOrPQwaA
P2Ox/DJusC7zmP79/R6NPbeN2bEAp0aZBXBG7A5Dkz7fpjd/SJckjezeUs8M6SxvxgSA09K8ib8e
uaKDHpy7US8u3qkL2keqbWSw2bEAp8VPGwCn7Xi5XXe+v7a6yD5wSbKeG5ZCkQXO0IjuLXVecjOV
Vzp03+yNqrSf/HHLACizAE5TTmGprnl9hf63NVs+Nqv+de1ZGj+gLasNAPXAYrHo2aEpCvbz0oZ9
x/T693vMjgQ4LcosgDrblV2oIa8s14b9+Woa4K0Pbu+lq1Kbmx0LcCvRoX567MqOkqR/Lt6lHVmF
JicCnBNlFkCdLEvL1dAZy3Xg2HElhAdo3rh+6pEQZnYswC0N69pCF7SLVLndoftnb1AF4wbA71Bm
AdTa7DX7dNN/VqmwtFLd45tq3rh+PHYTaEAWi0WThnZWqL+3Nh3I12vf7jY7EuB0KLMA/pRhGHrx
fzv0wJyNqnQYujK1ud4f3UthgT5mRwPcXlSIn564qmrcYNqSXdp2qMDkRIBzocwC+ENllXbdM3O9
Xvo6TZI0fkAbTbu6Cw87ABrRoC7NdVGHKFXYDd03i3ED4NcoswBO6WhxuUb9e5U+Xn9QXlaLnh+W
ogcuaScrTyQCGpXFYtEzQzqpSYC3th4q0CtL08yOBDgNyiyAk8rILdbQGcu1KuOIgn299PYtPTWy
R6zZsQCPFRnspycHdZIkvfx1mrYczDc5EeAcKLMAfmft3iMaOmO50nOL1aKJv+bc2VdnJ0aYHQvw
eFemxOiyTtGqdFSNG5RXMm4AUGYB1LBw40Fd+8ZKHSkuV+cWoZo/rq+So3mUJuAMLBaLnhrcSWGB
PtqeVaiXv95ldiTAdJRZAJKqViyY/k2a7vpwncorHbqwfZRmju2tyBA/s6MB+JWIIF89dWLc4JVv
dmvTfsYN4NkoswBUYXdo4rxNev6LHZKkW/ol6LVR3RTg42VyMgAnc3lKjC5PiZHdYei+2etVVmk3
OxJgGsos4OEKSit069ur9d/V+2S1SI9f2UGPXdlRNlYsAJzaU4M6KSLIRzuzi/SvJYwbwHNRZgEP
duDYcY2YsULf78qVv7dNr4/qrpv7tTI7FoBaCAv00dODO0uSZnyzWxv2HTM3EGASyizgoTbtz9eQ
V5ZpR3ahmgX7atbYPrqwQ5TZsQDUwaWdonVVanM5DOm+2RtUWsG4ATwPZRbwQF9tzdbI11Yop7BM
yVHBWjC+nzq3DDU7FoDT8MRVHRUR5Ku0nCL946udZscBGh1lFvAwby9L15j31uh4hV3nJEZo9p19
1KKJv9mxAJympoE+mjSkanWDN77bo58yj5qcCGhclFnAQ9gdhp78dKse/3SrHIZ0TY9Y/efmHgrx
8zY7GoAzdHHHaA09q4UchnQ/4wbwMJRZwAOUlFfqjvfX6j/L0iVJD16arMlDO8vbxrcAwF08dmVH
RQb7as/hYr34vx1mxwEaDT/JADeXU1iqa17/UYu3ZsvHy6qXrj1L485rK4uFpbcAdxIa4K3JQ6tW
N/j3D+lak3HE5ERA46DMAm5sZ3ahhryyXBv356tpgLc+HN1LV6Y2NzsWgAZyQfsoDe/WUsaJcYPj
5YwbwP1RZgE39cOuXA2bvlwHjh1Xq4hAzR/XT90TwsyOBaCBPXJFB0WH+Ckjr0QvfMm4AdwfZRZw
Q7NW79PNb61SYVmleiQ01bw7+yohItDsWAAaQai/t54dVjVu8NbydK3ck2dyIqBhUWYBN+JwGHrh
y+16cO5GVToMXZXaXO/d1ktNA33MjgagEZ2XHKmru8fKMKQH5mxUSXml2ZGABkOZBdxEaYVdf5m5
Xq8s3S1J+r/z22raNV3k520zORkAM/z9ivZqHuqnzCMlem7RdrPjAA2GMgu4gSPF5Rr15kp9uuGg
vKwWPT88RfddnMyKBYAHC/Hz1nPDUyRJ76zYqxW7GTeAe6LMAi4uPbdYQ6cv0+qMowr29dI7t/bU
yO6xZscC4ATOSWym63rFSZIemLNBxWWMG8D9UGYBF7Y644iGTl+mjLwStWjir7nj+qpf2wizYwFw
In8b2F4tmvhr/9Hjmrxom9lxgHpHmQVc1CcbDur6N1bqaEmFUlqGav74vkqKCjY7FgAnE+TrpedP
jBu8/2OmlqXlmpwIqF+UWcDFGIahV5am6e6P1qnc7tBFHaL03zG9FRnsZ3Y0AE6qX9sIjeodL0l6
cM5GFZZWmJwIqD+UWcCFVNgdmjB3U/VC6Led3Uqv3tBNAT5eJicD4OwmXNZOsWH+OnDsuCZ9zuoG
cB+UWcBFFJRW6Ja3Vmvmmn2yWqQnB3XUI1d0kM3KigUA/lygr5deGJ4qSfpoVaa+23nY5ERA/aDM
Ai5g/9ESDZ+xXD+k5SrAx6Y3buyuG/skmB0LgIvp3TpcN/dNkCQ9NHejChg3gBugzAJObtP+fA2Z
vlw7s4sUGeyrWWP76IL2UWbHAuCiHrw0WfHhATqUX6qnF241Ow5wxiizgBNbvDVbI19bocOFZWoX
HawF4/upU4tQs2MBcGEBPlXjBhaLNGvNfi3dkWN2JOCMUGYBJ/XWsnSNeW+NjlfY1T+pmWbf0UfN
m/ibHQuAG+jZKky39mslSZowd6PySxg3gOuizAJOxu4w9PgnW/TEp1tlGNK1PWP15k3dFeznbXY0
AG7k/ouT1SoiUNkFZXqScQO4MMos4ERKyis19r21ent5hqSqpXQmDeksbxv/qQKoX/4+Nk0ZkSKL
RZr70359tTXb7EjAaeEnJOAkcgpKdfVrP+qrbdny8bLqleu66o5z28hiYektAA2jW3yYbj+ntSTp
b/M36VhJucmJgLqjzAJOYEdWoYZMX65NB/IVFuijj27vpctTYsyOBcAD3HtRkto0C1ROYZme+JRx
A7geyixgsu93HdbwGct14NhxtY4I1PxxfdUtPszsWAA8hJ+3TVNGpMpqkeavO6Avt2SZHQmoE8os
YKKZqzN1y1urVVhWqZ4JYZo3rq/iwwPNjgXAw5wV11Rj+reRJP19/iYdKWbcAK6DMguYwOEw9PwX
2/XQ3E2qdBga3KW53hvdU00CfMyOBsBD3XNhohIjg5RbVK7HPtlidhyg1upUZktLSzV48GAlJSUp
NTVVF110kdLS0iRJt9xyS/X7/fr10+rVq6u/rqSkRNdee63atm2rpKQkzZkzp0G3Ac6stMKuu/+7
TtO/2S1Juvv8tvrH1V3k62UzORkAT/bzuIHNatGnGw5q0aZDZkcCaqXOV2bHjBmjHTt2aMOGDRo0
aJBGjx4tSRoyZIi2bt2qDRs2aOLEiRoxYkT110yZMkW+vr5KS0vTl19+qXHjxikvL6/BtgHO6khx
uW7490ot3HhIXlaLXhieonsvTmbFAgBOITW2ie48t2rc4OEFm5VXVGZyIuDP1anM+vn5aeDAgdU/
eHv37q2MjAxJ0lVXXSUvL6/q9w8cOKDKykpJ0syZM3XHHXdIklq1aqXzzjtP8+fPb7BtgDNKzy3W
0OnLtGbvUQX7eendW3tqRPdYs2MBQA3/d0FbJUcFK6+4XI9+zLgBnN8ZzcxOmzZNgwYNOun7AwcO
rC63mZmZio+Pr96ekJCgzMzMBtv2W2VlZSooKKjxstvtp/uxgTpbnXFEQ6YvU0ZeiVo29de8O/uq
b9sIs2MBwO/4etn04siqcYPPNh3Swo0HzY4E/KHTLrOTJk1SWlqaJk+eXOP9999/X7NmzdLrr79+
xuHqy+TJkxUaGlrjtWrVKrNjwUN8vP6Arn9jpY6VVCi1Zajmj+unxKhgs2MBwCl1ahGq8QPaSpIe
WbBZhwsZN4DzOq0yO2XKFM2bN0+LFi1SQEBA9fszZ87UE088ocWLFysqKqr6/bi4OO3du7f61xkZ
GYqLi2uwbb81ceJE5efn13j17NnzdD46UGuGYejlr3fpL/9dr3K7Q5d0jNJ/x/RRs2Bfs6MBwJ+6
a0BbtY8J0dGSCj28YJMMwzA7EnBSdS6zU6dO1UcffaTFixerSZMm1e/PmjVLDz/8sL766qvflcoR
I0bo1VdflSSlp6frm2++0eDBgxts22/5+voqJCSkxstm485xNJwKu0MPzd2oKf/bKUkafXYrTb++
m/x9OO8AuAYfL6umjEiRl9WiL7dk65MNjBvAOVmMOvxRa//+/YqNjVXr1q0VHFz116S+vr5auXKl
vL29FR0drfDw8Or9lyxZovDwcBUXF+vWW2/VmjVrZLPZ9PTTT2vkyJGS1CDbauPee+/V1KlTa70/
UFv5xys07oO1WpaWJ6tFeuKqjhrVJ8HsWABwWqZ9tUv/+GqnQv29tfiv/RUZ4md2JKCGOpVZd0KZ
RUPYf7REt7y1WrtyihTgY9PL152l89tF/fkXAoCTqrA7NPiVZdpysEAXto/SGzd2YzlBOBWeAAbU
kw37jmnwK8u1K6dIUSG+mjW2D0UWgMvztln14shUedss+mpbthasP2B2JKAGyixQD/63JUtXv75C
uUVlahcdrAXj+6lTi1CzYwFAvWgXHaJ7LkySJD328RZlF5SanAj4BWUWOAOGYejNH9I19v21Kq1w
6NykZpp9Rx/FhPqbHQ0A6tXY/q2V0jJUBaWVmjiP1Q3gPCizwGmyOww9/skWPbVwqwxDuq5XnN68
qbuC/bzNjgYA9c7LZtWUEanysVn19fYczVm73+xIgCTKLHBaissqNebdNXpnRdVaxxMva6dnBneS
l43/pAC4r6SoYP31oqpxgycXbtWh/OMmJwIos0CdZReUauRrK7Rke458vayafn1XjT23DXf3AvAI
t5/TSl1im6iwtFIT5jJuAPNRZoE62J5VoCEnlqgJD/TRh7f31sDOMWbHAoBGUz1u4GXVtzsPa9aa
fWZHgoejzAK19N3Owxo+Y4UO5peqdbNAzR/XT93im5odCwAaXdvIIN1/cdW4wVMLt+nAMcYNYB7K
LFALH63K1C1vr1ZRWaV6tQrTvDv7Ki48wOxYAGCa285ura5xTVRUVqkJczcybgDTUGaBP+BwGHp2
0XZNnLdJdoehoWe10Lu39VSTAB+zowGAqWxWi6aMSJWvl1Xf78rVR6sYN4A5KLPAKZRW2PV//12n
V7/dLUn6ywWJenFkqny9bCYnAwDn0LpZkB68tJ0k6ZnPtmrfkRKTE8ETUWaBk8grKtN1b/yozzYe
krfNohdHpOqvFyWxYgEA/MYtfRPUI6GpisvtemjuRjkcjBugcVFmgd/YfbhIQ2cs10+ZxxTi56V3
b+2lYd1amh0LAJyS1WrRC8NT5edt1fLdefpg5V6zI8HDUGaBX1m5J09Dpy/X3rwSxYb5a964vurT
JtzsWADg1BIiAjXhxLjB5EXblZnHuAEaD2UWOGHBugMa9eYq5R+vUJfYJpo/rp/aRgabHQsAXMKN
fRLUq1WYSsrtemDOBsYN0Ggos/B4hmHopSW7dM/M9Sq3O3RZp2h9dHtvRQT5mh0NAFzGz+MGAT42
rUw/ondXZJgdCR6CMguPVl7p0ANzNurFxTslSWP6t9Yr13WVvw8rFgBAXcWFB2jiZVXjBs9+sV0Z
ucUmJ4InoMzCY+Ufr9DNb63SnLX7ZbVITw3upL8NbC+rlRULAOB0Xd8rXn3bhKu0wsG4ARoFZRYe
ad+REg2bsVzLd+cp0MemN2/uoVG9482OBQAuz2q16LlhKQr0sWl1xlG9tTzD7Ehwc5RZeJz1+45p
yPRlSsspUnSIn2bd0UcDkiPNjgUAbiM2LEB/v7yDJOn5L7Zrz+EikxPBnVFm4VG+2Jyla15fodyi
crWPCdH88X3VsXmo2bEAwO1c2zNW5yRGqKzSoftnb5CdcQM0EMosPIJhGPr393t05wdrVVrh0HnJ
zTT7jj6KCfU3OxoAuCWLxaJnh6UoyNdLP2Ue05s/7DE7EtwUZRZur9Lu0GOfbNHTn22TYUjX94rT
v2/sriBfL7OjAYBba9HEX49c0V6SNOV/O5WWw7gB6h9lFm6tuKxSY95bq3dX7JXFIv19YHs9PbiT
vGyc+gDQGEZ2j9W5Sc1UXunQfbM3qNLuMDsS3Aw/0eG2sgtKNfK1Ffp6e458vayafl1X3d6/tSwW
lt4CgMZSNW7QWcF+Xtqw75je+D7d7EhwM5RZuKWtBws0+JVl2nKwQOGBPvpoTG9d1jnG7FgA4JFi
Qv316BVVqxv8Y/FO7cwuNDkR3AllFm7nmx05GvHqch3KL1WbZoGaP66fusY1NTsWAHi04d1a6vx2
kSq3V61uwLgB6gtlFm7lg5V7dds7a1Rcblfv1mGad2c/xYUHmB0LADyexWLR5KGdFeLnpY378/Xa
d6xugPpBmYVbcDgMTV60TX+fv1l2h6GhZ7XQu7f2UmiAt9nRAAAnRIX46fGrOkqS/vnVTm3PKjA5
EdwBZRYur7TCrrs++kmvfVv1p/x7LkzUiyNT5ePF6Q0AzmbIWS10YfsoVdgN3TdrgyoYN8AZ4qc9
XFpeUZmufeNHfb4pS942i6aOTNU9FyaxYgEAOCmLxaJJQzupSYC3thws0IxvdpsdCS6OMguXlZZT
pCHTl2td5jGF+Hnp3Vt7aWjXlmbHAgD8ichgPz1xYtzgX0t2acvBfJMTwZVRZuGSftyTp2Ezlivz
SIliw/w1b1w/9WkTbnYsAEAtXZXaXJd2jFalw9D9szeqvJJxA5weyixczvx1+zXqzZXKP16hs+Ka
aP64fmobGWR2LABAHVgsFj01uJOaBnhr26ECvbw0zexIcFGUWbgMwzA07atd+uvMDaqwG7qsU7Q+
ur23IoJ8zY4GADgNzYJ99dTgTpKkV5amafMBxg1Qd5RZuITySofun71R//hqpyRpbP/WeuW6rvLz
tpmcDABwJq5Iaa7LO8fI7jB0/+wNKqu0mx0JLoYyC6eXX1Khm/6zSnN/2i+b1aJnhnTSxIHtZbWy
YgEAuIMnB3VUeKCPtmcV6qUljBugbiizcGr7jpRo6IxlWrEnT4E+Nr15U3dd3yve7FgAgHoUHuSr
p0+MG8z4drc27DtmbiC4FMosnNa6zKMaMn2Zdh8uVnSIn2bf0VfnJUeaHQsA0AAu6xyjK1ObV48b
lFYwboDaoczCKX2x+ZCuef1H5RaVq0NMiBaM76cOzUPMjgUAaEBPXtVREUG+2pVTpGlLdpkdBy6C
MgunYhiG3vhuj+784CeVVTo0ILmZZt3RR9GhfmZHAwA0sKaBPpo0pGrc4LVvd2td5lGTE8EVUGbh
NCrtDj3y8WY98/k2GYY0qne83rixu4J8vcyOBgBoJBd3jNaQs1rIYYhxA9QKZRZOoaisUre/u0bv
/5gpi0V6+PL2enJQR3nZOEUBwNM8dmUHNQv21e7DxZq6eKfZceDkaAowXVZ+qUa+ukJLdxyWn7dV
M67vptHntJbFwtJbAOCJmgT4aPKQzpKkN77fo7V7j5icCM6MMgtTbT1YoMGvLNPWQwWKCPLRf8f0
0aWdos2OBQAw2YUdojSsa0sZhnT/7I06Xs64AU6OMgvTLN2RoxGvLldWQanaRgZp/rh+6hLbxOxY
AAAn8eiVHRQV4qv03GJN+d8Os+PASVFmYYr3f9yr0e+sUXG5XX1ah2vuHX0VGxZgdiwAgBMJ9ffW
s8NSJEn/WZauVemMG+D36lRmS0tLNXjwYCUlJSk1NVUXXXSR0tKqHjuXk5OjSy+9VImJierUqZO+
++676q9r7G1wXg6HoUmfb9PDCzbL7jA0rGtLvXNrT4UGeJsdDQDghAYkR2pk96pxgwfmbFBJeaXZ
keBk6nxldsyYMdqxY4c2bNigQYMGafTo0ZKkCRMmqHfv3tq1a5feeustXXfddaqoqDBlG5xTaYVd
4z/8Sa9/t0eSdO9FSZoyIkU+XvwFAQDg1B6+ooNiQv20N69Ez3/BuAFqqlOL8PPz08CBA6vvMu/d
u7cyMjIkSbNmzdIdd9whSerRo4eaN2+ub7/91pRtcD65RWW65vUftWhzlnxsVv3z6i66+4JEViwA
APypED9vPXdi3ODt5Rn6cU+eyYngTM7okti0adM0aNAg5eXlqaKiQtHRv9yFnpCQoMzMzEbfdjJl
ZWUqKCio8bLbuSuysaTlFGrI9GVav++YQv299d5tPTX4rBZmxwIAuJD+Sc10bc9YSVXjBsVljBug
ymmX2UmTJiktLU2TJ0+uzzwNYvLkyQoNDa3xWrVqldmxPMKK3XkaOn259h05rriwAM0b11e9Woeb
HQsA4IL+NrC9WjTx174jx/Xsou1mx4GTOK0yO2XKFM2bN0+LFi1SQECAwsPD5eXlpaysrOp9MjIy
FBcX1+jbTmbixInKz8+v8erZs+fpfHTUwdy1+3Xjf1aqoLRSXeOaaP64vmrTLMjsWAAAFxX8q3GD
937cq+VpuSYngjOoc5mdOnWqPvroIy1evFhNmjSpfn/EiBF69dVXJUmrV6/WgQMHdO6555qy7bd8
fX0VEhJS42Wz2er60VFLhmHoH4t36r7ZG1RhN3R55xh9eHtvhQf5mh0NAODizk6M0A29qy5ePTBn
o4oYN/B4FsMwjNruvH//fsXGxqp169YKDg6WVFUUV65cqezsbI0aNUrp6eny8fHRyy+/rAEDBkhS
o2+rjXvvvVdTp06t9f6onbJKuybO3aR56w5Iku44t40evCRZVis3egEA6kdxWaUu+ed32n/0uK7r
FadJJx59C89UpzLrTiiz9S+/pEJj3lujlelHZLNa9PTgTrq258lHPwAAOBMrdufp2jd+lCS9e2tP
9U9qZnIimIUFPlEvMvNKNGTGMq1MP6IgXy/95+YeFFkAQIPp0yZcN/WJlyRNmLtRBaWsNe+pKLM4
Yz9lHtWQ6cu053CxYkL9NPuOPjqXPyEDABrYQ5e1U1xYgA7ml2rSZ9vMjgOTUGZxRj7fdEjXvv6j
8orL1bF5iBaM76f2MSFmxwIAeIAAHy9NGZEqi0X67+p9+mZHjtmRYALKLE6LYRh67dvdGvfBTyqr
dOj8dpGaNbaPokL8zI4GAPAgPVuF6ea+CZKkCXM3Kf844waehjKLOqu0O/T3BZs1+cSC1Tf2idfr
o7op0NfL5GQAAE/04CXtlBAeoKyCUj21cKvZcdDIKLOok6KySt32zhp9uDJTFov0yBUd9MRVHeVl
41QCAJjD38dWPW4wZ+1+fb092+xIaEQ0ENTaofzjGj5jub7deVh+3la9ekM33XZ2K1ksrCELADBX
94QwjT67laQT4wYljBt4CsosamXLwXwNfmWZtmcVKiLIRzPH9NElHaPNjgUAQLX7Lk5W62aByiks
0xOfbjE7DhoJZRZ/aun2HI14dYWyC8qUGBmk+eP6KTW2idmxAACowc+7atzAapHmrTug/23JMjsS
GgFlFn/ovRUZuu2d1Sopt6tvm3DNubOvYsMCzI4FAMBJdY1rqtv7t5Yk/W3+Zh0tLjc5ERoaZRYn
5XAYeuazrXrk4y1yGNLwbi319i09FervbXY0AAD+0F8vTFLbyCDlFpXpccYN3B5lFr9zvNyucR/8
pDe+T5ck3X9xkl4YniIfL04XAIDz8/O26cURqbJZLfp4/UF9sfmQ2ZHQgGgnqOFwYZmueeNHfbEl
Sz42q6Zd00V3nZ/IigUAAJeSGttEY0+MG/x9/mblFZWZnAgNhTKLamk5hRoyfZk27DumJgHeen90
Lw3q0sLsWAAAnJa/XJiopKgg5RWX69FPGDdwV5RZSJKW787V0OnLtf/occWHB2jenX3Vs1WY2bEA
ADhtvl42vTiii2xWiz7beEifbWTcwB1RZqE5a/frxjdXqaC0Ut3im2r+uH5q3SzI7FgAAJyxzi1D
Nf68NpKkRz7erFzGDdwOZdaDGYahqYt36v7ZG1TpMHRFSow+GN1LYYE+ZkcDAKDe3HV+otpFB+tI
cbkeWbBZhmGYHQn1iDLrocoq7bp31gb9a8kuSdK489roX9ecJT9vm8nJAACoXz5eVr04MlVeVosW
bc7Sp4wbuBXKrAc6VlKuUW+u0vx1B2SzWvTs0M568NJ2slpZsQAA4J46Ng/VXee3lSQ9+vFm5RSW
mpwI9YUy62H25hVr6IzlWpV+RMG+Xnr7lh66pmec2bEAAGhw4we0VYeYEB0rqdDf5zNu4C4osx5k
7d6jGjJ9ufYcLlbzUD/NvrOPzklsZnYsAAAahbetatzA22bR4q3Z+nj9QbMjoR5QZj3EZxsP6do3
ftSR4nJ1ahGi+eP7qV10iNmxAABoVO1jQnT3+YmSpMc+2aLsAsYNXB1l1s0ZhqFXv92t8R/+pPJK
hy5sH6mZY/ooKsTP7GgAAJjijvPaqHOLUOUfr9Df5m1i3MDFUWbdWIXdob/N36xnF22XJN3cN0Gv
jequQF8vk5MBAGAeb5tVU0akysdm1ZLtOZr30wGzI+EMUGbdVGFphW57Z40+WpUpi0V69IoOevyq
jrKxYgEAAEqODtY9F1WNGzz+6RZl5TNu4Koos27o4LHjGvHqCn2387D8vW167YZuuvXsVmbHAgDA
qYw5p7VSY5uosLRSE+ZtZNzARVFm3czmA/ka/Moybc8qVESQr2aO7a2LO0abHQsAAKfjZbNqyvAU
+XhZ9c2Ow5q9Zr/ZkXAaKLNuZMm2bI18bYVyCsuUFBWkBeP7KqVlE7NjAQDgtBKjgnXfRUmSpKcW
btXBY8dNToS6osy6iXdXZOj2d9eopNyus9tGaM6dfdWyaYDZsQAAcHqjz2mts+KaqLCsUg/NZdzA
1VBmXZzdYeiphVv16Mdb5DCkkd1b6q1beijEz9vsaAAAuASb1aIpI1Ll62XV97ty9d/V+8yOhDqg
zLqw4+V23fn+Wr35Q7ok6YFLkvXcsBR52/jXCgBAXbRpFqQHLkmWJD29cKv2Hy0xORFqi9bjonIK
S3XN6yv0v63Z8rFZNe2aLho/oK0sFpbeAgDgdNzSr5W6xzdVcbmdcQMXQpl1QbuyCzXkleXasD9f
TQK89cHtvTSoSwuzYwEA4NJsVoteGJEqP2+rlqXl6YOVmWZHQi1QZl3MsrRcDZ2xXAeOHVdCeIDm
j+unHglhZscCAMAttIoI1EOXtpMkTfp8m/YdYdzA2VFmXcjsNft0039WqbC0Ut3jm2reuH5qFRFo
diwAANzKTX0S1LNVmErK7XpgzgY5HIwbODPKrAswDEMv/m+HHpizUZUOQ1emNtf7o3spLNDH7GgA
ALgdq9WiF4anyN/bph/3HNF7P+41OxL+AGXWyZVV2nXPzPV66es0SdL4AW007eou8vO2mZwMAAD3
FR8eqIkDq8YNnl20XXvzik1OhFOhzDqxo8XlGvXvVfp4/UF5WS16blhnPXBJO1mtrFgAAEBDu6FX
vPq0DtfxCrsemL2RcQMnRZl1Uhm5xRo6Y7lWZRxRsK+X3r6lp67uEWd2LAAAPIbVatHzw1MU6GPT
qowjent5htmRcBKUWSe0du8RDZ2xXOm5xWrRxF9z7uyrsxMjzI4FAIDHiQ0L0MSB7SVJz3+5XXsO
F5mcCL9FmXUyCzce1LVvrNSR4nJ1bhGq+eP6Kjk62OxYAAB4rOt7xensthEqrXDogTkbZWfcwKlQ
Zp2EYRia/k2a7vpwncorHbqwfZRmju2tyBA/s6MBAODRLBaLnh3WWUG+Xlq796jeWpZudiT8CmXW
CVTYHZo4b5Oe/2KHJOmWfgl6bVQ3Bfh4mZwMAABIUsumAXr48qpxgxe+3KG0HMYNnAVl1mQFpRW6
9e3V+u/qfbJapMeu7KDHruwoGysWAADgVK7uEav+Sc1UVunQ/bM3MG7gJCizJjpw7LhGzFih73fl
yt/bptdHddct/VqZHQsAAJyExWLRs0M7K9jXS+v3HdMb3+8xOxJEmTXNpv35GvLKMu3ILlSzYF/N
GttHF3aIMjsWAAD4A82b+OuRKztIkqYu3qld2YUmJwJl1gRfbc3WyNdWKKewTMlRwVowvp86tww1
OxYAAKiFEd1aakByM5WfGDeotDvMjuTR6lRm7777biUkJMhisWj9+vXV73/++efq2rWrunTpok6d
Oumdd96p3paTk6NLL71UiYmJ6tSpk7777rsG3ebs3l6WrjHvrdHxCrvOSYzQ7Dv7qEUTf7NjAQCA
WrJYLJo8NEXBfl7asD9fr33HuIGZ6lRmhw8frh9++EHx8fHV7xmGoRtuuEFvv/221q9fr4ULF2rs
2LEqLKy67D5hwgT17t1bu3bt0ltvvaXrrrtOFRUVDbbNWdkdhp74dIse/3SrHIZ0TY9Y/efmHgrx
8zY7GgAAqKPoUD89fmVHSdI/v9qp7VkFJifyXHUqs/3791fLli1/977FYtGxY8ckSQUFBQoPD5ev
r68kadasWbrjjjskST169FDz5s317bffNti2kykrK1NBQUGNl91ur8tHPyMl5ZW64/21emtZhiTp
wUuTNXloZ3nbmPIAAMBVDe3aQhe2j1SF3dD9szeognEDU5xxm7JYLJo5c6aGDh2q+Ph4nX322Xrn
nXfk4+OjvLw8VVRUKDo6unr/hIQEZWZmNsi2U5k8ebJCQ0NrvFatWnWmH71WjhSX65rXf9Tirdny
8bLqpWvP0rjz2spiYektAABcmcVi0aQhnRXq763NBwr06je7zY7kkc64zFZWVurpp5/WvHnztHfv
Xi1ZskSjRo1Sbm5ufeSrFxMnTlR+fn6NV8+ePRvl9w70tcnP26amAd76cHQvXZnavFF+XwAA0PAi
Q/z05KCqcYN/fb1LWw8ybtDYzrjMrl+/XgcPHlT//v0lVf21f8uWLbVu3TqFh4fLy8tLWVlZ1ftn
ZGQoLi6uQbadiq+vr0JCQmq8bDbbmX70WvH1sun1Ud00f1w/dU8Ia5TfEwAANJ6rUpvr4g5R1eMG
5ZWMGzSmMy6zsbGxOnTokLZt2yZJSktL0+7du5WcnCxJGjFihF599VVJ0urVq3XgwAGde+65DbbN
GTUJ8FFCRKDZMQAAQAOwWCx6ZkhnNQ3w1tZDBXplaZrZkTyKV112Hjt2rD777DNlZWXpkksuUXBw
sNLS0vT6669r5MiRslqtcjgcevnll6uvlD733HMaNWqUEhMT5ePjo/fff1/e3t4Ntg0AAKCxNQv2
1ZODOun/PlqnV5am6aIOUerUgjXkG4PFMAyPfLDwvffeq6lTp5odAwAAuAnDMDT+w5/0+aYstYsO
1id3nS0fL1YuamgcYQAAgHpgsVj05KBOCgv00fasQr309S6zI3kEyiwAAEA9iQjy1VODOkmSpn+z
Wxv3HzM3kAegzAIAANSjy1NidEVKjOyOqtUNyiob70FNnogyCwAAUM+eHNRJEUE+2pldpGlfMW7Q
kCizAAAA9Sws0EdPD+4sSXr1291av++YuYHcGGUWAACgAVzaKVqDujSXw5Dum7VepRWMGzQEyiwA
AEADefzKjmoW7Kvdh4v1j8U7zY7jliizAAAADaRpoI8mDakaN3jj+z1au/eoyYncD2UWAACgAV3U
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
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-4131e862-5774-4488-ad07-a57f03e0f1a7");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-4131e862-5774-4488-ad07-a57f03e0f1a7"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-d520f1c5-3cf5-41b4-8abd-2f7e2f77598d">
        <img style="width: 180px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAqkAAAFuCAYAAACiI4FWAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90
bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAP
YQAAD2EBqD+naQAASGJJREFUeJzt3XlYlXX+xvH34bDJqiibAiICbijuKVhZmdmmqVlWVo6520xN
mWbllNWorb9pmkxtzHYr18z2xRpHTTP3FVARVBZX9p3n9wdGMZKKAs85h/t1Xeca6XzB+zxzxNvz
Od8vFsMwDEREREREbIiT2QFERERERP6XSqqIiIiI2ByVVBERERGxOSqpIiIiImJzVFJFRERExOao
pIqIiIiIzVFJFRERERGbo5IqIiIiIjZHJVVEREREbI5KqoiIiIjYHJVUEZFa8tZbbxESEmJ2DBER
h6CSKiJykfr27csTTzxRq19z5MiRjBgxola/poiIPVJJFRERERGbo5IqInIRxo8fz5o1a3j++efx
8vLCy8ur8r558+YRHh6Or68vw4YNIzs7u/K+06dPM2HCBFq2bEnTpk254YYbOHDgAAAzZ87k/fff
56OPPqr8mikpKaSlpXHTTTcRGBiIt7c3nTp1YvHixReUs1u3bsybN6/y41atWtGlS5fKj5977jmu
ueaaS70cIiK1TiVVROQizJ07l8svv5wpU6aQm5tLbm4uAOnp6ezdu5c9e/awd+9etm7dyksvvQSA
YRgMHjyY7OxstmzZwtGjR+nYsSM33XQTJSUlPPbYY9x1113cfvvtlV8zLCyMsrIyRo0axf79+zl5
8iQPPPAAd955J7t27TpvzmuvvZavv/4agISEBAoLCzlw4ACZmZkAfPPNN/Tv37+OrpKIyMVTSRUR
qUXOzs48//zzNGrUiODgYG655RY2btwIwJYtW1i7di3z5s3Dz88PNzc3Zs6cycGDB9mwYcMffs2Q
kBCGDBmCl5cXLi4u3HfffbRv357vv//+vHn69+/P999/T1lZGV9//TXXXXcdffv25euvv6agoID/
/ve/KqkiYpOczQ4gIuJImjVrhouLS+XHnp6e5OTkAJCYmEhpaWm1JwCkpqb+4dc8deoUU6ZM4dtv
v+XEiRM4OTmRm5tb+WroucTHx1NcXMzGjRv5+uuvGT58OKdOneKrr77C398fHx8fOnfuXPMHKiJS
x1RSRUQukpNTzYZRQUFBuLq6cuzYsSpF9nxf89FHH2Xv3r38+OOPhIaGYrFYiI2NxTCM8/6ebm5u
XHHFFXz22Wf8+OOPLFiwgKysLJ5++mn8/f3p168fFoulRo9DRKQ+aNwvInKRgoKCSEhIuOD1ffr0
ISYmhgkTJlS+Cnrq1CmWLl1Kfn5+5dfcv38/ZWVllZ+XlZWFh4cHTZs2paSkhFdfffWC3o/6q/79
+/Ovf/2LyMhI/P39iYyMxNPTkzfffFOjfhGxWSqpIiIX6eGHH2bfvn00adKExo0bn3e91Wrlm2++
wcPDg8suuwxvb29iY2NZvnx55auZY8eOBSreNtC4cWNSUlJ49tlnKSgoIDAwkPDwcDIyMoiPj7/g
nP379ycrK6tKIb3uuuvIysri2muvrdmDFhGpJxbjQuZFIiIiIiL1SK+kioiIiIjNUUkVEbFj77//
fuXB//97mzp1qtnxREQumsb9IiIiImJz9EqqiIiIiNgclVQRERERsTkqqSIiIiJicxyqpL7yyitm
RxARERGRWuBQJfXQoUNmRxARERGRWuBQJVVEREREHINKqoiIiIjYHJVUEREREbE5KqkiIiIiYnNU
UkVERETE5qikioiIiIjNUUkVEREREZujkioiIiIiNkclVURERERsjkqqiIiIiNgclVQRERERsTnO
ZgcQERExk2EYZGQXsTc9m4SMHPal53I6v5jHb2xHhL+X2fFEGiyVVBERaTBO5xezLz2nooxm5LAv
veKWXVh61trMnCKWTYzDxaqho4gZVFJFRMThFBSXkZj5Wwndl1FRTDOyi6pdb3Wy0KqZJ20CvYkK
9GLh2mR2HMli7g/7+fM1UfWcXkRAJVVEROxYSVk5ycfz2Pvrq6NnCmnKyXwMo/rPCWnSiDaB3kQH
edMm0Js2Qd5E+Hvi5mytXBPe1JMHP9rKP79PpF/7QNoF+9TTIxKRX6mkioiIzSsvNzhyuqCyhP46
st9/LJeSsurbaDMvV6IDvYkO9KZtUEUpjQrwwtvd5by/36DOzfl8Rxpf787g4Y+38cn98Rr7i9Sz
GpfUxMRE7r33Xo4fP46vry9vvfUWHTp0OGvdggULmD17NuXl5Vx99dXMmTMHFxcXkpOTGTlyJFu2
bKFVq1Zs3bq18nO+//57Hn30UXJzc7FYLNx4443Mnj0bJyd9YxARaQgMw+B4bjEJGTkVr46eKaWJ
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
Dg7mu+++4+mnn642y6xZs/D19a1y27hxY00ejoiIiIhNuLlTMNfHBFFabjB58TaKSzX2t6mNU5s2
bWLnzp0cOXKEo0ePcs011zB+/Phq106bNo2srKwqt549e9ZzYhEREZFLZ7FYeOaWGPw8XdmbnsO/
vk80O5LpalRSQ0NDSUtLo7S0FADDMEhJSSEsLKzKurCwMA4dOlT5cXJycuWac933zjvvcPXVV9O4
cWOcnJy49957Wb16dbVZ3Nzc8PHxqXKzWq01eTgiIiIiNqOZlxvPnBn7v/bDfnYcbthj/xqV1ICA
ALp27cp7770HwNKlSwkJCSEyMrLKuqFDh7Jy5UrS09MxDIO5c+cyfPjw894XERHB999/T3FxMQCr
Vq0iJibmkh+kiIiIiD24sVMwN3YKpqzc4OHFWykqbbj7bWo87p83bx7z5s0jOjqa2bNns3DhQgBG
jx7NypUrgYqyOWPGDOLj44mMjMTf359x48ad975JkybRqlUrYmNj6dSpE9999x2vv/56bT1WERER
EZv3zKAYmnm5kpCRyyvfNtyxv8UwDMPsELXloYce4uWXXzY7hoiIiMgl+XJnOuPf+wUnCyyfGE9s
aGOzI9U7m9o4JSIiIiIwICaIQZ2bU27Aw4u3UVjS8Mb+KqkiIiIiNuipmzvg7+1GUmYu//dtgtlx
6p1KqoiIiIgNauLpyszBHQF44z8H+OXQKZMT1S+VVBEREREbdW37QIZ0aUG5AY80sLG/SqqIiIiI
DXvy5g4EeLtx4HgeL329z+w49UYlVURERMSG+Xq4MHtoxdj/3/89yKbkkyYnqh8qqSIiIiI27uq2
gdzaLQTDgMmLt1FQ7Phjf5VUERERETsw/ab2BPm4k3win+e/2mt2nDqnkioiIiJiB3wb/Tb2f2td
MhsOnDA5Ud1SSRURERGxE33bBDC8RyiGAY8s2U5+canZkeqMSqqIiIiIHXn8xnY093Un5WQ+z33h
uGN/lVQRERERO+Lt7sJzt3YC4O31h1i3/7jJieqGSqqIiIiInbk8yp87LwsDYMqS7eQVOd7YXyVV
RERExA49dkM7WjRuxOFTBcz6Yo/ZcWqdSqqIiIiIHfJyc+aFM2P/935K4b+JjjX2V0kVERERsVNx
kc24u1dLAKYu3U5OYYnJiWqPSqqIiIiIHXv0+raE+jXiyOkCZn7uOGN/lVQRERERO+bp5swLt8YC
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
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-d520f1c5-3cf5-41b4-8abd-2f7e2f77598d");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-d520f1c5-3cf5-41b4-8abd-2f7e2f77598d"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



      <div class="colab-quickchart-chart-with-code" id="chart-bccc2361-183e-4747-b3fe-e358f7eace46">
        <img style="width: 180px;" src="data:image/png;base64,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
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-bccc2361-183e-4747-b3fe-e358f7eace46");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-bccc2361-183e-4747-b3fe-e358f7eace46"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>



<h4 class="colab-quickchart-section-title">Faceted distributions</h4>
<style>
  .colab-quickchart-section-title {
      clear: both;
  }
</style>


    <string>:5: FutureWarning: 
    
    Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `y` variable to `hue` and set `legend=False` for the same effect.
    



      <div class="colab-quickchart-chart-with-code" id="chart-2ffb45a1-e725-46e4-96c7-ef8b20abf7d7">
        <img style="width: 180px;" src="data:image/png;base64,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
KVOMMcY8++yzZu7cuVY7c+fONVOnTjXGGDNhwgSTlpZmqqqqjDHGFBQUGGOMefrpp80jjzxijDEm
IyPD/PKXv7RqOLc/JZnVq1cbY4x57733TNeuXY0x5qL9Vd3Guf21fft263tUVJRxu911Po+6vPzy
yyYxMdGUl5eb8vJyk5iYaOLj463az39OKSkpZtGiRcYYYyIjI82nn35qjDGmqqrK6sukpCTrnIqK
CjNkyBCzcuXKC+oODQ01n332mTHGmNzcXNOhQwdTUVFhcnNzTUxMjCkqKjLGGPP111+bDh06mNLS
UuN2u42Pj4/ZsmWLMcaYV155xSQlJVn9e+7fqzFn/3aGDBliysvLLzhmzZo11rnGGPP999/X2U/G
nH3G7dq1Mz169LA+WVlZtR5bWlpqioqKanxKS0vrbR8AcGW55557zD333GN3GQAAABd1SedIJSQk
yOPx6NZbb1WfPn0knR1RtXnzZnk8Hms6nzcCAgIUGhqqcePGKSkpSXfeeac6deqkf/zjH4qIiLBG
x4wdO1b/8z//o2+//VaSFBISovj4+BptjRkzRpLUrl07hYSEaP/+/erYsWONYwYNGqQHHnhAd911
l4YNG6auXbvWW19wcLAGDRokSRo1apQefPBBHTp0SBs3btTo0aOtUTRpaWm67777JElr165VUVGR
3nrrLUlSeXm5goODJUnr169Xdna2WrQ4O+gtKCjIq/5q1aqVNcw/NjZWubm5kqTs7Ox6+6uh6noe
ddm0aZPGjRsnPz8/SVJKSopee+01a39tz6naoEGD9Mgjjyg5OVlJSUlyuVw6deqUNm3apGPHjlnH
/fDDD7WuMzVx4kRlZGQoOjpar7/+usaOHStfX1+9//772rdvn+644w7r2BYtWujgwYOSpC5duqhv
376Szvbh888/X2+fnHt/5+rRo4e+/PJLTZ06VfHx8Ro+fHi97Uhnn0tDpvUxhQ8AAAAA8FNxSX+t
LyEhQW63W26325q6FR8fb20bOHCg1236+Phoy5Ytmj59ugoKChQTE6OPPvrooue1bt36gm2tWrWq
0W5lZeUFx7z11ltasGCBKioqNHz4cGVlZXlVr8PhkMPhqHV7NWOMXnrpJeXk5CgnJ0d79uypMf3u
x/D397eu5ePjo6qqqka14+vrW+Pc0tJSq83GPI9q5/dNbc+p2p/+9CdlZGTo2muvVUpKip577jmZ
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
AAAA2KbZwqlnnnlGqamp1vePP/5YDodDHo/H2jZlyhQNGzZMo0ePliTl5eWpTZs21n6Hw6ETJ05I
koYPH669e/c2V7k1pKamyu12X5JrNVZ6eroWLlwoSVqyZInuvvtuSdK2bdus/mysvLw8paen/9gS
m8Ts2bOVmZnZ6POb8+/mxIkTWrBgQb3H5Ofnq3///l63feDAAV133XUqLy+3tnXp0kUTJkywvm/Z
skU/+9nPvG4bAAAAAIDLiW9zNZyQkKBJkyZZ391ut/r27SuPx6MBAwZY29LT05WQkHDR9jZs2NBk
tVVWVsrXt+5bf+2115rsWs1lypQptW6Pjo7Wm2+++aParg6n6rrGpVJZWam5c+f+qDaa8u/mfNXh
1OOPP17r/srKSt1yyy366KOPvG771ltvVfv27bV161bFxcXp0KFDuv7667VlyxbrGLfbXee7U1ZW
prKyshrb/P395e/v73UtAAAAAAA0p2YbORUTE6P8/HwdPnxYkuTxeDR79mxr5NSRI0d08OBBlZWV
yeVyXbS94OBg5eTkSJLmzZun7t27y+VyyeVy6cCBA5KkDz74QL169VJkZKTi4+O1Z88e69pOp1OT
J0+Wy+XS22+/reDgYM2ePVuxsbHq3Lmz5s2bZ11rwIABWrt2raSzQVVYWJhcLpciIiKUnZ1dZ40e
j0fh4eEaP368wsPDFRUVZdUsSQsXLpTT6VRERITGjh2roqIiSVJFRYUef/xx9enTRy6XS6NGjVJh
YaEkqaioSKmpqQoPD1ePHj2swG/OnDmaPn16rTVU92f1SLSnn35aUVFR6tKlS42wpq7+mjJlivbu
3SuXy6URI0Zc0P/S2RCs+lnW9TxqU1FRoalTp6pr166KiYnRY489ZoWVtT2nCRMm6IUXXpAk/f3v
f1dkZKRcLpfCw8P1zjvvSJKOHj2qUaNGqU+fPoqIiNCTTz5pXa+67k8++UQRERE1ahkwYIDVxgcf
fKC4uDhFRUWpT58+1si56mc6depU9ejRQ06nU9u2bbP6qbi4WC6XS9HR0Vab06ZNU2xsrJKSkmqM
BiwpKdHo0aMVFhamHj16KCkpqc5+ks4GvNV97PF4NGTIEN10003Ky8uzttUVTs2fP1+BgYE1PvPn
z6/3egAAAAAA2MI0o0GDBpmlS5ea0tJS07lzZ2OMMbfddpspKSkxmZmZJiEhwbjdbtOjRw9jjDH7
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
sbHq3Lmz5s2bZ11rwIABWrt2raSzQVVYWJhcLpciIiKUnZ1dZ40ej0fh4eEaP368wsPDFRUVZdUs
SQsXLpTT6VRERITGjh2roqIiSVJFRYUef/xx9enTRy6XS6NGjVJhYaEkqaioSKmpqQoPD1ePHj2s
wG/OnDmaPn16rTVU92f1SLSnn35aUVFR6tKlS42Qr67+mjJlivbu3SuXy2WFQOf2v3Q2BKt+lnU9
j4Z444031LdvX/Xq1Ut33HGHduzYIensCKuBAwdqxIgRCgsL0x133KG8vDxr37mjr+bMmaPQ0FBF
RUUpKyvrgvYjIyMVGRmpO++8U99++63VRmJiov77v/9bERERio6O1jfffGOdV9uzysnJUXp6ujIz
M+VyuTR37lxVVlZqyJAhio6OltPp1JgxY3Tq1Kk673f+/PkKDAys8Zk/f36D+wsAAAAAgEvlJx9O
tWzZUv369ZPb7VZZWZn279+v4cOH6/DhwyotLZXb7VZsbKxatWrlVbuFhYV6/vnn9fnnnysnJ0ef
fvqp2rdvr4KCAo0ZM0avv/66du7cqQcffFDJyckyxkiSvvzyS40fP145OTm67777JEknTpzQ5s2b
9dlnn2nhwoVWcHGuxx57TJs2bVJOTo4+//xzOZ3Oeuv74osvlJKSot27d2vmzJm6//77ZYzRe++9
p7/97W/65JNPtGvXLl133XV6/PHHJZ0NQq677jpt3bpVOTk5ioiI0JNPPilJmj59ulq2bKmdO3dq
x44d+uMf/+hVfxUVFSkyMlL//ve/9fLLL1vT4errr/T0dN1+++3KycnRunXrGvU8GuKTTz7RihUr
9OGHH+rzzz/Xs88+qzFjxtTY/8c//lF79uzRL37xCz344IMXtPHuu+9q1apV+ve//61t27ZZAZZ0
dorf//7v/+q9997Tzp071a9fvxpTTT/77DP94Q9/0K5du5SYmGj1bV3PyuVyacqUKRo7dqxycnI0
e/Zs+fj4aPny5dq2bZt2796twMBAvfTSS3Xe86xZs1RUVFTjM2vWrAb1FwAAAAAAl9JPPpySzk7t
83g8ys7OVp8+fSSdHVG1efNmeTyeBk3nO19AQIBCQ0M1btw4LV68WMePH1erVq2UnZ2tiIgIRURE
SJLGjh2r/Px8K3AKCQlRfHx8jbaqg5B27dopJCRE+/fvv+B6gwYN0gMPPKAXX3xR+/fvV+vWreut
Lzg4WIMGDZIkjRo1SkePHtWhQ4e0ceNGjR492lpTKy0tTf/4xz8kSWvXrtWyZcuskUcrVqywalm/
fr1mzJihFi3O/kkEBQV51V+tWrXSyJEjJUmxsbHKzc2VpIv2V0PV9Twa4p133tGOHTvUt29fuVwu
Pfzwwzp+/LhKSkokSf369VP37t0lSQ8++KA8Ho+qqqpqtLFp0yaNGjVKAQEBcjgceuihh6x9brdb
Q4cOVceOHSVJU6dO1T//+U+rjepRc+f3TX3P6nzGGC1atEg9e/ZUZGSk3n333RojzM7n7++vgICA
Gh+m9AEAAAAALkdXTDjldrvldrutdabi4+OtbQMHDvS6TR8fH23ZskXTp09XQUGBYmJi9NFHH130
vNpCpXNDFB8fH1VWVl5wzFtvvaUFCxaooqJCw4cPv2Da2MU4HA45HI5at1czxuill15STk6OcnJy
tGfPniZbY8vf39+6lo+PzwXhTkP5+vrWOLe0tNRqszHPQzp73ykpKdZ95+Tk6MiRI7rmmmsaVaOk
Wvu6rn0Nef4Xa3P58uX65z//qX/961/atWuXZsyYYfUNAAAAAAA/ZVdEONW7d28VFBQoMzOzRjiV
lZWlI0eOWKOpvFFcXKxjx46pf//+euqppxQXF6ft27crJiZGu3bt0u7duyVJWVlZ6tixozVqpjEq
KyuVm5ur6OhozZgxQ8nJydq6dWu95+Tl5Vm/9Ld69Wq1b99enTp1UmJiolauXKmTJ09KkhYvXqyk
pCRJ0t13361Fixbp9OnTkqTTp0/riy++kHR28e/nn39eZ86ckSR99913jb6fc9XXXwEBAdZ6WNW6
dOlirbe1detW65cT63oeDTFixAgtW7ZMBw8elCSdOXNG27Zts/Zv3rxZX331laSza38lJCTIx8en
RhuJiYlatWqViouLZYzRq6++au1LSEjQ+++/r/z8fElnf+Fw0KBBF7Rxvvqe1fl9U1hYqHbt2ikg
IEDFxcVasmRJg+4dAAAAAIDL3U/+1/okyc/PT3FxcdqxY4e6desmSeratauKi4sVFxcnPz8/r9ss
KipScnKyTp06JYfDodDQUKWkpCgwMFCZmZkaP368Kisr1bZtW61atareUS8XU1VVpUmTJun48ePy
9fVVUFCQMjIy6j3H6XRqyZIlmjZtmlq2bKkVK1bI4XBo2LBh2r17t2JjY9WiRQtFRkbqz3/+syRp
5syZKisrU9++fa16Z86cKafTqUWLFunRRx9VRESE/Pz81Lt3b/3lL39p9D1VCwoKqrO/IiMj5XQ6
FR4erpCQEK1bt07z5s1TSkqKFi9erNjYWGvtrbqeR0P0799fzz33nO655x5VVlaqvLxcd955p6Kj
oyWdndY3c+ZM7du3TzfeeKOWLl16QRvDhw/X1q1b1atXLwUEBGjYsGHWvvDwcC1cuND6Zb3/+q//
alDf1fes7rnnHr3xxhtyuVwaOXKkHnnkEb3zzju6/fbbFRQUpP79+3u1IDwAAAAAAJcrh6leyRs/
GR6PR9OnT693zSE0zJIlS7R27VrrVxMBAAAAALicVa/3vGbNGpsraTpXxLQ+AAAAAAAA/DQxcuoy
Fh0dfcHi2U6nU5mZmTZVdHkpKCiw1mg61+DBg7Vw4UIbKgIAAAAAoHldiSOnrog1p65U5y7ajQvd
dNNNTG0EAAAAAOAnjml9AAAAAAAAsA3hFAAAAAAAAGxDOAUAAAAAAADbEE4BAAAAAADANoRTAAAA
AAAAsA3hFAAAAAAAAGxDOAUAAAAAAADbEE4BAAAAAADANoRTAAAAAAAAsA3hFAAAAAAAAGxDOAUA
AAAAAADbEE4BAAAAAADANoRTAAAAAAAAsA3hFAAAAAAAAGxDOAUAAAAAAADbEE4BAAAAAADANoRT
AAAAAAAAsA3hFAAAAAAAAGxDOAUAAAAAAADbEE4BAAAAAADANoRTAAAAAAAAsA3hFAAAAAAAAGxD
OAUAAAAAAADbEE4BAAAAAADANoRTAAAAAAAAsA3hFAAAAAAAAGxDOAUAAAAAAADb+NpdAAAAAAAA
ABqmffv2dpfQ5BzGGGN3EQAAAAAAALg6Ma0PAAAAAAAAtiGcAgAAAAAAgG0IpwAAAAAAAGAbwikA
AAAAAADYhnAKAAAAAAAAtiGcAgAAAAAAgG0IpwAAAAAAAGAbwikAAAAAAADYhnAKAAAAAAAAtiGc
AgAAAAAAgG0IpwAAAAAAAGAbwikAAAAAAADYhnAKAAAAAAAAtiGcAgAAAAAAgG0IpwAAAAAAAGAb
wikAAAAAAADYhnAKwGUlLS1NaWlpdpcBAAAAALhEfO0uAADOdezYMbtLAAAAAABcQoycAgAAAAAA
gG0IpwAAAAAAAGAbwikAAAAAAADYhnAKAAAAAAAAtiGcAgAAAAAAgG0IpwAAAAAAAGAbwikAAAAA
AADYhnAKAAAAAAAAtiGcAgAAAAAAgG0IpwAAAAAAAGAbwikAAAAAAADYhnAKAAAAAAAAtiGcAgAA
AAAAgG0IpwAAAAAAAGAbwikAAAAAAADYhnAKAAAAAAAAtiGcAgAAAAAAgG0IpwAAAAAAAGAbwikA
AAAAAADYhnAKAAAAAAAAtiGcAgAAAAAAgG0IpwAAAAAAAGAbwikAAAAAAADYhnAKAAAAAAAAtiGc
AgAAAAAAgG0IpwAAAAAAAGAbwikAAAAAAADYhnAKAAAAAAAAtiGcAgAAAAAAgG0IpwAAAAAAAGAb
wikAAAAAAADYhnAKAAAAAAAAtiGcAgAAAAAAgG0Ip2z0zDPPKDU11fr+8ccfy+FwyOPxWNumTJmi
YcOGafTo0ZKkvLw8tWnTxtrvcDh04sQJSdLw4cO1d+/eS1G6UlNT5Xa7m7TN2bNnKzMzs9Z9L7/8
siZMmNCk12uI+mq6mKbooyVLlujuu+/+UW0AAAAAAHA587W7gKtZQkKCJk2aZH13u93q27evPB6P
BgwYYG1LT09XQkLCRdvbsGFDk9VWWVkpX9+6/zxee+21JrtWtblz5zZ5mz9WY2uqqqpqlj4CAAAA
AOBKw8gpG8XExCg/P1+HDx+WJHk8Hs2ePdsaOXXkyBEdPHhQZWVlcrlcF20vODhYOTk5kqR58+ap
e/fucrlccrlcOnDggCTpgw8+UK9evRQZGan4+Hjt2bPHurbT6dTkyZPlcrn09ttvKzg4WLNnz1Zs
bKw6d+6sefPmWdcaMGCA1q5dK+lsUBUWFiaXy6WIiAhlZ2fXWePgwYO1evVq67vH41HPnj0lSRMm
TNALL7wgSSouLtbo0aN1++23Ky4uTrt27bLOOX800fr1660w7+jRo0pISFBUVJScTqd+/etf68yZ
M/X224ABAzRjxgz1799ft912m6ZMmWLtq62mbt26qX///nrooYes0VxLlixRQkKC7r33XkVERGjr
1q01+ujIkSNKSkpSWFiYkpKSdP/992vOnDmSpDlz5mj69OnWNesaJebNvZWVlenkyZM1PmVlZfX2
AwAAAAAAdiCcslHLli3Vr18/ud1ulZWVaf/+/Ro+fLgOHz6s0tJSud1uxcbGqlWrVl61W1hYqOef
f16ff/65cnJy9Omnn6p9+/YqKCjQmDFj9Prrr2vnzp168MEHlZycLGOMJOnLL7/U+PHjlZOTo/vu
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
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-2ffb45a1-e725-46e4-96c7-ef8b20abf7d7");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-2ffb45a1-e725-46e4-96c7-ef8b20abf7d7"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>


    <string>:5: FutureWarning: 
    
    Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `y` variable to `hue` and set `legend=False` for the same effect.
    



      <div class="colab-quickchart-chart-with-code" id="chart-ef4c78d0-97bd-4cb5-8359-c4d85f8f0bf2">
        <img style="width: 180px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABKcAAAGkCAYAAADkLEZ0AAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90
bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAP
YQAAD2EBqD+naQAAQ85JREFUeJzt3XlwVVW+/v/nkEAQMYgSsYErIRKEnAwHEiChAyEMYbAbgUaw
AYkgrYSriC33Ig6ANAot3kZLq4XWagYJMok4oZbgOQ7IKASCKCUxYUogtIQQITPr9we/7G9CBhJM
2Ejer6pUefY+e+3PXnv14FNrreMwxhgBAAAAAAAANmhgdwEAAAAAAACovwinAAAAAAAAYBvCKQAA
AAAAANiGcAoAAAAAAAC2IZwCAAAAAACAbQinAAAAAAAAYBvCKQAAAAAAANiGcAoAAAAAAAC2IZwC
AAAAAACAbQinAAAAAAAAYBvCKQAAAAAAANiGcAoAAAAAAAC2IZwCAAAAAACAbQinAAAAAAAAYBvC
KQAAAAAAANiGcAoAAAAAAAC2IZwCAAAAAACAbQinAADANSMhIUEJCQl2lwEAAICryNvuAgAAAEqc
PHnS7hIAAABwlTFzCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAA
ALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAA
AABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAA
AAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcA
AAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZw
CgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALa5auHUc889p4kT
J1qfv/76azkcDnk8HuvYpEmTNGjQII0aNUqSlJaWpptvvtk673A4dObMGUnS4MGDdfDgwatRuiZO
nCi3231V7nWlFi1apAULFkiSli5dqqFDh0qSdu3aZfXnlUpLS9OiRYt+bYm1YubMmUpMTLzi6+ty
3Jw5c0bz58+v8jvp6enq2bPnFbU/e/Zs+fn5yeVyWX/PP//8FbUFAAAAAMC1wmGMMVfjRl9++aUm
TJigQ4cOSZL+9re/6aOPPtLAgQM1e/ZsSdJdd92lRYsWKTY2VtLFUMTlclmBlMPhUFZWVpnAqjYU
FRXJ29u7Vtu009KlS7VhwwZt2LChVtrzeDyaOnWqkpKSaqW9K3Wtv6dLx+ulfm39s2fP1pkzZ/Ty
yy9f9rv5+fnKz88vc8zHx0c+Pj5XfH8AuBqGDx8uSVq/fr3NlQAAAOBquWozpyIjI5Wenq5jx45J
uhh4zJw505o5lZGRoSNHjig/P18ul+uy7fn7+1thydy5c9WpUydrNsnhw4clSZ9++qm6dOmi0NBQ
xcTE6MCBA9a9nU6nHnzwQblcLr377rvy9/fXzJkzFRUVpXbt2mnu3LnWvXr37m0FPW+++aaCgoLk
crkUEhKi7du3V1qjx+NRcHCwxo0bp+DgYIWHh5cJeBYsWCCn06mQkBCNGTNG2dnZkqTCwkI9+eST
6tatm1wul0aOHKmsrCxJUnZ2tiZOnKjg4GCFhYVpwoQJki4GF1OnTq2whpL+LJmJNmvWLIWHh6t9
+/bauHGj9d3K+mvSpEk6ePCgXC6XhgwZUq7/JSkiIsJ6l5W9j4oUFhZq8uTJ6tChgyIjI/XEE0+o
d+/elb6nBx54wApnPvjgA4WGhsrlcik4OFjvvfeeJOnEiRMaOXKkunXrppCQED3zzDPW/Urq3rJl
i0JCQsrU0rt3b6uNTz/9VNHR0QoPD1e3bt2smXMl73Ty5MkKCwuT0+nUrl27rH7KycmRy+VSRESE
1eaUKVMUFRWluLi4MrMBc3NzNWrUKAUFBSksLExxcXGV9lNNzZs3T82aNSvzN2/evFprHwAAAACA
WmOuor59+5rly5ebvLw8065dO2OMMXfeeafJzc01iYmJJjY21rjdbhMWFmaMMSY1NdU0a9bMul6S
ycrKMsYY07ZtW7Nnzx5z+vRp06xZM3P+/HljjDHnzp0zubm55uTJk+aWW24x+/btM8YYs2LFCtOp
Uydz4cIF43a7jcPhMB6Px2q7bdu25tFHHzXGGHPq1Cnj6+trjh07ZowxJiYmxrz77rvGGGN8fX1N
enq6McaYgoICk5OTU+nzut1uI8ls2rTJGGPM6tWrzV133WUuXLhgNm7caDp27Gg9z1/+8hczadIk
Y4wxzz//vJkzZ47Vzpw5c8zkyZONMcY88MADJiEhwRQXFxtjjMnMzDTGGDNr1izz2GOPGWOMWbJk
ibnnnnusGkr3pySzbt06Y4wxH3/8senQoYMxxly2v0raKN1fe/bssT6Hh4cbt9td6fuozGuvvWb6
9etnCgoKTEFBgenXr5+JiYmxar/0PcXHx5uFCxcaY4wJDQ0133zzjTHGmOLiYqsv4+LirGsKCwvN
gAEDzJo1a8rVHRgYaHbu3GmMMSYlJcXcfvvtprCw0KSkpJjIyEiTnZ1tjDHmxx9/NLfffrvJy8sz
brfbeHl5mW3bthljjHn99ddNXFyc1b+lx6sxF8fOgAEDTEFBQbnvrF+/3rrWGGN+/vnnSvvJmIvv
uEWLFiYsLMz6W7VqVYXfzcvLM9nZ2WX+8vLyqmwfAK4Fw4YNM8OGDbO7DAAAAFxFV3WNVGxsrDwe
j9q2batu3bpJujijauvWrfJ4PNZyvprw9fVVYGCgxo4dq7i4ON19991q06aNPvvsM4WEhFizY8aM
GaP//u//1vHjxyVJAQEBiomJKdPW6NGjJUktWrRQQECAUlNT1bp16zLf6du3r+6//3798Y9/1KBB
g9ShQ4cq6/P391ffvn0lSSNHjtRDDz2ko0ePatOmTRo1apQ1iyYhIUH33nuvJGnDhg3Kzs7WO++8
I0kqKCiQv7+/JOnDDz/U9u3b1aDBxUlvfn5+Neqvxo0bW0smoqKilJKSIknavn17lf1VXZW9j8ps
3rxZY8eOVcOGDSVJ8fHxevPNN63zFb2nEn379tVjjz2mESNGKC4uTi6XS+fOndPmzZt18uRJ63u/
/PJLhftMjR8/XkuWLFFERISWLVumMWPGyNvbW5988okOHTqkXr16Wd9t0KCBjhw5Iklq3769unfv
LuliH7700ktV9knp5ystLCxM33//vSZPnqyYmBgNHjy4ynaki++lOsv6WMIHAAAAAPituKq/1hcb
Gyu32y23220t3YqJibGO9enTp8Ztenl5adu2bZo6daoyMzMVGRmpr7766rLXNW3atNyxxo0bl2m3
qKio3HfeeecdzZ8/X4WFhRo8eLBWrVpVo3odDoccDkeFx0sYY/Tqq68qKSlJSUlJOnDgQJnld7+G
j4+PdS8vLy8VFxdfUTve3t5lrs3Ly7PavJL3UeLSvqnoPZX4xz/+oSVLlqhJkyaKj4/Xiy++KPP/
b6G2bds2q/8OHTpUZmlfifj4eK1Zs0a5ublavny5xo8fL+li//fv39+6PikpScePH1dgYKCk6o2T
6jxDQECADhw4oIEDB2rLli0KDg62lm8CAAAAAFBfXNVwqmvXrsrMzFRiYmKZcGrVqlXKyMiwZlPV
RE5Ojk6ePKmePXvq2WefVXR0tPbs2aPIyEglJydr//79kqRVq1apdevW5WZC1URRUZFSUlIUERGh
adOmacSIEdqxY0eV16SlpVn7Fa1bt04tW7ZUmzZt1K9fP61Zs0Znz56VJC1evNjac2jo0KFauHCh
zp8/L0k6f/68vvvuO0nSkCFD9NJLL+nChQuSpFOnTl3x85RWVX/5+vpa+2GVaN++vbXf1o4dO6yZ
SZW9j8r06dNHK1euVGFhoQoLC7V8+fJq1/zDDz/I6XTqkUceUUJCgrZt26amTZsqNja2zK/mld7r
rLRWrVqpa9euevzxx3XbbbfJ6XRKkgYMGKBNmzZp37591ncv956li7PGcnNzVVBQUK36jx07JofD
Yb1TY4yOHj1arWsBAAAAALheXNVlfQ0bNlR0dLT27t2rjh07SpI6dOignJwcRUdHV7j06XKys7M1
YsQInTt3Tg6HQ4GBgYqPj1ezZs2UmJiocePGqaioSM2bN9fatWsrnLVUXcXFxZowYYJOnz4tb29v
+fn5acmSJVVe43Q6tXTpUk2ZMkWNGjXS22+/LYfDoUGDBmn//v2KiopSgwYNFBoaqn/+85+SpOnT
pys/P1/du3e36p0+fbqcTqcWLlyoxx9/XCEhIWrYsKG6du2qN95444qfqYSfn1+l/RUaGiqn06ng
4GAFBATo/fff19y5cxUfH6/FixcrKirKCnYqex+Vefjhh5WcnKygoCA1b95cERERSk9Pr1bNTz31
lA4ePKhGjRqpSZMmev311yVJiYmJ+utf/6rg4GA5HA7deOONWrx4cYXLC8ePH6+RI0da10oXg7eV
K1fq4Ycf1vnz51VQUKDOnTtr5cqVVdZzyy23aNy4cQoNDVXTpk2tjdIrk5ycrBkzZsgYo6KiIt1/
//0KDQ2t8prExERr43np4mzEhQsXVnkNAAAAAADXMocpWQeFWufxeDR16tQyv2qH8nJycnTTTTep
sLBQY8aMUXh4uKZPn253WQAAG5Tsi7h+/XqbKwEAAMDVclVnTgEV6devn/Lz85WXl6fo6GhNmTLF
7pIAAAAAAMBVQjhVCyIiIsptiu10OpWYmMisKUmZmZnWflql9e/fXwsWLLD2roK0ceNGPfXUU+WO
z5gxQ6NGjbKhIgAAAAAA6hbL+gAAwDWDZX0AAAD1z1X9tT4AAAAAAACgNMIpAAAAAAAA2IZwCgAA
AAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcA
AAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZw
CgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBt
CKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA
2IZwCgAAAAAAALbxtrsAAACAEi1btrS7BAAAAFxlDmOMsbsIAAAAAAAA1E8s6wMAAAAAAIBtCKcA
AAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZw
CgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBt
CKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAJeVkJCghIQEu8vAdcjb7gIAAAAA
AMC17+TJk3aXgOsUM6cAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAA
AABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAA
AAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcA
AAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZw
CgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBt
CKcAAAAAAABgG8IpAAAAAAAA2IZwCgAAAAAAALYhnAIAAAAAAIBtCKcAAAAAAABgG8IpAAAAAAAA
2KbOwqnnnntOEydOtD5//fXXcjgc8ng81rFJkyZp0KBBGjVqlCQpLS1NN998s3Xe4XDozJkzkqTB
gwfr4MGDdVVuGRMnTpTb7b4q97pSixYt0oIFCyRJS5cu1dChQyVJu3btsvrzSqWlpWnRokW/tsRa
MXPmTCUmJl7x9XU5bs6cOaP58+dX+Z309HT17Nmzxm0fPnxYN954owoKCqxj7du31wMPPGB93rZt
m+64444atw0AAAAAwLXEu64ajo2N1YQJE6zPbrdb3bt3l8fjUe/eva1jixYtUmxs7GXb27hxY63V
VlRUJG/vyh/9zTffrLV71ZVJkyZVeDwiIkKrV6/+VW2XhFOV3eNqKSoq0pw5c35VG7U5bi5VEk49
+eSTFZ4vKipSq1at9NVXX9W47bZt26ply5basWOHoqOjdfToUd10003atm2b9R23213pf3by8/OV
n59f5piPj498fHxqXAsAAAAAAHWpzmZORUZGKj09XceOHZMkeTwezZw505o5lZGRoSNHjig/P18u
l+uy7fn7+yspKUmSNHfuXHXq1Ekul0sul0uHDx+WJH366afq0qWLQkNDFRMTowMHDlj3djqdevDB
B+VyufTuu+/K399fM2fOVFRUlNq1a6e5c+da9+rdu7c2bNgg6WJQFRQUJJfLpZCQEG3fvr3SGj0e
j4KDgzVu3DgFBwcrPDzcqlmSFixYIKfTqZCQEI0ZM0bZ2dmSpMLCQj355JPq1q2bXC6XRo4cqays
LElSdna2Jk6cqODgYIWFhVmB3+zZszV16tQKayjpz5KZaLNmzVJ4eLjat29fJqyprL8mTZqkgwcP
yuVyaciQIeX6X7oYgpW8y8reR0UKCws1efJkdejQQZGRkXriiSessLKi9/TAAw/o5ZdfliR98MEH
Cg0NlcvlUnBwsN577z1J0okTJzRy5Eh169ZNISEheuaZZ6z7ldS9ZcsWhYSElKmld+/eVhuffvqp
oqOjFR4erm7dulkz50re6eTJkxUWFian06ldu3ZZ/ZSTkyOXy6WIiAirzSlTpigqKkpxcXFlZgPm
5uZq1KhRCgoKUlhYmOLi4irtJ+liwFvSxx6PRwMGDNBtt92mtLQ061hl4dS8efPUrFmzMn/z5s2r
8n4AAAAAANjC1KG+ffua5cuXm7y8PNOuXTtjjDF33nmnyc3NNYmJiSY2Nta43W4TFhZmjDEmNTXV
NGvWzLpeksnKyjLGGNO2bVuzZ88ec/r0adOsWTNz/vx5Y4wx586dM7m5uebkyZPmlltuMfv27TPG
GLNixQrTqVMnc+HCBeN2u43D4TAej8dqu23btubRRx81xhhz6tQp4+vra44dO2aMMSYmJsa8++67
xhhjfH19TXp6ujHGmIKCApOTk1Pp87rdbiPJbNq0yRhjzOrVq81dd91lLly4YDZu3Gg6duxoPc9f
/vIXM2nSJGOMMc8//7yZM2eO1c6cOXPM5MmTjTHGPPDAAyYhIcEUFxcbY4zJzMw0xhgza9Ys89hj
jxljjFmyZIm55557rBpK96cks27dOmOMMR9//LHp0KGDMcZctr9K2ijdX3v27LE+h4eHG7fbXen7
qMxrr71m+vXrZwoKCkxBQYHp16+fiYmJsWq/9D3Fx8ebhQsXGmOMCQ0NNd98840xxpji4mKrL+Pi
4qxrCgsLzYABA8yaNWvK1R0YGGh27txpjDEmJSXF3H777aawsNCkpKSYyMhIk52dbYwx5scffzS3
3367ycvLM26323h5eZlt27YZY4x5/fXXTVxcnNW/pcerMRfHzoABA0xBQUG576xfv9661hhjfv75
50r7yRhj3nrrLdOnTx9jjDHjx483H3/8sXnmmWfMv//9b1NQUGBuvPFGc/jw4QqvzcvLM9nZ2WX+
8vLyqrwfAAAAAFRl2LBhZtiwYXaXgetQnW6IXjLzY/v27erWrZukizOqtm7dWuWsj6r4+voqMDBQ
Y8eO1eLFi3X69Gk1btxY27dvV0hIiDU7ZsyYMUpPT9fx48clSQEBAYqJiSnT1ujRoyVJLVq0UEBA
gFJTU8vdr2/fvrr//vv1yiuvKDU1VU2bNq2yPn9/f/Xt21eSNHLkSJ04cUJHjx7Vpk2bNGrUKGsW
TUJCgj777DNJ0oYNG7RixQpr5tHbb79t1fLhhx9q2rRpatDg4qvy8/OrUX81btxYw4cPlyRFRUUp
JSVFki7bX9VV2fuozObNmzV27Fg1bNhQDRs2VHx8fJnzFb2nEn379tVjjz2mF198Ufv27dPNN9+s
c+fOafPmzXrsscesGUyHDh2qcJ+p8ePHa8mSJZKkZcuWacyYMfL29tYnn3yiQ4cOqVevXnK5XBox
YoQaNGigI0eOSLq411P37t3L9WFlSp7vUmFhYfr+++81efJkrV69usLvlBYbG6utW7eqoKBAX3/9
taKjoxUTEyOPx6OdO3eqZcuWle455ePjI19f3zJ/LOkDAAAAAFyL6jyccrvdcrvd1tKtmJgY61if
Pn1q3KaXl5e2bdumqVOnKjMzU5GRkdXa06eiUKl0iOLl5aWioqJy33nnnXc0f/58FRYWavDgwVq1
alWN6nU4HHI4HBUeL2GM0auvvqqkpCQlJSXpwIEDtbZXko+Pj3UvLy8vFRcXX1E73t7eZa7Ny8uz
2ryS91Hi0r6pKvz7xz/+oSVLlqhJkyaKj4/Xiy++KGOMpIubg5f036FDh8os7SsRHx+vNWvWKDc3
V8uXL9f48eMlXez//v37W9cnJSXp+PHjCgwMlFS9cVKdZwgICNCBAwc0cOBAbdmyRcHBwdbyzYq0
bt1abdq00erVq3XrrbeqadOm6tGjh7Zs2XLF//kBAAAAAOBaU6fhVNeuXZWZmanExMQy4dSqVauU
kZFhzaaqiZycHJ08eVI9e/bUs88+q+joaO3Zs0eRkZFKTk7W/v37JUmrVq1S69at1bp16yuuv6io
SCkpKYqIiNC0adM0YsQI7dixo8pr0tLSrP2K1q1bp5YtW6pNmzbq16+f1qxZo7Nnz0qSFi9ebO05
NHToUC1cuFDnz5+XJJ0/f17fffedJGnIkCF66aWXdOHCBUnSqVOnrvh5Squqv3x9fa39sEq0b9/e
2m9rx44d1sykyt5HZfr06aOVK1eqsLBQhYWFWr58ebVr/uGHH+R0OvXII48oISFB27ZtU9OmTRUb
G1vmV/NK73VWWqtWrdS1a1c9/vjjuu222+R0OiVJAwYM0KZNm7Rv3z7ru5d7z9LFWWO5ubllflGv
KseOHZPD4bDeqTFGR48erfKa2NhY/e1vf7NmkzVp0kS33Xabli1bdkUzDwEAAAAAuNbU2a/1SVLD
hg0VHR2tvXv3qmPHjpKkDh06KCcnR9HR0Zdd1lSR7OxsjRgxQufOnZPD4VBgYKDi4+PVrFkzJSYm
aty4cSoqKlLz5s21du3aCmctVVdxcbEmTJig06dPy9vbW35+ftaysMo4nU4tXbpUU6ZMUaNGjfT2
22/L4XBo0KBB2r9/v6KiotSgQQOFhobqn//8pyRp+vTpys/PV/fu3a16p0+fLqfTqYULF+rxxx9X
SEiIGjZsqK5du+qNN9644mcq4efnV2l/hYaGyul0Kjg4WAEBAXr//fc1d+5cxcfHa/HixYqKirKC
ncreR2UefvhhJScnKygoSM2bN1dERITS09OrVfNTTz2lgwcPqlGjRmrSpIlef/11SVJiYqL++te/
Kjg4WA6HQzfeeKMWL16sNm3alGtj/PjxGjlypHWtdDF4W7lypR5++GGdP39eBQUF6ty5s1auXFll
PbfccovGjRun0NBQNW3a1NoovTLJycmaMWOGjDEqKirS/fffr9DQ0CqviY2N1b/+9S8r3JUuBrzz
588nnAIAAAAAXBccpmRdFH41j8ejqVOnlvlVO5SXk5Ojm266SYWFhRozZozCw8M1ffp0u8sCAAAA
AFShZD/j9evX21wJrjd1OnMKqEi/fv2Un5+vvLw8RUdHa8qUKXaXBAAAAAAAbEI4dQUiIiLKbYrt
dDqVmJjIrClJmZmZ1n5apfXv318LFiyw9q6CtHHjRj311FPljs+YMUOjRo2yoSIAAAAAAK4ulvUB
AAAAAIDLYlkf6kqd/lofAAAAAAAAUBXCKQAAAAAAANiGcAoAAAAAAAC2IZwCAAAAAACAbQinAAAA
AAAAYBvCKQAAAAAAANiGcAoAAAAAAAC2IZwCAAAAAACAbQinAAAAAAAAYBvvK7koIyNDqampKioq
so716tWr1ooCAAAAAABA/VDjcOr555/XggULFBAQIC8vL0mSw+HQjh07ar04AAAAAAAAXN9qHE79
+9//VkpKim699da6qAcAAAAAAAD1SI33nGrZsiXBFAAAAAAAAGpFjWdO9e/fX1OnTtXo0aPVuHFj
63hoaGitFgYAAAAAAIDrX43DqeXLl0uS3nvvPeuYw+HQTz/9VHtVAQAAAAAAoF6ocTiVmppaF3UA
AAAAAACgHqpxOCVJO3bs0KZNmyRJcXFxioiIqNWiAAAAAAAAUD/UeEP0f/3rXxoxYoQyMzN16tQp
/elPf9Kbb75ZF7UBAAAAAADgOucwxpiaXBAaGqrNmzfLz89PknTq1Cn17dtX+/btq5MCAQAAAACA
/YYPHy5JWr9+vc2V4HpT45lTkqxg6tJ/BgAAAAAAAGqixuFUYGCgnn76aR05ckRHjhzRs88+q8DA
wLqoDQAAAAAAANe5GodTixYtUkpKirp06aIuXbro0KFDev311+uiNgAAAAAAAFznavxrfX5+flq1
alVd1AIAAAAAAIB6ptrh1BdffKGYmBi9//77FZ4fMmRIrRUFAAAAAACA+qHa4dSKFSsUExOjhQsX
ljvncDgIpwAAAAAAAFBjDmOMsbsIAAAAAABwbRs+fLgkaf369TZXgutNjTdE79atW7WOAQAAAAAA
AJdT4w3Ri4qKynwuLCxUTk5OrRUEAAAAAACuPS1btrS7BFynqr2s7+9//7vmz5+vX375RTfddJN1
PDc3V+PGjdPixYvrrEgAAAAAAABcn6odTmVnZysrK0sJCQlatGiRddzX11fNmzevswIBAAAAAABw
/WJDdAAAAAAAANimxntOZWZmatasWdq7d6/y8vKs47t3767VwgAAAAAAAHD9q/Gv9T344IPy9/fX
f/7zHz333HNq1aqV7r777rqoDQAAAAAAANe5Gi/rc7lcSkpKUkhIiJKTk1VQUKCYmBht3bq1rmoE
AAAAAADAdarGM6caNWokSWrcuLF+/vlneXt76z//+U+tFwYAAAAAAIDrX433nOrQoYN+/vlnjR07
Vt27d5evr6/Cw8ProjYAAAAAAABc537Vr/V9/fXXOnPmjAYOHChv7xrnXAAAAAAAAKjnflU4BQAA
AAAAAPwa1Z7u1Lx5czkcjnLHjTFyOBw6ffp0rRYGAAAAAACA61+1w6mkpKQ6LAMAAAAAAAD1Ecv6
AAAAAAAAYJsa72Lerl27Cpf3/fTTT7VSEAAAAAAAAOqPGodTH374ofXPeXl5euutt3TrrbfWalEA
AAAAAACoH2plWV+PHj30zTff1EY9AAAAAAAAqEca/NoGfv75Z504caI2agEAAAAAAEAVEhISlJCQ
YHcZtarGy/o6d+5s7TlVXFysw4cP63//939rvTAAAAAAAACUdfLkSbtLqHU1Dqdefvnl/3ext7cC
AgL0u9/9rjZrAgAAAAAAQD1R43AqJiZGknTs2DE5HA6CKQAAAAAAAFyxGu85tXfvXnXq1EkhISEK
CQlRUFCQ9u7dWxe1AQAAAAAA4DpX43Bq4sSJmjNnjrKysnT69GnNmTNHEydOrIvaAAAAAAAAcJ2r
cTiVl5ene++91/o8YsQI5efn12pRAAAAAAAAqB9qHE516dJFHo/H+vzFF18oPDy8NmsCAAAAAABA
PVHjDdF3796tFStWyN/fX5KUlpamoKAgdenSxToPAAAAAAAAVEeNw6nXXnutLuoAAAAAAABAPVTj
cComJkaSlJ6eLklq1apV7VYEAAAAAACAeqPGe059//33cjqd1l9ISIh++OGHuqgNAAAAAAAA17ka
h1OTJ0/W008/raysLGVlZenpp59WQkJCXdQGAAAAAACA61yNw6msrCyNHj3a+nzfffcpKyurVosC
AAAAAABA/VDjcMrLy0sHDhywPh84cEBeXl61WhQAAAAAAADqhxpviP7CCy+oV69eCg0NlTFG+/fv
V2JiYl3UBgAAAAAAgOtcjcOpAQMG6Pvvv9f27dslSZGRkWrRokWtFwYAAAAAAIDrX43DKUnKzc3V
mTNn5HA4lJubW9s1AQAAAAAAoJ6o8Z5TK1euVOfOnbV+/XqtW7dOXbp00apVq+qiNgAAAAAAAFzn
ajxzas6cOdq1a5fatWsnSUpLS9PAgQN133331XpxAAAAAAAAuL7VeOZUkyZNrGBKkvz9/dWkSZNa
LQoAAAAAAAD1Q43DqbvvvluzZ8/WsWPHdPToUc2ZM0d//OMfdfbsWZ09e7YuagQAAAAAAMB1ymGM
MTW5oEGDyvMsh8Oh4uLiX10UAAAAAAAAyhs+fLgkaf369TZXUntqvOfUhQsX6qIOAAAAAAAA1EM1
XtYHAAAAAAAA1BbCKQAAAAAAANiGcAoAAAAAAAC2IZwCAAAAAACAbQinAAAAAAAAYBvCKQAAAAAA
ANiGcAoAAAAAAAC2IZwCAAAAAACAbQinAAAAAAAAYBvCKQAAAAAAANiGcAoAAAAAAAC2IZwCAAAA
AACAbX7z4dRzzz2niRMnWp+//vprORwOeTwe69ikSZM0aNAgjRo1SpKUlpamm2++2TrvcDh05swZ
SdLgwYN18ODBq1G6Jk6cKLfbfVXudaUWLVqkBQsWSJKWLl2qoUOHSpJ27dpl9eeVSktL06JFi35t
iRUqXeuVev/99/X4449XeG7//v3y9/f/Ve1XxePx6JNPPqmz9gEAAAAAuFZ4213ArxUbG6sJEyZY
n91ut7p37y6Px6PevXtbxxYtWqTY2NjLtrdx48Zaq62oqEje3pV38Ztvvllr96orkyZNqvB4RESE
Vq9e/avaLgmnKruH3YYMGaIhQ4bYcm+Px6MzZ85o4MCBV3R9fn6+8vPzyxzz8fGRj49PbZQHAAAA
AECt+c3PnIqMjFR6erqOHTsm6eK/1M+cOdOaOZWRkaEjR44oPz9fLpfrsu35+/srKSlJkjR37lx1
6tRJLpdLLpdLhw8fliR9+umn6tKli0JDQxUTE6MDBw5Y93Y6nXrwwQflcrn07rvvyt/fXzNnzlRU
VJTatWunuXPnWvfq3bu3NmzYIOliUBUUFCSXy6WQkBBt37690ho9Ho+Cg4M1btw4BQcHKzw83KpZ
khYsWCCn06mQkBCNGTNG2dnZkqTCwkI9+eST6tatm1wul0aOHKmsrCxJUnZ2tiZOnKjg4GCFhYVZ
gd/s2bM1derUCmso6c+SmWizZs1SeHi42rdvXybkq6y/Jk2apIMHD8rlclkhUOn+ly6GYCXvsrL3
UR1vvfWWunfvri5duqhXr17au3evpIszrPr06aMhQ4YoKChIvXr1UlpamnWu9Oyr2bNnKzAwUOHh
4Vq1alW59kNDQxUaGqq7775bx48ft9ro16+f/vznPyskJEQRERH66aefrOsqeldJSUlatGiREhMT
5XK5NGfOHBUVFWnAgAGKiIiQ0+nU6NGjde7cuUqfd968eWrWrFmZv3nz5lW7vwAAAAAAuFp+8+FU
o0aN1KNHD7ndbuXn5ys1NVWDBw/WsWPHlJeXJ7fbraioKDVu3LhG7WZlZemll17S7t27lZSUpG++
+UYtW7ZUZmamRo8erWXLlmnfvn166KGHNGLECBljJEnff/+9xo0bp6SkJN17772SpDNnzmjr1q3a
uXOnFixYYAUXpT3xxBPavHmzkpKStHv3bjmdzirr++677xQfH6/9+/dr+vTpuu+++2SM0ccff6x/
//vf2rJli5KTk3XjjTfqySeflHQxCLnxxhu1Y8cOJSUlKSQkRM8884wkaerUqWrUqJH27dunvXv3
6u9//3uN+is7O1uhoaH69ttv9dprr1nL4arqr0WLFumuu+5SUlKS3n///St6H9WxZcsWvf322/ry
yy+1e/duPf/88xo9enSZ83//+9914MAB/eEPf9BDDz1Uro2PPvpIa9eu1bfffqtdu3ZZAZZ0cYnf
//zP/+jjjz/Wvn371KNHjzJLTXfu3KkXXnhBycnJ6tevn9W3lb0rl8ulSZMmacyYMUpKStLMmTPl
5eWllStXateuXdq/f7+aNWumV199tdJnnjFjhrKzs8v8zZgxo1r9BQAAAADA1fSbD6eki0v7PB6P
tm/frm7dukm6OKNq69at8ng81VrOdylfX18FBgZq7NixWrx4sU6fPq3GjRtr+/btCgkJUUhIiCRp
zJgxSk9PtwKngIAAxcTElGmrJAhp0aKFAgIClJqaWu5+ffv21f33369XXnlFqampatq0aZX1+fv7
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
RU5ErkJggg==
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-ef4c78d0-97bd-4cb5-8359-c4d85f8f0bf2");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-ef4c78d0-97bd-4cb5-8359-c4d85f8f0bf2"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>


    <string>:5: FutureWarning: 
    
    Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `y` variable to `hue` and set `legend=False` for the same effect.
    



      <div class="colab-quickchart-chart-with-code" id="chart-2492ecee-022b-4bcd-8925-622ee1222a09">
        <img style="width: 180px;" src="data:image/png;base64,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
GGNGjhxpEhISTH5+vjHGmLS0NGOMMdOmTTMTJkwwxhizcOFC89BDD1k1XN6fksyqVauMMcZ89NFH
pk2bNsYYc9X+Kmzj8v7avXu39T0kJMS43e4Sn0dJ5s6da2JjY01OTo7JyckxsbGxJioqyqr9188p
Li7OzJkzxxhjTFBQkNmyZYsxxpj8/HyrL3v06GGdk5uba3r27GlWrFhxRd2tW7c2X3/9tTHGmOTk
ZNO0aVOTm5trkpOTTXh4uMnIyDDGGPP999+bpk2bmqysLON2u42Xl5fZtm2bMcaYt956y/To0cPq
38vfV2MuvTs9e/Y0OTk5VxyzZs0a61xjjPn5559L7CdjLj3jRo0ameDgYOuzfPnyEo+VVOQzbdq0
UtsHgOtB//79Tf/+/e0uAwAAANfQNZ0jFR0dLY/Ho7vvvlthYWGSLo2o2rp1qzwejzWdrzx8fX3V
unVrDRs2TD169NADDzygFi1a6NNPP1VgYKA1Ombo0KH605/+pB9//FGS5O/vr6ioqCJtDRkyRJLU
qFEj+fv768iRI2revHmRY2JiYjR8+HA9+OCD6t27t9q0aVNqfX5+foqJiZEkDRo0SI899piOHz+u
TZs2afDgwdYomoSEBD3yyCOSpHXr1ikjI0OrV6+WJOXk5MjPz0+StGHDBm3fvl01alwa9Na4ceNy
9ZePj481ZSIiIkLJycmSpO3bt5faX2VV0vMoyebNmzVs2DDVrFlTkhQXF6cFCxZY+4t7ToViYmI0
YcIEDRw4UD169JDL5dL58+e1efNmnT592jrul19+KXadqVGjRmnhwoUKDQ3VO++8o6FDh8rb21sf
f/yxDh8+rPvvv986tkaNGjp27JgkqVWrVurcubOkS3346quvltonl9/f5YKDg3Xw4EGNGzdOUVFR
6tOnT6ntSJeeS1mm9U2ZMkVPPfVUkW21a9e+6nkAAAAAAFxr1zyc+ve//6277rrLmroVFRUlt9st
t9utRYsWKTc3t1xtenl5adu2bdqyZYs8Ho/Cw8O1bNmyq55Xr169K7b5+PgUaTcvL++KY1avXq1v
vvlGHo9Hffr00cyZM/Xoo4+WuV6HwyGHw1Hs9kLGGL355puVOs2rUO3ata1reXl5KT8/v0LteHt7
Fzk3KyvLarO451HWRcB/3TfFPadCr732mr799lu53W7FxcVp6NChGjdunCRp27ZtRZ5nceLi4hQc
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
mTNnWtfq2rWr1q1bJ+lSUNW+fXu5XC4FBgZq+/btJdbo8XgUEBCgESNGKCAgQCEhIVbNkjR79mw5
nU4FBgZq6NChysjIkCTl5ubq2WefVVhYmFwulwYNGqT09HRJUkZGhuLj4xUQEKDg4GAr8Js+fbom
TpxYbA2F/Vk4Em3atGkKCQlRq1atioR8JfXX2LFjdejQIblcLisEurz/pUshWOGzLOl5lMW7776r
zp07q2PHjrr//vu1Z88eSZdGWHXr1k19+/ZV+/btdf/99yslJcXad/noq+nTp6t169YKCQnR8uXL
r2g/KChIQUFBeuCBB/Tjjz9abcTGxur//u//FBgYqNDQUP3www/WecU9q6SkJM2bN0+JiYlyuVya
MWOG8vLy1LNnT4WGhsrpdGrIkCE6f/58iff78ssvq379+kU+L7/8cpn7CwAAAACAa+WGD6dq1aql
++67T263W9nZ2Tpy5Ij69OmjEydOKCsrS263WxEREfLx8SlXu+np6Xr11Ve1a9cuJSUlacuWLWrS
pInS0tI0ZMgQvfPOO9q7d68ee+wxDRw4UMYYSdLBgwc1YsQIJSUl6ZFHHpEknT17Vlu3btXXX3+t
2bNnW8HF5Z5++mlt3rxZSUlJ2rVrl5xOZ6n1ffvtt4qLi9P+/fs1efJkPfroozLG6KOPPtJ//vMf
ffXVV9q3b5/q1q2rZ599VtKlIKRu3brasWOHkpKSFBgYqOeff16SNHHiRNWqVUt79+7Vnj179I9/
/KNc/ZWRkaGgoCB98803mjt3rjUdrrT+mjdvnu69914lJSVp/fr1FXoeZfHVV19p2bJl+vzzz7Vr
1y699NJLGjJkSJH9//jHP3TgwAH94Q9/0GOPPXZFGx9++KFWrlypb775Rjt37rQCLOnSFL+//OUv
+uijj7R3717dd999Raaafv311/r73/+uffv2KTY21urbkp6Vy+XS2LFjNXToUCUlJWnq1Kny8vLS
0qVLtXPnTu3fv1/169fXm2++WeI9T5kyRRkZGUU+U6ZMKVN/AQAAAABwLd3w4ZR0aWqfx+PR9u3b
FRYWJunSiKqtW7fK4/GUaTrfr/n6+qp169YaNmyY5s+frzNnzsjHx0fbt29XYGCgAgMDJUlDhw5V
amqqFTj5+/srKiqqSFuFQUijRo3k7++vI0eOXHG9mJgYDR8+XG+88YaOHDmievXqlVqfn5+fYmJi
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
Z7V161Z9/fXXmj17tn788ccrrvf0009r8+bNSkpK0q5du+R0OkusbdSoUVq0aJH1feHChUVGjxWa
MWOGateure+++04ffvihPv/88zLde4MGDfTBBx/om2++0d69e5WSkqIVK1Zc9bzk5GS53W7t379f
n3zyibZu3VpsTbfccosOHjyojRs3asuWLUX2b9++XX//+9+1b98+RUREFNk3fvx4RURE6MCBA1q8
eHGRqZtlVZ57e/nll1W/fv0in5dffrnc1wQAAAAAoKoRTtksOjpaHo9H27dvV1hYmKRLI6q2bt0q
j8dTpul8v+br66vWrVtr2LBhmj9/vs6cOSMfHx9t375dgYGBCgwMlCQNHTpUqampVuDk7++vqKio
Im0NGTJEktSoUSP5+/vryJEjV1wvJiZGw4cP1xtvvKEjR46oXr16JdbWv39/bdu2TSdPntQvv/yi
DRs2WNe43ObNmzVmzBg5HA7Vr1+/2GOKU1BQoMmTJys4OFgdOnTQzp07rdFkpRk8eLC8vb11yy23
yOVyKTk5udiaRo0aJYfDoVtvvdVaB6zQfffdp3vvvbfY9jdv3myFcE2bNtUf/vCHMt1PRe9typQp
ysjIKPKZMmVKua8JAAAAAEBVI5yyWXR0tNxut9xutzU1LSoqytrWrVu3crfp5eWlbdu2aeLEiUpL
S1N4eLi++OKLq55XXKh0+agtLy8v5eXlXXHM6tWrNWvWLOXm5qpPnz5avnx5ide45ZZb9Mgjj+jd
d9/VypUr1a1bN91+++1Xrc3hcFh/e3t7Kz8/3/qelZVl/f3aa68pLS1N27dv1969ezVkyJAi+0tS
lvssrSap+P4ry7ml3c/lynNvtWvXlq+vb5FP7dq1y1wfAAAAAADXCuGUzTp16qS0tDQlJiYWCaeW
L1+ukydPWqOpyiMzM1OnT59Wly5d9MILLygyMlK7d+9WeHi49u3bp/3790uSli9frubNm6t58+YV
rj8vL0/JyckKDQ3VpEmTNHDgQO3YsaPUc0aNGqWFCxdq0aJFxU7pk6TY2FgtXLhQxhidO3dOy5Yt
s/a1atVKe/fu1cWLF5WXl6elS5da+9LT09W0aVP5+Pjo1KlTWrlyZYXv7de6deumd955R8YY/fLL
L2WaLnj5uYXTGU+fPq0NGzYUuZ+dO3cqPz9fFy5c0OrVq4ttoyrvDQAAAAAAu/BrfTarWbOmIiMj
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
YII=
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-2492ecee-022b-4bcd-8925-622ee1222a09");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-2492ecee-022b-4bcd-8925-622ee1222a09"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>


    <string>:5: FutureWarning: 
    
    Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `y` variable to `hue` and set `legend=False` for the same effect.
    



      <div class="colab-quickchart-chart-with-code" id="chart-8ba34ad3-e449-4845-8731-845d082e5279">
        <img style="width: 180px;" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABK0AAAGlCAYAAAA4UgUYAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90
bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAP
YQAAD2EBqD+naQAARINJREFUeJzt3XtUVXX+//HXERRLRS3JUr+TMoKXw+UoqNBgiBdMK0uHtPGG
FybFb3mZnDGbSc2xqUbXWN/6lnbTTLxkmZVZrrRzuqmoJV6y/KYDXkLFUUS8cPXz+8PF/kkCgoJs
5flY66zl2ZfPfu/PPnsdffnZn+MwxhgBAAAAAAAANlKrugsAAAAAAAAAfo3QCgAAAAAAALZDaAUA
AAAAAADbIbQCAAAAAACA7RBaAQAAAAAAwHYIrQAAAAAAAGA7hFYAAAAAAACwHUIrAAAAAAAA2A6h
FQAAAAAAAGyH0AoAAAAAAAC2Q2gFAAAAAAAA2yG0AgAAAAAAgO0QWgEAAAAAAMB2CK0AAAAAAABg
O4RWAAAAAAAAsB1CKwAAAAAAANgOoRUAAAAAAABsh9AKAEqQmJioxMTE6i4DAAAAAGos7+ouAADs
6OjRo9VdAgAAAADUaIy0AgAAAAAAgO0QWgEAAAAAAMB2CK0AAAAAAABgO4RWAAAAAAAAsB1CKwAA
AAAAANgOoRUAAAAAAABsh9AKAAAAAAAAtkNoBQAAAAAAANshtAIAAAAAAIDtEFoBAAAAAADAdgit
AAAAAAAAYDuEVgAAAAAAALAdQisAAAAAAADYDqEVAAAAAAAAbIfQCgAAAAAAALZDaAUAAAAAAADb
IbQCAAAAAACA7RBaAQAAAAAAwHYIrQAAAAAAAGA7hFYAAAAAAACwHUIrAAAAAAAA2A6hFQAAAAAA
AGyH0AoAAAAAAAC2Q2gFAAAAAAAA2yG0AgAAAAAAgO0QWgEAAAAAAMB2CK0AAAAAAABgO4RWAAAA
AAAAsB1CKwAAAAAAANgOoRUAAAAAAABsh9AKAAAAAAAAtkNoBQAAAAAAANshtAIAAAAAAIDtXLPQ
6umnn1ZCQoL1/ptvvpHD4ZDH47GWjR07Vn369NGgQYMkSWlpaWrUqJG13uFw6OTJk5Kkvn37as+e
PdeidCUkJMjtdl+TY12pefPmafbs2ZKkhQsX6sEHH5Qkbd261erPK5WWlqZ58+ZdbYmVYtq0aUpK
Srri/avyc3Py5Ek999xzZW6Tnp6url27XlH7M2bMkJ+fn1wul/V65plnrqgtAAAAAADszmGMMdfi
QF999ZVGjRqlvXv3SpL+/ve/65NPPtE999yjGTNmSJLatGmjefPmKSYmRtKFsMTlcllBlcPhUGZm
ZrEgqzIUFBTI29u7UtusTgsXLtSqVau0atWqSmnP4/Fo4sSJSklJqZT2rpTdr9OvP6+/drX1z5gx
QydPntQLL7xwxW2g/AYMGCBJWrlyZTVXAgAAAAA10zUbaRUREaH09HQdOnRI0oUgZNq0adZIq8OH
D+vAgQPKzc2Vy+W6bHstW7a0QpRZs2apXbt21uiT/fv3S5LWrl2rjh07KiQkRNHR0dq9e7d1bKfT
qdGjR8vlcumDDz5Qy5YtNW3aNEVGRqpVq1aaNWuWdaxu3bpZAdAbb7yh9u3by+VyKTg4WMnJyaXW
6PF4FBQUpOHDhysoKEhhYWHFgp/Zs2fL6XQqODhYQ4YMUVZWliQpPz9fTzzxhDp37iyXy6WBAwcq
MzNTkpSVlaWEhAQFBQUpNDRUo0aNknQh0Jg4cWKJNRT1Z9HItenTpyssLEytW7fWmjVrrG1L66+x
Y8dqz549crlc6tev3yX9L0nh4eHWtSztepQkPz9f48aNU2BgoCIiIvT444+rW7dupV6nESNGWKHN
xx9/rJCQELlcLgUFBenDDz+UJB05ckQDBw5U586dFRwcrL/97W/W8Yrq/vbbbxUcHFyslm7dullt
rF27VlFRUQoLC1Pnzp2tkXZF13TcuHEKDQ2V0+nU1q1brX7Kzs6Wy+VSeHi41eb48eMVGRmp2NjY
YqMHz507p0GDBql9+/YKDQ1VbGxsqf1UUbm5uTp16lSxV25ubqW1DwAAAABAlTPXUI8ePcyiRYtM
Tk6OadWqlTHGmN/+9rfm3LlzJikpycTExBi3221CQ0ONMcakpqaahg0bWvtLMpmZmcYYY+68806z
bds2c+LECdOwYUNz9uxZY4wxZ86cMefOnTNHjx41t9xyi9mxY4cxxpjFixebdu3amfPnzxu3220c
DofxeDxW23feead57LHHjDHGHDt2zPj6+ppDhw4ZY4yJjo42H3zwgTHGGF9fX5Oenm6MMSYvL89k
Z2eXer5ut9tIMuvWrTPGGLN8+XLTpk0bc/78ebNmzRrTtm1b63z++Mc/mrFjxxpjjHnmmWfMzJkz
rXZmzpxpxo0bZ4wxZsSIESYxMdEUFhYaY4zJyMgwxhgzffp0M2HCBGOMMQsWLDAPPPCAVcPF/SnJ
vPfee8YYYz799FMTGBhojDGX7a+iNi7ur23btlnvw8LCjNvtLvV6lObll182PXv2NHl5eSYvL8/0
7NnTREdHW7X/+jrFx8ebuXPnGmOMCQkJMRs2bDDGGFNYWGj1ZWxsrLVPfn6+6d27t3n33XcvqTsg
IMBs2bLFGGPMvn37zO23327y8/PNvn37TEREhMnKyjLGGPPzzz+b22+/3eTk5Bi32228vLzMpk2b
jDHGvPrqqyY2Ntbq34s/r8Zc+Oz07t3b5OXlXbLNypUrrX2NMeb48eOl9pMxF65xkyZNTGhoqPVa
tmxZqdtKKvaaPn16me2juP79+5v+/ftXdxkAAAAAUGNd02etYmJi5PF4dOedd6pz586SLozA2rhx
ozwej/VYYEX4+voqICBAQ4cOVWxsrO699161aNFCn3/+uYKDg63RNEOGDNF///d/65dffpEk+fv7
Kzo6ulhbgwcPliQ1adJE/v7+Sk1NVfPmzYtt06NHDw0bNkz333+/+vTpo8DAwDLra9mypXr06CFJ
GjhwoB555BEdPHhQ69at06BBg6xRN4mJiXrooYckSatWrVJWVpbef/99SVJeXp5atmwpSVq9erWS
k5NVq9aFQXJ+fn4V6q+6detajz1FRkZq3759kqTk5OQy+6u8SrsepVm/fr2GDh2q2rVrS5Li4+P1
xhtvWOtLuk5FevTooQkTJiguLk6xsbFyuVw6c+aM1q9fr6NHj1rbnT59usR5rEaOHKkFCxYoPDxc
b7/9toYMGSJvb2999tln2rt3r+6++25r21q1aunAgQOSpNatW6tLly6SLvThnDlzyuyTi8/vYqGh
ofrxxx81btw4RUdHq2/fvmW2I124LuV5PHDq1Kn605/+VGyZj4/PZfcDAAAAAMAurnlo9eabb+o3
v/mN9QhYdHS03G633G63Fi5cqPz8/Aq16eXlpU2bNmnDhg3yeDyKiIjQ0qVLL7tf/fr1L1lWt27d
Yu0WFBRcss3777+v7777Th6PR3379tWsWbP08MMPl7teh8Mhh8NR4vIixhi99NJLlfq4WBEfHx/r
WF5eXiosLLyidry9vYvtm5OTY7VZ0vUo7+Tjv+6bkq5TkX/961/64Ycf5Ha7FR8fryFDhmjcuHGS
pE2bNhW7niWJj49XaGio5syZo0WLFmn16tWSLvR/r169tGTJkkv2+eWXX8r1OSnPOfj7+2v37t36
4osvtG7dOv3lL39RSkqKGjduXGZ75eHj40NIBQAAAAC4rl2zOa0kqVOnTsrIyFBSUlKx0GrZsmU6
fPiwNfqqIrKzs3X06FF17dpVTz31lKKiorRt2zZFRERo586d2rVrlyRp2bJlat68+SUjpyqioKBA
+/btU3h4uCZPnqy4uDht3ry5zH3S0tKs+ZDee+89NW3aVC1atFDPnj317rvv6tSpU5Kk+fPnWyHV
gw8+qLlz5+rs2bOSpLNnz+qHH36QJPXr109z5szR+fPnJUnHjh274vO5WFn95evra823VaR169bW
fF6bN2+2RjKVdj1K0717dy1ZskT5+fnKz8/XokWLyl3zTz/9JKfTqUcffVSJiYnatGmT6tevr5iY
mGK/4nfxXGoXa9asmTp16qRJkybptttuk9PplCT17t1b69at044dO6xtL3edpQujzM6dO6e8vLxy
1X/o0CE5HA7rmhpjdPDgwXLtCwAAAADAje6ajrSqXbu2oqKitH37drVt21aSFBgYqOzsbEVFRZX4
CNXlZGVlKS4uTmfOnJHD4VBAQIDi4+PVsGFDJSUlafjw4SooKFDjxo21YsWKEkc5lVdhYaFGjRql
EydOyNvbW35+flqwYEGZ+zidTi1cuFDjx49XnTp1tHTpUjkcDvXp00e7du1SZGSkatWqpZCQEL3y
yiuSpClTpig3N1ddunSx6p0yZYqcTqfmzp2rSZMmKTg4WLVr11anTp30+uuvX/E5FfHz8yu1v0JC
QuR0OhUUFCR/f3999NFHmjVrluLj4zV//nxFRkZagU9p16M0Y8aM0c6dO9W+fXs1btxY4eHhSk9P
L1fNTz75pPbs2aM6dero5ptv1quvvipJSkpK0p/+9CcFBQXJ4XCoXr16mj9/fomPKY4cOVIDBw60
9pUuBHJLlizRmDFjdPbsWeXl5alDhw4ljry62C233KLhw4crJCRE9evXtyZoL83OnTs1depUGWNU
UFCgYcOGKSQkpMx9kpKSrAnvpQujF+fOnVvmPgAAAAAAXI8cxhhT3UXcqDwejyZOnFjsV/Zwqezs
bDVo0ED5+fkaMmSIwsLCNGXKlOouCzVc0dxvK1eurOZKAAAAAKBmuqYjrYCS9OzZU7m5ucrJyVFU
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
l5e4zcViYmK0ceNG5eXl6ZtvvlFUVJSio6Pl8Xi0ZcsWNW3atNQ5rXx8fOTr61vsxaOBAAAAAIDr
SZWHVm63W26323oELDo62lrWvXv3Crfp5eWlTZs2aeLEicrIyFBERES55gwqKWy6OFzx8vJSQUHB
Jdu8//77eu6555Sfn6++fftq2bJlFarX4XDI4XCUuLyIMUYvvfSSUlJSlJKSot27d1faXEw+Pj7W
sby8vFRYWHhF7Xh7exfbNycnx2rzSq5HkV/3TVmh4L/+9S8tWLBAN998s+Lj4/XPf/5TxhhJFyYl
L+q/vXv3FntEsEh8fLzeffddnTt3TosWLdLIkSMlXej/Xr16WfunpKTol19+UUBAgKTyfU7Kcw7+
/v7avXu37rnnHn377bcKCgqyHgMtSfPmzdWiRQstX75ct956q+rXr6+77rpL33777RXfPwAAAAAA
XC+qNLTq1KmTMjIylJSUVCy0WrZsmQ4fPmyNvqqI7OxsHT16VF27dtVTTz2lqKgobdu2TREREdq5
c6d27dolSVq2bJmaN2+u5s2bX3H9BQUF2rdvn8LDwzV58mTFxcVp8+bNZe6TlpZmzYf03nvvqWnT
pmrRooV69uypd999V6dOnZIkzZ8/35rT6MEHH9TcuXN19uxZSdLZs2f1ww8/SJL69eunOXPm6Pz5
85KkY8eOXfH5XKys/vL19bXm2yrSunVraz6vzZs3WyOZSrsepenevbuWLFmi/Px85efna9GiReWu
+aeffpLT6dSjjz6qxMREbdq0SfXr11dMTEyxX/G7eC61izVr1kydOnXSpEmTdNttt8npdEqSevfu
rXXr1mnHjh3Wtpe7ztKFUWbnzp0r9gt/ZTl06JAcDod1TY0xOnjwYJn7xMTE6O9//7s1+uzmm2/W
bbfdprfffvuKRioCAAAAAHC9qLJfD5Sk2rVrKyoqStu3b1fbtm0lSYGBgcrOzlZUVNRlH48qSVZW
luLi4nTmzBk5HA4FBAQoPj5eDRs2VFJSkoYPH66CggI1btxYK1asKHGUU3kVFhZq1KhROnHihLy9
veXn52c9XlYap9OphQsXavz48apTp46WLl0qh8OhPn36aNeuXYqMjFStWrUUEhKiV155RZI0ZcoU
5ebmqkuXLla9U6ZMkdPp1Ny5czVp0iQFBwerdu3a6tSpk15//fUrPqcifn5+pfZXSEiInE6ngoKC
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
2zq20+nU6NGj5XK59MEHH6hly5aaNm2aIiMj1apVK82aNcs6Vrdu3bRq1SpJFwKs9u3by+VyKTg4
WMnJyaXW6PF4FBQUpOHDhysoKEhhYWFWzZI0e/ZsOZ1OBQcHa8iQIcrKypIk5efn64knnlDnzp3l
crk0cOBAZWZmSpKysrKUkJCgoKAghYaGWkHgjBkzNHHixBJrKOrPopFr06dPV1hYmFq3bl0s/Cut
v8aOHas9e/bI5XJZ4dDF/S9dCMeKrmVp16M83nnnHXXp0kUdO3bU3Xffre3bt0u6MCKre/fu6tev
n9q3b6+7775baWlp1rqLR2vNmDFDAQEBCgsL07Jlyy5pPyQkRCEhIbr33nv1yy+/WG307NlTf/jD
HxQcHKzw8HD9+9//tvYr6VqlpKRo3rx5SkpKksvl0syZM1VQUKDevXsrPDxcTqdTgwcP1pkzZ0o9
32effVYNGzYs9nr22WfL3V8AAAAAAFS36z60qlOnju666y653W7l5uYqNTVVffv21aFDh5STkyO3
263IyEjVrVu3Qu1mZmZqzpw5+v7775WSkqINGzaoadOmysjI0ODBg/X2229rx44deuSRRxQXFydj
jCTpxx9/1PDhw5WSkqKHHnpIknTy5Elt3LhRW7Zs0ezZs61A42KPP/641q9fr5SUFH3//fdyOp1l
1vfDDz8oPj5eu3bt0pQpU/Twww/LGKNPP/1Ub731lr799lvt3LlT9erV0xNPPCHpQkBSr149bd68
WSkpKQoODtbf/vY3SdLEiRNVp04d7dixQ9u3b9fzzz9fof7KyspSSEiIvvvuO7388svWY3Vl9de8
efPUpk0bpaSk6KOPPrqi61Ee3377rZYuXaqvvvpK33//vZ555hkNHjy42Prnn39eu3fv1n333adH
HnnkkjY++eQTrVixQt999522bt1qBVvShUcF//znP+vTTz/Vjh07dNdddxV7ZHXLli36xz/+oZ07
d6pnz55W35Z2rVwul8aOHashQ4YoJSVF06ZNk5eXl5YsWaKtW7dq165datiwoV566aVSz3nq1KnK
ysoq9po6dWq5+gsAAAAAADu47kMr6cIjgh6PR8nJyercubOkCyOwNm7cKI/HU67HAn/N19dXAQEB
Gjp0qObPn68TJ06obt26Sk5OVnBwsIKDgyVJQ4YMUXp6uhVE+fv7Kzo6ulhbRQFJkyZN5O/vr9TU
1EuO16NHDw0bNkwvvviiUlNTVb9+/TLra9mypXr06CFJGjhwoI4cOaKDBw9q3bp1GjRokDVnV2Ji
oj7//HNJ0qpVq7R48WJrpNLSpUutWlavXq3JkyerVq0LHwk/P78K9VfdunU1YMAASVJkZKT27dsn
SZftr/Iq7XqUx4cffqjt27erS5cucrlceuyxx3TixAmdO3dOknTXXXepXbt2kqRHHnlEHo9HhYWF
xdpYv369Bg4cKF9fXzkcDo0ZM8Za53a7dc8996h58+aSpHHjxumLL76w2igaZffrvinrWv2aMUZz
585Vhw4dFBISok8++aTYiLRf8/Hxka+vb7EXjwYCAAAAAK4nN0xo5Xa75Xa7rXmsoqOjrWXdu3ev
cJteXl7atGmTJk6cqIyMDEVEROjrr7++7H4lhU0XhyteXl4qKCi4ZJv3339fzz33nPLz89W3b99L
Hj+7HIfDIYfDUeLyIsYYvfTSS0pJSVFKSop2795daXN4+fj4WMfy8vK6JPQpL29v72L75uTkWG1e
yfWQLpx3fHy8dd4pKSk6fPiwbrrppiuqUVKJfV3auvJc/8u1uWTJEn3xxRf68ssvtXPnTk2ePNnq
GwAAAAAAbkQ3RGjVqVMnZWRkKCkpqVhotWzZMh0+fNgafVUR2dnZOnr0qLp27aqnnnpKUVFR2rZt
myIiIrRz507t2rVLkrRs2TI1b97cGmVzJQoKCrRv3z6Fh4dr8uTJiouL0+bNm8vcJy0tzfrlwffe
e09NmzZVixYt1LNnT7377rs6deqUJGn+/PmKjY2VJD344IOaO3euzp49K0k6e/asfvjhB0kXJh2f
M2eOzp8/L0k6duzYFZ/PxcrqL19fX2u+rSKtW7e25vPavHmz9UuOpV2P8ujXr58WL16sAwcOSJLO
nz+vrVu3Wus3btyon376SdKFucViYmLk5eVVrI2ePXtqxYoVys7OljFGr732mrUuJiZGn332mdLT
0yVd+MXFHj16XNLGr5V1rX7dN5mZmWrSpIl8fX2VnZ2thQsXluvcAQAAAAC4Xl33vx4oSbVr11ZU
VJS2b9+utm3bSpICAwOVnZ2tqKgo1a5du8JtZmVlKS4uTmfOnJHD4VBAQIDi4+PVsGFDJSUlafjw
4SooKFDjxo21YsWKMkfJXE5hYaFGjRqlEydOyNvbW35+flqwYEGZ+zidTi1cuFDjx49XnTp1tHTp
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
kZGhwYMH6+2339aOHTv0yCOPKC4uTsYYSdKPP/6o4cOHKyUlRQ899JAk6eTJk9q4caO2bNmi2bNn
65dffrnkeI8//rjWr1+vlJQUff/993I6naXWNnLkSC1cuNB6v2DBgmKjzYrMnDlTPj4++umnn/TJ
J5/oq6++Kte5N2rUSB9//LG+++477dixQ2lpaXr33Xcvu9++ffvkdru1a9curV27Vhs3biyxpptu
ukk//vij1qxZow0bNhRbn5ycrH/84x/auXOnIiMji60bP368IiMjtXv3bi1atKjYI6DlVZFze/bZ
Z9WwYcNir2effbbCxwQAAAAAoLoQWlWzmJgYeTweJScnq3PnzpIujMDauHGjPB5PuR4L/DVfX18F
BARo6NChmj9/vk6cOKG6desqOTlZwcHBCg4OliQNGTJE6enpVhDl7++v6OjoYm0NHjxYktSkSRP5
+/srNTX1kuP16NFDw4YN04svvqjU1FTVr1+/1Nr69++vTZs26fDhwzp9+rRWr15tHeNi69ev1+jR
o+VwONSwYcMStynJ+fPnNWXKFIWGhqpDhw7aunWrNfqsLIMGDZK3t7duuukmuVwu7du3r8SaRo4c
KYfDoQYNGljzjBW566671KZNmxLbX79+vRXO3X777brvvvvKdT5Xem5Tp05VVlZWsdfUqVMrfEwA
AAAAAKoLoVU1i4mJkdvtltvtth5xi46OtpZ17969wm16eXlp06ZNmjhxojIyMhQREaGvv/76svuV
FDZdPMrLy8tLBQUFl2zz/vvv67nnnlN+fr769u2rZcuWlXqMm266SQ899JDeeecdrVixQt27d9et
t9562docDof1Z29vbxUWFlrvc3JyrD//61//UkZGhpKTk7Vjxw4NHjy42PrSlOc8y6pJKrn/yrNv
WedzsYqcm4+Pj3x9fYu9fHx8yl0fAAAAAADVjdCqmnXq1EkZGRlKSkoqFlotW7ZMhw8ftkZfVUR2
draOHj2qrl276qmnnlJUVJS2bdumiIgI7dy5U7t27ZIkLVu2TM2bN1fz5s2vuP6CggLt27dP4eHh
mjx5suLi4rR58+Yy9xk5cqQWLFighQsXlvhooCT17NlTCxYskDFGp06d0tKlS611rVu31o4dO3Tu
3DkVFBRoyZIl1rrMzEzdfvvtqlu3ro4cOaIVK1Zc8bn9Wvfu3fX222/LGKPTp0+X67HDi/cteizy
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
hLrKGpUAAAAASUVORK5CYII=
">
      </div>
      <script type="text/javascript">
        (() => {
          const chartElement = document.getElementById("chart-8ba34ad3-e449-4845-8731-845d082e5279");
          async function getCodeForChartHandler(event) {
            const chartCodeResponse =  await google.colab.kernel.invokeFunction(
                'getCodeForChart', ["chart-8ba34ad3-e449-4845-8731-845d082e5279"], {});
            const responseJson = chartCodeResponse.data['application/json'];
            await google.colab.notebook.addCell(responseJson.code, 'code');
          }
          chartElement.onclick = getCodeForChartHandler;
        })();
      </script>
      <style>
        .colab-quickchart-chart-with-code  {
            display: block;
            float: left;
            border: 1px solid transparent;
        }

        .colab-quickchart-chart-with-code:hover {
            cursor: pointer;
            border: 1px solid #aaa;
        }
      </style>


    WARNING: Runtime no longer has a reference to this dataframe, please re-run this cell and try again.



```
# Define window parameters
window_size = 20000
step_size = 5000

# Prepare summary and per-window data
results = []
window_pi_data = []
```


```
for pop_name, group in metadata.groupby('population'):
    pop_samples = group['sample'].tolist()
    pop_idx = [np.where(samples == s)[0][0] for s in pop_samples if s in samples]

    if len(pop_idx) < 2:
        print(f"Skipping {pop_name}: not enough samples.")
        continue

    # Subset
    gt_pop = gt[:, pop_idx]
    ac = gt_pop.count_alleles()
    gn = gt_pop.to_n_alt()

    # Sort
    sort_idx = np.argsort(positions)
    pos_sorted = positions[sort_idx]
    ac_sorted = ac[sort_idx]
    gn_sorted = gn[sort_idx]

    # Overall stats
    pi = allel.sequence_diversity(pos_sorted, ac_sorted)
    S = ac_sorted.count_segregating()
    theta_w = allel.watterson_theta(pos_sorted, ac_sorted)
    tajima_d = allel.tajima_d(gn_sorted)

    results.append({
        "population": pop_name,
        "n_samples": len(pop_idx),
        "pi": pi,
        "S": S,
        "theta_w": theta_w,
        "tajima_d": tajima_d
    })

# Sliding window π
pi_w = allel.windowed_diversity(pos_sorted, ac_sorted, size=window_size, step=step_size)

# Store each window value
for val in pi_w:
    window_pi_data.append({
        "population": pop_name,
        "pi_window": val
    })
```


```
# Overall stats
df_stats = pd.DataFrame(results).sort_values("population")

# Per-window π values for boxplot
df_windows = pd.DataFrame(window_pi_data)
```


```
# Ensure pi_window is float and drop any NaNs
df_windows['pi_window'] = pd.to_numeric(df_windows['pi_window'], errors='coerce')
df_windows = df_windows.dropna(subset=['pi_window'])
```


```
import seaborn as sns
plt.figure(figsize=(12, 6))
sns.boxplot(data=df_windows, x="population", y="pi_window")
plt.title("Sliding Window π per Population")
plt.ylabel("π (per window)")
plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()
```


    
![png](Estat%C3%ADsticas_files/Estat%C3%ADsticas_19_0.png)
    



```
print(df_windows.shape)
df_windows.head()
```

    (0, 2)






  <div id="df-909c3548-ec15-49b4-9205-1ea8605f364e" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>population</th>
      <th>pi_window</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-909c3548-ec15-49b4-9205-1ea8605f364e')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-909c3548-ec15-49b4-9205-1ea8605f364e button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-909c3548-ec15-49b4-9205-1ea8605f364e');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    </div>
  </div>





```
pi_w = allel.windowed_diversity(pos_sorted, ac_sorted, size=window_size, step=step_size)
```


```
print(f"{pop_name}: {len(pi_w)} windows")
print("Example values:", pi_w[:5])
```

    Willisornis_vidua_nigrigula: 4 windows
    Example values: (array([0.00264343, 0.00475663, 0.00373511, 0.00305689, 0.00412533,
           0.00369511, 0.00350622, 0.00527778, 0.00517378, 0.006184  ,
           0.007356  , 0.00692889, 0.00608711, 0.006672  , 0.00782978,
           0.00785022, 0.00742813, 0.008288  , 0.00705778, 0.00781556,
           0.00832267, 0.0092356 , 0.00893689, 0.00852956, 0.00883249,
           0.00826222, 0.00845867, 0.0089941 , 0.00796671, 0.00858533,
           0.01074311, 0.00940367, 0.00899733, 0.00968444, 0.00948813,
           0.00902011, 0.00905956, 0.01012343, 0.00921702, 0.00910013,
           0.00887124, 0.00901956, 0.00870711, 0.008928  , 0.007456  ,
           0.00879244, 0.01001244, 0.00854578, 0.00895522, 0.00929689,
           0.00963435, 0.00862978, 0.00860222, 0.00795065, 0.00888489,
           0.00884311, 0.01136578, 0.01015548, 0.01099867, 0.01038678,
           0.00980756, 0.00952329, 0.00969067, 0.00990889, 0.00953467,
           0.01173067, 0.00967867, 0.00997333, 0.00924178, 0.00915956,
           0.00931733, 0.01063378, 0.01073156, 0.01081168, 0.01066089,
           0.01105778, 0.00900311, 0.00849511, 0.0097779 , 0.01086844,
           0.00969556, 0.01063644, 0.01      , 0.00999067, 0.00967167,
           0.00939771, 0.01026946, 0.00940267, 0.009708  , 0.008548  ,
           0.00831511, 0.00830089, 0.00822089, 0.00512622, 0.005776  ,
           0.00633022, 0.00308279, 0.00225925, 0.003132  , 0.00297644,
           0.003896  , 0.00351733, 0.00331244, 0.00472889, 0.004048  ,
           0.00396178, 0.00297289, 0.002844  , 0.00229289, 0.00348267,
           0.00302933, 0.00215244, 0.00209511, 0.00161733, 0.00223422,
           0.00181822, 0.00177867, 0.00275644, 0.002176  , 0.00217733,
           0.00214089, 0.00131156, 0.00224267, 0.00215733, 0.00151778,
           0.00120133, 0.00133244, 0.0013276 , 0.00160337, 0.00085422,
           0.0010487 , 0.00107257, 0.00052978, 0.00091605, 0.00138889,
           0.00118933, 0.00232578, 0.00117822, 0.00185556, 0.00126311,
           0.00096711, 0.001148  , 0.00075111, 0.00090311, 0.00060356,
           0.00090697]), array([[    150,   50149],
           [  50150,  100149],
           [ 100150,  150149],
           [ 150150,  200149],
           [ 200150,  250149],
           [ 250150,  300149],
           [ 300150,  350149],
           [ 350150,  400149],
           [ 400150,  450149],
           [ 450150,  500149],
           [ 500150,  550149],
           [ 550150,  600149],
           [ 600150,  650149],
           [ 650150,  700149],
           [ 700150,  750149],
           [ 750150,  800149],
           [ 800150,  850149],
           [ 850150,  900149],
           [ 900150,  950149],
           [ 950150, 1000149],
           [1000150, 1050149],
           [1050150, 1100149],
           [1100150, 1150149],
           [1150150, 1200149],
           [1200150, 1250149],
           [1250150, 1300149],
           [1300150, 1350149],
           [1350150, 1400149],
           [1400150, 1450149],
           [1450150, 1500149],
           [1500150, 1550149],
           [1550150, 1600149],
           [1600150, 1650149],
           [1650150, 1700149],
           [1700150, 1750149],
           [1750150, 1800149],
           [1800150, 1850149],
           [1850150, 1900149],
           [1900150, 1950149],
           [1950150, 2000149],
           [2000150, 2050149],
           [2050150, 2100149],
           [2100150, 2150149],
           [2150150, 2200149],
           [2200150, 2250149],
           [2250150, 2300149],
           [2300150, 2350149],
           [2350150, 2400149],
           [2400150, 2450149],
           [2450150, 2500149],
           [2500150, 2550149],
           [2550150, 2600149],
           [2600150, 2650149],
           [2650150, 2700149],
           [2700150, 2750149],
           [2750150, 2800149],
           [2800150, 2850149],
           [2850150, 2900149],
           [2900150, 2950149],
           [2950150, 3000149],
           [3000150, 3050149],
           [3050150, 3100149],
           [3100150, 3150149],
           [3150150, 3200149],
           [3200150, 3250149],
           [3250150, 3300149],
           [3300150, 3350149],
           [3350150, 3400149],
           [3400150, 3450149],
           [3450150, 3500149],
           [3500150, 3550149],
           [3550150, 3600149],
           [3600150, 3650149],
           [3650150, 3700149],
           [3700150, 3750149],
           [3750150, 3800149],
           [3800150, 3850149],
           [3850150, 3900149],
           [3900150, 3950149],
           [3950150, 4000149],
           [4000150, 4050149],
           [4050150, 4100149],
           [4100150, 4150149],
           [4150150, 4200149],
           [4200150, 4250149],
           [4250150, 4300149],
           [4300150, 4350149],
           [4350150, 4400149],
           [4400150, 4450149],
           [4450150, 4500149],
           [4500150, 4550149],
           [4550150, 4600149],
           [4600150, 4650149],
           [4650150, 4700149],
           [4700150, 4750149],
           [4750150, 4800149],
           [4800150, 4850149],
           [4850150, 4900149],
           [4900150, 4950149],
           [4950150, 5000149],
           [5000150, 5050149],
           [5050150, 5100149],
           [5100150, 5150149],
           [5150150, 5200149],
           [5200150, 5250149],
           [5250150, 5300149],
           [5300150, 5350149],
           [5350150, 5400149],
           [5400150, 5450149],
           [5450150, 5500149],
           [5500150, 5550149],
           [5550150, 5600149],
           [5600150, 5650149],
           [5650150, 5700149],
           [5700150, 5750149],
           [5750150, 5800149],
           [5800150, 5850149],
           [5850150, 5900149],
           [5900150, 5950149],
           [5950150, 6000149],
           [6000150, 6050149],
           [6050150, 6100149],
           [6100150, 6150149],
           [6150150, 6200149],
           [6200150, 6250149],
           [6250150, 6300149],
           [6300150, 6350149],
           [6350150, 6400149],
           [6400150, 6450149],
           [6450150, 6500149],
           [6500150, 6550149],
           [6550150, 6600149],
           [6600150, 6650149],
           [6650150, 6700149],
           [6700150, 6750149],
           [6750150, 6800149],
           [6800150, 6850149],
           [6850150, 6900149],
           [6900150, 6950149],
           [6950150, 7000149],
           [7000150, 7050149],
           [7050150, 7100149],
           [7100150, 7150149],
           [7150150, 7200149],
           [7200150, 7250149],
           [7250150, 7271980]]), array([50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000, 50000,
           50000, 21831]), array([ 5004,  6746,  8929,  9796, 10096, 10075, 10802, 11485, 11853,
           13289, 13997, 13865, 13863, 14679, 15678, 15462, 14789, 14747,
           14134, 14921, 14988, 13983, 15369, 13228, 14389, 12853, 13327,
           14179, 13033, 14301, 14746, 14803, 14387, 14047, 13789, 13788,
           13989, 14665, 14480, 13831, 14260, 14074, 13973, 14903, 14516,
           15041, 15584, 14433, 14353, 14042, 14430, 14027, 12981, 13843,
           13971, 14338, 15575, 15407, 16507, 15542, 15758, 16078, 16034,
           16192, 15936, 16538, 15939, 15826, 15549, 16094, 15967, 15968,
           15603, 15559, 15427, 16714, 16590, 16219, 16536, 16566, 16371,
           16536, 15956, 15544, 15544, 15481, 16546, 14674, 14893, 14763,
           14858, 15278, 15903, 14776, 15228, 15650, 10982,  8880, 14494,
           14622, 15740, 13568, 11114, 15489, 14693, 15339, 14668, 14891,
           14673, 14803, 14577, 14431, 14681, 14499, 14385, 14752, 12052,
           13131, 11423,  9702, 10758,  7685, 11595,  9037,  9146,  7130,
            4957,  5718,  5297,  1714,  3261,  2069,  1962,  3517,  4777,
            3221,  6010,  4879,  5482,  6017,  5573,  5883,  5027,  4459,
            3924,  1485]))


##Heterozigosidade
A heterozigosidade é uma medida fundamental da diversidade genética dentro de indivíduos e populações. Ela representa a proporção de loci nos quais um indivíduo possui dois alelos diferentes (heterozigoto), refletindo o nível de variação genética disponível. Essa medida pode ser avaliada de duas formas principais: heterozigosidade observada (Hₒ), calculada diretamente a partir dos genótipos, e heterozigosidade esperada (Hₑ), estimada com base nas frequências alélicas sob o pressuposto de equilíbrio de Hardy-Weinberg.

Existem diferentes formas de analisar a heterozigosidade, dependendo da escala dos dados:

*   Em marcadores específicos (como microssatélites ou SNPs), pode-se calcular Hₒ e Hₑ locus a locus e por população;
*   Com dados genômicos, é comum estimar a heterozigosidade individual genômica, por exemplo, usando a proporção de heterozigotos em um arquivo VCF, ou a densidade de variantes por janela genômica;
*   Em contextos de conservação, pode-se também calcular métricas como F_ROH, baseadas em regiões contínuas de homozigose (ROH), para avaliar níveis de endogamia.

A heterozigosidade tem ampla aplicação em genética de populações e conservação, sendo usada para:
*   Avaliar a saúde genética de populações ameaçadas;
*   Monitorar efeitos de gargalos populacionais e endogamia;
*   Comparar a diversidade genética entre populações ou espécies;
*   Informar estratégias de manejo genético e decisões de translocação ou reprodução assistida.

Em suma, a heterozigosidade é um indicador-chave da capacidade adaptativa de populações naturais, sendo essencial para a manutenção da viabilidade evolutiva em longo prazo.

###Cálculo de heterozigosidade com o Plink


```
!conda run -n genomics_env plink
```

    PLINK v1.9.0-b.8 64-bit (22 Oct 2024)              cog-genomics.org/plink/1.9/
    (C) 2005-2024 Shaun Purcell, Christopher Chang   GNU General Public License v3
    
      plink <input flag(s)...> [command flag(s)...] [other flag(s)...]
      plink --help [flag name(s)...]
    
    Commands include --make-bed, --recode, --flip-scan, --merge-list,
    --write-snplist, --list-duplicate-vars, --freqx, --missing, --test-mishap,
    --hardy, --mendel, --ibc, --impute-sex, --indep-pairphase, --r2, --show-tags,
    --blocks, --distance, --genome, --homozyg, --make-rel, --make-grm-gz,
    --rel-cutoff, --cluster, --pca, --neighbour, --ibs-test, --regress-distance,
    --model, --bd, --gxe, --logistic, --dosage, --lasso, --test-missing,
    --make-perm-pheno, --unrelated-heritability, --tdt, --dfam, --qfam, --tucc,
    --annotate, --clump, --gene-report, --meta-analysis, --epistasis,
    --fast-epistasis, and --score.
    
    "plink --help | more" describes all functions (warning: long).
    
    ERROR conda.cli.main_run:execute(125): `conda run plink` failed. (See above for error)



```
!conda run -n genomics_env plink --bfile willisornis --het --out het_result --allow-extra-chr
```

    PLINK v1.90b6.24 64-bit (6 Jun 2021)           www.cog-genomics.org/plink/1.9/
    (C) 2005-2021 Shaun Purcell, Christopher Chang   GNU General Public License v3
    Logging to het_result.log.
    Options in effect:
      --allow-extra-chr
      --bfile willisornis
      --het
      --out het_result
    
    12977 MB RAM detected; reserving 6488 MB for main workspace.
    1855451 variants loaded from .bim file.
    20 people (0 males, 0 females, 20 ambiguous) loaded from .fam.
    Ambiguous sex IDs written to het_result.nosex .
    Using 1 thread (no multithreaded calculations invoked).
    Before main variant filters, 20 founders and 0 nonfounders present.
    Calculating allele frequencies... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99% done.
    Total genotyping rate is 0.999966.
    1855451 variants and 20 people pass filters and QC.
    Note: No phenotypes present.
    --het: 610491 variants scanned, report written to het_result.het .



```
import pandas as pd

# Load PLINK heterozygosity file
het = pd.read_csv("het_result.het", delim_whitespace=True)

# Add heterozygosity column
het['HET'] = (het['N(NM)'] - het['O(HOM)']) / het['N(NM)']

# Display top rows
het[['FID', 'IID', 'HET', 'F']].head()
```

    <i-input-65-a05fe853e0e6>:4: FutureWarning: The 'delim_whitespace' keyword in pd.read_csv is deprecated and will be removed in a future version. Use ``sep='\s+'`` instead
      het = pd.read_csv("het_result.het", delim_whitespace=True)






  <div id="df-1bc2b7ee-948f-4fbd-96ca-5614a1a88e33" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FID</th>
      <th>IID</th>
      <th>HET</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ITV66049</td>
      <td>ITV66049</td>
      <td>0.073923</td>
      <td>0.4550</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ITV66050</td>
      <td>ITV66050</td>
      <td>0.089244</td>
      <td>0.3420</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ITV66051</td>
      <td>ITV66051</td>
      <td>0.094933</td>
      <td>0.3001</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ITV66052</td>
      <td>ITV66052</td>
      <td>0.093199</td>
      <td>0.3128</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ITV66053</td>
      <td>ITV66053</td>
      <td>0.081469</td>
      <td>0.3993</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-1bc2b7ee-948f-4fbd-96ca-5614a1a88e33')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-1bc2b7ee-948f-4fbd-96ca-5614a1a88e33 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-1bc2b7ee-948f-4fbd-96ca-5614a1a88e33');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-a9641e84-31fd-40c7-af42-faa3e1b44755">
      <button class="colab-df-quickchart" onclick="quickchart('df-a9641e84-31fd-40c7-af42-faa3e1b44755')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-a9641e84-31fd-40c7-af42-faa3e1b44755 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 5))
plt.bar(het['IID'], het['HET'], color='skyblue')
plt.xticks(rotation=90)
plt.xlabel('Individual')
plt.ylabel('Heterozygosity')
plt.title('Observed Heterozygosity per Individual')
plt.tight_layout()
plt.show()
```


    
![png](Estat%C3%ADsticas_files/Estat%C3%ADsticas_28_0.png)
    



```
import pandas as pd

# Load PLINK .het file
het = pd.read_csv("het_result.het", delim_whitespace=True)

# Calculate genome-wide observed heterozygosity
het['HET'] = (het['N(NM)'] - het['O(HOM)']) / het['N(NM)']

# Save results
het[['FID', 'IID', 'HET']].to_csv("genomewide_heterozygosity.csv", index=False)

# Show a preview
het[['FID', 'IID', 'HET']].head()
```

    <i-input-67-d02425187ab8>:4: FutureWarning: The 'delim_whitespace' keyword in pd.read_csv is deprecated and will be removed in a future version. Use ``sep='\s+'`` instead
      het = pd.read_csv("het_result.het", delim_whitespace=True)






  <div id="df-50208b55-c6eb-4847-b761-6f99fdb0c250" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FID</th>
      <th>IID</th>
      <th>HET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ITV66049</td>
      <td>ITV66049</td>
      <td>0.073923</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ITV66050</td>
      <td>ITV66050</td>
      <td>0.089244</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ITV66051</td>
      <td>ITV66051</td>
      <td>0.094933</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ITV66052</td>
      <td>ITV66052</td>
      <td>0.093199</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ITV66053</td>
      <td>ITV66053</td>
      <td>0.081469</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-50208b55-c6eb-4847-b761-6f99fdb0c250')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-50208b55-c6eb-4847-b761-6f99fdb0c250 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-50208b55-c6eb-4847-b761-6f99fdb0c250');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-ca058e18-0a84-4a30-96c6-58283a18db50">
      <button class="colab-df-quickchart" onclick="quickchart('df-ca058e18-0a84-4a30-96c6-58283a18db50')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-ca058e18-0a84-4a30-96c6-58283a18db50 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```
genome_length = 1_200_000_000  # Replace with your actual length

# Number of heterozygous sites
het['N_HET'] = het['N(NM)'] - het['O(HOM)']

# Heterozygosity per base pair
het['HET_per_bp'] = het['N_HET'] / genome_length
```


```
# Save full result
het[['FID', 'IID', 'N_HET', 'HET_per_bp']].to_csv("genomewide_het_per_bp.csv", index=False)

# View
het[['FID', 'IID', 'N_HET', 'HET_per_bp']].head()
```





  <div id="df-2f8202b3-2000-4f05-823b-56961af7a3b3" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FID</th>
      <th>IID</th>
      <th>N_HET</th>
      <th>HET_per_bp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ITV66049</td>
      <td>ITV66049</td>
      <td>45126</td>
      <td>0.000038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ITV66050</td>
      <td>ITV66050</td>
      <td>54478</td>
      <td>0.000045</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ITV66051</td>
      <td>ITV66051</td>
      <td>57951</td>
      <td>0.000048</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ITV66052</td>
      <td>ITV66052</td>
      <td>56891</td>
      <td>0.000047</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ITV66053</td>
      <td>ITV66053</td>
      <td>49731</td>
      <td>0.000041</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-2f8202b3-2000-4f05-823b-56961af7a3b3')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-2f8202b3-2000-4f05-823b-56961af7a3b3 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-2f8202b3-2000-4f05-823b-56961af7a3b3');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-6b72fb16-7134-47b2-85bf-32a4036aac73">
      <button class="colab-df-quickchart" onclick="quickchart('df-6b72fb16-7134-47b2-85bf-32a4036aac73')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-6b72fb16-7134-47b2-85bf-32a4036aac73 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```
import pandas as pd
import matplotlib.pyplot as plt

# === Load PLINK .het output ===
het = pd.read_csv("het_result.het", delim_whitespace=True)

# === Define total genome length (in base pairs) ===
genome_length = 1_200_000_000  # replace with your actual value if different

# === Calculate heterozygous sites and heterozygosity per bp ===
het['N_HET'] = het['N(NM)'] - het['O(HOM)']
het['HET_per_bp'] = het['N_HET'] / genome_length

# === Plot heterozygosity per bp ===
plt.figure(figsize=(12, 5))
plt.bar(het['IID'], het['HET_per_bp'], color='cornflowerblue', edgecolor='black')
plt.xticks(rotation=90, fontsize=8)
plt.ylabel('Heterozygosity per bp')
plt.xlabel('Individual')
plt.title('Genome-wide Heterozygosity per Individual')
plt.tight_layout()
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()
```

    <i-input-71-82caa0ec4e2a>:5: FutureWarning: The 'delim_whitespace' keyword in pd.read_csv is deprecated and will be removed in a future version. Use ``sep='\s+'`` instead
      het = pd.read_csv("het_result.het", delim_whitespace=True)



    
![png](Estat%C3%ADsticas_files/Estat%C3%ADsticas_32_1.png)
    


---

##Frequência alélica
A frequência alélica é a proporção de um determinado alelo em relação ao total de alelos observados em uma população para um locus específico. Ela é uma medida central na genética de populações, pois descreve como os alelos estão distribuídos entre os indivíduos e fornece a base para diversas análises evolutivas e demográficas. Por exemplo, em um locus bialélico com alelos A e a, se 70% dos alelos observados forem A e 30% forem a, as frequências alélicas são 0,7 e 0,3, respectivamente.

Essa medida é utilizada para:
* Estimar a diversidade genética de uma população;
* Detectar mudanças evolutivas ao longo do tempo, como deriva genética, seleção natural ou migração;
* Calcular outras estatísticas como heterozigosidade, FST e estrutura populacional;
* Avaliar o impacto de eventos históricos, como gargalos populacionais.

A frequência alélica pode ser calculada a partir de genótipos individuais ou diretamente de contagens de alelos em dados genômicos (ex: VCF), sendo uma métrica simples, mas poderosa, para entender a dinâmica genética de populações.



```
!conda run -n genomics_env vcftools --version
```

    VCFtools (0.1.17)
    



```
import pandas as pd
import os

# === 1. Defina o caminho para o VCF ===
vcf_file = "willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz"
output_prefix = "allele_freq"

# === 2. Rode o vcftools ===
!conda run -n genomics_env vcftools --gzvcf {vcf_file} --freq --out {output_prefix} --maf 0.05

# === 3. Leia o arquivo .frq gerado ===
frq_file = output_prefix + ".frq"

# Confere se o arquivo foi criado
if not os.path.exists(frq_file):
    raise FileNotFoundError(f"{frq_file} não foi encontrado. Verifique o VCF e o vcftools.")

# === 4. Carrega o arquivo e calcula a MAF média ===
df = pd.read_csv(frq_file, delim_whitespace=True)

def get_minor_freq(row):
    # extrai apenas os campos de frequência (colunas a partir da 5ª em diante)
    freqs = []
    for col in row.index[4:]:
        try:
            freq = float(row[col].split(":")[1])
            freqs.append(freq)
        except:
            continue
    return min(freqs) if freqs else None

# Aplica a função
df['MAF'] = df.apply(get_minor_freq, axis=1)

# Calcula a média global da MAF
global_maf = df['MAF'].mean()

print(f"Média global da frequência do alelo menos frequente (MAF): {global_maf:.4f}")
```

    
    VCFtools - 0.1.17
    (C) Adam Auton and Anthony Marcketta 2009
    
    Parameters as interpreted:
    	--gzvcf willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz
    	--maf 0.05
    	--freq
    	--out allele_freq
    
    Using zlib version: 1.3.1
    Warning: Expected at least 2 parts in FORMAT entry: ID=PGT,Number=1,Type=String,Description="Physical phasing haplotype information, describing how the alternate alleles are phased in relation to one another; will always be heterozygous and is not intended to describe called alleles">
    Warning: Expected at least 2 parts in FORMAT entry: ID=PID,Number=1,Type=String,Description="Physical phasing ID information, where each unique ID within a given sample (but not across samples) connects records within a phasing group">
    Warning: Expected at least 2 parts in FORMAT entry: ID=PL,Number=G,Type=Integer,Description="Normalized, Phred-scaled likelihoods for genotypes as defined in the VCF specification">
    Warning: Expected at least 2 parts in FORMAT entry: ID=RGQ,Number=1,Type=Integer,Description="Unconditional reference genotype confidence, encoded as a phred quality -10*log10 p(genotype call is wrong)">
    Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes, for each ALT allele, in the same order as listed">
    Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes, for each ALT allele, in the same order as listed">
    Warning: Expected at least 2 parts in INFO entry: ID=AF,Number=A,Type=Float,Description="Allele Frequency, for each ALT allele, in the same order as listed">
    Warning: Expected at least 2 parts in INFO entry: ID=AF,Number=A,Type=Float,Description="Allele Frequency, for each ALT allele, in the same order as listed">
    Warning: Expected at least 2 parts in INFO entry: ID=MLEAC,Number=A,Type=Integer,Description="Maximum likelihood expectation (MLE) for the allele counts (not necessarily the same as the AC), for each ALT allele, in the same order as listed">
    Warning: Expected at least 2 parts in INFO entry: ID=MLEAC,Number=A,Type=Integer,Description="Maximum likelihood expectation (MLE) for the allele counts (not necessarily the same as the AC), for each ALT allele, in the same order as listed">
    Warning: Expected at least 2 parts in INFO entry: ID=MLEAF,Number=A,Type=Float,Description="Maximum likelihood expectation (MLE) for the allele frequency (not necessarily the same as the AF), for each ALT allele, in the same order as listed">
    Warning: Expected at least 2 parts in INFO entry: ID=MLEAF,Number=A,Type=Float,Description="Maximum likelihood expectation (MLE) for the allele frequency (not necessarily the same as the AF), for each ALT allele, in the same order as listed">
    After filtering, kept 20 out of 20 Individuals
    Outputting Frequency Statistics...
    After filtering, kept 298664 out of a possible 1855451 Sites
    Run Time = 25.00 seconds
    


    <i-input-25-a4c5fbd20c9b>:19: FutureWarning: The 'delim_whitespace' keyword in pd.read_csv is deprecated and will be removed in a future version. Use ``sep='\s+'`` instead
      df = pd.read_csv(frq_file, delim_whitespace=True)


    Média global da frequência do alelo menos frequente (MAF): 0.2180


___

##Estatísticas de diferenciação populacional
As estatísticas que comparam duas ou mais populações têm como objetivo quantificar a diferenciação genética entre grupos, ajudando a entender padrões de estrutura populacional, fluxo gênico e processos evolutivos como isolamento, migração e especiação. Essas métricas são fundamentais em estudos de genética de populações, biogeografia e genômica da conservação.

As principais estatísticas incluem:
* FST: mede a proporção da variação genética total que é devida à diferenciação entre populações. Valores próximos de 0 indicam populações geneticamente semelhantes; valores próximos de 1 indicam diferenciação acentuada.
* DXY: calcula a divergência absoluta entre populações, representando o número médio de diferenças por sítio entre indivíduos de diferentes grupos.
* DA (divergência líquida): estima a diferença entre populações descontando a variação dentro de cada uma, sendo útil para inferir tempo de divergência.
* Shared vs. Private Alleles: avalia a proporção de alelos compartilhados entre populações e aqueles exclusivos de cada grupo.

Essas estatísticas são particularmente úteis para identificar barreiras ao fluxo gênico, delimitar unidades evolutivamente significativas (ESUs), investigar a origem de populações e informar estratégias de manejo genético para espécies ameaçadas.


```
# Get all unique population names
populations = metadata['population'].unique()

# Prepare results
between_pop_results = []
```


```
for i in range(len(populations)):
    for j in range(i + 1, len(populations)):
        pop1 = populations[i]
        pop2 = populations[j]

        # Get sample indices
        pop1_samples = metadata.query("population == @pop1")['sample'].tolist()
        pop2_samples = metadata.query("population == @pop2")['sample'].tolist()

        pop1_idx = [np.where(samples == s)[0][0] for s in pop1_samples if s in samples]
        pop2_idx = [np.where(samples == s)[0][0] for s in pop2_samples if s in samples]

        if len(pop1_idx) < 2 or len(pop2_idx) < 2:
            print(f"Skipping {pop1} vs {pop2}: not enough samples.")
            continue

        # Subset genotype data
        gt1 = gt[:, pop1_idx]
        gt2 = gt[:, pop2_idx]

        ac1 = gt1.count_alleles()
        ac2 = gt2.count_alleles()

        # Sort by position
        sort_idx = np.argsort(positions)
        pos_sorted = positions[sort_idx]
        ac1_sorted = ac1[sort_idx]
        ac2_sorted = ac2[sort_idx]

        # Compute Fst (Hudson)
        num, den = allel.hudson_fst(ac1_sorted, ac2_sorted)
        fst_hudson = np.sum(num) / np.sum(den)

        # Compute DXY
        p1 = ac1_sorted.to_frequencies()
        p2 = ac2_sorted.to_frequencies()
        dxy = (p1[:, 0] * p2[:, 1]) + (p1[:, 1] * p2[:, 0])
        dxy = dxy[~np.isnan(dxy)]
        mean_dxy = np.mean(dxy)

        # Get π values for each pop
        pi1 = allel.sequence_diversity(pos_sorted, ac1_sorted)
        pi2 = allel.sequence_diversity(pos_sorted, ac2_sorted)
        da = mean_dxy - (pi1 + pi2) / 2

        # Save results
        between_pop_results.append({
            'pop1': pop1,
            'pop2': pop2,
            'Fst_Hudson': fst_hudson,
            'Dxy': mean_dxy,
            'Da': da
        })
```


```
df_between = pd.DataFrame(between_pop_results)
df_between = df_between.sort_values(["pop1", "pop2"])
df_between
```





  <div id="df-1dcca499-55a6-4423-9978-9ad9f52fc70c" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pop1</th>
      <th>pop2</th>
      <th>Fst_Hudson</th>
      <th>Dxy</th>
      <th>Da</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Willisornis_poecilinotus_griseiventris_E</td>
      <td>Willisornis_poecilinotus_griseiventris_W</td>
      <td>0.127853</td>
      <td>0.040093</td>
      <td>0.031171</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Willisornis_poecilinotus_griseiventris_E</td>
      <td>Willisornis_poecilinotus_lepidonota</td>
      <td>NaN</td>
      <td>0.049851</td>
      <td>0.040460</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Willisornis_poecilinotus_griseiventris_E</td>
      <td>Willisornis_vidua_nigrigula</td>
      <td>0.450267</td>
      <td>0.051407</td>
      <td>0.044196</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Willisornis_poecilinotus_griseiventris_W</td>
      <td>Willisornis_poecilinotus_lepidonota</td>
      <td>NaN</td>
      <td>0.049450</td>
      <td>0.039225</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Willisornis_poecilinotus_griseiventris_W</td>
      <td>Willisornis_vidua_nigrigula</td>
      <td>0.390142</td>
      <td>0.051695</td>
      <td>0.043650</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Willisornis_poecilinotus_lepidonota</td>
      <td>Willisornis_vidua_nigrigula</td>
      <td>NaN</td>
      <td>0.053047</td>
      <td>0.044533</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-1dcca499-55a6-4423-9978-9ad9f52fc70c')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-1dcca499-55a6-4423-9978-9ad9f52fc70c button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-1dcca499-55a6-4423-9978-9ad9f52fc70c');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-627c3dd0-bb72-4d88-bccb-8fd029eb5285">
      <button class="colab-df-quickchart" onclick="quickchart('df-627c3dd0-bb72-4d88-bccb-8fd029eb5285')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-627c3dd0-bb72-4d88-bccb-8fd029eb5285 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

  <div id="id_ac844d64-0c59-44d9-b3ad-8d27419da99a">
    <style>
      .colab-df-generate {
        background-color: #E8F0FE;
        border: none;
        border-radius: 50%;
        cursor: pointer;
        display: none;
        fill: #1967D2;
        height: 32px;
        padding: 0 0 0 0;
        width: 32px;
      }

      .colab-df-generate:hover {
        background-color: #E2EBFA;
        box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
        fill: #174EA6;
      }

      [theme=dark] .colab-df-generate {
        background-color: #3B4455;
        fill: #D2E3FC;
      }

      [theme=dark] .colab-df-generate:hover {
        background-color: #434B5C;
        box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
        filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
        fill: #FFFFFF;
      }
    </style>
    <button class="colab-df-generate" onclick="generateWithVariable('df_between')"
            title="Generate code using this dataframe."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M7,19H8.4L18.45,9,17,7.55,7,17.6ZM5,21V16.75L18.45,3.32a2,2,0,0,1,2.83,0l1.4,1.43a1.91,1.91,0,0,1,.58,1.4,1.91,1.91,0,0,1-.58,1.4L9.25,21ZM18.45,9,17,7.55Zm-12,3A5.31,5.31,0,0,0,4.9,8.1,5.31,5.31,0,0,0,1,6.5,5.31,5.31,0,0,0,4.9,4.9,5.31,5.31,0,0,0,6.5,1,5.31,5.31,0,0,0,8.1,4.9,5.31,5.31,0,0,0,12,6.5,5.46,5.46,0,0,0,6.5,12Z"/>
  </svg>
    </button>
    <script>
      (() => {
      const buttonEl =
        document.querySelector('#id_ac844d64-0c59-44d9-b3ad-8d27419da99a button.colab-df-generate');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      buttonEl.onclick = () => {
        google.colab.notebook.generateWithVariable('df_between');
      }
      })();
    </script>
  </div>

    </div>
  </div>





```
# Save to CSV
df_between.to_csv("between_population_stats.csv", index=False)
```

##DxY
DXY é uma medida de divergência genética absoluta entre duas populações, que representa o número médio de diferenças nucleotídicas por sítio entre indivíduos de grupos distintos. Ao contrário de medidas relativas como o FST, o DXY quantifica a diversidade entre populações, independentemente da variação dentro de cada uma.

Essa estatística é especialmente útil em estudos de:
* Especiação, para identificar regiões do genoma com alta divergência;
* Barreiras ao fluxo gênico, indicando trechos sob isolamento reprodutivo;
* Genômica da conservação, para comparar populações geograficamente isoladas e definir unidades de manejo.

O DXY é geralmente calculado com base em dados de SNPs ou genomas inteiros, usando janelas deslizantes ao longo do genoma para identificar padrões de divergência. Valores mais altos indicam maior acúmulo de diferenças, o que pode refletir isolamento de longo prazo, seleção divergente ou baixa migração entre as populações comparadas.


```
pip install scikit-allel
```

    Collecting scikit-allel
      Downloading scikit_allel-1.3.13-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (3.1 kB)
    Collecting numpy (from scikit-allel)
      Downloading numpy-2.2.6-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (62 kB)
    Collecting dask[array] (from scikit-allel)
      Downloading dask-2025.5.1-py3-none-any.whl.metadata (3.8 kB)
    Collecting click>=8.1 (from dask[array]->scikit-allel)
      Downloading click-8.2.1-py3-none-any.whl.metadata (2.5 kB)
    Collecting cloudpickle>=3.0.0 (from dask[array]->scikit-allel)
      Downloading cloudpickle-3.1.1-py3-none-any.whl.metadata (7.1 kB)
    Collecting fsspec>=2021.09.0 (from dask[array]->scikit-allel)
      Downloading fsspec-2025.5.1-py3-none-any.whl.metadata (11 kB)
    Requirement already satisfied: packaging>=20.0 in /usr/local/lib/3.13/site-packages (from dask[array]->scikit-allel) (24.2)
    Collecting partd>=1.4.0 (from dask[array]->scikit-allel)
      Downloading partd-1.4.2-py3-none-any.whl.metadata (4.6 kB)
    Collecting pyyaml>=5.3.1 (from dask[array]->scikit-allel)
      Downloading PyYAML-6.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (2.1 kB)
    Collecting toolz>=0.10.0 (from dask[array]->scikit-allel)
      Downloading toolz-1.0.0-py3-none-any.whl.metadata (5.1 kB)
    Collecting locket (from partd>=1.4.0->dask[array]->scikit-allel)
      Downloading locket-1.0.0-py2.py3-none-any.whl.metadata (2.8 kB)
    Downloading scikit_allel-1.3.13-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (8.4 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m8.4/8.4 MB[0m [31m29.7 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading numpy-2.2.6-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (16.5 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m16.5/16.5 MB[0m [31m50.8 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading click-8.2.1-py3-none-any.whl (102 kB)
    Downloading cloudpickle-3.1.1-py3-none-any.whl (20 kB)
    Downloading fsspec-2025.5.1-py3-none-any.whl (199 kB)
    Downloading partd-1.4.2-py3-none-any.whl (18 kB)
    Downloading PyYAML-6.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (759 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m759.5/759.5 kB[0m [31m20.6 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading toolz-1.0.0-py3-none-any.whl (56 kB)
    Downloading dask-2025.5.1-py3-none-any.whl (1.5 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m1.5/1.5 MB[0m [31m48.7 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading locket-1.0.0-py2.py3-none-any.whl (4.4 kB)
    Installing collected packages: toolz, pyyaml, numpy, locket, fsspec, cloudpickle, click, partd, dask, scikit-allel
    Successfully installed click-8.2.1 cloudpickle-3.1.1 dask-2025.5.1 fsspec-2025.5.1 locket-1.0.0 numpy-2.2.6 partd-1.4.2 pyyaml-6.0.2 scikit-allel-1.3.13 toolz-1.0.0





```
!conda run -n genomics_env bcftools query -l willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz
```

    ITV66049
    ITV66050
    ITV66051
    ITV66052
    ITV66053
    ITV66038
    ITV66039
    ITV66041
    ITV66042
    ITV66043
    ITV66028
    ITV66066
    ITV66081
    ITV66083
    ITV66084
    ITV66071
    ITV66072
    ITV66073
    ITV66075
    ITV66076
    



```
%cat ../willisornis_tutorial_metadata.tsv
```

    ITV66049	Willisornis_poecilinotus_griseiventris_E
    ITV66050	Willisornis_poecilinotus_griseiventris_E
    ITV66051	Willisornis_poecilinotus_griseiventris_E
    ITV66052	Willisornis_poecilinotus_griseiventris_E
    ITV66053	Willisornis_poecilinotus_griseiventris_E
    ITV66038	Willisornis_poecilinotus_griseiventris_W
    ITV66039	Willisornis_poecilinotus_griseiventris_W
    ITV66041	Willisornis_poecilinotus_griseiventris_W
    ITV66042	Willisornis_poecilinotus_griseiventris_W
    ITV66043	Willisornis_poecilinotus_griseiventris_W
    ITV66028	Willisornis_poecilinotus_lepidonota
    ITV66066	Willisornis_poecilinotus_lepidonota
    ITV66081	Willisornis_poecilinotus_lepidonota
    ITV66083	Willisornis_poecilinotus_lepidonota
    ITV66084	Willisornis_poecilinotus_lepidonota
    ITV66071	Willisornis_vidua_nigrigula
    ITV66072	Willisornis_vidua_nigrigula
    ITV66073	Willisornis_vidua_nigrigula
    ITV66075	Willisornis_vidua_nigrigula
    ITV66076	Willisornis_vidua_nigrigula


```
# -------------------------
# 1. Install Required Package (if needed)
# -------------------------
#!pip install -q scikit-allel

# -------------------------
# 2. Import Libraries
# -------------------------
import allel
import numpy as np
import pandas as pd

# -------------------------
# 3. Define Parameters
# -------------------------

# Path to VCF (must be uploaded or from GDrive)
vcf_path = 'willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz'

# Define your populations using sample names (must match those in the VCF header)
pop1_samples = ['ITV66049', 'ITV66050', 'ITV66051', 'ITV66052', 'ITV66053']
pop2_samples = ['ITV66038', 'ITV66039', 'ITV66041', 'ITV66042', 'ITV66043']

# -------------------------
# 4. Load VCF and Prepare Genotype Data
# -------------------------
print("Loading VCF...")
callset = allel.read_vcf(vcf_path, fields=['samples', 'calldata/GT'], alt_number=1)

samples = callset['samples']
gt = allel.GenotypeArray(callset['calldata/GT'])

# -------------------------
# 5. Match Sample Indices
# -------------------------
pop1_idx = [np.where(samples == s)[0][0] for s in pop1_samples]
pop2_idx = [np.where(samples == s)[0][0] for s in pop2_samples]

# -------------------------
# 6. Calculate DXY
# -------------------------
ac1 = gt[:, pop1_idx].count_alleles()
ac2 = gt[:, pop2_idx].count_alleles()

# Allele frequencies
p1 = ac1.to_frequencies()
p2 = ac2.to_frequencies()

# DXY per variant
dxy_per_variant = (p1[:, 0] * p2[:, 1]) + (p1[:, 1] * p2[:, 0])
dxy_per_variant = dxy_per_variant[~np.isnan(dxy_per_variant)]

# Mean DXY
mean_dxy = np.mean(dxy_per_variant)

print(f"Genome-wide DXY: {mean_dxy:.6f}")
```

    Loading VCF...
    Genome-wide DXY: 0.040093



```
# -------------------------
# 1. Install Required Package
# -------------------------
#!pip install -q scikit-allel

# -------------------------
# 2. Import Libraries
# -------------------------
import allel
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# -------------------------
# 3. Load Metadata and Define Groups
# -------------------------
from io import StringIO

# Paste your metadata as a multiline string (tab-separated)
metadata_str = """
ITV66049	Willisornis_poecilinotus_griseiventris_E
ITV66050	Willisornis_poecilinotus_griseiventris_E
ITV66051	Willisornis_poecilinotus_griseiventris_E
ITV66052	Willisornis_poecilinotus_griseiventris_E
ITV66053	Willisornis_poecilinotus_griseiventris_E
ITV66038	Willisornis_poecilinotus_griseiventris_W
ITV66039	Willisornis_poecilinotus_griseiventris_W
ITV66041	Willisornis_poecilinotus_griseiventris_W
ITV66042	Willisornis_poecilinotus_griseiventris_W
ITV66043	Willisornis_poecilinotus_griseiventris_W
ITV66028	Willisornis_poecilinotus_lepidonota
ITV66066	Willisornis_poecilinotus_lepidonota
ITV66081	Willisornis_poecilinotus_lepidonota
ITV66083	Willisornis_poecilinotus_lepidonota
ITV66084	Willisornis_poecilinotus_lepidonota
ITV66071	Willisornis_vidua_nigrigula
ITV66072	Willisornis_vidua_nigrigula
ITV66073	Willisornis_vidua_nigrigula
ITV66075	Willisornis_vidua_nigrigula
ITV66076	Willisornis_vidua_nigrigula
"""

# Read metadata into a DataFrame
metadata = pd.read_csv(StringIO(metadata_str), sep="\t", names=["sample", "group"])

# Show available groups
print("Available groups:")
print(metadata['group'].value_counts())

# Define which groups to compare
group1_name = "Willisornis_poecilinotus_griseiventris_E"
group2_name = "Willisornis_poecilinotus_griseiventris_W"

# Extract sample names for each group
pop1_samples = metadata.query("group == @group1_name")['sample'].tolist()
pop2_samples = metadata.query("group == @group2_name")['sample'].tolist()

print(f"\nGroup 1: {group1_name} → {len(pop1_samples)} samples")
print(f"Group 2: {group2_name} → {len(pop2_samples)} samples")
```

    Available groups:
    group
    Willisornis_poecilinotus_griseiventris_E    5
    Willisornis_poecilinotus_griseiventris_W    5
    Willisornis_poecilinotus_lepidonota         5
    Willisornis_vidua_nigrigula                 5
    Name: count, dtype: int64
    
    Group 1: Willisornis_poecilinotus_griseiventris_E → 5 samples
    Group 2: Willisornis_poecilinotus_griseiventris_W → 5 samples



```
# -------------------------
# 4. Load VCF
# -------------------------
vcf_path = 'willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz'  # Replace with the actual path or mount from Google Drive

print("Loading VCF...")

callset = allel.read_vcf(vcf_path, fields=['samples', 'calldata/GT', 'variants/POS'], alt_number=1)

samples = callset['samples']
gt = allel.GenotypeArray(callset['calldata/GT'])

# -------------------------
# 5. Match sample indices
# -------------------------
pop1_idx = [np.where(samples == s)[0][0] for s in pop1_samples]
pop2_idx = [np.where(samples == s)[0][0] for s in pop2_samples]

# -------------------------
# 6. Calculate DXY
# -------------------------
ac1 = gt[:, pop1_idx].count_alleles()
ac2 = gt[:, pop2_idx].count_alleles()

p1 = ac1.to_frequencies()
p2 = ac2.to_frequencies()

dxy_per_variant = (p1[:, 0] * p2[:, 1]) + (p1[:, 1] * p2[:, 0])
dxy_per_variant = dxy_per_variant[~np.isnan(dxy_per_variant)]
mean_dxy = np.mean(dxy_per_variant)

print(f"\nGenome-wide DXY between '{group1_name}' and '{group2_name}': {mean_dxy:.6f}")
```

    Loading VCF...
    
    Genome-wide DXY between 'Willisornis_poecilinotus_griseiventris_E' and 'Willisornis_poecilinotus_griseiventris_W': 0.040093



```
plt.figure(figsize=(10, 4))
plt.plot(dxy_per_variant, alpha=0.6, lw=0.8)
plt.title(f"DXY per Variant: {group1_name} vs {group2_name}")
plt.ylabel("DXY")
plt.xlabel("Variant Index")
plt.grid(True)
plt.tight_layout()
plt.show()
```


    
![png](Estat%C3%ADsticas_files/Estat%C3%ADsticas_50_0.png)
    



```
# Define window size and step in base pairs
window_size = 50000
step_size = 50000
positions = callset['variants/POS']

## -------------------------
# Define non-overlapping sliding windows
# -------------------------


# Get min and max positions
pos_min = positions.min()
pos_max = positions.max()

# Build window edges
windows = np.arange(pos_min, pos_max, step_size)
window_starts = windows
window_ends = windows + window_size
window_midpoints = windows + window_size // 2

# Compute DXY per window
dxy_windowed = []
for start, end in zip(window_starts, window_ends):
    mask = (positions >= start) & (positions < end)
    if np.sum(mask) > 0:
        dxy_windowed.append(np.mean(dxy_per_variant[mask]))
    else:
        dxy_windowed.append(np.nan)

dxy_windowed = np.array(dxy_windowed)

```


```
plt.figure(figsize=(10, 4))
plt.plot(window_midpoints, dxy_windowed, marker='o', linestyle='-', alpha=0.7)
plt.title(f"Sliding Window DXY: {group1_name} vs {group2_name}")
plt.xlabel("Genomic Position (bp)")
plt.ylabel("DXY (mean per 50kb)")
plt.grid(True)
plt.tight_layout()
plt.show()
```


    
![png](Estat%C3%ADsticas_files/Estat%C3%ADsticas_52_0.png)
    


---
