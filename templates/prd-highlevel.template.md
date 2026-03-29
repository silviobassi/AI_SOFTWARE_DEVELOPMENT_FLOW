# PRD Alto Nível — [Nome do Produto / Iniciativa]

> **Versão:** 1.0
> **Data:** YYYY-MM-DD
> **Status:** [Rascunho / Em Revisão / Aprovado]
> **Autor:** [Nome]
> **Audiência:** Stakeholders, clientes, você como PM — sem jargão técnico

---

## 1. Visão do Produto

### Problema

[Descreva o problema real que existe no mundo — não a solução.
Responda: quem sofre com isso? qual é o impacto concreto? por que ainda não está resolvido?
Seja específico e factual. Evite jargão técnico.]

**Exemplo:**
> Pequenos times de desenvolvimento perdem tempo significativo reescrevendo contexto para o agente de IA
> a cada nova sessão, porque não existe um padrão documentado que o agente possa consumir diretamente.

### Quem É Afetado

| Persona | Papel | Dor Principal |
| --- | --- | --- |
| [ex: Solo Developer] | [ex: Único responsável pelo produto] | [ex: Perde horas repetindo contexto ao agente de IA] |
| [ex: Tech Lead] | [ex: Revisa código gerado por IA] | [ex: Falta de rastreabilidade entre requisito e código] |

### Por Que Agora

[O que mudou no contexto que torna este momento o certo para resolver o problema?
Ex: nova tecnologia disponível, mudança de mercado, regulação, crescimento de usuários.]

---

## 2. Solução Proposta

### Visão de Alto Nível

[Descreva a solução em 2–4 frases. O que o produto faz? Para quem? Qual transformação ele entrega?
NÃO entre em tecnologia, stack ou implementação — isso pertence ao PRD Técnico.]

**Exemplo:**
> Um kit de metodologia com documentos, templates e padrões que definem um fluxo completo
> de desenvolvimento de software com IA integrada, do levantamento de requisitos até a entrega.

### O Que Muda Para o Usuário

| Antes | Depois |
| --- | --- |
| [ex: Repete contexto em cada sessão de IA] | [ex: Contexto estruturado e reutilizável via CLAUDE.md] |
| [ex: Sem padrão para documentar requisitos] | [ex: Templates prontos para PRD, Spec, ADR] |
| [ex: Retrabalho por falta de spec] | [ex: Spec-before-code como princípio central] |

---

## 3. Objetivos de Negócio

> Objetivos devem ser mensuráveis. Prefira OKR ou formato "métrica → meta".

- [ ] [ex: Reduzir o tempo médio de onboarding de um novo projeto em 50%]
- [ ] [ex: Eliminar retrabalho por falta de spec em 100% das features entregues]
- [ ] [ex: Atingir adoção por X desenvolvedores nos primeiros 3 meses]

---

## 4. Escopo

### Dentro do Escopo (v1.0)

- [ex: Templates para as 7 fases do fluxo de desenvolvimento]
- [ex: Documentação do fluxo com diagramas Mermaid]
- [ex: Guia de uso por tamanho de projeto (pequeno / médio / grande)]

### Fora do Escopo (v1.0)

- [ex: Integração automatizada com CI/CD]
- [ex: Interface web para preencher templates]
- [ex: Suporte a múltiplos idiomas]

> **Decisões de escopo:** registrar motivação aqui, ou referenciar ADR se existir.

---

## 5. Funcionalidades Principais (Epics)

> Liste os grandes blocos de funcionalidade. Não detalhe implementação — isso vai para as Specs.

| ID | Epic | Descrição | Prioridade |
| --- | --- | --- | --- |
| EP-001 | [ex: Templates de Documentação] | [ex: Conjunto de templates cobrindo todas as fases do fluxo] | Alta |
| EP-002 | [ex: Guia de Fluxo Visual] | [ex: Diagrama Mermaid interativo com o fluxo completo de 7 fases] | Alta |
| EP-003 | [ex: Metodologia Formal] | [ex: Documento de referência com princípios, checklists e estrutura de diretórios] | Média |
| EP-004 | [ex: Exemplos por Stack] | [ex: Exemplos preenchidos para Node.js, Python, etc.] | Baixa |

---

## 6. Critérios de Sucesso (KPIs de Negócio)

> Como saberemos que o produto funcionou? Defina antes de construir.

| Critério | Métrica | Meta | Prazo |
| --- | --- | --- | --- |
| [ex: Adoção] | [ex: Projetos iniciados usando o kit] | [ex: 10 projetos] | [ex: 60 dias após lançamento] |
| [ex: Qualidade] | [ex: % de features entregues sem retrabalho de requisitos] | [ex: > 90%] | [ex: Por sprint] |
| [ex: Eficiência] | [ex: Redução no tempo de setup inicial] | [ex: < 30 min para novo projeto] | [ex: Medido em onboarding] |

---

## 7. Restrições e Premissas

### Restrições

- [ex: Deve funcionar sem ferramentas pagas adicionais]
- [ex: Compatível com qualquer stack de programação]
- [ex: Documentação em português (BR)]

### Premissas

- [ex: O usuário tem acesso a um agente de IA (Claude, Cursor, Copilot, etc.)]
- [ex: O usuário tem familiaridade básica com Git e Markdown]
- [ex: Projetos-alvo têm entre 1 e 5 desenvolvedores]

---

## 8. Riscos de Negócio

| Risco | Probabilidade | Impacto | Mitigação |
| --- | --- | --- | --- |
| [ex: Baixa adoção por complexidade percebida] | [ex: Média] | [ex: Alto] | [ex: Simplificar guia de início rápido; escalar por tamanho de projeto] |
| [ex: Templates ficam desatualizados com evolução das ferramentas] | [ex: Alta] | [ex: Médio] | [ex: Versionar templates; log de mudanças por versão] |

---

## 9. Linha do Tempo (Alto Nível)

| Marco | Descrição | Data Alvo |
| --- | --- | --- |
| [ex: MVP] | [ex: Templates core + FLOW.md + methodology.md] | [ex: YYYY-MM-DD] |
| [ex: v1.0] | [ex: Todos os templates + README completo + exemplos] | [ex: YYYY-MM-DD] |
| [ex: v1.1] | [ex: Templates de exemplos por stack] | [ex: YYYY-MM-DD] |

---

## 10. Referências

- PRD Técnico: `docs/prd-technical.md`
- Diagrama do Fluxo: `docs/FLOW.md`
- Metodologia Formal: `docs/methodology.md`
- ADRs: `docs/architecture/decisions/`
