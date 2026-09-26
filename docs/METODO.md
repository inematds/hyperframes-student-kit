# O método: editar vídeo com IA em cinco etapas

Este é o raciocínio por trás do kit, tirado do tutorial em que o autor original mostra
edições feitas com um único prompt (set/2026). Use-o como checklist: cada etapa já tem
uma skill ou um script no kit. As receitas de prompt prontas estão em [PROMPTS.md](PROMPTS.md).

| Etapa | O que fazer | No kit |
| --- | --- | --- |
| 1. Transcrever | Transcrição com tempo por palavra. É o que permite que cada gráfico entre na hora certa e que o agente entenda a história. | `scripts/transcribe-elevenlabs.mjs` ou Whisper local ([TOOLS-AND-API-KEYS.md](TOOLS-AND-API-KEYS.md)) |
| 2. Cortar | Tirar ar morto e erros antes de qualquer design. Mesmo se você for editar no seu programa, deixe o agente limpar primeiro. | `cut-silences`, depois `cut-mistakes` |
| 3. Planejar os beats | Dizer o que aparece e em qual fala. Quanto mais claro o combinado, mais próximo o resultado do que você quer. | `video-storytelling`, `hyperframes-video-beats`, `data-anchor` + `validate-beat-sync.mjs` |
| 4. Usar e criar skills | Tudo o que você repete vira skill. A cada rodada, diga o que gostou e o que não gostou e mande atualizar a skill. | `.claude/skills/`, `npm run sync:skills` |
| 5. Verificar em ciclo | O agente renderiza, extrai quadros, olha, corrige e só entrega depois de algumas voltas. Você recebe a versão 5, não a 1. | `preflight.mjs`, `hyperframes lint`, render de rascunho, `VERIFY.md` |

## Prompt ancorado na fala

A forma mais direta de dirigir a edição é amarrar cada elemento a uma frase falada:
"quando eu disser X, mostre Y, em tal lugar da tela". O agente traduz isso em beats com
`data-anchor` (a frase exata da transcrição) e o `validate-beat-sync.mjs` confere se cada
beat entra entre 0,2 s depois e 1,8 s antes da palavra.

Três pedidos que funcionam bem:

- **Posição:** "quando eu citar os dois modelos, mostre o primeiro onde eu aponto à esquerda
  e o segundo à direita, cada um com o logo animado".
- **Legibilidade:** "ponha um card de vidro (liquid glass) atrás de cada elemento animado,
  senão não dá para ler" (os glass cards do `hyperframes-video-beats`).
- **Destaque de fala:** "quando eu disser as três palavras da tese, elas aparecem embaixo,
  destacadas, com um fundo que garanta a leitura".

Você pode descrever emoção em vez de técnica ("quero que dê vontade de assistir até o
fim"). O agente costuma acrescentar ideias próprias, como um zoom lento ou um fundo novo.
Revise essas escolhas no rascunho como faria com um editor humano.

## Provar ao vivo: B-roll, captura e geração

Peça para o agente buscar o material em vez de só animar texto: gravar a tela de um site,
tirar screenshots, usar imagens reais do site ou do catálogo da marca, e gerar imagens
ou vídeos quando faltar algo. A geração de vídeo segue a ordem de
[TOOLS-AND-API-KEYS.md](TOOLS-AND-API-KEYS.md): assinatura própria (Kling pelo CLI `kling`),
depois opções locais ou gratuitas, e só por último serviços pagos com confirmação.

## Da referência à skill

Quando você gostar de um vídeo que outra pessoa fez:

1. Baixe o vídeo e meça: `node .claude/skills/motion-showreel/scripts/analyze-reference.mjs referencia.mp4 qa/ref --fps 6 --bpm 128`
   gera contact sheets e um relatório de cortes, ritmo de luz e áudio.
2. Peça ao agente: "analise por que este vídeo funciona, com base no relatório e nas imagens".
3. Peça: "transforme essa análise numa skill". Foi assim que nasceu a `motion-showreel`.
4. Use a skill no seu tema, dê retorno ("isto ficou ótimo, isto não") e mande atualizar a
   skill. Cada rodada deixa a skill mais parecida com o seu gosto.

## Uma gravação, vários estilos

A mesma gravação pode virar edições bem diferentes só trocando o prompt. Receitas em
[PROMPTS.md](PROMPTS.md#uma-gravação-três-estilos):

- **Quadro branco desenhado à mão**: simples e amigável, como explicar para uma criança;
  cenas em tela cheia ou divididas (quadro à esquerda, rosto à direita).
- **Estilo curso**: câmera em tela cheia e depois recortada em círculo, fundo minimalista,
  poucos tópicos grandes e imagens geradas por IA como apoio.
- **Short dinâmico**: B-roll variado ao fundo, ritmo rápido, cortes frequentes.

## Outros usos

- **Sizzle reel de evento**: aponte uma pasta de gravações e peça uma história. O agente
  transcreve, escolhe falas que mostram a energia do evento e monta o reel. É trabalhoso;
  planeje duas ou três rodadas de revisão.
- **Vídeo de produto ou marca**: imagens reais do catálogo, fotos geradas a partir delas,
  vídeos curtos gerados dessas fotos e tudo montado no tempo da música.
- **Showreel de motion design**: a skill `motion-showreel`.

## Esforço

Os exemplos do tutorial rodaram com esforço **alto**. Para edições completas vale usar
esforço alto; para ajustes pontuais, o padrão basta.
