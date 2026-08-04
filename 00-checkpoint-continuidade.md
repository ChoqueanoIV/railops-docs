# RailOps — Checkpoint e Instruções de Continuidade

> Este documento existe para permitir que o projeto seja retomado por
> qualquer IA (ou desenvolvedor humano) exatamente de onde parou, sem
> perda de contexto, decisões ou metodologia. Deve ser mantido atualizado
> a cada fase concluída.

**Última atualização:** 04/08/2026
**Fase atual:** Fase 9 — Implementação (em andamento — Épico 0 concluído, Épico 1 iniciando)

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
> O Product Owner é **nível iniciante na prática**, apesar do projeto bem
> estruturado — aprendeu a teoria o suficiente para ser aprovado nos
> cursos, mas tem pouca experiência de mão na massa. Aprende melhor
> **fazendo**, no próprio projeto. Por isso, cada etapa prática deve ser
> ensinada **passo a passo, um comando por vez, aguardando confirmação do
> resultado antes de seguir para o próximo** — nunca entregar uma
> sequência longa de comandos de uma vez.
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
| 9 | Implementação | 🔄 Em andamento (Épico 0 concluído — ver seção 12; Épico 1 iniciando — ver seção 14) |
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
  ou texto) antes de prosseguir para o próximo passo. Isso é uma regra
  reforçada explicitamente: o Product Owner é iniciante na prática e quer
  aprender fazendo, não apenas copiando blocos grandes de comando.
- Sempre que o Git ou o terminal apresentar mensagens de erro ou avisos, a
  IA deve ler e explicar a causa raiz antes de propor a correção — não
  apenas fornecer o comando de correção.
- O Product Owner já praticou com fluência: `git status`, `git add`,
  `git commit`, `git push`, `git pull`, `git checkout`, `git branch -d`,
  `git clone`, `git log --oneline`, e entende working directory, staging
  area, "ahead/behind of origin", fast-forward e como ler hashes de
  commit. Já abriu e mesclou Pull Requests pelo GitHub.
- O Product Owner trabalha **inteiramente pelo terminal (PowerShell)**,
  não usa VS Code (nem outro editor) para criar/editar arquivos — prefere
  comandos como `Out-File`, here-strings (`@'...'@`), `Get-ChildItem`,
  `Get-Location`. A IA deve adaptar todas as instruções de criação/edição
  de arquivo para esse fluxo, sem sugerir abrir um editor gráfico, a
  menos que o Product Owner peça.
- Antes de confiar que um arquivo foi colado/commitado, prefira pedir
  `git status` (ou `git log --oneline -5`) para confirmar com certeza, em
  vez de presumir — já houve um caso de confusão de caminho de pasta
  (OneDrive redirecionando `Documents`) que só foi esclarecido assim.
- Scripts de teste/debug temporários (ex.: teste manual de conexão com
  banco) devem ser tratados como descartáveis: usados uma vez para
  validar, depois removidos do repositório antes de qualquer commit —
  nunca versionados como "sujeira". Funcionalidades reais equivalentes
  (ex.: um health check de verdade) devem ser implementadas como parte
  da aplicação (ex.: rota FastAPI), não como script solto.
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
├── README.md                          (aponta para railops-docs)
└── backend/
    ├── app/
    │   ├── __init__.py
    │   ├── core/
    │   │   ├── __init__.py
    │   │   └── database.py            (engine, SessionLocal, Base, get_db())
    │   ├── models/__init__.py         (vazio, aguardando Épico 1)
    │   ├── repositories/__init__.py   (vazio, aguardando Épico 1)
    │   ├── routers/__init__.py        (vazio, aguardando Épico 1)
    │   └── services/__init__.py       (vazio, aguardando Épico 1)
    ├── .env                           (DATABASE_URL, ignorado pelo Git)
    └── requirements.txt
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

## 5. Todas as Regras de Negócio já fechadas

Ver `03-regras-de-negocio.md` no repositório `railops-docs` para a lista
completa (RN01 a RN10+). Não repetir o levantamento — já fechado desde a
Fase 3.

## 6. Decisões de arquitetura (ADRs)

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
   health check, proteção de branch) — **✅ CONCLUÍDO** (ver seção 12)
1. Autenticação (UC01) — **🔄 INICIANDO** (ver seção 14)
2. Preenchimento — Núcleo + Brisamar (UC02)
3. Preenchimento — TECON (UC02, RN10)
4. Edição e Histórico (UC04)
5. Consulta (UC03)
6. Exportação (UC05)
7. Diagrama de Manobras (UC06)
8. Relatório de Falhas por Rádio (UC07)

## 10. Ambiente já instalado

Python 3.13 (comando `python`), VS Code instalado mas **não usado** (PO
trabalha via terminal), Git. Ambiente virtual em `railops-app/backend/venv`
(nota: verificar se venv está em `backend/venv` ou `railops-app/venv` —
ver seção 12, checar antes de assumir). PostgreSQL não instalado
localmente — banco é 100% Supabase (nuvem).

## 11. Pendências conhecidas (não esquecer, mas não é bloqueante agora)

- **Health check real**: o Épico 0 previa um health check como item da
  Fundação do Projeto. Ele **ainda não foi implementado como rota**
  (só existiu um script de teste manual de conexão, já removido de
  propósito — ver seção 12). Quando for a hora de implementar, deve ser
  uma rota FastAPI (ex.: `GET /health`), não um script solto.
- Alembic (migrations) mencionado no backlog do Épico 0, mas a
  configuração/primeira migration ainda não foi feita nesta sessão —
  confirmar com o Product Owner se isso já existe ou se entra junto com
  o primeiro model do Épico 1.

---

## 12. Progresso da Implementação — Épico 0 (CONCLUÍDO)

### Ambiente de desenvolvimento configurado
- Python 3.13 confirmado como comando padrão (`python`, não `py` —
  máquina tem as duas versões instaladas; `python` foi escolhido por
  consistência).
- Ambiente virtual criado, ativado via `.\venv\Scripts\Activate.ps1`
  (funciona sem bloqueio de política de execução nesta máquina).
- Bibliotecas instaladas: `fastapi`, `uvicorn`, `sqlalchemy`, `alembic`,
  `psycopg2-binary`, `python-dotenv`. Registradas em
  `backend/requirements.txt` via `pip freeze`.

### Estrutura de pastas do backend — CONCLUÍDO (via PR #1, mesclado)
Estrutura `backend/app/{routers,services,repositories,models,core}`
criada e mesclada na `main` do `railops-app` através do primeiro Pull
Request do projeto (branch `chore/estrutura-inicial-backend`), seguindo
o GitHub Flow (ADR-006). Cada pasta contém um `__init__.py` vazio.

### Banco de dados Supabase — CRIADO
- Projeto criado do zero: nome `railops`, região **South America (São
  Paulo)**, plano Free.
- **Data API desabilitada deliberadamente** (Enable Data API,
  Automatically expose new tables, Enable automatic RLS — todos
  desmarcados na criação), pois o projeto não usa bibliotecas cliente
  Supabase (supabase-js): todo acesso ao banco passa exclusivamente pelo
  SQLAlchemy dentro do FastAPI.
- **Método de conexão: Session Pooler** (não "Direct Connection") — a
  conexão direta exige IPv6, que a rede do Product Owner não suporta;
  o Session Pooler faz proxy IPv4 gratuito. Host:
  `aws-0-sa-east-1.pooler.supabase.com`, porta `5432`, usuário no formato
  `postgres.<referência-do-projeto>`.
- String de conexão salva em `backend/.env` (variável `DATABASE_URL`),
  arquivo corretamente ignorado pelo `.gitignore` — nunca versionado.
- **Atenção de segurança:** a senha original do banco foi exibida em um
  print compartilhado durante o desenvolvimento; recomendou-se resetar
  a senha do banco no painel do Supabase (botão "Reset database
  password") como precaução. Confirmar com o Product Owner se isso já
  foi feito antes de prosseguir com qualquer coisa sensível.

### Conexão com o banco — CONFIGURADA E VALIDADA
`backend/app/core/database.py` criado com `engine` (SQLAlchemy,
gerencia a conexão de baixo nível), `SessionLocal` (fábrica de sessões
por requisição), `Base` (classe base declarativa para os futuros
models) e `get_db()` (dependência do FastAPI para injetar sessões nas
rotas).

Conexão testada manualmente com sucesso (script descartável
`testar_conexao.py`, executando `SELECT 1` via `engine.connect()`,
usando `text()` do SQLAlchemy para SQL literal). Funcionou de primeira.
Script removido do repositório após a validação, por não fazer parte da
aplicação em si (não deve virar artefato permanente — ver pendência do
health check na seção 11).

### Branch mesclada — PR #2
`chore/configura-conexao-banco` → aberta, sem conflitos, mesclada na
`main` via Pull Request #2 no GitHub, branch remota e local deletadas
após o merge. `git pull` na `main` local trouxe as mudanças com
fast-forward simples.

### Lições de ambiente registradas (para evitar repetir os mesmos problemas)
1. **OneDrive sincronizando a pasta do projeto pode travar comandos Git**
   que criam/apagam muitos arquivos ao mesmo tempo (ex.: `git checkout`
   trocando de branch). Sintoma: `"Deletion of directory '...' failed.
   Should I try again? (y/n)"` repetidamente. Solução: pausar a
   sincronização do OneDrive (ícone da bandeja do sistema) antes de
   operações Git que mexem em muitos arquivos, reativar depois.
   **Atualização:** esse sintoma específico já ocorreu uma vez mesmo
   com o OneDrive pausado — nesse caso, a causa não foi o OneDrive, e a
   solução foi simplesmente responder `n` (não tentar de novo) a cada
   prompt de exclusão até o Git terminar a operação sozinho; o
   `checkout` completou corretamente mesmo recusando as exclusões, sem
   perda de arquivos reais. Se acontecer de novo, responder `n` em
   sequência até o prompt limpar é uma solução válida e seguem, não é
   preciso decidir a causa raiz na hora.
2. **Um `git checkout` interrompido no meio (por causa do problema acima)
   pode deixar a branch local desatualizada em relação ao GitHub**, mesmo
   que um `git pull` pareça ter sido executado depois. Se algo parecer
   "faltando" depois de uma troca de branch turbulenta, rodar
   `git fetch origin` seguido de `git log --oneline origin/main` para
   comparar a verdade do servidor com o estado local, em vez de
   presumir. Se a branch local estiver atrasada e sem conflitos reais,
   `git merge origin/main` (ou `git pull`) resolve com um fast-forward
   simples.
3. **O Bloco de Notas do Windows pode adicionar `.txt` ao final de
   arquivos sem extensão reconhecida** (como `.env`), sem aviso claro.
   Isso já causou a perda aparente (mas não real) de uma configuração de
   `.env`. Solução adotada: sempre criar/editar arquivos via terminal
   (PowerShell), nunca via "Novo arquivo" do Explorador do Windows nem
   Bloco de Notas. Para criar arquivos com conteúdo multilinha direto
   pelo PowerShell, usar here-strings: `@'...conteúdo...'@ | Out-File
   -FilePath nome.py -Encoding utf8`.
4. Ao investigar arquivos "sumidos", usar `Get-ChildItem <pasta> -Force
   -File` em vez de `dir`, pois é mais confiável para mostrar arquivos
   ocultos/com extensão inesperada no Windows.
5. **Nunca digitar um novo comando enquanto o terminal ainda espera
   resposta a um prompt interativo anterior** (ex.: o `(y/n)` do Git
   durante um checkout travado) — o texto digitado é interpretado como
   resposta ao prompt pendente, não como um novo comando, e pode
   encadear outras perguntas inesperadas. Sempre resolver o prompt
   pendente primeiro (responder y ou n), só então digitar o próximo
   comando.

---

## 13. Convenções já estabelecidas para o projeto

- Commits seguem **Conventional Commits** (`docs:`, `feat:`, `fix:`,
  `refactor:`, `test:`, `chore:`). Já usado na prática: `docs:` para
  atualização de documentação, `feat:` para entrega de funcionalidade
  (ex.: a configuração de conexão com banco foi classificada como
  `feat`, não `chore`, por entregar comportamento novo ao sistema,
  mesmo a branch tendo nome `chore/...`).
- Branches seguem **GitHub Flow** (ADR-006): `<tipo>/<descricao-curta>`,
  nascem de `main`, integradas via Pull Request revisado antes do merge,
  deletadas (local e remota) logo após o merge.
- Descrição de PR segue estrutura: **O que foi feito** / **Decisões
  técnicas** / **Validação** / **Próximos passos** — já usada com bons
  resultados no PR #2.
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
- Scripts de validação/debug pontuais nunca são commitados — são criados,
  usados uma vez, e apagados antes do próximo `git add`.

---

## 14. Próximo passo no momento deste checkpoint — Épico 1 (Autenticação)

Ainda **não iniciado** na prática. Segue o fatiamento vertical: modelo de
dados (tabela `usuario`) → repository → service → rota → teste → tela,
antes de avançar ao Épico 2.

**Primeira decisão pendente a ser conduzida com o Product Owner:**
apresentar (em texto corrido, com trade-offs, sem tabela) o que é um
"model" do SQLAlchemy, por que ele vem antes do repository nessa ordem
de fatiamento, e então caminhar para a criação do primeiro model —
`usuario` — como arquivo `backend/app/models/usuario.py`, herdando de
`Base` (definido em `core/database.py`).

Pontos de negócio já fechados que o model de `usuario` precisa respeitar
(não perguntar de novo, já decidido — ver ADR-002 e seção 5/7):
- Matrículas são pré-cadastradas (não há cadastro público de usuário).
- No primeiro acesso, o operador define seu próprio PIN.
- PIN deve ser armazenado com hash (`passlib`), nunca em texto puro.
- Autenticação própria via JWT, não Supabase Auth.

A IA deve perguntar ao Product Owner se ele já sabe/lembra os campos
exatos da tabela `usuario` conforme `05-modelagem-banco.md`, ou se deve
consultar esse documento antes de propor o model — evitar assumir
estrutura de campos sem confirmar contra a modelagem já fechada.

Alembic (migrations) ainda não foi configurado nesta sessão — deve ser
tratado como parte do primeiro trabalho prático do Épico 1, junto com o
model, pois é o mecanismo que vai efetivamente criar a tabela `usuario`
no banco Supabase a partir do model Python.
