---
layout: single
title: "Endocruzamento"
permalink: /pages/endocruzamento/
---

```
from google.colab import drive
drive.mount('/content/drive')
```

    Mounted at /content/drive



```
!ls /content/drive/MyDrive/Congen_ITV/
```

    raw_data  reference  vcf  willisornis_tutorial_metadata.tsv



```
import pandas as pd

file_path = '/content/drive/MyDrive/Congen_ITV/willisornis_tutorial_metadata.tsv'
df = pd.read_csv(file_path, sep='\t')
df.head()
```





  <div id="df-6245e68b-14a4-444e-bdab-005ee16d5b3f" class="colab-df-container">
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
      <th>ITV66049</th>
      <th>Willisornis_poecilinotus_griseiventris_E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ITV66050</td>
      <td>Willisornis_poecilinotus_griseiventris_E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ITV66051</td>
      <td>Willisornis_poecilinotus_griseiventris_E</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ITV66052</td>
      <td>Willisornis_poecilinotus_griseiventris_E</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ITV66053</td>
      <td>Willisornis_poecilinotus_griseiventris_E</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ITV66038</td>
      <td>Willisornis_poecilinotus_griseiventris_W</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-6245e68b-14a4-444e-bdab-005ee16d5b3f')"
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
        document.querySelector('#df-6245e68b-14a4-444e-bdab-005ee16d5b3f button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-6245e68b-14a4-444e-bdab-005ee16d5b3f');
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


    <div id="df-352d4f39-f61d-4493-9d5d-1773a5e1a766">
      <button class="colab-df-quickchart" onclick="quickchart('df-352d4f39-f61d-4493-9d5d-1773a5e1a766')"
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
            document.querySelector('#df-352d4f39-f61d-4493-9d5d-1773a5e1a766 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```
!mkdir /content/drive/MyDrive/Congen_ITV/analises
```


```
!mkdir /content/drive/MyDrive/Congen_ITV/analises/roh
```


```
working_dir = '/content/drive/MyDrive/Congen_ITV/analises/roh'
vcf_path = '/content/drive/MyDrive/Congen_ITV/vcf/willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz'
```

Conda


```
!conda create -n bcftools_env -y
```

    Channels:
     - defaults
    Platform: linux-64
    Collecting package metadata (repodata.json): - \ | / - \ | / - \ | / - \ | / - \ | / done
    Solving environment: \ done
    
    ## Package Plan ##
    
      environment location: /usr/local/envs/bcftools_env
    
    
    
    
    Downloading and Extracting Packages:
    
    Preparing transaction: / done
    Verifying transaction: \ done
    Executing transaction: / done
    #
    # To activate this environment, use
    #
    #     $ conda activate bcftools_env
    #
    # To deactivate an active environment, use
    #
    #     $ conda deactivate
    



```
!conda run -n bcftools_env conda install -c bioconda -c conda-forge -y bcftools
```

    Channels:
     - bioconda
     - conda-forge
     - defaults
    Platform: linux-64
    Collecting package metadata (repodata.json): - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ | / - \ done
    Solving environment: / - done
    
    ## Package Plan ##
    
      environment location: /usr/local/envs/bcftools_env
    
      added / updated specs:
        - bcftools
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        _libgcc_mutex-0.1          |      conda_forge           3 KB  conda-forge
        _openmp_mutex-4.5          |            2_gnu          23 KB  conda-forge
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
        libcurl-8.13.0             |       h332b0f4_0         428 KB  conda-forge
        libdeflate-1.24            |       h86f0d12_0          71 KB  conda-forge
        libedit-3.1.20250104       | pl5321h7949ede_0         132 KB  conda-forge
        libev-4.33                 |       hd590300_2         110 KB  conda-forge
        libgcc-15.1.0              |       h767d61c_2         810 KB  conda-forge
        libgcc-ng-15.1.0           |       h69a702a_2          34 KB  conda-forge
        libgfortran-15.1.0         |       h69a702a_2          34 KB  conda-forge
        libgfortran5-15.1.0        |       hcea5267_2         1.5 MB  conda-forge
        libgomp-15.1.0             |       h767d61c_2         442 KB  conda-forge
        liblzma-5.8.1              |       hb9d3cd8_1         110 KB  conda-forge
        libnghttp2-1.64.0          |       h161d5f1_0         632 KB  conda-forge
        libopenblas-0.3.29         |pthreads_h94d23a6_0         5.6 MB  conda-forge
        libssh2-1.11.1             |       hcf80075_0         298 KB  conda-forge
        libstdcxx-15.1.0           |       h8f9b012_2         3.7 MB  conda-forge
        libstdcxx-ng-15.1.0        |       h4852527_2          34 KB  conda-forge
        libxcrypt-4.4.36           |       hd590300_1          98 KB  conda-forge
        libzlib-1.3.1              |       hb9d3cd8_2          60 KB  conda-forge
        ncurses-6.5                |       h2d0b736_3         871 KB  conda-forge
        openssl-3.5.0              |       h7b32b05_1         3.0 MB  conda-forge
        perl-5.32.1                | 7_hd590300_perl5        12.7 MB  conda-forge
        zstd-1.5.7                 |       hb8e6e7a_2         554 KB  conda-forge
        ------------------------------------------------------------
                                               Total:        40.4 MB
    
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
      libcurl            conda-forge/linux-64::libcurl-8.13.0-h332b0f4_0 
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
      zstd               conda-forge/linux-64::zstd-1.5.7-hb8e6e7a_2 
    
    
    
    Downloading and Extracting Packages: ...working...
    perl-5.32.1          | 12.7 MB   |            |   0% 
    
    libopenblas-0.3.29   | 5.6 MB    |            |   0% [A
    
    
    libstdcxx-15.1.0     | 3.7 MB    |            |   0% [A[A
    
    
    
    gsl-2.7              | 3.2 MB    |            |   0% [A[A[A
    
    
    
    
    htslib-1.21          | 3.0 MB    |            |   0% [A[A[A[A
    
    
    
    
    
    openssl-3.5.0        | 3.0 MB    |            |   0% [A[A[A[A[A
    
    
    
    
    
    
    libgfortran5-15.1.0  | 1.5 MB    |            |   0% [A[A[A[A[A[A
    
    
    
    
    
    
    
    krb5-1.21.3          | 1.3 MB    |            |   0% [A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    bcftools-1.21        | 987 KB    |            |   0% [A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    ncurses-6.5          | 871 KB    |            |   0% [A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    libgcc-15.1.0        | 810 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    libgomp-15.1.0       | 442 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libcurl-8.13.0       | 428 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    |            |   0% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    libstdcxx-15.1.0     | 3.7 MB    |            |   0% [A[A
    perl-5.32.1          | 12.7 MB   |            |   0% 
    
    libopenblas-0.3.29   | 5.6 MB    | 2          |   2% [A
    
    
    
    
    htslib-1.21          | 3.0 MB    | 3          |   4% [A[A[A[A
    
    
    
    gsl-2.7              | 3.2 MB    | 3          |   4% [A[A[A
    
    libopenblas-0.3.29   | 5.6 MB    | #####3     |  53% [A
    
    
    libstdcxx-15.1.0     | 3.7 MB    | ######2    |  63% [A[A
    
    
    
    gsl-2.7              | 3.2 MB    | ########3  |  83% [A[A[A
    
    
    
    
    htslib-1.21          | 3.0 MB    | #########4 |  94% [A[A[A[A
    perl-5.32.1          | 12.7 MB   | ##5        |  25% 
    
    
    
    
    htslib-1.21          | 3.0 MB    | ########## | 100% [A[A[A[A
    
    
    
    gsl-2.7              | 3.2 MB    | ########## | 100% [A[A[A
    perl-5.32.1          | 12.7 MB   | ######2    |  62% 
    
    
    
    
    
    openssl-3.5.0        | 3.0 MB    |            |   1% [A[A[A[A[A
    
    
    libstdcxx-15.1.0     | 3.7 MB    | ########## | 100% [A[A
    
    
    libstdcxx-15.1.0     | 3.7 MB    | ########## | 100% [A[A
    
    
    
    
    
    
    libgfortran5-15.1.0  | 1.5 MB    | 1          |   1% [A[A[A[A[A[A
    
    
    
    
    
    
    
    krb5-1.21.3          | 1.3 MB    | 1          |   1% [A[A[A[A[A[A[A
    
    libopenblas-0.3.29   | 5.6 MB    | ########## | 100% [A
    
    libopenblas-0.3.29   | 5.6 MB    | ########## | 100% [A
    perl-5.32.1          | 12.7 MB   | #########4 |  95% 
    
    
    
    
    
    
    
    
    bcftools-1.21        | 987 KB    | 1          |   2% [A[A[A[A[A[A[A[A
    
    
    
    
    
    
    libgfortran5-15.1.0  | 1.5 MB    | ########## | 100% [A[A[A[A[A[A
    
    
    
    
    
    
    
    krb5-1.21.3          | 1.3 MB    | ########## | 100% [A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    bcftools-1.21        | 987 KB    | ########## | 100% [A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    libgcc-15.1.0        | 810 KB    | 1          |   2% [A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    ncurses-6.5          | 871 KB    | 1          |   2% [A[A[A[A[A[A[A[A[A
    
    
    
    
    
    openssl-3.5.0        | 3.0 MB    | ########## | 100% [A[A[A[A[A
    
    
    
    
    
    openssl-3.5.0        | 3.0 MB    | ########## | 100% [A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | 2          |   3% [A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    libgcc-15.1.0        | 810 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    ncurses-6.5          | 871 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    | 2          |   3% [A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    libgomp-15.1.0       | 442 KB    | 3          |   4% [A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    libgomp-15.1.0       | 442 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libcurl-8.13.0       | 428 KB    | 3          |   4% [A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | 5          |   5% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libcurl-8.13.0       | 428 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | 6          |   6% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | #          |  11% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | 7          |   8% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    perl-5.32.1          | 12.7 MB   | ########## | 100% 
    
    
    
    
    htslib-1.21          | 3.0 MB    | ########## | 100% [A[A[A[A
    
    
    libstdcxx-15.1.0     | 3.7 MB    | ########## | 100% [A[A
    
    libopenblas-0.3.29   | 5.6 MB    | ########## | 100% [A
    
    
    
    
    
    
    libgfortran5-15.1.0  | 1.5 MB    | ########## | 100% [A[A[A[A[A[A
    
    
    
    
    
    
    libgfortran5-15.1.0  | 1.5 MB    | ########## | 100% [A[A[A[A[A[A
    
    
    
    gsl-2.7              | 3.2 MB    | ########## | 100% [A[A[A
    
    
    
    
    
    
    
    krb5-1.21.3          | 1.3 MB    | ########## | 100% [A[A[A[A[A[A[A
    
    
    
    
    
    
    
    krb5-1.21.3          | 1.3 MB    | ########## | 100% [A[A[A[A[A[A[A
    
    
    
    
    
    openssl-3.5.0        | 3.0 MB    | ########## | 100% [A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    libgcc-15.1.0        | 810 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    libgcc-15.1.0        | 810 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    bcftools-1.21        | 987 KB    | ########## | 100% [A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    bcftools-1.21        | 987 KB    | ########## | 100% [A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    libnghttp2-1.64.0    | 632 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    zstd-1.5.7           | 554 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    libgomp-15.1.0       | 442 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    libgomp-15.1.0       | 442 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libcurl-8.13.0       | 428 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libcurl-8.13.0       | 428 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    libssh2-1.11.1       | 298 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    bzip2-1.0.8          | 247 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
     ... (more hidden) ...[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    ca-certificates-2025 | 149 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    c-ares-1.34.5        | 202 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    ncurses-6.5          | 871 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A
    
    
    
    
    
    
    
    
    
    ncurses-6.5          | 871 KB    | ########## | 100% [A[A[A[A[A[A[A[A[A
    perl-5.32.1          | 12.7 MB   | ########## | 100% 
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
                          
    [A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A[A
                                                         
    
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
    
    
    
    
    
    
    
    
    
    
    
    
    
    [A[A[A[A[A[A[A[A[A[A[A[A[A done
    Preparing transaction: | / done
    Verifying transaction: \ | / - \ | / - \ | / - \ | / - \ done
    Executing transaction: / - \ | / - \ done
    



```
!conda run -n bcftools_env bcftools --version
```

    bcftools 1.21
    Using htslib 1.21
    Copyright (C) 2024 Genome Research Ltd.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.
    



```
# to extract per‐site AF from your VCF (if it has an AF/AC/AN INFO tag):
!conda run -n bcftools_env bcftools query -f '%CHROM\t%POS\t%INFO/AF\n' {vcf_path} > af.txt

# then use --AF-file af.txt in the ROH step
```


```
!ls -lht
```

    total 340M
    -rw-r--r-- 1 root root  44M May 22 14:10 af.txt
    drwx------ 7 root root 4.0K May 22 13:25 drive
    drwxr-xr-x 1 root root 4.0K May 14 13:38 sample_data
    -rw-r--r-- 1 root root 149M Apr 30 20:14 Miniconda3-latest-Linux-x86_64.sh
    -rw-r--r-- 1 root root 149M Apr 30 20:14 Miniconda3-latest-Linux-x86_64.sh.1



```
!conda run -n bcftools_env bcftools roh \
--AF-dflt 0.4 \
--rec-rate 1e-8 \
--threads 4 \
-o willisornis.roh.txt \
{vcf_path}
```

    Number of target samples: 20
    Number of --estimate-AF samples: 0
    Number of sites in the buffer/overlap: unlimited
    Number of lines total/processed: 1855451/1824979
    Number of lines filtered/no AF/no alt/multiallelic/dup: 0/0/0/0/0
    



```
!grep "RG" willisornis.roh.txt | head
```

    # RG	[2]Sample	[3]Chromosome	[4]Start	[5]End	[6]Length (bp)	[7]Number of markers	[8]Quality (average fwd-bwd phred score)
    RG	ITV66049	h1.Chr24	1589	23833	22245	270	88.3
    RG	ITV66049	h1.Chr24	26490	73203	46714	314	33.9
    RG	ITV66049	h1.Chr24	73328	92633	19306	342	56.7
    RG	ITV66049	h1.Chr24	107113	109844	2732	211	82.8
    RG	ITV66049	h1.Chr24	111548	114682	3135	243	80.5
    RG	ITV66049	h1.Chr24	117961	125192	7232	671	90.9
    RG	ITV66049	h1.Chr24	126023	135682	9660	570	72.8
    RG	ITV66049	h1.Chr24	141880	151388	9509	844	89.6
    RG	ITV66049	h1.Chr24	152223	156802	4580	363	90.6



```
import pandas as pd
import matplotlib.pyplot as plt

# 1) Read in the TRACK‐style ROH file
fn = 'willisornis.roh.txt'  # adjust path
#   - skip lines beginning with '#'
#   - no header in the data lines
#   - columns: tag, sample, chrom, start, end, length_bp, n_markers, quality
df = pd.read_csv(
    fn,
    sep=r'\s+',
    comment='#',
    header=None,
    names=['tag','sample','chrom','start','end','length_bp','n_markers','quality']
)

# 2) Keep only the actual ROH records (the 'RG' tag)
df = df[df['tag'] == 'RG']

# 3) Aggregate by sample
agg = (
    df
    .groupby('sample')
    .agg(
        n_roh=('length_bp','count'),         # number of ROH segments
        total_length_bp=('length_bp','sum')   # total ROH length in bp
    )
    .reset_index()
)
# convert to megabases for easier plotting
agg['total_length_Mb'] = agg['total_length_bp'] / 1e6

# 4) Plot
plt.figure(figsize=(8,6))
plt.scatter(agg['n_roh'], agg['total_length_Mb'], s=80)

# annotate each point with the sample name
for _, row in agg.iterrows():
    plt.text(
        row['n_roh'] + 0.2,
        row['total_length_Mb'],
        row['sample'],
        fontsize=8,
        va='center'
    )

plt.xlabel('Number of ROH segments')
plt.ylabel('Total ROH length (Mb)')
plt.title('Per‐Sample ROH burden')
plt.grid(linestyle='--', alpha=0.5)
plt.tight_layout()
plt.show()
```


    
![png](Endocruzamento_files/Endocruzamento_14_0.png)
    


## Coeficiente de endocruzamento (FROH)

Como a análise foi feita para apenas dois cromossomos precisamos usar o comprimento total dos dois. Caso o genoma completo seja usado é utilizado o comprimento total do genoma.


```
!cat /content/drive/MyDrive/Congen_ITV/reference/bWildVid.fna.fai | grep Chr24 | cut -f2
!cat /content/drive/MyDrive/Congen_ITV/reference/bWildVid.fna.fai | grep Chr25 | cut -f2
```

    7271986
    7195970



```bash
%%bash
a=7271986
b=7195970
sum=$((a + b))
echo "Sum is $sum"
```

    Sum is 14467956



```
# total genome length (bp) for FROH
genome_length = 14_467_956

# 1) Compute FROH per individual
agg['FROH'] = agg['total_length_bp'] / genome_length

# 2) Sort by FROH (optional, for nicer plotting)
agg = agg.sort_values('FROH', ascending=False)

# 3) Bar‐plot FROH
plt.figure(figsize=(10,6))
plt.bar(agg['sample'], agg['FROH'], edgecolor='k')
plt.xticks(rotation=90, fontsize=8)
plt.ylabel('FROH')
plt.title('Per‐Sample FROH')
plt.tight_layout()
plt.show()
```


    
![png](Endocruzamento_files/Endocruzamento_19_0.png)
    



```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# 1) (Re)load your TRACK‐style ROH file if needed
fn = 'willisornis.roh.txt'  # adjust path
#   - skip lines beginning with '#'
#   - no header in the data lines
#   - columns: tag, sample, chrom, start, end, length_bp, n_markers, quality
df = pd.read_csv(
    fn,
    sep=r'\s+',
    comment='#',
    header=None,
    names=['tag','sample','chrom','start','end','length_bp','n_markers','quality']
)
df = df[df['tag'] == 'RG']  # keep only ROH lines

# 2) (Optionally) drop any segments <500 kb so they don’t get “uncategorized”
df = df[df['length_bp'] >= 500_000]

# 3) Define strictly increasing bins and labels
bins   = [500_000,      1_000_000,      5_000_000,      np.inf]
labels = ['500 kb – 1 Mb', '1 Mb – 5 Mb', '> 5 Mb']

# 4) Cut into categories
df['size_cat'] = pd.cut(
    df['length_bp'],
    bins=bins,
    labels=labels,
    right=True,
    include_lowest=True
)

# 5) Count segments per sample × category
counts = (
    df
    .groupby(['sample','size_cat'])
    .size()
    .unstack(fill_value=0)
)

# 6) Plot stacked bars
ax = counts.plot(
    kind='bar',
    stacked=True,
    figsize=(12,6),
    edgecolor='black'
)
ax.set_xlabel('Sample')
ax.set_ylabel('Number of ROH segments')
ax.set_title('ROH counts by length category per individual')
plt.xticks(rotation=90, fontsize=8)
plt.legend(title='ROH size', bbox_to_anchor=(1.02,1), loc='upper left')
plt.tight_layout()
plt.show()
```

    <i-input-39-2e73adbec492>:38: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      .groupby(['sample','size_cat'])



    
![png](Endocruzamento_files/Endocruzamento_20_1.png)
    



```
import matplotlib.pyplot as plt

def plot_roh_tracks(df, sample):
    # 1) Filter to that sample and only real ROH records
    samp = df[(df['tag']=='RG') & (df['sample']==sample)]
    if samp.empty:
        raise ValueError(f"No ROH records found for {sample}")

    # 2) Determine chromosome order (by the number after “Chr”)
    chroms = sorted(
        samp['chrom'].unique(),
        key=lambda s: int(s.split('Chr')[-1])
    )
    y_pos = {chrom:i for i,chrom in enumerate(chroms)}

    # 3) Plot
    fig, ax = plt.subplots(figsize=(10, len(chroms)*0.4))
    for _, row in samp.iterrows():
        c = row['chrom']
        start, length = row['start'], row['end']-row['start']
        ax.broken_barh(
            [(start, length)],
            (y_pos[c]-0.3, 0.6),
        )

    # 4) Cosmetics
    ax.set_yticks(list(y_pos.values()))
    ax.set_yticklabels(chroms)
    ax.set_xlabel("Position (bp)")
    ax.set_ylabel("Chromosome")
    ax.set_title(f"ROH segments for {sample}")
    ax.grid(True, linestyle='--', alpha=0.3)
    plt.tight_layout()
    plt.show()

# — example usage —
plot_roh_tracks(df, 'ITV66028')
```

    <i-input-42-27044ce11a50>:33: UserWarning: Tight layout not applied. The bottom and top margins cannot be made large enough to accommodate all Axes decorations.
      plt.tight_layout()



    
![png](Endocruzamento_files/Endocruzamento_21_1.png)
    

