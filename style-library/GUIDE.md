# Style Library — Guia

A biblioteca de estilos de cards de motion graphics de onde o **Agent 3** puxa para adicionar cards em camadas (tiers) a um vídeo. Cada "estilo" é uma identidade visual nomeada (ex.: um look de explainer inspirado na Vox, ou um look de YouTuber com animação suave) com seu próprio conjunto de variantes de card. Este arquivo é o contrato: leia-o antes de adicionar um estilo ou um card.

## Modelo mental

```
style-library/
├── GUIDE.md                  ← this file (the rules)
├── registry.json             ← GENERATED master index of every style + card (Agent 3 reads this)
├── _blueprint/               ← copy-me skeleton for a new style (never edit in place; clone it)
└── NN-slug/                  ← one folder per style, e.g. 01-vox-explainer/
```

Um estilo = uma pasta. Construímos **um estilo por completo antes de começar o próximo**.

## Identificadores

Esquema em três partes (prefixo numérico + slug + IDs pontuados):

- **Pasta do estilo:** `NN-slug` — ex.: `02-vox-explainer`. `NN` é uma ordem/ID com zero à esquerda, `slug` é um kebab-case legível por humanos.
- **Id do estilo:** o slug sem o número — ex.: `vox-explainer` (estável mesmo que o número mude).
- **Id do card:** `<style>.<tier>.<purpose>[.<variant>]` — ex.: `vox-explainer.t1.stat`, `vox-explainer.t2.list.compact`. Estável, único, autodescritivo.

## Uma pasta de estilo

```
NN-slug/
├── style.json        ← manifest: identity + palette + fonts + motion + the list of cards (with slots, tiers, purposes)
├── DESIGN.md         ← the design spec: palette, typography, motion rules, "what not to do" (the shared design format)
├── tokens.css        ← style-scoped CSS variables (colors, fonts, easings). Cards reference these, never hardcode.
├── references/       ← the reference images you upload for this style (the source of truth for the look)
├── cards/
│   ├── tier1/        ← full-screen takeovers (thesis, big stat, section marker, quote)
│   ├── tier2/        ← supporting cards that keep the speaker visible (label, list, equation, lower-third)
│   └── <custom>/     ← optional "innovations" — any card kind that doesn't fit tier1/tier2
└── preview/          ← per-card preview: <card-id>.mp4 (2-3s loop) + <card-id>.png (poster)
```

Commitados: `style.json`, `DESIGN.md`, `tokens.css`, os arquivos `.html` dos cards, as especificações de design e quaisquer posters de preview originais. A versão para estudantes exclui screenshots de referência de terceiros. Gitignorados: os loops **MP4** de preview, mais pesados (regeneráveis a partir dos cards).

## O contrato do card

Todo card é uma **sub-composição HyperFrames independente** — um `.html` autocontido que faz preview e renderiza sozinho, e que o Agent 3 monta num vídeo via `data-composition-src`. Cada card DEVE:

1. Ter um `<div>` raiz com `id`, `data-composition-id`, `data-start="0"`, `data-width="1920"`, `data-height="1080"`.
2. Registrar exatamente uma timeline GSAP **pausada** em `window.__timelines["<data-composition-id>"]` (chave === id da composição).
3. Puxar todas as cores / fontes / easings das variáveis de `tokens.css` — nunca hardcodar hex ou nomes de fonte. (Esta é a regra "layout sob medida, tokens compartilhados": os layouts são construídos à mão por estilo; paleta/tipografia vivem nos tokens para que fiquem consistentes e ajustáveis.)
4. **Disciplina de fundo por tier:**
   - `tier1` (takeover) → fundo opaco em frame inteiro (ele substitui o vídeo).
   - `tier2` (suporte) → transparente em todo lugar exceto no card em si (ele sobrepõe o vídeo; nunca cubra o rosto de quem fala).
5. Expor seu texto como **slots nomeados**: elementos carregando `data-slot="<name>"` (ex.: `data-slot="headline"`). O Agent 3 preenche o texto dos slots por beat. Declare todos os slots em `style.json` (nome, tipo, máximo de caracteres) para que o agente saiba o que preencher.
6. Ser **determinístico** — nada de `Date.now()`, nada de `Math.random()` sem seed, nada de fetches de rede.

Um card está "pronto" quando passa limpo no lint, faz preview corretamente no Studio e tem um MP4 de preview renderizado + poster.

### Forma standalone vs. montada

Os cards são escritos como **composições standalone** — o div `data-composition-id` fica diretamente no `<body>` e o card inclui seu próprio `<script src=".../gsap.min.js">`, então faz preview/renderiza sozinho. Quando o **Agent 3 monta** um card num vídeo real, ele envolve o card num `<template>` e o referencia com `data-composition-src` (a forma de sub-composição do HyperFrames); o pai fornece o GSAP e o `data-duration`. O envelopamento é uma transformação mecânica que o Agent 3 executa na hora da montagem — os cards da biblioteca permanecem na forma standalone.

## Tiers e propósitos (a taxonomia pela qual o Agent 3 consulta)

- **tier1 — takeover**, propósitos: `thesis`, `stat`, `section`, `quote`, `overview`
- **tier2 — suporte**, propósitos: `label`, `list`, `equation`, `definition`, `lower-third`
- **custom** — qualquer outra coisa; dê a ele uma tag de propósito clara.

O Agent 3 pede ao registry "um card `tier1` `stat` no estilo `vox-explainer`", preenche os slots e sincroniza com a transcrição.

## O loop de construção (por estilo)

1. **Scaffold:** `node scripts/style-library/new-style.mjs <NN> <slug>` → clona `_blueprint/` para `NN-slug/`.
2. **Subir referências** em `NN-slug/references/`.
3. **Definir o look:** preencher `DESIGN.md` + `tokens.css` + os campos de identidade de `style.json` a partir das referências.
4. **Construir cards:** escrever cada card `.html` em `cards/`, adicionar sua entrada (id, tier, propósito, slots, arquivo) em `style.json`. Rodar lint em cada um.
5. **Preview:** renderizar um loop de 2-3s + poster por card em `preview/` (ver abaixo).
6. **Indexar:** `node scripts/style-library/build-registry.mjs` → regenera `registry.json`.
7. **Revisar e iterar** contra as referências, depois passar para o próximo estilo.

## Previews

O movimento é metade da identidade, então os previews são loops MP4 curtos (2-3s) + um PNG de poster, um por card, no `preview/` do estilo. Como cada card é uma composição válida, o preview é produzido renderizando o card sozinho (via CLI do HyperFrames) e capturando o frame 1 como poster. O script de preview é adicionado assim que o primeiro card real existir (para que seja testado contra algo real).

## Notas

- Os `style-templates/` do kit (dark-graph-paper, left-glass-popout) são **apenas referência** — não fazem parte desta biblioteca. Pegue ideias emprestadas, não porte por atacado.
- `MOTION_PHILOSOPHY.md` e `.claude/skills/hyperframes/` (palettes, transitions, typography, visual-styles) são recursos de ofício compartilhados dos quais todo estilo pode se valer.
