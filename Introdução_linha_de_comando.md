---
layout: single
title: "Introdução a linha de comando"
permalink: /pages/tutorial_unix_2/
---

# 💻 Introdução à linha de comando
A linha de comando (como aquela usada em um terminal) é uma interface de texto usada para interagir com o sistema operacional. Em vez de clicar em ícones ou menus, você digita comandos para executar tarefas como criar pastas, mover arquivos, instalar programas e muito mais. No geral, a estrutura é a mesma que aquela interface gráfica que estamos acostumados, mas acessada de forma diferente.

## ↠ Por que usamos a linha de comando?
Apesar das interfaces gráficas serem mais intuitivas, a linha de comando oferece mais controle, precisão e velocidade, especialmente em tarefas repetitivas ou complexas. Além disso, muitos servidores e ferramentas de programação só estão disponíveis via terminal.

## ↠ Principais vantagens:
* **Eficiência e agilidade**: tarefas podem ser executadas com poucos comandos.
* **Automação**: é possível criar scripts para automatizar processos.
* **Leveza**: ideal para trabalhar em sistemas sem interface gráfica, como servidores Linux.

## ↠ Entendendo Estrutura e Caminhos de Pastas
Antes de usar a linha de comando, é importante entender como as pastas (ou diretórios) e seus arquivos são organizadas no sistema.

Na interface gráfica, navegamos por pastas clicando em ícones. Por exemplo: você abre "Documentos", depois uma pasta chamada "Projetos", e finalmente um arquivo. Visualmente, você vê onde está e pode voltar ou avançar com cliques.

Na linha de comando, essa navegação é feita por caminhos de diretórios, que mostram onde você está no sistema. Por exemplo, o caminho a seguir mostra que você está dentro da pasta “Projetos”, que está dentro de “Documentos”, que está dentro da pasta do seu usuário:

`/home/usuario/Documentos/Projetos`

Aqui, podemos usar o comando `pwd` (print working directory) para mostrar o caminho completo da pasta em que você está no momento.



```python
# Vamos testar o pwd nesse notebook ou no seu terminal.
!pwd
# obs.: Aqui no notebook, quando vamos utilizar um comando que seja nativo de Linux, devemos colocar o "!" antes do comando. Quando estiver no terminal do seu computador,
# isso não será necessário.
```

    /content


Perceba que o pwd não retorna o nosso drive. Então, o primeiro passo é acessarmos o nosso drive a partir do Google Colab. Para isso precisamos executar o seguinte comando:

```
from google.colab import drive
drive.mount('/content/drive')
```

Esse comando conecta seu Google Drive ao Colab, permitindo acessar e salvar arquivos diretamente nele.
O Drive será montado no caminho /content/drive, como se fosse uma pasta do seu computador.


```python
from google.colab import drive
drive.mount('/content/drive')
```

    Mounted at /content/drive


---
## ↠ Navegando por diretórios

Agora que já conseguimos acessar o nosso drive, vamos entrar na pasta do curso, aonde estão todos os dados que usaremos.

O comando que utilizamos para isso é o `cd` (*change directory* ou troca de diretório), ele é usado para navegar entre pastas no sistema de arquivos pela linha de comando.

Por exemplo, `cd Documentos` move você para a pasta chamada "Documentos", como se estivesse clicando nela num explorador de arquivos.


```python
# Vamos mudar de pasta e acessar o diretório do curso.
!cd Congen_ITV

```

    /bin/bash: line 1: cd: curso_popgen: No such file or directory


Quais arquivos temos aqui?

Para ver a estrutura de pastas e arquivos dentro de uma pasta podemos usar os comandos `ls`, `ll -h` e `ls -lhtr`.

O comando `ls` é usado para listar os arquivos e pastas dentro de um diretório. É uma forma simples e rápida de ver o que existe na pasta em que você está. Por padrão, ele mostra apenas os nomes dos arquivos e diretórios, sem muitos detalhes.

Já o comando `ll -h` mostra a mesma lista, mas com mais informações, como permissões de acesso, dono do arquivo, tamanho e data da última modificação. O parâmetro -h significa "human readable", ou seja, exibe os tamanhos dos arquivos em formatos mais fáceis de entender, como KB, MB ou GB (em vez de apenas bytes).

O comando `ls -lhtr` é parecido com o anterior, mas adiciona mais dois parâmetros: -t, que ordena os arquivos pela data de modificação (do mais recente para o mais antigo), e -r, que inverte essa ordem, mostrando os arquivos mais antigos primeiro. Isso é útil, por exemplo, para ver quais arquivos estão na pasta há mais tempo.

**Vamos olhar quais arquivos estão dentro da pasta e o tamanho deles? Como você faria? Digite na linha de comando abaixo!**



```python
# Digite aqui o comando para ver os arquivos e seus tamanho. Não esqueça do "!" antes do comando!
```

Agora que estamos na pasta do curso, vamos começar a criar as pastas que usaremos para guardar o resultado de cada tipo de análise que usaremos.

Quando queremos criar uma nova pasta na linha de comando, nós utilizamos o comando `mkdir`, que significa "*make directory*" ou criar diretório.

Por exemplo, `mkdir mapeamento` cria uma nova pasta, com o nome de mapeamento, onde podemos guardar os resultados do mapeamento que faremos durante o curso.


```python
# Vamos criar a pasta que usamos de exemplo acima.
!mkdir resultados_analises
```

**# Agora é a sua vez!! Crie as pastas para o restante das análises. Cada uma delas devem ser nomeada da seguinte forma: mapeamento, pca, psmc, qc_filtragem, roh, snp_calling, admixture e ref.**


```python
# Siga o exemplo:
!mkdir mapeamento
```


```python

```

A pasta `ref` será usada para praticarmos alguns comandos básicos. Mais adiante, ela será excluída, então não se preocupe com o conteúdo dela a longo prazo.

Vamos começar copiando um arquivo `.fasta` que está na pasta `Congen_ITV/reference` para dentro da pasta `ref`, que acabamos de criar.

Antes de fazer isso, precisamos entender como indicar corretamente o caminho do arquivo de origem. Como estamos atualmente dentro da pasta `ref`, e a pasta `reference` está em outro local na estrutura de diretórios, precisamos navegar “de volta” até ela.

Sabemos que:

* O caminho para a pasta reference é: `Congen_ITV/reference`

* Já a pasta ref está em: `Congen_ITV/analises/ref`.

Como a pasta `reference` está duas pastas acima da atual, podemos usar o comando especial `../` para voltar:

* `../` significa “voltar uma pasta”

* `../../` volta duas pastas

Assim, em vez de digitar o caminho completo, podemos usar o caminho relativo da pasta reference estando dentro da pasta ref, que seria:
* `../../reference`.

**Vamos ver o que há dentro da pasta `../../reference`.**



```python
!ls ../../reference
```

Agora que vimos os arquivos que tem ali dentro, vamos copiar o arquivo `.fasta` para  apasta que estamos, a `ref`.

Para isso vamos utilizar o comando `cp`. Ele é utilizado para **copiar** pastas e arquivos de um local para outro, bem similar ao que acontece com o `CTRL+C` e `CTRL+V`.


```python
!cp ../../reference/bWildVid.reduced.fasta ./
```

Note que usamos a expressão `./` para indicar o destino da cópia. Isso acontece porque queremos copiar o arquivo para a pasta em que estamos no momento, e `./` representa exatamente o diretório atual.

**Será que conseguimos copiar o arquivo direitinho? Vamos conferir e também ver qual é o tamanho dele. Como podemos fazer isso?**


```python
# Utilize esse bloco para verificar se o arquivo foi copiado corretamente e seu tamanho.
```

---
## ↠ Visualização de arquivos

Agora vamos dar uma olhada no conteúdo do arquivo `.fasta` que copiamos.
Se estivéssemos usando uma interface gráfica, poderíamos abrir esse arquivo em um visualizador de texto ou em um programa de bioinformática, como um editor de sequências.
Mas, como estamos na linha de comando, usamos outras ferramentas para isso.

Existem comandos que permitem visualizar o conteúdo de arquivos diretamente no terminal, e dois dos mais comuns são `cat` e `less`.

O comando `cat` (concatenate) exibe todo o conteúdo do arquivo de uma vez só na tela. Isso é útil para arquivos pequenos, mas se o arquivo for muito grande, pode travar o terminal ou dificultar a visualização.

O comando `less`, por outro lado, é mais indicado para arquivos grandes. Ele permite rolar o conteúdo para cima e para baixo usando as setas do teclado, tornando a leitura mais prática. E para sair dele, usamos a letra "q".

Exemplo:
*  `cat bWildVid.reduced.fasta`
*  `less bWildVid.reduced.fasta`

Apesar de `cat` e `less` serem úteis para visualizar arquivos, eles não são ideais para arquivos muito grandes — especialmente os comuns em bioinformática, como arquivos `.fasta`, `.fastq` ou `.vcf`. Nesses casos, pode ser mais prático visualizar apenas o início ou o final do arquivo.

Para isso, usamos os comandos `head` e `tail`.

* `head` mostra as primeiras linhas de um arquivo.

* `tail` mostra as últimas linhas.

Por padrão, ambos exibem as primeiras ou últimas 10 linhas, mas você pode usar o parâmetro `-n` para definir quantas linhas deseja visualizar.

Vamos testar!


```python
# Vamos começar pelo head:

!head bWildVid.reduced.fasta
```


```python
# Agora vamos ver as últimas linhas:

!tail bWildVid.reduced.fasta
```


```python
# Agora é sua vez, precisamos ver as primeiras 5 e as primeiras 15 linhas do fasta. Como você faria isso?
```

---
## ↠ Filtragem e manipulação de arquivos

Agora que já entendemos como os comandos `head` e `tail` funcionam, é importante notar que eles são ótimos para dar uma visão geral do início ou do fim de um arquivo — mas não servem tão bem quando queremos encontrar informações específicas.

Por exemplo: imagine que queremos saber apenas o nome da sequência presente em um arquivo `.fasta`. Como fazer isso de forma rápida e direta?

É aí que entra o comando `grep`.

O `grep` é uma ferramenta poderosa para buscar padrões de texto dentro de arquivos. Ele é muito usado em bioinformática para localizar, filtrar ou contar informações em grandes arquivos de texto, como `.fasta`, `.fastq`, `.vcf`, entre outros.

No caso dos arquivos `.fasta`, as linhas que contêm os nomes das sequências geralmente começam com o caractere ">" (maior que). Podemos usar isso a nosso favor com `grep`.

Por exemplo:

* `grep ">" arquivo.fasta` mostra todas as linhas que apresentam `">"`, ou seja, o nome das sequências.

Podemos também querer contar quantas sequências tem no arquivo, para isso usamos o:

* `grep -c ">" arquivo.fasta`, aonde o `-c` indica que queremos contar quantas linhas tem `">"`.

**Vamos testar:**

Use o `grep` para contar quantas sequências estão presentes no arquivo genoma.fasta, e depois liste os nomes delas.


```python
# Use essa célula para isso.
```

Agora que usamos o `grep` para localizar os nomes das sequências no nosso arquivo `.fasta`, podemos querer ir além da simples visualização.

Por exemplo:

* E se quisermos extrair só o identificador (sem o símbolo ">")?

* Ou alterar algo nesses identificadores, como substituir um termo ou adicionar um prefixo?

Para isso, vamos usar dois comandos muito úteis: `awk` e `sed`.

O `awk` é uma ferramenta da linha de comando usada principalmente para ler arquivos linha por linha e trabalhar com colunas de texto. Ele quebra cada linha em partes (campos), normalmente usando espaço ou tabulação como separador, o que permite imprimir só as colunas que você quiser, ou aplicar condições e cálculos. Ele é utilizado principalmente para manipular arquivos tabulares como `.tsv`, `.xls` e `.vcf`.

Quando o `awk` lê uma linha, ele divide automaticamente essa linha em campos (também chamados de "colunas"), usando como separador o espaço em branco (ou tabulação, dependendo do caso).

Cada campo pode ser acessado com `$1`, `$2`, `$3`, e assim por diante. Sendo `$1` a primeira palavra da linha, o `$2` a segunda e o `$3` a terceira.

Por exemplo, vamos extrair o nome das sequências do `.fasta`, sem `">"`.


```python
!grep ">" bWildVid.reduced.fasta | awk '{print $1}'
```

Perceba que usamos um pipe (`|`) e demos dois comandos consecutivos.

O símbolo `|` é chamado de pipe (em inglês, “tubo”), e serve para conectar dois comandos, de forma que a saída do primeiro vire a entrada do segundo.

Em outras palavras: em vez de mostrar o resultado do primeiro comando na tela, ele envia esse resultado diretamente para o próximo comando.

No exemplo acima o `grep ">"` seleciona somente as linhas que começam com ">", que são os cabeçalhos das sequências.

Em seguida, o a`wk '{print $1}'` pega apenas a primeira palavra de cada linha, ou seja, o identificador da sequência (antes do primeiro espaço).


Já o `sed` (stream editor) é usado para editar textos de forma automática, diretamente na linha de comando. Ele pode substituir palavras, remover trechos, ou até alterar padrões inteiros em arquivos. Ele faz isso  lendo o arquivo linha por linha, aplicando a regra que você define, e mostrando (ou salvando) o resultado.

Diferente do `awk`, o `sed` não divide automaticamente a linha em colunas ou campos numerados. Ele trabalha com a linha como um todo, e faz substituições com base em padrões de texto.

O sed lê cada linha do arquivo e aplica uma instrução de edição que você define. A forma mais comum de uso é a substituição, com a seguinte estrutura:

```
sed 's/padrao/novo_texto/'
```
onde o padrão é o que você quer encontrar e o novo_texto é pelo o que você quer substituir (s) aquele padrão.

Por exemplo:

Perceba que aqui extraímos o nome do cromossomo como h1.Chr24 e h1.Chr25. E se quisermos retirar o h1 e deixarmos Chr todo maíusculo, como fariámos?

Vamos começar apenas retirando o "h1." com o sed.


```python
!sed 's/h1\.//' bWildVid.reduced.fasta
```

Agora vamos mudar apenas o "Chr" para "CHR".


```python
!sed 's/Chr/CHR/' bWildVid.reduced.fasta
```

Até agora, fizemos as substituições separadamente: primeiro removemos o "h1.", depois trocamos "Chr" por "CHR". Mas também é possível fazer as duas mudanças ao mesmo tempo em um único comando `sed` e salvar um novo arquivo com essas alterações.

O símbolo `>` é chamado de redirecionador de saída. Ele é usado para enviar o resultado de um comando para um arquivo, em vez de mostrar esse resultado na tela (no terminal).

Dica: Caso a intenção seja adicionar algo ao final de uma rquivo sem em sobrescrevê-lo, pode usar `>>` (dois sinais de maior), que anexa o conteúdo em vez de substituir.


```python
!sed 's/h1\.//; s/Chr/CHR/' bWildVid.reduced.fasta > bWildVid.reduced.mod.fasta
```



---
## ↠ Criando um script

Até agora, realizamos cada passo da nossa análise manualmente: buscamos linhas, editamos cabeçalhos, salvamos arquivos… Mas será que existe uma forma de automatizar todos esses processos de uma só vez?

A resposta é: **sim**!

Na bioinformática, é comum automatizar tarefas repetitivas com scripts. Eles podem ser escritos em diversas linguagens, como `Bash`, `Python`, ou `Perl` — e aqui vamos aprender um pouco mais sobre scripts em `Bash`, que são simples e muito úteis para manipulação de arquivos e execução de comandos do sistema.

Um script em `Bash` é um arquivo de texto que contém uma sequência de comandos, exatamente como os que digitamos no terminal. Em vez de executar cada comando manualmente, colocamos todos dentro de um arquivo .sh, e rodamos tudo de uma vez.

Você pode imaginar um script como uma receita: cada linha é uma instrução que o computador vai seguir, passo a passo.

**Para que serve um script em `Bash`?**
* Automatizar tarefas repetitivas (como formatar vários arquivos .fasta)
* Padronizar análises, evitando erros humanos
* Reproduzir pipelines, garantindo que você (ou outra pessoa) possa executar exatamente o mesmo processo depois
* Compartilhar workflows com colegas de equipe

**Como criar um script em `Bash`?**

Um script é apenas um arquivo de texto, mas precisa de um detalhe importante logo no início:

```
#!/bin/bash
```

Essa linha diz ao computador que o script será executado usando o interpretador Bash (o terminal que estamos usando).

Depois disso, basta escrever os comandos normalmente, um por linha, como se você estivesse no terminal.


Por exemplo, vamos refazer os passos anteriores em um script bash. Ou seja, vamos acessar um arquivo fasta, extrair o nome das sequências, remover o "h1.", trocar "Chr" por "CHR" e salvar o resultado.



```python
# Obs.: Vamos usar um pouco de python apenas para salvar o script em bash que vamos criar.
# Criando o script:

script_bash = """#!/bin/bash

ARQUIVO="bWildVid.reduced.fasta" # isso se chama variável, ou seja, o genoma recebeu temporárioamente o nome de ARQUIVO.
SAIDA="bWildVid.reduced.modified.fasta"

mkdir ref_modificado # criamos uma nova pasta com o arquivo fasta novo.

grep ">" $ARQUIVO | sed 's/h1\\.//; s/Chr/CHR/' > $SAIDA

echo "Cabeçalhos processados com sucesso!" # o echo é um comando usado para mostrar mensagens ou o conteúdo de variáveis no terminal.
echo "Resultado salvo em $SAIDA"
"""

# Salvando o script como um arquivo .sh
with open("muda_cabecalho_fasta.sh", "w") as f:
    f.write(script_bash)
```

Agora vamos executar o script que acabamos de criar.

Para isso, usamos o comando bash, que indica ao computador que queremos rodar o script usando o interpretador `Bash` — ou seja, o mesmo ambiente de linha de comando que usamos até agora.

Em seguida, informamos o nome do arquivo .sh que contém as instruções que queremos executar.

Esse comando faz com que o `Bash` leia o arquivo linha por linha e execute cada comando exatamente como se você estivesse digitando no terminal.




```python
!bash muda_cabecalho_fasta.sh
```



---
## ↠ Removendo arquivos

Agora que já temos um novo arquivo .fasta com os cabeçalhos ajustados, não precisamos mais do antigo. Para evitar confusão com versões antigas, é uma boa prática remover arquivos que não serão mais usados.

Mas como fazemos isso na linha de comando?

Usamos o comando `rm`, que significa remove (remover). Ele é usado para apagar arquivos ou pastas.

```
rm nome_do_arquivo.fasta
```
Esse comando apaga o arquivo indicado sem pedir confirmação, então é importante ter certeza antes de usar.

Se quisermos apagar uma pasta e todos os arquivos dentro sem pedir confirmação usamos:

```
rm -rf nome_da_pasta
```

Mas tenha cuidado, uma vez apagado com o `rm`, o arquivo não vai para a lixeira, ele é imediatamente removido. Caso você prefira que seja pedido uma confirmação, adicione o parâmetro `-i` após o `rm`.

```
rm -i nome_do_arquivo
```

Vamos então apagar o arquivo fasta com o cabeçalho antigo? Como você faria isso?
**negrito**




```python
# Utilize esse bloco para isso.
```



---
## ↠ Movendo e renomeando arquivos

Nós podemos precisar mover ou renomear um arquivo, nesse caso, usamos o comando `mv`, que é muito útil para organizar arquivos e atualizar nomes de forma rápida.

Para mover arquivos:

```
mv arquivo_no_local_de_origem novo_destino
```
onde o arquivo arquivo_no_local_de_origem será movido para a pasta novo_destino.

Para renomear arquivos:

```
arquivo_no_local_de_origem novo_destino
```
onde o arquivo antigo.fasta será renomeado para novo.fasta.

**Vamos renomear o fasta que geramos com o cabeçalho modificado? Como você faria?**





```python
# Faça aqui
```
