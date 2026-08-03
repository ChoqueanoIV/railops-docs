# RailOps — Planejamento de Branches (Fase 7)

> Documento vivo. Parte da série de documentação técnica do projeto RailOps.

**Status:** Aprovado
**Fase anterior:** Fase 6 — Protótipos (concluída)
**Próxima fase:** Fase 8 — Backlog

---

## Estratégia adotada: GitHub Flow

Ver justificativa completa em `adrs/006-estrategia-de-branching.md`.

## Convenção de nomenclatura de branches

Toda branch nasce a partir da `main` e segue o padrão:

```
<tipo>/<descricao-curta-em-kebab-case>
```

| Tipo | Quando usar | Exemplo |
|---|---|---|
| `feature/` | Nova funcionalidade | `feature/login-matricula` |
| `fix/` | Correção de bug | `fix/validacao-turno-invalido` |
| `docs/` | Mudança apenas em documentação | `docs/atualiza-checkpoint` |
| `refactor/` | Reorganização de código sem mudar comportamento | `refactor/camada-repository` |
| `test/` | Adição ou ajuste de testes | `test/cobertura-rn01` |
| `chore/` | Tarefas de manutenção (configs, dependências) | `chore/configura-alembic` |

O prefixo espelha deliberadamente os tipos já usados nas mensagens de
commit (Conventional Commits), mantendo consistência entre nome de
branch, mensagem de commit e, futuramente, título do Pull Request.

## Fluxo de trabalho passo a passo

Este é o ciclo que será praticado a partir da Fase 9, para cada nova
tarefa do backlog:

1. **Atualizar a `main` local** antes de começar qualquer tarefa nova
   (`git checkout main` + `git pull`), garantindo que a nova branch parta
   do estado mais recente.
2. **Criar a branch** a partir da `main`, com nome descritivo seguindo a
   convenção acima (`git checkout -b feature/nome-da-tarefa`).
3. **Trabalhar na branch**, com commits atômicos e mensagens em
   Conventional Commits, exatamente como já praticado no `railops-docs`.
4. **Enviar a branch ao GitHub** (`git push -u origin nome-da-branch`).
5. **Abrir um Pull Request** da branch para a `main`, com uma descrição
   do que foi feito e por quê.
6. **Revisar o próprio Pull Request** antes de aprovar — mesmo trabalhando
   sozinho, esse passo simula o processo de revisão de código de um time
   real e é o momento de reler o próprio trabalho com distância crítica.
7. **Fazer o merge** do Pull Request para a `main`.
8. **Apagar a branch** já mesclada, mantendo o repositório limpo.

## Por que este processo importa mesmo trabalhando sozinho

Um Pull Request bem escrito, mesmo autoaprovado, cria um registro
permanente e pesquisável de *por que* uma mudança foi feita — algo que
commits isolados não capturam tão bem. Para fins de portfólio, a aba
"Pull Requests" de um repositório com esse histórico comunica domínio de
processo de equipe, não apenas capacidade de escrever código.

## Proteção da branch `main` (a ser configurada na Fase 9)

Quando o `railops-app` começar a receber código de fato, será configurada
uma regra de proteção de branch no GitHub, exigindo que toda mudança
passe por Pull Request — impedindo push direto na `main`, mesmo pelo
próprio autor. Isso reforça, na prática, a disciplina descrita acima.

---

## Aprovação

| Papel | Responsável | Status |
|---|---|---|
| Product Owner | (você) | ✅ Aprovado |
| Arquiteto de Software | (parceiro) | Pendente de revisão |
| Desenvolvedor Sênior | Claude | Documento elaborado |
