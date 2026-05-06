# marfin-weekly-review

Weekly review estruturada pra founder solo, dentro do Claude. 6 prompts curtos, 1 página de output, padrões detectados ao longo do tempo.

## O que essa Skill faz

- Faz 6 perguntas em sequência (uma por turno, não bombarda)
- Cava 1 follow-up pra cada resposta vaga
- Detecta padrões recorrentes se você tiver reviews anteriores no disco
- Devolve 1 página em markdown pronta pra Notion ou investor update

## Os 6 prompts

1. **Wins reais** (não tarefas)
2. **Losses** com causa real (não "esperando feedback")
3. **Decisões pendentes** (não execução)
4. **Foco da próxima semana** em 1 frase
5. **Riscos não cobertos** (o mais valioso)
6. **Energia** 1-10 e o que está sugando

## Demo

Pergunta no Claude: *"weekly review"* na sexta às 17h.

Resposta em ~10min: review formatada com padrões cruzados.
[Veja exemplo](examples/example-output-pt.md)

## Como instalar

### One-liner

```bash
curl -sSL https://raw.githubusercontent.com/marfin-lab/skills/main/install.sh | sh -s weekly-review
```

### Manual

```bash
git clone https://github.com/marfin-lab/skills-marfin-weekly-review.git ~/.claude/skills/marfin-weekly-review
```

## Pré-requisitos

- Nenhum obrigatório.
- **Recomendado:** Filesystem MCP — habilita detecção de padrões cruzando suas últimas 3-4 reviews na pasta `~/weekly-reviews/`.

## Quando usar

- Toda sexta-feira às 17h (ou domingo de manhã)
- Antes de fechar o laptop pro fim de semana
- Antes de mandar update mensal pro investor

## Quando NÃO usar

- Daily standup → use [marfin-daily-standup-synth](#)
- Quarterly review (escala maior, KRs em vez de reflection)
- Mood/wellbeing journaling → use [marfin-estoic-morning-pages](https://github.com/marfin-lab/skills-marfin-estoic-morning-pages)

## Detecção de padrões

Se você usar a skill por 4+ semanas (e tiver Filesystem MCP), ela passa a detectar padrões silenciosos:

- Loss recorrente na mesma área (3+ semanas seguidas)
- Decisão pendente em 2+ semanas
- Energia caindo 3 semanas seguidas
- Foco da semana NÃO atendido (trade-off não funcionou)

A skill só reporta. Não prega moral, não sugere fix. Você decide o que fazer com o sinal.

## Roadmap

- [x] V1: 6 prompts + 1-pager
- [ ] V2: investor update mode (mais sucinta, menos íntima)
- [ ] V3: quarterly review (4 weekly reviews → 1 quarterly summary)

## Licença

MIT.

## Outras Skills da Marfin

| Skill | O que faz |
|---|---|
| [marfin-pitch-deck-roast](https://github.com/marfin-lab/skills-marfin-pitch-deck-roast) | Crítica brutal do seu pitch deck |
| [marfin-cold-email-engine](https://github.com/marfin-lab/skills-marfin-cold-email-engine) | Cold email personalizado |
| [marfin-competitor-teardown](https://github.com/marfin-lab/skills-marfin-competitor-teardown) | Teardown público de concorrente |
| [marfin-prd-writer](https://github.com/marfin-lab/skills-marfin-prd-writer) | PRD a partir de ideia bruta |

Catálogo completo: https://marfin.co/skills

---

Feito por [Marfin Co.](https://marfin.co) — venture builder de produtos com IA.
