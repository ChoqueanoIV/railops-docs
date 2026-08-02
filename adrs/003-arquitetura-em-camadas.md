# ADR-003 — Arquitetura em Camadas do Backend

**Status:** Aceito
**Data:** 30/07/2026
**Decisor:** Product Owner, com recomendação técnica do Desenvolvedor Sênior

---

## Contexto

Com o versionamento de histórico (ADR-001) e a estratégia de autenticação
(ADR-002) definidos, era necessário decidir como o código do backend
FastAPI seria organizado internamente, de forma a acomodar de maneira
sustentável as regras de negócio já documentadas (RN01 a RN09) conforme o
projeto avança para as fases de implementação.

## Alternativas Consideradas

**1. Tudo em um único arquivo/camada técnica.** Rotas, lógica de negócio e
acesso ao banco de dados misturados no mesmo arquivo. Rápido para
prototipagem inicial, mas rapidamente se torna difícil de testar,
manter e localizar regras específicas ("arquitetura em espaguete"),
inadequado para um projeto com volume relevante de regras de negócio
documentadas.

**2. Arquitetura em Camadas (Routers → Services → Repositories).** Separa
responsabilidades em três camadas: rotas (recebem e validam requisições
HTTP), serviços (concentram a lógica de negócio, ex. RN01 a RN09) e
repositórios (única camada que acessa o PostgreSQL). Padrão amplamente
utilizado no mercado para APIs REST de porte médio.

**3. Arquitetura Hexagonal / Clean Architecture.** Evolução da Abordagem
2, com inversão de dependência — a camada de serviço não conhece a
implementação concreta de persistência, apenas uma interface abstrata.
Maior flexibilidade e testabilidade, porém complexidade desproporcional
ao estágio e escopo atuais do projeto (over-engineering).

## Decisão

Adotar a **Abordagem 2 — Arquitetura em Camadas**, estruturada como:

```
backend/
└── app/
    ├── routers/       # camada de rotas (FastAPI, Pydantic)
    ├── services/       # camada de regras de negócio
    ├── repositories/    # camada de acesso a dados (PostgreSQL)
    ├── models/         # modelos de dados (ORM / Pydantic)
    └── core/          # configurações, autenticação, dependências
```

## Justificativa

- Padrão amplamente adotado no mercado para projetos FastAPI de porte
  equivalente, com valor direto para o objetivo de portfólio e preparação
  para entrevistas técnicas do Product Owner.
- Permite testar a lógica de negócio (camada de serviço) de forma isolada
  via Pytest, sem dependência de um banco de dados real em execução —
  essencial para cobrir adequadamente as regras RN01 a RN09.
- Evita tanto a desorganização da Abordagem 1 quanto a complexidade
  desnecessária da Abordagem 3 para o tamanho atual do projeto.
- Mantém rastreabilidade clara entre regra de negócio e código: cada RN
  documentada deve corresponder a uma função identificável na camada de
  serviço.

## Consequências

- A estrutura de pastas acima deverá ser criada no repositório
  `railops-app` no início da Fase 9 (Implementação).
- Toda nova funcionalidade implementada deverá seguir o fluxo
  Router → Service → Repository, sem exceções, para manter a consistência
  arquitetural.
- Testes automatizados (Fase 10) devem, sempre que possível, testar a
  camada de serviço isoladamente, usando repositórios substitutos (mocks
  ou fakes) para evitar dependência de banco real nos testes unitários.
- Caso o projeto cresça significativamente em complexidade ou número de
  integrações externas no futuro, esta decisão poderá ser revisitada em um
  novo ADR, evoluindo para Arquitetura Hexagonal.
