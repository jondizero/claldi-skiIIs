# Deploy sem terminal — guia de referência

Isso é configurado **uma vez só**. Depois disso, publicar um deck novo é só arrastar uma pasta no site do GitHub — nada de terminal, git, ou linha de comando.

## Configuração inicial (fazer uma única vez)

1. Em github.com, criar um repositório novo e vazio (ex: `audit-decks`)
2. Fazer o primeiro upload manual: dentro do repo, "Add file" → "Upload files", subir a pasta `assets/icons/` (ícones genéricos de plataforma — gmail, discord, instagram, etc.) direto na raiz do repositório. Essa pasta é compartilhada por todos os clientes; só se faz esse upload uma vez.
3. No mesmo commit ou em um separado, subir a primeira pasta de cliente como `decks/{client-slug}/` (o GitHub cria as pastas do caminho automaticamente)
4. Em vercel.com, "Add New Project" → "Import Git Repository" → escolher esse repositório → Deploy
5. Pronto — esse projeto Vercel fica permanentemente ligado a esse repositório. Nunca mais precisa criar projeto novo, e a pasta `/assets/icons/` nunca mais precisa de novo upload.

## Toda vez que a skill gerar um deck novo

O output já vem pronto em `/mnt/user-data/outputs/decks/{client-slug}/` (index.html + images/), seguindo exatamente a estrutura de pasta que o repositório espera.

Passos pro usuário (sem terminal):
1. Baixar o `.zip` que a skill entrega
2. Abrir o repositório em github.com
3. "Add file" → "Upload files"
4. Arrastar a pasta `{client-slug}/` inteira (descompactada) pra dentro da pasta `decks/` do repositório
5. "Commit changes"
6. Aguardar ~30-60s — o Vercel publica sozinho em `{projeto}.vercel.app/decks/{client-slug}/`

Nenhuma etapa aqui usa git, terminal, ou linha de comando — é só arrastar e soltar no navegador.
