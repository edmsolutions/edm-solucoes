# EdM Soluções — Website institucional

Site institucional da **EdM Soluções**, estúdio de desenvolvimento web. Página única (one-page), estática (HTML + CSS + JS puros, sem build), com animações integradas como parte visual do site e efeito de rolagem estilo Apple nas seções de produto.

## Estrutura

```
.
├── index.html          → todo o site (HTML + CSS + JS em um único arquivo)
└── assets/
    ├── img/             → logo (mark + full), favicon e imagem de preview (og-cover)
    └── sprites/         → sprite sheets (.webp) usadas no scroll-scrubbing + manifest.json
```

Não há processo de build, framework ou dependências — é um site 100% estático. Qualquer servidor de arquivos estáticos serve.

### Como os vídeos funcionam

As 4 animações do site (MacBook, smartphone, dispositivos, crescimento) **não são arquivos de vídeo** — são sequências de 144 frames combinadas em uma única imagem "sprite sheet" (grade 12×12) por seção, desenhadas em um `<canvas>` via JavaScript. Isso permite:

- **Scroll-scrubbing instantâneo no desktop** (a posição da rolagem escolhe o frame exato, sem delay de seek de vídeo — é a mesma técnica usada em páginas de produto como a da Apple).
- **Loop ambiente leve no mobile e em preferência de "reduzir movimento"** (mostra uma animação simples ou um frame estático, respectivamente).
- **Consistência entre navegadores** — sem depender de decodificação de vídeo H.264, que se comporta de forma inconsistente entre Chrome/Safari/Firefox/iOS.

Para trocar alguma animação, gere uma nova sequência de frames PNG e recrie o sprite sheet correspondente em `assets/sprites/` (grade 12×12, mesma resolução por frame) — o mapeamento de cada sprite (dimensões, quantidade de frames) fica em `SPRITE_META` dentro do `<script>` no final do `index.html`.

## Rodar localmente

Como o site usa `fetch`/módulos relativos para os assets, abra por um servidor local (não é obrigatório, mas evita qualquer restrição de `file://` no navegador):

```bash
# Python
python3 -m http.server 8000

# Node
npx serve .
```

Depois acesse `http://localhost:8000`.

## Deploy

O projeto é estático, então funciona em qualquer plataforma de hospedagem estática, sem configuração de build. Alguns exemplos:

### Vercel
1. Importe o repositório em [vercel.com/new](https://vercel.com/new).
2. Framework preset: **Other** (ou "No framework").
3. Build command: (vazio) · Output directory: `.`
4. Deploy.

### Netlify
1. [app.netlify.com](https://app.netlify.com) → **Add new site → Import an existing project**.
2. Conecte o repositório.
3. Build command: (vazio) · Publish directory: `.`
4. Deploy.

### GitHub Pages
1. No repositório: **Settings → Pages**.
2. Source: **Deploy from a branch**.
3. Branch: `main` · Pasta: `/ (root)`.
4. Salvar — o GitHub gera a URL em alguns minutos.

### Cloudflare Pages
1. [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages → Create → Pages → Connect to Git**.
2. Build command: (vazio) · Build output directory: `/`.
3. Deploy.

## Domínio próprio

Depois do deploy, todas as plataformas acima permitem apontar um domínio próprio (ex: `edmsolucoes.com.br`) nas configurações do projeto, via CNAME/A record no seu provedor de DNS.

## Contato configurado no site

Os botões de CTA abrem o WhatsApp com mensagens pré-preenchidas para o número **+55 82 99615-3289**. Para trocar o número, busque por `PHONE` dentro do `<script>` no final do `index.html`.

---

© EdM Soluções. Todos os direitos reservados.
