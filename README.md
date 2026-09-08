# Protótipo Mobile — Organização dos Estilos com BEM

Protótipo de baixa fidelidade de uma aplicação mobile, construído em HTML e CSS
puro. O objetivo desta etapa não é implementar a aplicação, mas representar as
telas, identificar os elementos de interface recorrentes e organizar os estilos
com o padrão **BEM (Block, Element, Modifier)**, preparando a estrutura para a
futura componentização em React.

Os wireframes foram construídos diretamente em HTML/CSS, e não em ferramenta de
prototipação. Dessa forma as telas são navegáveis e a organização dos estilos já
corresponde aos componentes identificados. As 14 telas requisitadas ainda estão
em desenvolvimento. Até o momento, estamos em 4.

---

## Integrantes do grupo (vão ser adicionados mais depois)

| Nome | RA |
|---|---|
| Enzo Senatori | 10427175 |
|  |  |

---

## Descrição da aplicação

Aplicação de conteúdo com área pública e área de usuário. O fluxo público parte
da tela inicial, onde estão os destaques; a partir dela é possível ver a listagem
completa de destaques ou acessar a área de autenticação. O fluxo de autenticação
liga as telas de login e cadastro.

Fluxo de navegação implementado:

```
index.html  ──"Ver todos"──▶  destaques.html
    │                              │
    └────────"Sair"────────▶  login.html  ──"Criar conta"──▶  cadastro.html
                                   ▲                              │
                                   └──────"Já tenho conta"────────┘
```

A barra de navegação inferior (`nav`) está presente em todas as telas e permite
acesso direto a qualquer uma delas.

---

## Telas elaboradas

| Arquivo | Tela | Função |
|---|---|---|
| `index.html` | Início | Ponto de entrada. Apresenta o destaque principal e o acesso à listagem completa. |
| `destaques.html` | Destaques | Listagem dos conteúdos em destaque, organizada em grade de duas colunas. |
| `login.html` | Entrar | Autenticação do usuário. Campos de e-mail e senha. |
| `cadastro.html` | Criar conta | Cadastro de novo usuário. Dados pessoais, contato e senha. |

<!-- PREENCHER: as demais telas previstas, conforme definido pelo grupo -->

---

## Componentes identificados

| Componente | Onde aparece | Variações |
|---|---|---|
| `page` | todas as telas | `--row`, `--grid` |
| `button` | Início, Destaques, Entrar, Cadastro | `--primary`, `--secondary`, `--small` |
| `card` | Início, Destaques | `--featured` |
| `form` | Entrar, Cadastro | — |
| `nav` | todas as telas | `--active` (no elemento `nav__item`) |

### Detalhamento

**`page`** — define o enquadramento da tela (largura de 390px, centralizada) e a
organização das seções internas.

| Classe | Tipo | Descrição |
|---|---|---|
| `.page` | bloco | Quadro da tela. Um por página. |
| `.page__section` | elemento | Seção de conteúdo. Empilha os filhos em coluna. |
| `.page__section--row` | modificador | Dispõe os filhos em linha, com espaço entre eles. Usado no cabeçalho. |
| `.page__section--grid` | modificador | Dispõe os filhos em grade de duas colunas. Usado na listagem de destaques. |

**`button`** — ações e links de navegação. Os modificadores são independentes
entre si: cor e tamanho podem ser combinados na mesma tag.

| Classe | Tipo | Descrição |
|---|---|---|
| `.button` | bloco | Forma, altura mínima de toque e alinhamento. |
| `.button--primary` | modificador | Ação principal. Preenchido. |
| `.button--secondary` | modificador | Ação alternativa. Apenas contornado. |
| `.button--small` | modificador | Versão reduzida, para ações secundárias no cabeçalho. |

**`card`** — apresentação de um conteúdo na listagem.

| Classe | Tipo | Descrição |
|---|---|---|
| `.card` | bloco | Caixa com borda e espaçamento interno. |
| `.card__title` | elemento | Título do conteúdo. |
| `.card__text` | elemento | Texto de apoio (resumo, data). |
| `.card--featured` | modificador | Destaque, com borda acentuada. |

**`form`** — entrada de dados nas telas de autenticação.

| Classe | Tipo | Descrição |
|---|---|---|
| `.form` | bloco | Agrupa os campos do formulário. |
| `.form__field` | elemento | Par rótulo + campo. |
| `.form__label` | elemento | Rótulo do campo. |
| `.form__input` | elemento | Campo de entrada, com altura mínima de toque. |

**`nav`** — barra de navegação fixa no rodapé, presente em todas as telas.

| Classe | Tipo | Descrição |
|---|---|---|
| `.nav` | bloco | Barra fixa na base da tela. |
| `.nav__item` | elemento | Item de navegação. |
| `.nav__item--active` | modificador | Marca a tela atualmente aberta. |

---

## Organização dos arquivos

```
prototipo-mobile/
├── css/
│   ├── style.css     reset, tipografia e cores gerais (não contém componentes)
│   ├── page.css      bloco page
│   ├── button.css    bloco button
│   ├── card.css      bloco card
│   ├── form.css      bloco form
│   └── nav.css       bloco nav
├── index.html
├── destaques.html
├── login.html
├── cadastro.html
└── README.md
```

Cada bloco BEM tem o seu próprio arquivo CSS, nomeado igual ao bloco. O
`style.css` é a única exceção: reúne o reset do navegador e as definições que
valem para o documento inteiro, e por isso não usa BEM — o padrão se aplica a
componentes, não a regras globais.

A ordem de carregamento no `<head>` é sempre: `style.css` primeiro (base), depois
os blocos. Cada tela carrega apenas os blocos que utiliza.

---

## Decisões de organização

**Estilização por classe, não por tag.** Nenhum componente é estilizado pelo nome
da tag. Isso permite que `<a>` e `<button>` compartilhem a mesma aparência: os
links de navegação usam `.button` sem deixarem de ser links.

**Modificador carrega apenas a diferença.** `.button--primary` define somente
cor; forma, espaçamento e altura continuam vindo de `.button`. Por isso a classe
do bloco acompanha sempre a do modificador no HTML:
`class="button button--primary"`.

**Um modificador, um eixo de variação.** Cor e tamanho são modificadores
separados, o que permite combiná-los livremente
(`button button--secondary button--small`) sem multiplicar classes.

**Altura mínima de toque de 44px** em botões, campos e itens de navegação.

**`box-sizing: border-box`** aplicado globalmente, para que padding e borda sejam
descontados por dentro da largura declarada.

---

## Relação com a futura implementação em React

Cada bloco corresponde a um componente previsto, e cada modificador a uma
propriedade desse componente:

| Bloco CSS | Componente React | Propriedades previstas |
|---|---|---|
| `card` | `<Card />` | `title`, `text`, `featured` |
| `button` | `<Button />` | `variant` (primary/secondary), `size` |
| `form` | `<Form />` + `<Field />` | `label`, `type`, `placeholder` |
| `nav` | `<Nav />` | `activeItem` |
| `page` | `<Page />` + `<Section />` | `layout` (column/row/grid) |

A repetição atual do `<head>` e do `<nav>` em cada arquivo HTML é justamente o
que a componentização em React elimina.
