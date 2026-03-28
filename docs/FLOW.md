# AI-Assisted Software Development Flow

> Metodologia para desenvolvimento de software com IA — Solo Dev + AI Agent
> Versão: 1.0 | Data: 2026-03-28

---

## Diagrama do Fluxo

```mermaid
flowchart TD
    classDef phase fill:#1e3a5f,stroke:#4a90d9,color:#ffffff,font-weight:bold
    classDef artifact fill:#0d2b0d,stroke:#4caf50,color:#ffffff
    classDef gate fill:#3d1a00,stroke:#ff8c00,color:#ffffff,font-weight:bold
    classDef optional fill:#1a1a2e,stroke:#7b68ee,color:#cccccc,stroke-dasharray:5 5
    classDef feedback stroke:#e74c3c,stroke-width:2px,stroke-dasharray:4 4

    %% FASE 1
    subgraph F1["FASE 1 — ENTENDIMENTO"]
        A["📄 PRD Alto Nível\n(stakeholder-facing)"]
        G1{{"✅ Checkpoint 1\nLeitura crítica própria:\nO problema está claro?"}}
        B["📄 PRD Técnico\n(engineering-facing)"]
        G2{{"✅ Checkpoint 2\nStack, escopo e restrições\nestão bem definidos?"}}
    end

    %% FASE 2
    subgraph F2["FASE 2 — ARQUITETURA"]
        C["🏗️ Extração de Arquitetura"]
        C1["Diagramas\nC4 / Sequência / ER"]
        C2["Requisitos\nFuncionais e Não Funcionais"]
        C3["ADRs\n(decisões com justificativa)"]
        C4["RFCs\n(opcional: mudanças\nsignificativas)"]
        G3{{"✅ Checkpoint 3\nArquitetura responde\ntodos os RNFs?"}}
    end

    %% FASE 3
    subgraph F3["FASE 3 — CONFIGURAÇÃO DO AGENTE"]
        D["🤖 CLAUDE.md / AGENTS.md\n(contexto permanente do agente)"]
        E["⚙️ Skills e Rules\n(comportamentos reutilizáveis)"]
    end

    %% FASE 4
    subgraph F4["FASE 4 — ESPECIFICAÇÕES"]
        F["📋 Feature Specs\n(uma por funcionalidade)"]
        G4{{"✅ Checkpoint 4\nSpec está completa?\nCritérios de aceite definidos?"}}
    end

    %% FASE 5
    subgraph F5["FASE 5 — PLANEJAMENTO DE EXECUÇÃO"]
        H["🗂️ Plano de Execução por Spec"]
        H1["DAG de Dependências\nentre Tasks"]
        H2["Paralelismo\nidentificado"]
        H3["Testes definidos\npor Task (TDD-first)"]
        H4["Estimativa de contexto\nnecessário por Task"]
    end

    %% FASE 6
    subgraph F6["FASE 6 — IMPLEMENTAÇÃO"]
        I["💻 Execução pelo Agente\n(task por task)"]
        I1["Código\nimplementado"]
        I2["Testes\nescritos"]
        I3["Rastreabilidade\nspec ↔ código"]
        J["🔍 Validação Automatizada\nLint + Tests + Build"]
    end

    %% FASE 7
    subgraph F7["FASE 7 — ENTREGA"]
        K["👁️ Code Review\n(auto-revisão + agente)"]
        L["🚀 Merge + CI/CD"]
        M["📝 Atualização de ADRs\nse necessário"]
    end

    %% CONEXÕES PRINCIPAIS
    A --> G1 --> B --> G2
    G2 --> C --> C1 & C2 & C3 & C4
    C1 & C2 & C3 & C4 --> G3
    G3 --> D --> E
    E --> F --> G4
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
    class A,B,C,C1,C2,C3,D,E,F,H,H1,H2,H3,I,I1,I2,I3,J,K,L,M artifact
    class G1,G2,G3,G4 gate
    class C4,H4 optional
```

---

## Escala do Projeto

O fluxo é **escalável**. Para projetos pequenos, algumas etapas são opcionais:

| Etapa | Projeto Pequeno | Projeto Médio | Projeto Grande |
| --- | --- | --- | --- |
| PRD Alto Nível | Opcional | ✅ | ✅ |
| PRD Técnico | ✅ Simplificado | ✅ | ✅ Completo |
| Diagramas | Opcional | ✅ Principais | ✅ Todos |
| ADRs | 1–2 decisões críticas | ✅ | ✅ Completo |
| RFCs | ❌ | Opcional | ✅ |
| CLAUDE.md | ✅ Essencial | ✅ | ✅ |
| Skills e Rules | Reutilizar existentes | ✅ | ✅ Customizadas |
| Specs | ✅ Por feature | ✅ | ✅ |
| CI/CD | Básico | ✅ | ✅ Completo |

---

## Fluxo de Contexto do Agente

```mermaid
flowchart LR
    CM["CLAUDE.md\n(contexto permanente\nentre sessões)"]
    SP["Spec\n(contexto da feature\ncorrente)"]
    TP["Task Prompt\n(contexto mínimo\nda task)"]
    MEM["memory/\n(estado da sessão)"]

    CM -->|"sempre carregado"| AGT["🤖 Agente"]
    SP -->|"início de feature"| AGT
    TP -->|"por task"| AGT
    MEM -->|"quando relevante"| AGT
```

**Regra de ouro para economizar tokens:**

- `CLAUDE.md` = o que **sempre** vale → stack, padrões, anti-padrões, estrutura de pastas
- `Spec` = o que vale **para a feature** → requisitos, aceite, dependências
- `Task prompt` = o que vale **para essa task** → instrução objetiva, contexto mínimo
- `memory/` = decisões e estado que precisam **persistir entre sessões**
