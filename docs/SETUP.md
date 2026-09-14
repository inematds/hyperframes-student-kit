# Configuração e solução de problemas

Comece por [ferramentas, contas e chaves de API](TOOLS-AND-API-KEYS.md): o kit usa
ElevenLabs e Kie.ai por padrão (traga suas próprias chaves e créditos), com alternativas
via Whisper e serviços opcionais.

Use Node 22+ (exigido pela CLI fixada do HyperFrames), FFmpeg com ffprobe e
Chrome/Chromium. Rode `npm ci` na raiz do repositório antes de qualquer comando de projeto.
`npm run setup` verifica a disponibilidade das ferramentas; `npx hyperframes doctor` diagnostica o
renderizador. Se o Chromium não estiver disponível, siga `npx hyperframes browser --help`.
No Windows, reabra o terminal depois de instalar o FFmpeg para atualizar o PATH.

O repositório fixa HyperFrames 0.7.109 e GSAP 3.14.2. Mantenha o package-lock.json
junto com o package.json. Atualize de forma deliberada e repita as verificações de renderização.

`npm run new-video -- lesson-one` copia o starter sintético e o GSAP local para
uma nova pasta autocontida. Nomes de projeto já existentes são recusados para que nada seja
sobrescrito. Substitua o starter pela sua gravação e pelas composições planejadas.
O template de vidro à esquerda precisa do seu próprio `assets/source.mp4` com uma faixa de áudio.

O Codex lê o AGENTS.md e descobre `.agents/skills/`. Marque o projeto como confiável no Codex
se quiser que as configurações dele sejam carregadas. Reinicie se as skills não aparecerem.
O Claude Code lê o CLAUDE.md e `.claude/skills/`. Nenhum dos dois runtimes precisa de um servidor
MCP especial para edição local. O `npm run check` da raiz valida a distribuição;
ele não faz lint do trabalho pessoal em video-projects.

Para atualizar uma skill, edite `.claude/skills/<name>/`, rode `npm run sync:skills` e
depois `npm run check:skills`. Não são necessários privilégios de symlink no Windows.

Erros do Scribe: um 401 geralmente significa autenticação inválida; um 403 pode significar
falta de acesso a speech_to_text. Verifique as permissões da chave sem imprimi-la.
Nenhuma chave é necessária para `npm test`, preview do starter ou renderização do starter.
Para fontes sem fala, forneça uma faixa de áudio separada antes de usar os renderizadores
de corte: eles esperam um stream de vídeo e um stream de áudio.
