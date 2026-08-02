# ADR-004 — Escolha do ORM para Acesso ao Banco de Dados

**Status:** Aceito
**Data:** 30/07/2026
**Decisor:** Product Owner, com recomendação técnica do Desenvolvedor Sênior

---

## Contexto

Com a Arquitetura em Camadas definida (ADR-003), era necessário decidir
como a camada de Repository efetivamente acessaria o PostgreSQL: por meio
de SQL puro (via driver de baixo nível) ou por meio de um ORM
(Object-Relational Mapper).

## Alternativas Consideradas

**1. SQL puro** (via `psycopg2` ou `asyncpg`). Queries escritas
manualmente como texto dentro do código Python. Oferece controle total e
transparência sobre o comando exato enviado ao banco, mas é repetitivo,
mais sujeito a erro de digitação, e exige gestão manual de proteção
contra SQL Injection, além de exigir controle manual de migrations de
schema.

**2. ORM — SQLAlchemy.** Permite trabalhar com o banco através de classes
e objetos Python. Reduz repetição de código para operações comuns
(inserir, atualizar, buscar com filtros), protege por padrão contra SQL
Injection, e se integra à ferramenta de migrations Alembic, atendendo
diretamente ao RNF04 (migrations versionadas desde o primeiro commit). É a
combinação mais comum em projetos FastAPI no mercado. Como desvantagem,
introduz uma camada de abstração que exige aprendizado para não se tornar
uma "caixa-preta", e queries muito complexas por vezes ainda exigem SQL
manual complementar.

## Decisão

Adotar **SQLAlchemy** como ORM, com **Alembic** para gestão de migrations.

## Justificativa

- Atende diretamente ao RNF04 (`01-requisitos.md`), sem necessidade de
  gestão manual de scripts de alteração de schema.
- Protege por padrão contra SQL Injection, uma das vulnerabilidades mais
  citadas em avaliações de segurança de backend.
- É a combinação mais utilizada em vagas de mercado para FastAPI,
  reforçando o valor de portfólio e preparação para entrevistas técnicas
  do Product Owner.
- Reduz volume de código repetitivo na camada de Repository (ADR-003),
  liberando foco de aprendizado para as regras de negócio (RN01–RN09) e
  para o funcionamento interno do próprio SQLAlchemy — não para
  reescrever operações básicas de CRUD manualmente.

## Consequências

- A camada de Repository (ADR-003) será implementada com modelos
  SQLAlchemy (classes Python representando tabelas).
- Toda alteração de schema do banco de dados, a partir da Fase 5, deverá
  ser feita através de migrations geradas pelo Alembic, nunca por
  alteração manual direta no banco (reforça também o espírito do RN02 —
  rastreabilidade de mudanças, agora aplicada ao próprio schema, não só
  aos dados).
- Durante a implementação (Fase 9), cada operação do SQLAlchemy usada pela
  primeira vez deverá ser acompanhada de explicação do SQL equivalente
  gerado por trás, para que o Product Owner não trate a ferramenta como
  caixa-preta.
- Esta decisão encerra as definições de arquitetura de alto nível da Fase
  4. Próxima fase: Fase 5 — Modelagem do Banco de Dados.
