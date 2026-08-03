# RailOps — Protótipos (Fase 6)

> Documento vivo. Parte da série de documentação técnica do projeto RailOps.

**Status:** Em revisão com o Product Owner
**Fase anterior:** Fase 5 — Modelagem do Banco de Dados (concluída)
**Próxima fase:** Fase 7 — Planejamento de Branches

---

## Sobre esta fase

Os protótipos foram construídos como mockups de média fidelidade em
HTML/CSS, apresentados interativamente durante a sessão de elaboração
deste documento. A escolha de HTML/CSS (em vez de uma ferramenta de
design externa) é deliberada: o RailOps já usará HTML/CSS no frontend
(RNF01–RNF03), então esses protótipos podem evoluir diretamente para
componentes reais na Fase 9, em vez de serem descartados após aprovação.

Cada tela abaixo está diretamente ligada a um ou mais Casos de Uso
(`02-casos-de-uso.md`) e às Regras de Negócio (`03-regras-de-negocio.md`)
que a moldam.

---

## Tela 1 — Login

**Caso de uso:** UC01

Campos: matrícula e PIN (não e-mail/senha complexa — reflete diretamente
o ADR-002). Mensagem de apoio para o fluxo de primeiro acesso, cobrindo a
ativação de matrícula pré-cadastrada.

---

## Tela 2 — Escolha de terminal

**Caso de uso:** Início do UC02

Duas opções apresentadas como cartões: Pátio Brisamar (terminal base) e
Terminal TECON (atendimento a cliente). Simples por design — é uma
decisão binária que não exige explicação adicional na tela.

**Decisão de fluxo (a partir desta conversa):** após o envio de uma
passagem, a tela de confirmação deve oferecer diretamente a opção
"Preencher passagem do outro terminal agora", já que ambos os terminais
são normalmente atendidos no mesmo turno pela mesma equipe. Esta tela de
confirmação com essa ação ainda precisa ser prototipada visualmente — item
pendente para revisão futura desta fase.

---

## Tela 3 — Formulário de preenchimento (Pátio Brisamar)

**Caso de uso:** UC02, variante Brisamar (Fluxo Alternativo A1)

Estrutura: data/turno, lista dinâmica de ocupação de linhas (com campo
Sup/Inf exibido apenas para as linhas 22 e 24, conforme RN06),
Observações, Relatório de Ocorrências, checkbox de uso do Mobile, e
seção de rádios/baterias/EOTs.

**Confirmado nesta sessão:** o preenchimento é sempre feito de uma vez
só, ao término do turno — não há necessidade de salvamento de rascunho.

---

## Tela 4 — Formulário de preenchimento (Terminal TECON)

**Caso de uso:** UC02, variante TECON (Fluxo Alternativo A2), com RN10

Estrutura idêntica ao núcleo do Brisamar (data/turno, linhas, Observações,
Relatório de Ocorrências), acrescida da pergunta condicional **"Houve
atendimento ao terminal hoje?"** (RN10):

- **Se não:** apenas ocupação de linhas, Observações e Ocorrências são
  exibidos — os campos de vistoria de carga e Áreas 1/2 permanecem
  ocultos. Linhas, Observações e Ocorrências continuam obrigatórios
  independentemente da resposta.
- **Se sim:** exibe adicionalmente vistoria de carga mal posicionada e um
  bloco por área (Área 1 — Píer, Área 2 — Galpão), **cada uma com seu
  próprio checkbox "Atendida" e seus próprios horários de início/término,
  de forma independente**. É comum que ambas sejam atendidas no mesmo
  turno, mas não é regra — o formulário não exige que uma área implique
  a outra.

---

## Tela 5 — Consulta de passagens

**Casos de uso:** UC03, UC05

Filtros por data, turno, terminal e palavra-chave, com lista de
resultados expansível e ações de exportação (CSV e Excel) posicionadas
junto ao resultado filtrado, não como uma tela separada — reflete que
consulta e exportação são, na prática, a mesma jornada do usuário.

---

## Tela 6 — Relatório de falhas recorrentes por rádio

**Caso de uso:** UC07

Lista de rádios ordenada por quantidade de falhas (decrescente), com
destaque visual (cor) para rádios com maior número de ocorrências —
prioriza visualmente a informação mais acionável para a coordenação.

---

## Tela 7 — Diagrama de manobras (referência visual)

**Caso de uso:** UC06

Exibido dentro do fluxo de preenchimento (Telas 3 e 4), com opção de
download em PDF. Na implementação real, exibirá os diagramas de manobra
originais (anexados em `docs/anexos/`), não uma representação genérica
como no protótipo.

---

## Pendências identificadas nesta fase

- Prototipar a tela de confirmação de envio, incluindo a ação de
  encadeamento para o segundo terminal (ver Tela 2).
- Validar com o Product Owner se a resposta "Não" em RN10 deve, por
  padrão, pré-preencher Observações/Ocorrências com "Sem alterações" ou
  deixar em branco para digitação manual.

---

## Aprovação

| Papel | Responsável | Status |
|---|---|---|
| Product Owner | (você) | Aprovado nesta sessão (Telas 1, 3, 4, 5, 6, 7); Tela 2 com pendência registrada |
| Arquiteto de Software | (parceiro) | Pendente de revisão |
| Desenvolvedor Sênior | Claude | Documento elaborado |
