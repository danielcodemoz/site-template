# Guia do Dots: publicar um projecto NOVO em <nome>.danielpro.dev (sem SSH)

Actualizado a 24 Nov 2026 (Grok Bot). Não precisas de acesso ao servidor nem de pedir nada ao Grok Bot.

## O que fazer (só 1 ficheiro)
1. Na raiz do repositório novo (`danielcodemoz/<repo>`, **público ou privado**) cria `danielpro-site.json`:
   ```json
   {"subdominio": "<nome>"}
   ```
2. O site tem de ser **estático**: `index.html` na raiz, **ou** faz o build (ex.: `npm ci && npm run build`) e faz commit de `dist/` com `dist/index.html`. Se existir `dist/index.html`, é só o `dist/` que vai para o ar.
3. Põe isso na `main` (merge do PR). Em ~2–5 min o servidor cria `https://<nome>.danielpro.dev`, com HTTPS. Depois, cada push/merge na `main` publica em ~1–2 min.
   - Repos privados: o servidor adiciona sozinho uma deploy key **só de leitura** ao repo. Não a apagues.
   - O mais fácil é criar o repo a partir de `danielcodemoz/site-template`.

## Ver o estado (JSON público, sem segredos)
- `https://danielpro.dev/__site-status/<nome>.json`: estado (`publicado` / `criado-sem-deploy` / `desligado`), commit publicado, último deploy.
- `https://danielpro.dev/__site-status/_repos.json`: o que o servidor decidiu para cada repo com `danielpro-site.json` (`created:<nome>`, `refused:<nome>` + `detalhe`, `invalid-name`, `placeholder-name`).
- O estado actualiza-se de 1 em 1 a 2 min.

## Limites (o servidor recusa sozinho)
- Nome: `^[a-z][a-z0-9-]{1,30}$`, sem hífen no fim. Só subdomínios de `danielpro.dev`; outros domínios não.
- Nomes reservados ou que já existem (www, api, admin, test, willow, educare, guestflow, versa…): recusado. Nunca se altera nem se substitui um site existente.
- O repo tem de ser `danielcodemoz/*` e não pode estar já a publicar outro site.
- Só estático: **sem PHP, sem Node no servidor, sem base de dados**. Apps com base de dados ou backend precisam do Daniel/Grok Bot.
- O servidor não corre comandos de build: o build é teu e o `dist/` vai no commit.
- Nada de `.env`, chaves, tokens ou ficheiros grandes (.exe/.apk/.zip/vídeos) no git.
- Recusado? Escolhe outro nome e faz novo push (só volta a tentar depois de um push novo).
- Não mudes o subdomínio depois de publicado (isso não cria um segundo site).
- **Remover um site não é self-service**: só o Daniel. Um site removido não volta a ser criado sozinho.

## Merges
- Repos novos criados do `site-template`: o workflow `auto-merge-codex` faz o merge dos PRs `codex/...`.
- Repos de aplicações existentes: o merge precisa do OK do Daniel, **excepto** versa, tako-site e coinquest-play (auto-merge só com os testes verdes).

## Reverter
Reverte o PR/commit na `main`; o servidor volta a publicar sozinho. O servidor guarda as últimas 5 versões.
