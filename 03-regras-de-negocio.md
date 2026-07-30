# RailOps — Regras de Negócio (Fase 3)

> Documento vivo. Parte da série de documentação técnica do projeto RailOps.

**Status:** Em revisão com o Product Owner
**Fase anterior:** Fase 2 — Casos de Uso (concluída)
**Próxima fase:** Fase 4 — Arquitetura

---

## Como ler este documento

Regras de negócio são restrições e comportamentos obrigatórios que
independem de qual tela ou tecnologia implementa o sistema — são verdades
sobre a operação ferroviária em si. Cada regra é numerada (RN) e referencia
os Casos de Uso e Requisitos de onde se origina, garantindo rastreabilidade
completa entre os três documentos já produzidos.

---

## RN01 — Edição restrita ao autor, dentro do próprio turno

Uma passagem de serviço só pode ser editada pelo usuário responsável que a
criou, e apenas enquanto o turno ao qual ela pertence ainda estiver em
curso. Após o encerramento do turno, a passagem torna-se somente leitura.

**Justificativa:** preserva a integridade histórica do registro e evita
reescrita de fatos operacionais tempo depois de ocorridos. Qualquer
correção necessária após esse prazo deve ser tratada por um mecanismo
diferente (a definir, se necessário, como um adendo separado que referencia
a passagem original — não uma edição retroativa).

**Casos de uso relacionados:** UC04
**Requisitos relacionados:** RF17, RF18

---

## RN02 — Toda edição gera uma nova versão, nunca sobrescreve

Quando uma edição é realizada dentro da janela permitida (RN01), o sistema
não deve sobrescrever o registro original. Deve ser criada uma nova versão,
mantendo a versão anterior acessível no histórico.

**Justificativa:** rastreabilidade é um objetivo central do projeto (ver
`01-requisitos.md`, seção de Contexto). Qualquer estratégia de
implementação (tabela de auditoria, versionamento por linha, etc.) deve
respeitar esse princípio — a decisão técnica específica será registrada
como ADR na Fase 4.

**Casos de uso relacionados:** UC04
**Requisitos relacionados:** RF17, RF18

---

## RN03 — Nenhum registro é excluído permanentemente

O sistema não deve oferecer, em nenhuma tela, uma ação de exclusão
definitiva de passagens de serviço.

**Justificativa:** requisito explícito de negócio — o histórico completo é
mais valioso que a limpeza de registros antigos ou incorretos.

**Requisitos relacionados:** RF17

---

## RN04 — Um único responsável autenticado por passagem

Apenas um usuário realiza login (matrícula + senha) para preencher e
enviar uma passagem de serviço. Os demais membros da equipe presentes no
turno são registrados como texto (nome e matrícula), sem conta própria no
sistema.

**Justificativa:** reflete o processo real de trabalho, no qual apenas uma
pessoa formaliza a passagem por turno. Simplifica a primeira versão do
sistema de autenticação, sem abrir mão da rastreabilidade sobre quem é o
responsável legal pelo registro.

**Casos de uso relacionados:** UC01, UC02
**Requisitos relacionados:** RF01, RF02, RF03

---

## RN05 — Campos específicos por terminal

O formulário de preenchimento varia conforme o terminal selecionado:

- **Pátio Brisamar:** inclui indicação Sup/Inf (ver RN06), controle de
  rádios (operantes/inoperantes, baterias, carregadores) e EOTs
  (disponíveis/avariados).
- **Terminal TECON:** inclui vistoria de carga mal posicionada e controle
  de atendimento nas Áreas 1 (Píer) e 2 (Galpão), com horário de início e
  término.

**Justificativa:** os dois terminais compartilham um núcleo comum de dados,
mas têm particularidades operacionais reais, confirmadas pelos modelos em
papel utilizados atualmente.

**Casos de uso relacionados:** UC02
**Requisitos relacionados:** RF04, RF12, RF13, RF14, RF15

---

## RN06 — Indicação Sup/Inf restrita às linhas 22 e 24 do Brisamar

O campo de indicação Superior/Inferior só é exibido quando a linha
selecionada for a 22 ou a 24. Para as demais linhas do Pátio Brisamar (16,
18, 20, 26, 28, 30), esse campo não é exibido, pois essas linhas possuem
apenas o lado Superior.

**Justificativa:** confirmado pelo layout físico do pátio — apenas as
linhas 22 e 24 se estendem por ambos os lados (Superior e Inferior).

**Casos de uso relacionados:** UC02
**Requisitos relacionados:** RF08

---

## RN07 — Linhas de manobra são listas fixas, não texto livre

As linhas disponíveis para seleção em cada terminal são pré-definidas:

- **Brisamar:** 16, 18, 20, 22, 24, 26, 28, 30.
- **TECON:** Viaduto (DM1A), L1, L2, Travessão, DM4, DM6, DM1, DM3, Funil
  (DM2).

O campo de veículos posicionados em cada linha permanece como texto livre,
sem restrição de formato.

**Justificativa:** reduz erro de digitação e viabiliza filtros e
indicadores confiáveis por linha; ao mesmo tempo, preserva a flexibilidade
necessária para o campo de veículos, cujo conteúdo muda constantemente e
não deve ser engessado.

**Casos de uso relacionados:** UC02
**Requisitos relacionados:** RF06, RF07

---

## RN08 — Rádios são rastreados individualmente; EOTs não

Cada rádio referenciado em uma passagem de serviço deve ser vinculado a um
catálogo com identificador único, permitindo consultar o histórico de
falhas de um rádio específico ao longo do tempo. EOTs são registrados
apenas como texto livre, sem rastreamento individual.

**Justificativa:** rádios têm valor operacional claro em serem rastreados
(gerar relatório de falhas recorrentes para a coordenação); EOTs, segundo
avaliação do Product Owner, não agregam esse mesmo valor pelo padrão atual
de uso.

**Casos de uso relacionados:** UC02, UC07
**Requisitos relacionados:** RF15, RF16, RF23

---

## RN09 — Sem validação de capacidade por linha

O sistema não deve validar ou alertar sobre a quantidade de vagões
informada em uma linha em relação a qualquer limite de capacidade.

**Justificativa:** a capacidade de cada linha varia conforme o tipo de
vagão (existem vagões menores e maiores), tornando qualquer regra fixa de
capacidade inconsistente e potencialmente geradora de alertas falsos.

**Casos de uso relacionados:** UC02
**Requisitos relacionados:** Fora de escopo (ver `01-requisitos.md`, seção
4)

---

## Matriz de Rastreabilidade (Regras de Negócio → Casos de Uso → Requisitos)

| Regra | Casos de Uso | Requisitos |
|---|---|---|
| RN01 | UC04 | RF17, RF18 |
| RN02 | UC04 | RF17, RF18 |
| RN03 | — | RF17 |
| RN04 | UC01, UC02 | RF01, RF02, RF03 |
| RN05 | UC02 | RF04, RF12–RF15 |
| RN06 | UC02 | RF08 |
| RN07 | UC02 | RF06, RF07 |
| RN08 | UC02, UC07 | RF15, RF16, RF23 |
| RN09 | UC02 | Fora de escopo |

---

## Aprovação

| Papel | Responsável | Status |
|---|---|---|
| Product Owner | (você) | ✅ RN01 e RN02 aprovadas nesta conversa; demais pendentes de revisão |
| Arquiteto de Software | (parceiro) | Pendente de revisão |
| Desenvolvedor Sênior | Claude | Documento elaborado |
