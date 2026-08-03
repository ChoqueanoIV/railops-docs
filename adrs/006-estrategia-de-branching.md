# ADR-006 — Estratégia de Branching

**Status:** Aceito
**Data:** 03/08/2026
**Decisor:** Product Owner, com recomendação técnica do Desenvolvedor Sênior

---

## Contexto

Com a Fase 6 (Protótipos) concluída, era necessário definir a estratégia
de branches a ser usada no repositório `railops-app` antes do início da
implementação (Fase 9), de forma a equilibrar prática real de fluxo de
trabalho em equipe com a realidade de um projeto conduzido por um único
desenvolvedor.

## Alternativas Consideradas

**1. Git Flow.** Múltiplos tipos de branch com papéis fixos (`main`,
`develop`, `feature/*`, `release/*`, `hotfix/*`). Adequado a times grandes
com ciclos de release programados; introduz complexidade de processo
desproporcional a um projeto solo sem lançamentos programados.

**2. GitHub Flow.** Apenas `main` (sempre estável) e branches descritivas
de curta duração por mudança, integradas via Pull Request. Simples,
mantém a prática de branch/PR/revisão/merge, e é o padrão mais comum em
produtos com deploy contínuo hoje no mercado.

**3. Trunk-Based Development.** Commits quase diretos na `main`, com
forte dependência de testes automatizados e feature flags. Otimizado para
altíssima frequência de deploy em equipes maduras; pressupõe uma suíte de
testes que o projeto ainda não possui (Fase 10 não iniciada) e não deixa
rastro de processo (branches, PRs) relevante para fins de portfólio.

## Decisão

Adotar o **GitHub Flow**.

## Justificativa

- Equilibra simplicidade (sem a burocracia de `develop`/`release` do Git
  Flow) com prática genuína de branch, Pull Request, revisão e merge —
  objetivo explícito de aprendizado do Product Owner.
- É o padrão mais comum em vagas de mercado atuais, com valor direto para
  o objetivo de portfólio do projeto.
- Compatível com o estágio atual do projeto (sem suíte de testes
  automatizados ainda implementada), diferente do Trunk-Based
  Development.

## Consequências

- A branch `main` do `railops-app` deve permanecer sempre estável.
- Toda nova funcionalidade, correção ou tarefa será desenvolvida em uma
  branch própria, nomeada de forma descritiva, e integrada via Pull
  Request revisado antes do merge — convenção detalhada em
  `07-planejamento-branches.md`.
- Esta convenção será ensinada e praticada meticulosamente a partir da
  Fase 9 (Implementação).
