---
layout: single
title: "GONE"
permalink: /pages/GONE/
---

# ## GONE ##
**Tutorial desenvolvido para o curso de PopGen do ITV-DS/ICMBio**  
**Autora:** Carlynne Simões (carlynne.simoes@pq.itv.org)  
**Data:** Maio de 2025  

---

### 🎯 Objetivo

O **GONE** (Genetic Optimization for Ne estimation) é um programa que calcula e usa o desequilíbrio de ligação (*linkage disequilibrium* - LD) em SNPs para inferir a trajetória do **tamanho efetivo populacional (Ne)** ao longo de ~100-200 gerações passadas, oferecendo alta resolução para eventos recentes.  

- Disponível em: [GitHub - GONE](https://github.com/esrud/GONE/tree/master)  
- Publicado em: [DOI:10.1093/molbev/msaa169](https://doi.org/10.1093/molbev/msaa169)

---

### ✅ Requisitos Básicos

- **Tamanho amostral:** ≥ 25 indivíduos (menos que isso pode prejudicar a análise)
- **Estrutura populacional:** Amostras de uma única população, indivíduos não aparentados
- **VCF:** Apenas variantes bialélicas

---

### 🛠️ Pré-requisitos de Software

- **Ambiente Conda:** (disponível no servidor do ITV)
  ```bash
  $ conda activate gone_env
  ```

- **GONE:** (clonar repositório)
  ```bash
  $ git clone https://github.com/esrud/GONE.git
  ```

- **Exemplo de estrutura de diretórios:**
  ```
  GONE/
  ├── GONE/                # Pasta clonada do GitHub
  ├── input.vcf.gz
  ├── preprocess_gone.pbs
  ├── run_gone.pbs
  ├── run_0525/
  │   ├── PROGRAMMES/      # Binário compilado
  │   └── arquivos de saída
  └── run_0625/
      ├── PROGRAMMES/
      └── arquivos de saída
  ```

- **Formato de entrada esperado:** `.ped` e `.map`

---

### 🔄 Pré-processamento

1. **Ativar o Conda**
   ```bash
   $ conda activate gone_env
   ```

2. **Filtragem**
   ```bash
   # Variantes bialélicas
   $ bcftools view -m2 -M2 -v snps input.vcf.gz -o step1_biallelic.vcf

   # SNPs com >5% de missing
   $ vcftools --vcf step1_biallelic.vcf --max-missing 0.95 --recode --out step2_filtered

   # Qualidade mínima Q30
   $ vcftools --vcf step2_filtered.recode.vcf --minQ 30 --recode --out step3_quality

   # MAF < 0.01
   $ vcftools --vcf step3_quality.recode.vcf --maf 0.01 --recode --out final_filtered
   ```

3. **Conversão para PLINK (.bed/.bim/.fam)**
   ```bash
   $ plink --vcf final_filtered.recode.vcf --make-bed --allow-extra-chr --chr-set 36 --out nome_arquivo_saida
   ```

   > ⚠️ **Flags importantes**  
   - `--allow-extra-chr`: Permite uso de cromossomos com nomes não padrão.  
   - `--chr-set`: Define o número de cromossomos (e.g., `--chr-set 36`).

4. **Checar número de SNPs e indivíduos**
   ```bash
   $ plink --bfile nome_arquivo_saida --freq --out nome_arquivo_saida_freq
   ```

5. **Conversão para formato PED/MAP**
   ```bash
   $ plink --bfile nome_arquivo_saida --recode --allow-extra-chr --chr-set 36 --out nome_arquivo_saida
   ```

   > 💡 **Dica:**  
   O PLINK pode não aceitar nomes diferentes de cromossomos (e.g., muitos scaffolds). Para resolver:

   ```bash
   # Listar cromossomos únicos
   $ bcftools query -f '%CHROM\n' "$VCF" | sort | uniq > chroms_present.txt

   # Extrair cromossomos desejados
   $ bcftools view -R cromossomos.txt input.vcf.gz -Oz -o output.main.vcf.gz

   # Renomear cromossomos
   $ bcftools annotate --rename-chrs rename_chrom.txt -o output.cleaned.vcf.gz -O z input.main.vcf.gz

   # Indexar
   $ bcftools index output.cleaned.vcf.gz
   ```

---

### 🚀 Rodando o GONE

Entre na pasta com o programa compilado:

```bash
$ ./script_GONE.sh nome_arquivos_saida
```

---

### 📂 Arquivos de saída

1. `TEMPORARY_FILES/` - arquivos temporários  
2. `OUTPUT_<nome>` - informações dos SNPs, HW, cromossomos  
3. `OUTPUT_Ne_<nome>` - resultado principal (Ne vs. gerações)  
4. `OUTPUT_d2_<nome>` - desequilíbrio de ligação (observado vs. esperado)  
5. `TIMEFILE` - progresso da análise

---

### 📊 Plotando o Gráfico no R

```r
# Diretório de trabalho
setwd("diretorio_com_meus_arquivos")
dir()

# Carregar dados
dados <- read.table("Output_Ne", skip = 1, header = FALSE)

# Nomear colunas
colnames(dados) <- c("Gen", "Ne")

# Converter para numérico
dados$Gen <- as.numeric(as.character(dados$Gen))

# Definir tempo de geração
tempo_geracao <- 10.4  # ajuste para sua espécie
dados$Ano <- dados$Gen * tempo_geracao

# Plotar
plot(dados$Ano, dados$Ne, type = "l", col = "#efb430ff", lwd = 5,
     xlab = "Tempo (anos)", ylab = "Tamanho efetivo (Ne)",
     main = "Nome da Espécie")

# Linha de referência
abline(h = 500, col = "red", lty = 2, lwd = 1)
```
