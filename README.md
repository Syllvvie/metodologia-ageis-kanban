# Trabalho B1-T3 — Implementação de Kanban para um Time de Desenvolvimento Web

**Faculdade UNIALFA** — Sistemas para Internet · Metodologias Ágeis
Umuarama, 2026

## Equipe

| Integrante | RA |
|---|---|
| João Vitor Paiva Borges | 250256 |
| James Soares Silva | 250380 |
| Thiago José da Silva Braz | 250300 |
| Henrique Curioni Esteves | 240016 |
| Gabriel Priori de Morais | 250313 |
| Giovanni Bernandi Rodrigues | 250394 |

📄 Trello: [`Trello`](https://trello.com/invite/b/6aa08b271beb4e0c3d310dd5/ATTIaa016d3ad0b4ce422af19fb792c433f405DBB446/kanban)

---

## Sumário

1. [Contexto do produto e do serviço observado](#1-contexto-do-produto-e-do-serviço-observado)
2. [Quadro Kanban](#2-quadro-kanban)
3. [Mapeamento textual do fluxo](#3-mapeamento-textual-do-fluxo)
4. [Políticas explícitas](#4-políticas-explícitas)
5. [Justificativa das principais decisões](#5-justificativa-das-principais-decisões)
6. [Simulação do fluxo](#6-simulação-do-fluxo)
7. [Melhoria baseada em evidências](#7-melhoria-baseada-em-evidências)

---

## 1. Contexto do produto e do serviço observado

**Produto:** plataforma de e-commerce e marketplace para compra e venda de roupas, calçados e acessórios de segunda mão (definido no trabalho B1-T2).

**Serviço observado:** o fluxo de trabalho do time de desenvolvimento web responsável por evoluir a plataforma. As demandas chegam ao time de três formas: novas funcionalidades priorizadas no Product Backlog (B1-T2), defeitos (bugs) reportados em produção e melhorias técnicas identificadas pelo próprio time.

**Situação atual:** parte dos itens depende de terceiros para avançar (por exemplo, integração com a API de frete dos Correios/transportadoras) e outros aguardam aprovação de áreas internas (como infraestrutura), o que gera bloqueios temporários que precisam ficar visíveis no fluxo de trabalho.

---

## 2. Quadro Kanban

Tipos de trabalho: 🟢 **Feature** (nova funcionalidade) · 🟠 **Bug** (defeito) · 🟡 **Melhoria técnica** (refatoração/otimização interna)
🔴 **Bloqueado** = dependência externa impede o avanço do item.

### Estado inicial da simulação (T0)

| Backlog priorizado<br>(sem limite) | Em análise<br>(WIP: 3) | Em desenvolvimento<br>(WIP: 4) | Em revisão<br>(WIP: 3) | Em testes<br>(WIP: 3) | Pronto p/ deploy<br>(WIP: 2) | Entregue<br>(sem limite) |
|---|---|---|---|---|---|---|
| 🟢 Painel do vendedor | 🔴🟢 Cálculo de frete *(bloqueado: API dos Correios)* | 🟢 Carrinho e checkout | 🟢 Chat comprador-vendedor | 🟢 Busca e filtros | — | 🟢 Anúncio de produtos |
| 🟡 Otimização de upload de imagens | 🟠 Notificação de favoritos falha | 🟠 Cupom aplicado em duplicidade | | | | 🟢 Autenticação e perfil |
| 🟢 Rastreador de envio *(depende de: Cálculo de frete)* | | | | | | 🔴🟡 Migração do BD de avaliações *(bloqueado: aprovação da infra)* |
| 🟢 Avaliação de vendedores | | | | | | |

**Ponto de compromisso:** entrada em *Em desenvolvimento* (o time assume o item).
**Ponto de entrega:** entrada em *Entregue* (funcionalidade em produção, acessível ao usuário final).

---

## 3. Mapeamento textual do fluxo

1. **Solicitação** — item entra no Backlog priorizado, oriundo do Product Backlog do B1-T2, de um bug reportado ou de uma melhoria técnica identificada pelo time.
2. **Análise/Refinamento** — o time detalha critérios de aceitação, mapeia dependências e confirma a prioridade.
3. **Desenvolvimento (ponto de compromisso)** — o time assume a implementação do item.
4. **Revisão de código** — o código é revisado por outro membro do time via pull request.
5. **Testes/QA** — validação funcional do item antes do deploy.
6. **Pronto para deploy** — item aprovado, aguardando janela de publicação.
7. **Entregue (ponto de entrega)** — item publicado em produção e acessível ao usuário final.

---

## 4. Políticas explícitas

| Política | Descrição |
|---|---|
| **Entrada no fluxo comprometido** | Um item só sai do Backlog para Em análise quando a história de usuário estiver completa, com critérios de aceitação definidos e prioridade confirmada pelo Product Owner. |
| **Início do desenvolvimento (ponto de compromisso)** | O item avança de Em análise para Em desenvolvimento somente quando o refinamento estiver concluído, as dependências externas estiverem mapeadas e houver vaga dentro do limite de WIP da coluna. |
| **Avanço para revisão e testes** | O código só avança para Em revisão quando houver pull request aberto, build sem falhas e testes unitários mínimos escritos; só avança para Em testes após aprovação de ao menos um revisor. |
| **Tratamento de bloqueio** | Um item bloqueado recebe a etiqueta "Bloqueado" com o motivo visível no cartão. Ele não conta para o limite de WIP da coluna, mas o time prioriza o desbloqueio antes de puxar novos itens. |
| **Sistema puxado** | Nenhum item é empurrado para uma coluna: um novo item só é puxado quando existe capacidade livre conforme o limite de WIP definido para aquela etapa. |
| **Condição de entrega (ponto de entrega)** | Um item só é considerado Entregue após o deploy em produção e a confirmação de que a funcionalidade está acessível e funcional para o usuário final. |

---

## 5. Justificativa das principais decisões

**Colunas do fluxo** — foram desenhadas para refletir o processo real de um time de desenvolvimento web, incluindo as etapas de revisão de código e testes, que normalmente ficam invisíveis quando o fluxo é resumido apenas a "a fazer / fazendo / feito". Tornar essas etapas explícitas ajuda o time a enxergar onde o trabalho realmente se acumula.

**Limites de WIP** — definidos considerando uma equipe de cinco integrantes: WIP mais alto em Desenvolvimento (4), por ser a etapa que concentra a maior parte do esforço da equipe, e limites menores nas etapas de Revisão e Deploy, que dependem de disponibilidade pontual de revisores e de janelas de publicação.

**Bloqueios e dependências** — manter os itens bloqueados visíveis dentro da própria coluna (em vez de escondê-los ou removê-los do quadro) segue o princípio de que o quadro deve representar a realidade do processo, incluindo os pontos de espera. Isso evita a falsa sensação de progresso e prioriza o desbloqueio antes de novas demandas.

**Sistema puxado** — a política de puxar itens apenas quando há capacidade livre evita sobrecarga do time e mantém o fluxo mais previsível, coerente com o objetivo de reduzir multitarefa e melhorar a entrega contínua de valor para compradores e vendedores da plataforma.

---

## 6. Simulação do fluxo

A simulação foi conduzida em rodadas curtas, cada uma representando um período de trabalho da equipe. Papéis distribuídos: um integrante conduziu as rodadas, um movimentou os cartões, um registrou os eventos e os demais analisaram o fluxo e aplicaram as políticas definidas na seção 4.

| Rodada | Eventos e movimentações | Observação da equipe |
|---|---|---|
| **T0** (estado inicial) | Quadro no estado apresentado na seção 2, com "Cálculo de frete" já bloqueado por dependência da API dos Correios. | Ponto de partida da simulação. |
| **T1** | "Busca e filtros" passa nos testes e avança para Pronto para deploy. "Chat comprador-vendedor" conclui revisão e avança para Em testes. Como abriu vaga em Em desenvolvimento, o time puxa o bug "Notificação de favoritos falha" de Em análise. Como abriu vaga em Em análise, o time puxa "Painel do vendedor" do Backlog. "Cálculo de frete" segue bloqueado. | Sistema puxado funcionando: nada foi empurrado, só puxado por vaga livre. |
| **T2** | Evento: demanda com data fixa entra no sistema ("selo de desconto Black Friday"), classificada com prioridade máxima no Backlog. Evento: o revisor principal (Henrique) fica temporariamente indisponível. Deploy é realizado: "Busca e filtros" vai para Entregue. "Carrinho e checkout" conclui o desenvolvimento, mas não há revisor disponível para assumir o item — ele aguarda dentro da própria etapa mesmo com vaga livre em Em revisão (0/3). | Primeiro sinal de gargalo: WIP livre, mas capacidade humana insuficiente. |
| **T3** | "Carrinho e checkout" completa uma rodada inteira aguardando revisor (3ª rodada seguida com "Cálculo de frete" também bloqueado). A fila antes de Em revisão fica evidente para a equipe. | Gargalo confirmado ao longo de mais de uma rodada, não é apenas fila isolada. |
| **T4** (com melhoria) | Aplicada a política experimental de revisor backup: Gabriel assume a revisão de "Carrinho e checkout" no lugar de Henrique. O item avança para Em testes na mesma rodada em que fica disponível. | Fila antes de Em revisão se dissolve. |

### Gargalo identificado

Ao longo das rodadas T2 e T3, o item "Carrinho e checkout" permaneceu aguardando mesmo com vaga livre no limite de WIP de Em revisão (0/3). O indício não foi uma fila isolada, mas um acúmulo que se repetiu por mais de uma rodada e esteve associado à indisponibilidade de uma única pessoa (o revisor principal) — um sinal característico de gargalo por dependência de recurso único, e não por excesso de WIP.

### Lead time e cycle time observados

Tomando o cartão "Carrinho e checkout" como referência, o ponto de compromisso ocorreu em T0 (já em Em desenvolvimento) e a entrada efetiva em Em revisão só ocorreu em T4, após a aplicação da melhoria. O cycle time observado até a revisão foi de quatro rodadas, das quais duas rodadas (T2 e T3) foram de espera pura por disponibilidade de revisor, evidenciando como um gargalo humano — não representado pelo limite numérico de WIP — impacta diretamente o tempo de entrega.

---

## 7. Melhoria baseada em evidências

A partir do gargalo identificado na simulação, a equipe formulou e testou uma hipótese de melhoria, registrada a seguir.

| Aspecto | Registro da equipe |
|---|---|
| **Problema observado** | Mesmo com o limite de WIP de Em revisão livre (0/3), um item concluído no desenvolvimento ficou parado por uma rodada inteira porque o único revisor disponível estava temporariamente indisponível. |
| **Evidência** | "Carrinho e checkout" terminou o desenvolvimento em T2 e só entrou efetivamente em revisão em T4, mesmo com vaga de WIP livre durante todo esse período (T2 e T3). |
| **Hipótese** | Definir um revisor backup evita que a indisponibilidade de uma única pessoa pare o fluxo, mesmo quando o limite numérico de WIP permitiria avançar. |
| **Experimento** | Adição da política: "quando o revisor principal estiver indisponível, outro integrante com conhecimento no módulo assume a revisão do item". Gabriel revisou o item em T4 no lugar de Henrique. |
| **Resultado** | O item avançou imediatamente após a revisão alternativa; o tempo de espera antes de Em revisão caiu de uma rodada inteira para zero na rodada seguinte. |
| **Próximo passo** | Formalizar a política de revisor backup no quadro oficial e observar, em rodadas futuras, se distribuir revisões entre mais integrantes reduz picos de espera sem comprometer a qualidade da revisão. |

---

## Estrutura do repositório

```
.
├── README.md                          # este arquivo — trabalho completo
```
