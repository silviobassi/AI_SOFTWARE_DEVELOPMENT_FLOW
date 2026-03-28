# AI-Assisted Software Development Flow

> Metodologia para desenvolvimento de software com IA — Solo Dev + AI Agent
> Versão: 1.1 | Data: 2026-03-28

---

## Diagrama do Fluxo

```mermaid
flowchart TD
    classDef phase fill:#1e3a5f,stroke:#4a90d9,color:#ffffff,font-weight:bold
    classDef artifact fill:#0d2b0d,stroke:#4caf50,color:#ffffff
    classDef gate fill:#3d1a00,stroke:#ff8c00,color:#ffffff,font-weight:bold
    classDef optional fill:#1a1a2e,stroke:#7b68ee,color:#cccccc,stroke-dasharray:5 5
    classDef skill fill:#1a0d2e,stroke:#9b59b6,color:#ffffff
    classDef rule fill:#2e0d0d,stroke:#e74c3c,color:#ffffff

    %% FASE 1
    subgraph F1["FASE 1 — ENTENDIMENTO"]
        A["📄 PRD Alto Nível (stakeholder-facing)"]
        G1{{"✅ Checkpoint 1 O problema está claro? Critérios de sucesso mensuráveis?"}}
        B["📄 PRD Técnico (engineering-facing)"]
        G2{{"✅ Checkpoint 2 Stack, escopo e RNFs estão bem definidos?"}}
    end

    %% FASE 2
    subgraph F2["FASE 2 — ARQUITETURA"]
        C["🏗️ Extração de Arquitetura"]
        C1["Diagramas C4 / Sequência / ER"]
        C2["Requisitos Funcionais e Não Funcionais"]
        C3["ADRs (decisões com justificativa)"]
        C4["RFCs (opcional)"]
        G3{{"✅ Checkpoint 3 Arquitetura responde todos os RNFs? Decisões críticas têm ADR?"}}
    end

    %% FASE 3
    subgraph F3["FASE 3 — CONFIGURAÇÃO DO AGENTE"]
        D["🤖 CLAUDE.md / AGENTS.md (contexto permanente — stack, anti-padrões, refs)"]
        E1["📘 Skills (.claude/skills/) file-structure obrigatória"]
        E2["📏 Rules (.claude/rules/) rastreability · spec-before-code · atomic-task"]
    end

    %% FASE 4
    subgraph F4["FASE 4 — ESPECIFICAÇÕES"]
        F["📋 Feature Spec [ID]-[nome].spec.md (O QUÊ — requisitos, BDD, contrato)"]
        G4{{"✅ Checkpoint 4 Spec está completa? Critérios de aceite testáveis? Dependências mapeadas?"}}
    end

    %% FASE 5
    subgraph F5["FASE 5 — PLANEJAMENTO DE EXECUÇÃO"]
        H["🗂️ Execution Plan [ID]-execution-plan.md (O COMO — tasks, DAG, prompts)"]
        H1["DAG de Dependências + Paralelismo"]
        H2["Testes definidos por Task (TDD-first)"]
        H3["Prompt base por Task"]
        H4["Estimativa de Contexto por Task (≤ 5 arquivos)"]
    end

    %% FASE 6
    subgraph F6["FASE 6 — IMPLEMENTAÇÃO"]
        I["💻 Execução pelo Agente (task por task — execution plan)"]
        I1["Código implementado"]
        I2["Testes escritos"]
        I3["// spec: ID (RULE-rastreability)"]
        J["🔍 Validação Automatizada Lint + Tests + Build"]
    end

    %% FASE 7
    subgraph F7["FASE 7 — ENTREGA"]
        K["👁️ Code Review (auto-revisão + agente vs spec)"]
        L["🚀 Merge + CI/CD"]
        M["📝 Atualização de ADRs se necessário"]
    end

    %% CONEXÕES PRINCIPAIS
    A --> G1 --> B --> G2
    G2 --> C --> C1 & C2 & C3 & C4
    C1 & C2 & C3 & C4 --> G3
    G3 --> D
    D --> E1 & E2
    E1 & E2 --> F --> G4
    G4 --> H --> H1 & H2 & H3 & H4
    H1 & H2 & H3 & H4 --> I
    I --> I1 & I2 & I3
    I1 & I2 & I3 --> J
    J --> K --> L --> M

    %% FEEDBACK LOOPS
    G1 -. "refinar" .-> A
    G2 -. "refinar" .-> B
    G3 -. "revisar arquitetura" .-> C
    G4 -. "completar spec" .-> F
    J -. "falha: fix & retry" .-> I
    K -. "ajuste" .-> I

    %% ESTILOS
    class F1,F2,F3,F4,F5,F6,F7 phase
    class A,B,C,C1,C2,C3,D,F,H,H1,H2,H3,I,I1,I2,J,K,L,M artifact
    class G1,G2,G3,G4 gate
    class C4,H4 optional
    class E1 skill
    class E2,I3 rule
```

---

## Separação de Responsabilidades por Artefato

| Artefato | Responde a | Não contém |
| --- | --- | --- |
| **CLAUDE.md** | Padrões permanentes (stack, anti-padrões, refs a skills/rules) | Requisitos de feature, lógica de negócio |
| **Skill** `file-structure` | Naming, estrutura de pastas, padrão arquitetural | Requisitos, constraints globais |
| **Rule** | Constraint sempre ativa (rastreability, spec-before-code...) | Instruções situacionais |
| **PRD Técnico** | Requisitos do sistema (RF, RNF, arquitetura) | Tasks, planos de execução |
| **ADR** | Decisão arquitetural com trade-offs | Instruções de implementação |
| **Spec** | O QUÊ implementar (requisitos, BDD, contrato) | COMO implementar, tasks, padrões de stack |
| **Execution Plan** | O COMO executar (tasks, DAG, prompts base) | Requisitos, critérios de aceite |

---

## Escala do Projeto

O fluxo é **escalável**. Para projetos pequenos, algumas etapas são opcionais:

| Etapa | Projeto Pequeno | Projeto Médio | Projeto Grande |
| --- | --- | --- | --- |
| PRD Alto Nível | Opcional | ✅ | ✅ |
| PRD Técnico | ✅ Simplificado | ✅ | ✅ Completo |
| Diagramas | Opcional | ✅ Principais | ✅ Todos |
| ADRs | 1–2 críticos | ✅ | ✅ Completo |
| RFCs | ❌ | Opcional | ✅ |
| CLAUDE.md | ✅ Essencial | ✅ | ✅ |
| Skill `file-structure` | ✅ Essencial | ✅ | ✅ Completa |
| Rules base | ✅ 3 padrões | ✅ | ✅ Customizadas |
| Specs | ✅ Por feature | ✅ | ✅ |
| Execution Plan | ✅ Simplificado | ✅ | ✅ Completo |
| CI/CD | Básico | ✅ | ✅ Completo |

---

## Fluxo de Contexto do Agente

```mermaid
flowchart LR
    subgraph PERM["Contexto Permanente"]
        CM["CLAUDE.md (stack · anti-padrões · refs)"]
        SK["Skills (.claude/skills/)"]
        RU["Rules (.claude/rules/)"]
    end

    subgraph FEAT["Contexto de Feature"]
        SP["Spec [ID]-[nome].spec.md (O QUÊ)"]
        EP["Execution Plan [ID]-execution-plan.md (O COMO)"]
    end

    subgraph TASK["Contexto de Task"]
        TP["Task Prompt (instrução mínima + arquivos da task)"]
    end

    MEM["memory/ (estado entre sessões)"]

    PERM -->|"sempre carregado"| AGT["🤖 Agente"]
    SP   -->|"início de feature"| AGT
    EP   -->|"início de feature"| AGT
    TP   -->|"por task"| AGT
    MEM  -->|"quando relevante"| AGT
```

**Regra de ouro para economizar tokens:**

| Camada | O que contém | Quando carregar |
| --- | --- | --- |
| `CLAUDE.md` + Skills + Rules | Sempre válido — stack, padrões, constraints | Toda sessão (automático) |
| `Spec` | Válido para esta feature — O QUÊ | Início da feature |
| `Execution Plan` | Válido para esta feature — O COMO | Início da feature |
| `Task Prompt` | Instrução mínima + arquivos desta task | Por task |
| `memory/` | Estado que deve persistir entre sessões | Quando relevante |

> **Sinal de contexto degradado:** agente faz perguntas já respondidas na spec, repete código
> escrito ou ignora uma rule. Ação: nova sessão com CLAUDE.md + spec + execution plan.
