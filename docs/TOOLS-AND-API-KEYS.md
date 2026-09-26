# Ferramentas, contas e chaves de API

O kit usa **ElevenLabs Scribe para transcrição** e **Kie.ai para vídeo e imagens
gerados** por padrão. São os serviços preferidos do kit, não requisitos para toda
edição. Traga suas próprias contas e créditos e configure só o que você usar.

## O que você precisa

| Ferramenta ou serviço | Quando você precisa | Configuração e custo |
| --- | --- | --- |
| Codex ou Claude Code | Para seguir as skills de edição guiadas pelo agente | Seu próprio acesso ao assistente. Os créditos de serviços externos são separados. |
| Node.js 22+, npm, Git, FFmpeg com ffprobe e Chrome/Chromium | Edição, preview e renderização locais | Instale no seu computador. Veja a [configuração](SETUP.md). |
| HyperFrames e GSAP | Composições de vídeo e animação | Instalados pelo `npm ci`. A renderização roda localmente. Alguns templates usam fontes online ou CDNs; localize os assets escolhidos para a renderização final. |
| ElevenLabs Scribe | Padrão do kit para transcrição de fala | Sua própria chave com acesso a `speech_to_text` e uso disponível. O helper incluído envia o áudio para a ElevenLabs. |
| OpenAI Whisper API | Transcrição alternativa na nuvem | Sua própria chave de API da OpenAI e faturamento de API. Peça ao assistente para configurar a chamada e normalizar os timestamps de palavra. |
| Whisper local ou whisper.cpp | Transcrição alternativa no seu computador | Instalação separada de runtime e modelo. Sem chave de transcrição na nuvem; precisa de download de modelos, espaço em disco e processamento local. |
| Kie.ai | B-roll em movimento, imagens e outros assets gerados (opcional) | Sua própria conta, chave e créditos. Filmagens existentes e assets fornecidos podem ser usados no lugar. |
| Sua filmagem, logos, fontes, música e efeitos sonoros | Montar a edição em si | Forneça assets que você pode usar. O kit não inclui assinatura de mídia de estoque ou de música. |

O starter sintético e as verificações não precisam de API paga de transcrição ou
geração. Escolha um provedor só quando a próxima etapa precisar dele.

## ElevenLabs: o helper incluído

Crie uma chave nas [configurações de API da ElevenLabs](https://elevenlabs.io/app/settings/api-keys)
e coloque-a no `.env` da raiz como `ELEVENLABS_API_KEY`. Depois rode:

```sh
node scripts/transcribe-elevenlabs.mjs video-projects/my-video/assets/raw.mp4
```

O helper grava um JSON com timing por palavra ao lado do arquivo de entrada. Reutilize
um transcript verificado em vez de pagar para transcrever o mesmo take de novo. Veja a
[documentação oficial da API do Scribe](https://elevenlabs.io/docs/api-reference/speech-to-text/convert)
para as opções de modelo e detalhes de uso atuais.

## Whisper e outras alternativas de transcrição

Você pode pedir ao assistente para usar outro provedor. O script
`transcribe-elevenlabs.mjs` incluído chama somente a ElevenLabs; trocar a chave
não muda o provedor dele.

Para a API da OpenAI, use `whisper-1`, `response_format="verbose_json"` e
`timestamp_granularities=["word"]`. Guarde `OPENAI_API_KEY` localmente. Confira os
limites de upload atuais antes de enviar gravações longas; divida o áudio quando
necessário e preserve os offsets de cada trecho. Veja o [guia da OpenAI](https://developers.openai.com/api/docs/guides/speech-to-text).

Para transcrição local, peça ao assistente para configurar o
[OpenAI Whisper](https://github.com/openai/whisper) ou o
[whisper.cpp](https://github.com/ggml-org/whisper.cpp). O Whisper em Python exige
Python e as dependências do modelo; o whisper.cpp tem sua própria instalação. Nenhum
dos dois é instalado pelo `npm ci`.

As ferramentas de corte exigem este formato comum, com **tempos por palavra em segundos**
na linha do tempo exata da gravação de origem:

```json
{
  "text": "Hello world.",
  "audio_duration_secs": 2.0,
  "words": [
    { "text": "Hello", "start": 0.2, "end": 0.6 },
    { "text": "world.", "start": 0.7, "end": 1.2 }
  ]
}
```

Peça ao assistente para mapear o campo `word` do provedor para `text` quando necessário,
preservar os tempos numéricos de início/fim e obter a duração completa da origem pelo ffprobe.
Parágrafos, legendas SRT e transcripts só por segmento não fornecem o timing por
palavra exigido. Solicite timestamps de palavra ou faça alinhamento; não invente timing.
Edições posteriores precisam retemporizar as palavras usando a EDL delas. Planos de
short-form também rastreiam `sourceStart` e `sourceEnd`.

Copie um dos prompts:

> Use OpenAI Whisper em vez de ElevenLabs para esta gravação. Use minha chave de API
> configurada localmente, solicite timestamps de palavra e normalize o resultado para
> este kit antes de cortar. Preserve a origem e verifique o timing contra o áudio dela.

> Use Whisper local para a transcrição. Confira primeiro a instalação e os downloads
> de modelo, depois normalize os timestamps de palavra para este kit. Mantenha a
> transcrição local e use minha filmagem e meus assets sem geração paga.

## Kie.ai: vídeo e assets gerados

O kit usa o [Kie.ai](https://kie.ai/) para vídeo e imagens gerados. Defina
`KIE_API_KEY` no `.env` se você optar por ele. Siga o
[guia oficial da API](https://docs.kie.ai/) para chaves, requisições por modelo,
status de tarefas e créditos.

A skill de short-form descreve um fluxo com Kie; o kit **não** inclui um script
universal de geração Kie nem uma conta conectada. A skill `motion-showreel` inclui um
cliente pequeno (`scripts/kie.mjs`) para os três assets do showreel: música instrumental
Suno, stills Seedream a partir do logo e movimentos de câmera Kling. Ele é opcional: com
assinatura Kling AI, prefira o CLI oficial `kling` (veja a seção abaixo). Peça ao assistente para configurar
o modelo escolhido usando a documentação atual. Adicionar a chave sozinha não cria
uma integração. Entradas de modelo, formatos, disponibilidade e custos em créditos variam.

Prepare os assets propostos e o custo esperado antes de qualquer trabalho pago que você
ainda não tenha autorizado. Guarde os IDs de tarefa, reutilize saídas bem-sucedidas e
salve os resultados escolhidos dentro dos assets do seu projeto privado. Inspecione
movimento, duração, proporção e enquadramento deles na edição final.

> Use Kie.ai para B-roll em movimento nas cenas aprovadas. Confira minha configuração
> local e a documentação atual do modelo, depois me mostre os assets que você propõe e
> o custo esperado em créditos. Reutilize a filmagem existente onde ela couber.

## Kling AI pela sua assinatura (preferido para o showreel)

Quem tem assinatura [Kling AI](https://kling.ai/) usa o CLI oficial `kling` em vez de
passar pelo Kie. Faça `kling login` (OAuth no navegador, sem chave em `.env`) e
`kling who_am_i` para ver os modelos e os inputs de cada um. Depois:
`kling image_to_image --image logo.png "<prompt>"`,
`kling image_to_video --image heroi.png "<prompt>"` e
`kling query_tasks <generation_id>` para pegar as URLs finais. Consome créditos da
assinatura: confirme antes de gerar.

## Música e efeitos sonoros do showreel

A `motion-showreel` funciona só com áudio seu ou gratuito: Pixabay Music e Pixabay
Sound Effects (licença livre) ou Freesound (confira a licença de cada arquivo). Os
scripts de grade, emenda e mixagem são locais (FFmpeg). Como alternativa paga opcional,
`scripts/sfx.mjs` gera um kit de SFX com ElevenLabs sound generation (usa
`ELEVENLABS_API_KEY`, com acesso a sound effects) e `kie.mjs music` gera música Suno.

## Outras ferramentas opcionais citadas pelas skills incluídas

São opções de fluxo, não uma afirmação de que todo exemplo as usou:

- **Narração:** os guias do HyperFrames descrevem o Kokoro TTS local, com configuração
  extra de runtime e modelo. ElevenLabs e HeyGen TTS também são mencionados; cada um
  precisa do seu acesso ao serviço e de uma API ou conector configurado.
- **Captura de site:** o Gemini pode, opcionalmente, fornecer descrições de imagem mais
  ricas. A captura básica não exige essa chave.
- **Outra transcrição:** a referência do framework menciona o Groq Whisper. Ele
  precisa da própria chave e das mesmas verificações de normalização de transcript.
- **Exemplos de integração mais antigos:** acesso ao ClickUp ou à OpenAI só é necessário
  quando o projeto escolhido realmente chama esses serviços.
- **Runtimes auxiliares:** alguns scripts opcionais exigem Python. O Playwright está
  incluído para os fluxos de navegador relevantes; os binários do navegador podem
  precisar de instalação.

Uma skill mencionar uma ferramenta MCP não instala essa integração. Peça ao
assistente para conferir primeiro quais conectores ou clientes de API estão disponíveis.

## Configure só o que você usa

`npm run setup` cria o `.env` a partir do [`.env.example`](../.env.example) se ele não
existir. Deixe as entradas não usadas em branco. Arquivos `.env` existentes são
preservados, então adicione você mesmo as entradas novas que precisar. O helper da
ElevenLabs carrega o `.env` da raiz; outros provedores precisam do próprio cliente ou
carregamento de ambiente configurado pelo assistente.
Mantenha credenciais fora de composições, screenshots e commits.

Confira o preço atual do provedor e o seu saldo antes de chamadas pagas. O student
kit não inclui créditos de transcrição, geração, locução ou assistente.
