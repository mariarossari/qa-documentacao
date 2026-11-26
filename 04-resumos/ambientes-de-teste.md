# Ambientes de teste

Um software passa por etapas antes de chegar ao cliente, e cada uma tem um objetivo. A ordem é: **desenvolvimento → homologação → staging → produção**.

| Ambiente | O que é | O que se testa |
| --- | --- | --- |
| Desenvolvimento (dev) | Onde os desenvolvedores escrevem e alteram o código. Instável por natureza | Testes rápidos feitos pelo próprio dev, como os unitários |
| Homologação (QA) | Ambiente do time de qualidade, estável o bastante para testar de verdade, ainda sem cliente real | Casos de teste, cenários, bugs, regressão e demais testes funcionais |
| Staging (pré-produção) | Cópia quase idêntica da produção, sem clientes reais | Última checagem antes do lançamento: desempenho, integrações, teste de fumaça da versão final |
| Produção (prod) | O sistema real, em uso por clientes | O mínimo e com cuidado: teste de fumaça após o lançamento |

> Algumas empresas juntam ou renomeiam essas etapas. Em entrevista, vale perguntar como é o fluxo de ambientes da equipe.

## Onde o QA atua

A execução dos testes acontece principalmente em **homologação**, mas o QA participa desde o início do ciclo (STLC): na análise dos requisitos, no refinamento do backlog e no planejamento dos testes. Por isso o papel não começa só quando o código fica pronto.

## Por que não testar direto em produção

Em produção estão usuários e dados reais. Um teste mais específico pode:
- alterar ou apagar dados verdadeiros;
- gerar pedidos, cobranças ou e-mails reais;
- sobrecarregar o sistema e derrubar o serviço para todo mundo.

Por isso, em produção o recomendado é só uma verificação geral das funções essenciais (teste de fumaça) depois do lançamento. Os testes mais profundos ficam nos ambientes anteriores.

## Teste alfa e teste beta

São testes de aceitação feitos com usuários antes do lançamento amplo.

| | Alfa | Beta |
| --- | --- | --- |
| Quem testa | Grupo restrito e interno (equipe, funcionários) | Grupo maior de usuários reais, externos |
| Ambiente | Controlado, com a equipe acompanhando de perto | Uso real, com pouco ou nenhum acompanhamento direto |
| Como os problemas chegam | A equipe observa e registra | Os próprios usuários relatam os bugs |

## Cards mais importantes do tema

**Quais são os ambientes de um software, em ordem?**
Desenvolvimento, homologação, staging (pré-produção) e produção.

**Em qual ambiente o QA mais atua?**
Em homologação, onde os testes são executados. Mas participa antes, na análise de requisitos e no planejamento, e também depois, no teste de fumaça em produção.

**O que se testa em cada ambiente?**
Dev: testes rápidos do desenvolvedor. Homologação: casos de teste, cenários e regressão. Staging: desempenho, integrações e a checagem final. Produção: só o essencial, com cuidado.

**Por que não se testa direto em produção?**
Porque há usuários e dados reais: um teste pode alterar dados, gerar cobranças ou derrubar o sistema. Em produção faz-se apenas um teste de fumaça.

**Qual a diferença entre staging e homologação?**
Homologação é onde o QA testa as funcionalidades. Staging é uma cópia fiel da produção, usada para a validação final antes do lançamento. Algumas empresas usam um só ambiente para os dois papéis.

**Qual a diferença entre teste alfa e beta?**
Alfa: grupo restrito e interno, com a equipe acompanhando. Beta: usuários reais, em grupo maior, que relatam os bugs por conta própria.

**O que é teste de fumaça?**
Verificação rápida das funções críticas de uma nova versão, para decidir se vale seguir com testes mais profundos.
