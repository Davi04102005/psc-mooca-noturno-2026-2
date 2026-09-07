# Habit — Protótipo Mobile e Organização dos Estilos com BEM

Atividade da disciplina **Desenvolvimento Mobile** — etapa de organização visual
(wireframes mobile em baixa fidelidade + estrutura inicial de estilos CSS com
BEM) que dará suporte à futura implementação da aplicação em React.

> Entrega: 07/09/2026

## Integrantes

- [ ] Nome 1 — usuário GitHub
- [ ] Nome 2 — usuário GitHub

*(preencher antes da entrega — todos devem participar via commits no
repositório)*

## Sobre a aplicação

O **Habit** é um blog/portal de conteúdo com área pública (navegação por
categorias, destaques, busca, newsletter, login/cadastro, perfil de usuário) e
uma área administrativa (gestão de categorias, criação e revisão de
postagens, escolhas do editor, gestão de usuários e moderação de
comentários).

Esta etapa **não implementa a aplicação funcional**. O objetivo foi:

1. Redesenhar as 14 telas de referência para o contexto mobile (reorganização,
   não redução);
2. Identificar os elementos de interface recorrentes entre as telas;
3. Reconhecer componentes reutilizáveis e suas variações;
4. Organizar os estilos iniciais com BEM, preparando a futura
   componentização em React.

## As 14 telas

Wireframes em `/wireframes`. **Decisão registrada:** os arquivos foram
exportados em **SVG** (e não PNG) — SVG também é um formato de imagem,
válido conforme o enunciado, e tem a vantagem de permanecer nítido em
qualquer zoom e de ser visualizado diretamente pelo GitHub.

| # | Tela | Arquivo |
|---|---|---|
| 01 | Página inicial | `wireframes/tela_01.svg` |
| 02 | Categoria | `wireframes/tela_02.svg` |
| 03 | Destaques | `wireframes/tela_03.svg` |
| 04 | Newsletter | `wireframes/tela_04.svg` |
| 05 | Administração de categorias | `wireframes/tela_05.svg` |
| 06 | Criar postagem | `wireframes/tela_06.svg` |
| 07 | Escolhas do editor | `wireframes/tela_07.svg` |
| 08 | Gerenciamento de usuários | `wireframes/tela_08.svg` |
| 09 | Fila de revisão | `wireframes/tela_09.svg` |
| 10 | Fila de comentários | `wireframes/tela_10.svg` |
| 11 | Resultados de busca | `wireframes/tela_11.svg` |
| 12 | Login | `wireframes/tela_12.svg` |
| 13 | Criar conta | `wireframes/tela_13.svg` |
| 14 | Perfil | `wireframes/tela_14.svg` |

Cada wireframe traz pequenas anotações em itálico cinza ao lado dos blocos
(ex.: `.card--featured`, `.navigation--admin-tabs`) indicando a qual
componente/classe CSS aquele elemento corresponde — isso liga diretamente o
desenho da interface à organização de estilos descrita abaixo.

### Fluxos principais representados

- Página inicial → Categoria / Destaques / Newsletter
- Busca (header, em todas as telas) → Resultados de busca
- Login ↔ Criar conta → Perfil
- Criar postagem → Fila de revisão → aprovação/publicação
- Escolhas do editor (admin) → área "Escolhas do Editor" da página inicial

## Componentes identificados e variações

| Componente | Onde aparece | Variações |
|---|---|---|
| Header | todas as telas | pública / logada (`.header__avatar` vs `.header__login`) |
| Navigation | todas as telas | `--drawer` (menu público, telas 01–04 e 11–14) / `--admin-tabs` (telas 05–10) |
| Card | Home, Categoria, Destaques, Busca, Perfil | `--standard`, `--featured`, `--compact` |
| Button | quase todas | `--primary`, `--secondary`, `--disabled` |
| Form | Login, Criar conta, Newsletter, Criar post | mesmo bloco, campos diferentes por tela |
| List | Categorias, Usuários, Fila de revisão, Fila de comentários, Perfil | `--admin-row` (com status + ações) |
| Status | Perfil, Fila de revisão, Fila de comentários, Usuários | `--draft`, `--published`, `--pending`, `--active`, `--blocked` |
| Indicator | telas administrativas 05–10 | uso único, repetido 4x por tela |
| Search | header (global) + telas administrativas | `--admin` (borda menos arredondada) |
| Chip | Home (categorias), Categoria (filtros) | uso único |

## Organização dos arquivos

```
projeto-mobile/
├── wireframes/          # as 14 telas mobile em baixa fidelidade (SVG)
│   ├── tela_01.svg ... tela_14.svg
├── css/                 # um arquivo por responsabilidade/componente
│   ├── variables.css    # tokens de cor, espaçamento, tipografia, box-sizing global
│   ├── header.css
│   ├── navigation.css
│   ├── card.css
│   ├── button.css
│   ├── form.css
│   ├── list.css
│   ├── status.css
│   ├── indicator.css
│   └── search.css       # inclui .search e .chip
└── README.md
```

Cada arquivo CSS corresponde a um único componente (ou família de
componentes muito próxima, como `search` + `chip`), seguindo o princípio da
responsabilidade única (SRP) apresentado nas aulas: facilita achar, manter e
reaproveitar o estilo de cada peça quando a aplicação for implementada em
React (cada arquivo tende a virar o CSS de um componente `.tsx`).

## Decisões de adaptação para mobile

**Tela 01 — Página inicial:** o menu público (Início, Páginas, Destaques,
Assinar, Admin) saiu do header e virou um drawer acionado por ícone de menu.
"Categorias Populares" e "Todas as Categorias" viraram listas/chips
compactos; "Postagens em Destaque" e "Escolhas do Editor" (lado a lado no
desktop) foram empilhados verticalmente, destaque primeiro.

**Tela 02 e Tela 03 — Categoria / Destaques:** grid de 3 colunas de cards
virou sequência vertical de 1 coluna (`.card-grid` com
`grid-template-columns: 1fr`), preservando imagem, título e data de cada
post.

**Tela 04 — Newsletter:** já era essencialmente vertical; mantida a mesma
lógica, apenas com largura total do container mobile.

**Telas 05 a 10 — Área administrativa:** o padrão de referência (indicadores
em linha + menu lateral fixo + área principal em 2 colunas) foi o ponto mais
crítico da adaptação. Os indicadores viraram grade 2x2 (`.indicator-group`),
o menu lateral virou uma fila de abas roláveis horizontalmente
(`.navigation--admin-tabs`), e as tabelas (categorias, usuários, fila de
revisão, fila de comentários) foram reorganizadas de linhas de tabela para
uma lista de cartões empilhados (`.list__item--admin-row`), já que uma
tabela de 4 colunas não cabe em uma tela estreita sem gerar scroll
horizontal.

**Tela 11 — Resultados de busca:** lista já era vertical; cada resultado
passou a usar a variação compacta do card (imagem pequena + título + meta),
reaproveitando o mesmo componente usado no perfil.

**Tela 12 — Login:** formulário organizado verticalmente, com botões de
largura total para facilitar o toque.

**Tela 13 — Criar conta:** mesma lógica da tela de login — campos
empilhados, largura total, alvo de toque confortável.

**Tela 14 — Perfil:** o layout de referência era 2 colunas (dados do perfil
+ postagens/comentários); em mobile os dados do perfil vêm primeiro,
seguidos pela lista de postagens/comentários, cada item já usando o card
compacto com o badge de `.status` correspondente ao estado do conteúdo.

## Relação com a futura implementação em React

A organização acima foi pensada para que cada bloco BEM (`card`, `button`,
`form`, `list`, `status`, `navigation`, `indicator`, `search`) possa virar um
componente React independente, recebendo o modifier como uma prop de
variante (ex.: `<Card variant="featured" />`), sem exigir nenhuma
reestruturação dos estilos já escritos aqui.
