---
layout: single
title: "📘 Comandos Básicos de UNIX – Para Iniciantes"
permalink: /pages/tutorial_unix_1/
---

Este é um guia introdutório para quem está começando a usar o terminal em sistemas baseados em UNIX, como Linux e macOS. Aqui você vai aprender os comandos mais importantes para manipulação de arquivos e pastas, permissões e redirecionamentos.

---

## 🧭 Navegação entre diretórios

| Comando            | Descrição                                                  | Observações                                                             |
|--------------------|------------------------------------------------------------|-------------------------------------------------------------------------|
| `pwd`              | Mostra o caminho completo do diretório atual               |                                                                         |
| `ls`               | Lista os arquivos e pastas do diretório atual              |                                                                         |
| `ls -l`            | Lista com detalhes (permissões, dono, tamanho etc.)        |                                                                         |
| `ls -a`            | Lista todos os arquivos, inclusive os ocultos              |                                                                         |
| `cd nome_da_pasta` | Entra em uma pasta                                         | Pode ser caminho relativo ou absoluto                                  |
| `cd ..`            | Volta para o diretório anterior                            | Pode ser acumulativo. Ex: `cd ../../../`                               |
| `cd /`             | Vai para o diretório raiz                                  |                                                                         |
| `cd ~`             | Vai para a pasta pessoal do usuário                        |                                                                         |

---

## 📂 Manipulação de diretórios (pastas)

| Comando                  | Descrição                                  |
|--------------------------|--------------------------------------------|
| `mkdir nome_da_pasta`    | Cria uma nova pasta                        |
| `rmdir nome_da_pasta`    | Remove uma pasta vazia                     |
| `rm -r nome_da_pasta`    | Remove uma pasta e todo o seu conteúdo     |

---

## 📄 Manipulação de arquivos

| Comando                    | Descrição                                              | Observações                                        |
|----------------------------|--------------------------------------------------------|---------------------------------------------------|
| `touch nome_do_arquivo`    | Cria um arquivo vazio                                  |                                                   |
| `nano nome_do_arquivo`     | Abre o editor de texto `nano` para editar o arquivo    | Para salvar: `Ctrl + O`, sair: `Ctrl + X`        |
| `cat nome_do_arquivo`      | Mostra o conteúdo do arquivo                           |                                                   |
| `cp origem destino`        | Copia um arquivo ou pasta                              | Para copiar pastas, use `-r`: `cp -r pasta1 pasta2` |
| `mv origem destino`        | Move ou renomeia um arquivo ou pasta                   |                                                   |
| `rm nome_do_arquivo`       | Apaga um arquivo                                       |                                                   |

---

## 🔍 Visualização de conteúdo

| Comando           | Descrição                                             | Observações                                                             |
|-------------------|-------------------------------------------------------|-------------------------------------------------------------------------|
| `cat arquivo`     | Mostra o conteúdo de um arquivo                       |                                                                         |
| `less arquivo`    | Mostra o conteúdo paginado                            | Use `q` para sair                                                       |
| `head arquivo`    | Mostra as primeiras linhas de um arquivo              | Pode usar `-n`: `head -10 arquivo`                                     |
| `tail arquivo`    | Mostra as últimas linhas de um arquivo                | Pode usar `-n`: `tail -10 arquivo`                                     |
| `tail -f arquivo` | Mostra as últimas linhas com atualização em tempo real| Útil para logs em tempo real                                           |

---

## 🔐 Permissões de arquivos e pastas

| Comando        | Descrição                                             |
|----------------|-------------------------------------------------------|
| `ls -l`        | Mostra as permissões dos arquivos                     |
| `chmod`        | Altera as permissões de arquivos ou pastas            |
| `chown`        | Altera o dono de arquivos ou pastas                   |

### 📌 Exemplos úteis:
- `chmod +x script.sh` — Dá permissão de execução
- `chmod 644 arquivo.txt` — Permissões comuns para arquivos de texto
- `chown usuario:grupo arquivo.txt` — Altera proprietário e grupo

---

## ➡️ Redirecionamentos e Pipes

| Símbolo | Descrição                                                  |
|---------|------------------------------------------------------------|
| `>`     | Redireciona saída para sobrescrever um arquivo             |
| `>>`    | Redireciona saída e adiciona ao final do arquivo           |
| `<`     | Lê entrada a partir de um arquivo                          |
| `|`     | Conecta a saída de um comando na entrada de outro (pipe)   |

### 📌 Exemplos:
- `echo "Olá" > teste.txt` — Cria ou sobrescreve o conteúdo
- `echo "Mais uma linha" >> teste.txt` — Adiciona sem apagar
- `cat teste.txt | grep "linha"` — Filtra linhas que contêm "linha"

---

## 🧰 Comandos úteis

| Comando       | Descrição                                                      |
|---------------|----------------------------------------------------------------|
| `clear`       | Limpa a tela do terminal                                       |
| `history`     | Mostra os comandos digitados anteriormente                     |
| `man comando` | Mostra o manual de um comando (ex: `man ls`)                   |
| `which comando` | Mostra o caminho do comando executável (ex: `which python`) |
| `exit`        | Sai do terminal                                                |

