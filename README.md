# Controle de Estoque — Almoxarifado

App web para controle de estoque do almoxarifado de obras, com cadastro de obras e materiais, entradas (por nota fiscal, com vários itens), saídas por requisição, saldo com alerta de estoque mínimo e relatórios.

🔗 **App publicado:** https://devguilhermelima-dev.github.io/Estoque-obras/

## O que o app faz

- **Obras** — cadastro de cada canteiro/obra
- **Entradas** — lançamento de notas fiscais, com vários materiais na mesma nota, leitura automática (OCR gratuito) que tenta preencher NF, data, fornecedor e descrição do produto a partir de uma foto, e anexo da foto da nota
- **Saídas** — requisições de materiais por funcionário/setor
- **Saldo** — estoque atual por material, com alerta visual quando atinge o mínimo e botões de aviso rápido por WhatsApp/e-mail
- **Relatórios** — estoque atual, valor em estoque (calculado por custo real de cada lote/FIFO), materiais mais retirados, por funcionário e por fornecedor, com exportação em CSV e impressão
- **Usuários** — login por e-mail/senha (Supabase Auth), com dois papéis: **administrador** (gerencia obras, edita/remove materiais, gerencia usuários) e **usuário** (lança entradas e saídas, vê relatórios)

## Tecnologia

Um único arquivo `index.html` (HTML + CSS + JavaScript puro, sem build), publicado via **GitHub Pages**. Os dados ficam num banco **Supabase** (Postgres + Auth + Storage).

- `index.html` — o app inteiro
- `logo-turita.png` — logo da empresa, referenciada pelo app (mantida como arquivo separado pra deixar o `index.html` mais leve e o upload mais confiável)

## Como atualizar o app

1. Edite o `index.html` (ou peça pro Claude ajustar)
2. No GitHub, abra este repositório → **Add file → Upload files**
3. Selecione o novo `index.html` (mantendo o mesmo nome, pra substituir o antigo)
4. **Commit changes**
5. Espera 1–2 minutos e testa em uma aba anônima do navegador

> A logo (`logo-turita.png`) só precisa ser reenviada se ela mudar — o `index.html` referencia esse arquivo pelo nome, então ele precisa continuar no repositório com esse mesmo nome.

## Configuração do banco (Supabase)

O app se conecta a um projeto Supabase próprio. Se for necessário recriar o banco do zero:

1. Crie um projeto em [supabase.com](https://supabase.com)
2. No **SQL Editor**, rode o script de criação das tabelas (obras, materiais, entradas, saídas, perfis de usuário) e das políticas de segurança (RLS)
3. Em **Authentication → Providers → Email**, desligue "Confirm email" (pra login não exigir confirmação por link)
4. Em **Storage**, crie um bucket público chamado `notas-fiscais` (usado para guardar as fotos das notas fiscais anexadas nas entradas)
5. No `index.html`, atualize as constantes `SUPABASE_URL` e `SUPABASE_KEY` com os dados do novo projeto (em Project Settings → API)

**Importante:** o primeiro usuário a criar conta no app vira administrador automaticamente. Os demais entram como usuário comum, e podem ser promovidos depois pela aba Usuários.

## Estrutura de dados

- `obras` — canteiros de obra
- `materiais` — cadastro de materiais, vinculados a uma obra
- `entradas` — lançamentos de recebimento (cada linha é um material de uma nota fiscal)
- `saidas` — requisições/retiradas de material
- `profiles` — perfis de usuário (nome, papel: admin/usuario)

## Limitações conhecidas

- A leitura automática de notas fiscais (OCR) é gratuita e roda no navegador — funciona bem para NF, data, fornecedor e descrição do produto, mas costuma errar em quantidade e unidade de medida em tabelas mais complexas. Sempre revise antes de salvar.
- Não há controle de versão automático dos dados — exclusões são definitivas.
