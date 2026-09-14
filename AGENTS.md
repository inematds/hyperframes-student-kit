# HyperFrames Student Kit: guia do espaço de trabalho

Transforme uma gravação de talking-head em uma edição intencional usando renderização
local do HyperFrames, cortes guiados por transcrição e motion graphics reutilizáveis.

## Roteamento de runtime

`AGENTS.md` e `CLAUDE.md` contêm o mesmo guia permanente. Mantenha-os sincronizados.
O Claude Code usa `.claude/skills/`. O Codex usa o `.agents/skills/` gerado.
Edite as skills canônicas em `.claude/skills/` e depois rode `npm run sync:skills`.
Use `$edit-video` no Codex, `/edit-video` no Claude Code, ou linguagem natural.
As escolhas de modelo e permissão pertencem ao usuário; `.codex/config.toml` só adiciona
padrões de carregamento de documentos. Trabalhe localmente, a menos que o usuário peça subagentes.

## Início e roteamento

Leia `README.md` para a instalação, `docs/GUIA-RAPIDO.md` para o passo a passo inicial
e `docs/WORKFLOW.md` para uma edição completa.
Use `edit-video` para coordenar transcrição, cortes, design e verificação.
Para uma única etapa, carregue a skill local correspondente:

| Necessidade | Skill |
| --- | --- |
| Reels, Shorts e anúncios curtos | `short-form-edit` |
| Manutenção do exemplo existente May Shorts | `short-form-video` |
| Novo vídeo de motion graphics a partir de um briefing | `make-a-video` |
| Composições inspiradas em sites | `website-to-hyperframes` |
| Edição completa de vídeo bruto | `edit-video` |
| Remoção de silêncios | `cut-silences` |
| Retakes, falsos começos ou gagueiras | `cut-mistakes` |
| Arco narrativo, mundo persistente e callbacks visuais | `video-storytelling` |
| Beats de overlay, paper takeovers e glass cards | `hyperframes-video-beats` |
| Selecionar ou estender estilos e templates | `style-library` |
| Composições HTML e timing de mídia | `hyperframes` |
| Preview, lint e renderização | `hyperframes-cli` |
| Animação de timeline | `gsap` |
| Instalar blocos do catálogo HyperFrames | `hyperframes-registry` |

Antes de uma sessão criativa, leia `MOTION_PHILOSOPHY.md` e o DESIGN.md do projeto.
O ritmo acelerado de sizzle da filosofia é uma referência de estilo. Dê à fala educativa
espaço para respirar. O briefing do projeto controla ritmo, paleta e tipografia.
Os contratos das skills do framework prevalecem sobre receitas de código históricas do guia de estilo.

## Espaço de trabalho

Crie um projeto com `npm run new-video -- my-video`. Mantenha mídia de origem, EDLs,
transcrições, composições e renders juntos em `video-projects/<slug>/`.
Rode o HyperFrames a partir do diretório desse projeto. Os utilitários de `scripts/` na raiz
aceitam caminhos a partir do diretório de trabalho. `style-library/registry.json` indexa
os cards; cada estilo tem um DESIGN.md, tokens CSS e slots de texto nomeados.
Veja `style-templates/README.md` para templates de cena inteira.

Preserve os arquivos brutos. Use um novo nome de arquivo de saída para cada etapa de edição. Arquive
trabalho obsoleto. Projetos de vídeo, filmagens pessoais, transcrições, credenciais e
renders estão no gitignore. Os 12 projetos já publicados são mantidos como exemplos didáticos; novos projetos
privados continuam ignorados. Não trate os exemplos públicos existentes como permissão para adicionar
novas filmagens pessoais. Nunca imprima segredos nem copie mídia privada para os exemplos da biblioteca.

## Edição e timing

Antes de escolher um serviço de transcrição, geração de assets ou voz, leia
`docs/TOOLS-AND-API-KEYS.md` e confira a escolha de provedor do usuário e a configuração local.
ElevenLabs Scribe é o padrão do kit; atenda pedidos de OpenAI Whisper, Whisper
local ou outro provedor. Normalize os timestamps de palavra verificados para as ferramentas
de corte. O script de transcrição incluído é exclusivo da ElevenLabs. O Kie.ai é opcional
e precisa de uma integração configurada e de créditos. Reutilize transcrições e assets
existentes; torne concretos quaisquer uploads ou chamadas pagas não aprovados antes de perguntar.

Corte os silêncios primeiro; use o vídeo editado E a transcrição retemporizada dele no cut-mistakes.
Revise cada erro em contexto. Repetição intencional não é erro. Registre
as decisões revisadas antes de aplicá-las. Se nenhum corte for necessário, preserve a
entrada ou use uma lista de cortes vazia. Nunca misture timestamps originais com filmagem editada.

Carregue as skills relevantes antes de alterar HTML. Composições raiz usam uma div visível
com id, data-composition-id, data-start, duration, width e height. Elementos temporizados
carregam data-start, data-duration e data-track-index; clipes da mesma track
não podem se sobrepor. Divs temporizadas visíveis usam class="clip"; vídeos não.
Deixe os vídeos mudos e use áudio irmão para a mixagem. Anime um wrapper de vídeo não temporizado.
Registre uma timeline GSAP síncrona e pausada por composição em window.__timelines
usando o ID exato da composição. Mantenha timelines finitas e durações explícitas.
O HyperFrames é dono da reprodução de mídia. Use animação determinística e assets locais.

## Verificação e aprovações

Rode `node scripts/preflight.mjs <project>` e o lint do HyperFrames. Para sub-composições
ancoradas, rode `node scripts/validate-beat-sync.mjs <project>`; cada beat
precisa de data-anchor com uma frase exata da transcrição. Entre entre 0,2 segundo depois
e 1,8 segundo antes dessa palavra. Timelines inline exigem revisão manual de timing.

Revise no Studio antes de renderizar o draft. Revise o draft codificado, extraia e
inspecione hero frames e limites de transição, e ouça as junções de áudio.
Verifique rostos cortados, overflow, flashes pretos, rótulos legíveis, clipping e sincronia
A/V. Resolva os problemas antes do render final. Salve as evidências em VERIFY.md.
Use um servidor de preview com suporte a Range para scrubbing de MP4 (por exemplo `npx serve`).

Honre as aprovações já fornecidas. Quando faltar aprovação de revisão, prepare primeiro
o preview concreto ou a proposta de cortes. Nunca afirme que um resultado de lint prova qualidade
visual. Só publique ou faça upload de um vídeo finalizado quando o usuário autorizar.
Verificações automatizadas de navegador devem ser headless, com Pointer Lock e captura de cursor
desabilitados; a entrada sintética deve permanecer dentro do navegador virtual.

Para edições short-form, leia `docs/SHORT-FORM.md` e use os validadores de plano e de
filmagem. Verificações estruturais complementam a revisão do vídeo e do áudio renderizados.
