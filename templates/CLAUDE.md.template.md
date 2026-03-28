# CLAUDE.md — [Nome do Projeto]

> Este arquivo é o contexto permanente do agente de IA. Lido em toda sessão.
> Mantenha-o objetivo e atualizado. Não é documentação — é instrução operacional.
> Última atualização: YYYY-MM-DD

---

## Projeto

**Nome:** [nome-do-projeto]
**Descrição:** [1–2 frases descrevendo o que o sistema faz e para quem]
**Status:** [Em desenvolvimento / Produção / Manutenção]

---

## Stack Tecnológica

| Camada | Tecnologia | Versão |
| --- | --- | --- |
| Runtime | [ex: Node.js] | [ex: 22.x] |
| Framework | [ex: NestJS / Next.js] | [ex: 14.x] |
| Linguagem | [ex: TypeScript] | [ex: 5.x] |
| Banco de dados | [ex: PostgreSQL] | [ex: 16] |
| ORM / Query | [ex: Drizzle ORM] | [ex: 0.30] |
| Validação | [ex: Zod] | [ex: 3.x] |
| Testes | [ex: Vitest] | [ex: 2.x] |
| CI/CD | [ex: GitHub Actions] | — |

> Decisões de escolha de stack: ver ADRs referenciados abaixo.

---

## Padrões de Código

> Detalhamento de estrutura de pastas, naming e arquitetura: ver **skill: file-structure**
> em `.claude/skills/file-structure.md`.

### Naming (resumo rápido)

- Arquivos: `kebab-case.ts`
- Classes: `PascalCase`
- Funções e variáveis: `camelCase`
- Constantes: `UPPER_SNAKE_CASE`
- Tipos/Interfaces: `PascalCase`

### Organização

- [Descrever o padrão arquitetural adotado — ex: Vertical Slice, Hexagonal, MVC]
- [Princípio principal de organização dos módulos]
- [Onde fica código compartilhado]

### Padrões de Stack Obrigatórios
>
> O que o agente DEVE usar neste projeto (libs, padrões, convenções de framework):

- **Validação:** [ex: Zod — usar sempre para validação de entrada em controllers e DTOs]
- **HTTP Errors:** [ex: classe `AppError` de `shared/errors/` — nunca lançar Error nativo]
- **Logging:** [ex: `logger` de `shared/logger/` — nunca usar console.log]
- **[Padrão N]:** [descrição]

---

## Padrões Proibidos (Anti-Padrões)

> NUNCA faça isso neste projeto:

- [ ] **Sem `any` explícito** — use tipos específicos ou `unknown`
- [ ] **Sem lógica de negócio em Controllers** — apenas validação + delegate para Service
- [ ] **Sem queries SQL inline** — use o ORM
- [ ] **Sem secrets no código** — use variáveis de ambiente
- [ ] **Sem console.log** — use o logger configurado
- [ ] **Sem testes de implementação** — teste comportamento, não internals
- [ ] **Sem abstrações prematuras** — não crie helpers para uso único
- [ ] [Adicionar anti-padrões específicos do projeto]

---

## Estratégia de Testes

- **Unitários:** Toda lógica de negócio (Services, Use Cases) com cobertura de casos de borda
- **Integração:** Endpoints críticos contra infraestrutura real (não mock)
- **E2E:** [Fluxos críticos — quando aplicável]
- **Cobertura mínima:** [ex: 80%] em arquivos de lógica de negócio
- **Arquivo de teste:** `[nome].spec.ts` co-localizado com o arquivo testado
- **Framework:** [ex: Vitest com describe/it/expect]
- **Cenários:** derivados diretamente dos critérios de aceite BDD das specs

---

## Skills do Projeto

> Skills são instruções reutilizáveis invocadas pelo agente para tarefas específicas.
> Template para criar novas skills: `[templates/skill.template.md]`

| Skill | Propósito | Arquivo |
| --- | --- | --- |
| `file-structure` | Naming, diretórios e arquitetura do projeto — **obrigatório consultar ao criar arquivos** | `.claude/skills/file-structure.md` |
| [nome-da-skill] | [propósito] | `.claude/skills/[nome].md` |

---

## Rules Ativas

> Rules são constraints que o agente DEVE sempre seguir, sem exceção.
> Template para criar novas rules: `[templates/rule.template.md]`

| Rule | Descrição resumida | Arquivo |
| --- | --- | --- |
| `RULE-rastreability` | Adicionar `// spec: [ID]` no topo de todo arquivo criado para uma spec | `.claude/rules/rastreability.md` |
| `RULE-spec-before-code` | Nunca iniciar implementação sem spec aprovada referenciada | `.claude/rules/spec-before-code.md` |
| `RULE-atomic-task` | Uma task = um arquivo ou uma responsabilidade — nunca misturar | `.claude/rules/atomic-task.md` |
| [RULE-nome] | [descrição] | `.claude/rules/[nome].md` |

---

## Decisões Arquiteturais (ADRs)

| ADR | Decisão | Arquivo |
| --- | --- | --- |
| ADR-001 | [ex: Escolha do ORM] | `docs/architecture/decisions/ADR-001-orm.md` |
| ADR-002 | [ex: Estratégia de autenticação] | `docs/architecture/decisions/ADR-002-auth.md` |

---

## Contexto do Agente — Como Trabalhar

### Hierarquia de contexto

1. **Este arquivo** = sempre válido (padrões permanentes do projeto)
2. **Spec da feature** = válido para a feature atual (`docs/specs/[ID]-[nome].spec.md`)
3. **Execution Plan** = válido para o planejamento da feature (`docs/specs/[ID]-execution-plan.md`)
4. **Prompt da task** = instrução mínima e específica da task corrente
5. **memory/** = estado persistido entre sessões (`.claude/memory/`)

### Antes de implementar qualquer feature

1. Ler a spec em `docs/specs/[ID]-[nome].spec.md`
2. Ler o execution plan em `docs/specs/[ID]-execution-plan.md`
3. Consultar a skill `file-structure` para naming e localização dos arquivos
4. Verificar ADRs referenciados na spec
5. Confirmar dependências de outras specs/módulos

### Quando pedir esclarecimento

- Antes de criar um padrão, módulo ou abstração não descrita aqui
- Quando a spec conflitar com uma rule ou padrão deste arquivo
- Quando a task exigir mudança arquitetural (sugerir ADR antes de implementar)

---

## Variáveis de Ambiente

| Variável | Descrição | Obrigatória |
| --- | --- | --- |
| `DATABASE_URL` | Connection string do banco | Sim |
| `[VARIAVEL]` | [descrição] | [Sim/Não] |

Referência completa: `.env.example`
