# Spec: [ID] — [Nome da Feature]

> **ID:** [DOMÍNIO-NNN] (ex: AUTH-001, PAYMENT-003, USER-012)
> **Status:** [Rascunho | Em Revisão | Aprovada | Em Implementação | Concluída]
> **Data:** YYYY-MM-DD
> **PRD Técnico:** `docs/prd-technical.md` (seção N)
> **ADRs Relacionados:** [ADR-NNN, ADR-NNN | N/A]
> **Depende de:** [ID-NNN, ID-NNN | Nenhuma]
> **Bloqueada por:** [ID-NNN | Nenhuma]
> **Plano de Execução:** `docs/specs/[ID]-execution-plan.md`

---

## Contexto e Motivação

[Por que esta feature existe? Qual problema de negócio ou técnico ela resolve?
Qual RF do PRD técnico ela implementa? (ex: Implementa RF-001, RF-002)]

---

## User Story

```
Como [tipo de usuário],
Quero [ação / capacidade],
Para que [benefício / objetivo].
```

---

## Requisitos Funcionais

> O que o sistema DEVE fazer. Comportamentos observáveis externamente.

1. [O sistema deve ...]
2. [O sistema deve ...]
3. [O sistema deve ...]

---

## Requisitos Não Funcionais

> Constraints de qualidade específicos desta feature.
> Padrões globais do projeto (performance base, segurança, cobertura mínima) estão em **CLAUDE.md**.

- **Performance:** [apenas se divergir ou adicionar ao padrão global — ex: este endpoint < 100ms]
- **Segurança:** [apenas constraint específico desta feature]
- **Manutenibilidade:** [apenas constraint específico desta feature]

---

## Critérios de Aceite

> Formato BDD. Cada cenário deve ter cobertura de teste correspondente.

### Cenário 1: [Nome — caminho feliz]
```gherkin
Dado que [pré-condição / estado inicial]
Quando [ação do usuário ou sistema]
Então [resultado esperado observável]
```

### Cenário 2: [Nome — caso de erro]
```gherkin
Dado que [pré-condição]
Quando [ação inválida ou estado de erro]
Então [comportamento esperado do sistema]
  E [comportamento adicional, se houver]
```

### Cenário 3: [Nome — caso de borda]
```gherkin
Dado que [pré-condição de borda]
Quando [ação]
Então [resultado esperado]
```

---

## Casos de Borda Explícitos

> Situações que devem ser tratadas mas podem não ser óbvias.

- [ ] [ex: O que acontece quando o e-mail já está cadastrado?]
- [ ] [ex: O que acontece quando o token expira durante a requisição?]
- [ ] [ex: Comportamento com campos opcionais ausentes]

---

## Fora de Escopo

> O que explicitamente NÃO será implementado nesta spec.

- [ex: Login com redes sociais — ver spec AUTH-003]
- [ex: 2FA — fora do MVP, backlog]

---

## Contrato de Interface

### Endpoint(s) *(se aplicável)*

```
[MÉTODO] /api/v1/[recurso]
Authorization: Bearer <token> | N/A

Request Body:
{
  "campo1": string,        // obrigatório
  "campo2?": string        // opcional
}

Response 2XX:
{
  "id": string,
  "campo1": string,
  "createdAt": string      // ISO 8601
}

Response 4XX: { "error": "CÓDIGO_ERRO", "details": [...] }
Response 500: { "error": "INTERNAL_ERROR" }
```

### Eventos / Mensagens *(se aplicável)*
```
Emite: [nome-do-evento] → { campo1, campo2 }
Consome: [nome-do-evento] → handler: [função]
```

---

## Modelo de Dados

> Apenas o que é criado/modificado por esta spec.

```sql
-- Tabela criada ou modificada
CREATE TABLE [tabela] (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  campo1 VARCHAR(255) NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

---

## Arquivos a Criar / Modificar

> Naming de arquivos, diretórios e localização: seguir **skill: file-structure** do projeto
> (`CLAUDE.md` → seção "Skills do Projeto").

| Ação | Arquivo | Notas |
|---|---|---|
| Criar | [caminho derivado da skill file-structure] | [responsabilidade] |
| Modificar | [caminho] | [o que muda] |
| Criar | [caminho do arquivo de teste] | Cobre os cenários BDD acima |

---

## Referências de Implementação

> Padrões globais de stack e código: **CLAUDE.md** (sempre carregado pelo agente).
> Constraints obrigatórias do projeto: **Rules ativas** em CLAUDE.md → seção "Rules Ativas".
> Estrutura e naming: **skill: file-structure** em `.claude/skills/file-structure.md`.
>
> Nota específica desta feature *(preencher apenas se houver exceção ou detalhe único)*:
- [ex: Reutilizar `[ServiçoExistente]` de `shared/[módulo]/` — não criar novo]
- [ex: Exceção ao padrão X por causa de Y — ver ADR-NNN]

---

## Rastreabilidade

> Convenção de rastreabilidade: **RULE-rastreability** (`CLAUDE.md` → seção "Rules Ativas").
> Preencher após implementação concluída.

- **Branch:** `feature/[ID]-[nome-kebab]`
- **Arquivos criados:** [lista]
- **Arquivos modificados:** [lista]
- **Testes:** [N] unitários, [N] integração
