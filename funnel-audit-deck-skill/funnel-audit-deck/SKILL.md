---
name: funnel-audit-deck
description: Gera um deck de slides HTML (auditoria de funil "problema → solução") pra apresentar/enviar a um prospect ou cliente, através de uma árvore de perguntas curtas. Use sempre que o usuário disser "/audit-prospect", pedir pra montar uma "auditoria de funil", "funnel audit deck", ou pedir pra transformar uma lista de bottlenecks/correções num deck de slides HTML pra deploy no Vercel. Não escreve copy de vendas do produto do cliente — escreve a copy da PRÓPRIA auditoria (a apresentação que o usuário vai mandar pro prospect).
---

# Funnel Audit Deck — Skill de Auditoria em Slides

## O que essa skill faz

Conduz o usuário por uma árvore de perguntas curtas (uma de cada vez) e gera como output um deck de slides em HTML puro (scroll-snap, sem dependências além de Google Fonts), pronto pra push num repo GitHub e deploy no Vercel. Reaproveita o template validado com a GAR Capital em `references/gar-capital-example.html` — **copie esse arquivo como ponto de partida e edite**, nunca regenere o CSS/JS do zero.

## Antes de começar

Confira `assets/icons/` — já existe uma bibliotequinha de ícones genéricos de plataforma (gmail, discord, instagram, x, threads, youtube, linkedin, meta, webinar-generic, free-training-generic). Nunca peça pro usuário re-upload desses; use direto se alguma "opportunity" mencionar uma dessas plataformas. Só peça upload de ícone novo se for uma plataforma que ainda não está na pasta.

**Importante sobre caminhos de imagem** — existem duas categorias, e cada uma usa um tipo de caminho diferente:
- **Screenshots específicos do cliente** (prints de fact/fix, logo do cliente): ficam em `images/` dentro da própria pasta do deck, e são referenciados com caminho **relativo**: `src="images/nome-do-arquivo.png"`.
- **Ícones genéricos de plataforma** (os de `assets/icons/`): NUNCA copie pra dentro da pasta `images/` do cliente. Referencie com caminho **absoluto a partir da raiz do site**: `src="/assets/icons/gmail.png"`. Isso funciona porque o Vercel serve o repositório inteiro a partir da raiz, e a pasta `/assets/icons/` é compartilhada por todos os clientes (upload único, feito uma vez no repositório — ver `references/deploy-guide.md`).
- Sempre confira antes de entregar: todo `<img src="...">` de screenshot do cliente deve começar com `images/`; todo ícone genérico deve começar com `/assets/icons/`. Um `src` de imagem sem esse prefixo é um bug — a imagem não vai carregar quando publicada.

O CTA final é sempre o mesmo link (Instagram Direct do usuário) — **não pergunte por isso**:
```
https://www.instagram.com/direct/t/115335963192552/
```

## Árvore de perguntas

Faça uma pergunta por vez, aguardando resposta antes de seguir.

**1. Intake inicial** (uma mensagem só, três respostas):
> "Client Name / Brand Color / Business name (if applicable)"

**2. Tema:**
> "Dark theme or light theme?"
- Dark → `<html data-theme="dark">` (fundo quase preto, texto claro — é o default do template)
- Light → `<html data-theme="light">` (fundo `#e6e6e6`, texto escuro). Os dois já estão implementados via CSS custom properties no template — só trocar o atributo `data-theme` na tag `<html>`, nunca reescrever cores manualmente.

**3. Logo:**
> "Upload the logo (or type 'no' if you don't have one)"
- Se "no" → capa usa só o nome do cliente em texto grande (`h1`), sem `<img>`.

**4. Lista de bottlenecks, já em ordem de prioridade:**
> "List your bottlenecks, most important first"
- Guarde a ordem exata — ela vira a ordem dos slides. Não reordene por conta própria.

**5. Loop por bottleneck da lista** (repita as 4 perguntas abaixo pra cada item, um bottleneck de cada vez, antes de ir pro próximo):
1. `"What's the issue with '[bottleneck]'?"` → vira o parágrafo do slide **Fact**
2. `"What's the fix for '[bottleneck]'?"` → vira o parágrafo do slide **Fix**
3. `"Screenshot of the problem? (or type 'skip')"`
4. `"Screenshot of the fix? (or type 'skip' for a flow diagram instead)"`
   - Se "skip" nessa etapa → gere um `.flow` (3 caixas + setas, ver componente na referência) representando o fix em vez de screenshot.

**6. Opportunities:**
> "Any new opportunities to add? List them one by one — each one becomes its own slide."
- **Regra crítica: cada item vira seu próprio slide. Nunca agrupe múltiplas opportunities num slide só**, mesmo que pareçam relacionadas (ex: "webinar" e "free training" são dois slides separados, não um).
- Pra cada opportunity, pergunte só o headline + 1 frase de contexto. Ícones vêm de `assets/icons/` quando aplicável (ex: opportunity sobre redes sociais → puxa os ícones relevantes automaticamente, sem perguntar).
- O kicker desses slides é **"NEXT-LEVEL OPPORTUNITY N"** (não apenas "OPPORTUNITY N").

**6b. The Big Picture (slide fixo, sempre depois da última opportunity):**
Depois do loop de opportunities, sempre insira um slide `slide-section` fixo, antes do fechamento, com este conteúdo (não pergunte — é padrão):
- kicker: `THE BIG PICTURE`
- h2: `What I See Is Leverage.`
- parágrafo: `None of these recommendations are about adding complexity. We want to create leverage.`
- `.list-terminal.is-solution` (alinhado à esquerda, `max-width:44ch`) com 3 itens:
  - `Leverage that turns more visitors into buyers`
  - `Leverage that turns buyers into long-term customers`
  - `Leverage that makes future growth easier than past growth`
- parágrafo final: `That's the opportunity I believe is sitting in front of you.`

Depois de tudo respondido, confirme um resumo curto (nome do cliente, nº de bottlenecks, nº de opportunities) antes de gerar o HTML final. **Não pergunte pelo texto de fechamento** — o slide de CTA final é sempre fixo (ver item 9 em "Como montar o HTML").

## Como montar o HTML

1. Copie `references/gar-capital-example.html` pra um novo arquivo de trabalho.
2. Troque `--accent-fill` e `--accent-text` (em ambos os blocos `:root/[data-theme="dark"]` e `[data-theme="light"]`) pela `Brand Color` informada — ajuste também `--accent-soft` pra a mesma cor em baixa opacidade (`rgba(R,G,B,0.1)`). São as únicas variáveis que mudam — todo o resto do design system fica igual.
2b. **Sempre inclua `<base href="/decks/{client-slug}/">` no `<head>`**, logo depois do `<meta viewport>`. Isso trava a resolução de caminhos relativos (`images/...`) independente de a URL ser acessada com ou sem barra final — sem essa tag, o deck quebra quando alguém acessa `/decks/{slug}` sem a barra no fim (bug real que já aconteceu). Os ícones genéricos em `/assets/icons/...` não são afetados por essa tag (caminho absoluto já ignora `<base>`).
3. Delete os slides de exemplo (Fact/Fix 01–06, Opportunities 01–03) e reconstrua a sequência com a quantidade real de blocos que o usuário passou, seguindo a ordem exata que ele deu.
4. Slide de capa (regra fixa, não pergunte):
   - `<title>` e `<h1>` seguem sempre o padrão `{Nome do Cliente} — Info Operation` (o `<em>` fica só com "Info Operation"). Nunca use "Funnel Audit" ou variações — é sempre "Info Operation".
   - Business name (se houver) substitui/complementa o nome do cliente na mesma linha do `<h1>`, mas "Info Operation" permanece igual.
   - `.cover-tag` (subheadline embaixo do h1) é **só a data**, sem nome do cliente: `· {MÊS} {ANO} ·` em caps, ex. `· JULY 2026 ·`.
   - Logo (se houver) substitui/acompanha o `<img class="cover-logo">`.
5. Slide de intro: copy fixa, sempre a mesma estrutura (não pergunte — é padrão):
   - kicker/pre-headline: `IT ONLY TAKES 45 SECONDS`
   - h2/headline: `{Nome do Cliente}, I went through your info setup (funnel) and a few things stood out.` (sempre começa com o primeiro nome do cliente + vírgula)
   - parágrafo/subheadline: `Some are quietly costing you revenue. Others are opportunities that could increase your return without increasing your work.`
   - Mantém o `stat-row` abaixo (nº de facts / fixes / opportunities).
6. Cada bottleneck vira 2 slides (Fact N / Fix N), na ordem dada.
7. Cada opportunity vira 1 slide (`slide-section` com `icon-grid` ou `icon-circle` se tiver 5 itens visuais, senão sem ícones), com kicker "NEXT-LEVEL OPPORTUNITY N". Os `<img>` desses ícones apontam pra `/assets/icons/...` (caminho absoluto) — nunca copie o arquivo do ícone pra dentro de `images/` do cliente.
8. Depois da última opportunity, insira o slide fixo "The Big Picture" (ver seção 6b).
9. Slide final (regra fixa, não pergunte — nunca customize esse texto):
   - kicker: `NEXT STEPS`
   - h2: `If this resonates, let's talk.`
   - parágrafo: `I already have a few ideas on how I'd approach this. If you'd like to explore them together, click the button below and we'll take it from there.`
   - CTA: link fixo do Instagram, texto `Message me on Instagram →`.
10. Atualize o `slide-meta` (número/total) de cada slide pro total real.
11. **Checklist do botão `.cta-btn` (confira sempre antes de entregar)** — já vem certo no template de referência, mas confira que nenhuma edição quebrou isso:
   - Retangular, não pill: `border-radius` pequeno (10px), nunca 100px.
   - Grande: padding generoso (`22px 48px` no template), fonte maior que os outros elementos mono.
   - Sempre com animação de hover: `transition` em `transform`/`box-shadow`/`background` + estado `:hover` com `translateY` + leve `scale`, e um `:active` pro clique. Nunca remova o hover/transition ao editar cor ou copy do botão.

## Onde salvar o output (pro deploy sem fricção)

Sempre salve o resultado em `/mnt/user-data/outputs/decks/{client-slug}/` (slug = nome do cliente em kebab-case), com `index.html` + `images/` dentro dessa pasta — nunca solto na raiz de outputs.

**A pasta `/decks/` fica dentro do mesmo repositório GitHub do site institucional do usuário (infinitta-partners), não num projeto separado.** Isso é intencional, por segurança: se um prospect apagar `/decks/{slug}` do link, ele cai no site institucional de verdade (sem problema — é público mesmo). Se apagar só `{slug}`, sobra `/decks/`, que dá 404 porque nunca existe um `index.html` ali. O painel de controle (dashboard) NUNCA fica dentro de `/decks/` por esse motivo — ele mora num caminho separado e não-óbvio (ver seção "Dashboard de controle").

O usuário não usa terminal/git — o fluxo de publicação é 100% pelo navegador (arrastar pasta no "Add file → Upload files" do GitHub). Ver `references/deploy-guide.md` pro passo a passo completo, e mencione esse arquivo se o usuário perguntar como publicar.

## Painel de controle (protegido por login)

O painel vive em `/panel` (não é um arquivo estático — é uma Vercel Function em `api/panel.js`, protegida por HTTP Basic Auth, mapeada via `vercel.json`). Arquivos de referência: `references/panel-auth/api/panel.js` e `references/panel-auth/vercel.json`.

**Toda vez que essa skill gerar um deck novo, adicione uma entrada no array `DECKS` dentro de `api/panel.js`** (não é mais um `decks.json` separado — os dados ficam embutidos na própria função, pra nunca ficarem públicos/acessíveis fora do login):
```js
{ client: "Nome do Cliente", slug: "nome-do-cliente", date: "AAAA-MM-DD", color: "#hexcode", status: "Sent" }
```
Entregue o `api/panel.js` inteiro atualizado (é só um arquivo, o usuário substitui o antigo). O campo `status` pode ser editado manualmente depois (Sent / Replied / Won / Pending).

Credenciais (`PANEL_USER` / `PANEL_PASS`) são configuradas uma única vez nas Environment Variables do projeto no site do Vercel (sem terminal) — não ficam no código.

## O que essa skill NÃO faz

- Não escreve a copy de vendas do produto do cliente (isso é outra skill/trabalho)
- Não tira os screenshots — o usuário sobe prontos, nomeados por bottleneck
- Não decide branding além da cor de destaque informada
