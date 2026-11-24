# Tipos de Teste

Resumo de estudo: testes funcionais, caixa branca e preta, regressão e testes não funcionais.
Os mapas mentais estão na versão em PDF: [tipos-de-teste.pdf](tipos-de-teste.pdf).

> **Frase-chave:** o teste funcional verifica **o que** o sistema faz (conforme os requisitos); o não funcional verifica **como** ele faz (velocidade, uso, proteção). A regressão garante que uma mudança não quebrou o que já funcionava.

## 1. Testes funcionais

Verificam se cada funcionalidade faz o que deveria, de acordo com os requisitos. O foco é validar as ações do software. Podem ser manuais ou automatizados, normalmente em **caixa preta**, sem se preocupar com o código-fonte.

**Onde atuam:** interface do usuário, APIs e banco de dados, segurança, cliente/servidor.
**Exemplos:** teste de fumaça, unitário e de integração, regressão, aceitação do usuário.

| Aspecto | Funcional | Não funcional |
| --- | --- | --- |
| Pergunta que responde | O que o sistema faz? | Como o sistema faz? |
| Baseado em | Requisitos do cliente | Expectativa do cliente |
| Foco | Cada funcionalidade faz o que deveria | Desempenho, usabilidade, confiabilidade, segurança |
| Execução | Pode ser manual | Difícil de executar manualmente |
| Exemplo | Login com usuário e senha corretos entra no sistema | Quantas pessoas conseguem fazer login ao mesmo tempo |

**Teste de fumaça (smoke test):** verificação rápida das funções mais críticas de uma nova versão, para decidir se vale seguir com testes mais profundos.

## 2. Caixa preta, caixa branca e teste estático

| | Caixa preta (fechada) | Caixa branca (aberta) |
| --- | --- | --- |
| Vê o código? | Não | Sim |
| Base do teste | Entradas e saídas | O interior do sistema |
| Onde aparece | Teste funcional (principalmente) | Análise do código |

**Teste estático:** não executa o sistema. Analisa o código e aponta falhas e melhorias, inclusive com ferramentas automáticas (FindBugs, hoje substituído pelo SpotBugs).

## 3. Exemplos práticos

### Exemplo 1: app de compra de ingresso

**Funcionalidade:** selecionar o setor e a quantidade de ingressos.
**Requisito:** o link abre a página de seleção do setor desejado e da quantidade de ingressos que posso comprar.

| Tipo | Situação | Resultado esperado |
| --- | --- | --- |
| Positivo | Escolher o setor Norte com 2 ingressos e avançar | A página de revisão mostra Setor Norte, quantidade 2 e total igual a 2 × o preço do setor |
| Negativo | Pedir mais ingressos do que o limite permitido | O sistema bloqueia ou avisa |
| Negativo | Escolher um setor esgotado | O sistema bloqueia ou avisa |
| Extra | Conferir ingressos disponíveis e diferença de preço por setor | Disponibilidade e preços corretos por setor |

Dois pontos que fortalecem o exemplo: comparar sempre o **esperado com o obtido**, e incluir um **caso negativo**.

### Exemplo 2: campo de CEP no checkout do Sauce Demo

Site de treino: saucedemo.com. Mostra a diferença entre cenário e caso de teste.

**Cenário:** validar o campo de CEP no checkout com mais de 8 dígitos.

**Caso de teste**
- Pré-condição: logado com `standard_user` e com 1 item no carrinho
- Passos: 1. Ir ao checkout. 2. Preencher nome e sobrenome. 3. Digitar `123456789` no CEP. 4. Clicar em Continue
- Dado: CEP `123456789`
- Resultado esperado: mensagem de erro de CEP inválido e bloqueio do avanço

**Anotação do teste exploratório (mesmo teste)**

| Campo | Anotação |
| --- | --- |
| Funcionalidade | Checkout, campo de CEP (Postal Code) |
| O que fiz | Informei um CEP com mais de 8 números |
| O que esperava | Mensagem de erro dizendo que o CEP é inválido por ultrapassar 8 números (padrão brasileiro) |
| O que aconteceu | O sistema deixou prosseguir para o pagamento, sem bloqueio |
| Observação | Possível falha de validação. Requisito não confirmado: assumi o padrão brasileiro |

**Exploratório x caso de teste:** no exploratório, você anota o resultado depois de testar. No caso de teste, o resultado esperado é escrito antes, a partir do requisito.

**Boa prática:** como o requisito não estava confirmado, vale confirmar com quem o definiu antes de abrir o defeito.

## 4. Teste de regressão

Execução de testes em uma versão mais recente do software, para garantir que não surgiram novos defeitos em componentes já analisados.

**Estratégias:** rodar todos os casos da versão anterior (mais comum), ou só os afetados pela mudança (mais eficiente, mas pode não cobrir tudo).

### Selecionar x priorizar

| Etapa | Pergunta | O que fazer |
| --- | --- | --- |
| Seleção | Quais testes entram? | Do total existente, escolher alguns para repetir, descartando os obsoletos |
| Priorização | Em que ordem executar? | Ordenar os selecionados, começando pelos mais críticos |

- **Reutilizáveis:** continuam valendo e entram nos próximos ciclos (ex.: "Escolher setor e quantidade").
- **Obsoletos:** não servem mais (ex.: "Pagar com boleto", se o boleto deixou de existir).

### Critérios de priorização

| Critério | Pergunta | Exemplo |
| --- | --- | --- |
| Impacto no negócio | O que dá prejuízo se quebrar? | Pagamento: se falhar, ninguém compra |
| Criticidade | O que não pode falhar de jeito nenhum? | Login, que bloqueia o app todo |
| Uso frequente | O que muita gente usa o tempo todo? | A cor de um botão é só incômodo (criticidade baixa) |

### Exemplo de ordem de prioridade: login

| Prioridade | Teste | Por quê |
| --- | --- | --- |
| 1 | Login com usuário e senha certos | Se quebrar, ninguém usa o sistema |
| 2 | Login com senha errada | Se deixar entrar, é falha grave de segurança |
| 3 | Login com usuário não cadastrado | Mesmo risco de deixar entrar quem não deveria |
| 4 | Logout, depois de entrar com sucesso | Só importa depois que o login funciona |
| 5 | Login com os dois campos vazios | Costuma ser bloqueado por validações simples, com menor impacto |

### Tipos de regressão (do menor para o maior)

| Tipo | Em palavras simples | Exemplo |
| --- | --- | --- |
| Unitária (URT) | Testa só a parte modificada | Corrigiram um bug no login: testa só o login |
| Parcial | Testa as partes afetadas, escolhidas por análise de impacto | Mudaram o carrinho: testa carrinho e checkout |
| Seletiva | Usa um subconjunto dos testes (módulos alterados, dependências, criticidade, histórico de defeitos) | Dos 200 testes, roda os 30 mais ligados à mudança |
| Regional (RRT) | Testa a área modificada e as vizinhas que podem ter sido afetadas | Corrigiram o módulo B, mas A e C dependem dele: testa B, A e C |
| Completa (FRT) | Testa o aplicativo inteiro, normalmente antes do lançamento | Antes da versão final, repete tudo |

### Reteste x regressão

| | Reteste | Regressão |
| --- | --- | --- |
| Objetivo | Confirmar que o defeito foi corrigido | Garantir que o novo código não afetou outras partes |
| O que testa | O bug de novo | O aplicativo após uma alteração de código |
| Resultado | Não corrigido: defeito reaberto. Corrigido: defeito fechado | Se algo que funcionava quebrou, há novo defeito |

## 5. Testes não funcionais

Verificam aspectos como desempenho, usabilidade e confiabilidade. Não é o que o sistema faz, mas **como** faz. Baseiam-se na expectativa do cliente e são difíceis de fazer manualmente.

| Teste | O que valida | Exemplo |
| --- | --- | --- |
| Desempenho | Tempo de resposta de certas operações, comparado ao tempo esperado pelo cliente (ponta a ponta) | Quantas pessoas fazem login ao mesmo tempo |
| Carga e stress | Carga: muitos usuários ao mesmo tempo. Stress: condições extremas, além do limite, para ver quando quebra | Simular muito mais usuários do que o sistema suporta |
| Usabilidade | O quão agradável e claro é o sistema | O usuário encontra o botão de comprar sem ajuda |
| Segurança | Proteção contra invasão e acesso não autorizado | Tráfego de informações criptografado |
| Portabilidade | Funcionamento em outras plataformas e dispositivos | Navegadores, sistemas e aparelhos diferentes |
| Recuperação de desastres | Como o sistema volta após uma falha grave | Restaurar o serviço após queda do servidor |

**Apache JMeter:** ferramenta para testes de performance, carga e estresse. Dispara milhares de requisições HTTP/HTTPS direto contra o servidor, simulando multidões, sem abrir o navegador. Métricas: tempo de resposta (ms), taxa de erro (%), vazão (throughput) e comportamento sob estresse.

## 6. Cards de revisão para entrevistas de QA

**Qual a diferença entre teste funcional e não funcional?**
Funcional verifica o que o sistema faz, conforme os requisitos. Não funcional verifica como ele faz: velocidade, uso, proteção.

**O que é teste de caixa preta? E de caixa branca?**
Preta: baseado em entradas e saídas, sem olhar o código. Branca: você vê o interior do sistema e o código.

**O que é teste estático?**
Analisa o código sem executar o sistema, apontando falhas e melhorias. Pode ser feito por ferramentas automáticas.

**Qual a diferença entre cenário e caso de teste?**
Cenário é o que será testado, em uma frase. Caso de teste é o passo a passo, com pré-condição, dados e resultado esperado.

**Teste exploratório x caso de teste?**
No exploratório, o resultado é anotado depois de testar. No caso de teste, o esperado é escrito antes, a partir do requisito.

**Como você testaria um login?**
Positivo: usuário e senha certos. Negativos: senha errada, usuário não cadastrado, campos vazios. Depois, o logout.

**Achei um comportamento estranho, mas não sei o requisito. E agora?**
Registrar o que fiz, o esperado e o obtido, marcar como possível falha e confirmar o requisito antes de abrir o defeito.

**O que é teste de regressão?**
Repetir testes depois de uma mudança, para garantir que o que já funcionava não quebrou.

**Qual a diferença entre reteste e regressão?**
Reteste confirma que um bug específico foi corrigido (se não, reabre; se sim, fecha). Regressão verifica se a mudança afetou outras partes.

**Como escolher quais testes entram na regressão?**
Selecionar os reutilizáveis e descartar os obsoletos. Priorizar por impacto no negócio, criticidade e uso frequente.

**Regressão completa, parcial e regional?**
Completa: o app inteiro. Parcial: só as partes afetadas. Regional: a área alterada mais as vizinhas que dependem dela.

**O que é teste de fumaça?**
Verificação rápida das funções críticas de uma nova versão, para decidir se vale seguir com testes mais profundos.

**Desempenho, carga e stress: qual a diferença?**
Desempenho mede tempo de resposta sob uso esperado. Carga testa muitos usuários. Stress leva o sistema além do limite para ver quando quebra.

**O que são testes de usabilidade, segurança e portabilidade?**
Usabilidade: é fácil e claro de usar? Segurança: dados e acesso protegidos? Portabilidade: funciona em outros navegadores, sistemas e aparelhos?

**Dá para fazer teste não funcional manualmente?**
É difícil. Normalmente usam-se ferramentas, como o JMeter, que dispara milhares de requisições simulando multidões.

**O que o JMeter mede?**
Tempo de resposta (ms), taxa de erro (%), vazão (throughput) e comportamento sob estresse.

## Pontos de atenção

- **FindBugs** foi descontinuado; o sucessor é o **SpotBugs**. Vale conhecer os dois nomes.
- **Carga x stress** são diferentes: carga é o comportamento com muitos usuários; stress vai além do limite.
- **Siglas da regressão:** URT = Unit Regression Testing, RRT = Regional Regression Testing, FRT = Full Regression Testing.
