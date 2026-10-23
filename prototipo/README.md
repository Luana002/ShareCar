# ShareCar · Protótipo visual

Telas estáticas do MVP de aluguel de carros entre pessoas (P2P). **Somente visual**: não há backend, banco de dados ou regras implementadas. Os dados são fictícios.

O protótipo tem **duas versões da mesma navegação**:

| Versão | Pasta | Para que serve |
|---|---|---|
| Mobile | `prototipo/` | Demonstra o app como PWA, com moldura de celular de 440 px. |
| Desktop | `prototipo/desktop/` | Demonstra a mesma navegação em telas largas, com cabeçalho fixo, filtros laterais, tabelas e resumos fixos. |

## Como abrir

Dê dois cliques em um dos índices ou abra no navegador:

```
prototipo/index.html            # galeria da versão mobile
prototipo/desktop/index.html    # galeria da versão desktop
```

Não precisa de servidor. A fonte Inter e os ícones do Bootstrap Icons vêm por CDN, então é necessário estar conectado à internet para vê-los. Sem conexão, o layout continua funcionando, mas sem os ícones.

## Telas (15 em cada versão)

| # | Arquivo | Tela |
|---|---------|------|
| 01 | `01-entrar.html` | Entrar / Criar conta |
| 02 | `02-home.html` | Home / Busca |
| 03 | `03-resultados.html` | Resultados da busca |
| 04 | `04-detalhes.html` | Detalhes do veículo |
| 05 | `05-resumo.html` | Resumo da reserva |
| 06 | `06-pagamento.html` | Pagamento simulado |
| 07 | `07-reservas.html` | Minhas reservas |
| 08 | `08-checkin.html` | Detalhe da reserva + check-in/out |
| 09 | `09-veiculo.html` | Cadastro de veículo |
| 10 | `10-avaliacao.html` | Avaliação |
| 11 | `11-perfil.html` | Perfil e verificação de documentos |
| 12 | `12-painel.html` | Painel do proprietário (frota e repasses) |
| 13 | `13-admin.html` | Área administrativa |
| 14 | `14-recuperar-senha.html` | Recuperação de senha |
| 15 | `15-offline.html` | Estado offline do PWA |

## Fluxo clicável

`01 → 02 → 03 → 04 → 05 → 06 → 07 → 08 → 10`

Atalhos paralelos:

- `09` (cadastro de veículo) pela home, pelo painel do proprietário e pelo perfil.
- `11` (perfil) pela aba **Perfil** e pelo avatar do cabeçalho.
- `12` (painel) pelo perfil e pelo menu do cabeçalho.
- `13` (administração) pelo perfil, para simular o papel de administrador.
- `14` (recuperar senha) pelo link "Esqueci minha senha" nas telas de login.
- `15` (offline) é um estado do PWA, sem entrada principal no fluxo.

## Diferenças entre mobile e desktop

| Mobile | Desktop |
|---|---|
| Moldura de celular de 440 px | Layout fluido até 1240 px de conteúdo |
| Barra inferior fixa (Início · Buscar · Reservas · Perfil) | Cabeçalho fixo com navegação, busca e avatar |
| Filtros em gaveta inferior | Filtros em coluna lateral fixa |
| Cartões empilhados | Grades de 3–4 colunas e tabelas |
| Ação principal fixa no rodapé (`sticky-cta`) | Cartão de resumo fixo na coluna direita |
| Perfil em página única | Menu lateral de conta |

## Padrões visuais

- **Mobile-first** na versão mobile; a versão desktop reutiliza o mesmo CSS base e adiciona `desktop/css/desktop.css`.
- Cabeçalho/barras usam fundo translúcido com desfoque; cartões claros, cantos arredondados e sombras suaves.
- Verde-teal como cor da marca; estados com amarelo (pendente), azul (informativo), verde (ok) e vermelho (risco).
- Fotos de carros são placeholders em SVG (`assets/car.svg`), com variações de cor.
- Datas fictícias em novembro de 2026.
- Exemplo financeiro alinhado à RN07: 4 diárias × R$ 150 = R$ 600; comissão de 15% (R$ 90); repasse de R$ 510.

## Estrutura

```
prototipo/
├── index.html              # galeria das 15 telas (mobile)
├── 01-entrar.html          # ... 15 telas mobile
├── css/style.css           # estilos compartilhados + componentes das telas 11–15
├── assets/car.svg          # ilustração usada nos placeholders
└── desktop/
    ├── index.html          # galeria das 15 telas (desktop)
    ├── 01-entrar.html      # ... 15 telas desktop
    └── css/desktop.css     # camada de layout amplo sobre ../css/style.css
```

As páginas do desktop carregam `../css/style.css` **e** `css/desktop.css`: os tokens, botões, cartões e selos são os mesmos do mobile, e só o esqueleto de página muda.

## O que ainda não existe

Backend Django, API REST, PostgreSQL, autenticação real, upload real de imagens, PWA de verdade (manifesto e service worker funcionais) e testes. Esta etapa serve apenas para validar aparência e navegação antes do desenvolvimento. A tela `15-offline.html` é apenas a representação visual desse estado.
