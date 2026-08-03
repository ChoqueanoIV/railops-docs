# RailOps — Casos de Uso (Fase 2)

> Documento vivo. Parte da série de documentação técnica do projeto RailOps.

**Status:** Em revisão com o Product Owner
**Fase anterior:** Fase 1 — Levantamento de Requisitos (concluída)
**Próxima fase:** Fase 3 — Regras de Negócio

---

## Como ler este documento

Cada caso de uso segue a estrutura clássica de Engenharia de Requisitos:

- **Ator**: quem interage com o sistema nesse cenário.
- **Pré-condições**: o que precisa ser verdade antes do caso de uso começar.
- **Fluxo Principal**: o "caminho feliz" — passo a passo esperado quando tudo
  corre bem.
- **Fluxos Alternativos**: o que acontece quando algo foge do fluxo principal
  (erros, exceções, variações). Estes itens alimentam diretamente a Fase 3
  (Regras de Negócio).
- **Pós-condições**: o estado do sistema após o caso de uso ser concluído
  com sucesso.
- **Requisitos relacionados**: rastreabilidade com o documento
  `01-requisitos.md`.

---

## UC01 — Autenticar Responsável

| Campo | Descrição |
|---|---|
| **Ator** | Responsável pela passagem de serviço |
| **Pré-condições** | Usuário possui matrícula e senha previamente cadastradas no sistema |
| **Fluxo Principal** | 1. Usuário acessa a tela de login.<br>2. Usuário informa matrícula e senha.<br>3. Sistema valida as credenciais.<br>4. Sistema autentica o usuário e redireciona para a tela de escolha de terminal. |
| **Fluxos Alternativos** | **A1** — Credenciais inválidas: sistema exibe mensagem de erro e permanece na tela de login.<br>**A2** — Usuário não cadastrado: sistema exibe mensagem de erro genérica, sem revelar se a matrícula existe. |
| **Pós-condições** | Usuário autenticado, com sessão ativa vinculada à sua matrícula. |
| **Requisitos relacionados** | RF01, RF03 |

---

## UC02 — Preencher Passagem de Serviço

| Campo | Descrição |
|---|---|
| **Ator** | Responsável pela passagem de serviço (autenticado) |
| **Pré-condições** | Usuário autenticado (UC01) |
| **Fluxo Principal** | 1. Usuário seleciona o terminal (Pátio Brisamar ou TECON).<br>2. Sistema exibe o formulário correspondente, com o diagrama de manobras do terminal como referência visual.<br>3. Usuário informa data e turno.<br>4. Usuário registra nomes e matrículas dos demais membros da equipe presentes no turno.<br>5. Usuário preenche o estado de ocupação das linhas (linha pré-definida + veículos em texto livre).<br>6. Usuário preenche Observações e Relatório de Ocorrências.<br>7. Usuário responde se o Mobile foi utilizado (e justificativa, se não).<br>8. Usuário preenche as seções específicas do terminal escolhido (ver A1 e A2).<br>9. Usuário registra os rádios utilizados (número, manobrador, retirada, entrega, falhas).<br>10. Usuário confirma e envia a passagem.<br>11. Sistema salva a passagem, vinculada ao responsável autenticado. |
| **Fluxos Alternativos** | **A1 (Terminal = Brisamar)** — Sistema solicita indicação Sup/Inf apenas para as linhas 22 e 24; solicita quantidade de rádios operantes/inoperantes, baterias e carregadores; solicita EOTs disponíveis/avariados.<br>**A2 (Terminal = TECON)** — Sistema pergunta se houve atendimento ao terminal no turno (RN10). Se sim, solicita vistoria de carga mal posicionada (com descrição, se houver) e atendimento nas Áreas 1 (Píer) e 2 (Galpão), com horário de início/término. Se não, os campos de vistoria de carga e Áreas 1/2 não são exibidos; apenas ocupação de linhas, Observações e Relatório de Ocorrências permanecem no formulário.<br>**A3** — Campo obrigatório não preenchido: sistema impede o envio e destaca o campo pendente.<br>**A4** — Usuário tenta preencher Sup/Inf em linha que não permite (16, 18, 20, 26, 28, 30): sistema não exibe essa opção para essas linhas. |
| **Pós-condições** | Passagem de serviço registrada permanentemente no banco, vinculada ao responsável, ao terminal, à data/turno e disponível para consulta futura. |
| **Requisitos relacionados** | RF02, RF04–RF16 |

---

## UC03 — Consultar Passagens de Serviço

| Campo | Descrição |
|---|---|
| **Ator** | Responsável, ou (futuramente) monitor/supervisor/coordenador |
| **Pré-condições** | Usuário autenticado; existir ao menos uma passagem registrada |
| **Fluxo Principal** | 1. Usuário acessa a tela de consulta.<br>2. Usuário informa um ou mais filtros (data, turno, terminal, responsável, linha, ocorrências, palavra-chave).<br>3. Sistema retorna a lista de passagens que atendem aos filtros.<br>4. Usuário seleciona uma passagem para visualizar detalhes completos. |
| **Fluxos Alternativos** | **A1** — Nenhum resultado encontrado: sistema exibe mensagem informando que não há passagens para os filtros aplicados.<br>**A2** — Usuário não informa nenhum filtro: sistema retorna as passagens mais recentes, ordenadas por data decrescente. |
| **Pós-condições** | Usuário visualiza os dados da(s) passagem(ns) consultada(s), sem alterar nenhum registro. |
| **Requisitos relacionados** | RF19 |

---

## UC04 — Editar Passagem de Serviço

| Campo | Descrição |
|---|---|
| **Ator** | Responsável (regra de autorização a confirmar em ADR na Fase 4) |
| **Pré-condições** | Passagem de serviço já existente e localizada (via UC03) |
| **Fluxo Principal** | 1. Usuário abre uma passagem existente.<br>2. Usuário aciona a opção de edição.<br>3. Usuário altera os campos desejados.<br>4. Usuário confirma a edição.<br>5. Sistema cria uma nova versão do registro, preservando a versão anterior no histórico. |
| **Fluxos Alternativos** | **A1** — Usuário cancela a edição antes de confirmar: nenhuma alteração é salva.<br>**A2** — Usuário tenta editar uma passagem sem permissão: sistema bloqueia a ação. |
| **Pós-condições** | Nova versão da passagem registrada; versão anterior preservada e consultável no histórico; nenhum dado é perdido. |
| **Requisitos relacionados** | RF17, RF18 |

---

## UC05 — Exportar Dados

| Campo | Descrição |
|---|---|
| **Ator** | Responsável, ou (futuramente) supervisor/coordenador |
| **Pré-condições** | Usuário autenticado; existir ao menos uma passagem que atenda aos filtros aplicados |
| **Fluxo Principal** | 1. Usuário realiza uma consulta (UC03).<br>2. Usuário aciona a opção de exportar.<br>3. Usuário escolhe o formato (Excel ou CSV).<br>4. Sistema gera o arquivo com os dados filtrados.<br>5. Usuário realiza o download. |
| **Fluxos Alternativos** | **A1** — Nenhum dado a exportar (consulta vazia): sistema impede a ação e informa o motivo. |
| **Pós-condições** | Arquivo gerado e disponível para download; nenhum dado é alterado no sistema. |
| **Requisitos relacionados** | RF20 |

---

## UC06 — Visualizar/Baixar Diagrama de Manobras

| Campo | Descrição |
|---|---|
| **Ator** | Qualquer usuário autenticado |
| **Pré-condições** | Usuário autenticado |
| **Fluxo Principal** | 1. Usuário seleciona um terminal (durante o preenchimento da passagem, ou em uma tela dedicada).<br>2. Sistema exibe o diagrama de manobras correspondente.<br>3. Usuário aciona a opção de download em PDF, se desejar. |
| **Fluxos Alternativos** | Nenhum fluxo alternativo relevante identificado nesta fase. |
| **Pós-condições** | Usuário visualiza ou baixa o diagrama; nenhum dado é alterado. |
| **Requisitos relacionados** | RF21, RF22 |

---

## UC07 — Gerar Relatório de Falhas Recorrentes por Rádio

| Campo | Descrição |
|---|---|
| **Ator** | Responsável, ou (futuramente) supervisor/coordenador |
| **Pré-condições** | Usuário autenticado; existir ao menos um registro de rádio com falha reportada |
| **Fluxo Principal** | 1. Usuário acessa a tela de relatórios.<br>2. Usuário seleciona o relatório de falhas por rádio.<br>3. Sistema consulta o catálogo de rádios e agrega as falhas reportadas em todas as passagens.<br>4. Sistema exibe a lista de rádios ordenada por quantidade de falhas. |
| **Fluxos Alternativos** | **A1** — Nenhum rádio com falha registrada: sistema exibe mensagem informando ausência de dados. |
| **Pós-condições** | Usuário visualiza o relatório; nenhum dado é alterado. |
| **Requisitos relacionados** | RF16, RF23 |

---

## Matriz de Rastreabilidade (Requisitos → Casos de Uso)

| Requisito | Caso(s) de Uso |
|---|---|
| RF01–RF03 | UC01 |
| RF04–RF16 | UC02 |
| RF17–RF18 | UC04 |
| RF19 | UC03 |
| RF20 | UC05 |
| RF21–RF22 | UC06 |
| RF23 | UC07 |

---

## Aprovação

| Papel | Responsável | Status |
|---|---|---|
| Product Owner | (você) | Pendente de revisão |
| Arquiteto de Software | (parceiro) | Pendente de revisão |
| Desenvolvedor Sênior | Claude | Documento elaborado |
