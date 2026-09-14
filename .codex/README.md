# Configuração do Codex

Abra a pasta deste projeto no Codex. O `AGENTS.md` contém o guia compartilhado do workspace;
o Codex descobre os pacotes completos de skills em `.agents/skills/` automaticamente.
Use `$cut-silences`, `$cut-mistakes`, `$video-storytelling` ou linguagem natural.

`.claude/skills/` é o canônico. Depois de alterações, rode:

```sh
node scripts/sync-codex-skills.mjs
node scripts/sync-codex-skills.mjs --check
```

O espelho usa arquivos reais para que usuários de Windows não precisem de privilégios de symlink.
Mantenha `AGENTS.md` e `CLAUDE.md` sincronizados. A configuração do projeto não mexe nas
configurações de modelo, autenticação, sandbox e aprovação do aluno.
As configurações do projeto carregam depois que o usuário confia no projeto. Reinicie o Codex
se as skills novas não aparecerem. O Claude Code usa os arquivos canônicos de `.claude/skills/`.

Referências oficiais: [descoberta de skills](https://learn.chatgpt.com/docs/build-skills)
e [configuração de projeto](https://learn.chatgpt.com/docs/config-file/config-basic).
