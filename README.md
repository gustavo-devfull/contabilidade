# Patrimônio & Dívidas

Dashboard editável (HTML/CSS/JS puro, sem dependências) convertido a partir da
planilha "Patrimônio e dívidas" (abas *Patrimônio* e *Dívidas*).

Página estática única (`index.html`), sem build nem dependências — pode ser
aberta direto no navegador ou publicada como está em qualquer host estático
(Vercel, GitHub Pages, Netlify etc.). Abas:

- **Início** — saldo mais recente por instituição.
- **Patrimônio** — evolução dos investimentos, composição por instituição,
  imóveis e o detalhamento por categoria/data (tudo editável).
- **Dívidas** — parcelas do cartão e despesas fixas mensais (também editável).

Os valores iniciais foram extraídos diretamente da planilha original.

## Deploy no Vercel

Não há build step: o Vercel só precisa servir `index.html` como site estático.
Basta importar este repositório em vercel.com/new (framework preset "Other")
ou rodar `vercel --prod` na raiz do projeto.
