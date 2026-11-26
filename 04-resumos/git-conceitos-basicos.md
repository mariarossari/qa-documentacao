# Git e GitHub: conceitos básicos

## O que é cada coisa

**Repositório** é a pasta do projeto, junto com todo o histórico do que foi mudando nela.

**Commit** é o registro de um conjunto de alterações, com uma mensagem dizendo o que mudou. Dá para voltar a esse ponto depois.

**Main** é a versão principal e estável do projeto. É a base de tudo.

**Branch** é uma cópia paralela da main, onde dá para mexer sem estragar a versão principal. Quando fica pronta, ela volta para a main.

**Pull request (PR)** é o pedido de "olha o que mudei nesta branch, podem revisar e juntar na main?". Em equipe, alguém revisa e aprova. Em projeto individual, o próprio autor faz o merge.

**Merge** é juntar o que está na branch dentro da main.

**git pull** é outra coisa: baixa para o computador as mudanças mais novas do GitHub. Não confundir com o pull request.

Uma comparação que ajuda: a main é o documento final que a empresa usa. A branch é uma cópia para editar. O commit é salvar com uma nota do que mudou. O PR é mandar a cópia para alguém revisar, e o merge é colar as alterações aprovadas no documento final.

## Comandos principais

| Comando | O que faz |
| --- | --- |
| `git clone` | Baixa um repositório do GitHub para o computador |
| `git status` | Mostra o que mudou e o que está pronto para commit |
| `git add` | Separa os arquivos alterados para o commit |
| `git commit -m "mensagem"` | Registra as alterações no histórico |
| `git push` | Envia os commits para o GitHub |
| `git pull` | Baixa as mudanças do GitHub |
| `git checkout -b nome` | Cria uma branch nova e já entra nela |
| `git diff` | Mostra o que mudou e ainda não foi commitado |
| `git log` | Mostra o histórico de commits |

## Fluxo básico

1. Criar uma branch para a alteração
2. Mudar os arquivos
3. Fazer `git add` e `git commit` com uma mensagem clara
4. Enviar a branch com `git push`
5. Abrir o pull request no GitHub
6. Revisar e fazer o merge na main

## Cards mais importantes do tema

**O que é um commit?**
O registro de um conjunto de alterações, com uma mensagem explicando o que mudou. Fica no histórico e dá para voltar a esse ponto.

**O que é uma branch e por que usar?**
Uma cópia paralela da main para trabalhar sem afetar a versão estável. Depois de pronta, é juntada à main.

**O que é um pull request?**
Um pedido para juntar uma branch na main. Permite que outras pessoas revisem, comentem e aprovem antes do merge.

**Qual a diferença entre pull request e git pull?**
Pull request é o pedido de revisão e junção no GitHub. O `git pull` é o comando que baixa para o computador as mudanças mais recentes do repositório.

**Qual a diferença entre git diff e git log?**
O `git diff` mostra o que mudou e ainda não foi commitado. O `git log` mostra o histórico de commits já feitos.

**O que é merge?**
Juntar as alterações de uma branch em outra, normalmente na main.

**Por que um QA precisa conhecer Git?**
Para versionar a documentação de testes e os scripts de automação, trabalhar em branches, abrir PRs e acompanhar o que mudou entre versões para decidir o que retestar (regressão).
