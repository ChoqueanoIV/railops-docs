# RailOps — Levantamento de Requisitos (Fase 1)

> Documento vivo. Parte da série de documentação técnica do projeto RailOps,
> que substitui a passagem de serviço em papel utilizada na operação
> ferroviária (Pátio Brisamar e Terminal TECON).

**Status:** Aprovado com o Product Owner em 29/07/2026
**Fase anterior:** Fase 0 — Entendimento do Problema (concluída)
**Próxima fase:** Fase 2 — Casos de Uso

---

## 1. Contexto

Hoje a passagem de serviço é preenchida em papel ao término de cada turno,
tanto no Pátio Brisamar quanto no Terminal TECON. As folhas são arquivadas
fisicamente, dificultando consulta, geração de indicadores e rastreabilidade
histórica.

O RailOps substitui esse processo por um sistema web único, com um núcleo de
dados comum aos dois terminais e seções específicas para cada um, preparado
para a adição de novos terminais no futuro.

**Decisão de arquitetura consolidada na Fase 0:** o sistema registra o
**estado de ocupação de cada linha ao término do turno** (uma fotografia),
e não o histórico de movimentações durante o turno.

---

## 2. Requisitos Funcionais (RF)

### 2.1 Autenticação e Responsabilidade

| ID | Requisito |
|----|-----------|
| RF01 | O sistema deve permitir login via matrícula + senha, restrito a um responsável por passagem de serviço. |
| RF02 | O responsável autenticado deve poder registrar nomes e matrículas de todos os demais membros da equipe presentes no turno, sem que estes precisem de login próprio. |
| RF03 | Cada passagem de serviço deve ficar permanentemente vinculada ao usuário responsável que a criou. |

### 2.2 Preenchimento da Passagem

| ID | Requisito |
|----|-----------|
| RF04 | O sistema deve permitir a escolha do terminal (Pátio Brisamar ou TECON) antes do preenchimento. |
| RF05 | O sistema deve registrar data e turno da passagem. |
| RF06 | O sistema deve permitir o registro do estado de ocupação de cada linha do terminal escolhido ao término do turno (linha + veículos em texto livre). |
| RF07 | As linhas disponíveis para seleção devem ser listas fixas conhecidas por terminal: Brisamar (16, 18, 20, 22, 24, 26, 28, 30); TECON (Viaduto/DM1A, L1, L2, Travessão, DM4, DM6, DM1, DM3, Funil/DM2). |
| RF08 | Para as linhas 22 e 24 do Brisamar, o sistema deve solicitar indicação Superior/Inferior; as demais linhas do Brisamar não exibem esse campo. |
| RF09 | O sistema deve permitir o registro de Observações (texto livre). |
| RF10 | O sistema deve permitir o registro de Relatório de Ocorrências e/ou Alterações (texto livre). |
| RF11 | O sistema deve perguntar se o Mobile foi utilizado durante o serviço e, em caso negativo, solicitar justificativa. |
| RF12 | *(Exclusivo TECON)* O sistema deve perguntar se foi detectada carga mal posicionada na vistoria de vagões, com campo de descrição. |
| RF13 | *(Exclusivo TECON)* O sistema deve registrar atendimento nas Áreas 1 (Píer) e 2 (Galpão), com horário de início e término para cada uma. |
| RF14 | *(Exclusivo Brisamar)* O sistema deve registrar quantidade de rádios operantes/inoperantes, baterias e carregadores entregues ao término do turno. |
| RF15 | *(Exclusivo Brisamar)* O sistema deve permitir o registro de EOTs disponíveis e avariados como texto livre, sem rastreamento individual. |
| RF16 | O sistema deve permitir o registro de rádios utilizados (número, manobrador, hora de retirada, hora de entrega, se apresentou falha e qual), com o número do rádio vinculado a um catálogo rastreável. |

### 2.3 Histórico e Consulta

| ID | Requisito |
|----|-----------|
| RF17 | Nenhum registro de passagem de serviço deve ser excluído permanentemente do sistema. |
| RF18 | Edições em uma passagem já registrada devem gerar uma nova versão, preservando o histórico de alterações (mecanismo formal a definir em ADR na Fase 4). |
| RF19 | O sistema deve permitir busca e filtro de passagens por data, turno, terminal, responsável, linha, ocorrências e palavras-chave em observações. |
| RF20 | O sistema deve permitir exportação de dados filtrados para Excel e CSV. |

### 2.4 Referência Visual

| ID | Requisito |
|----|-----------|
| RF21 | O sistema deve exibir o diagrama de manobras do terminal selecionado como referência visual durante o preenchimento. |
| RF22 | O sistema deve permitir o download do diagrama de manobras em PDF. |

### 2.5 Relatórios

| ID | Requisito |
|----|-----------|
| RF23 | O sistema deve permitir a geração de relatório de falhas recorrentes por rádio, com base no catálogo rastreável (RF16). |

---

## 3. Requisitos Não Funcionais (RNF)

| ID | Requisito |
|----|-----------|
| RNF01 | O sistema deve ser acessível via navegador web, sem necessidade de instalação. |
| RNF02 | O sistema deve utilizar apenas serviços gratuitos na primeira versão (Supabase, Render ou equivalente). |
| RNF03 | O sistema deve responder em tempo hábil para preenchimento ao final de turno. Modo offline não é requisito, pois a conectividade é garantida no momento de uso. |
| RNF04 | O banco de dados deve ser PostgreSQL, com migrations versionadas desde o primeiro commit. |
| RNF05 | A arquitetura deve suportar a adição de novos terminais no futuro sem reescrita do núcleo do sistema. |
| RNF06 | O sistema deve validar entradas no backend, não apenas no frontend. |
| RNF07 | O sistema deve expor um endpoint de health check. |
| RNF08 | A documentação técnica deve incluir ADRs para decisões estruturais (versionamento de histórico, autenticação, modelo de dados). |

---

## 4. Fora de Escopo (v1)

- Integração com IA para consultas em linguagem natural.
- Integração com Power BI (apenas estrutura de banco preparada para futura integração).
- Alerta de capacidade por linha — descartado; capacidade varia conforme o tipo de vagão, o que tornaria a regra inconsistente.
- Rastreamento individual de EOTs — tratados como texto livre.
- Modo offline / sincronização posterior.
- Múltiplos usuários autenticados por passagem — apenas 1 responsável realiza login.
- Vínculo formal entre passagem de TECON e passagem de Brisamar no banco de dados — no papel existia por exigência de assinatura física; no sistema digital deixa de ser necessário.

---

## 5. Glossário de Domínio

| Termo | Significado |
|-------|-------------|
| **EOT** | End of Train device — dispositivo instalado ao final de composições de até 272 vagões, gerenciado pelos manobradores. |
| **KVS** | Sigla de um trem recebido normalmente entre 00h e 05h; se chega após esse horário, geralmente não há tempo hábil de manobrá-lo. |
| **PN** | Passagem de Nível. |
| **Sup / Inf** | Indicação de lado superior ou inferior do Pátio Brisamar. Aplicável apenas às linhas 22 e 24, que possuem os dois lados; linhas 16, 18 e 20 possuem apenas lado Superior. |
| **AMV/Chave** | Aparelho de Mudança de Via — ponto de troca entre linhas. |
| **Área 1 (Píer) / Área 2 (Galpão)** | Zonas de atendimento do Terminal TECON. |

---

## 6. Anexos de Referência

- `PASSAGEM_DE_SERVIÇO_PADRÃO_TECON.pdf` — modelo em papel da passagem de serviço do TECON.
- `PASSAGEM_DE_SERVIÇO_PADRÃO_PÁTIO.pdf` — modelo em papel da passagem de serviço do Pátio Brisamar.
- `TECON_20260127_112350_0000.pdf` — diagrama de manobras do Terminal TECON.
- `PATIO_BRISAMAR_20260127_112226_0000.pdf` — diagrama de manobras do Pátio Brisamar.

---

## 7. Aprovação

| Papel | Responsável | Status |
|-------|--------------|--------|
| Product Owner | (você) | ✅ Aprovado |
| Arquiteto de Software | (parceiro) | Pendente de revisão |
| Desenvolvedor Sênior | Claude | Documento elaborado |
