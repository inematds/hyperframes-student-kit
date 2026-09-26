---
name: motion-showreel
description: Projeta e constrói um showreel de motion design ou brand reel de 10 a 30 segundos no estilo do "CLAUDE / MOTION REEL" do Opus 5.5 — um motivo da marca transformado por capítulos de ofício rotulados, uma HUD persistente, cortes travados numa grade de 120-130 BPM, uma rajada de cortes com menos de um segundo por quadro e um lockup final do logo que responde à abertura. Use quando pedirem showreel, sizzle reel, brand reel, motion reel, "um reel como o do Opus", "mostrar motion design para <marca>", um filme de marca de 15 segundos cortado na música, ou para estudar por que um reel de referência funciona e reconstruir essa energia para outra marca.
---

# Motion Showreel

Um showreel promete que cada quadro foi uma decisão. Esta skill transforma isso num
processo repetível: escolha um motivo, passe-o por seis ou sete capítulos de ofício que
não se parecem entre si, corte no tempo da música e termine no motivo.

A fonte do estilo é [a análise da referência](references/reference-breakdown.md), uma
análise medida, segundo a segundo, do reel a partir do qual esta skill foi construída.
Leia primeiro. As técnicas por capítulo estão na [biblioteca de capítulos](references/chapter-library.md).

## A gramática

1. **Um motivo, transformado.** Escolha o elemento mais reduzido da marca (um ponto, um
   triângulo de play, uma letra, um canto do logo). Ele abre o reel, vira o material de
   cada capítulo e fecha o reel como pontuação no lockup. Quase toda transição transforma
   o objeto atual no próximo. Cortes secos ficam reservados para a rajada.
2. **Capítulos de ofício rotulados.** Seis ou sete, cada um nomeado na HUD pela disciplina
   que demonstra (`01 · SQUASH & STRETCH`). Use as palavras da própria marca quando ela
   tiver (o YouTube tem "capítulos" e "tela final").
3. **Mostre o trabalho.** Cada capítulo ganha um overlay de ferramenta: leitura de física,
   caixa de seleção com cursor, painel de parâmetros, gráfico bezier ou um equivalente
   nativo da marca (barra de busca, contador, menu de configurações).
4. **HUD persistente.** Marcas de corte nos cantos, linha de título no topo à esquerda,
   linha de especificação no topo à direita (`1920×1080 · 60P · 129 BPM`, com ponto
   piscando), timecode embaixo à esquerda, régua de progresso com vãos entre capítulos
   embaixo no centro e o rótulo do capítulo embaixo à direita. Mono em caixa alta,
   tracking 0.2em, cerca de 16px. Inverte tinta/papel junto com o fundo. Parta do
   [template da HUD](templates/hud.html).
5. **Tinta, papel, um acento e uma cor guardada.** A cor guardada só aparece na rajada do
   clímax. Toda troca de capítulo inverte o valor (escuro/claro) ou a saturação
   (neutro/acento em tela cheia).
6. **Corte no tempo, anime nos contratempos.** 120-130 BPM. Todo corte seco, flash e
   inversão cai a no máximo 2 quadros de uma linha da grade. Um vão antes do drop (música
   abafada por meio tempo) vem antes da batida do meio.
7. **Acelere, depois segure.** Para 15s a cerca de 129 BPM (32 tempos): introdução de física
   em 4 tempos, capítulo de tipografia em 8 tempos com uma inversão interna, três capítulos
   de 4 tempos (drop no meio), uma rajada de cerca de 6 cortes em 4 tempos (um tempo cada,
   depois meios tempos) e cerca de 4 tempos de lockup. Aproximadamente 75% desenvolvimento,
   13% rajada e 12% resolução.
8. **A densidade respira.** Um objeto, depois um muro de tipografia, uma grade de 100,
   milhares de partículas, um objeto herói, a rajada, e de novo uma palavra.
9. **Três vozes tipográficas.** Uma display pesada para afirmações, uma voz de acento
   contrastante (serifada itálica ou a fonte de interface da marca) e mono com tracking
   largo para a camada de máquina. Tipografia também funciona como textura: repetida em
   papel de parede, em faixas e invertida.
10. **Acabamento físico.** Squash and stretch, motion blur nos movimentos rápidos, bloom em
    luz sobre escuro, leve franja cromática, grão, vinheta e uma grade de pontos nos quadros
    escuros. Tem de parecer fotografado, não exportado.
11. **Moldura.** O lockup se resolve a partir de ruído, uma linha se desenha, os subtítulos
    são digitados com um cursor de bloco, o motivo pousa como pontuação final com um
    squash, e um slogan discreto chega por último.

## Fluxo de trabalho

Rode os scripts a partir da raiz do kit. Eles precisam de Node 22+ e FFmpeg com ffprobe.

**Assets: gratuito, local ou assinatura própria primeiro.** Música e efeitos sonoros vêm
de fontes gratuitas ou locais (seções 4 e 5). Imagem e movimento do objeto herói vêm, de
preferência, da assinatura Kling AI pelo CLI `kling` (seção 6). Os scripts `kie.mjs`
(Kie.ai) e `sfx.mjs` (ElevenLabs) fazem **chamadas pagas** nos créditos do próprio
usuário: são alternativas opcionais, só rode com confirmação explícita e depois de mostrar
o que será gerado. Todos os outros scripts são locais e gratuitos (só FFmpeg).

### 1. Estude a referência (quando houver uma)

```sh
node .claude/skills/motion-showreel/scripts/analyze-reference.mjs caminho/para/referencia.mp4 video-projects/meu-reel/qa/reference --fps 6 --bpm 128
```

Leia todas as contact sheets e depois o `report.txt` (cortes, ritmo de luminância, RMS,
onsets e a distância de cada corte até o tempo). Escreva a timeline no formato da
análise da referência.

### 2. Levantamento da marca: `BRAND.md`

Colete cores exatas (do arquivo de logo atual, não de um blog), tipografia (a fonte
proprietária e o substituto gratuito mais próximo), SVGs oficiais do logo, a interface e
os objetos característicos, e 3 a 5 fatos verificados, com data, para a tipografia
cinética. Reúna imagens reais que o usuário possui ou pode usar. Escolha o motivo e a cor
guardada do clímax. Só use marcas que o usuário tem direito de usar para esse fim.

### 3. Mapa de capítulos: `STORYBOARD.md`

Crie o projeto com `npm run new-video -- meu-reel` e preencha
[o template de storyboard](templates/storyboard.md). Para cada capítulo, registre o
rótulo da disciplina, o objeto nativo da marca, o overlay de ferramenta, a
transformação de entrada, a de saída, o valor (escuro/papel/acento) e os SFX. Antes de construir:

- Congele qualquer quadro na cabeça: ele se lê como a marca com a HUD coberta?
- Toda transição transforma o objeto, exceto os cortes da rajada?
- O valor inverte em toda troca de capítulo?

### 4. Música e a grade de tempos

Leia `docs/TOOLS-AND-API-KEYS.md` primeiro. Escolha a fonte da música nesta ordem:

1. **Uma faixa do próprio usuário**, com licença para o uso pretendido.
2. **Biblioteca gratuita**: Pixabay Music (licença livre, sem atribuição) ou Freesound
   (confira a licença de cada faixa). Busque por "punchy electronic 128 bpm", "showreel",
   "big drop". Quem tem o `inemavox` pode buscar e baixar direto:
   `python3 musica_v1.py --query "electronic showreel 128 bpm" --min-duration 20 --outdir video-projects/meu-reel/assets/audio`.
3. **Gerada (paga, opcional)**: Suno via Kie.ai. Só com confirmação:

```sh
node .claude/skills/motion-showreel/scripts/kie.mjs music --title "Reel" --style "punchy electronic showreel, instrumental, 128 bpm, starts instantly, riser into a big drop" --out video-projects/meu-reel/assets/audio
```

Depois meça a grade (gratuito, local):

```sh
node .claude/skills/motion-showreel/scripts/music-grid.mjs video-projects/meu-reel/assets/audio/take-0.mp3 --bpm 128 --beats
```

O `music-grid` imprime o período, a **fase da banda do bumbo** (use esta) e a energia por
compasso e por tempo. A fase da banda inteira pode travar nos chimbais do contratempo e
colocar cada emenda meio tempo atrasada; o script avisa quando as duas discordam. Na coluna
de banda grave por tempo, ache o riser, o drop (um salto de 10+ dB), o break e o retorno.
Depois emende 32 tempos: introdução e build (16, terminando no primeiro tempo do drop),
drop (8), break (4, sob a rajada) e a batida de retorno mais uma cauda:

```sh
node .claude/skills/motion-showreel/scripts/splice-music.mjs take-0.mp3 --period 0.4644 --phase 0.085 --segments "4-28,48-56.7" --length 15 --gap 16 --out music-bed.wav
node .claude/skills/motion-showreel/scripts/music-grid.mjs music-bed.wav --bpm 129
node .claude/skills/motion-showreel/scripts/beatgrid.mjs --period 0.4644 --length 15 --chapters "01 Physics:4, 02 Type:8, 03 Grid:4, 04 Drop:4, 05 Lookdev:4, Flurry:4, 07 End:rest"
```

A fase do bumbo da base re-medida deve ficar a cerca de 25 ms de 0. Coloque o BPM medido
na HUD (129, não 128). O `beatgrid` imprime a folha de quadros para o `STORYBOARD.md`.

### 5. SFX e a mixagem

Monte um kit de efeitos (pops, cliques, whooshes, glitch, ticks, shimmer, sub boom, batida
final e sons nativos da marca):

1. **Biblioteca do usuário** ou **biblioteca gratuita** (Pixabay Sound Effects, Freesound).
   Com o `inemavox`: `python3 sfx_v1.py --query "whoosh" --max-duration 3 --outdir video-projects/meu-reel/assets/audio/sfx`,
   um termo por som do kit.
2. **Gerado (pago, opcional)**: `scripts/sfx.mjs` com ElevenLabs sound generation.
   Só com confirmação.

Coloque os cues na folha de tempos num `events.json` e faça a pré-mixagem (local):

```sh
node .claude/skills/motion-showreel/scripts/mix.mjs --bed music-bed.wav --events events.json --out master.wav
```

Use o `master.wav` como a única faixa `<audio>` da composição. Ele mira -14 LUFS e -1.2 dBTP.

### 6. Assets

- **Objeto herói (lookdev).** Precisa de uma imagem fotorrealista feita a partir do logo
  (image-to-image mantém a silhueta) e de um movimento de câmera de ~5s. Opções, em
  ordem de prioridade:
  1. **Assinatura Kling AI própria (preferida):** o CLI oficial `kling`. Rode
     `kling login` e `kling who_am_i` (lista modelos e os inputs de cada um), depois
     `kling image_to_image --image logo.png "<prompt>"` para o still e
     `kling image_to_video --image heroi.png "<prompt>"` para o movimento, acompanhando
     com `kling query_tasks <generation_id>` e baixando de `works[].url`. Consome créditos
     da assinatura: confirme antes.
  2. **Local/gratuito:** um modelo de imagem local (ex.: FLUX.2 klein) com o logo como
     referência, ou um render 3D próprio. Sem movimento gerado, faça o push-in com GSAP
     sobre a imagem parada.
  3. **Kie.ai (pago, opcional, só sem assinatura Kling):** `kie.mjs image --ref <logo.png>`
     (Seedream) e `kie.mjs video --image <still>` (Kling via Kie). Só com confirmação.

  O Kling devolve 24fps: acelere e re-encode em 60fps all-intra
  (`ffmpeg -i hero-raw.mp4 -an -vf "setpts=PTS/3,fps=60" -c:v libx264 -crf 16 -g 1 -keyint_min 1 -bf 0 -pix_fmt yuv420p hero-60.mp4`).
- **Fontes.** Baixe para `assets/fonts/` do projeto e use `@font-face`. Nunca dependa
  de fontes do sistema.

### 7. Construção (HyperFrames)

Carregue `hyperframes` e `gsap` antes de escrever HTML. Leia `MOTION_PHILOSOPHY.md` (o
ritmo de showreel é o modo sizzle rápido dela) e `video-storytelling` para movimentos de
câmera em mundo persistente. Construa um único `index.html` com uma timeline GSAP pausada.
Capítulos são camadas `.clip` temporizadas, e a HUD fica por cima.

- Todo tempo vem da folha de tempos: `const B = (n) => n * PERIOD`.
- Coloque um **driver por quadro** na timeline e faça dele uma função pura do tempo:
  `tl.fromTo(drv, {t:0}, {t:END, duration:END, ease:'none', onUpdate:() => frame(drv.t)}, 0)`.
  Desenhe ali os efeitos de canvas (partículas, ruído, grades grandes) a partir de valores
  com seed, nunca de `Math.random()` na hora do desenho. Controle a HUD pela mesma função.
- Anime só transform e opacity. Nunca anime `left/top/width/height`. Para transformar um
  ponto num bloco, desenhe o bloco final e reduza-o a um círculo
  (`border-radius: 128px / 72px` num bloco de 256x144), depois volte para scale 1.
- Uma propriedade animada por vários tweens ao longo do reel (uma camada de flash
  compartilhada) pode travar num seek para trás. Calcule-a no driver por quadro, e faça
  o mesmo com trocas de classe como uma inversão.
- Meça o layout (`getBoundingClientRect`) uma vez, no início da construção, antes de
  qualquer tween `immediateRender` aplicar um transform. Nunca meça dentro de `onUpdate`.
- `<video data-start>` não pode ficar dentro de um `.clip` temporizado. Ponha vídeos num
  wrapper sem tempo, que ainda pode ser escalado para push-ins.
- Fases escalonadas precisam terminar antes do corte: término mais tardio = início +
  maior atraso + duração. Confira no papel, depois numa faixa de quadros do render.
- `background-clip: text` fica invisível na captura. Use preenchimento sólido e
  `text-shadow` para o bloom.

### 8. Verifique antes de dar por pronto

- `npx hyperframes lint`, depois preview no Studio, depois um render de rascunho (`hyperframes-cli`).
- Rode `analyze-reference.mjs` no seu próprio render com `--bpm` e `--phase`: os cortes
  devem cair a cerca de 35 ms de um tempo ou meio tempo, a luminância deve alternar por
  capítulo e o áudio deve mostrar o vão antes do drop.
- Extraia faixas de 10 quadros em cada transição e olhe. Snapshots não mostram camadas
  travadas ou fantasmas:
  `ffmpeg -i render.mp4 -vf "select='between(n\,440\,449)',scale=384:-1,tile=layout=5x2" -frames:v 1 -fps_mode passthrough strip.png`
- Teste de marca: escolha 6 quadros ao acaso. Cada um deve se ler como a marca com a HUD coberta.
- Loudness perto de -14 LUFS. O HyperFrames re-encoda o áudio um pouco mais baixo, então
  faça o remux do `master.wav` no render (veja o cabeçalho de `mix.mjs`).
- Salve as evidências em `VERIFY.md`.

## Briefing de exemplo

O primeiro teste da skill foi um reel de 15s do YouTube: o ponto vermelho do scrubber
quica numa barra de progresso, vira um laser, digita "broadcast yourself" numa barra de
busca reconstruída, vira oito pontos, depois um feed de thumbnails reais, e explode de um
botão de like em partículas que formam uma estatística verificada. Vira um botão de play
laqueado e uma placa Silver Creator Award, passa por uma rajada de Subscribe, Shorts,
LIVE, Skip e "+10 segundos", e pousa como o ícone de play no logo. Use como modelo de como
mapear capítulos para os objetos nativos de uma marca, não como template para copiar.
