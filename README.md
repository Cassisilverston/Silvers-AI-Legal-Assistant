# Silvers AI Legal Assistant (Procon Disputes)
Copiloto jurídico para **estruturar disputas de consumo** (ex.: audiência no **Procon**) com foco em:
- cadastro de partes (consumidor x fornecedor),
- **cronologia exata dos fatos**,
- organização de **evidências** (prints/e-mails/protocolos),
- preparação de **síntese do caso** e **argumentação inicial** assistidas por IA.

> Objetivo do projeto: demonstrar engenharia de software de nível corporativo (SDLC + Agile + DDD + Clean Architecture + CQRS + testes + CI).

---

## Visão do Produto
Processos administrativos/consumeristas costumam falhar não por falta de “direito”, mas por falta de **estrutura**:
- fatos fora de ordem,
- evidências sem vínculo com eventos,
- valores e prazos inconsistentes,
- argumentos repetitivos e sem lastro nos documentos.

Este sistema cria um **dossiê estruturado**, legível e auditável: do registro do caso até um pacote de preparação para audiência.

---

## Caso de Uso âncora (Procon — Reembolso internacional)
Exemplo: compra internacional (ex.: vestido de noiva), com disputa que se arrasta por meses.  
O usuário precisa:
1) registrar fornecedor e consumidor,  
2) registrar compra, tentativas de suporte e protocolos,  
3) anexar evidências (prints, e-mails, comprovantes),  
4) gerar uma síntese + tópicos de argumentação baseados na cronologia/evidências.

---

## Escopo (MVP)
- CRUD de Disputa (Dispute/Litigation)
- Partes (Consumer, Supplier/Defendant)
- Linha do tempo (TimelineEvent) com ordenação e consistência
- Evidências (Evidence) vinculadas a eventos e/ou à disputa
- Exportação/relatório (texto/markdown) do dossiê
- IA: geração de **síntese** e **argumentos iniciais** com base nos dados estruturados

### Fora do escopo (por enquanto)
- Peticionamento eletrônico e integrações com tribunais
- OCR/extração avançada de PDFs e imagens
- Controle financeiro completo/contábil
- Múltiplos escritórios e permissões complexas (RBAC avançado)

---

## Arquitetura & Padrões
**Clean Architecture**
- `Domain`: entidades, value objects, regras de negócio
- `Application`: casos de uso (CQRS), validações, interfaces
- `Infrastructure`: EF Core, PostgreSQL, integrações externas, IA
- `Presentation`: Minimal APIs, Scalar/OpenAPI

**CQRS + MediatR**
- Commands: escrita (criar disputa, adicionar evento, anexar evidência…)
- Queries: leitura (detalhe do dossiê, timeline ordenada, evidências por evento…)

**DDD (Tactical Design)**
- Entidades ricas: `Dispute`, `Party`, `TimelineEvent`, `Evidence`
- Value Objects: `Money`, `DocumentNumber`, `EmailAddress`, `PhoneNumber`, `ProtocolNumber`, `DateRange`
- Regras de domínio: consistência de datas, valores, prazos, vínculo de evidência, integridade do dossiê

---

## Stack (prevista)
- .NET (C#)
- Minimal APIs + Scalar/OpenAPI
- PostgreSQL
- Entity Framework Core (migrations)
- MediatR
- xUnit (testes unitários)
- Semantic Kernel (orquestração IA)

---

## Estrutura do Repositório (proposta)

```text
/
├── docs
│   ├── architecture
│   │   └── decisions (ADRs)
│   ├── diagrams
│   └── REQUIREMENTS.md
├── src
│   ├── Silvers.AI.LegalAssistant.Application
│   ├── Silvers.AI.LegalAssistant.Domain
│   ├── Silvers.AI.LegalAssistant.Infrastructure
│   └── Silvers.AI.LegalAssistant.Presentation
├── tests
│   ├── Silvers.AI.LegalAssistant.Application.Tests
│   └── Silvers.AI.LegalAssistant.Domain.Tests
└── README.md
```


---

## Qualidade & Governança
- Pull Requests obrigatórios
- Branches com padrão: `feature/...`, `fix/...`, `chore/...`, `docs/...`
- CI em PR: build + testes
- Convenções:
  - Commits: Conventional Commits
  - ADRs para decisões importantes

---

## Como rodar

- Requisitos: .NET SDK, PostgreSQL
- `dotnet restore`
- `dotnet test`
- `dotnet run --project src/Silvers.AI.LegalAssistant.Presentation`

---

## Roadmap de Entregas
1) **Fase 1**: requisitos (README + REQUIREMENTS) ✅
2) **Fase 2**: diagrama arquitetura + estrutura de solução
3) **Fase 3**: CI básico + convenções + templates de PR
4) **Fase 4**: modelagem de domínio + testes unitários
5) **Fase 5**: EF Core + PostgreSQL + IA (Semantic Kernel)
6) **Fase 6**: Minimal APIs + Swagger + deploy (posteriormente)

---

## Licença


---

## Status
🚧 Em construção — este repositório segue SDLC com entregas incrementais e rastreáveis.
