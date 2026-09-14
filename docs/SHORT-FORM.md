# Reels e YouTube Shorts

Use `short-form-edit` para uma gravação bruta de apresentador, reel, YouTube Short ou
anúncio curto. O padrão é 1080x1920. Peça uma versão 1920x1080 composta separadamente
quando necessário; um recorte central não é uma segunda composição.

## Início

Rode `npm run new-video -- my-reel` e então peça ao Codex (`$short-form-edit`) ou ao Claude
Code (`/short-form-edit`) para editar sua gravação local dentro desse projeto. Informe
o público, a ação pretendida, a duração alvo e as preferências de marca.
O starter é horizontal; a skill precisa definir as dimensões da composição, o layout
e os metadados do reel para 1080x1920 antes de escrever. Mantenha toda a gravação de origem e
as saídas dentro do novo projeto.

1. Transcreva com ElevenLabs Scribe, com a alternativa que você escolher, ou reutilize marcações por palavra verificadas.
2. Revise os erros em contexto e estabeleça uma única lista de decisões de edição alinhada a quadros.
3. Explore três direções de abertura e escolha uma promessa e uma recompensa verdadeiras.
4. Planeje cenas, legendas exatas, imagens em movimento e pistas sonoras em relação às palavras mantidas.
5. Construa a composição com as skills HyperFrames e GSAP. Use um livro-razão de imagens
   para registrar a procedência e evitar o reuso não intencional da mesma cena de origem.
6. Valide, visualize, renderize um rascunho, inspecione quadros e ouça a edição completa.
7. Resolva problemas de timing, áudio e composição antes de exportar o vídeo final.

O kit usa ElevenLabs Scribe por padrão para transcrição e Kie.ai para vídeos e imagens
gerados; traga suas próprias chaves e créditos para esses serviços. O Whisper
é uma alternativa de transcrição, e imagens existentes podem substituir assets gerados.
Veja [ferramentas, contas e chaves de API](TOOLS-AND-API-KEYS.md) para configuração e prompts
copiáveis. Respeite a autorização já existente ao propor assets pagos. O exemplo sintético
de validação não precisa de nenhum serviço pago.

## Valide a partir da raiz do repositório

```sh
npm run validate:short-form -- video-projects/my-reel
npm run validate:footage -- video-projects/my-reel
node scripts/preflight.mjs video-projects/my-reel
```

O primeiro validador lê `assets/plan.json`, `assets/transcript.json` e
`assets/edit-decisions.json`. O segundo lê o plano e, para imagens em movimento,
`assets/footage-ledger.json`, verifica hashes e confirma que os arquivos referenciados existem.
Siga o [esquema do plano](../.claude/skills/short-form-edit/references/plan-schema.md).
Use a [planilha de referência](../.claude/skills/short-form-edit/references/reference-analysis.md)
ao analisar um vídeo de referência fornecido.

Experimente a fixture sintética sem gravação nem chave de API:

```sh
npm run validate:short-form -- examples/short-form
npm run validate:footage -- examples/short-form
npm test
```

A fixture contém palavras e dados de timing inventados, não um reel renderizado.
Os validadores verificam a estrutura e o timing declarado. Eles não provam um gancho
convincente, afirmações factuais, direitos sobre as imagens, recorte visual correto ou sincronia audível.
Os portões de qualidade da skill exigem assistir e ouvir a saída real.
