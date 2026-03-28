# AI-Assisted Software Development — Metodologia Formal

> **Versão:** 1.0
> **Data:** 2026-03-28
> **Contexto:** Solo Developer + AI Agent | Projetos variados
> **Objetivo:** Fluxo de desenvolvimento que maximiza qualidade, rastreabilidade e eficiência com uso de IA

---

## 1. Visão Geral

Esta metodologia define um processo estruturado para desenvolvimento de software assistido por IA, cobrindo desde a coleta de requisitos até a entrega. O fluxo é agnóstico de stack e escalável por tamanho de projeto.

**Princípios fundamentais:**

| Princípio | Aplicação no fluxo |
|---|---|
| **Spec-before-code** | Nenhuma task de implementação inicia sem spec validada |
| **Contexto mínimo suficiente** | O agente recebe apenas o contexto necessário para a task atual |
| **Rastreabilidade vertical** | PRD → Spec → Task → Código → Teste formam uma cadeia rastreável |
| **Decisões documentadas** | Todo trade-off arquitetural vira um ADR com justificativa |
| **Feedback loops explícitos** | Checkpoints de auto-revisão em cada transição de fase |

---

## 2. Fases do Fluxo

### Fase 1 — Entendimento

**Objetivo:** Capturar o problema e os requisitos com clareza suficiente para derivar a arquitetura.

#### 1.1 PRD Alto Nível
- **Audiência:** Stakeholders, clientes, você mesmo como PM
- **Formato:** Narrativo, sem jargão técnico
- **Conteúdo obrigatório:** Problema, usuários afetados, solução proposta, métricas de sucesso, fora de escopo
- **Template:** `templates/prd-business.template.md` *(futuro)*

**Checkpoint 1:** O problema está claro? A solução proposta responde ao problema? Os critérios de sucesso são mensuráveis?

#### 1.2 PRD Técnico
- **Audiência:** Você como engenheiro + o agente de IA
- **Formato:** Estruturado, com seções padronizadas
- **Conteúdo obrigatório:** Stack, arquitetura de alto nível, RF, RNF, integrações, restrições
- **Template:** `templates/prd-technical.template.md`

**Checkpoint 2:** Stack e restrições definidos? Requisitos não funcionais cobrem performance, segurança e manutenibilidade?

---

### Fase 2 — Arquitetura

**Objetivo:** Traduzir o PRD técnico em decisões e artefatos de arquitetura rastreáveis.

#### 2.1 Diagramas
- **C4 Nível 1 (Context):** Sistema e atores externos
- **C4 Nível 2 (Container):** Principais componentes/serviços
- **Sequência:** Fluxos críticos de negócio
- **ER / Schema:** Modelo de dados (quando aplicável)
- **Ferramenta recomendada:** Mermaid (version-controlável)

#### 2.2 Requisitos Funcionais e Não Funcionais
- Derivados do PRD técnico
- RF: comportamentos observáveis do sistema
- RNF: constraints de qualidade (performance, segurança, escalabilidade, manutenibilidade)

#### 2.3 ADRs (Architecture Decision Records)
- **Quando criar:** Para cada decisão com trade-offs significativos
- **Exemplos:** Escolha de banco de dados, estratégia de autenticação, padrão de comunicação entre serviços
- **Template:** `templates/adr.template.md`
- **Localização padrão:** `docs/architecture/decisions/ADR-NNN-titulo.md`

#### 2.4 RFCs *(opcional, projetos médios/grandes)*
- Propostas de mudanças arquiteturais significativas
- Útil quando você quer registrar o raciocínio antes de executar

**Checkpoint 3:** A arquitetura responde todos os RNFs? Todas as decisões críticas têm ADR?

---

### Fase 3 — Configuração do Agente

**Objetivo:** Preparar o agente para operar com o contexto correto e comportamento previsível.

#### 3.1 CLAUDE.md / AGENTS.md
O arquivo mais importante do fluxo. Lido em toda sessão pelo agente.

**Seções obrigatórias:**
- Visão geral do projeto (2–3 linhas)
- Stack tecnológica com versões
- Estrutura de pastas comentada
- Padrões de código (naming, organização, patterns adotados)
- **Anti-padrões** (o que o agente NÃO deve fazer — crítico)
- Estratégia de testes
- Referências a ADRs relevantes
- Instruções de contexto (como usar specs, memory/, etc.)

**Template:** `templates/CLAUDE.md.template.md`

#### 3.2 Skills e Rules

**Skills** — instruções reutilizáveis que o agente consulta sob demanda:
- Definem **como** executar um tipo de tarefa (criar arquivo, escrever teste, nomear módulo)
- Escopo: global (qualquer projeto) ou específico do projeto
- A skill mais importante de todo projeto: **`file-structure`** — define o padrão arquitetural, naming e estrutura de pastas
- **Template genérico:** `templates/skill.template.md`
- **Template de estrutura de arquivos:** `templates/skill-file-structure.template.md` (inclui CRUD/MVC, Vertical Slice, Hexagonal, Clean Architecture)
- **Localização no projeto:** `.claude/skills/[nome].md`

**Rules** — constraints que o agente SEMPRE deve cumprir, sem exceção:
- Definem **o que nunca violar** independentemente do contexto
- Exemplos prontos para criar: `RULE-rastreability`, `RULE-spec-before-code`, `RULE-atomic-task`
- **Template:** `templates/rule.template.md`
- **Localização no projeto:** `.claude/rules/[nome].md`

**Princípio de extração:**
- Instrução dada **2x** → virar Rule (constraint permanente)
- Instrução dada **3x** → virar Skill (comportamento reutilizável)
- Padrão de stack (lib, framework, convenção) → sempre em CLAUDE.md, nunca em specs

---

### Fase 4 — Especificações

**Objetivo:** Definir o "quê" de cada feature antes do "como". É a principal alavanca contra retrabalho.

#### Estrutura de uma Spec
Cada feature tem sua própria spec. A spec é o contrato entre você e o agente.

**Conteúdo obrigatório:**
- ID único (ex: `AUTH-001`)
- Contexto e motivação
- Requisitos funcionais (lista numerada)
- Requisitos não funcionais específicos da feature
- Critérios de aceite em formato BDD (Given/When/Then)
- Casos de borda explícitos
- Fora de escopo
- Dependências de outras specs ou serviços

**Template:** `templates/spec.template.md`

**Checkpoint 4:** Spec está completa? Todos os critérios de aceite são testáveis? Dependências mapeadas?

---

### Fase 5 — Planejamento de Execução

**Objetivo:** Transformar cada spec em um plano de execução autônomo que o agente segue task a task.

**Template:** `templates/execution-plan.template.md`
**Localização no projeto:** `docs/specs/[ID]-execution-plan.md` (arquivo separado da spec)

#### 5.1 Breakdown em Tasks Atômicas
- Cada task deve ser executável em uma única sessão de agente (ver RULE-atomic-task)
- Tasks com escopo bem delimitado: um arquivo ou uma responsabilidade
- Nunca misturar responsabilidades: "criar service E escrever testes" = 2 tasks separadas

#### 5.2 DAG de Dependências
- Mapear quais tasks bloqueiam outras
- Identificar quais podem rodar em paralelo
- O execution plan inclui diagrama Mermaid do DAG e tabela de status

#### 5.3 Testes First
- Para cada task de implementação, a task de teste é planejada junto
- Critérios de aceite BDD da spec → cenários de teste concretos no execution plan

#### 5.4 Estimativa de Contexto por Task
- Mapear quais arquivos o agente precisa ler para cada task
- Se > 5 arquivos → quebrar a task em partes menores
- O execution plan inclui tabela de estimativa de contexto
- Incluir no execution plan o **prompt base** por task (reduz tokens na execução)

---

### Fase 6 — Implementação

**Objetivo:** Executar o plano com o agente, mantendo rastreabilidade e qualidade.

#### 6.1 Execução pelo Agente
- Iniciar cada task passando: a spec da feature + a task específica + contexto mínimo necessário
- O agente tem CLAUDE.md como contexto permanente
- **Não** reutilizar sessões longas demais — janela de contexto contamina

#### 6.2 Rastreabilidade Spec ↔ Código
- Convenção definida em **RULE-rastreability** (`.claude/rules/rastreability.md`)
- Spec atualizada com referência aos arquivos criados/modificados após conclusão

#### 6.3 Validação Automatizada
- Lint + formatação
- Testes unitários + integração
- Build
- CI deve rodar em toda task concluída, não só no final

---

### Fase 7 — Entrega

**Objetivo:** Garantir qualidade final e manter a documentação atualizada.

#### 7.1 Code Review
- Auto-revisão com checklist: lógica, segurança, performance, manutenibilidade
- Usar o agente para review secundário: "revise este código contra a spec AUTH-001"

#### 7.2 Merge + CI/CD
- Pipeline obrigatório antes do merge
- Estratégia de branch: feature branches por spec

#### 7.3 Manutenção de ADRs
- Se a implementação divergiu de decisões arquiteturais, atualizar o ADR
- Registrar o porquê da divergência

---

## 3. Gestão de Contexto do Agente

A maior fonte de desperdício de tokens é contexto mal gerenciado. Hierarquia de contexto:

```
CLAUDE.md          → contexto permanente (sempre carregado)
  └── Spec         → contexto de feature (carregado por feature)
       └── Task    → contexto de task (instrução + arquivos mínimos)
            └── memory/ → estado persistido entre sessões
```

### Regras de Ouro para Economizar Tokens

1. **CLAUDE.md denso mas objetivo** — não é documentação, é instrução operacional
2. **Spec antes do prompt** — não explique o contexto inline, referencie a spec
3. **Task atômica** — quanto menor o escopo, menos contexto necessário
4. **Evite "faça tudo"** — uma task = um arquivo ou uma função
5. **Não repita instruções** — se você repetiu 2x, vire uma Rule; se 3x, vire uma Skill

### Sinais de Contexto Degradado
- Agente faz perguntas já respondidas na spec
- Agente repete código já escrito
- Respostas ficam genéricas ou inconsistentes com decisões anteriores

**Ação:** Iniciar nova sessão, passar CLAUDE.md + spec + estado atual da task

---

## 4. Rastreabilidade Vertical

```
PRD Técnico (requisito)
  └── ADR (decisão arquitetural)
       └── Spec (feature spec)
            └── Task (unidade de implementação)
                 └── Código (arquivo + comentário spec: ID)
                      └── Teste (cenário derivado do critério de aceite)
```

Cada artefato referencia o anterior. Isso garante:
- Manutenibilidade: saber **por que** o código existe
- Auditabilidade: rastrear mudanças até o requisito original
- Onboarding: novos contextos (sua própria sessão futura) entendem o sistema rapidamente

---

## 5. Estrutura de Diretórios Recomendada

```
project-root/
├── CLAUDE.md                              # Contexto permanente do agente
├── docs/
│   ├── prd-technical.md                   # PRD técnico do projeto
│   ├── architecture/
│   │   ├── diagrams/                      # Diagramas Mermaid (C4, sequência, ER)
│   │   └── decisions/                     # ADRs (ADR-001-titulo.md)
│   └── specs/                             # Feature specs + execution plans
│       ├── AUTH-001-login.spec.md         # Spec da feature
│       └── AUTH-001-execution-plan.md     # Plano de execução da feature
├── .claude/
│   ├── skills/                            # Skills do projeto
│   │   └── file-structure.md             # Skill obrigatória — arquitetura e naming
│   ├── rules/                             # Rules ativas do projeto
│   │   ├── rastreability.md
│   │   ├── spec-before-code.md
│   │   └── atomic-task.md
│   └── memory/                            # Estado persistido entre sessões
└── src/                                   # Código fonte (estrutura definida em skill: file-structure)
```

---

## 6. Checklist de Qualidade por Fase

### Antes de avançar da Fase 1 → 2
- [ ] Problema claramente definido
- [ ] Stack e restrições documentadas
- [ ] RNFs identificados (performance, segurança, escalabilidade)

### Antes de avançar da Fase 2 → 3
- [ ] Diagramas cobrem os fluxos críticos
- [ ] Toda decisão com trade-off tem ADR
- [ ] Arquitetura responde todos os RNFs

### Antes de avançar da Fase 3 → 4
- [ ] CLAUDE.md tem anti-padrões explícitos
- [ ] Skill `file-structure` criada com arquitetura e naming do projeto
- [ ] Rules básicas criadas: `RULE-rastreability`, `RULE-spec-before-code`, `RULE-atomic-task`
- [ ] Skills e Rules referenciadas no CLAUDE.md

### Antes de avançar da Fase 4 → 5
- [ ] Spec tem critérios de aceite testáveis
- [ ] Dependências entre specs mapeadas
- [ ] Fora de escopo explícito

### Antes de avançar da Fase 5 → 6
- [ ] Execution plan criado em `docs/specs/[ID]-execution-plan.md`
- [ ] Tasks são atômicas (escopo de 1 sessão) — ver RULE-atomic-task
- [ ] DAG de dependências documentado no execution plan
- [ ] Testes planejados junto com a implementação
- [ ] Prompts base por task documentados no execution plan

### Antes de merge (Fase 7)
- [ ] Todos os testes passando
- [ ] Lint e build sem erros
- [ ] Code review concluído
- [ ] ADRs atualizados se necessário
