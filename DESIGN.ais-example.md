# AI Automation Society — Identidade Visual

Verdade de base extraída de `assets/AIS Brand Guideline Small.jpg` e `assets/AIS Logo PNG.png`. Toda composição deste projeto DEVE rastrear suas escolhas de paleta, tipografia e movimento até este arquivo.

## Prompt de Estilo

A AI Automation Society é uma marca técnica, confiante e orientada a comandos — "Bloomberg Terminal encontra lançamento de SaaS". As composições devem parecer uma sala de controle sendo ligada: telas em azul-marinho profundo, acentos nítidos em azul-ciano, tickers monoespaçados, movimento cortado, e um único acento laranja quente usado apenas onde o contraste importa. Não é brincalhona. Não é carregada de gradientes. Não é neon. O clima é precisão, urgência e autoridade — uma comunidade de builders que entregam.

## Cores

| Token | Hex | Papel |
|---|---|---|
| `--ais-bg` | `#07121c` | Fundo primário (azul-marinho profundo) |
| `--ais-surface` | `#0d2031` | Cards, painéis, superfícies |
| `--ais-surface-2` | `#195066` | Superfície secundária (teal-marinho) |
| `--ais-border` | `#252d33` | Bordas, divisores, linhas finas |
| `--ais-accent` | `#37bdf8` | Acento primário — destaques, números, CTAs |
| `--ais-accent-glow` | `#0307ff` | Brilho externo do logo (75% de opacidade, 50px de blur) |
| `--ais-warn` | `#f09025` | Acento secundário — use com parcimônia para contraste |
| `--ais-text` | `#ffffff` | Texto primário sobre fundo escuro |
| `--ais-text-dim` | `#96a2b6` | Texto secundário/meta |

## Tipografia

- **Roboto Mono (Medium 500)** — monoespaçada. Use para: rótulos de UI, estatísticas, números, linhas de terminal, texto de pills, URLs, CTAs.
- **Montserrat (Light 300 / Bold 700)** — sans display. Use para: títulos, corpo de texto, taglines. Light 300 para corpo, Bold 700 para impacto.

Combine as duas — nunca use só uma. Rótulos em Roboto Mono acima de títulos em Montserrat é o padrão da casa.

## Logo

- Arquivo: `assets/AIS Logo PNG.png` — wordmark "AIS" branco em itálico com brilho externo azul, fundo transparente.
- Brilho em CSS quando colocado sobre fundo escuro: `filter: drop-shadow(0 0 50px rgba(3, 7, 255, 0.75));`
- Área de respiro: meia altura do logo de margem em todos os lados.
- Nunca recolorir. Nunca esticar. Nunca adicionar efeitos além do brilho especificado.

## Regras de Movimento

- **Só entrada** (conforme a regra da skill Hyperframes): todo elemento entra animado via `gsap.from()`. As transições entre cenas cuidam das saídas.
- **Paleta de easing:** `power3.out`, `expo.out`, `back.out(1.4)`, `power4.out` para entradas; `power2.in` para passagens em direção às transições; `sine.inOut` para loops de ambiente.
- **Use pelo menos 3 eases diferentes por cena.** Varie a sensação.
- **Faixas de duração:** entradas rápidas 0.3–0.5s, entradas de título 0.5–0.8s, derivas de ambiente 2–4s.
- **Desloque a primeira animação** 0.1–0.3s a partir do início da cena.
- **Stagger de texto:** 0.04–0.08s por caractere em tipografia display, 0.12–0.18s por palavra em títulos.
- **Números:** use GSAP `{innerText: N, snap: {innerText: 1}}` para contagem crescente, adicione `font-variant-numeric: tabular-nums`.

## Transições

Todas em CSS (não shader) para que as cenas permaneçam simples.

| Mudança de cena | Transição | Duração | Ease |
|---|---|---|---|
| 1 → 2 | Zoom through | 0.35s | `power4.inOut` (abertura em clímax) |
| 2 → 3 | Push slide para a esquerda | 0.35s | `power2.inOut` |
| 3 → 4 | Push slide para a esquerda | 0.35s | `power2.inOut` |
| 4 → 5 | Blur crossfade | 0.5s | `sine.inOut` (desaceleração rumo ao CTA) |

Primária = push slide (60%). Acentos = zoom through (abertura) + blur crossfade (encerramento).

## Botões

Pill arredondada, preenchimento transparente, borda de 1.5px em `--ais-accent`, Roboto Mono em caixa alta, 16–18px, padding de 14–18px vertical + 28–36px horizontal, brilho interno sutil no hover (não há hover em vídeo renderizado — usado para peso visual).

Exemplo: `[ JOIN THE SOCIETY → ]`

## Iconografia

Traço fino, 1.5–2px de espessura, azul-acento (`#37bdf8`), sem preenchimentos. Setas chevron (`»`), telas, gráficos, corações, lâmpadas.

## O que NÃO Fazer

1. **Nada de gradientes lineares em tela cheia** sobre fundos escuros — banding no H.264. Use `--ais-bg` sólido + brilho radial localizado atrás dos elementos focais.
2. **Nada de rosas neon, roxos ou verdes saturados.** A paleta é marinho + ciano + branco + um único laranja quente. Só isso.
3. **Nada de Arial, Helvetica, Roboto (sans), Inter ou fontes de sistema.** Só Roboto Mono + Montserrat.
4. **Nada de ícones ou ilustrações fofos** — a marca é centro de comando, não dashboard brincalhão.
5. **Nada da palavra-chave `transparent` em gradientes** — regra de CSS compatível com shader. Use `rgba(7,18,28,0)`.
6. **Nada de `Math.random()` ou `Date.now()`** — determinismo do render. Use PRNG com seed se necessário.
7. **Nada de animações de saída** em nenhuma cena exceto a final — as transições cuidam das saídas.
8. **Nada de esticar o logo.** Mantenha a proporção. Respeite a área de respiro.

## Referências de Arquivo

- `assets/AIS Logo PNG.png` — logo oficial
- `assets/AIS Brand Guideline Small.jpg` — o one-pager de onde estas especificações vieram
- `assets/brand-tokens.css` — as variáveis CSS `:root` importadas por toda composição
