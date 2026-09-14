# FILOSOFIA DE MOVIMENTO — O Padrão-Ouro

> Desconstrução do spot de 30s **Infinite — Global Payments**. A referência que toda peça de motion em Hyperframes aspira alcançar. Releia o §0 e o §4 antes e durante qualquer sessão criativa.

---

## 0 · As 11 Leis (memorize)

1. **Uma ideia por beat. Corte rápido.** Cena média ≈ **1.5s**. Cada visual entrega UM conceito. Se uma cena diz duas coisas, divida-a.
2. **O preto é a tela.** ~90% de cada frame é preto ou quase preto. O espaço negativo é o design.
3. **A luz é a marca, não a cor.** Gradientes cromados, halos, vinhetas, feixes de luz. A peça é *iluminada*, não *colorida*.
4. **A câmera nunca dorme.** O grid recua, a moeda gira, partículas flutuam, a vinheta respira. Estático = morte.
5. **Motion blur é recurso.** Toda transição monta num rastro de streak/blur — mascara o corte E transmite energia.
6. **Metáforas de objeto carregam significado.** Cartão vermelho = quebrado. Moeda teal = funcionando. A mesma moeda volta 3×.
7. **A paleta é simbólica, não decorativa.** Cada cor é dona de um conceito. Se você não consegue nomear o significado dela, ela não conquistou o lugar.
8. **A tipografia é um personagem.** Palavras ESCALAM 8×, fazem MORPH, BRILHAM. A tipografia conduz ~60% da narrativa.
9. **Segure o hero shot.** Revelação do logo ~2s. Card de encerramento 5s+. Caos cinético → calma = catarse.
10. **Uma textura unificadora.** Grid em perspectiva + marcadores crosshair (+) são a espinha dorsal de toda a peça.
11. **As timelines precisam preencher seus slots.** O HF esconde uma sub-composição no instante em que `timeline.duration()` < `data-duration` → flash de frame preto. Toda timeline termina com `tl.to({}, { duration: SLOT_DURATION }, 0)`. Não negociável. (Receita §3.5; diagnóstico §4.)

**Estrutura em três atos / regra dos três:** problema→marca · benefícios→superfícies→produtos · fundação→CTA→silêncio. 3 benefícios, 3 superfícies, 3 nomes de produto, 3 moedas.

---

## 2 · Vocabulário Visual

### 2.1 Fundos Essenciais

- **Piso de grid em perspectiva** — sempre. `<div>` com `transform: perspective(900px) rotateX(60deg)` + dois eixos `repeating-linear-gradient` em `rgba(255,255,255,.05) 0 1px, transparent 1px 80px`. O GSAP anima `background-position-y` para parallax.
- **Vinheta** — sempre por cima. `radial-gradient(ellipse at center, transparent 30%, #000 95%)`, `pointer-events: none`.
- **Overlay de grain** — sempre. `npx hyperframes add grain-overlay`.
- **Partículas sparkle** (cenas de grid) — 20–40 formas `+` absolutas em interseções aleatórias, opacidade com GSAP `stagger.from('random')`, `repeat: -1, yoyo: true`.
- **Palco de gradiente iridescente** (cenas de roda) — conic-gradient suave ou `<video muted loop>` pré-renderizado para um brilho cromático real.
- **Feixe de luz cônico** (finais) — `conic-gradient(from 180deg at 50% 0%, transparent, rgba(0,255,200,.15) 50%, transparent)`, desfocado.
- **Card de vidro líquido** — gradiente diagonal de 4 paradas `rgba(255,255,255,.075/.025/.010/.055)` + `backdrop-filter: blur(14px) saturate(1.12)` + brilho interno `inset 0 1px 0 rgba(255,255,255,.22)` + borda de 1px. O brilho interno é a assinatura do iOS-26.

### 2.2 Sistema Tipográfico

- **Uma única sans-serif.** Geométrica, tracking generoso (Inter, Suisse Int'l, SF Pro).
- **Gradiente cromado em todos os títulos:** `background: linear-gradient(180deg, #fff 0%, #999 60%, #ccc 100%); -webkit-background-clip: text; color: transparent;`
- **Halo de brilho na ênfase:** `text-shadow: 0 0 20px rgba(255,255,255,0.6), 0 0 40px rgba(255,255,255,0.3)`
- **Revelação palavra por palavra** (não caractere por caractere):
  ```html
  <span class="clip word" data-start="0.0">Global</span>
  <span class="clip word" data-start="0.4">payments</span>
  ```
  `tl.from('.word', { y: 30, opacity: 0, scale: 0.85, duration: 0.6, ease: 'power3.out', stagger: 0.35 })`
- **A tipografia ESCALA dramaticamente.** Rótulos 48px. Tipografia cinética hero 480px+. Anime via `scale`.
- **Title Case para ênfase**, não sentence-case.
- **Varredura de gradiente cromado.** Gradiente de 8 paradas com EXTREMIDADES ESCURAS (evita rasgo nas bordas):
  ```css
  background: linear-gradient(90deg,
    #14110a 0%, #14110a 15%,
    #5a3215 25%, #c84f1c 40%, #e2b53f 55%, #2a8a7c 70%,
    #14110a 85%, #14110a 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-size: 300% 100%;
  background-position: 100% 0;
  ```
  Tween de `backgroundPosition` `100% 0` → `0% 0` ao longo de `0.6s` `power2.out`, `stagger: 0.04`.
- **Palavra-carrier na revelação.** A primeira palavra carrega o impulso com um deslize de 360px; as palavras seguintes decaem: `360 → 120 → 60 → 25 → 12px`. Carrier `expo.out 0.33s`; cauda `power2.out 0.20s`. Ancore nos onsets do Whisper. Nas punchlines, adiante o VISUAL 0.2s em relação ao áudio — lê-se como inevitável.
- **Disciplina de fonte por beat.** Cada beat usa uma família DIFERENTE do Google Fonts; o contraste vende "universos diferentes". Elenco: Instrument Serif · Space Grotesk · Bebas Neue · Inter · EB Garamond italic · Cormorant Garamond italic · Azeret Mono · Geist · JetBrains Mono / SF Mono.

### 2.3 Disciplina de Cor

Para cada cena nova, nomeie a única cor que carrega o beat. Se não conseguir, ela não conquistou o lugar.

Paleta de referência (específica do spot Infinite — o AIS ou outros briefs sobrescrevem): Cromado branco→cinza `#fff → #999` (voz premium/marca) · Vermelho `#e10b1f` (problema/quebrado) · Teal `#33d4c8`/`#5ee2d9` (solução/núcleo) · Magenta/roxo `#a155ff`/`#7e42d8` (velocidade/energia/API) · Azul `#3b82f6`/`#5db4ff` (conexão/global) · Laranja/amarelo neon `#ff9430`/`#ffd84a` (valor/acessibilidade de preço).

**Teto da paleta: 5 matizes ativos, cada um com um significado.** Acima disso, você está decorando.

### 2.4 Vocabulário de Movimento

| Movimento | Receita GSAP |
|------|-------------|
| **Dolly de câmera através do texto** | `tl.fromTo(text, { scale: 1, opacity: 1 }, { scale: 8, opacity: 0, duration: 1.5, ease: 'power2.in' })` |
| **Whip de light-streak** | `gsap.fromTo(streak, { xPercent: -150 }, { xPercent: 250, duration: 0.4, ease: 'power3.in' })` — dispare NO corte |
| **Revelação fantasma de palavras** | `stagger: 0.35` com saídas em `+= 0.5` → sobreposição de 0.15s |
| **Deriva com morph de objeto** | `to A: { x: 200, scale: 0.5, opacity: 0 }`, `from B: { x: -200, scale: 0.5, opacity: 0 }`, ambos 0.5s. O light streak esconde a troca. |
| **Revelação com giro de moeda** | `tl.from(coin, { rotateY: 90, duration: 0.8, ease: 'back.out(1.4)' })` — precisa de `transform-style: preserve-3d` |
| **Cristalizar → wordmark** | Duas timelines alinhadas: a moeda escala 1→0.3 + move para a posição do logo, o wordmark faz fade 0→1, ambos terminam no mesmo X/Y |
| **Pulso de energia ao longo do caminho** | SVG `stroke-dasharray + stroke-dashoffset` 1→0; tween de `boxShadow` no nó ao fim do caminho |
| **Recolorir (sem corte)** | `tl.to(':root', { '--accent': '#ff9430', duration: 0.6 })` — variáveis CSS |
| **Revelação de celular deslizando pra cima** | `tl.from(phone, { y: '100%', duration: 1, ease: 'power3.out' }).from(headline, { y: 30, opacity: 0, duration: 0.6 }, 0.3)` |
| **Roda + painel lateral** | `tl.to(wheel, { rotation: 120, duration: 1.5 }).from(panel, { x: -100, opacity: 0, duration: 0.6 }, '<0.3')` |
| **Deriva de cluster flutuante** | `gsap.to(coins, { y: '-=15', duration: 2, repeat: -1, yoyo: true, ease: 'sine.inOut', stagger: { each: 0.4, from: 'random' } })` |
| **Respiração da vinheta** | `gsap.to(vignette, { opacity: 0.9, duration: 4, repeat: -1, yoyo: true, ease: 'sine.inOut' })` |
| **Whip vertical "cut-the-curve"** (transição padrão entre beats adjacentes) | **Saída:** `tl.to(wrap, { y: -150, filter: "blur(30px)", duration: 0.33, ease: "power2.in" })` · **Entrada:** `gsap.set(wrap, { y: 150, filter: "blur(30px)" })` e então `tl.to(wrap, { y: 0, filter: "blur(0px)", duration: 1.0, ease: "power2.out" }, 0)` — mesma direção nos dois lados, velocidade casada no corte |

**Menu de transições:** whip de light-streak (padrão) · cross-warp morph (objeto→objeto) · recolorir (ideias relacionadas na mesma comp) · slide-up (mockups de produto) · flash-through-white (quebra de ato) · swirl-vortex (rotações de família de produto). Blocos do registry para a maioria: `whip-pan`, `cross-warp-morph`, `flash-through-white`, `swirl-vortex`, `cinematic-zoom`, `chromatic-radial-split`.

### 2.5 Ritmo

- **Duração de cena:** 1.0–2.0s. Mais longa só em momentos hero ou no encerramento.
- **Cadência de revelação:** novo elemento a cada 0.3–0.6s dentro de uma cena. Nenhum vazio > 1s no meio da peça.
- **Stagger de palavras:** 0.3–0.4s narrativo, 0.5–0.6s dramático.
- **Transição whip:** 0.3–0.4s.
- **Holds:** cristalização do logo 1.5–2s · card de CTA 4–6s · títulos de seção 1–1.5s após a revelação completa.
- **Regra da respiração:** a cada ~7–8s de densidade cinética, dê um beat de descanso de 1s.

### 2.6 Mixagem de Áudio

Toda cena de áudio define `data-volume` explicitamente. Use por padrão um pad quente em 0.15, não uma trilha musical.

| Camada | `data-volume` | Papel |
|------|---------------|------|
| Voiceover | `1.0` | Primária, conduz o timing |
| Underscore (pad quente) | `0.15` | Quase imperceptível |
| SFX (cliques, whooshes) | `0.2` | As caudas vazam para o próximo beat |

Conecte como elementos `<audio>` irmãos na raiz, nunca dentro de `<video>`.

---

## 3 · Receitas HyperFrames

### 3.1 Fundo de grid (reutilize em todo lugar)

```html
<div class="stage" data-composition-id="...">
  <div class="grid-floor clip" data-start="0" data-duration="30" data-track-index="0"></div>
  <svg class="crosshairs clip" data-start="0" data-duration="30" data-track-index="1">
    <!-- 16 + marks at grid intersections -->
  </svg>
  <div class="vignette clip" data-start="0" data-duration="30" data-track-index="9"></div>
  <div class="grain    clip" data-start="0" data-duration="30" data-track-index="10"></div>
  <!-- Content in tracks 2–8 -->
</div>

<style>
.grid-floor {
  position: absolute; inset: 0;
  transform: perspective(900px) rotateX(60deg) translateY(20%);
  background:
    repeating-linear-gradient(0deg,  rgba(255,255,255,.05) 0 1px, transparent 1px 80px),
    repeating-linear-gradient(90deg, rgba(255,255,255,.05) 0 1px, transparent 1px 80px);
  background-color: #000;
}
.vignette {
  position: absolute; inset: 0; pointer-events: none;
  background: radial-gradient(ellipse at center, transparent 30%, #000 95%);
}
</style>
```

### 3.2 Transição whip-streak

```html
<div class="whip-streak clip" data-start="3.6" data-duration="0.4" data-track-index="8"></div>

<style>
.whip-streak {
  position: absolute; top: 50%; left: 0;
  width: 40%; height: 8px;
  background: linear-gradient(90deg, transparent, #fff, transparent);
  filter: blur(6px); transform: translateY(-50%);
}
</style>

<script>
gsap.fromTo('.whip-streak',
  { xPercent: -100, scaleX: 0.5 },
  { xPercent: 250, scaleX: 1.5, duration: 0.4, ease: 'power3.in' }
);
</script>
```

O `data-start` da próxima cena cai no pico do streak (~metade do caminho) para que o corte se esconda no brilho.

### 3.3 Truque de Recolorir (sem corte)

```html
<div class="flowchart" style="--edge: #5db4ff; --node-glow: rgba(91,180,255,.6);">
  <!-- nodes use var(--edge) for borders, var(--node-glow) for shadow -->
</div>

<script>
tl.to('.flowchart', {
  '--edge': '#ffd84a',
  '--node-glow': 'rgba(255,148,48,0.7)',
  duration: 0.6, ease: 'power2.inOut'
}, 2.5);
</script>
```

Mesmo DOM, o significado muda (Global → Acessível). Barato de construir, caro de olhar.

### 3.4 Objetos 3D — pré-renderizar ou CSS?

- Cromado iridescente / refração cromática → **MP4 pré-renderizado** (alpha ou loop). WebGL é exagero.
- Movimento planar, texturas planas girando → **CSS 3D + GSAP**.
- Card com motion blur → PNG + keyframe de `filter: blur()` no pico.

### 3.5 Regra do padding de timeline (crítica para o framework)

Toda sub-composição encerra sua timeline com uma âncora no-op:

```js
const tl = gsap.timeline({ paused: true });
// … tweens …
tl.to({}, { duration: SLOT_DURATION }, 0);  // forces timeline.duration() >= SLOT_DURATION
window.__timelines['my-comp'] = tl;
```

O HF aplica `visibility: hidden` quando `timeline.duration() < data-duration` — flash de frame preto na cauda do beat. A âncora tem custo zero de animação, mas mantém a composição viva pelo slot inteiro.

### 3.6 Padrão de proxy GSAP (Canvas 2D / shaders numa timeline)

Conduza a renderização procedural a partir de um único tween que avança um valor de tempo proxy:

```js
const proxy = { time: 0 };
tl.to(proxy, {
  time: DURATION, duration: DURATION, ease: "none",
  onUpdate: () => renderAtTime(proxy.time)
}, 0);
```

Regras:
1. **Canvas 2D é seguro em headless; WebGL ao vivo pode travar o render.** Entregue um fallback em Canvas 2D condicionado a `renderOptions.headless`.
2. **Nada de `Math.random()` / `Date.now()`.** Use PRNGs com seed ou hashes de seno harmônico — os renders precisam ser determinísticos.

### 3.7 Poster + lastframe envolvendo o `<video>`

Tags `<video>` piscam na inicialização e ficam em frame preto depois que a fonte termina. Envolva com stills JPG estáticos:

```html
<img id="beat-poster"    src="assets/beat4-poster.jpg">
<video id="beat-video"   src="assets/beat4-clip.mp4"
       data-start="7.1" data-duration="8.94" data-track-index="5" muted></video>
<img id="beat-lastframe" src="assets/beat4-lastframe.jpg">
```

```js
tl.set("#beat-poster",    { display: "none" }, 7.1);   // hand off at video-start
tl.set("#beat-lastframe", { opacity: 1 },      16.04); // cover video-end → beat-end
```

```bash
ffmpeg -y -ss 0        -i clip.mp4 -frames:v 1 -q:v 2 poster.jpg
ffmpeg -y -sseof -0.04 -i clip.mp4 -frames:v 1 -q:v 2 lastframe.jpg
```

### 3.8 Legendas como irmãs no nível do body

Mantenha as legendas FORA das timelines das sub-composições. Coloque-as no `index.html` fora do `<div>` da composição master, cada uma com `data-track-index ≥ 20` único:

```html
<div class="cap clip" data-start="7.29"  data-duration="1.86" data-track-index="30">HyperFrames by HeyGen.</div>
<div class="cap clip" data-start="9.10"  data-duration="3.44" data-track-index="31">Agents write HTML and render MP4s.</div>

<style>
.cap {
  position: absolute; bottom: 72px; left: 50%; transform: translateX(-50%);
  padding: 12px 22px; border-radius: 14px;
  background: rgba(10, 8, 5, 0.55);
  backdrop-filter: blur(8px);
  font: 500 28px/1.3 Inter, sans-serif; color: #fff;
}
</style>
```

### 3.9 Convenção de comentário nos tweens

O comentário de cada tween de entrada/saída nomeia o tween correspondente no beat adjacente:

```js
// ENTRY — blur de-ramps from 22px to match outgoing "circle" beat's 22px exit.
gsap.set(wrap, { filter: "blur(22px)" });
tl.to(wrap, { filter: "blur(0px)", duration: 0.33, ease: "power2.out" }, 0);

// EXIT — matches incoming "engine" beat's 150px y + 30px blur entry.
tl.to(wrap, { y: -150, filter: "blur(30px)", duration: 0.33, ease: "power2.in" }, 1.89);
```

Se você não consegue nomear o tween correspondente, não projetou a costura.

---

## 4 · Checklist de Pré-voo (antes de declarar "pronto")

- [ ] **Duração média de cena ≤ 2s** na seção do meio
- [ ] **Nenhum vazio > 1s** fora dos holds deliberados
- [ ] **Toda transição usa movimento** (nada de fades secos)
- [ ] **Paleta ≤ 5 matizes ativos**, cada um com um significado
- [ ] **Todo bloco de texto usa gradiente cromado + halo** — nada de branco chapado
- [ ] **Grid + crosshairs em ≥ 60% das cenas**
- [ ] **Vinheta + grain em toda cena**
- [ ] **No mínimo um callback** — um visual que retorna depois
- [ ] **O encerramento segura 4+ segundos**
- [ ] **Verificação visual feita** — frames extraídos, lidos com Read, confirmados: sem rostos cortados, sem overflow de texto, sem beat na palavra errada, sem transições quebradas
- [ ] **Toda timeline termina com `tl.to({}, { duration: SLOT_DURATION }, 0)`** (Lei #11)
- [ ] **Todos os tempos finais de tween encaixam em múltiplos de `1/fps`** — easings de cauda íngreme (`expo.in`, `power4.in`) geram aliasing em fronteiras sub-frame
- [ ] **Rodou o diagnóstico de duração das timelines:**
  ```js
  const p = document.querySelector('hyperframes-player');
  const iw = p.shadowRoot.querySelector('iframe').contentWindow;
  Object.fromEntries(Object.entries(iw.__timelines).map(([k, v]) =>
    [k, +v.duration().toFixed(4)]));
  ```
  Qualquer lacuna em que `timeline.duration() < data-duration` é risco de frame preto.

### 4.1 Dicionário de Código GSAP

| Propósito | Ease | Duração |
|---|---|---|
| Revelação de palavra (slide-in) | `expo.out` | 0.20–0.33s |
| Entrada genérica | `power2.out` | 0.2–0.5s |
| Saída genérica | `power2.in` | 0.2–0.33s |
| SAÍDA com whip de beat | `expo.in` / `power2.in` | 0.2–0.33s |
| ENTRADA com whip de beat | `expo.out` / `power2.out` | 0.5–1.0s |
| Pan de câmera entre paradas | `power2.inOut` | 1.2–2.3s |
| Hold linear (após a entrada) | `"none"` | 0.4–0.65s |
| Assentamento elástico de card | `back.out(1.2–1.5)` | 0.3–0.5s |
| Overshoot de UI | `elastic.out(1, 0.3–0.4)` | 0.20s |
| Respirar / derivar | `sine.inOut` yoyo | 2–4s, `repeat: -1` |

**Staggers:** varredura cromada pelas palavras `0.04` · ondulação de dot-grid por coluna `0.019`, dentro da coluna `0.004` · linhas de code-stream `0.06`.

**Nada de `gsap.defaults()`** — declare ease/duração em cada tween. Bugs de herança são mais difíceis de diagnosticar do que tweens verbosos.

**Esqueleto de timeline:**
```js
(() => {
  const tl = gsap.timeline({ paused: true });
  // … tweens …
  tl.to({}, { duration: SLOT_DURATION }, 0);  // Law #11 anchor
  window.__timelines['<data-composition-id>'] = tl;
})();
```
A chave precisa bater exatamente com `data-composition-id`.

---

## 5 · Anti-padrões (além das inversões das Leis)

- ❌ **`Math.random()` / `Date.now()` / PRNGs sem seed num loop de render.** Use hashes harmônicos: `80 + 220 * Math.abs(Math.sin(i*0.7 + 0.3) * Math.cos(i*1.3 + 0.7))`.
- ❌ **Depender de `npx hyperframes add <block>` em peças de referência.** O vídeo de lançamento do HF instala ZERO blocos do registry. Registry = velocidade; feito à mão = qualidade de referência.
- ❌ **Entregar sem ver os frames.** Lint passando ≠ design funcionando. **VEJA OS FRAMES.**
- ❌ **Grain ou vinheta decorativos.** Não são decoração — são *textura* unificadora. Toda cena, toda vez.

---

## 6 · Resumo

> **Uma ideia por beat, iluminado e não colorido, cinético e não estático, callbacks e não novidade, segure o hero, deixe o encerramento respirar — o grid está sempre por baixo de tudo, toda timeline preenche seu slot, toda saída encaixa numa fronteira de frame e todo corte se esconde dentro de um whip com motion blur.**
