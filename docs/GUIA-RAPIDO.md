# Guia rápido: do zero ao primeiro vídeo editado com IA

Este guia condensa o fluxo mostrado no tutorial em vídeo que acompanha o kit
(a transcrição completa está em [TRANSCRICAO-TUTORIAL.md](TRANSCRICAO-TUTORIAL.md)).
Ele funciona com **Codex** (app desktop ou CLI) e com **Claude Code**; os passos
são os mesmos, muda só o prefixo das skills (`$edit-video` no Codex, `/edit-video`
no Claude Code).

## O que o kit faz

A partir de uma gravação (webcam, tela, ou os dois), o assistente:

1. **Transcreve** o áudio com marcação de tempo por palavra.
2. **Corta** silêncios, gagueiras, falsos começos e retakes.
3. **Planeja os beats** (cada cena é um beat) lendo a transcrição e entendendo a intenção.
4. **Gera as cenas em HTML animado** com HyperFrames e GSAP: cards, legendas
   sincronizadas, crop arredondado da sua câmera, B-roll, música e efeitos.
5. **Verifica em loop**: tira screenshots, confere se cada beat bate com a fala,
   se nada sai da tela, e volta a ajustar até ficar pronto.

Também dá para partir de um roteiro sem gravação (motion graphics, sizzle, demo
de produto): é só pular a etapa 1 e usar `make-a-video`.

## 1. Preparar a máquina

Instale Node.js 22+, Git, FFmpeg (com ffprobe) e Chrome ou Chromium.
Depois:

```sh
git clone https://github.com/inematds/hyperframes-student-kit.git
cd hyperframes-student-kit
npm ci
npm run setup
npm test
```

`npm run setup` confere as ferramentas e cria o `.env`. `npm test` não precisa
de nenhuma chave. Problemas? Veja [SETUP.md](SETUP.md).

## 2. Abrir a pasta como projeto no assistente

- **Codex (app desktop):** New project → Local → escolha a pasta do kit. As skills
  em `.agents/skills/` são descobertas sozinhas; se não aparecerem, marque o
  projeto como confiável e reinicie.
- **Claude Code:** abra o terminal na pasta e rode `claude`. Ele lê `CLAUDE.md`
  e `.claude/skills/`.

O arquivo `AGENTS.md` (idêntico ao `CLAUDE.md`) explica como o projeto funciona.
À medida que você descobre como gosta de editar, anote as regras lá. Exemplo de
pedido que vira regra permanente:

> Sempre que eu pedir para transcrever um vídeo, use a ElevenLabs. Registre
> isso no AGENTS.md.

## 3. Configurar a transcrição

O kit usa **ElevenLabs Scribe** por padrão (rápido; plano de entrada barato).
Whisper local é gratuito, porém mais lento; peça ao assistente se preferir.

1. Crie uma conta em elevenlabs.io.
2. Menu inferior esquerdo → **Developers** → **API keys** → **Create key**.
3. Restrinja a chave a **speech_to_text** (só transcrição). Se for usar efeitos
   sonoros ou música da ElevenLabs, libere esses escopos também.
4. Cole a chave no `.env` da raiz, em `ELEVENLABS_API_KEY=`. Nunca compartilhe
   nem commite essa chave.

Alternativas (OpenAI Whisper, Whisper local, Groq) e o formato de transcrição
exigido estão em [TOOLS-AND-API-KEYS.md](TOOLS-AND-API-KEYS.md).

## 4. Primeiro vídeo: o prompt

Coloque a gravação em `video-projects/<nome>/assets/raw.mp4` (ou só passe o
caminho do arquivo). No Codex use `/goal` para que ele persiga o objetivo até
o fim; no Claude Code, mande o pedido direto. Duas formas de pedir:

**Curta (deixa o assistente decidir):**

> Use edit-video em [caminho]. Corte pausas e erros, planeje a história e
> monte um draft com motion graphics no estilo papel quadriculado escuro.

**Dirigida (você descreve cena a cena):** é assim que se obtém o resultado mais
próximo do que você imagina. Modelo baseado no tutorial:

> Acabei de colocar um vídeo em [caminho]. É uma intro. Quero um resultado
> premium e polido, não uma versão 1.
>
> 1. Transcreva o vídeo: todos os beats e motion graphics precisam sincronizar
>    exatamente com o que eu falo.
> 2. Corte erros e silêncios. Não deve sobrar nem meio segundo de pausa; quero
>    ritmo rápido.
> 3. Só depois comece a animar. A estética é motion design estilo Apple:
>    liquid glass, animações limpas, profissional.
>
> Quando eu digo "motion graphics aqui" (aponto para a esquerda), traga no
> terço esquerdo da tela, de cima para baixo, três tipos de motion graphics
> sobre um card de vidro: cards, documentos, telas de laptop, gráficos.
> Quando eu digo "animações aqui" (direita), traga elementos 3D com sombra e
> profundidade sobre um overlay escuro sutil. Os dois blocos ficam na tela
> enquanto falo deles e saem juntos.
>
> Quando eu falo de legendas, mostre legendas limpas no terço inferior, sem
> cobrir meu rosto, com a palavra falada destacada em azul, sincronizada com a
> transcrição.
>
> Quando eu falo de mover meu rosto, reduza minha câmera a um crop vertical
> arredondado com sombra, centrado, sobre o fundo [imagem] a 50% de opacidade.
>
> Quando eu falo dos meus vídeos anteriores, pegue 4 a 6 arquivos de
> [pasta], abra-os em leque em 3D, com sombra, girando levemente, todos
> reproduzindo ao mesmo tempo (não imagens estáticas).
>
> No fechamento, minha câmera grande à esquerda (tocando topo e base, ainda com
> crop arredondado) e, à direita, ícones animados contando o que eu digo: não
> precisa ser técnico, não precisa ter usado Codex, não precisa saber editar.
>
> Ao terminar, verifique: tire screenshots, confira que os beats batem com o
> texto, que nada sai da tela e que a qualidade é de produto final.

No tutorial, uma intro de 1 min 05 s virou 28 s na primeira passada, em cerca de
18 minutos de trabalho do agente.

## 5. Feedback e segunda versão

Assista ao draft e devolva feedback concreto, como faria com um editor:

> Ótima primeira versão. Mudanças: (1) nos dois primeiros segundos, crie um
> gancho visual on-brand, sem cobrir meu rosto; (2) os vídeos em leque estão se
> atravessando, deixe-os mais espessos e enfileirados atrás uns dos outros para a
> física parecer real; (3) o texto "seus vídeos" está pequeno, aumente. O resto
> ficou bom e sincronizado.

A segunda passada costuma levar menos tempo (10 minutos no tutorial). Repita até
aprovar.

## 6. Ajustes finos no Studio

Enquanto o agente trabalha, o HyperFrames sobe um **Studio** em `localhost`
(`npx hyperframes preview` dentro do projeto). Ali você faz scrub da timeline,
seleciona um elemento e muda tamanho de fonte, posição ou cor na hora, sem
mandar outro prompt. Para um vídeo longo em que só falta um retoque, é mais
rápido que reprompt. Depois: `npx hyperframes render --quality draft --output renders/v2.mp4`.

## 7. Transforme o que deu certo em skill

Quando um resultado ficar do seu jeito, peça:

> Isso ficou ótimo. Transforme esse fluxo em uma skill.

Na próxima vez, em vez do prompt gigante, basta "edite este vídeo com a skill X".
Se algo sair errado, diga o que não gostou e peça para atualizar a skill.
As skills canônicas ficam em `.claude/skills/`; depois de editar, rode
`npm run sync:skills` para espelhar no Codex. O prompt grande é o mais difícil
que vai ser; depois disso é só iterar as skills.

## Onde continuar

- Fluxo completo com comandos: [WORKFLOW.md](WORKFLOW.md)
- Reels e Shorts 9:16: [SHORT-FORM.md](SHORT-FORM.md)
- Receitas de prompt: [PROMPTS.md](PROMPTS.md)
- Estilos e cards prontos: [../style-library/GUIDE.md](../style-library/GUIDE.md)
