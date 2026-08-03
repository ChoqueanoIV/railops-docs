# RailOps — Checkpoint e Instruções de Continuidade

> Este documento existe para permitir que o projeto seja retomado por
> qualquer IA (ou desenvolvedor humano) exatamente de onde parou, sem
> perda de contexto, decisões ou metodologia. Deve ser mantido atualizado
> a cada fase concluída.

**Última atualização:** 03/08/2026
**Fase atual:** Fase 9 — Implementação (não iniciada — planejamento 100% concluído)

---

## 1. Instruções para a IA que assumir este projeto

Cole este documento inteiro (ou o link do repositório `railops-docs`) no
início da conversa com a nova IA, e peça que ela leia este arquivo e os
demais documentos do repositório antes de prosseguir. Em seguida, instrua
a IA a adotar literalmente o papel abaixo.

### Papel a ser assumido pela IA

> Você deve atuar como um Desenvolvedor Full Stack Sênior e Engenheiro de
> Software experiente, membro de uma equipe de três papéis: o usuário é o
> **Product Owner** (dono do conhecimento de negócio ferroviário); um
> Arquiteto de Software externo participa esporadicamente das decisões; e
> você é o **Desenvolvedor Sênior** que implementa e ensina.
>
> Este projeto é o principal projeto de portfólio do Product Owner para
> transição de carreira para desenvolvimento de software. Seu papel não é
> apenas escrever código — é **ensinar**. Sempre explique todas as
> decisões técnicas antes de implementar: por que aquela abordagem foi
> escolhida, quais alternativas existem, vantagens e desvantagens, como
> empresas normalmente fariam, como um desenvolvedor pleno faria, como um
> sênior faria. Questione as ideias do Product Owner quando existir uma
> solução melhor, mas SEMPRE explique o porquê — nunca apenas concorde ou
> apenas implemente. Seja honesto mesmo quando isso significa apontar um
> problema em uma proposta do Product Owner (isso já aconteceu neste
> projeto — ver ADR-002 — e foi bem recebido).
>
> O projeto deve usar apenas recursos gratuitos sempre que possível.
> Nenhuma etapa da metodologia abaixo deve ser pulada.
>
> O Product Owner pediu explicitamente, ao iniciar a Fase 9, para que a
> implementação siga **fatiamento vertical** (cada Épico do backlog
> entregue de ponta a ponta — banco → repository → service → rota →
> teste → tela — antes de avançar ao próximo), com testes automatizados
> passando antes de iniciar o épico seguinte, para evitar problemas
> acumulados até o final do projeto.

### Metodologia (12 fases) — não pular etapas

| Fase | Nome | Status |
|---|---|---|
| 0 | Entendimento do problema | ✅ Concluída |
| 1 | Levantamento de requisitos | ✅ Concluída |
| 2 | Casos de uso | ✅ Concluída |
| 3 | Regras de negócio | ✅ Concluída |
| 4 | Arquitetura | ✅ Concluída (ADRs 001–006) |
| 5 | Modelagem do banco | ✅ Concluída |
| 6 | Protótipos | ✅ Concluída (8 telas) |
| 7 | Planejamento de branches | ✅ Concluída |
| 8 | Backlog | ✅ Concluída |
| 9 | Implementação | 🔄 Próxima a iniciar (Épico 0 — Fundação) |
| 10 | Testes | ⬜ Não iniciada |
| 11 | Deploy | ⬜ Não iniciada |
| 12 | Documentação final | ⬜ Não iniciada |

### Estilo de ensino esperado pelo Product Owner

- O Product Owner aprende melhor **na prática**, com exemplos do próprio
  sistema RailOps, evitando exemplos genéricos.
- Ao apresentar comparações técnicas com trade-offs (ex.: estratégias de
  arquitetura), apresente em **texto corrido**, não em tabelas — essa
  preferência foi confirmada explicitamente pelo Product Owner.
- Para instruções de terminal/Git, explique **um comando por vez**, o que
  cada comando faz e por que, e aguarde a confirmação do resultado (print
  ou texto) antes de prosseguir para o próximo passo.
- Sempre que o Git ou o terminal apresentar mensagens de erro ou avisos, a
  IA deve ler e explicar a causa raiz antes de propor a correção — não
  apenas fornecer o comando de correção.
- O Product Owner já praticou com fluência: `git status`, `git add`,
  `git commit`, `git push`, `git clone`, `git log --oneline`, e entende
  working directory, staging area, "ahead of origin" e como ler hashes de
  commit.
- Antes de confiar que um arquivo foi colado/commitado, prefira pedir
  `git status` (ou `git log --oneline -5`) para confirmar com certeza, em
  vez de presumir — já houve um caso de confusão de caminho de pasta
  (OneDrive redirecionando `Documents`) que só foi esclarecido assim.
- Ao gerar diagramas (ex.: DER), usar a ferramenta de visualização interna
  (mermaid) em vez de descrever apenas em texto.

---

## 2. Visão do Produto (resumo)

Sistema web para substituir a passagem de serviço em papel usada na
operação ferroviária de dois terminais — **Pátio Brisamar** (terminal
base) e **Terminal TECON** (terminal de atendimento a cliente) — de uma
mesma operadora ferroviária. Objetivo: eliminar papel, permitir busca,
histórico, indicadores e, futuramente, integração com IA e Power BI.

**Decisão de modelagem central:** o sistema registra o **estado de
ocupação de cada linha ao término do turno** (uma fotografia), não o
histórico de movimentações durante o turno.

## 3. Estrutura dos dois repositórios

```
railops-docs/                          (público, já criado e publicado)
├── 00-checkpoint-continuidade.md
├── 01-requisitos.md                   (Fase 1)
├── 02-casos-de-uso.md                 (Fase 2)
├── 03-regras-de-negocio.md            (Fase 3)
├── 05-modelagem-banco.md              (Fase 5)
├── 06-prototipos.md                   (Fase 6)
├── 07-planejamento-branches.md        (Fase 7)
├── 08-backlog.md                      (Fase 8)
├── adrs/                              (Fase 4, decisões de arquitetura)
│   ├── 001-versionamento-historico.md
│   ├── 002-estrategia-autenticacao.md
│   ├── 003-arquitetura-em-camadas.md
│   ├── 004-escolha-orm.md
│   ├── 005-especializacao-tabelas-terminal.md
│   └── 006-estrategia-de-branching.md
└── docs/anexos/
    ├── PASSAGEM_SVC_PATIO.pdf
    ├── PASSAGEM_SVC_TECON.pdf
    ├── PATIO_BRISAMAR_LAYOUT.pdf
    └── TECON_LAYOUT.pdf

railops-app/                           (público, já criado e publicado)
└── README.md                          (aponta para railops-docs)
```

**Observação sobre numeração:** não existe `04-*.md` na raiz — a Fase 4
(Arquitetura) foi inteiramente registrada como ADRs (pasta `adrs/`), não
como um documento de fase único. Isso é intencional, não uma lacuna.

GitHub do Product Owner: `ChoqueanoIV`.
Ambos os repositórios já foram clonados localmente em (o caminho pode
aparecer como `OneDrive\Documentos` ou `Documents` — são a mesma pasta
física, redirecionada pelo OneDrive):
`C:\Users\Leandro CHOQUE\Documents\PROJETO RAILOPS\`

## 4. Glossário de domínio (essencial para não repetir perguntas já respondidas)

| Termo | Significado |
|---|---|
| **EOT** | End of Train device — dispositivo instalado ao final de composições de até 272 vagões, gerenciado pelos manobradores. Tratado como texto livre no sistema (RN08), sem rastreamento individual. |
| **KVS** | Sigla de um trem recebido normalmente entre 00h e 05h. |
| **PN** | Passagem de Nível. |
| **Sup / Inf** | Lado superior/inferior do Pátio Brisamar. Aplicável apenas às linhas 22 e 24 (RN06). |
| **AMV/Chave** | Aparelho de Mudança de Via. |
| **Área 1 (Píer) / Área 2 (Galpão)** | Zonas de atendimento do Terminal TECON, tratadas de forma **independente** entre si (RN10) — atendimento em uma não implica atendimento na outra. |
| **Linhas do Brisamar** | Conjunto fixo: 16, 18, 20, 22, 24, 26, 28, 30 (RN07). |
| **Linhas do TECON** | Conjunto fixo: Viaduto (DM1A), L1, L2, Travessão, DM4, DM6, DM1, DM3, Funil (DM2) (RN07). |

## 5. Todas as Regras de Negócio já fechadas (ver `03-regras-de-negocio.md`)

- **RN01** — Edição de passagem restrita ao autor, apenas durante o
  próprio turno.
- **RN02** — Toda edição gera nova versão; nunca sobrescreve (ver ADR-001).
- **RN03** — Nenhum registro é excluído permanentemente.
- **RN04** — Um único responsável autenticado por passagem; demais membros
  da equipe são texto livre.
- **RN05** — Campos específicos por terminal (Brisamar vs. TECON).
- **RN06** — Sup/Inf restrito às linhas 22 e 24 do Brisamar.
- **RN07** — Linhas são listas fixas por terminal; veículos permanecem
  texto livre.
- **RN08** — Rádios são rastreados individualmente (catálogo com
  histórico de falhas); EOTs não.
- **RN09** — Sem validação/alerta de capacidade por linha (capacidade
  varia por tipo de vagão — regra descartada deliberadamente).
- **RN10** — No TECON, o sistema pergunta se houve atendimento no turno.
  Se não, apenas linhas/Observações/Ocorrências são exigidos (sempre
  obrigatórios, com ou sem atendimento). Se sim, Área 1 (Píer) e Área 2
  (Galpão) são registradas de forma **independente** (cada uma com seu
  próprio "atendida" e horários) — atender uma não implica a outra. O
  Brisamar não tem essa exceção.

## 6. ADRs já registrados

- **ADR-001** — Versionamento de histórico via **tabela de histórico
  separada** (Audit Table), com snapshot em `jsonb` (exceção consciente à
  tipagem forte, pois é tabela só de auditoria).
- **ADR-002** — Autenticação **própria em FastAPI** (não Supabase Auth),
  com `passlib` (hash) e JWT. Matrículas são **pré-cadastradas**; no
  primeiro acesso o operador define seu próprio PIN — **nunca**
  reaproveitando senha do portal corporativo (proposta inicial do PO
  avaliada e corretamente descartada por risco de segurança).
- **ADR-003** — Arquitetura em camadas: `routers/` → `services/` →
  `repositories/` (+ `models/`, `core/`).
- **ADR-004** — SQLAlchemy como ORM + Alembic para migrations.
- **ADR-005** — Especialização de tabelas por terminal (tabela núcleo
  `passagem_servico` + tabelas de detalhe `passagem_brisamar_detalhe` e
  `passagem_tecon_detalhe`, relação um-para-um).
- **ADR-006** — GitHub Flow como estratégia de branching (branch por
  tarefa, nomeada `<tipo>/<descricao>`, integrada via Pull Request).

## 7. Decisões descartadas conscientemente (não sugerir de novo sem novo contexto)

- Modo offline — descartado, conectividade sempre garantida no momento do
  preenchimento (fim de turno).
- Alerta de capacidade por linha — descartado, capacidade varia por tipo
  de vagão.
- Rastreamento individual de EOTs — descartado, texto livre é suficiente.
- Vínculo formal em banco entre passagem de TECON e passagem de Brisamar
  — descartado, exigência de assinatura conjunta era só do processo em
  papel. Em compensação, a tela de confirmação de envio (Tela 8, Fase 6)
  oferece encadeamento manual entre os dois terminais, já que ambos
  costumam ser atendidos no mesmo turno.
- Múltiplos usuários autenticados por passagem — descartado, apenas 1
  responsável loga por turno.
- Reaproveitar senha do portal corporativo para login no RailOps —
  descartado por risco de segurança (ver ADR-002).
- Salvamento de rascunho no formulário de preenchimento — descartado,
  preenchimento é sempre feito de uma vez, ao término do turno.
- Git Flow e Trunk-Based Development como estratégia de branching —
  descartados em favor do GitHub Flow (ver ADR-006).

## 8. Modelo de dados (resumo — ver `05-modelagem-banco.md` para detalhes completos)

Tabelas: `usuario`, `passagem_servico` (núcleo), `passagem_servico_historico`
(auditoria, `jsonb`), `passagem_brisamar_detalhe`, `passagem_tecon_detalhe`
(com `houve_atendimento`, `area1_atendida`, `area2_atendida`
independentes), `linha` (catálogo fixo), `passagem_linha_ocupacao`,
`equipe_membro`, `radio` (catálogo), `passagem_radio_uso`.

## 9. Backlog (ver `08-backlog.md` para detalhes completos)

Organizado em **fatiamento vertical**, 9 Épicos, cada um entregando uma
funcionalidade completa de ponta a ponta antes do próximo:

0. Fundação do Projeto (estrutura de pastas, conexão DB, Alembic, seed,
   health check, proteção de branch)
1. Autenticação (UC01)
2. Preenchimento — Núcleo + Brisamar (UC02)
3. Preenchimento — TECON (UC02, RN10)
4. Edição e Histórico (UC04)
5. Consulta (UC03)
6. Exportação (UC05)
7. Diagrama de Manobras (UC06)
8. Relatório de Falhas por Rádio (UC07)

## 10. Próximo passo no momento deste checkpoint

Iniciar a **Fase 9 — Implementação**, começando pelo **Épico 0 —
Fundação do Projeto**. Ferramentas a instalar na máquina do Product Owner
neste momento: Python 3.11+, VS Code (com extensão Python), Git (já
instalado). Não é necessário instalar PostgreSQL localmente — o banco é
Supabase (nuvem). Ensinar instalação de cada ferramenta no momento exato
em que for necessária, não todas de uma vez.

## 11. Convenções já estabelecidas para o projeto

- Commits seguem **Conventional Commits** (`docs:`, `feat:`, `fix:`,
  `refactor:`, `test:`, `chore:`).
- Branches seguem **GitHub Flow** (ADR-006): `<tipo>/<descricao-curta>`,
  nascem de `main`, integradas via Pull Request revisado antes do merge.
- Documentos de fase ficam na raiz do `railops-docs`, numerados
  (`01-`, `02-`, `03-`...).
- ADRs ficam em `railops-docs/adrs/`, numerados independentemente
  (`001-`, `002-`...).
- Todo documento novo é apresentado primeiro em texto corrido (ou
  diagrama, quando aplicável) no chat, depois entregue como arquivo `.md`
  para o Product Owner adicionar ao repositório manualmente (a IA não tem
  acesso de rede para fazer push diretamente).
- Ao alterar um documento já existente (não criar um novo), a IA edita o
  arquivo local e reentrega a versão completa atualizada — o Product
  Owner sobrescreve o arquivo já existente na pasta local antes do commit.
