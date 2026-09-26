# Biblioteca de capítulos

Cada entrada cobre a técnica, como a referência a usou, como construí-la de forma segura para seek no HyperFrames (GSAP numa única timeline pausada) e trocas por marca. Escolha 5 a 7 e encadeie de modo que a transformação de saída de um seja a de entrada do seguinte.

## Física: squash and stretch
- **Referência:** um ponto vermelho cai numa régua e quica três vezes, com uma elipse de chão e uma leitura POS/SCL/VEL numa linha-guia.
- **Construção:** anime o quique à mão a partir da folha de tempos (um contato em cada tempo). A queda usa `power2.in`, a subida `power2.out`. No contato, aplique `scaleX 1.35 / scaleY 0.7` por 2-3 quadros e volte com `elastic.out(1, 0.5)`. Na queda rápida, estique para `scaleY 1.3`. Calcule o texto da leitura num `onUpdate` a partir do `y` atual do ponto e da velocidade (diferença para o quadro anterior), com uma casa decimal.
- **Transformação de saída:** o ponto acelera de lado e vira uma linha de laser (`scaleX` até 200, `height` 4px, brilho), que estoura num flash em tela cheia (camada branca com opacity 1 por 2 quadros) e vira o papel do capítulo seguinte.
- **Trocas:** o ponto do scrubber numa barra de progresso (YouTube), um ponto de cursor (app SaaS), uma gota (marca de bebida), um pin (mapas).

## Tipografia cinética
- **Referência:** letras caem fora de ordem com overshoot e motion blur. Uma caixa de seleção vermelha com números W/H/SX e um cursor arrasta a palavra maior. Um aparte em serifada itálica vermelha contrasta com grotesca pesada em caixa alta. A palavra-chave se repete num papel de parede vazado, que vira diagonal com faixas vermelhas, e depois o quadro inverte.
- **Construção:** divida as palavras em `<span>`s. Escalone `y: -120 -> 0` com `back.out(2)`, deslocamentos de 1 quadro numa ordem embaralhada. Simule motion blur animando `filter: blur(6px) -> 0` junto com um esticamento em `scaleY`. Para a caixa de seleção, uma div posicionada com 8 alças quadradas acompanha a caixa da palavra (leia a largura uma vez depois de `document.fonts.ready`) e um SVG de cursor se move junto. Para o papel de parede, gere 12 linhas da palavra com `-webkit-text-stroke` (vazado). Gire o contêiner -12deg, deslize linhas alternadas em direções opostas e preencha 2 linhas com o acento. Para a inversão, um `tl.set` troca uma camada de fundo e as cores do texto no tempo.
- **Trocas:** uma barra de busca que digita a frase (YouTube, Google), um input de chat (produtos de IA), uma etiqueta de preço, uma manchete com o primeiro slogan da marca.

## Letras em pontos em grade (linguagem de formas)
- **Referência:** as letras de DECISION viram pontos vermelhos uma a uma, os pontos se alinham numa fileira de 8 e a fileira vira uma grade em tela cheia. Uma onda vermelha ondula, círculos viram quadrados, depois cruzes, depois triângulos, e a câmera inclina e avança.
- **Construção:** uma grade de divs (por exemplo 16x9) com `border-radius`. A onda é função do tempo e da distância a uma origem: no `onUpdate`, para cada célula, `phase = t*speed - dist`, e então defina escala, mistura de cor e raio a partir de `phase`. É seguro para seek porque é função pura de `t`. Transformações de forma são `border-radius` de 50% a 8% e um `clip-path` poligonal. A câmera é um pai com `perspective` e `rotateX`/`rotateZ`.
- **Trocas:** uma grade 16:9 de thumbnails (YouTube) com imagens reais surgindo nas células, ícones de app, SKUs de produto, um mosaico de pixels do logo.

## Sistema de partículas
- **Referência:** uma íris de lente pisca no drop do meio, partículas explodem em vermelho, branco e azul sobre azul-marinho, giram e se condensam num anel.
- **Construção:** um único `<canvas>` com 2.000 a 4.000 partículas. Pré-calcule posições iniciais e alvos com seed (qualquer forma: anel, logo a partir de pixels amostrados de uma imagem, um número). Desenhe cada partícula por `lerp(start, target, ease(clamp((t - t0)/d - delay_i)))` mais um desvio em espiral `sin(i*1.7 + t*3)*amp*(1 - progress)`. Redesenhe no `onUpdate` de um tween proxy para que o quadro seja função pura de `tl.time()`. Use mistura aditiva (`globalCompositeOperation = 'lighter'`) para o brilho.
- **Transformação de saída:** as partículas se condensam na silhueta do objeto herói do capítulo seguinte, e o objeto real entra em crossfade em 2-3 quadros de um flash.
- **Trocas:** explosão de um botão de like (social), confete com as cores do produto, pontos de dados, estrelas.

## Lookdev / objeto herói
- **Referência:** um toro cromado iridescente faz SDF-morph em bolhas e num cubo, com leitura de parâmetros (SDF, SMOOTH UNION, MORPH, IOR, ROUGH), e avança para a lente.
- **Construção:** um still fotorrealista gerado a partir do logo (image-to-image) mais um movimento de câmera do Kling (de preferência pela assinatura, com o CLI `kling`), acelerado cerca de 3x e re-encodado em 60fps all-intra. Para um segundo material, corte ou faça crossfade para outro material do mesmo objeto (laca para cromo, prata para ouro). Coloque uma leitura numa linha-guia com números que sobem no `onUpdate`. O push de saída é `scale 1 -> 3` mais `blur 0 -> 20px` no último meio tempo, com corte seco no tempo.
- **Trocas:** uma placa de prêmio de criador (YouTube), uma garrafa ou lata, um aparelho, uma escultura do logo.

## Gráfico de easing
- **Referência:** uma curva cubic-bezier(0.83, 0, 0.17, 1) no papel, com uma bola que a percorre deixando fantasmas de onion-skin.
- **Construção:** um path SVG desenhado com `stroke-dasharray`. A posição da bola vem da avaliação da bezier em `ease(t)`, e os fantasmas são 8 cópias em tempos anteriores com opacidade decrescente.
- **Trocas:** uma curva de crescimento de uma métrica da marca, uma curva de velocidade de reprodução.

## Rajada (4 tempos, cerca de 6 cortes)
- **Referência:** Easing, depois Op Art (listras mais alvo), Glitch ("REEL" com separação RGB), blocos isométricos, Simetria (caleidoscópio) e ruído de pixels, cortados em tempos e depois em meios tempos, introduzindo o azul guardado.
- **Construção:** cada corte é uma camada em tela cheia exibida por `tl.set` no tempo da grade, com um movimento interno (giro, deslize, pop de escala ou gagueira). O glitch usa 3 cópias do texto (vermelha, ciano e branca) deslocadas alguns px com `mix-blend-mode: screen`, mais 4-6 fatias horizontais (`clip-path: inset()`) tremendo em quadros alternados. O ruído é um canvas de pixels grandes nas cores da marca, com seed por quadro.
- **Trocas:** cada quadro da rajada é um momento de interface característico da marca em tela cheia (YouTube: clique em Inscrever-se, deslize de Shorts, selo LIVE, Pular anúncio, toque duplo +10s, selo 4K/HD).

## Lockup (tela final)
- **Referência:** o wordmark se resolve do ruído com bloom, uma linha se desenha embaixo, dois subtítulos mono são digitados com um cursor de bloco vermelho, o motivo pousa como ponto final com squash e elipse de chão, e o slogan aparece por último em baixa opacidade.
- **Construção:** o canvas de ruído desvanece enquanto `filter: blur(20px) -> 0` e a `opacity` do wordmark sobem em 12 quadros, e o bloom é um `text-shadow` ou `drop-shadow`. A linha é `scaleX 0 -> 1` a partir da esquerda. Para a digitação, revele os caracteres com um tween `steps()` numa máscara de largura, ou um `tl.set` por caractere, e um bloco piscando via `tl.set` alternando a cada 16 quadros. A queda do motivo reaproveita as chaves de contato do capítulo de física.
- **Trocas:** o motivo vira o próprio elemento do logo (o ponto do scrubber vira o botão de play, um ponto vira o pingo do i, uma gota vira o O).
