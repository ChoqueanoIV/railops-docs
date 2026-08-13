# RailOps --- Checkpoint e Instruções de Continuidade

> Este documento existe para permitir que o projeto seja retomado por
> qualquer IA (ou desenvolvedor humano) exatamente de onde parou, sem
> perda de contexto, decisões ou metodologia. Deve ser mantido
> atualizado a cada fase concluída.

**Última atualização:** 13/08/2026 **Fase atual:** Fase 9 ---
Implementação (em andamento --- Épicos 0 e 1 concluídos; próximo passo:
iniciar o Épico 2, Preenchimento --- Núcleo + Brisamar)

------------------------------------------------------------------------

## 1. Instruções para a IA que assumir este projeto

Cole este documento inteiro (ou o link do repositório `railops-docs`) no
início da conversa com a nova IA, e peça que ela leia este arquivo e os
demais documentos do repositório antes de prosseguir. Em seguida,
instrua a IA a adotar literalmente o papel abaixo.

### Papel a ser assumido pela IA

> Você deve atuar como um Desenvolvedor Full Stack Sênior e Engenheiro
> de Software experiente, membro de uma equipe de três papéis: o usuário
> é o **Product Owner** (dono do conhecimento de negócio ferroviário);
> um Arquiteto de Software externo participa esporadicamente das
> decisões; e você é o **Desenvolvedor Sênior** que implementa e ensina.
>
> Este projeto é o principal projeto de portfólio do Product Owner para
> transição de carreira para desenvolvimento de software. Seu papel não
> é apenas escrever código --- é **ensinar**. Sempre explique todas as
> decisões técnicas antes de implementar: por que aquela abordagem foi
> escolhida, quais alternativas existem, vantagens e desvantagens, como
> empresas normalmente fariam, como um desenvolvedor pleno faria, como
> um sênior faria. Questione as ideias do Product Owner quando existir
> uma solução melhor, mas SEMPRE explique o porquê --- nunca apenas
> concorde ou apenas implemente. Seja honesto mesmo quando isso
> significa apontar um problema em uma proposta do Product Owner (isso
> já aconteceu neste projeto --- ver ADR-002 --- e foi bem recebido).
>
> O Product Owner é **nível iniciante na prática**, apesar do projeto
> bem estruturado --- aprendeu a teoria o suficiente para ser aprovado
> nos cursos, mas tem pouca experiência de mão na massa. Aprende melhor
> **fazendo**, no próprio projeto. Por isso, cada etapa prática deve ser
> ensinada **passo a passo, um comando por vez, aguardando confirmação
> do resultado antes de seguir para o próximo** --- nunca entregar uma
> sequência longa de comandos de uma vez.
>
> O projeto deve usar apenas recursos gratuitos sempre que possível.
> Nenhuma etapa da metodologia abaixo deve ser pulada.
>
> O Product Owner pediu explicitamente, ao iniciar a Fase 9, para que a
> implementação siga **fatiamento vertical** (cada Épico do backlog
> entregue de ponta a ponta --- banco → repository → service → rota →
> teste → tela --- antes de avançar ao próximo), com testes
> automatizados passando antes de iniciar o épico seguinte, para evitar
> problemas acumulados até o final do projeto.

### Metodologia (12 fases) --- não pular etapas

  -----------------------------------------------------------------------
  Fase                    Nome                    Status
  ----------------------- ----------------------- -----------------------
  0                       Entendimento do         ✅ Concluída
                          problema                

  1                       Levantamento de         ✅ Concluída
                          requisitos              

  2                       Casos de uso            ✅ Concluída

  3                       Regras de negócio       ✅ Concluída

  4                       Arquitetura             ✅ Concluída (ADRs
                                                  001--006)

  5                       Modelagem do banco      ✅ Concluída

  6                       Protótipos              ✅ Concluída (8 telas)

  7                       Planejamento de         ✅ Concluída
                          branches                

  8                       Backlog                 ✅ Concluída

  9                       Implementação           🔄 Em andamento (Épicos
                                                  0 e 1 concluídos; próximo:
                                                  Épico 2 --- ver seção 20)

  10                      Testes                  ⬜ Não iniciada

  11                      Deploy                  ⬜ Não iniciada

  12                      Documentação final      ⬜ Não iniciada
  -----------------------------------------------------------------------

### Estilo de ensino esperado pelo Product Owner

-   O Product Owner aprende melhor **na prática**, com exemplos do
    próprio sistema RailOps, evitando exemplos genéricos.
-   Ao apresentar comparações técnicas com trade-offs (ex.: estratégias
    de arquitetura), apresente em **texto corrido**, não em tabelas ---
    essa preferência foi confirmada explicitamente pelo Product Owner.
-   Para instruções de terminal/Git, explique **um comando por vez**, o
    que cada comando faz e por que, e aguarde a confirmação do resultado
    (print ou texto) antes de prosseguir para o próximo passo. Isso é
    uma regra reforçada explicitamente: o Product Owner é iniciante na
    prática e quer aprender fazendo, não apenas copiando blocos grandes
    de comando.
-   Sempre que o Git ou o terminal apresentar mensagens de erro ou
    avisos, a IA deve ler e explicar a causa raiz antes de propor a
    correção --- não apenas fornecer o comando de correção.
-   O Product Owner já praticou com fluência: `git status`, `git add`,
    `git commit`, `git push`, `git pull`, `git checkout`,
    `git branch -d`, `git clone`, `git log --oneline`, e entende working
    directory, staging area, "ahead/behind of origin", fast-forward e
    como ler hashes de commit. Já abriu e mesclou Pull Requests pelo
    GitHub.
-   O Product Owner trabalha **inteiramente pelo terminal
    (PowerShell)**, não usa VS Code (nem outro editor) para criar/editar
    arquivos --- prefere comandos como `Out-File`, here-strings
    (`@'...'@`), `Get-ChildItem`, `Get-Location`. A IA deve adaptar
    todas as instruções de criação/edição de arquivo para esse fluxo,
    sem sugerir abrir um editor gráfico, a menos que o Product Owner
    peça.
-   Antes de confiar que um arquivo foi colado/commitado, prefira pedir
    `git status` (ou `git log --oneline -5`) para confirmar com certeza,
    em vez de presumir --- já houve um caso de confusão de caminho de
    pasta (OneDrive redirecionando `Documents`) que só foi esclarecido
    assim.
-   Scripts de teste/debug temporários (ex.: teste manual de conexão com
    banco) devem ser tratados como descartáveis: usados uma vez para
    validar, depois removidos do repositório antes de qualquer commit
    --- nunca versionados como "sujeira". Funcionalidades reais
    equivalentes (ex.: um health check de verdade) devem ser
    implementadas como parte da aplicação (ex.: rota FastAPI), não como
    script solto.
-   Ao gerar diagramas (ex.: DER), usar a ferramenta de visualização
    interna (mermaid) em vez de descrever apenas em texto.

------------------------------------------------------------------------

## 2. Visão do Produto (resumo)

Sistema web para substituir a passagem de serviço em papel usada na
operação ferroviária de dois terminais --- **Pátio Brisamar** (terminal
base) e **Terminal TECON** (terminal de atendimento a cliente) --- de
uma mesma operadora ferroviária. Objetivo: eliminar papel, permitir
busca, histórico, indicadores e, futuramente, integração com IA e Power
BI.

**Decisão de modelagem central:** o sistema registra o **estado de
ocupação de cada linha ao término do turno** (uma fotografia), não o
histórico de movimentações durante o turno.

## 3. Estrutura dos dois repositórios

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

**Observação sobre numeração:** não existe `04-*.md` na raiz --- a Fase
4 (Arquitetura) foi inteiramente registrada como ADRs (pasta `adrs/`),
não como um documento de fase único. Isso é intencional, não uma lacuna.

GitHub do Product Owner: `ChoqueanoIV`. Ambos os repositórios já foram
clonados localmente em (o caminho pode aparecer como
`OneDrive\Documentos` ou `Documents` --- são a mesma pasta física,
redirecionada pelo OneDrive):
`C:\Users\Leandro CHOQUE\Documents\PROJETO RAILOPS\`

## 4. Glossário de domínio (essencial para não repetir perguntas já respondidas)

  -----------------------------------------------------------------------
  Termo                               Significado
  ----------------------------------- -----------------------------------
  **EOT**                             End of Train device --- dispositivo
                                      instalado ao final de composições
                                      de até 272 vagões, gerenciado pelos
                                      manobradores. Tratado como texto
                                      livre no sistema (RN08), sem
                                      rastreamento individual.

  **KVS**                             Sigla de um trem recebido
                                      normalmente entre 00h e 05h.

  **PN**                              Passagem de Nível.

  **Sup / Inf**                       Lado superior/inferior do Pátio
                                      Brisamar. Aplicável apenas às
                                      linhas 22 e 24 (RN06).

  **AMV/Chave**                       Aparelho de Mudança de Via.

  **Área 1 (Píer) / Área 2 (Galpão)** Zonas de atendimento do Terminal
                                      TECON, tratadas de forma
                                      **independente** entre si (RN10)
                                      --- atendimento em uma não implica
                                      atendimento na outra.

  **Linhas do Brisamar**              Conjunto fixo: 16, 18, 20, 22, 24,
                                      26, 28, 30 (RN07).

  **Linhas do TECON**                 Conjunto fixo: Viaduto (DM1A), L1,
                                      L2, Travessão, DM4, DM6, DM1, DM3,
                                      Funil (DM2) (RN07).
  -----------------------------------------------------------------------

## 5. Todas as Regras de Negócio já fechadas

Ver `03-regras-de-negocio.md` no repositório `railops-docs` para a lista
completa (RN01 a RN10+). Não repetir o levantamento --- já fechado desde
a Fase 3.

## 6. Decisões de arquitetura (ADRs)

-   **ADR-001** --- Versionamento de histórico via **tabela de histórico
    separada** (Audit Table), com snapshot em `jsonb` (exceção
    consciente à tipagem forte, pois é tabela só de auditoria).
-   **ADR-002** --- Autenticação **própria em FastAPI** (não Supabase
    Auth), com `passlib` (hash) e JWT. Matrículas são
    **pré-cadastradas**; no primeiro acesso o operador define seu
    próprio PIN --- **nunca** reaproveitando senha do portal corporativo
    (proposta inicial do PO avaliada e corretamente descartada por risco
    de segurança).
-   **ADR-003** --- Arquitetura em camadas: `routers/` → `services/` →
    `repositories/` (+ `models/`, `core/`).
-   **ADR-004** --- SQLAlchemy como ORM + Alembic para migrations.
-   **ADR-005** --- Especialização de tabelas por terminal (tabela
    núcleo `passagem_servico` + tabelas de detalhe
    `passagem_brisamar_detalhe` e `passagem_tecon_detalhe`, relação
    um-para-um).
-   **ADR-006** --- GitHub Flow como estratégia de branching (branch por
    tarefa, nomeada `<tipo>/<descricao>`, integrada via Pull Request).

## 7. Decisões descartadas conscientemente (não sugerir de novo sem novo contexto)

-   Modo offline --- descartado, conectividade sempre garantida no
    momento do preenchimento (fim de turno).
-   Alerta de capacidade por linha --- descartado, capacidade varia por
    tipo de vagão.
-   Rastreamento individual de EOTs --- descartado, texto livre é
    suficiente.
-   Vínculo formal em banco entre passagem de TECON e passagem de
    Brisamar --- descartado, exigência de assinatura conjunta era só do
    processo em papel. Em compensação, a tela de confirmação de envio
    (Tela 8, Fase 6) oferece encadeamento manual entre os dois
    terminais, já que ambos costumam ser atendidos no mesmo turno.
-   Múltiplos usuários autenticados por passagem --- descartado, apenas
    1 responsável loga por turno.
-   Reaproveitar senha do portal corporativo para login no RailOps ---
    descartado por risco de segurança (ver ADR-002).
-   Salvamento de rascunho no formulário de preenchimento ---
    descartado, preenchimento é sempre feito de uma vez, ao término do
    turno.
-   Git Flow e Trunk-Based Development como estratégia de branching ---
    descartados em favor do GitHub Flow (ver ADR-006).

## 8. Modelo de dados (resumo --- ver `05-modelagem-banco.md` para detalhes completos)

Tabelas: `usuario`, `passagem_servico` (núcleo),
`passagem_servico_historico` (auditoria, `jsonb`),
`passagem_brisamar_detalhe`, `passagem_tecon_detalhe` (com
`houve_atendimento`, `area1_atendida`, `area2_atendida` independentes),
`linha` (catálogo fixo), `passagem_linha_ocupacao`, `equipe_membro`,
`radio` (catálogo), `passagem_radio_uso`.

## 9. Backlog (ver `08-backlog.md` para detalhes completos)

Organizado em **fatiamento vertical**, 9 Épicos, cada um entregando uma
funcionalidade completa de ponta a ponta antes do próximo:

0.  Fundação do Projeto (estrutura de pastas, conexão DB, Alembic, seed,
    health check, proteção de branch) --- **✅ CONCLUÍDO** (ver seção
    12)
1.  Autenticação (UC01) --- **✅ CONCLUÍDO** (ver seção 20)
2.  Preenchimento --- Núcleo + Brisamar (UC02) --- **PRÓXIMO**
3.  Preenchimento --- TECON (UC02, RN10)
4.  Edição e Histórico (UC04)
5.  Consulta (UC03)
6.  Exportação (UC05)
7.  Diagrama de Manobras (UC06)
8.  Relatório de Falhas por Rádio (UC07)

## 10. Ambiente já instalado

Python 3.13 (comando `python`), VS Code instalado mas **não usado** (PO
trabalha via terminal), Git. Ambiente virtual em
`railops-app/backend/venv` (nota: verificar se venv está em
`backend/venv` ou `railops-app/venv` --- ver seção 12, checar antes de
assumir). PostgreSQL não instalado localmente --- banco é 100% Supabase
(nuvem).

## 11. Pendências conhecidas (não esquecer, mas não é bloqueante agora)

-   **Health check real**: o Épico 0 previa um health check como item da
    Fundação do Projeto. Ele **ainda não foi implementado como rota**
    (só existiu um script de teste manual de conexão, já removido de
    propósito --- ver seção 12). Quando for a hora de implementar, deve
    ser uma rota FastAPI (ex.: `GET /health`), não um script solto.
-   **Alembic --- resolvido:** configuração e primeira migration já
    existiam; a migration `7cfb74542084` foi criada e aplicada durante o
    fechamento do Épico 1. Manter migrations versionadas em todos os
    próximos incrementos que alterarem o schema.

------------------------------------------------------------------------

## 12. Progresso da Implementação --- Épico 0 (CONCLUÍDO)

### Ambiente de desenvolvimento configurado

-   Python 3.13 confirmado como comando padrão (`python`, não `py` ---
    máquina tem as duas versões instaladas; `python` foi escolhido por
    consistência).
-   Ambiente virtual criado, ativado via `.\venv\Scripts\Activate.ps1`
    (funciona sem bloqueio de política de execução nesta máquina).
-   Bibliotecas instaladas: `fastapi`, `uvicorn`, `sqlalchemy`,
    `alembic`, `psycopg2-binary`, `python-dotenv`. Registradas em
    `backend/requirements.txt` via `pip freeze`.

### Estrutura de pastas do backend --- CONCLUÍDO (via PR #1, mesclado)

Estrutura `backend/app/{routers,services,repositories,models,core}`
criada e mesclada na `main` do `railops-app` através do primeiro Pull
Request do projeto (branch `chore/estrutura-inicial-backend`), seguindo
o GitHub Flow (ADR-006). Cada pasta contém um `__init__.py` vazio.

### Banco de dados Supabase --- CRIADO

-   Projeto criado do zero: nome `railops`, região **South America (São
    Paulo)**, plano Free.
-   **Data API desabilitada deliberadamente** (Enable Data API,
    Automatically expose new tables, Enable automatic RLS --- todos
    desmarcados na criação), pois o projeto não usa bibliotecas cliente
    Supabase (supabase-js): todo acesso ao banco passa exclusivamente
    pelo SQLAlchemy dentro do FastAPI.
-   **Método de conexão: Session Pooler** (não "Direct Connection") ---
    a conexão direta exige IPv6, que a rede do Product Owner não
    suporta; o Session Pooler faz proxy IPv4 gratuito. Host:
    `aws-0-sa-east-1.pooler.supabase.com`, porta `5432`, usuário no
    formato `postgres.<referência-do-projeto>`.
-   String de conexão salva em `backend/.env` (variável `DATABASE_URL`),
    arquivo corretamente ignorado pelo `.gitignore` --- nunca
    versionado.
-   **Atenção de segurança:** a senha original do banco foi exibida em
    um print compartilhado durante o desenvolvimento; recomendou-se
    resetar a senha do banco no painel do Supabase (botão "Reset
    database password") como precaução. Confirmar com o Product Owner se
    isso já foi feito antes de prosseguir com qualquer coisa sensível.

### Conexão com o banco --- CONFIGURADA E VALIDADA

`backend/app/core/database.py` criado com `engine` (SQLAlchemy, gerencia
a conexão de baixo nível), `SessionLocal` (fábrica de sessões por
requisição), `Base` (classe base declarativa para os futuros models) e
`get_db()` (dependência do FastAPI para injetar sessões nas rotas).

Conexão testada manualmente com sucesso (script descartável
`testar_conexao.py`, executando `SELECT 1` via `engine.connect()`,
usando `text()` do SQLAlchemy para SQL literal). Funcionou de primeira.
Script removido do repositório após a validação, por não fazer parte da
aplicação em si (não deve virar artefato permanente --- ver pendência do
health check na seção 11).

### Branch mesclada --- PR #2

`chore/configura-conexao-banco` → aberta, sem conflitos, mesclada na
`main` via Pull Request #2 no GitHub, branch remota e local deletadas
após o merge. `git pull` na `main` local trouxe as mudanças com
fast-forward simples.

### Lições de ambiente registradas (para evitar repetir os mesmos problemas)

1.  **OneDrive sincronizando a pasta do projeto pode travar comandos
    Git** que criam/apagam muitos arquivos ao mesmo tempo (ex.:
    `git checkout` trocando de branch). Sintoma:
    `"Deletion of directory '...' failed.    Should I try again? (y/n)"`
    repetidamente. Solução: pausar a sincronização do OneDrive (ícone da
    bandeja do sistema) antes de operações Git que mexem em muitos
    arquivos, reativar depois. **Atualização:** esse sintoma específico
    já ocorreu uma vez mesmo com o OneDrive pausado --- nesse caso, a
    causa não foi o OneDrive, e a solução foi simplesmente responder `n`
    (não tentar de novo) a cada prompt de exclusão até o Git terminar a
    operação sozinho; o `checkout` completou corretamente mesmo
    recusando as exclusões, sem perda de arquivos reais. Se acontecer de
    novo, responder `n` em sequência até o prompt limpar é uma solução
    válida e seguem, não é preciso decidir a causa raiz na hora.
2.  **Um `git checkout` interrompido no meio (por causa do problema
    acima) pode deixar a branch local desatualizada em relação ao
    GitHub**, mesmo que um `git pull` pareça ter sido executado depois.
    Se algo parecer "faltando" depois de uma troca de branch turbulenta,
    rodar `git fetch origin` seguido de `git log --oneline origin/main`
    para comparar a verdade do servidor com o estado local, em vez de
    presumir. Se a branch local estiver atrasada e sem conflitos reais,
    `git merge origin/main` (ou `git pull`) resolve com um fast-forward
    simples.
3.  **O Bloco de Notas do Windows pode adicionar `.txt` ao final de
    arquivos sem extensão reconhecida** (como `.env`), sem aviso claro.
    Isso já causou a perda aparente (mas não real) de uma configuração
    de `.env`. Solução adotada: sempre criar/editar arquivos via
    terminal (PowerShell), nunca via "Novo arquivo" do Explorador do
    Windows nem Bloco de Notas. Para criar arquivos com conteúdo
    multilinha direto pelo PowerShell, usar here-strings:
    `@'...conteúdo...'@ | Out-File    -FilePath nome.py -Encoding utf8`.
4.  Ao investigar arquivos "sumidos", usar
    `Get-ChildItem <pasta> -Force    -File` em vez de `dir`, pois é mais
    confiável para mostrar arquivos ocultos/com extensão inesperada no
    Windows.
5.  **Nunca digitar um novo comando enquanto o terminal ainda espera
    resposta a um prompt interativo anterior** (ex.: o `(y/n)` do Git
    durante um checkout travado) --- o texto digitado é interpretado
    como resposta ao prompt pendente, não como um novo comando, e pode
    encadear outras perguntas inesperadas. Sempre resolver o prompt
    pendente primeiro (responder y ou n), só então digitar o próximo
    comando.

------------------------------------------------------------------------

## 13. Convenções já estabelecidas para o projeto

-   Commits seguem **Conventional Commits** (`docs:`, `feat:`, `fix:`,
    `refactor:`, `test:`, `chore:`). Já usado na prática: `docs:` para
    atualização de documentação, `feat:` para entrega de funcionalidade
    (ex.: a configuração de conexão com banco foi classificada como
    `feat`, não `chore`, por entregar comportamento novo ao sistema,
    mesmo a branch tendo nome `chore/...`).
-   Branches seguem **GitHub Flow** (ADR-006):
    `<tipo>/<descricao-curta>`, nascem de `main`, integradas via Pull
    Request revisado antes do merge, deletadas (local e remota) logo
    após o merge.
-   Descrição de PR segue estrutura: **O que foi feito** / **Decisões
    técnicas** / **Validação** / **Próximos passos** --- já usada com
    bons resultados no PR #2.
-   Documentos de fase ficam na raiz do `railops-docs`, numerados
    (`01-`, `02-`, `03-`...).
-   ADRs ficam em `railops-docs/adrs/`, numerados independentemente
    (`001-`, `002-`...).
-   Todo documento novo é apresentado primeiro em texto corrido (ou
    diagrama, quando aplicável) no chat, depois entregue como arquivo
    `.md` para o Product Owner adicionar ao repositório manualmente (a
    IA não tem acesso de rede para fazer push diretamente).
-   Ao alterar um documento já existente (não criar um novo), a IA edita
    o arquivo local e reentrega a versão completa atualizada --- o
    Product Owner sobrescreve o arquivo já existente na pasta local
    antes do commit.
-   Scripts de validação/debug pontuais nunca são commitados --- são
    criados, usados uma vez, e apagados antes do próximo `git add`.

------------------------------------------------------------------------

## 14. Progresso da Implementação --- Épico 1 (Autenticação, EM ANDAMENTO)

### Model `Usuario` --- CONCLUÍDO

Criado `backend/app/models/usuario.py`, herdando de `Base`
(`core/database.py`), usando o estilo moderno do SQLAlchemy
(`Mapped`/`mapped_column`), coerente com a tipagem forte já adotada no
projeto (ADR-004/ADR-005). Campos, todos conferidos contra
`05-modelagem-banco.md`:

``` python
import uuid
from datetime import datetime

from sqlalchemy import String, Boolean, DateTime, func
from sqlalchemy.orm import Mapped, mapped_column
from sqlalchemy.dialects.postgresql import UUID

from app.core.database import Base


class Usuario(Base):
    __tablename__ = "usuario"

    id: Mapped[uuid.UUID] = mapped_column(
        UUID(as_uuid=True), primary_key=True, default=uuid.uuid4
    )
    matricula: Mapped[str] = mapped_column(String, unique=True, nullable=False)
    nome: Mapped[str] = mapped_column(String, nullable=False)
    senha_hash: Mapped[str | None] = mapped_column(String, nullable=True)
    pin_definido: Mapped[bool] = mapped_column(Boolean, nullable=False, default=False)
    criado_em: Mapped[datetime] = mapped_column(
        DateTime(timezone=True), nullable=False, server_default=func.now()
    )
```

Decisão de design registrada: `criado_em` usa
`server_default=func.now()` (o próprio PostgreSQL preenche o timestamp),
não `default=datetime.now()` do lado da aplicação --- evita
inconsistência de fuso/instante entre app e banco.

### Alembic --- CONFIGURADO PELA PRIMEIRA VEZ NO PROJETO (pendência da seção

11 antiga, agora resolvida) - `pip show alembic` confirmou que a lib já
estava instalada (via `requirements.txt` do Épico 0), bastando
inicializar. - `alembic init alembic` rodado dentro de `backend/`,
gerando `alembic.ini`, `alembic/env.py`, `alembic/README`,
`alembic/script.py.mako` e `alembic/versions/`. - `alembic/env.py`
editado em dois pontos: 1. Carrega `DATABASE_URL` do `.env` via
`load_dotenv()` + `config.set_main_option(...)` --- a URL do banco
**nunca** é escrita no `alembic.ini` (que é versionado), só lida em
tempo de execução do `.env` (que é ignorado pelo Git). Checagem de
segurança feita manualmente no diff do PR antes do merge. 2. Importa
`Base` (`core/database.py`) e `Usuario` (`app/models/usuario.py`) e
define `target_metadata = Base.metadata`, habilitando o
`--autogenerate`. - Primeira migration gerada:
`alembic revision --autogenerate -m "cria   tabela usuario"` → arquivo
`alembic/versions/5fcddf9283ef_cria_tabela_usuario.py`, revisado
manualmente antes de aplicar (conferido: colunas, tipos, constraints,
`server_default`, tudo batendo com o model). - Aplicada com
`alembic upgrade head` --- criou a tabela `usuario` de fato no Supabase.
Validado visualmente no Table Editor do Supabase: 6 colunas certas,
tipos certos. - A tabela auxiliar `alembic_version` apareceu
automaticamente no banco (controle interno do próprio Alembic, não é uma
tabela de domínio do projeto --- não confundir com tabelas de
`05-modelagem-banco.md`).

### Lição de ambiente registrada nesta sessão

O `venv` do projeto está em `railops-app/venv` (raiz do repositório
`railops-app`), **não** em `railops-app/backend/venv` como a Fase 8/9
anterior deixava em aberto como dúvida. Confirmado via
`Get-ChildItem -Force` em ambos os níveis. Ativação a partir de
`backend`, portanto, usa caminho relativo subindo um nível:
`..\venv\Scripts\Activate.ps1`. Não foi movido, apenas documentado ---
avaliar depois, sem pressa, se vale mover para dentro de `backend` por
organização (mais comum no mercado ter o venv junto do
`requirements.txt`).

Também vale registrar: uma sessão nova do PowerShell nunca começa com o
venv ativado (isso não é uma configuração permanente da pasta) ---
checar sempre com `Get-Command python` (deve apontar para dentro de
`...\venv\Scripts\...`) antes de instalar ou rodar qualquer coisa
sensível a ambiente, para não repetir o susto de instalar algo no Python
global do Windows por engano.

### Branch, PR e merge --- CONCLUÍDO (PR #3)

-   Detectado que o trabalho tinha sido iniciado (model + Alembic)
    direto na `main` local, por descuido. Corrigido sem perda:
    `git checkout -b   feature/model-usuario-alembic` a partir do estado
    com mudanças ainda não commitadas --- o Git levou as mudanças para a
    branch nova e devolveu a `main` limpa.
-   `.gitignore` do projeto (template padrão GitHub para Python) já
    cobre `__pycache__/` e `.env` --- confirmado que nenhum arquivo de
    cache ou credencial entrou no commit.
-   Commit único
    (`feat: cria model Usuario e configura Alembic com   primeira migration`)
    --- decisão consciente de não separar model e Alembic em dois
    commits, pois um não é testável sem o outro nesta entrega.
-   PR #3 aberto, descrição no padrão já estabelecido (O que foi feito /
    Decisões técnicas / Validação / Próximos passos), auto-revisado
    (conferência linha a linha do diff, com atenção especial a
    `alembic.ini` para garantir que nenhuma credencial vazou) e mesclado
    na `main` do `railops-app`.
-   Sincronização local pós-merge: `git checkout main` + `git pull`
    (Fast-forward `9a014c9..4fb05dc`) +
    `git branch -d   feature/model-usuario-alembic`. Ciclo completo,
    igual ao PR #2.

### Próximo passo real --- Repository de usuário

Ainda **não iniciado** na prática. Conforme o backlog do Épico 1
(`08-backlog.md`): - Repository de usuário: buscar por matrícula, criar,
atualizar PIN. - Em seguida: Service de autenticação (hash de PIN via
`passlib`, geração e validação de JWT --- ADR-002), depois rotas, tela
de login e testes.

Antes de escrever código, a IA deve explicar o papel exato da camada de
Repository (o que pode e não pode fazer) e por que regra de negócio
(ex.: hashear o PIN antes de salvar) não pertence a essa camada, e sim à
camada de Service que vem em seguida --- reforçando a separação de
responsabilidades do ADR-003.

Expectativa de esforço combinada com o Product Owner: esta próxima etapa
deve ser mais rápida que a anterior, pois toda a infraestrutura (venv,
Alembic, conexão com banco) já está resolvida --- o trabalho agora é
majoritariamente escrever código de poucas funções, sem a fase de
"descoberta e correção de ambiente" que consumiu boa parte da sessão
anterior.

## 15. Novo processo adotado --- Issues do GitHub (a partir desta sessão)

Por recomendação de um Software Architect externo (amigo sênior do
Product Owner) em uma sessão de revisão ao vivo do projeto, passou a ser
seguido um fluxo de trabalho rastreável via GitHub Issues, além do
GitHub Flow já em uso (ADR-006):

-   Antes de iniciar qualquer entrega de código, abrir uma Issue no
    repositório `railops-app` descrevendo objetivo, escopo (checklist) e
    o que fica fora de escopo, com rótulo `enhancement` para novas
    funcionalidades.
-   Ao abrir o PR correspondente, incluir `Closes #N` na primeira linha
    da descrição --- isso faz o GitHub reconhecer o vínculo
    automaticamente (visível na barra lateral do PR) e fechar a Issue
    sozinho assim que o PR é mesclado. Validado funcionando de ponta a
    ponta com a Issue #4 → PR #5 (repository) e Issue #6 → PR #7
    (service de autenticação).
-   Outras duas observações do mesmo Software Architect já estavam
    cobertas sem esforço adicional: a estrutura de pastas em camadas
    (models/repositories/services/routers) já seguida desde a ADR-003, e
    a documentação Swagger/OpenAPI, que o FastAPI gera automaticamente
    na rota `/docs` assim que existirem rotas reais --- ainda não
    aplicável, pois o Épico 1 ainda não chegou à camada de routers.
-   Daqui em diante, toda nova entrega do backlog deve abrir sua Issue
    correspondente antes do código, seguindo esse mesmo padrão.

## 16. Progresso da Implementação --- Épico 1 (Autenticação), continuação

### Repository de usuário --- CONCLUÍDO (Issue #4 → PR #5)

Criado `backend/app/repositories/usuario_repository.py`. Classe
`UsuarioRepository`, recebendo a `Session` do banco via injeção de
dependência no construtor (não cria sua própria conexão --- facilita
testes futuros com banco mock). Três métodos, todos sem nenhuma regra de
negócio (fronteira reforçada explicitamente com o Product Owner, ver
ADR-003):

``` python
class UsuarioRepository:
    def __init__(self, db: Session):
        self.db = db

    def buscar_por_matricula(self, matricula: str) -> Usuario | None: ...
    def criar(self, usuario: Usuario) -> Usuario: ...
    def atualizar_pin(self, usuario: Usuario, novo_pin_hash: str) -> Usuario: ...
```

Validado com script manual descartável (`teste_manual.py`, criado, usado
e apagado dentro da mesma sessão --- não deve ser recriado como parte
permanente do projeto) contra o banco real do Supabase: criação, busca
por matrícula existente, busca por matrícula inexistente (retorno
`None`) e atualização de PIN. Todos os registros de teste foram
removidos do banco manualmente após a validação --- este é o padrão a
repetir em toda validação manual futura (nunca deixar dado de teste no
banco real).

### Service de autenticação --- CONCLUÍDO (Issue #6 → PR #7)

Criado `backend/app/services/auth_service.py`. Classe `AuthService`,
recebendo um `UsuarioRepository` via injeção de dependência (nunca
acessa `Session`/banco diretamente --- toda leitura/escrita passa pelo
repository). Exceção própria `AutenticacaoError` para os casos de
negócio inválidos. Dois métodos públicos:

-   `primeiro_acesso(matricula, pin)`: valida que a matrícula existe e
    que o PIN ainda não foi definido, gera hash do PIN via `passlib`
    (bcrypt) e persiste via repository.
-   `login(matricula, pin)`: valida existência, valida que o PIN já foi
    definido, verifica o PIN contra o hash salvo (`pwd_context.verify`,
    sem nunca descriptografar) e, se válido, emite um token JWT
    (`python-jose`) válido por 8 horas (duração de um turno), assinado
    com `JWT_SECRET_KEY`.

Novas dependências instaladas: `passlib[bcrypt]`,
`python-jose[cryptography]`. `requirements.txt` atualizado via
`pip freeze` **duas vezes** nesta sessão (ver lição de troubleshooting
abaixo --- a segunda vez foi necessária por causa da fixação de versão
do bcrypt).

Nova variável de ambiente adicionada ao `.env` (nunca commitada, segue a
mesma regra do `DATABASE_URL`): `JWT_SECRET_KEY`, gerada com
`python -c "import secrets; print(secrets.token_hex(32))"`. Não
reproduzir o valor real deste checkpoint nem em nenhum outro lugar
versionado --- se precisar regenerar, gerar uma nova chave com o mesmo
comando (isso invalidaria tokens já emitidos, o que é aceitável nesta
fase pré-produção).

Validado com o mesmo padrão de script manual descartável: primeiro
acesso, login com PIN correto (token gerado), login com PIN errado
(bloqueado com `AutenticacaoError`), login em matrícula sem PIN definido
(bloqueado). Todos os cenários passaram após a correção de
compatibilidade abaixo.

### Lição de troubleshooting registrada nesta sessão --- incompatibilidade passlib/bcrypt

Ao instalar `passlib[bcrypt]` sem fixar versão, o pip trouxe
`bcrypt==5.0.0`, que **não é compatível** com `passlib==1.7.4`: a partir
do bcrypt 4.x, o pacote removido o atributo interno
`__about__.__version__` que o passlib usa para detectar a versão do
backend, causando `AttributeError` em cascata que se manifesta como um
erro de aparência confusa
(`ValueError: password cannot be longer than 72 bytes`) vindo de um
autoteste interno do passlib --- não é um problema com o PIN em si.
Correção aplicada: fixar `pip install bcrypt==4.0.1` (downgrade
explícito). Se o ambiente for recriado do zero no futuro
(`pip install -r requirements.txt`), o arquivo já está corrigido e vai
instalar a versão certa automaticamente --- mas caso alguém rode
`pip install --upgrade bcrypt` manualmente por qualquer motivo, este
problema pode voltar a acontecer.

### Branches, Issues e PRs desta sessão --- CONCLUÍDO

-   Issue #4 → branch `feature/repository-usuario` → commit único → PR
    #5 (`Closes #4`) → mesclado → Issue fechada automaticamente →
    sincronização local (`checkout main` + `pull` + `branch -d`).
-   Issue #6 → branch `feature/service-autenticacao` → commit único → PR
    #7 (`Closes #6`) → mesclado → Issue fechada automaticamente →
    sincronização local, mesmo padrão.
-   Ambos os ciclos seguiram a mesma rotina de limpeza pré-commit:
    apagar dado de teste do Supabase e apagar o script `teste_manual.py`
    antes de `git add`, para nunca commitar sujeira de validação manual.

### Próximo passo real --- Rotas HTTP (routers)

Ainda **não iniciado**. Conforme o backlog do Épico 1
(`08-backlog.md`): - `POST /auth/primeiro-acesso` - `POST /auth/login` -
Ambas as rotas devem instanciar `UsuarioRepository` e `AuthService`
(usando `get_db()` de `core/database.py` via injeção de dependência do
FastAPI, com `Depends`), capturar `AutenticacaoError` e traduzir para
respostas HTTP apropriadas (ex.: 401 para credenciais inválidas, 400
para erros de validação). - Assim que a primeira rota existir, a
documentação Swagger/OpenAPI fica disponível automaticamente em `/docs`
--- não requer nenhuma configuração adicional (ver seção 15). - Antes de
codar, seguir o novo processo: abrir Issue no `railops-app` descrevendo
objetivo/escopo, rótulo `enhancement`, branch
`feature/rotas-autenticacao` (ou nome equivalente a combinar),
`Closes   #N` no PR.

## 17. Progresso da Implementação --- Épico 1 (Autenticação), rotas HTTP

### Rotas de autenticação --- CONCLUÍDO (Issue #8 → PR #9)

Criados `backend/app/schemas/auth_schema.py` (schemas Pydantic
`PrimeiroAcessoRequest`, `LoginRequest`, `LoginResponse`) e
`backend/app/routers/auth_router.py` (`APIRouter` com prefixo `/auth`).

Duas rotas implementadas, cada uma instanciando `UsuarioRepository` e
`AuthService` via `Depends(get_db)`: - `POST /auth/primeiro-acesso` ---
captura `AutenticacaoError` e retorna 400 - `POST /auth/login` ---
captura `AutenticacaoError` e retorna 401, devolve o token JWT em caso
de sucesso

Criado também `backend/main.py`, o entrypoint da aplicação
(`FastAPI(title="RailOps API")` + `include_router`) --- não existia
nenhum arquivo instanciando `FastAPI()` no projeto até este ponto. A
partir da criação da primeira rota real, a documentação Swagger/OpenAPI
passou a ficar disponível em `/docs` automaticamente, como previsto.

### Validação manual --- CONCLUÍDA

Testado via Swagger UI, contra o banco real do Supabase, com o servidor
rodando via `uvicorn main:app --reload`: - ✅ Primeiro acesso com
sucesso (201) - ✅ Primeiro acesso duplicado, PIN já definido (400) - ✅
Login com PIN correto (200, token JWT gerado) - ✅ Login com PIN
incorreto (401)

Matrícula de teste usada: `30032552` (matrícula real do Product Owner),
inserida via script descartável (criado e apagado na mesma sessão,
seguindo o padrão já estabelecido). Diferente das validações anteriores,
este registro **não foi removido do banco** --- foi uma decisão
deliberada do Product Owner, já que é sua matrícula real e será
reaproveitada como usuário de teste contínuo do projeto daqui em diante.
Isso é uma exceção documentada à regra geral de "nunca deixar dado de
teste no banco real" (seção 1) --- válida apenas para este registro
específico.

### Branches, Issues e PRs desta sessão --- CONCLUÍDO

Issue #8 → branch `feature/rotas-autenticacao` → commit único → PR #9
(`Closes #8`) → mesclado → Issue fechada automaticamente → sincronização
local (`checkout main` + `pull`, fast-forward `03e9f15..45c4f1e`) +
`branch -d`. Mesmo padrão dos ciclos anteriores.

### Próximo passo real --- Tela de login (frontend)

Ainda **não iniciado**. Conforme o backlog do Épico 1 (`08-backlog.md`),
falta a última fatia vertical: a tela de login consumindo as rotas
`/auth/primeiro-acesso` e `/auth/login` recém implementadas. Isso fecha
o Épico 1 por completo (banco → repository → service → rota → teste →
tela) antes de avançar ao Épico 2.

Alternativamente, o Product Owner pode optar por deixar as telas de
todos os épicos para uma fase posterior e avançar direto para o Épico 2
no backend --- essa decisão ainda não foi tomada e deve ser retomada no
início da próxima sessão.

## 18. Progresso da Implementação --- Épico 1 (Autenticação), frontend em andamento

### Issue e branch da entrega

-   Issue **#10 --- Implementar tela de login e primeiro acesso** criada
    no repositório `railops-app`, seguindo o processo adotado a partir
    da seção 15.
-   Branch criada a partir da `main` limpa e sincronizada:
    `feature/tela-login`.
-   O trabalho desta sessão ainda **não foi commitado** nem enviado para
    PR.
-   Próximo PR deverá incluir `Closes #10` na primeira linha da
    descrição.

### Estrutura inicial do frontend --- CRIADA

Até esta sessão, o repositório possuía apenas `backend/`. Foi iniciada a
estrutura real do frontend:

``` text
railops-app/
├── backend/
└── frontend/
    ├── index.html
    ├── css/
    │   └── styles.css
    └── js/
        └── login.js
```

A decisão anterior de usar **HTML/CSS no frontend** foi recuperada em
`06-prototipos.md`, que registra explicitamente que os mockups de média
fidelidade foram feitos em HTML/CSS para poderem evoluir diretamente
para componentes reais na Fase 9. Não introduzir React/Vite ou outro
framework sem nova decisão arquitetural formal.

### Ambiente frontend confirmado

-   Node.js: `v24.18.0`
-   npm: `11.16.0`
-   Frontend servido localmente com: `npx serve .\frontend`
-   URL local usada durante os testes: `http://localhost:3000`
-   Observação de ambiente: enquanto `npx serve .\frontend` estava em
    execução, houve um caso em que o Windows bloqueou a sobrescrita de
    `frontend/js/login.js` com `Set-Content`. A solução prática foi
    parar temporariamente o `serve` com `Ctrl + C`, editar o arquivo e
    iniciar o servidor novamente. Se ocorrer de novo, repetir esse
    procedimento antes de investigar causas mais profundas.

### Tela de login --- ESTRUTURA E ESTILO IMPLEMENTADOS

`frontend/index.html` criado com: - título RailOps; - subtítulo
"Passagem de Serviço Ferroviária"; - campo Matrícula; - campo PIN; -
botão Entrar; - ação "Primeiro acesso? Definir meu PIN"; - área de
mensagem com `role="alert"`; - referência a `./css/styles.css`; -
referência a `./js/login.js`.

`frontend/css/styles.css` criado com uma identidade visual inicial: -
visual corporativo/operacional; - fundo claro; - cartão centralizado; -
cor primária azul escuro; - campos com foco visível; - botão Entrar como
ação primária; - primeiro acesso como ação secundária; - classes de
mensagem de erro e sucesso; - ajuste responsivo básico para telas
menores.

Essa identidade ainda não é branding oficial da MRS; foi tratada apenas
como identidade inicial do RailOps, já que os documentos não registravam
paleta visual definitiva.

### Integração frontend → backend --- FUNCIONAL

O JavaScript foi evoluído incrementalmente até consumir de fato:

`POST http://127.0.0.1:8000/auth/login`

Fluxo validado: - captura matrícula e PIN do formulário; - usa `fetch()`
com `Content-Type: application/json`; - envia JSON no contrato já
existente da API; - trata respostas de sucesso e erro; - exibe mensagem
diretamente na tela; - guarda o JWT em `sessionStorage` com a chave
`access_token`; - PIN e token não permanecem sendo exibidos em
`console.log` (logs foram usados apenas durante validação pontual e
depois removidos).

Primeiro login real ponta a ponta validado com sucesso:

`HTML → JavaScript → CORS → FastAPI → AuthService → UsuarioRepository → Supabase → JWT → frontend`

### CORS --- CONFIGURADO NO BACKEND

`backend/main.py` foi atualizado para usar `CORSMiddleware`, permitindo
apenas as origens locais de desenvolvimento:

``` text
http://localhost:3000
http://127.0.0.1:3000
```

Não foi usado `allow_origins=["*"]`, por ser permissivo demais para uma
API de autenticação.

Configuração atual inclui: - `allow_methods=["*"]` -
`allow_headers=["*"]`

Quando houver deploy, adicionar a URL real do frontend à lista de
origens permitidas.

### Validação de matrícula e PIN --- FRONTEND E BACKEND

Decisão tomada nesta sessão: - matrícula: **exatamente 8 dígitos** -
PIN: **exatamente 4 dígitos**

No frontend, os inputs foram atualizados com: - `inputmode="numeric"` -
`minlength` - `maxlength` - `pattern` - `title`

Regras: - matrícula: `[0-9]{8}` - PIN: `[0-9]{4}`

Testes manuais no navegador: - matrícula com 7 dígitos: bloqueada; -
matrícula com 9º dígito: 9º caractere impedido; - PIN com 5º dígito: 5º
caractere impedido; - PIN com 3 dígitos: bloqueado.

No backend, `backend/app/schemas/auth_schema.py` foi atualizado para
usar `Field(pattern=...)` do Pydantic:

``` python
matricula: str = Field(pattern=r"^\d{8}$")
pin: str = Field(pattern=r"^\d{4}$")
```

Aplicado tanto a `PrimeiroAcessoRequest` quanto a `LoginRequest`.

Validação direta via Swagger: - matrícula com 7 dígitos → HTTP 422; -
PIN com 3 dígitos → HTTP 422.

Isso confirma RNF06: validação no backend, não apenas no frontend.

### Segurança do login --- ENUMERAÇÃO DE USUÁRIOS CORRIGIDA

Durante os testes foi identificado que o login retornava mensagens
diferentes para: - matrícula inexistente → "Matrícula não cadastrada." -
matrícula existente + PIN errado → "PIN incorreto." - matrícula
existente sem PIN → "PIN ainda não foi definido para esta matrícula."

Isso permitia inferir se uma matrícula existia no sistema (enumeração de
usuários).

Decisão aplicada **somente ao fluxo de login**: todos esses cenários
agora retornam:

`Matrícula ou PIN inválidos.`

O método `primeiro_acesso()` foi mantido sem alteração nesta sessão,
pois seu fluxo de segurança precisa ser discutido separadamente.

Durante essa correção houve um erro de digitação temporário:
`AutenticacaoErro` foi usado no lugar da classe real
`AutenticacaoError`, gerando `NameError`, HTTP 500 e uma mensagem
aparente de CORS no navegador. A causa raiz foi identificada pelo
traceback do Uvicorn e corrigida. Lição reforçada: quando o navegador
mostrar CORS junto com HTTP 500, verificar primeiro o traceback do
backend antes de concluir que CORS é a causa.

Testes finais: - matrícula inexistente + PIN qualquer → "Matrícula ou
PIN inválidos." - matrícula válida + PIN incorreto → mesma mensagem; -
matrícula válida + PIN correto → login realizado com sucesso.

### Primeiro acesso --- AINDA NÃO IMPLEMENTADO NO FRONTEND

A ação "Definir meu PIN" já existe visualmente no HTML, mas ainda não
possui comportamento JavaScript.

Próximo passo real ao retomar: 1. revisar o fluxo de segurança do
primeiro acesso; 2. decidir como evitar que uma pessoa informe a
matrícula de outro colaborador e defina o PIN dele; 3. implementar o
comportamento da ação "Definir meu PIN" consumindo
`POST /auth/primeiro-acesso`; 4. validar PIN e confirmação de PIN; 5.
testar sucesso e erros; 6. só então considerar o Épico 1 fechado de
ponta a ponta.

### Estado da sessão ao pausar

-   Frontend parado com `Ctrl + C`.
-   Backend/Uvicorn parado com `Ctrl + C`.
-   Branch atual de trabalho: `feature/tela-login`.
-   Alterações desta sessão ainda não commitadas.
-   `railops-docs` havia sido atualizado e publicado antes do início
    desta implementação, com commit de checkpoint anterior já enviado à
    `main`.
-   Ao retomar, executar primeiro `git status` em `railops-app` para
    conferir a working tree antes de qualquer novo comando.

## 19. Continuação --- Primeiro Acesso e preparação para continuidade no Codex

### Estado ao retomar

O desenvolvimento foi retomado no repositório `railops-app` e o Git
confirmou:

``` text
On branch feature/tela-login
Your branch is up to date with 'origin/feature/tela-login'.
nothing to commit, working tree clean
```

Portanto: - branch atual: `feature/tela-login`; - branch sincronizada
com `origin/feature/tela-login`; - nenhum trabalho anterior foi
perdido; - o commit de checkpoint funcional do login permanece publicado
na branch remota.

Backend e frontend foram iniciados novamente para continuidade: -
backend: `uvicorn main:app --reload` - frontend: `npx serve .\frontend`

### Revisão do frontend de Primeiro Acesso

Foi revisado `frontend/index.html`.

Já existe o botão:

``` html
<button type="button" id="primeiro-acesso">
    Definir meu PIN
</button>
```

Também foi revisado integralmente `frontend/js/login.js`.

Situação confirmada: - login normal já possui integração real com
`POST /auth/login`; - token JWT de login bem-sucedido é armazenado em
`sessionStorage`; - erros são exibidos na própria tela; - o botão
`#primeiro-acesso` ainda NÃO possui `addEventListener`; - o fluxo de
Primeiro Acesso ainda NÃO foi implementado no frontend.

### Problema de segurança identificado no Primeiro Acesso

A implementação atual do backend permite conceitualmente definir o
primeiro PIN a partir da matrícula e do novo PIN.

Como os colaboradores autorizados serão pré-cadastrados no banco, foi
identificado um risco: conhecer apenas a matrícula de um colaborador que
ainda não definiu PIN não deve ser suficiente para uma pessoa definir o
PIN daquela conta.

Portanto, antes de conectar o botão `Definir meu PIN` diretamente à rota
existente, foi decidido criar uma validação adicional para provar que o
colaborador está autorizado a ativar aquela matrícula.

### Alternativas consideradas

Foram discutidas duas alternativas:

1.  validação por e-mail;
2.  código de ativação de uso único.

A validação por e-mail é tecnicamente viável e existem serviços com
faixas gratuitas, porém adicionaria: - integração com serviço externo; -
configuração de remetente/domínio para cenário real; - infraestrutura
adicional; - mais tempo dedicado a uma parte que não é o problema
operacional central do RailOps.

Foi considerado que, na apresentação ao gestor, o principal valor do
projeto será a solução do problema real da passagem de serviço
ferroviária, e não a sofisticação do mecanismo de login.

### Decisão atual --- código de ativação de uso único

Foi escolhida a abordagem de **código de ativação de uso único**, sem
dependência de e-mail ou SMS.

Objetivos: - custo zero; - implementação simples; - nenhuma dependência
externa; - segurança suficiente para impedir definição de PIN conhecendo
somente a matrícula; - manter o foco de desenvolvimento nas
funcionalidades operacionais centrais do RailOps.

Fluxo planejado:

``` text
Colaborador é pré-cadastrado/autorizado
        ↓
É associado um código de ativação
        ↓
Primeiro acesso
        ↓
Matrícula (8 dígitos)
Código de ativação (6 dígitos)
Novo PIN (4 dígitos)
Confirmação do PIN
        ↓
Backend valida matrícula + código
        ↓
Backend salva o hash do PIN
pin_definido = true
código de ativação é invalidado
        ↓
Próximos acessos
        ↓
Matrícula + PIN
```

### Regras definidas para o código de ativação

-   código com **6 dígitos**;
-   uso único;
-   não armazenar o código em texto puro;
-   armazenar apenas o hash;
-   após ativação bem-sucedida, invalidar o código;
-   não implementar expiração automática neste momento.

A expiração foi considerada, inicialmente com ideia de 24 horas, mas foi
retirada do escopo atual porque exigiria também um mecanismo de
regeneração/gestão de códigos. Isso poderá ser evoluído futuramente
quando houver módulo administrativo.

### Modelagem revisada

Foi revisado:

`railops-docs/05-modelagem-banco.md`

A documentação confirma a estratégia já existente: - usuários são
pré-cadastrados administrativamente; - `senha_hash` pode começar nulo; -
`pin_definido` começa falso; - o PIN é definido no primeiro acesso.

Também foi revisado o model real:

`railops-app/backend/app/models/usuario.py`

Estrutura atual observada:

``` text
id
matricula
nome
senha_hash
pin_definido
criado_em
```

### Alteração planejada no model

A alteração mínima definida para `Usuario` é adicionar:

``` python
codigo_ativacao_hash: Mapped[str | None] = mapped_column(String, nullable=True)
```

Não será criada uma tabela separada apenas para o código de ativação
neste momento.

**IMPORTANTE: esta alteração ainda NÃO foi aplicada ao código.**

A sessão foi interrompida justamente antes de modificar
`backend/app/models/usuario.py`.

### Próximo passo exato da implementação

Ao continuar:

1.  conferir `git status` em `railops-app`;
2.  confirmar branch `feature/tela-login`;
3.  adicionar `codigo_ativacao_hash` ao model `Usuario`;
4.  revisar como Alembic/migrations estão configurados atualmente;
5.  criar a migration da nova coluna;
6.  revisar/aplicar a migration no banco de desenvolvimento;
7.  evoluir os schemas de Primeiro Acesso;
8.  evoluir repository/service para validar código de ativação;
9.  ajustar `POST /auth/primeiro-acesso`;
10. implementar o comportamento de `#primeiro-acesso` no frontend;
11. adicionar PIN + confirmação de PIN no fluxo;
12. testar sucesso, código incorreto, matrícula inválida, PIN inválido e
    tentativa de reutilização do código;
13. revisar `git diff`;
14. executar testes;
15. somente depois criar novo commit, mediante autorização.

### Continuidade pelo Codex

Foi decidido continuar a execução do projeto pelo Codex para permitir
acesso direto aos arquivos locais, edição do código, execução de
comandos/testes e revisão de diffs sem necessidade de copiar e colar
cada alteração manualmente.

Workspace recomendado:

``` text
PROJETO RAILOPS/
├── railops-app/
└── railops-docs/
```

Ao iniciar no Codex, ele deve primeiro: 1. ler
`railops-docs/00-checkpoint-continuidade.md`; 2. consultar os demais
documentos relevantes em `railops-docs`; 3. inspecionar o código real em
`railops-app`; 4. conferir branch e `git status`; 5. comparar o
checkpoint com o estado real do repositório; 6. não alterar arquivos
antes dessa conferência.

### Forma de trabalho que deve ser mantida no Codex

Continuar com a mesma dinâmica didática usada até aqui: - uma etapa por
vez; - explicar o objetivo antes de cada alteração; - explicar de forma
simples o que o código faz; - inspecionar o código existente antes de
modificá-lo; - evitar alterações desnecessárias; - preservar arquitetura
e decisões já documentadas; - priorizar tecnologias gratuitas; - testar
cada incremento; - investigar a causa raiz antes de corrigir erros; -
mostrar/revisar o diff antes de commit; - não fazer `git commit`,
`git push`, merge ou abrir PR sem autorização explícita; - atualizar o
checkpoint quando houver uma pausa relevante.

### Ponto exato para amanhã

**Issue #10 → Primeiro Acesso → código de ativação de uso único →
adicionar `codigo_ativacao_hash` ao model `Usuario` → revisar/criar
migration → evoluir backend → implementar frontend.**

Nenhuma alteração referente ao código de ativação foi aplicada ao
`railops-app` até este checkpoint.

------------------------------------------------------------------------

## 20. Fechamento do Épico 1 --- Autenticação (CONCLUÍDO)

### Código de ativação de uso único --- IMPLEMENTADO

O fluxo de primeiro acesso foi protegido com código de ativação de seis
dígitos. O model `Usuario` recebeu `codigo_ativacao_hash`, opcional, e a
migration Alembic `7cfb74542084_adiciona_codigo_de_ativacao_ao_usuario.py`
foi criada, revisada e aplicada no Supabase.

O código nunca é armazenado em texto puro. O backend compara o valor
informado com o hash usando `passlib`/bcrypt. Em uma ativação válida, o
repository salva o hash do novo PIN, define `pin_definido = true` e apaga
`codigo_ativacao_hash` no mesmo commit, garantindo uso único.

Para evitar enumeração de usuários, matrícula inexistente, conta já
ativada, código ausente ou código incorreto retornam a mesma mensagem:
`Dados de ativação inválidos.`

### Backend e contrato HTTP --- CONCLUÍDOS

-   `PrimeiroAcessoRequest` exige matrícula com 8 dígitos, código de
    ativação com 6 dígitos e PIN com 4 dígitos;
-   `AuthService.primeiro_acesso()` valida matrícula, estado da conta e
    hash do código;
-   `UsuarioRepository.ativar_usuario()` persiste o PIN e invalida o
    código atomicamente;
-   `POST /auth/primeiro-acesso` encaminha o novo contrato e responde
    HTTP 201 em caso de sucesso;
-   a migration aplicada deixou o banco na revisão `7cfb74542084 (head)`.

### Testes --- CONCLUÍDOS

Foi adicionada a primeira estrutura automatizada de testes do projeto,
com `pytest==9.1.1`, `pytest.ini` na raiz e
`backend/tests/test_auth_service.py`.

Sete cenários automatizados passam:

1.  ativação bem-sucedida e invalidação do código;
2.  código incorreto;
3.  matrícula inexistente;
4.  usuário já ativado;
5.  tentativa de reutilização do código;
6.  código fora do formato;
7.  PIN fora do formato.

Também foi executado teste HTTP real com usuário fictício descartável:
frontend/HTTP → FastAPI → service → repository → Supabase. A rota
respondeu 201, o banco confirmou PIN definido e código invalidado, e o
registro fictício foi removido ao final. Nenhum script temporário foi
versionado.

### Frontend de primeiro acesso --- CONCLUÍDO

A tela existente passou a alternar entre login e primeiro acesso sem
introduzir framework novo. O modo de ativação contém:

-   matrícula;
-   código de ativação;
-   novo PIN;
-   confirmação do PIN;
-   validação de igualdade dos PINs antes da requisição;
-   integração com `POST /auth/primeiro-acesso`;
-   retorno automático ao modo login após sucesso;
-   bloqueio dos botões durante a requisição para evitar troca de modo e
    mensagens fora de contexto.

O comportamento foi validado no navegador, sem erros no console. O login
normal continua integrado a `POST /auth/login` e armazena o JWT em
`sessionStorage`.

### GitHub Flow --- CONCLUÍDO

-   Issue: **#10 --- Implementar tela de login e primeiro acesso**;
-   branch: `feature/tela-login`;
-   commits principais: `26769db` e `123440b`;
-   PR: **#11 --- feat: implementa login e primeiro acesso**;
-   merge commit: `889c738`;
-   Issue #10 fechada automaticamente por `Closes #10`;
-   `main` local sincronizada com `origin/main` e working tree limpa;
-   branch `feature/tela-login` preservada local e remotamente até
    decisão explícita de limpeza.

### Estado atual e próximo passo real

O **Épico 1 está concluído de ponta a ponta**: banco → repository →
service → rota → testes → tela.

Próximo passo: iniciar o **Épico 2 --- Preenchimento: Núcleo + Brisamar
(UC02)**. Antes de codar:

1.  revisar o escopo do Épico 2 em `08-backlog.md`;
2.  revisar UC02, regras de negócio relacionadas e modelagem das tabelas;
3.  conferir o protótipo das telas envolvidas;
4.  definir a primeira fatia implementável do Épico 2;
5.  abrir nova Issue com rótulo `enhancement`;
6.  criar branch a partir da `main` limpa e sincronizada;
7.  manter o fatiamento vertical e testes passando antes de avançar.

------------------------------------------------------------------------

## 21. Atualização de continuidade --- Épico 2 / Brisamar (13/08/2026)

### Estado da Issue #12

A Issue **#12 --- Implementar passagem de serviço do Pátio Brisamar**
foi implementada na branch `feature/passagem-brisamar`. A branch ainda
não foi publicada e o PR ainda não foi aberto neste checkpoint.

Foram concluídos:

-   validação de JWT para rotas protegidas;
-   modelos e migration das tabelas do núcleo e detalhe Brisamar;
-   seed das linhas 16, 18, 20, 22, 24, 26, 28 e 30;
-   repositories, schemas e service;
-   regra SUP/INF obrigatória apenas para 22 e 24;
-   rota autenticada `POST /passagens/brisamar`;
-   tela de escolha do terminal;
-   formulário completo do Brisamar;
-   envio do formulário com JWT e tratamento de sucesso, validação e
    sessão expirada.

A tela de confirmação/encadeamento para o TECON permanece pendente. Ela
deve ser implementada depois do envio bem-sucedido, sem iniciar ainda o
formulário específico do TECON.

### Banco e migration

A migration `eb6f25372f82_cria_estrutura_da_passagem_brisamar.py` foi
aplicada ao Supabase. O banco ficou na revisão `eb6f25372f82 (head)` e o
seed das oito linhas do Brisamar foi conferido por leitura.

### Validação

-   30 testes automatizados passam;
-   o contrato JSON do frontend foi comparado com
    `PassagemBrisamarRequest`;
-   a aplicação carregou a rota protegida no OpenAPI;
-   um teste HTTP real percorreu Uvicorn → FastAPI → service → repository
    → Supabase e respondeu HTTP 201;
-   o banco confirmou passagem, oito ocupações de linha, equipe, rádio
    utilizado e detalhe Brisamar.

O registro do teste ponta a ponta foi mantido no banco, claramente
identificado pelo turno `TESTE-E2E`, ID
`cd22ddab-cbd4-48ad-9220-4d01a448cf0a`.

### Commits da branch de implementação

-   `f6cd911` --- validação de JWT;
-   `3f70b26` --- estrutura e migration da passagem;
-   `69af2b6` --- cadastro backend do Brisamar;
-   `aafb90e` --- escolha do terminal;
-   `9a96704` --- dados do turno e equipe;
-   `3a8bc80` --- ocupação das linhas;
-   `09f1814` --- registros gerais;
-   `e5031b0` --- recursos entregues;
-   `858fc4d` --- rádios utilizados;
-   `8b1dd1d` --- integração do formulário com a API.

Todos os commits estão com autoria de Leandro CHOQUE
`<leandro.cristine1@gmail.com>` e sem trailers de coautoria de IA.

### Próximo passo exato

1.  concluir a tela de confirmação de envio e encadeamento para o TECON;
2.  executar novamente os testes e uma revisão do diff completo;
3.  publicar `feature/passagem-brisamar`;
4.  abrir PR para `main` com `Closes #12` na descrição;
5.  aguardar e conferir os checks antes do merge;
6.  após o merge, sincronizar a `main` e atualizar este checkpoint.
