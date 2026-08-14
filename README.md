# RailOps — Documentação

Documentação técnica e de produto do RailOps, sistema web para digitalizar a
passagem de serviço na operação ferroviária do Pátio Brisamar e do Terminal
TECON.

> **Status:** Fase 9 — implementação em andamento. Os épicos de autenticação,
> Brisamar e TECON estão implementados. O próximo incremento planejado é o
> Épico 4, edição e histórico.

O código da aplicação está no repositório
[railops-app](https://github.com/ChoqueanoIV/railops-app).

## Conteúdo do repositório

| Documento | Conteúdo |
|---|---|
| [00-checkpoint-continuidade.md](00-checkpoint-continuidade.md) | Estado atual, decisões e instruções para retomar o projeto |
| [01-requisitos.md](01-requisitos.md) | Requisitos funcionais e não funcionais |
| [02-casos-de-uso.md](02-casos-de-uso.md) | Casos de uso do sistema |
| [03-regras-de-negocio.md](03-regras-de-negocio.md) | Regras de negócio ferroviárias |
| [05-modelagem-banco.md](05-modelagem-banco.md) | Modelagem do banco de dados |
| [06-prototipos.md](06-prototipos.md) | Protótipos das interfaces |
| [07-planejamento-branches.md](07-planejamento-branches.md) | Estratégia de branches e entregas |
| [08-backlog.md](08-backlog.md) | Épicos e tarefas da implementação |
| [adrs/](adrs/) | Registros das decisões de arquitetura |
| [docs/anexos/](docs/anexos/) | Formulários e diagramas operacionais de referência |

## Decisões de arquitetura

- [ADR-001 — Versionamento e histórico](adrs/001-versionamento-historico.md)
- [ADR-002 — Estratégia de autenticação](adrs/002-estrategia-autenticacao.md)
- [ADR-003 — Arquitetura em camadas](adrs/003-arquitetura-em-camadas.md)
- [ADR-004 — Escolha do ORM](adrs/004-escolha-orm.md)
- [ADR-005 — Especialização das tabelas por terminal](adrs/005-especializacao-tabelas-terminal.md)
- [ADR-006 — Estratégia de branching](adrs/006-estrategia-de-branching.md)

## Progresso

- [x] Entendimento do problema e levantamento de requisitos
- [x] Casos de uso e regras de negócio
- [x] Arquitetura e decisões registradas em ADRs
- [x] Modelagem do banco de dados
- [x] Protótipos de interface
- [x] Planejamento de branches e backlog
- [x] Épico 1 — autenticação
- [x] Épico 2 — passagem do Pátio Brisamar
- [x] Épico 3 — passagem do Terminal TECON
- [ ] Épico 4 — edição e histórico
- [ ] Épico 5 — consulta de passagens
- [ ] Épico 6 — exportação de dados
- [ ] Épico 7 — diagramas de manobras
- [ ] Épico 8 — relatório de falhas por rádio
- [ ] Deploy e documentação final

## Como acompanhar ou retomar

Leia primeiro o [checkpoint de continuidade](00-checkpoint-continuidade.md).
Ele registra o estado técnico, os testes executados, as branches, os Pull
Requests e o próximo passo seguro. O [backlog](08-backlog.md) detalha a ordem e
o escopo dos incrementos restantes.

## Autor

Documentação mantida por [Leandro](https://github.com/ChoqueanoIV) como parte
do projeto de portfólio RailOps.
