# AI-Assisted Software Development Flow

> Kit de metodologia para desenvolvimento de software com IA.
> Contexto: Solo Developer + AI Agent | Projetos variados | Agnóstico de stack.

---

## O que é isso

Um conjunto de documentos, templates e padrões que definem um fluxo completo de desenvolvimento
de software, desde a coleta de requisitos até a entrega, com IA integrada em cada etapa.

**Objetivos:**

- Minimizar retrabalho via spec-before-code
- Economizar tokens com contexto bem gerenciado
- Garantir rastreabilidade vertical (PRD → Spec → Código → Teste)
- Produzir software com qualidade e manutenibilidade

---

## Estrutura

```gherkin
AI_Software_Development_Flow/
├── README.md
├── docs/
│   ├── FLOW.md                                # Diagrama Mermaid do fluxo + fluxo de contexto
│   └── methodology.md                         # Documento formal da metodologia (7 fases)
└── templates/
    │
    │  ── PROJETO ──────────────────────────────
    ├── CLAUDE.md.template.md                  # Contexto permanente do agente (Skills + Rules)
    ├── prd-technical.template.md              # PRD Técnico
    ├── adr.template.md                        # Architecture Decision Record
    │
    │  ── FEATURE ──────────────────────────────
    ├── spec.template.md                       # Spec de feature (contrato do que implementar)
    ├── execution-plan.template.md             # Plano de execução autônomo por feature
    │
    │  ── SKILLS & RULES ────────────────────────
    ├── skill.template.md                      # Meta-template para criar qualquer skill
    ├── skill-file-structure.template.md       # Skill de estrutura + 4 padrões arquiteturais
    └── rule.template.md                       # Template para criar rules (constraints)
```

---

## Como usar em um novo projeto

### Fase 1–2: Entendimento + Arquitetura

```flow
templates/prd-technical.template.md   → docs/prd-technical.md
templates/adr.template.md             → docs/architecture/decisions/ADR-NNN-titulo.md
```

### Fase 3: Configurar o Agente

```gherkin
templates/CLAUDE.md.template.md       → CLAUDE.md (raiz do projeto)

templates/skill-file-structure.template.md
  → escolher arquitetura (CRUD, Vertical Slice, Hexagonal, Clean)
  → preencher com padrões do projeto
  → .claude/skills/file-structure.md

templates/rule.template.md            → .claude/rules/rastreability.md
templates/rule.template.md            → .claude/rules/spec-before-code.md
templates/rule.template.md            → .claude/rules/atomic-task.md
```

### Fase 4–5: Especificações + Planejamento

```gherkin
Para cada feature:
  templates/spec.template.md           → docs/specs/[ID]-[nome].spec.md
  templates/execution-plan.template.md → docs/specs/[ID]-execution-plan.md
```

### Fase 6–7: Implementação + Entrega

```gherkin
Seguir o execution plan task a task.
Agente usa: CLAUDE.md + spec + execution plan como contexto.
```

---

## Separação de Responsabilidades dos Templates

| Template | Responde a | Não contém |
| --- | --- | --- |
| `CLAUDE.md` | Padrões permanentes do projeto (stack, anti-padrões, skills, rules) | Requisitos de features, lógica de negócio |
| `prd-technical` | Requisitos do sistema (RF, RNF, arquitetura) | Plano de execução, tasks |
| `adr` | Decisão arquitetural com trade-offs | Instruções de implementação |
| `spec` | O QUÊ implementar (contrato da feature) | COMO implementar, tasks, padrões de stack |
| `execution-plan` | COMO executar (tasks, DAG, prompts) | Requisitos, critérios de aceite |
| `skill-file-structure` | Naming, estrutura, arquitetura | Lógica de negócio, requisitos |
| `rule` | Constraint sempre ativa | Instruções situacionais |
| `skill` | Instrução reutilizável por tipo de tarefa | Constraints permanentes (isso é rule) |

---

## Princípios Fundamentais

1. **Spec-before-code** — nenhuma implementação sem spec validada
2. **Contexto mínimo suficiente** — agente recebe só o que precisa para a task atual
3. **Rastreabilidade vertical** — PRD → Spec → Código → Teste formam cadeia rastreável
4. **Decisões documentadas** — todo trade-off vira ADR com justificativa
5. **Extrair para reutilizar** — instrução repetida 2x vira Rule; 3x vira Skill; padrão de stack vai para CLAUDE.md

---

## Documentos de Referência

| Documento | Propósito |
| --- | --- |
| [Diagrama do Fluxo](docs/FLOW.md) | Visão visual + fluxo de contexto do agente |
| [Metodologia Formal](docs/methodology.md) | Fases, princípios, checklists e estrutura de diretórios |
