# RailOps — Backlog (Fase 8)

> Documento vivo. Parte da série de documentação técnica do projeto RailOps.

**Status:** Aprovado
**Fase anterior:** Fase 7 — Planejamento de Branches (concluída)
**Próxima fase:** Fase 9 — Implementação

---

## Estratégia de fatiamento

O backlog é organizado em **fatiamento vertical**: cada Épico corresponde
a um Caso de Uso e atravessa todas as camadas da arquitetura (banco,
repository, service, rota, frontend), entregando uma funcionalidade
completa e testável antes de avançar para o próximo Épico. A ordem dos
Épicos respeita as dependências naturais entre eles.

Cada item de tarefa abaixo está pronto para ser transportado como uma
Issue no GitHub (`railops-app`) na Fase 9, seguindo a convenção de branch
definida em `07-planejamento-branches.md`.

---

## Épico 0 — Fundação do Projeto

Pré-requisito técnico para todos os demais épicos; não corresponde a um
Caso de Uso específico.

- [ ] Criar estrutura de pastas do backend (`routers/`, `services/`,
  `repositories/`, `models/`, `core/`) — ADR-003
- [ ] Configurar conexão com PostgreSQL (Supabase) via SQLAlchemy —
  ADR-004
- [ ] Configurar Alembic e gerar a primeira migration (todas as tabelas
  de `05-modelagem-banco.md`)
- [ ] Popular tabela `linha` via seed (linhas fixas do Brisamar e TECON —
  RN07)
- [ ] Configurar endpoint de health check — RNF07
- [ ] Configurar proteção da branch `main` no GitHub — `07-planejamento-branches.md`

## Épico 1 — Autenticação (UC01)

- [ ] Modelo SQLAlchemy da tabela `usuario`
- [ ] Repository de usuário (buscar por matrícula, criar, atualizar PIN)
- [ ] Service de autenticação: hashing de PIN (`passlib`), geração e
  validação de JWT — ADR-002
- [ ] Service: fluxo de primeiro acesso (matrícula pré-cadastrada define
  PIN) — ADR-002
- [ ] Rota `POST /auth/login`
- [ ] Rota `POST /auth/primeiro-acesso`
- [ ] Tela de login (HTML/CSS, a partir do protótipo da Fase 6)
- [ ] Testes automatizados: login válido, credenciais inválidas,
  matrícula não pré-cadastrada (RN04)

## Épico 2 — Preenchimento da Passagem: Núcleo + Pátio Brisamar (UC02, RN01, RN02, RN04–RN09)

- [x] Modelos SQLAlchemy: `passagem_servico`, `passagem_linha_ocupacao`,
  `equipe_membro`, `passagem_brisamar_detalhe`, `radio`,
  `passagem_radio_uso`
- [x] Repositories correspondentes
- [x] Service: validação de Sup/Inf restrito às linhas 22 e 24 (RN06)
- [x] Service: validação de campos obrigatórios do núcleo e do detalhe
  Brisamar
- [x] Rota `POST /passagens/brisamar`
- [x] Tela de escolha de terminal (a partir do protótipo da Fase 6)
- [x] Tela do formulário Brisamar (a partir do protótipo da Fase 6)
- [ ] Tela de confirmação de envio, com encadeamento para o TECON (Tela 8
  da Fase 6)
- [x] Testes automatizados: envio válido, Sup/Inf inválido em linha não
  permitida, campo obrigatório ausente

## Épico 3 — Preenchimento da Passagem: Terminal TECON (UC02, RN10)

- [ ] Modelo SQLAlchemy: `passagem_tecon_detalhe`, com `houve_atendimento`
- [ ] Repository correspondente
- [ ] Service: validação condicional conforme RN10 (campos de Área 1/Área
  2 obrigatórios apenas se atendidos individualmente)
- [ ] Rota `POST /passagens/tecon`
- [ ] Tela do formulário TECON, com pergunta condicional e Áreas 1/2
  independentes (a partir do protótipo da Fase 6)
- [ ] Testes automatizados: sem atendimento (só linhas), atendimento
  parcial (só uma área), atendimento completo

## Épico 4 — Edição e Histórico (UC04, RN01, RN02)

- [ ] Modelo SQLAlchemy: `passagem_servico_historico`
- [ ] Repository: criação de snapshot antes de atualizar
- [ ] Service: validação de janela de edição (apenas dentro do próprio
  turno — RN01)
- [ ] Rota `PUT /passagens/{id}`
- [ ] Testes automatizados: edição dentro do turno (sucesso), edição fora
  do turno (bloqueio), edição por usuário diferente do autor (bloqueio)

## Épico 5 — Consulta de Passagens (UC03)

- [ ] Repository: consulta com filtros combináveis (data, turno,
  terminal, responsável, linha, palavra-chave)
- [ ] Rota `GET /passagens` com query params de filtro
- [ ] Tela de consulta (a partir do protótipo da Fase 6)
- [ ] Testes automatizados: filtros combinados, nenhum resultado, sem
  filtro (RF19)

## Épico 6 — Exportação de Dados (UC05)

- [ ] Service: geração de arquivo CSV a partir do resultado filtrado
- [ ] Service: geração de arquivo Excel a partir do resultado filtrado
- [ ] Rota `GET /passagens/exportar`
- [ ] Botões de exportação na tela de consulta (a partir do protótipo da
  Fase 6)
- [ ] Testes automatizados: exportação com dados, exportação vazia
  (bloqueio, A1 do UC05)

## Épico 7 — Diagrama de Manobras (UC06)

- [ ] Disponibilizar os PDFs de diagrama (`docs/anexos/`) como arquivos
  estáticos servidos pela aplicação
- [ ] Rota de download do diagrama por terminal
- [ ] Exibição do diagrama na tela de formulário (Brisamar e TECON)

## Épico 8 — Relatório de Falhas por Rádio (UC07, RN08)

- [ ] Repository: agregação de falhas por `radio_id`
- [ ] Rota `GET /relatorios/falhas-radio`
- [ ] Tela de relatório (a partir do protótipo da Fase 6)
- [ ] Testes automatizados: agregação correta, nenhuma falha registrada

---

## Rastreabilidade (Épico → Caso de Uso → Regras de Negócio)

| Épico | Caso de Uso | Regras de Negócio |
|---|---|---|
| 0 — Fundação | — | RNF04, RNF07 |
| 1 — Autenticação | UC01 | RN04 |
| 2 — Núcleo + Brisamar | UC02 | RN01, RN02, RN04–RN09 |
| 3 — TECON | UC02 | RN10 |
| 4 — Edição/Histórico | UC04 | RN01, RN02 |
| 5 — Consulta | UC03 | — |
| 6 — Exportação | UC05 | — |
| 7 — Diagrama | UC06 | — |
| 8 — Relatório de rádios | UC07 | RN08 |

---

## Aprovação

| Papel | Responsável | Status |
|---|---|---|
| Product Owner | (você) | Pendente de revisão |
| Arquiteto de Software | (parceiro) | Pendente de revisão |
| Desenvolvedor Sênior | Claude | Documento elaborado |
