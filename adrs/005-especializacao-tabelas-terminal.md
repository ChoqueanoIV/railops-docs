# ADR-005 — Especialização de Tabelas por Terminal

**Status:** Aceito
**Data:** 30/07/2026
**Decisor:** Product Owner, com recomendação técnica do Desenvolvedor Sênior

---

## Contexto

A RN05 (`03-regras-de-negocio.md`) estabelece que o formulário de
preenchimento varia conforme o terminal selecionado (Pátio Brisamar ou
Terminal TECON), cada um com campos próprios além de um núcleo comum de
dados. Era necessário decidir como representar essa variação no modelo
relacional do PostgreSQL, especialmente considerando o RNF05 (a
arquitetura deve suportar novos terminais no futuro sem reescrita do
núcleo do sistema).

## Alternativas Consideradas

**1. Tabela única e larga.** Todas as colunas de todos os terminais
convivem na mesma tabela `passagem_servico`; colunas irrelevantes para o
terminal daquele registro ficam `NULL`. Simples para consulta (sem
`JOIN`), mas cresce indefinidamente a cada novo terminal, permite
preenchimento indevido de campos que não pertencem ao terminal do
registro, e viola o espírito do RNF05.

**2. Tabelas de especialização.** A tabela `passagem_servico` guarda
apenas os campos comuns aos terminais. Cada terminal tem sua própria
tabela de detalhe (`passagem_brisamar_detalhe`,
`passagem_tecon_detalhe`), em relação um-para-um com a tabela núcleo.
Exige `JOIN` para obter os detalhes completos, mas garante que cada
tabela de detalhe só contenha campos válidos para aquele terminal, e
permite adicionar novos terminais criando apenas novas tabelas, sem
alterar as existentes.

**3. Tabela núcleo + coluna JSONB.** Campos específicos de terminal
armazenados em uma coluna semiestruturada. Máxima flexibilidade para
mudanças futuras imprevisíveis, mas perde validação de tipo em nível de
banco e dificulta filtros/relatórios eficientes sobre esses campos —
contrapondo-se ao valor de tipagem forte que motivou a adoção do
SQLAlchemy (ADR-004).

## Decisão

Adotar a **Abordagem 2 — Tabelas de especialização por terminal**.

## Justificativa

- Atende diretamente ao RNF05: um novo terminal no futuro (já mencionado
  como plano do Product Owner) implica apenas a criação de uma nova tabela
  de detalhe, sem qualquer alteração na tabela núcleo ou nas tabelas de
  detalhe já existentes.
- Impede, a nível de schema, o preenchimento de campos que não pertencem
  ao terminal do registro — validação estrutural, não apenas de
  aplicação.
- Os campos de cada terminal já são conhecidos e estáveis (originados dos
  modelos em papel analisados na Fase 0), não havendo necessidade da
  flexibilidade adicional — e das perdas de tipagem — da Abordagem 3.
- Mantém coerência com a tipagem forte buscada na escolha do SQLAlchemy
  (ADR-004).

## Consequências

- O modelo relacional da Fase 5 incluirá `passagem_servico` (núcleo),
  `passagem_brisamar_detalhe` e `passagem_tecon_detalhe`, cada uma
  vinculada por chave estrangeira única (relação um-para-um) à tabela
  núcleo.
- Consultas que precisem do detalhe completo de uma passagem exigirão
  `JOIN` condicional (com a tabela de detalhe correspondente ao terminal
  do registro), a ser implementado na camada de Repository (ADR-003).
- A adição de um novo terminal no futuro seguirá o mesmo padrão: uma nova
  tabela `passagem_<terminal>_detalhe`, sem impacto nas tabelas
  existentes.
