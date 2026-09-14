# Verificação de release

## Student kit consolidado, versão 2

- Todas as 14 skills canônicas passaram na validação de frontmatter, e todos os 94
  recursos de skill batem com os espelhos do Codex. A verificação de distribuição
  reporta zero erros nos 406 cards e nas duas árvores de skills.
- Um `npm ci` limpo foi bem-sucedido. Todos os 11 testes de comportamento passaram,
  incluindo as novas verificações de drift de legenda, mapeamento de origem incorreto,
  cobertura de cena, reutilização de filmagem, linhas ausentes no ledger e hashes de
  filmagem alterados.
- O starter passou no preflight e no lint do HyperFrames com zero erros ou avisos.
- A verificação de fumaça com mídia sintética passou de novo: saída de silêncios com
  3,960 segundos, saída limpa com 3,673 segundos contra um transcript de 3,660 segundos.
- Todos os arquivos originais de `video-projects/` e dos `assets/` da raiz foram mantidos
  sem alteração. Não foram todos re-renderizados. Nenhum reel real ou geração paga foi
  executado para esta migração; a verificação de short-form cobre seus validadores e o
  pacote de recursos.

## Evidência de release do pipeline original

A evidência de renderização a seguir vem do release do pipeline anterior a esta
mesclagem. Seu starter e sua implementação de edição foram mantidos.


Verificado localmente no Windows, em 8 de setembro de 2026, usando Node 24.15.0,
FFmpeg 8.1.1 e HyperFrames 0.7.109.

- A instalação limpa de dependências foi bem-sucedida; o npm reportou zero vulnerabilidades conhecidas.
- `AGENTS.md` e `CLAUDE.md` da raiz batem. Dez pacotes de skills e todos os recursos
  de apoio batem com seus espelhos gerados para o Codex.
- A verificação do kit valida a sintaxe JavaScript, o frontmatter das skills e os recursos
  vinculados, as contagens do registry, IDs únicos, slots declarados e dependências locais
  dos cards. Todos os 406 arquivos de card resolvem. Dois cards constroem alguns slots em
  JavaScript; as declarações no código-fonte deles são verificadas, não o DOM executado.
- Cinco testes de comportamento passam: retemporização de silêncios, remoção revisada de
  gagueira, comportamento de identidade com corte vazio, limites de corte inválidos e
  timing de beat adiantado/atrasado. A CLI aceita caminhos de projeto explícitos e ordem
  de atributos independente dos exemplos antigos.
- `npm run test:media` gerou um padrão de teste de oito segundos com um tom senoidal,
  renderizou as duas etapas de edição e montou o HTML de revisão da EDL. A saída de
  silêncios teve 3,960 segundos; a saída limpa teve 3,673 segundos contra um transcript
  de 3,660 segundos. As durações de A/V diferem em menos de 0,04 segundo.
- O starter passou no preflight e no lint do HyperFrames com zero erros/avisos.
  O Studio carregou e reproduziu até oito segundos. A renderização de rascunho produziu
  240 quadros H.264 em 1920x1080 a 30fps. Os quadros extraídos foram inspecionados
  visualmente ao longo da sequência de oito segundos. Nenhum intervalo preto de 0,1
  segundo ou mais foi detectado.

## Escopo e limites

Este é um toolkit funcional, não uma promessa de julgamento editorial totalmente automático.
Os 406 cards da biblioteca mantêm status de rascunho; este release não renderizou nem
aprovou visualmente cada um deles individualmente. Teste cada card escolhido com a sua cópia.
O starter é só motion graphics, sem fala. O exercício de fumaça com mídia verifica timing
e integridade de stream com um tom, não a qualidade subjetiva do corte de fala.
A transcrição paga do Scribe não foi chamada durante o QA do release. Os templates exigem
adaptação; o glass popout precisa da filmagem do aluno. O HyperFrames pode baixar
substituições de fonte na primeira renderização. As verificações multiplataforma rodam
no CI do GitHub; o QA local de mídia e renderização foi feito no Windows.

Repita: `npm ci`, `npm run check`, `npm test`, `npm run test:media`, depois crie um
novo projeto starter e faça lint, preview, renderização e inspeção da saída dele.
