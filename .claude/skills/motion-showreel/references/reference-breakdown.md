# Análise da referência: o "CLAUDE / MOTION REEL" do Opus 5.5

Fonte: um motion reel de 15 segundos feito pelo Opus 5.5 para Nate Herk (setembro de 2026). 1920x1080, 60fps, 15,06s, 900 quadros. Analisado com `scripts/analyze-reference.mjs` (contact sheets a 6fps, diferença de luminância por quadro, RMS do áudio e onsets).

## Fatos medidos

- **Andamento:** 128 BPM (tempo = 0,469s). Onsets em 2,39, 2,86, 3,32, 4,25, 4,71, 5,20, 5,67, 6,13 ... estão exatamente a um tempo de distância. Âncora da grade em cerca de 0,51s.
- **Cortes secos caem nos tempos:** 1,77 (flash), 4,23 (inversão), 7,50 (flash da íris), 11,25, 11,73, 12,20, 12,43 (meio tempo), 12,67, 12,93 (meio tempo), 13,13. Todos a no máximo 2 quadros de uma linha da grade.
- **Vão antes do drop:** o áudio cai de cerca de -12 dB para -25 dB em 7,25s, e o flash da íris bate em 7,50s. Um vão, uma batida, no meio.
- **Ritmo de luminância (média 0-255, a cada 0,5s):** 11, 11, 14, 219, 212, 224, 190, 65, 48, 14, 17, 97, 98, 71, 22, 22, 21, 54, 46, 37, 223, 95, 109, 40, 22, 31, 32. Escuro, papel, escuro, saturado, azul-marinho, papel, rajada saturada, preto. Trocas de capítulo invertem o valor.
- **Nível do áudio:** base musical em cerca de -12 dB RMS o tempo todo. Sem narração. Fade out nos últimos 0,25s.

## Timeline

| Tempo | Rótulo HUD | O que acontece | Transição de saída |
|---|---|---|---|
| 0,00-1,78 | 01 Squash & Stretch | Um ponto vermelho cai numa régua e quica. Estica na queda e achata no contato, e uma elipse de chão ondula. Uma linha-guia carrega uma leitura ao vivo (POS, SCL, VEL). | O ponto se arrasta num laser vermelho horizontal, o laser estoura em branco, e o branco vira o papel do capítulo seguinte. |
| 1,78-4,23 | 02 Kinetic Type | As letras de "EVERY" caem com quique e overshoot. "FRAME" ganha uma caixa de seleção vermelha de ferramenta de design (W 929, H 282, SX 100.0%) e um cursor que a arrasta maior. "is a" chega em serifada itálica vermelha. As letras de "DECISION" pousam fora de ordem com motion blur, e a palavra se repete num papel de parede vazado. | O papel de parede gira em faixas diagonais, algumas faixas se enchem de vermelho, e o quadro inteiro inverte para preto num tempo. |
| 4,23-5,30 | (02 continua) | "DECISION" branco brilha no preto. As letras viram pontos vermelhos uma a uma. | Os pontos viram uma fileira de 8. |
| 5,30-7,40 | 03 Shape Language | A fileira vira uma grade em tela cheia. Uma onda vermelha percorre a grade, círculos viram quadrados, quadrados viram cruzes, cruzes viram triângulos, e a câmera inclina e avança na grade. | A grade vira um túnel numa íris de lente. |
| 7,40-9,30 | 04 Particle Systems | Um anel de íris pisca (o drop), uma explosão de partículas vermelhas, brancas e azuis gira sobre azul-marinho e depois se condensa num anel fino. | O anel de partículas se solidifica num toro cromado. |
| 9,30-11,25 | 05 Lookdev / SDF | Um toro cromado iridescente inclina, faz SDF smooth-union em bolhas e depois num cubo. Uma leitura acompanha (SDF, SMOOTH UNION, MORPH 0.000 a 1.994, IOR 1.40, ROUGH 0.02). | O cubo avança para a lente. Corte seco. |
| 11,25-11,73 | 06 Easing | Um gráfico cubic-bezier(0.83, 0, 0.17, 1) no papel. Uma bola vermelha percorre a curva e deixa fantasmas de onion-skin. | Corte no tempo. |
| 11,73-12,20 | Op Art | Listras diagonais vermelhas e pretas atrás de um alvo listrado azul e branco. | Corte. |
| 12,20-12,43 | Glitch | "REEL" em branco sobre azul elétrico com separação RGB e deslocamento em fatias. | Corte no meio tempo. |
| 12,43-12,67 | Isometric | Campo de blocos isométricos azuis e brancos sobre vermelho. | Corte. |
| 12,67-12,93 | Symmetry | Um caleidoscópio de círculos vermelhos e azuis e triângulos brancos em volta de um anel. | Wipe de preto para azul. |
| 12,93-13,13 | (ruído) | Ruído de pixels vermelho, azul e branco enche o quadro. | O ruído se resolve no wordmark. |
| 13,13-15,06 | 07 Fin | "CLAUDE" se resolve do ruído com bloom. Uma linha se desenha embaixo. "MOTION DESIGN" e "SHOWREEL 2026" são digitados com um cursor de bloco vermelho. O ponto vermelho cai como o ponto final com squash e elipse de chão. O slogan "EVERY FRAME WRITTEN IN CODE" aparece por último, apagado. | Segura, depois fade. |

## HUD persistente (todo quadro)

- Marcas de corte nos cantos, braços de 30px, recuo de cerca de 36px.
- Topo à esquerda: `CLAUDE  /  MOTION REEL`. Topo à direita: `● 1920×1080 · 60P · 128 BPM`. O ponto pisca como luz de REC.
- Embaixo à esquerda: timecode `TC 00:00:02:36` correndo. Embaixo no centro: uma régua que se preenche como barra de progresso. Embaixo à direita: `02 · KINETIC TYPE` (o reel usa um traço entre número e nome).
- Mono, caixa alta, tracking de cerca de 0.2em, cerca de 16px, branco 85% no escuro ou tinta 85% no papel. A HUD inverte de cor com o fundo.
- Uma grade de pontos discreta cobre os quadros escuros.

## Por que funciona

1. **Um protagonista, transformado.** O ponto vermelho é bola, depois laser, letras, pontos, grade, partículas, anel, toro e cubo, e no fim é o ponto final. Quase toda transição transforma o objeto atual no próximo. O espectador nunca precisa se reorientar, então 15 segundos parecem um único pensamento contínuo.
2. **O reel narra o próprio ofício.** Os rótulos de capítulo nomeiam a disciplina na tela. Overlays de ferramenta (a leitura de física, a caixa de seleção com cursor, os parâmetros SDF, o gráfico bezier) mostram o trabalho por trás do quadro. Parece reel de designer, não anúncio.
3. **Paleta pequena, com uma cor guardada.** Tinta, papel e um vermelho carregam os primeiros 11 segundos. O azul elétrico só aparece na rajada do clímax, então a cor nova parece o drop.
4. **O valor inverte no tempo.** Trocas de capítulo alternam escuro e claro ou neutro e saturado. Flash, inversão e cortes de íris são os socos, e todos caem em linhas da grade.
5. **Curva de cortes que acelera, com final segurado.** Os capítulos duram 1,8s, 3,5s (com uma inversão interna), 2,1s, 1,9s, 1,9s. Depois vêm seis cortes em 1,9s, espaçados em um tempo e meio tempo, e um lockup de 1,9s deixa o olho descansar. Cerca de 75% desenvolvimento, 13% rajada e 12% resolução.
6. **A densidade respira.** Um ponto, depois uma palavra, um muro de palavras, uma grade de 100, milhares de partículas, um objeto, a rajada, e de novo uma palavra. Pouco e muito se alternam.
7. **Três vozes tipográficas.** Grotesca pesada em caixa alta para afirmações, serifada itálica vermelha para o aparte humano, e mono de tracking largo para a camada de máquina. Tipografia também funciona como imagem: repetida em papel de parede, em faixas e invertida.
8. **Acabamento físico.** Squash and stretch, motion blur nas letras rápidas, bloom em branco sobre preto, leve franja cromática, grão, vinheta e uma íris de lente. Parece filmado, não exportado.
9. **Saltos de escala.** Ponto em macro, padrão em tela cheia, campo cósmico de partículas, close do objeto e um avanço através do objeto até o próximo corte. O movimento de câmera vira a edição.
10. **Uma moldura.** O primeiro objeto volta como a pontuação final, e o slogan chega por último em baixa opacidade. O final responde à abertura.
