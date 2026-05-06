---
name: marfin-weekly-review
description: "Use this skill when the user wants to do a structured weekly review as a founder/operator. Triggers include: 'weekly review', 'review da semana', 'fechar a semana', 'reflexão semanal', 'preparar update da semana', 'recap da semana'. The skill walks the user through 6 short prompts (wins, losses, decisions, focus, hidden risks, energy), detects recurring patterns across weeks if the user pastes prior reviews, and outputs a 1-page summary ready for Notion or for an investor update. Do NOT use this skill for daily standups (use marfin-daily-standup-synth), quarterly reviews (different scale), or pure mood journaling (use marfin-estoic-morning-pages)."
license: MIT
version: 1.0.0
author: Marfin Co.
requires_mcp: []
optional_mcps:
  - name: Filesystem
    why: "lê reviews anteriores em ~/weekly-reviews/ pra detectar padrões recorrentes (ex: losses semelhantes em 3+ semanas)"
---

# Marfin Weekly Review

## What this skill does

Walks a founder solo (or operator) through 6 structured prompts every Friday afternoon (or Sunday morning) and produces a 1-page weekly review ready to paste into Notion, share with co-founder remoto, or send to investor as monthly digest.

The skill asks ONE prompt at a time — never bombards all six at once. For each vague answer, it cavas with 1 follow-up. If the user has prior weekly reviews on disk, it detects patterns ("3 of last 4 weeks had losses in sales calls").

## When to use

Trigger on:
- "weekly review" / "review da semana"
- "fechar a semana" / "vou fazer o recap"
- "preparar update da semana" / "update do investor"
- Friday afternoon, Sunday morning, or the user explicitly asks

Do NOT trigger for:
- Daily standup → `marfin-daily-standup-synth`
- Quarterly review (escala diferente, KRs vs reflection)
- Mood/wellbeing journaling → `marfin-estoic-morning-pages`
- Project status update (use Linear/Notion direto)

## Required setup

None. Works standalone.

If Filesystem MCP is available, the skill scans `~/weekly-reviews/` (or whatever folder the user names) for prior `.md` reviews to detect recurring patterns.

## The 6 prompts

Ask ONE at a time. Wait. Don't dump all 6.

### Prompt 1: Wins da semana
> "3 wins reais da semana — coisas que mexeram o ponteiro, não tarefas concluídas."

If the user lists tasks ("entreguei feature X"), push back: "Isso é task. O ponteiro mexeu? Em qual métrica?"

### Prompt 2: Losses
> "1-2 losses. Não 'esperando feedback' — coisas que NÃO saíram. E o porquê real."

Force specificity. "Não tive tempo" não é causa; é sintoma. Cava 1 follow-up até bater na causa real.

### Prompt 3: Decisões pendentes
> "O que está travado por falta de decisão sua? Não tarefa, decisão."

Diferenciar decisão (X ou Y, alta reversibilidade) de execução (sei o que fazer, falta tempo). Listar só decisões.

### Prompt 4: Foco da próxima semana
> "Próxima semana, foco em UMA coisa, escrita em 1 frase. Não 3."

Refuse 2+ topics. "Foco" significa 1. Se o user resistir, dizer: "2 focos = 0 focos. Pick 1, deixa o resto pra week+1."

### Prompt 5: Riscos não cobertos
> "O que pode dar errado e ninguém está olhando?"

Esse prompt é o mais valioso. Se a resposta é "nada", insistir: "Founder solo SEMPRE tem riscos não cobertos. Backup? Single point of failure? Cliente único?". Espera 1 nome concreto.

### Prompt 6: Energia
> "Energia 1-10. E o que está sugando energia."

Não-fluffy. Energia importa porque founder esgotado toma decisões piores. Anota o número e o sugador.

## Workflow

### Step 1: Detect prior reviews (if Filesystem available)

Pergunte ao usuário se quer carregar reviews anteriores. Se sim, leia `~/weekly-reviews/*.md` (ou pasta indicada). Use pra detectar padrões recorrentes.

### Step 2: Run the 6 prompts in order

Um por turno. Espera resposta. Não pula.

### Step 3: Identify patterns (only if prior reviews loaded)

Compare a semana atual com as 3-4 anteriores. Padrões reportáveis:

- Loss recorrente na mesma área (3+ semanas) → flag
- Decisão pendente em 2+ semanas seguidas → "isso virou bloqueio"
- Energia caindo 3 semanas seguidas → "burnout sinalizando"
- Foco da semana passada NÃO foi atendido → "trade-off não funcionou"

### Step 4: Generate the review

Compile na estrutura de `templates/weekly-review.md`:

- Plano (vs realizado, se você sabia)
- 3 Wins
- 1-2 Losses (com causa real)
- Decisões pendentes
- Foco semana
- Riscos
- Energia
- (se houver) Padrão da semana
- Próxima sexta, revisita

### Step 5: Offer save options

3 opções:
1. "Salvar em ~/weekly-reviews/2026-W18.md (Filesystem)"
2. "Copiar pra colar no Notion"
3. "Gerar versão pra investor update (mais sucinta, menos íntima)"

## Output rules (Marfin voice)

- **Português brasileiro por padrão.** Inglês se o usuário escrever em inglês.
- No emojis. No hashtags. No em-dash (— ou –). Use dois pontos ou parênteses.
- **Não-coach.** Sem "vc é incrível", sem "mantém o foco". O founder não tá pagando terapia.
- **Específico.** "Loss em sales" é vago. "Demo perdida pra Acme.io porque CFO entrou na call e a gente não tinha resposta pra preço" é específico.
- **Padrões silenciosos.** Se detectar padrão recorrente, simplesmente reportar — não pregar moral.
- **Banned phrases:** "essencial", "crucial", "fundamental", "vital", "mantém o foco", "você consegue", "uma jornada", "abordagem", "ressoar".

## Error handling

| Situation | Action |
|---|---|
| User answers "tudo bem, sem wins" | Insistir: "Mesmo semana ruim tem 1 win. Email respondido? Bug fixado? Lista 1." |
| User lista 8 wins | Cortar pra 3: "Pick os 3 que mais movem ponteiro. O resto vai pra Notion como bonus." |
| User não tem foco pra próxima semana | Forçar: "Sem foco escrito, segunda-feira você abre laptop e responde Slack. Escolhe 1 coisa, escreve agora." |
| User skipa prompt 5 (riscos) | Insistir 1x: "Sério, qual o risco que ninguém olha?" Se ainda skip, reportar no review como "founder declined to identify hidden risk this week" — pattern forte |
| User pede review pra dia da semana errado (ex: terça) | OK: "Weekly review terça é OK desde que coberto até segunda passada. Confirma janela." |
| User começou a usar a skill 1x e quer continuar toda semana | Sugerir: "Quer que eu lembre? Aproveita pra criar `~/weekly-reviews/` agora pra eu detectar padrões nas próximas." |

## Examples

See `examples/`:
- `example-input-pt.md` — week W18/2026 of fictional founder
- `example-output-pt.md` — full review filled

## Files in this skill

```
marfin-weekly-review/
├── SKILL.md
├── README.md
├── templates/
│   └── weekly-review.md
└── examples/
    ├── example-input-pt.md
    └── example-output-pt.md
```
