# Da gravação à edição finalizada

## 1. Crie um projeto e transcreva

Na raiz do repositório rode `npm run new-video -- my-video`. Coloque sua gravação
em `video-projects/my-video/assets/raw.mp4`. Preserve este original.

```sh
node scripts/transcribe-elevenlabs.mjs video-projects/my-video/assets/raw.mp4
```

Este é o padrão do kit e envia o áudio para a ElevenLabs usando sua própria chave e créditos.
Você pode escolher OpenAI Whisper, Whisper local ou outro provedor; veja
[configuração de provedores e normalização de transcrição](TOOLS-AND-API-KEYS.md).
Você também pode fornecer uma transcrição JSON existente com marcação por palavra,
com `words: [{text, start, end}]` e `audio_duration_secs`.

## 2. Remova o ar morto

```sh
node .agents/skills/cut-silences/scripts/cut-silences.mjs video-projects/my-video/assets/raw.json --video video-projects/my-video/assets/raw.mp4 --out-dir video-projects/my-video/assets --output video-projects/my-video/assets/silenced.mp4 --apply
```

A transcrição retemporizada é `raw.silence-transcript.json`. Ajuste os parâmetros de intervalo e
respiração na skill; não remova pausas que carregam significado.

## 3. Revise os erros

```sh
node .agents/skills/cut-mistakes/scripts/find-cut-candidates.mjs video-projects/my-video/assets/raw.silence-transcript.json --out-dir video-projects/my-video/assets
```

Leia cada proposta em relação à fala. Escreva as decisões revisadas em
`approved-cuts.json` usando `{ "cuts": [{ "start": 1.2, "end": 1.5, "reason": "stutter" }] }`.
Esses tempos estão na linha do tempo SILENCIADA. Uma lista de cortes vazia é válida.

```sh
node .agents/skills/cut-mistakes/scripts/apply-cuts.mjs video-projects/my-video/assets/raw.silence-transcript.json --cuts video-projects/my-video/assets/approved-cuts.json --video video-projects/my-video/assets/silenced.mp4 --out-dir video-projects/my-video/assets --output video-projects/my-video/assets/clean.mp4 --apply
```

Use `raw.mistakes-transcript.json` junto com `clean.mp4`. Copie essa transcrição para o
`assets/transcript.json` do projeto para a validação de beats. Nunca use os tempos do bruto
contra o vídeo limpo. Usuários do Claude podem substituir `.agents` por `.claude`.

## 4. Projete a camada visual

Leia video-storytelling e a filosofia de movimento. Preencha uma beat sheet com a
âncora falada, a ideia em tela, o visual, o tempo de entrada e o callback. Escolha um estilo e
copie os cards com seus CSS/assets para a sua composição. Preserve o rosto de quem fala
e use um wrapper não temporizado para reenquadramento de câmera. Veja a skill style-library.
O template de papel quadriculado é um fundo; ele precisa do conteúdo da sua história por cima.

## 5. Verifique, visualize e renderize

```sh
node scripts/preflight.mjs video-projects/my-video
node scripts/validate-beat-sync.mjs video-projects/my-video
cd video-projects/my-video
npx hyperframes lint
npx hyperframes preview
```

Rode o beat-sync apenas para sub-composições externas ancoradas; ele falha quando não há
nenhuma. Para composições inline, verifique o timing manualmente. Revise no Studio e depois
renderize um rascunho. Inspecione os quadros codificados nos momentos-chave e nas transições, e
ouça as junções. Corrija os problemas e escreva o VERIFY.md antes de uma renderização padrão.
A publicação é uma ação separada que exige a instrução do aluno.
