# Skill: [nome-da-skill]

> **ID:** SKILL-[NOME] (ex: SKILL-FILE-STRUCTURE, SKILL-WRITE-TESTS)
> **Versão:** 1.0
> **Escopo:** [Global — reutilizável em qualquer projeto | Projeto — específico deste projeto]
> **Localização no projeto:** `.claude/skills/[nome].md`
> **Referenciada em:** CLAUDE.md → seção "Skills do Projeto"

---

## Propósito

[Uma frase clara: "Esta skill define como [fazer X] neste projeto."]
[Quando o agente deve consultar esta skill? Qual trigger ou situação a torna relevante?]

---

## Quando Usar

> O agente deve consultar esta skill ANTES de:

- [ ] [Situação 1 — ex: criar qualquer novo arquivo de código]
- [ ] [Situação 2 — ex: definir o caminho de um novo módulo]
- [ ] [Situação 3]

---

## Instruções

> Escreva as instruções como se estivesse explicando diretamente ao agente.
> Seja específico e objetivo — evite ambiguidade.

### [Seção 1 — ex: Estrutura]

[Instrução clara e direta]

```gherkin
[Exemplo concreto — código, estrutura de pastas, naming, etc.]
```

### [Seção 2 — ex: Naming]

[Instrução]

| Elemento | Padrão | Exemplo |
| --- | --- | --- |
| [elemento] | [padrão] | [exemplo concreto] |

### [Seção 3 — ex: Exceções]

[Quando esta skill NÃO se aplica ou tem comportamento diferente]

---

## Exemplos

### Exemplo 1: [Cenário comum]

**Input (situação):** [Descrição do que o agente está fazendo]
**Output esperado:** [O que o agente deve produzir seguindo esta skill]

```gherkin
[Exemplo concreto]
```

### Exemplo 2: [Cenário de borda]

**Input:** [Situação]
**Output esperado:** [Resultado]

---

## Anti-padrões (o que esta skill proíbe)

- [ex: Nunca criar arquivos fora da estrutura definida]
- [ex: Nunca misturar responsabilidades no mesmo arquivo]

---

## Referências

- ADR relacionado: [ADR-NNN — se a skill deriva de uma decisão arquitetural]
- CLAUDE.md → seção: [seção relevante]
- Skill relacionada: [SKILL-NOME — se complementar]
