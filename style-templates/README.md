# Templates de cena

- `dark-graph-paper-template/`: fundo animado; não exige filmagem. Copie o HTML e a
  config para um novo projeto de vídeo, depois adicione o conteúdo da sua história.
- `left-glass-popout/`: um reenquadramento de câmera com um painel de vidro animado.
  Forneça seu próprio `assets/source.mp4` com vídeo e áudio, e ajuste a duração da
  origem, o crop, o texto do card e os timings antes de renderizar.

São pontos de partida de cena independentes, enquanto a style-library contém cards.
Os templates atualmente referenciam um CDN público do GSAP. Para um projeto
autocontido, copie `node_modules/gsap/dist/gsap.min.js` para a pasta de assets dele e
troque o `src` do script para `assets/gsap.min.js`, como faz a configuração do starter.
