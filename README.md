# HyperFrames Student Kit

Kit reutilizável de edição de vídeo para **Codex e Claude Code**.
Traga sua própria gravação. Corte o ar morto, revise os erros, planeje a história e
construa motion graphics com HyperFrames e GSAP.

![Acabou. É o fim de uma era: edição complexa, horas de trabalho e softwares tradicionais dão lugar ao agente](docs/images/capa-acabou.jpg)

**Começando agora?** Leia o [guia rápido](docs/GUIA-RAPIDO.md): instalação, abertura do
projeto no Codex ou no Claude Code, chave da ElevenLabs, o prompt do primeiro vídeo,
loop de feedback e como transformar o resultado em skill. A
[transcrição do tutorial em vídeo](docs/TRANSCRICAO-TUTORIAL.md) está traduzida.

## Exemplos de vídeo curto

Os quadros abaixo são fotos simuladas de apresentador, usadas como referência de enquadramento 9:16.

### Reel de curiosidade: destrave seu projeto

![Reel de curiosidade: destrave seu projeto](docs/images/exemplo-1.jpg)

### Reel de curiosidade: construa um sistema de IA melhor

![Reel de curiosidade: construa um sistema de IA melhor](docs/images/exemplo-2.jpg)

### Anúncio ao vivo

![Anúncio ao vivo](docs/images/exemplo-3.jpg)

## O que está incluído

- **14 skills**, espelhadas para os dois assistentes, com seus scripts auxiliares e referências.
- **406 cards de motion graphics em rascunho** em dois estilos, com manifestos, tokens CSS e slots editáveis.
- **Dois templates de cena:** papel quadriculado escuro e um popout de vidro à esquerda.
- Ferramentas de transcrição, corte de silêncios, detecção de erros, renderização dos cortes revisados,
  retemporização de transcrição, revisão de EDL, validação de sincronia de beats e preflight.
- **Edição de vídeo curto:** reels, YouTube Shorts, planejamento de gancho e recompensa, legendas precisas, B-roll em movimento e revisão de áudio.
- **12 projetos de ensino existentes**, preservados do kit original (ficam só na cópia local; `video-projects/` não é versionado neste repositório).
- Uma composição inicial sintética e uma fixture de edição que não precisam de gravação nem de chave de API.

## Ferramentas e serviços opcionais

**O kit usa ElevenLabs Scribe por padrão para transcrever e Kie.ai para gerar vídeos e
imagens.** Traga suas próprias chaves de API e créditos ao usar esses serviços.
Você pode pedir ao assistente para usar OpenAI Whisper ou Whisper local, ou
fornecer uma transcrição existente com marcação por palavra. Os assets gerados são opcionais.

O starter local não precisa de nenhuma API paga de transcrição ou geração. Veja o
[guia de ferramentas, contas e chaves de API](docs/TOOLS-AND-API-KEYS.md) para as ferramentas obrigatórias,
serviços opcionais, detalhes de configuração e prompts que você pode copiar.

## Instalação

Instale Node.js **22 ou mais recente**, Git, FFmpeg (incluindo ffprobe) e Chrome ou
Chromium. Deixe `node`, `ffmpeg` e `ffprobe` disponíveis no seu terminal.
Depois rode estes comandos no PowerShell, no Terminal do macOS ou em um shell Linux:

```sh
git clone https://github.com/inematds/hyperframes-student-kit.git
cd hyperframes-student-kit
npm ci
npm run setup
npm test
```

O setup verifica as ferramentas e cria o `.env` apenas se ele não existir. Adicione só as chaves
dos serviços que você escolher. O auxiliar de transcrição incluído usa ElevenLabs;
as integrações com Whisper e Kie.ai precisam da configuração descrita no
[guia de ferramentas](docs/TOOLS-AND-API-KEYS.md). [Configuração e solução de problemas](docs/SETUP.md).

## Renderize seu primeiro exemplo

```sh
npm run demo
cd video-projects/demo
npx hyperframes lint
npx hyperframes preview
```

Percorra a animação de oito segundos no Studio. Depois de revisá-la, interrompa o preview
com Ctrl+C e renderize:

```sh
npx hyperframes render --quality draft --output renders/demo.mp4
```

A demo usa GSAP local. O HyperFrames pode baixar e armazenar em cache suas substituições de fontes na primeira renderização. Ela não tem narração. A
[transcrição sintética](examples/editing/source.json) separada exercita as ferramentas de corte;
são dados de teste fictícios, não uma transcrição da animação de título.

## Edite sua gravação

Abra a pasta deste repositório no Codex ou no Claude Code e diga:

> Use edit-video para editar minha gravação em [caminho local]. Mantenha meus exemplos e as
> lições centrais. Aperte o ar morto, me mostre os cortes de erro propostos e use o estilo de
> papel quadriculado escuro com cards de vidro ocasionais. Produza um rascunho revisado.

Codex: `$edit-video`. Claude Code: `/edit-video`. Para uma única operação use
`cut-silences`, `cut-mistakes`, `video-storytelling` ou `style-library`.
Veja o [fluxo passo a passo](docs/WORKFLOW.md), as [receitas de prompt](docs/PROMPTS.md)
e o [caderno de storytelling](docs/STORYTELLING-WORKBOOK.md).

## Crie um reel ou YouTube Short

> Use short-form-edit para transformar minha gravação em [caminho local] em um reel 9:16.
> Construa um gancho e uma recompensa verdadeiros, preserve meu sentido, aperte os erros e
> adicione legendas precisas, imagens em movimento com propósito e design de som. Prepare
> um rascunho para revisão. Use minha gravação existente antes de propor assets gerados.

Codex: `$short-form-edit`. Claude Code: `/short-form-edit`.
A skill inclui referências de planejamento e validadores para timing de legendas, mapeamento
de fontes, cobertura de cenas e reuso de imagens. É um fluxo guiado por agente;
revise o movimento e o áudio reais antes de publicar.
[Passo a passo de vídeo curto e comandos de validação](docs/SHORT-FORM.md).

## Exemplos existentes e migração

Este é o repositório principal do student kit. O kit de pipeline de vídeo mais novo foi
mesclado aqui com os dois históricos Git preservados. Os 12 projetos originais continuam
em `video-projects/`, junto com os exemplos de marca compartilhados originais e as
skills `make-a-video`, `short-form-video` e `website-to-hyperframes`.
Use `short-form-edit` para reels novos; `short-form-video` documenta as composições mais antigas
dos Shorts de maio. [Notas de migração e compatibilidade](docs/MIGRATION.md).

## Explore e personalize

| Recurso | Comece aqui |
| --- | --- |
| Guia rápido passo a passo | [docs/GUIA-RAPIDO.md](docs/GUIA-RAPIDO.md) |
| Transcrição do tutorial em vídeo | [docs/TRANSCRICAO-TUTORIAL.md](docs/TRANSCRICAO-TUTORIAL.md) |
| Instruções compartilhadas dos agentes | [AGENTS.md](AGENTS.md) |
| Vocabulário de movimento e transição | [MOTION_PHILOSOPHY.md](MOTION_PHILOSOPHY.md) |
| Biblioteca de cards e tokens de design | [Guia da biblioteca](style-library/GUIDE.md) |
| Metadados pesquisáveis dos cards | [registry.json](style-library/registry.json) |
| Templates de cena completa | [Guia de templates](style-templates/README.md) |
| Configuração e espelhamento do Codex | [.codex/README.md](.codex/README.md) |
| Verificações de release e limites | [Verificação](docs/VERIFICATION.md) |
| Recursos de terceiros | [Avisos](THIRD_PARTY_NOTICES.md) |

Os cards são **assets de rascunho** reutilizáveis. Teste os cards escolhidos com o seu texto
e a sua gravação. Os templates da biblioteca podem carregar GSAP e Google Fonts de suas
CDNs públicas; localize essas dependências ao montar um projeto final.

As imagens de exemplo são fotos simuladas; nenhuma gravação privada, transcrição, credencial
ou configuração pessoal foi incluída. Os exemplos já públicos anteriormente continuam no
repositório. Pastas novas em `video-projects/` e `raw-media/` são ignoradas automaticamente;
arquivos já rastreados pelo Git continuam rastreados. Crie um projeto novo para a sua própria gravação.
