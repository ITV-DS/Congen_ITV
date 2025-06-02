---
layout: single
title: "Análises de Estruturação: PCA"
permalink: /pages/PCA/
---

```

```

##Arquivos VCF


```
!wget -qO- https://s3.amazonaws.com/plink1-assets/plink_linux_x86_64_20210606.zip > plink.zip
!unzip -o plink.zip -d /usr/local/bin/
!chmod +x /usr/local/bin/plink
```

    Archive:  plink.zip
      inflating: /usr/local/bin/plink    
      inflating: /usr/local/bin/LICENSE  
      inflating: /usr/local/bin/toy.ped  
      inflating: /usr/local/bin/toy.map  
      inflating: /usr/local/bin/prettify  



```
!plink --version
```

    PLINK v1.90b6.24 64-bit (6 Jun 2021)



```
from google.colab import files
uploaded = files.upload()
```



     <input type="file" id="files-eb837589-f92e-4474-a23c-652dceed0153" name="files[]" multiple disabled
        style="border:none" />
     <output id="result-eb837589-f92e-4474-a23c-652dceed0153">
      Upload widget is only available when the cell has been executed in the
      current browser session. Please rerun this cell to enable.
      </output>
      <script>// Copyright 2017 Google LLC
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//      http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * @fileoverview Helpers for google.colab  module.
 */
(function(scope) {
function span(text, styleAttributes = {}) {
  const element = document.createElement('span');
  element.textContent = text;
  for (const key of Object.keys(styleAttributes)) {
    element.style[key] = styleAttributes[key];
  }
  return element;
}

// Max number of bytes which will be uploaded at a time.
const MAX_PAYLOAD_SIZE = 100 * 1024;

function _uploadFiles(inputId, outputId) {
  const steps = uploadFilesStep(inputId, outputId);
  const outputElement = document.getElementById(outputId);
  // Cache steps on the outputElement to make it available for the next call
  // to uploadFilesContinue from .
  outputElement.steps = steps;

  return _uploadFilesContinue(outputId);
}

// This is roughly an async generator (not supported in the browser yet),
// where there are multiple asynchronous steps and the  side is going
// to poll for completion of each step.
// This uses a Promise to block the  side on completion of each step,
// then passes the result of the previous step as the input to the next step.
function _uploadFilesContinue(outputId) {
  const outputElement = document.getElementById(outputId);
  const steps = outputElement.steps;

  const next = steps.next(outputElement.lastPromiseValue);
  return Promise.resolve(next.value.promise).then((value) => {
    // Cache the last promise value to make it available to the next
    // step of the generator.
    outputElement.lastPromiseValue = value;
    return next.value.response;
  });
}

/**
 * Generator function which is called between each async step of the upload
 * process.
 * @param {string} inputId Element ID of the input file picker element.
 * @param {string} outputId Element ID of the output display.
 * @return {!Iterable<!Object>} Iterable of next steps.
 */
function* uploadFilesStep(inputId, outputId) {
  const inputElement = document.getElementById(inputId);
  inputElement.disabled = false;

  const outputElement = document.getElementById(outputId);
  outputElement.innerHTML = '';

  const pickedPromise = new Promise((resolve) => {
    inputElement.addEventListener('change', (e) => {
      resolve(e.target.files);
    });
  });

  const cancel = document.createElement('button');
  inputElement.parentElement.appendChild(cancel);
  cancel.textContent = 'Cancel upload';
  const cancelPromise = new Promise((resolve) => {
    cancel.onclick = () => {
      resolve(null);
    };
  });

  // Wait for the user to pick the files.
  const files = yield {
    promise: Promise.race([pickedPromise, cancelPromise]),
    response: {
      action: 'starting',
    }
  };

  cancel.remove();

  // Disable the input element since further picks are not allowed.
  inputElement.disabled = true;

  if (!files) {
    return {
      response: {
        action: 'complete',
      }
    };
  }

  for (const file of files) {
    const li = document.createElement('li');
    li.append(span(file.name, {fontWeight: 'bold'}));
    li.append(span(
        `(${file.type || 'n/a'}) - ${file.size} bytes, ` +
        `last modified: ${
            file.lastModifiedDate ? file.lastModifiedDate.toLocaleDateString() :
                                    'n/a'} - `));
    const percent = span('0% done');
    li.appendChild(percent);

    outputElement.appendChild(li);

    const fileDataPromise = new Promise((resolve) => {
      const reader = new FileReader();
      reader.onload = (e) => {
        resolve(e.target.result);
      };
      reader.readAsArrayBuffer(file);
    });
    // Wait for the data to be ready.
    let fileData = yield {
      promise: fileDataPromise,
      response: {
        action: 'continue',
      }
    };

    // Use a chunked sending to avoid message size limits. See b/62115660.
    let position = 0;
    do {
      const length = Math.min(fileData.byteLength - position, MAX_PAYLOAD_SIZE);
      const chunk = new Uint8Array(fileData, position, length);
      position += length;

      const base64 = btoa(String.fromCharCode.apply(null, chunk));
      yield {
        response: {
          action: 'append',
          file: file.name,
          data: base64,
        },
      };

      let percentDone = fileData.byteLength === 0 ?
          100 :
          Math.round((position / fileData.byteLength) * 100);
      percent.textContent = `${percentDone}% done`;

    } while (position < fileData.byteLength);
  }

  // All done.
  yield {
    response: {
      action: 'complete',
    }
  };
}

scope.google = scope.google || {};
scope.google.colab = scope.google.colab || {};
scope.google.colab._files = {
  _uploadFiles,
  _uploadFilesContinue,
};
})(self);
</script> 


    Saving willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz to willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz



```
!plink --vcf willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz --make-bed --out willisornis --allow-extra-chr
```

    PLINK v1.90b6.24 64-bit (6 Jun 2021)           www.cog-genomics.org/plink/1.9/
    (C) 2005-2021 Shaun Purcell, Christopher Chang   GNU General Public License v3
    Logging to willisornis.log.
    Options in effect:
      --allow-extra-chr
      --make-bed
      --out willisornis
      --vcf willisornis.snps.biallelic.filtered.PASS.20samples.chr24_25.vcf.gz
    
    12977 MB RAM detected; reserving 6488 MB for main workspace.
    --vcf: willisornis-temporary.bed + willisornis-temporary.bim +
    willisornis-temporary.fam written.
    1855451 variants loaded from .bim file.
    20 people (0 males, 0 females, 20 ambiguous) loaded from .fam.
    Ambiguous sex IDs written to willisornis.nosex .
    Using 1 thread (no multithreaded calculations invoked).
    Before main variant filters, 20 founders and 0 nonfounders present.
    Calculating allele frequencies... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99% done.
    Total genotyping rate is 0.999966.
    1855451 variants and 20 people pass filters and QC.
    Note: No phenotypes present.
    --make-bed to willisornis.bed + willisornis.bim + willisornis.fam ... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99%done.



```
!plink --bfile willisornis --indep-pairwise 50 5 0.2 --allow-extra-chr --out willisornis_ld
```

    PLINK v1.90b6.24 64-bit (6 Jun 2021)           www.cog-genomics.org/plink/1.9/
    (C) 2005-2021 Shaun Purcell, Christopher Chang   GNU General Public License v3
    Logging to willisornis_ld.log.
    Options in effect:
      --allow-extra-chr
      --bfile willisornis
      --indep-pairwise 50 5 0.2
      --out willisornis_ld
    
    12977 MB RAM detected; reserving 6488 MB for main workspace.
    1855451 variants loaded from .bim file.
    20 people (0 males, 0 females, 20 ambiguous) loaded from .fam.
    Ambiguous sex IDs written to willisornis_ld.nosex .
    Using 1 thread (no multithreaded calculations invoked).
    Before main variant filters, 20 founders and 0 nonfounders present.
    Calculating allele frequencies... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99% done.
    Total genotyping rate is 0.999966.
    1855451 variants and 20 people pass filters and QC.
    Note: No phenotypes present.
    Pruned 879020 variants from chromosome 27, leaving 130226.
    Pruned 736048 variants from chromosome 28, leaving 110157.
    Pruning complete.  1615068 of 1855451 variants removed.
    Marker lists written to willisornis_ld.prune.in and willisornis_ld.prune.out .



```
!plink --bfile willisornis --extract willisornis_ld.prune.in --make-bed --out willisornis_ld_final --allow-extra-chr
```

    PLINK v1.90b6.24 64-bit (6 Jun 2021)           www.cog-genomics.org/plink/1.9/
    (C) 2005-2021 Shaun Purcell, Christopher Chang   GNU General Public License v3
    Logging to willisornis_ld_final.log.
    Options in effect:
      --allow-extra-chr
      --bfile willisornis
      --extract willisornis_ld.prune.in
      --make-bed
      --out willisornis_ld_final
    
    12977 MB RAM detected; reserving 6488 MB for main workspace.
    1855451 variants loaded from .bim file.
    20 people (0 males, 0 females, 20 ambiguous) loaded from .fam.
    Ambiguous sex IDs written to willisornis_ld_final.nosex .
    --extract: 1855451 variants remaining.
    Warning: At least 240382 duplicate IDs in --extract file.
    Using 1 thread (no multithreaded calculations invoked).
    Before main variant filters, 20 founders and 0 nonfounders present.
    Calculating allele frequencies... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99% done.
    Total genotyping rate is 0.999966.
    1855451 variants and 20 people pass filters and QC.
    Note: No phenotypes present.
    --make-bed to willisornis_ld_final.bed + willisornis_ld_final.bim +
    willisornis_ld_final.fam ... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99%done.



```
!plink --bfile willisornis_ld_final --recode vcf --out willisornis_ld_final --allow-extra-chr
```

    PLINK v1.90b6.24 64-bit (6 Jun 2021)           www.cog-genomics.org/plink/1.9/
    (C) 2005-2021 Shaun Purcell, Christopher Chang   GNU General Public License v3
    Logging to willisornis_ld_final.log.
    Options in effect:
      --allow-extra-chr
      --bfile willisornis_ld_final
      --out willisornis_ld_final
      --recode vcf
    
    12977 MB RAM detected; reserving 6488 MB for main workspace.
    1855451 variants loaded from .bim file.
    20 people (0 males, 0 females, 20 ambiguous) loaded from .fam.
    Ambiguous sex IDs written to willisornis_ld_final.nosex .
    Using 1 thread (no multithreaded calculations invoked).
    Before main variant filters, 20 founders and 0 nonfounders present.
    Calculating allele frequencies... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99% done.
    Total genotyping rate is 0.999966.
    1855451 variants and 20 people pass filters and QC.
    Note: No phenotypes present.
    --recode vcf to willisornis_ld_final.vcf ... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99%done.


---


```

```


```
!plink --bfile willisornis_ld_final --pca 10 --out pca_result --allow-extra-chr
```

    PLINK v1.90b6.24 64-bit (6 Jun 2021)           www.cog-genomics.org/plink/1.9/
    (C) 2005-2021 Shaun Purcell, Christopher Chang   GNU General Public License v3
    Logging to pca_result.log.
    Options in effect:
      --allow-extra-chr
      --bfile willisornis_ld_final
      --out pca_result
      --pca 10
    
    12977 MB RAM detected; reserving 6488 MB for main workspace.
    1855451 variants loaded from .bim file.
    20 people (0 males, 0 females, 20 ambiguous) loaded from .fam.
    Ambiguous sex IDs written to pca_result.nosex .
    Using up to 2 threads (change this with --threads).
    Before main variant filters, 20 founders and 0 nonfounders present.
    Calculating allele frequencies... 0%1%2%3%4%5%6%7%8%9%10%11%12%13%14%15%16%17%18%19%20%21%22%23%24%25%26%27%28%29%30%31%32%33%34%35%36%37%38%39%40%41%42%43%44%45%46%47%48%49%50%51%52%53%54%55%56%57%58%59%60%61%62%63%64%65%66%67%68%69%70%71%72%73%74%75%76%77%78%79%80%81%82%83%84%85%86%87%88%89%90%91%92%93%94%95%96%97%98%99% done.
    Total genotyping rate is 0.999966.
    1855451 variants and 20 people pass filters and QC.
    Note: No phenotypes present.
    Relationship matrix calculation complete.
    --pca: Results saved to pca_result.eigenval and pca_result.eigenvec .



```
import pandas as pd
import matplotlib.pyplot as plt

# Load eigenvectors
eigenvec = pd.read_csv("pca_result.eigenvec", delim_whitespace=True, header=None)
eigenvec.columns = ['FID', 'IID'] + [f'PC{i}' for i in range(1, eigenvec.shape[1] - 1)]

# Plot PC1 vs PC2
plt.figure(figsize=(8,6))
plt.scatter(eigenvec['PC1'], eigenvec['PC2'], c='steelblue', edgecolor='k')
plt.xlabel('PC1')
plt.ylabel('PC2')
plt.title('PCA of Genomic Data')
plt.grid(True)
plt.show()
```

    <i-input-63-607bf9d2f42b>:5: FutureWarning: The 'delim_whitespace' keyword in pd.read_csv is deprecated and will be removed in a future version. Use ``sep='\s+'`` instead
      eigenvec = pd.read_csv("pca_result.eigenvec", delim_whitespace=True, header=None)



    
![png](PCA_files/PCA_12_1.png)
    

