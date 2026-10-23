# AGENTS.md — Memória do projeto ShareCar

> **Leia este arquivo antes de retomar o trabalho.** Ele resume o contexto, as decisões e o estado atual para não ser necessário reler os documentos originais.
> Última atualização: 21/10/2026.

---

## 1. Contexto

Projeto acadêmico em dupla: **ShareCar**, um marketplace **peer-to-peer de aluguel de carros**, entregue como **PWA em Django**.

- **Proprietário** cadastra o carro, define preço e disponibilidade, aceita/recusa reservas e recebe repasses.
- **Locatário** busca, reserva, paga (simulado), faz check-in/check-out e avalia.
- **Administrador** aprova documentos e veículos, configura a comissão e bloqueia contas.
- A plataforma retém **15%** sobre as diárias (configurável).
- Interface em **português do Brasil**, valores em **R$** e datas no padrão brasileiro.

**Documento-fonte (não é preciso reler):** `C:\Users\00022593\Desktop\projetoShareCar.txt` (198 linhas, orientações completas do professor).

**Resumos já produzidos:**
- `projeto-sharecar.md` — visão completa: escopo, RF01–RF29, RNF01–RNF13, RN01–RN10, stack e marcos.
- `10-telas-mvp.md` — descrição textual das 10 primeiras telas.
- `prototipo/README.md` — instruções do protótipo.

---

## 2. Estado atual (concluído)

| Etapa | Situação |
|---|---|
| Leitura e entendimento do documento-fonte | ✅ |
| Resumo estruturado em Markdown | ✅ `projeto-sharecar.md` |
| Definição descritiva das 10 primeiras telas | ✅ `10-telas-mvp.md` |
| Protótipo visual em HTML/CSS das 10 telas | ✅ `prototipo/` |
| Validação de renderização (Chrome headless) | ✅ |
| Repositório Git inicializado + `.gitignore` (commit `e2d6fee`) | ✅ 21/10/2026 |
| Remoto no GitHub `Luana002/ShareCar` (público, branch `main`) | ✅ 21/10/2026 |
| README raiz | ❌ ainda não iniciado |
| Projeto Django / PostgreSQL / API | ❌ ainda não iniciado |

**Ainda não existe código de backend.** O protótipo é somente visual, com dados fictícios.
**PostgreSQL ficou adiado por decisão da dupla:** por enquanto o projeto não usa banco; a configuração do Postgres entra quando o Django for criado.

---

## 3. Decisões tomadas

### 3.1 Fluxo da reserva (adotado no protótipo)

```
solicitada → paga (aguardando aceite) → aceita/confirmada
→ em andamento (check-in) → concluída (check-out) → avaliada
```

Desvios: `recusada` (gera estorno), `cancelada`, `estornada`.

> O pagamento acontece **antes** do aceite do proprietário. Se ele recusar, o valor é estornado. Essa ordem está refletida nas telas 05 → 06 → 07.

### 3.2 Personas usadas no protótipo

- **Marina Souza** — usuária logada; é locatária **e** proprietária (demonstra o papel duplo).
- **Rafael Costa** — proprietário do Fiat Argo alugado por Marina.
- Locatários da frota da Marina: João Lima, Carla Dias, Pedro Alves, Ana Beatriz.

### 3.3 Dados de referência (mantêm coerência entre as telas)

- Veículo: **Fiat Argo Drive 2021**, hatch, manual, flex, 5 lugares, Vila Madalena (São Paulo).
- Período: **12–16 nov 2026**, 4 diárias, retirada 09:00 e devolução 18:00.
- Diária: **R$ 150** → total **R$ 600**.
- Comissão 15%: **R$ 90**; repasse: **R$ 510** (bate com a RN07).
- Check-in: 42.180 km e 7/8 de combustível.

### 3.4 Padrões visuais do protótipo

- Mobile-first, largura máxima de **440 px**, moldura de celular no desktop.
- Barra inferior fixa: **Início · Buscar · Reservas · Perfil**.
- Cor da marca: **verde-teal** (`#0d9488` / `#0f766e`), fundo claro e cartões arredondados.
- CSS próprio (`prototipo/css/style.css`), **sem Bootstrap CSS**; apenas **Bootstrap Icons** e fonte **Inter** via CDN.
- Fotos de carro são placeholders em SVG (`prototipo/assets/car.svg`), com variações de cor.
- O frontend de produção deve usar **Bootstrap 5**, conforme a stack obrigatória; o protótipo é apenas referência visual.

### 3.5 Ajustes já feitos no protótipo

- Classes de status (`.is-pending`, `.is-ok`, etc.) foram escopadas em `.badge-status` para não vazarem para a timeline.
- Grids de formulário usam `minmax(0, 1fr)` + `min-width: 0` para os inputs não estourarem.
- Moldura do celular usa `overflow: clip` no desktop para arredondar as bordas sem quebrar o `position: sticky`.
- Screenshots com Chrome headless precisam de `--virtual-time-budget=8000` para esperar a fonte de ícones carregar.

### 3.6 Versionamento (definido em 21/10/2026)

- Repositório inicializado na própria pasta `C:\Users\00022593\projeto`, branch **`main`**.
- Identidade Git configurada **apenas no repositório** (não na máquina): `Luana002 / correaluana623@gmail.com`.
- `core.autocrlf=true`: o histórico guarda **LF** e a pasta de trabalho recebe **CRLF** no Windows. Por isso **não** há `.gitattributes` de normalização.
- `.gitignore` cobre Python/Django, `.env` (com exceção de `.env.example`), mídia, `staticfiles`, caches de teste, editores e arquivos de SO.
- Arquivos versionados no primeiro commit: `AGENTS.md`, `projeto-sharecar.md`, `10-telas-mvp.md`, `.gitignore` e todo o `prototipo/`.
- Remoto `origin`: **https://github.com/Luana002/ShareCar.git** (público), com `main` rastreando `origin/main`.
- O remoto foi criado **vazio** (sem README gerado pelo GitHub), então o primeiro push não exigiu `pull --rebase`.
- O `README.md` raiz continua pendente: é o próximo item do Marco 1.

---

## 4. Estrutura de arquivos

```
C:\Users\00022593\projeto\
├── .git\                      # repositório local (branch master)
├── .gitignore                 # Python/Django, .env, mídia, editores, SO
├── AGENTS.md                  # este arquivo (memória do projeto)
├── projeto-sharecar.md        # resumo completo do documento-fonte
├── 10-telas-mvp.md            # descrição das 10 telas
└── prototipo\
    ├── README.md
    ├── index.html             # galeria das 10 telas
    ├── 01-entrar.html
    ├── 02-home.html
    ├── 03-resultados.html
    ├── 04-detalhes.html
    ├── 05-resumo.html
    ├── 06-pagamento.html
    ├── 07-reservas.html
    ├── 08-checkin.html
    ├── 09-veiculo.html
    ├── 10-avaliacao.html
    ├── css\style.css
    └── assets\car.svg
```

Fluxo clicável: `01 → 02 → 03 → 04 → 05 → 06 → 07 → 08 → 10`. O cadastro de veículo (`09`) é acessado pelo atalho "Tem um carro parado?" na home.

---

## 5. Stack obrigatória

| Camada | Tecnologia |
|---|---|
| Linguagem | Python 3 |
| Framework | Django |
| API | djangorestframework |
| Banco | PostgreSQL (nunca SQLite na entrega) |
| Driver | psycopg (v3) |
| Imagens | Pillow |
| Configuração | django-environ ou python-decouple |
| Frontend | HTML, CSS, Bootstrap 5, JavaScript (Fetch API) |
| PWA | manifest.json, Service Worker, Cache API |
| Versionamento | Git + GitHub |

**Recomendados:** django-filter, drf-spectacular (Swagger), simplejwt, pytest-django, whitenoise, gunicorn, Docker + Docker Compose.

Fixar versões no `requirements.txt`; variáveis sensíveis em `.env` com `.env.example`.

---

## 6. Regras de negócio que serão cobradas

| Código | Regra |
|---|---|
| RN01 | Sem reservas ativas sobrepostas para o mesmo veículo. |
| RN02 | Com acessos simultâneos, apenas uma reserva pode vencer (testar concorrência). |
| RN03 | Reserva apenas em período disponível definido pelo proprietário. |
| RN04 | CNH vencida ou não aprovada não pode reservar. |
| RN05 | Proprietário não pode alugar o próprio veículo. |
| RN06 | Comissão padrão de 15%, configurável pelo administrador. |
| RN07 | Referência: 4 × R$ 150 = R$ 600; comissão R$ 90; repasse R$ 510. |
| RN08 | Ciclo de vida com status definidos; transições inválidas bloqueadas. |
| RN09 | Avaliação só após o check-out, uma por parte e por reserva. |
| RN10 | KM da devolução ≥ KM da retirada. |

Requisitos não funcionais críticos: PWA instalável + offline (RNF01–02), privacidade de documentos/CNH (RNF05), uploads validados (RNF07), busca paginada (RNF08), API REST documentada (RNF09), testes das regras críticas (RNF10), README reprodutível (RNF12).

---

## 7. Marcos de entrega

1. **Planejamento** — repositório, README inicial, divisão de tarefas, prompt v1 e wireframes.
2. **Base do sistema** — Django + PostgreSQL + `.env`; contas/perfis (RF01–04); veículos (RF05–08).
3. **Busca e reservas** — busca/filtros (RF09–12); reservas e pagamento (RF13–20); testes de concorrência (RN01–03).
4. **Locação e PWA** — check-in/out e avaliações (RF21–25); administração (RF26–29); PWA + offline.
5. **Entrega final** — sistema completo, README com dados de exemplo, documentação da API, histórico de prompts e demonstração ao vivo.

---

## 8. Próximos passos sugeridos

1. `README.md` raiz com descrição, stack, como rodar o protótipo e divisão de tarefas (Marco 1) — o `git init` já foi feito.
2. Criar `docs/prompts/` e salvar o primeiro prompt com nota do que mudou.
3. Estrutura Django + Docker Compose + `.env.example` + PostgreSQL (o Postgres está adiado até aqui).
4. Modelagem de usuários/perfis (usuário pode ser proprietário e locatário).
5. Implementar veículos e disponibilidade; depois busca, reservas e pagamento.
6. Testes das regras RN01–RN10 desde o início.
7. Manter `origin/main` atualizado (`git push`) ao fim de cada marco.

---

## 9. Convenções de trabalho

- Responder e escrever código/documentação em **português do Brasil**.
- Datas `dd/mm/aaaa` ou `12–16 nov`; valores `R$ 1.234,56`.
- Não commitar segredos; usar `.env`.
- Toda decisão nova relevante deve ser registrada neste `AGENTS.md` ou em `docs/`.
- Revisar e testar todo código gerado por IA; a dupla deve conseguir explicar qualquer trecho.
