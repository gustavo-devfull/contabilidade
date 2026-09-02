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

Os valores iniciais foram extraídos diretamente da planilha original — eles
só são usados para popular a conta de um usuário na primeira vez que ele
entra (ver "Login e dados" abaixo).

## Deploy no Vercel

Não há build step: o Vercel serve `index.html` como site estático e
`api/config.js` como função serverless. Basta importar este repositório em
vercel.com/new (framework preset "Other") ou rodar `vercel --prod` na raiz
do projeto.

## Login e dados (Firebase)

A tela de login usa **Firebase Authentication** (e-mail/senha) e os dados de
cada conta são salvos no **Cloud Firestore**, no documento
`dashboards/{uid}` do usuário logado. `api/config.js` lê as credenciais do
projeto Firebase das variáveis de ambiente do Vercel (nunca ficam no
repositório) e as expõe ao navegador em `/api/config`.

### 1. Variáveis de ambiente no Vercel

Em **Project Settings → Environment Variables** do projeto `contabilidade`,
adicione (Production + Preview):

| Nome | Valor |
|---|---|
| `FIREBASE_API_KEY` | `AIzaSyCV6o0z8Fc5LjBZawNvqkHJnoUJzZ4avXo` |
| `FIREBASE_AUTH_DOMAIN` | `contabilidade-g.firebaseapp.com` |
| `FIREBASE_PROJECT_ID` | `contabilidade-g` |
| `FIREBASE_STORAGE_BUCKET` | `contabilidade-g.firebasestorage.app` |
| `FIREBASE_MESSAGING_SENDER_ID` | `561916391206` |
| `FIREBASE_APP_ID` | `1:561916391206:web:042ab85772bfa49b609be7` |

(Essas chaves do SDK Web do Firebase não são secretas — a segurança vem das
regras do Firestore abaixo — mas ficam em env vars para não hardcodear
config específica de ambiente no código.) Depois de salvar, faça um
redeploy para elas entrarem em vigor.

### 2. No console do Firebase (console.firebase.google.com → projeto `contabilidade-g`)

1. **Authentication → Sign-in method** → ative **E-mail/senha**.
2. **Firestore Database** → crie o banco (modo produção).
3. **Firestore → Regras**, use:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /dashboards/{uid} {
         allow read, write: if request.auth != null && request.auth.uid == uid;
       }
     }
   }
   ```

   Isso garante que cada usuário só lê/escreve o próprio documento.

Depois disso, qualquer pessoa pode criar conta pela própria tela de login;
os dados de exemplo da planilha só aparecem na primeira vez que uma conta
nova entra (ela pode editar tudo livremente a partir daí).
