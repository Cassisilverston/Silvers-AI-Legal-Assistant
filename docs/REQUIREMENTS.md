# Engenharia de Requisitos — Silvers AI Legal Assistant (Procon Disputes)

## 1) Contexto e Problema
Em disputas de consumo, o maior gargalo prático é a **organização do caso**:
- fatos narrados sem ordem cronológica,
- evidências dispersas (prints, e-mails, protocolos),
- inconsistência de valores (frete, taxas, câmbio),
- dificuldade em transformar dados em **argumentos objetivos** para audiência (ex.: Procon).

Este sistema resolve isso através de **estruturação** e **assistência por IA** (síntese e argumentação inicial baseadas na timeline + evidências).

---

## 2) Objetivos do Produto
### Objetivos (o que deve entregar)
- Permitir criar uma disputa e **montar um dossiê confiável** (fatos + evidências + valores + partes).
- Garantir consistência (datas, vínculos, regras básicas de integridade).
- Gerar automaticamente:
  - **Síntese do caso**
  - **Tópicos de argumentação inicial** (não “sentença”, e sim rascunho assistido)

### Não objetivos (por agora)
- Decidir o resultado jurídico.
- Protocolar em sistemas oficiais.
- Substituir atuação profissional.

---

## 3) Stakeholders & Personas
- **Advogado(a)/Assistente Jurídico**: quer rapidez + qualidade + rastreabilidade.
- **Consumidor (autoatendimento futuro)**: quer registrar fatos/evidências sem saber “juridiquês”.
- **Gestor do escritório (futuro)**: quer visão por caso, prazos e status.

---

## 4) Glossário (Linguagem Ubíqua)
- **Dispute/Litigation (Disputa)**: unidade central do caso.
- **Party (Parte)**: consumidor, fornecedor (parte contrária), intermediários.
- **TimelineEvent (Evento)**: fato datado (compra, contato, negativa, entrega, devolução).
- **Evidence (Evidência)**: artefato que comprova algo (print, e-mail, comprovante).
- **ProtocolNumber (Protocolo)**: identificador de atendimento (Procon/fornecedor/cartão).
- **Synthesis (Síntese)**: resumo objetivo do caso.
- **Arguments (Argumentos iniciais)**: tópicos estruturados com base nos fatos/evidências.

---

## 5) Requisitos Funcionais (User Stories + Critérios de Aceite)

### EPIC A — Gestão de Disputas (Dispute)
**US-A1 — Criar disputa**
- Como usuário, quero criar uma disputa informando título, categoria e descrição curta, para iniciar a organização do caso.
**Aceite**
- Deve ser possível criar disputa com:
  - Título (obrigatório)
  - Categoria (ex.: reembolso, entrega, produto defeituoso) (obrigatório)
  - Descrição curta (opcional)
- Deve gerar um identificador único.
- Deve registrar data de criação.

**US-A2 — Editar dados da disputa**
- Como usuário, quero atualizar informações da disputa para manter o dossiê correto.
**Aceite**
- Deve permitir alterar título, categoria e descrição curta.
- Deve manter histórico de atualização (mínimo: timestamp de última alteração).

**US-A3 — Visualizar dossiê da disputa**
- Como usuário, quero visualizar o dossiê consolidado para revisar o caso antes da audiência.
**Aceite**
- Deve exibir: partes, timeline ordenada, evidências, valores (se houver), status.
- A timeline deve estar ordenada por data/hora do evento (asc).

---

### EPIC B — Partes (Parties)
**US-B1 — Cadastrar parte consumidora**
- Como usuário, quero cadastrar a parte consumidora para identificar corretamente quem reclama.
**Aceite**
- Campos mínimos: nome, documento (CPF/CNPJ opcional no MVP), e-mail/telefone opcionais.
- Documento, se informado, deve validar formato básico (sem validação governamental).

**US-B2 — Cadastrar parte contrária (fornecedor)**
- Como usuário, quero cadastrar a parte contrária para vincular contatos e obrigações.
**Aceite**
- Campos mínimos: nome/razão social, país/marketplace (opcional), canal de suporte (opcional).
- Permitir múltiplas partes (ex.: marketplace + vendedor + transportadora) (opcional no MVP; se não, ao menos 1 fornecedor).

---

### EPIC C — Linha do Tempo (Timeline)
**US-C1 — Adicionar evento na timeline**
- Como usuário, quero registrar um evento com data/hora e descrição, para construir a cronologia exata.
**Aceite**
- Evento deve ter:
  - Data/hora (obrigatório)
  - Tipo (ex.: compra, contato suporte, resposta, envio, entrega, negativa) (obrigatório)
  - Descrição (obrigatório)
- Deve permitir associar evento a uma parte (opcional).

**US-C2 — Garantir consistência cronológica**
- Como usuário, quero que o sistema valide datas incoerentes para evitar cronologias falsas.
**Aceite**
- Ao inserir evento com data muito anterior/posterior sem contexto:
  - MVP: apenas validação simples (não permitir data futura **se o evento for “ocorrido”**).
  - Permitir evento futuro somente se tipo indicar “prazo agendado” (ex.: audiência, prazo de resposta).
- Deve ordenar sempre a timeline.

**US-C3 — Editar/remover evento**
- Como usuário, quero corrigir eventos para manter o caso fiel aos fatos.
**Aceite**
- Permitir editar campos do evento.
- Remover evento deve:
  - impedir remoção se existirem evidências vinculadas **sem tratamento** (ver US-D2).

---

### EPIC D — Evidências (Evidence)
**US-D1 — Anexar evidência**
- Como usuário, quero registrar evidências para sustentar os fatos.
**Aceite**
- Evidência deve ter:
  - Tipo (print, e-mail, comprovante, conversa, protocolo)
  - Descrição curta
  - Fonte (opcional: e-mail, WhatsApp, site)
  - Data/hora (opcional)
  - Referência (link/arquivo) (no MVP pode ser apenas metadata; upload pode vir depois)
- Evidência deve ser vinculada:
  - à disputa (obrigatório) e
  - opcionalmente a um evento da timeline.

**US-D2 — Integridade evidência-evento**
- Como usuário, quero evitar evidências “órfãs” ou inconsistentes.
**Aceite**
- Se um evento for removido:
  - o sistema deve impedir a remoção se houver evidências vinculadas, **ou**
  - exigir remapeamento/desvinculação (definir regra do domínio; no MVP: bloquear remoção).
- O dossiê deve indicar evidências sem evento (se existirem).

---

### EPIC E — Valores e Reembolso (Money)
**US-E1 — Registrar valores relevantes**
- Como usuário, quero registrar valores (produto, frete, taxas) para calcular o prejuízo.
**Aceite**
- Permitir registrar valores em moeda + quantia:
  - valor do produto
  - frete
  - taxas
- Permitir informar moeda (ex.: BRL, USD).
- MVP: não exige conversão automática; apenas armazenar corretamente.

**US-E2 — Registrar pedidos e ofertas**
- Como usuário, quero registrar o que foi solicitado e o que foi ofertado para comparar propostas.
**Aceite**
- Campos:
  - pedido do consumidor (ex.: reembolso integral)
  - oferta do fornecedor (ex.: cupom parcial)
  - datas associadas (opcional)

---

### EPIC F — IA (Síntese e Argumentos)
**US-F1 — Gerar síntese do caso**
- Como usuário, quero gerar uma síntese baseada na timeline + evidências para levar à audiência.
**Aceite**
- Entrada: disputa + timeline + evidências
- Saída: texto estruturado com:
  - contexto
  - principais eventos (ordem temporal)
  - pontos de prova (evidências-chave)
  - pedido do consumidor
- Deve incluir aviso de que é rascunho e requer revisão humana.

**US-F2 — Gerar tópicos de argumentação inicial**
- Como usuário, quero obter tópicos de argumento para negociar melhor no Procon.
**Aceite**
- Saída em tópicos:
  - tese fática (o que ocorreu)
  - falha do fornecedor (em linguagem objetiva)
  - pedido/pretensão
  - alternativas (acordo: integral / parcial / etc.)
- Deve referenciar (por ID/título) eventos e evidências usados como base (rastreabilidade).

**US-F3 — Controle de privacidade na IA**
- Como usuário, quero evitar envio indevido de dados sensíveis ao modelo.
**Aceite**
- Deve existir um modo “redigir/mascarar” campos sensíveis (MVP: mascarar documento e e-mail antes de enviar).
- Registrar log do prompt “sanitizado” (sem dados sensíveis brutos).

---

### EPIC G — Exportação do Dossiê
**US-G1 — Exportar dossiê em formato texto/markdown**
- Como usuário, quero exportar o dossiê para enviar/colar em documentos.
**Aceite**
- Exportação deve conter:
  - identificação do caso
  - partes
  - timeline ordenada
  - evidências com vínculos
  - valores
  - síntese e argumentos (se gerados)
- Deve manter IDs/referências para rastreabilidade.

---

## 6) Requisitos Não Funcionais (NFRs)
- **Auditabilidade**: toda saída de IA deve ser rastreável (quais eventos/evidências foram usados).
- **Testabilidade**: regras de domínio testadas com xUnit; Application testável via handlers.
- **Manutenibilidade**: separação por camadas (Clean Architecture) e baixa acoplagem.
- **Segurança/Privacidade (MVP)**:
  - mascarar documentos/e-mails antes de IA
  - não persistir segredos no repositório
- **Performance (MVP)**:
  - timeline e evidências devem carregar em tempo aceitável para casos médios (centenas de itens).

---

## 7) Regras de Negócio (Domínio) — primeiras definições
- Uma **Disputa** deve ter ao menos 1 parte consumidora e 1 parte contrária (para estar “Pronta para síntese”).
- Evento “ocorrido” não pode estar no futuro.
- Evidência deve pertencer a exatamente 1 disputa.
- Evento com evidências vinculadas não pode ser removido (MVP).
- Valores devem ter moeda válida (ISO code) e quantia > 0.

---

## 8) Entregáveis (por fase)
**Fase 1**
- README.md
- REQUIREMENTS.md

**Fase 2**
- diagrama de arquitetura (texto/mermaid/draw.io)
- estrutura inicial da solução e pastas

**Fase 3**
- GitHub Action (build + test)
- templates de PR/Issues
- convenções

---

## 9) Critérios de Pronto (Definition of Done)
Uma história está “Done” quando:
- implementada
- testada
- documentada (README/ADRs se decisão relevante)
- CI passando em PR
- revisada via PR

---

## 10) Riscos & Assunções
- IA pode “alucinar”: mitigação via rastreabilidade + aviso + revisão humana obrigatória.
- Evidências podem conter dados sensíveis: mitigação via mascaramento e políticas de logging.
- MVP não terá upload/armazenamento real de arquivos (apenas metadata): risco aceito.

---