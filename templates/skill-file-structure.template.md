# Skill: file-structure

> **ID:** SKILL-FILE-STRUCTURE
> **Escopo:** Projeto — específico deste projeto
> **Localização:** `.claude/skills/file-structure.md`
> **Referenciada em:** CLAUDE.md → seção "Skills do Projeto"

---

## Propósito

Esta skill define a estrutura de diretórios, naming de arquivos e organização de código
para este projeto. O agente DEVE consultar esta skill antes de criar ou mover qualquer arquivo.

---

## Quando Usar

O agente deve consultar esta skill ANTES de:

- [ ] Criar qualquer arquivo de código novo
- [ ] Definir o caminho de um novo módulo ou feature
- [ ] Decidir onde colocar código compartilhado
- [ ] Nomear qualquer arquivo, classe, função ou variável

---

## Padrão Arquitetural Adotado

> Escolha UMA das arquiteturas abaixo e remova as outras.
> Referencie o ADR que justificou a escolha.
> ADR: [ADR-NNN — decisão do padrão arquitetural]

---

### OPÇÃO A — Simple CRUD / MVC

> Indicado para: APIs simples, CRUDs, projetos com domínio pouco complexo.

```
src/
├── [recurso]/               # Ex: users/, products/
│   ├── [recurso].controller.ts    # HTTP: rotas, validação de input, response
│   ├── [recurso].service.ts       # Lógica de negócio
│   ├── [recurso].repository.ts    # Acesso a dados
│   ├── [recurso].dto.ts           # Data Transfer Objects (validação de entrada)
│   ├── [recurso].types.ts         # Tipos e interfaces do domínio
│   └── [recurso].spec.ts          # Testes do service
├── shared/
│   ├── errors/                    # Classes de erro da aplicação
│   ├── middleware/                # Middlewares HTTP
│   ├── types/                     # Tipos globais
│   └── utils/                     # Funções utilitárias puras
└── main.ts
```

**Regras:**
- Lógica de negócio SOMENTE em `*.service.ts`
- Controller SOMENTE valida input e chama service
- Repository SOMENTE faz acesso a dados — sem lógica de negócio
- DTOs validados com [ex: Zod] no controller antes de passar ao service

**Naming:**

| Elemento | Padrão | Exemplo |
|---|---|---|
| Arquivo | `kebab-case.ts` | `user-profile.service.ts` |
| Classe de Service | `[Recurso]Service` | `UserService` |
| Classe de Controller | `[Recurso]Controller` | `UserController` |
| Classe de Repository | `[Recurso]Repository` | `UserRepository` |
| DTO | `[Ação][Recurso]Dto` | `CreateUserDto` |
| Tipo/Interface | `[Nome]` (PascalCase) | `UserProfile` |
| Arquivo de teste | `[nome].spec.ts` | `user.service.spec.ts` |

---

### OPÇÃO B — Vertical Slice Architecture

> Indicado para: aplicações médias/grandes onde features evoluem independentemente.
> Cada feature é um "slice" vertical completo — sem camadas horizontais.

```
src/
├── features/
│   ├── [feature-name]/            # Ex: create-user/, list-products/
│   │   ├── [feature].handler.ts   # Orquestra o fluxo (entrada → saída)
│   │   ├── [feature].command.ts   # Dados de entrada (Command/Query)
│   │   ├── [feature].result.ts    # Dados de saída
│   │   ├── [feature].validator.ts # Validação de entrada
│   │   └── [feature].spec.ts      # Testes do handler
├── shared/
│   ├── domain/                    # Entidades e value objects compartilhados
│   ├── infrastructure/            # DB, cache, serviços externos
│   │   ├── db/
│   │   └── [serviço-externo]/
│   ├── errors/
│   └── types/
└── main.ts
```

**Regras:**
- Cada feature é completamente autocontida
- Features NÃO importam umas das outras — apenas de `shared/`
- Handler = único ponto de entrada da feature
- Sem camadas horizontais (não existe "a camada de service" — cada slice tem seu próprio)

**Naming:**

| Elemento | Padrão | Exemplo |
|---|---|---|
| Diretório de feature | `kebab-case` | `create-user/` |
| Handler | `[Ação][Recurso]Handler` | `CreateUserHandler` |
| Command | `[Ação][Recurso]Command` | `CreateUserCommand` |
| Query | `[Busca][Recurso]Query` | `GetUserByIdQuery` |
| Result | `[Ação][Recurso]Result` | `CreateUserResult` |
| Arquivo de teste | `[feature].spec.ts` | `create-user.spec.ts` |

---

### OPÇÃO C — Hexagonal Architecture (Ports & Adapters)

> Indicado para: domínio rico, múltiplas formas de entrada/saída, alta testabilidade.
> O domínio não depende de nada externo — apenas de si mesmo.

```
src/
├── domain/                        # Núcleo — sem dependências externas
│   ├── [entidade]/
│   │   ├── [entidade].entity.ts   # Entidade de domínio
│   │   ├── [entidade].vo.ts       # Value Objects
│   │   └── [entidade].errors.ts   # Erros de domínio
│   └── ports/                     # Interfaces (contratos)
│       ├── in/                    # Portas de entrada (use cases)
│       │   └── [use-case].port.ts
│       └── out/                   # Portas de saída (repositórios, serviços)
│           └── [repositorio].port.ts
├── application/                   # Casos de uso — orquestra domínio
│   └── [use-case]/
│       ├── [use-case].usecase.ts
│       └── [use-case].spec.ts
├── adapters/                      # Implementações concretas dos ports
│   ├── in/                        # Adaptadores de entrada
│   │   ├── http/                  # Controllers HTTP
│   │   └── [outro-canal]/
│   └── out/                       # Adaptadores de saída
│       ├── persistence/           # Implementações de repositório
│       └── [serviço-externo]/
└── main.ts
```

**Regras:**
- `domain/` NUNCA importa de `adapters/` ou `application/`
- `application/` importa de `domain/` (ports) — nunca de `adapters/`
- `adapters/` implementa os ports definidos em `domain/ports/`
- Inversão de dependência: domain define a interface, adapter implementa

**Naming:**

| Elemento | Padrão | Exemplo |
|---|---|---|
| Entidade | `[Nome].entity.ts` | `User.entity.ts` |
| Value Object | `[Nome].vo.ts` | `Email.vo.ts` |
| Port de entrada | `[UseCase]Port` | `CreateUserPort` |
| Port de saída | `[Repositorio]Port` | `UserRepositoryPort` |
| Use Case | `[Ação][Recurso]UseCase` | `CreateUserUseCase` |
| Adapter HTTP | `[Recurso]Controller` | `UserController` |
| Adapter Persistence | `[Recurso]Repository` | `UserRepository` |

---

### OPÇÃO D — Clean Architecture

> Indicado para: sistemas complexos, múltiplos casos de uso por entidade, necessidade de
> total independência entre regras de negócio e infraestrutura.

```
src/
├── entities/                      # Regras de negócio corporativas (mais internas)
│   └── [entidade]/
│       ├── [entidade].ts
│       └── [entidade].spec.ts
├── use-cases/                     # Regras de negócio da aplicação
│   └── [use-case]/
│       ├── [use-case].ts
│       ├── [use-case].input.ts    # Input boundary
│       ├── [use-case].output.ts   # Output boundary
│       └── [use-case].spec.ts
├── interface-adapters/            # Converte dados entre use-cases e frameworks
│   ├── controllers/
│   ├── presenters/
│   └── gateways/
└── frameworks-drivers/            # Frameworks, DB, UI (mais externos)
    ├── http/                      # Framework HTTP (Express, Fastify, etc.)
    ├── db/                        # ORM, migrações, schemas
    └── external/                  # Serviços externos
```

**Regras:**
- Dependências apontam sempre para dentro (entities ← use-cases ← adapters ← frameworks)
- Entities não conhecem use-cases, use-cases não conhecem controllers
- Toda comunicação entre camadas via interfaces (boundaries)
- Frameworks são detalhes — substituíveis sem tocar em use-cases ou entities

**Naming:**

| Elemento | Padrão | Exemplo |
|---|---|---|
| Entity | `[Nome].ts` em `entities/` | `User.ts` |
| Use Case | `[Ação][Recurso]UseCase.ts` | `CreateUserUseCase.ts` |
| Input Boundary | `[UseCase]Input.ts` | `CreateUserInput.ts` |
| Output Boundary | `[UseCase]Output.ts` | `CreateUserOutput.ts` |
| Controller | `[Recurso]Controller.ts` | `UserController.ts` |
| Gateway | `[Recurso]Gateway.ts` | `UserGateway.ts` |
| Presenter | `[Recurso]Presenter.ts` | `UserPresenter.ts` |

---

## Regras Globais de Naming (todas as arquiteturas)

| Elemento | Padrão |
|---|---|
| Nome de arquivo | `kebab-case.ts` |
| Classe | `PascalCase` |
| Função / método | `camelCase` |
| Variável | `camelCase` |
| Constante | `UPPER_SNAKE_CASE` |
| Enum | `PascalCase` (membros: `UPPER_SNAKE_CASE`) |
| Tipo / Interface | `PascalCase` |
| Arquivo de teste | `[nome].spec.ts` (co-localizado) |
| Arquivo de integração | `[nome].integration.spec.ts` |

---

## Anti-padrões

- Nunca criar arquivos fora da estrutura definida sem justificativa em ADR
- Nunca misturar responsabilidades de camadas diferentes no mesmo arquivo
- Nunca importar de uma camada interna para uma externa (exceto via interface)
- Nunca colocar lógica de negócio em controllers, adaptadores ou gateways
