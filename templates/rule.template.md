# Rule: [RULE-NOME]

> **ID:** RULE-[NOME] (ex: RULE-RASTREABILITY, RULE-SPEC-BEFORE-CODE)
> **Severidade:** [Obrigatória | Recomendada]
> **Escopo:** [Global — vale sempre | Feature — válida neste contexto específico]
> **Localização no projeto:** `.claude/rules/[nome].md`
> **Referenciada em:** CLAUDE.md → seção "Rules Ativas"

---

## Regra

> Uma frase imperativa e inequívoca.

**[Verbo imperativo]: [o que fazer ou não fazer].**

---

## Motivação

[Por que esta rule existe? Qual problema ela previne?
Qual foi o custo (retrabalho, bug, token waste) que motivou criá-la?]

---

## Quando se Aplica

- [ ] [Situação 1 — ex: sempre que criar um novo arquivo de código]
- [ ] [Situação 2 — ex: sempre que finalizar uma task de implementação]
- [ ] [Situação 3]

## Quando NÃO se Aplica

- [ex: Arquivos de configuração (.env, tsconfig.json) — não precisam do comentário de spec]
- [ex: Arquivos gerados automaticamente por ferramentas]

---

## Como Aplicar

> Instrução passo a passo para o agente cumprir esta rule.

1. [Passo 1]
2. [Passo 2]
3. [Passo 3]

**Exemplo correto:**
```
[Exemplo de código, nome de arquivo, comentário, etc. que satisfaz a rule]
```

**Exemplo incorreto (violação):**
```
[O que o agente NÃO deve fazer]
```

---

## Verificação

> Como confirmar que a rule foi cumprida.

- [ ] [Critério 1 — ex: arquivo tem `// spec: [ID]` na primeira linha]
- [ ] [Critério 2]

---

## Referências

- Motivação original: [ex: issue de rastreabilidade identificada em [data]]
- Rule relacionada: [RULE-NOME]
- ADR relacionado: [ADR-NNN — se a rule deriva de uma decisão arquitetural]
