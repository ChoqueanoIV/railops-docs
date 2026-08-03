# RailOps — Modelagem do Banco de Dados (Fase 5)

> Documento vivo. Parte da série de documentação técnica do projeto RailOps.

**Status:** Em revisão com o Product Owner
**Fase anterior:** Fase 4 — Arquitetura (concluída)
**Próxima fase:** Fase 6 — Protótipos

---

## Diagrama Entidade-Relacionamento (DER)

O diagrama completo foi apresentado interativamente durante a sessão de
elaboração deste documento (ferramenta de visualização). Este documento
registra a definição textual detalhada de cada tabela, para referência
permanente e para orientar a criação das migrations (Alembic) na Fase 9.

---

## Tabelas

### `usuario`

Origem: ADR-002 (estratégia de autenticação), RN04.

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| matricula | varchar | UNIQUE, NOT NULL |
| nome | varchar | NOT NULL |
| senha_hash | varchar | NULL (preenchido apenas após o primeiro acesso) |
| pin_definido | boolean | NOT NULL, default `false` |
| criado_em | timestamp | NOT NULL, default now() |

**Observação:** matrículas são pré-cadastradas (`pin_definido = false`) por
um processo administrativo antes do primeiro acesso do operador (ADR-002).

---

### `passagem_servico` (núcleo)

Origem: RF04–RF11, ADR-001, ADR-005.

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| terminal | varchar (enum: `BRISAMAR`, `TECON`) | NOT NULL |
| data | date | NOT NULL |
| turno | varchar | NOT NULL |
| responsavel_id | uuid | FK → `usuario.id`, NOT NULL |
| observacoes | text | NULL |
| relatorio_ocorrencias | text | NULL |
| mobile_utilizado | boolean | NOT NULL |
| mobile_justificativa | text | NULL (obrigatório na aplicação se `mobile_utilizado = false`) |
| criado_em | timestamp | NOT NULL, default now() |
| atualizado_em | timestamp | NOT NULL, default now() |

---

### `passagem_servico_historico`

Origem: ADR-001 (RN02).

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| passagem_id | uuid | FK → `passagem_servico.id`, NOT NULL |
| versao | int | NOT NULL |
| snapshot | jsonb | NOT NULL — cópia completa do estado da passagem (núcleo + detalhe) antes da edição |
| alterado_por | uuid | FK → `usuario.id`, NOT NULL |
| alterado_em | timestamp | NOT NULL, default now() |

**Nota de design:** diferente das demais tabelas, o histórico usa uma
coluna `jsonb` em vez de colunas tipadas espelhadas. Trade-off aceito
conscientemente: esta tabela é exclusivamente de auditoria/consulta
eventual (RF17, RF18), nunca fonte de regra de negócio ativa — o ganho de
simplicidade de implementação supera, aqui, a perda pontual de tipagem
forte adotada como padrão geral no ADR-004.

---

### `passagem_brisamar_detalhe`

Origem: RF14, RF15, RN05.

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| passagem_id | uuid | FK → `passagem_servico.id`, UNIQUE, NOT NULL |
| radios_operantes | int | NOT NULL |
| radios_inoperantes | int | NOT NULL |
| baterias | int | NOT NULL |
| carregadores | int | NOT NULL |
| eots_disponiveis | text | NULL |
| eots_avariados | text | NULL |

---

### `passagem_tecon_detalhe`

Origem: RF12, RF13, RN05, RN10.

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| passagem_id | uuid | FK → `passagem_servico.id`, UNIQUE, NOT NULL |
| houve_atendimento | boolean | NOT NULL — resposta à pergunta de RN10 |
| carga_mal_posicionada | boolean | NULL — obrigatório apenas se `houve_atendimento = true` |
| carga_mal_posicionada_descricao | text | NULL |
| area1_atendida | boolean | NULL — obrigatório apenas se `houve_atendimento = true` |
| area1_inicio | time | NULL |
| area1_termino | time | NULL |
| area2_atendida | boolean | NULL — obrigatório apenas se `houve_atendimento = true` |
| area2_inicio | time | NULL |
| area2_termino | time | NULL |

**Observação (RN10):** quando `houve_atendimento = false`, o registro
ainda é criado nesta tabela (para não perder o rastro de que a pergunta
foi respondida), mas os demais campos ficam `NULL` — a validação de
obrigatoriedade condicional é responsabilidade da camada de Service
(ADR-003), não do schema.

---

### `linha`

Origem: RF07, RN06, RN07. Tabela de catálogo (dado de referência, não
transacional).

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| terminal | varchar (enum: `BRISAMAR`, `TECON`) | NOT NULL |
| codigo | varchar | NOT NULL — ex.: `22`, `DM4` |
| permite_sup_inf | boolean | NOT NULL, default `false` — `true` apenas para as linhas 22 e 24 do Brisamar (RN06) |

**Carga inicial:** esta tabela é populada via seed/migration na Fase 9,
com os valores fixos já levantados na Fase 0 (linhas 16–30 do Brisamar;
Viaduto, L1, L2, Travessão, DM1, DM3, DM4, DM6, Funil do TECON).

---

### `passagem_linha_ocupacao`

Origem: RF06, RF08, UC02.

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| passagem_id | uuid | FK → `passagem_servico.id`, NOT NULL |
| linha_id | uuid | FK → `linha.id`, NOT NULL |
| veiculos | text | NULL — texto livre (ex.: "2MK + 5BOB + 8LV") |
| sup_inf | varchar (enum: `SUP`, `INF`) | NULL — só aplicável quando `linha.permite_sup_inf = true` (RN06) |

---

### `equipe_membro`

Origem: RF02, RN04.

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| passagem_id | uuid | FK → `passagem_servico.id`, NOT NULL |
| nome | varchar | NOT NULL |
| matricula | varchar | NOT NULL |

**Observação:** membros de equipe aqui são texto simples, sem vínculo com
`usuario` — apenas o responsável (RN04) possui conta no sistema.

---

### `radio`

Origem: RN08 (catálogo rastreável).

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| numero | varchar | UNIQUE, NOT NULL |
| criado_em | timestamp | NOT NULL, default now() |

---

### `passagem_radio_uso`

Origem: RF16, RN08, UC07.

| Coluna | Tipo | Restrições |
|---|---|---|
| id | uuid | PK |
| passagem_id | uuid | FK → `passagem_servico.id`, NOT NULL |
| radio_id | uuid | FK → `radio.id`, NOT NULL |
| manobrador_nome | varchar | NOT NULL |
| hora_retirada | time | NULL |
| hora_entrega | time | NULL |
| apresentou_falha | boolean | NOT NULL, default `false` |
| falha_descricao | text | NULL |

---

## Rastreabilidade (Tabelas → Requisitos/Regras/ADRs)

| Tabela | Origem |
|---|---|
| `usuario` | ADR-002, RN04 |
| `passagem_servico` | RF04–RF11, ADR-005 |
| `passagem_servico_historico` | ADR-001, RN02 |
| `passagem_brisamar_detalhe` | RF14, RF15, RN05 |
| `passagem_tecon_detalhe` | RF12, RF13, RN05 |
| `linha` | RF07, RN06, RN07 |
| `passagem_linha_ocupacao` | RF06, RF08 |
| `equipe_membro` | RF02, RN04 |
| `radio` | RN08 |
| `passagem_radio_uso` | RF16, RN08, UC07 |

---

## Aprovação

| Papel | Responsável | Status |
|---|---|---|
| Product Owner | (você) | Pendente de revisão |
| Arquiteto de Software | (parceiro) | Pendente de revisão |
| Desenvolvedor Sênior | Claude | Documento elaborado |
