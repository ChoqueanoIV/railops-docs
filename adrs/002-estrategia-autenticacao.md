# ADR-002 — Estratégia de Autenticação

**Status:** Aceito
**Data:** 30/07/2026
**Decisor:** Product Owner, com recomendação técnica do Desenvolvedor Sênior

---

## Contexto

A RN04 (`03-regras-de-negocio.md`) estabelece que apenas um responsável
autenticado (via matrícula + senha) preenche e envia cada passagem de
serviço. Era necessário decidir como essa autenticação seria implementada.

Durante a discussão, o Product Owner propôs reaproveitar a senha do portal
corporativo da empresa como senha de acesso ao RailOps, com criação de
conta automática no primeiro login (padrão *Trust On First Use*). Essa
proposta foi avaliada e **rejeitada** pelos motivos descritos abaixo, o que
levou ao desenho final registrado neste ADR.

## Alternativas Consideradas

**1. Autenticação própria construída do zero**, sem bibliotecas
especializadas de segurança. Máximo valor de aprendizado, mas risco maior
de erros sutis de implementação em uma área sensível (hashing de senha,
gestão de tokens).

**2. Supabase Authentication.** Solução pronta e testada em produção por
terceiros, reduz esforço de implementação. Desvantagem: desenhada em torno
de e-mail como identificador principal, exigindo adaptação (ex: e-mail
sintético) para funcionar com login por matrícula.

**3. Autenticação própria, apoiada em bibliotecas especializadas do
ecossistema Python** (ex.: `passlib` para hashing de senha, `python-jose`
ou `pyjwt` para tokens JWT). Equilibra aprendizado real do fluxo de
autenticação com segurança madura nas partes mais sensíveis, sem exigir
adaptação para o login por matrícula.

**Proposta inicial do Product Owner (descartada):** reutilizar a senha do
portal corporativo da empresa, com cadastro automático da matrícula no
primeiro login bem-sucedido.

- **Risco 1 — Reutilização de credencial entre sistemas.** Mesmo
  armazenando apenas o hash da senha, pedir ao operador que digite, em um
  sistema novo e paralelo, a mesma senha que dá acesso a sistemas
  corporativos sensíveis (e-mail, RH, folha de pagamento) cria uma
  superfície de ataque em cascata: uma eventual vulnerabilidade no RailOps
  passaria a comprometer também os sistemas principais da empresa.
- **Risco 2 — Sequestro de identidade via Trust On First Use.** Matrículas
  não são informação sigilosa (aparecem em escalas e crachás). Se
  qualquer pessoa pudesse "cadastrar" uma matrícula alheia ao simplesmente
  ser a primeira a tentar login com ela, a rastreabilidade das passagens
  de serviço — um dos objetivos centrais do projeto — ficaria
  comprometida.

## Decisão

Adotar a **Abordagem 3 — Autenticação própria em FastAPI**, apoiada em
bibliotecas especializadas para hashing de senha e geração/validação de
JWT, com o seguinte modelo de acesso:

- Matrículas autorizadas a acessar o sistema são **pré-cadastradas**
  previamente (pelo administrador do sistema), e não criadas livremente
  por quem tentar logar primeiro.
- No primeiro acesso a uma matrícula já pré-cadastrada, o operador
  **define seu próprio PIN/senha**, específico do RailOps — sem qualquer
  reaproveitamento de senha de outros sistemas da empresa.
- Login subsequente compara o PIN/senha informado com o hash armazenado
  na primeira definição.

## Justificativa

- Elimina os dois riscos de segurança identificados na proposta inicial,
  sem sacrificar a simplicidade de uso desejada pelo Product Owner (PIN
  curto, sem necessidade de decorar senha complexa nova).
- Mantém o valor de aprendizado do fluxo de autenticação real (hashing,
  tokens, expiração de sessão), alinhado ao objetivo educacional do
  projeto.
- Evita a necessidade de adaptação para login por matrícula que a Opção 2
  (Supabase Auth) exigiria.

## Consequências

- Será necessário, na Fase 5 (Modelagem do Banco), desenhar uma tabela de
  usuários com um campo de status (ex.: `pin_definido: boolean`) para
  diferenciar matrículas pré-cadastradas aguardando primeiro acesso das já
  ativas.
- O processo de pré-cadastro de matrículas autorizadas precisa de um fluxo
  administrativo próprio (mesmo que simples, como uma tela ou comando
  restrito ao Product Owner/administrador) — a ser detalhado na Fase 6/8.
- A biblioteca específica de hashing (`passlib`) e de tokens (`python-jose`
  ou `pyjwt`) será definida durante a Fase 9 (Implementação), com base na
  compatibilidade com a versão do FastAPI utilizada.
