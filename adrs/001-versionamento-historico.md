# ADR-001 — Estratégia de Versionamento de Histórico de Passagens de Serviço

**Status:** Aceito
**Data:** 30/07/2026
**Decisor:** Product Owner, com recomendação técnica do Desenvolvedor Sênior

---

## Contexto

A RN02 (`03-regras-de-negocio.md`) estabelece que toda edição de uma
passagem de serviço deve gerar uma nova versão, preservando a versão
anterior no histórico, nunca sobrescrevendo dados. Era necessário escolher
como isso seria implementado no PostgreSQL.

## Alternativas Consideradas

**1. Tabela de histórico separada (Audit Table).** A tabela principal
(`passagem_servico`) guarda apenas o estado atual de cada passagem. Antes
de uma edição ser aplicada, o estado anterior é copiado para uma tabela
`passagem_servico_historico`. Consultas do dia a dia trabalham apenas com
a tabela principal, sem necessidade de filtros adicionais.

**2. Versionamento por linha na própria tabela (Row Versioning).** Todas
as versões (atual e antigas) vivem na mesma tabela, diferenciadas por um
campo `versao` e um marcador `e_versao_atual`. Uniformiza o armazenamento,
mas exige que toda consulta do sistema filtre explicitamente pela versão
vigente, sob risco de bugs silenciosos caso esse filtro seja esquecido em
alguma query futura.

**3. Trigger de banco de dados (PL/pgSQL).** A cópia da versão anterior é
feita automaticamente por um trigger no PostgreSQL, de forma transparente
à aplicação. Garante auditoria mesmo em alterações feitas fora do backend
(ex: diretamente pelo painel do Supabase), mas introduz uma tecnologia
adicional (PL/pgSQL) fora do escopo de aprendizado prioritário do projeto
(Python/FastAPI), além de dificultar a cobertura por testes em Pytest.

## Decisão

Adotar a **Abordagem 1 — Tabela de histórico separada**.

## Justificativa

- Mantém as consultas mais frequentes do sistema (busca, RF19; exportação,
  RF20) simples e performáticas, sem necessidade de filtros de versão em
  toda query.
- Toda a lógica de versionamento permanece no código Python/FastAPI,
  alinhado ao objetivo de aprendizado prioritário definido para o projeto.
- É plenamente suficiente para o volume e o perfil de uso atual (edição
  restrita ao próprio responsável, dentro do próprio turno — RN01),
  reduzindo o risco de bugs silenciosos presente na Abordagem 2.
- A Abordagem 3 permanece como aprendizado futuro, fora do escopo desta
  primeira versão do sistema.

## Consequências

- Será necessário criar, na Fase 5 (Modelagem do Banco), a tabela
  `passagem_servico_historico`, espelhando a estrutura de
  `passagem_servico` no momento da cópia.
- A lógica de "copiar antes de editar" deverá ser implementada
  explicitamente na camada de serviço do backend (FastAPI), e coberta por
  testes automatizados (Pytest) que validem que nenhuma edição ocorre sem
  a preservação da versão anterior.
- Caso o projeto evolua para múltiplos usuários editando diretamente pelo
  banco (fora do backend), esta decisão deverá ser revisitada em um novo
  ADR.
