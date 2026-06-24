# Orçamento & Contrato · Festa 15 Anos

Sistema de orçamento online + contrato digital com aceite eletrônico para serviços de fotografia e filmagem.

## Arquivos

- **`orcamento-15anos.html`** — Página principal do orçamento. Formulário de aprovação que salva no Supabase.
- **`contrato-15anos.html`** — Página do contrato. Lê o registro do cliente via `?id=XX` e registra o aceite eletrônico (IP + data/hora) no Supabase.
- **`_redirects`** — Configuração do Netlify: raiz `/` → abre o orçamento.

## Stack

- **Front-end:** HTML + CSS + JavaScript puro (sem build step)
- **Back-end / DB:** Supabase (PostgreSQL + Row Level Security)
- **Email transacional:** EmailJS (template único enviado para cliente e fotógrafo)

## Configuração

### Supabase
As credenciais estão hardcoded nos 2 HTMLs (chave `anon` é publishable por design):
```
const SUPABASE_URL  = 'https://phwbljrusnrvhrdrxqso.supabase.co';
const SUPABASE_ANON = 'sb_publishable_...';
```

A tabela esperada é `public.clientes` com os campos usados no payload do INSERT.

### EmailJS
Preencher as 3 constantes em `contrato-15anos.html`:
```js
const EMAILJS_PUBLIC_KEY  = '...';
const EMAILJS_SERVICE_ID  = 'service_xxx';
const EMAILJS_TEMPLATE_ID = 'template_xxx';
const PHOTOGRAPHER_EMAIL  = 'seu@email.com';
```

## Deploy (Netlify)

1. https://app.netlify.com/drop
2. Arrastar a pasta `orcamento-15anos/` pra janela
3. URL gerada em ~30s

## Fluxo

```
Cliente preenche orçamento
        ↓
INSERT no Supabase (status='novo')
        ↓
Botão "Ir para o contrato" → contrato-15anos.html?id=X
        ↓
Cliente lê o contrato e marca checkbox
        ↓
UPDATE no Supabase (status='aprovado', aceite_em, aceite_ip, contrato_numero)
        ↓
EmailJS dispara 2 emails (cliente + fotógrafo)
```

## Contato

Pauleanderson Souza · CNPJ 20.805.712/0001-95
pauleandersongomes@gmail.com · (92) 99241-1099