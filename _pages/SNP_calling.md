---
layout: single
title: "SNP Calling e Filtragem de SNPs"
permalink: /pages/SNP_calling/
---

# 💻 SNP calling (chamada de variantes)

Após ter as *reads* mapeadas contra um genoma de referêcnia, nós precisamos identificar posições em que as *reads* não são idênticas à referência. Essas diferenças são chamadas de variantes genômicas, e representam regiões onde o organismo sequenciado possui uma base diferente da referência. Isso pode ocorrer por motivos naturais (variação entre indivíduos) ou patológicos (mutações), fornecendo informações importantes sobre diversidade genética, saúde, evolução e muito mais.

## ↠ Tipos de variantes genômicas

Essas variantes podem ocorrer em diferentes escalas, desde a troca de uma única base até mudanças que envolvem grandes regiões cromossômicas. Entre os tipos mais importantes estão os SNPs, os indels e as variantes estruturais (SVs).

## SNPs (Single Nucleotide Polymorphisms)

Os SNPs são os tipos de variantes mais simples e mais frequentes no genoma. Um SNP ocorre quando uma única base no DNA é substituída por outra. Por exemplo, se a base "C" está presente em uma posição específica da sequência de referência, mas um indivíduo apresenta um "T" nessa mesma posição, isso configura um SNP.

Essas pequenas variações podem parecer discretas, mas são altamente informativas. Elas podem ocorrer em regiões codificantes ou não codificantes do genoma, e podem influenciar desde a expressão de um gene até a estrutura de uma proteína. Em muitos casos, os SNPs não têm efeito funcional direto, mas ainda assim são úteis como marcadores genéticos.

* SNPs são amplamente utilizados em estudos de:
* Diversidade genética entre populações
* Seleção natural e adaptação
* Evolução molecular
* Associação genômica com doenças ou características fenotípicas (como em estudos GWAS)

## INDELs (Inserções e Deleções)

Indels são variantes em que ocorrem inserções ou deleções de pequenas sequências de nucleotídeos em comparação com a referência. Elas geralmente envolvem poucos pares de bases, mas mesmo mudanças mínimas podem causar um impacto funcional significativo — especialmente em regiões codificantes de genes.

Por exemplo, a deleção de uma única base dentro de um gene pode causar uma mudança no quadro de leitura (*frameshift*), alterando todos os códons seguintes e, consequentemente, a proteína resultante. Da mesma forma, uma inserção pode introduzir bases extras, modificando a estrutura da proteína ou interrompendo sua função.

Indels são importantes em estudos de:

* Mutagênese
* Disfunções proteicas
* Doenças genéticas
* Seleção purificadora ou positiva

## Variantes Estruturais (SVs)

As variantes estruturais (SVs) são alterações genômicas em larga escala, geralmente envolvendo segmentos com dezenas a milhões de bases. Ao contrário dos SNPs e indels, que afetam pequenas porções da sequência, as SVs podem modificar significativamente a arquitetura genômica.

Entre os principais tipos de SVs estão:

* Grandes deleções: remoção de segmentos inteiros do DNA
* Duplicações: cópia adicional de uma região
* Inversões: quando um trecho do DNA é invertido
* Translocações: troca de segmentos entre cromossomos diferentes

As SVs podem interromper genes, fundir partes de genes diferentes, alterar a regulação de regiões vizinhas e até gerar novas combinações gênicas. São frequentemente associadas a distúrbios genéticos, câncer e doenças complexas, além de terem papel importante em processos evolutivos, como a especiação.

## ↠ Ferramentas disponíveis

Existem diversas ferramentas bioinformáticas desenvolvidas para identificar variantes genômicas a partir de dados de sequenciamento. Cada ferramenta tem suas abordagens, pontos fortes e requisitos, e a escolha ideal depende do tipo de projeto, da profundidade dos dados, da qualidade do genoma de referência e dos objetivos da análise.

Entre as ferramentas mais utilizadas, destacam-se:

* GATK (Genome Analysis Toolkit): amplamente usado em projetos médicos e humanos, é conhecido como o padrão ouro em chamadas de variantes. No entanto, exige etapas complexas de pré-processamento (como realinhamento e recalibração de base) e depende de genomas de alta qualidade, o que pode ser um desafio em espécies selvagens com genomas menos completos.

* DeepVariant: usa aprendizado profundo (inteligência artificial) para identificar variantes com alta precisão. Apesar do bom desempenho, é uma ferramenta mais pesada e depende de estruturas de dados organizadas, o que nem sempre se encaixa bem em projetos com espécies não-modelo e genomas rascunho.

* bcftools: leve, rápido e de fácil uso, o bcftools é altamente recomendado para projetos com espécies não-modelo, genomas ainda em construção ou análises populacionais com dezenas de indivíduos. Ele oferece um fluxo simples, com poucos requisitos e excelente compatibilidade com outras ferramentas como samtools, vcftools e plink.

No contexto do curso, vamos utilizar apenas o bcftools, por ser leve, direto e eficiente. Podemos citar algumas vantagens que ele apresenta, como:

* Simples de instalar e usar, com comandos diretos
* Não exige pré-processamento complexo, ideal para genomas não-modelo
* Funciona bem com dados de cobertura moderada
* Produz arquivos .vcf compatíveis com ferramentas *downstream* (como vcftools, plink, angsd)

Com o bcftools, vamos realizar o SNP calling completo, da leitura dos dados ao arquivo final de variantes, que poderá ser usado em análises de diversidade genética, estrutura populacional, seleção natural e outros tópicos fundamentais em genômica para conservação.

## ↠ Arquivos de entrada e saída

### Arquivos de entrada obrigatórios:

**Genoma de referência (.fasta)**

É o arquivo com a sequência de DNA usada como base para comparar os reads. Mesmo que o genoma da espécie ainda seja um rascunho ou incompleto, ele é necessário para alinhar e identificar variantes.

**Arquivo .fai (índice do .fasta)**

Criado com samtools faidx, esse índice permite acesso rápido a regiões específicas do genoma. Sem ele, o bcftools não consegue navegar eficientemente pela referência.

**Arquivo .bam (alinhamento dos reads)**

Resultado do mapeamento, contém os reads posicionados ao longo do genoma. É o que o bcftools usa para verificar onde ocorrem as diferenças.

**Arquivo .bai (índice do .bam)**

Permite que o bcftools acesse rapidamente partes específicas do .bam, o que acelera a análise. Criado com samtools index.

### Arquivos de saída:

**Arquivo .bcf (binário de variantes brutas)**

Resultado intermediário gerado pelo mpileup. É um formato binário eficiente, usado como entrada para a etapa de chamada de variantes.

**Arquivo .vcf (Variant Call Format)**

Contém as variantes identificadas (SNPs e indels) em texto legível. Cada linha descreve uma variante: posição, base de referência, base alternativa, qualidade, genótipos, etc.

**Arquivo .vcf.gz**

É a versão compactada do .vcf, mais leve e rápida para carregar. Usada por padrão em análises downstream.

**Arquivo .gvcf (Genomic VCF)**

É uma variação do formato VCF que contém informações tanto sobre posições variantes quanto sobre posições invariantes do genoma. Enquanto um VCF normal reporta apenas as posições onde foi encontrada uma variante, o gVCF também inclui regiões onde não houve variação detectada, mas onde há evidência suficiente para afirmar que o indivíduo é homozigoto para a referência.

### Vamos começar verificando se os arquivos fai e bai já existem.


```
# Vamos entrar na pasta do curso.
from google.colab import drive
drive.mount('/content/drive')

%cd /content/drive/MyDrive/Congen_ITV
```


```
# Vamos verificar se existe o arquivo .fai e o arquivo .bai
!ls /content/drive/MyDrive/Congen_ITV/reference/
!ls /content/drive/MyDrive/Congen_ITV/bams/
```


```
!conda run -n genomics_env samtools faidx /content/drive/MyDrive/Congen_ITV/reference/bWildVid.reduced.fasta
```

---
## ↠ Rodando o bcftools

Vamos começar instalando o conda e os pacotes que vamos utilizar.




```
# Install Miniconda (1–2 min)
!wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
! Miniconda3-latest-Linux-x86_64.sh -bfp /usr/local

# Add conda to the environment
import sys
sys.path.append('/usr/local/lib/3.7/site-packages')

# Add Bioconda channels
!conda config --add channels defaults
!conda config --add channels bioconda
!conda config --add channels conda-forge
!conda create -n genomics_env -y
!conda install -n genomics_env -c bioconda -c conda-forge bcftools samtools -y
```

Agora que já sabemos quais são os arquivos de entrada necessários para realizar o SNP calling, vamos tornar nosso processo mais organizado e reutilizável.

Para isso, vamos definir variáveis no início do nosso script. Assim, se precisarmos mudar o nome do arquivo .bam ou da referência, podemos fazer isso em um só lugar, sem alterar todos os comandos manualmente.




```
gDNA_ref = "/content/drive/MyDrive/Congen_ITV/reference/bWildVid.reduced.fasta"
gDNA_fai = "/content/drive/MyDrive/Congen_ITV/reference/bWildVid.reduced.fasta.fai"
map_bam = "/content/drive/MyDrive/Congen_ITV/bams/"
map_bai = "/content/drive/MyDrive/Congen_ITV/bams/"
bam_dir = "/content/drive/MyDrive/Congen_ITV/bams/"
```

Para fazer a chamada de variantes com o bcftools, passamos por algumas etapas específicas com parâmetros importantes. Vamos olhar eles rapidamente.

O primeiro passo é usar o `bcftoos mpileup`. Esta etapa calcula a profundidade de cobertura e a qualidade de base para cada posição do genoma. O `mpileup` lê os arquivos `.bam` mapeados e compara os reads com o genoma de referência, gerando um arquivo `.bcf` com as informações necessárias para detectar variantes.

A linha de comando básica para essa etapa é:
```
bcftools mpileup -Ou -f referencia.fasta mapeamento.bam > saida.bcf
```
Os parâmetros mais usados nessa etapa são:

* `-f`: que indica o arquivo de referência `.fasta`.
* `-Ou`: indica uma saída descompactada em formato BCF.
* `-q`: para filtrar reads com baixa qualidade de mapeamento (ex: -q 20).
* `-Q`: para filtrar bases com baixa qualidade de chamada (ex: -Q 20).


A segunda etapa é a chamada de variantes, com o `bcftools call`. Nessa etapa os dados do `mpileup` são análisados e é indentificado onde há variantes, gerando um arquivo `.vcf` com as chamadas.

A linha de comando básica para essa etapa é:
```
bcftools call -mv -Ov -o variantes.vcf saida.bcf
```
Os parâmetros mais usados nessa etapa são:
* `-m`: usa o algoritmo multialélico (mais sensível, recomendado para estudos populacionais).
* `-v`: reporta apenas as posições com variantes (ignora posições idênticas à referência).
* `-c`: usa o algoritmo simples bialélico (opcional, menos sensível, mas útil para diversas análises posteriores).
* `-Ov`: saída em formato texto VCF (em vez de binário .bcf).
* `-o`: define o nome do arquivo de saída.

A terceira etapa é a filtragem de variantes com baixa confiança, utilizando o `bcftools filter`. Isso ajuda a remover falsos positivos, que podem ocorrer por erros de sequenciamento, cobertura insuficiente ou mapeamentos ambíguos.

A linha de comando básica para essa etapa é:
```
bcftools filter -i 'QUAL>20 && DP>10' -o variantes_filtradas.vcf variantes.vcf
```
Os parâmetros mais usados nessa etapa são:
* `-i`: expressão lógica que define os critérios mínimos que uma variante deve atender para ser mantida
* `QUAL`: qualidade da variante (quanto maior, mais confiável).
* `DP`: profundidade de cobertura (número de leituras cobrindo a posição).
* `MQ`: qualidade média de mapeamento das leituras (útil para excluir regiões de mapeamento ruim).
* `-m`: número mínimo de alelos alternativos (por exemplo, -m2 exclui variantes monomórficas).
* `-M`: número máximo de alelos alternativos (por exemplo, -M1 mantém apenas variantes bialélicas).

Configuirar os filtros para que restem apenas variantes bialélicas é útil em análises populacionais, onde muitas ferramentas (como plink, ADMIXTURE e vcftools) só funcionam corretamente com SNPs bialélicos.

Por fim, fazemos a normalização do VCF, com o `bcftools norm`. Essa etapa é responsável pela organização das variantes, e é fundamental se você for comparar arquivos VCF, fundir variantes de múltiplos indivíduos ou realizar anotação funcional.

A linha de comando básica para essa etapa é:
```
bcftools norm -f referencia.fasta -Oz -o variantes_norm.vcf.gz variantes_filtradas.vcf
```
Os parâmetros mais usados nessa etapa são:

* `-f`: genoma de referência.
* `-Oz`: saída comprimida em .vcf.gz.
* `--check-ref w`: ignora discrepâncias leves com a referência.
* `--rm-dup`: remove duplicatas de variantes idênticas.

Apesar de não ser obrigatório, podemos fazer a indexação do VCF com o `bcftools index`. Além de gerar um VCF comapctado, o que já é útil pelo VCF ser um arquivo grande, ele também gera o arquivo .tbi que permite buscas rápidas no VCF, útil para ferramentas como bcftools view e IGV.

A linha de comando básica para essa etapa é:
```
bcftools index variantes_norm.vcf.gz
```

Vamos agora rodar todas essas etapas em um script .


```
!conda run -n genomics_env  -c \
"bcftools mpileup -Ou -f {gDNA_ref} {map_bam} | \
bcftools call -mv -Ob -o {bams_dir}bWildVid.reduced.raw.bcf && \
bcftools norm -f {gDNA_ref} -Oz -o {bams_dir}bWildVid.reduced.variants.norm.vcf.gz {bams_dir}bWildVid.reduced.raw.bcf && \
bcftools filter -i 'QUAL>30 && DP>8' -m2 -M2 -Oz -o {bams_dir}bWildVid.reduced.filtered.vcf.gz {bams_dir}bWildVid.reduced.variants.norm.vcf.gz && \
bcftools index {bams_dir}bWildVid.reduced.filtered.vcf.gz"
```

---
## ↠ Arquivo VCF e visualização
Um arquivo VCF possui duas seções principais:

* Cabeçalho (header): começa com ##, contendo informações sobre a amostra, filtros, formatos, ferramentas utilizadas etc.

* Tabela de variantes: começa com uma linha iniciada por #CHROM, que define as colunas da tabela. Cada linha a seguir descreve uma variante.

### O cabeçalho do VCF segue esse padrão:

| Campo        | Descrição                                                                            |
| ------------ | ------------------------------------------------------------------------------------ |
| `#CHROM`     | Cromossomo onde a variante foi encontrada                                            |
| `POS`        | Posição da variante no cromossomo                                                    |
| `ID`         | Identificador da variante (se existir, ex: dbSNP); geralmente `.` em variantes novas |
| `REF`        | Base(s) presente(s) na referência                                                    |
| `ALT`        | Base(s) alternativa(s) encontradas nos reads                                         |
| `QUAL`       | Qualidade da chamada da variante (quanto maior, mais confiável)                      |
| `FILTER`     | Resultado da filtragem (ex: `PASS` ou o nome do filtro que excluiu a variante)       |
| `INFO`       | Informações adicionais (como profundidade total, número de reads de apoio, etc.)     |
| `FORMAT`     | Formato dos dados genotípicos nas colunas seguintes                                  |
| `<amostras>` | Genótipos e métricas para cada indivíduo (ex: `0/1`, `1/1`, `DP=20`, etc.)           |


**Vamos visualizar o arquivo.**


```
!zcat {bams_dir}bWildVid.reduced.filtered.vcf.gz | head -n 20
```
