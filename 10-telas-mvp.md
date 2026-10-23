# As 10 primeiras telas do projeto ShareCar

Sugestão descritiva das telas iniciais do MVP, pensada para orientar os wireframes e a divisão do trabalho.

**Lógica de navegação:** interface **mobile-first**, com barra inferior fixa (`Início · Buscar · Reservas · Perfil`) e, no desktop, cabeçalho com logo, busca e menu do usuário.

**Ciclo da reserva (RN08):** `solicitada` → `aceita (aguardando pagamento)` → `confirmada` → `em andamento (check-in)` → `concluída (check-out)` → `avaliada`, com os desvios `recusada`, `cancelada` e `estornada`.

---

## Tela 1 — Entrar / Criar conta

É a porta de entrada. A tela se divide em duas abas: **Entrar** (e-mail e senha, link "Esqueci minha senha", botão principal) e **Criar conta** (nome completo, e-mail, telefone, senha com confirmação e aceite dos termos). O cadastro é único — não se escolhe "sou locatário" ou "sou proprietário" no início, porque o mesmo usuário pode ser os dois (RF01). Logo abaixo do formulário, um bloco discreto explica o que falta para começar a usar: para **anunciar**, é preciso enviar documento de identidade (RF03); para **reservar**, é preciso informar a CNH e a validade (RF04). Um selo de status ("Documento em análise", "CNH válida até 12/2027") acompanha o usuário depois do login. A tela também pode exibir um banner sugerindo instalar o app na tela inicial (RNF01), escondido quando já estiver rodando em modo standalone.

## Tela 2 — Home / Busca

A vitrine. No topo, um bloco de busca com três campos — **cidade** (com autocomplete), **retirada** e **devolução** (data e hora) — e o botão "Buscar". Abaixo, atalhos por categoria (hatch, sedã, SUV, picape) e uma faixa de veículos em destaque na cidade do usuário, cada um com foto, modelo, nota média e preço da diária. Fecha com um "Como funciona" em três passos (busque, reserve, retire) para reduzir a desconfiança típica de marketplace. A validação básica já começa aqui: a devolução precisa ser posterior à retirada, e o formulário não avança sem cidade e período (RF09). O cabeçalho mostra o avatar do usuário e, se ele já tiver veículos, o atalho "Meus veículos".

## Tela 3 — Resultados da busca

Lista paginada de veículos (RNF08), com um resumo do período pesquisado fixo no topo ("São Paulo · 12–15 out · 3 diárias") e a opção de editar a busca. Cada cartão traz foto, modelo e ano, bairro/cidade, nota média, ícones de câmbio, combustível e passageiros, e o preço da diária em destaque. Os filtros — faixa de preço, categoria, câmbio, combustível, número de passageiros e nota mínima (RF10) — abrem em uma gaveta inferior no celular e em coluna lateral no desktop. Estados importantes: *carregando* com esqueletos, *vazio* com sugestão de ampliar o período ou remover filtros, e a garantia de que só aparecem veículos **aprovados pelo administrador** e **livres no período** (RF08, RF11).

## Tela 4 — Detalhes do veículo

A tela que convence. Galeria de fotos com rolagem horizontal e miniaturas, título com modelo/ano e nota média, localização aproximada (cidade e bairro — sem mapa, que está fora do escopo). Em seguida, a ficha técnica (categoria, câmbio, combustível, passageiros, portas, ar-condicionado) e o card do proprietário com nome, nota, tempo de resposta e selo de verificação. Abaixo vêm a descrição do carro, as regras da locação (política de combustível e limpeza), um calendário de disponibilidade com os períodos bloqueados (RF07) e a lista de avaliações com distribuição de notas (RF12, RF25). A barra fixa no rodapé mostra "R$ 150/dia · total estimado" e o botão **Reservar**, que fica desabilitado com explicação quando o usuário é o próprio dono do carro (RN05) ou está com a CNH vencida/não aprovada (RN04).

## Tela 5 — Resumo da reserva

O momento de transparência antes do compromisso. Mostra o carro escolhido, o período com data/hora de retirada e devolução, a quebra de valores — **quantidade de diárias × valor da diária**, taxas e **total** (RF14) — e um resumo da política de cancelamento. A referência de cálculo deve ser visível e correta: 4 diárias de R$ 150 = R$ 600. Um aviso deixa claro que a solicitação ainda depende do aceite do proprietário. O botão principal leva ao pagamento; o secundário volta aos detalhes. Aqui também se aplicam as validações de sobreposição e disponibilidade no momento da confirmação, não só na busca (RN01, RN03).

## Tela 6 — Pagamento simulado

Tela de checkout com duas opções: **Pix** (QR code fictício e código copia-e-cola) e **Cartão** (formulário simulado com número, validade e CVV). O resumo de valores permanece visível para conferência. Como o pagamento é simulado, a tela oferece um seletor de cenário — **aprovado**, **recusado** e **estornado** (RF18, RF20) — útil tanto para testes quanto para a demonstração. Cada cenário tem um estado próprio: processando (spinner), aprovado (confirmação e novo status da reserva), recusado (mensagem clara e botão "tentar novamente") e estornado (com valor e prazo fictício). Internamente, o registro guarda valor bruto, comissão e repasse (RF19).

## Tela 7 — Minhas reservas

Central de acompanhamento. Duas abas — **Como locatário** e **Como proprietário** — e filtros por status (pendentes, confirmadas, em andamento, concluídas, canceladas). Cada cartão mostra foto do carro, contraparte, período, total e um selo colorido de status. Na aba de proprietário, reservas `solicitadas` exibem os botões **Aceitar** e **Recusar** (RF15); na de locatário, aparece o atalho para pagar quando a reserva foi aceita. Reservas ativas permitem cancelamento conforme a regra definida pela dupla (RF16). É a tela que materializa o histórico e o status de cada parte (RF17) e onde transições inválidas precisam ser bloqueadas na interface (RN08).

## Tela 8 — Detalhe da reserva (com Check-in e Check-out)

A tela operacional da locação. No topo, uma **linha do tempo** com o status atual e o que já aconteceu; abaixo, os dados do carro, do período e do pagamento. Quando chega a hora da retirada, surge o formulário de **check-in**: fotos obrigatórias (frente, traseira, laterais, interior e painel), quilometragem e nível de combustível, com captura direta pela câmera e validação de tipo e tamanho (RF21, RNF07). Na devolução, o mesmo formulário de **check-out** (RF22), com a quilometragem inicial como piso — o sistema impede valor menor (RN10). Depois de ambos, a tela passa a exibir os dois registros **lado a lado**, destacando a diferença de quilometragem e de combustível (RF23).

## Tela 9 — Cadastro de veículo (proprietário)

Onde a oferta nasce. Formulário em etapas para não assustar: **dados** (marca, modelo, ano, categoria, câmbio, combustível, passageiros, portas), **preço e localização** (valor da diária, cidade e bairro), **fotos** (upload múltiplo com arrastar para reordenar e uma foto de capa) e **disponibilidade** (calendário para marcar períodos livres e bloqueios, RF07). Cada foto é validada por tipo e tamanho (RNF07). Ao salvar, o veículo entra como **em análise** e não aparece na busca até a aprovação do administrador (RF08); a tela deixa isso explícito com um selo de status e a possibilidade de editar, desativar ou reenviar (RF05, RF06).

## Tela 10 — Avaliação

O fechamento do ciclo. Liberada apenas **após o check-out** e **uma vez por parte** (RN09), a tela pede uma nota de 1 a 5 estrelas, etiquetas rápidas (limpeza, comunicação, pontualidade, estado do carro) e um comentário opcional. O locatário avalia o carro/proprietário; o proprietário avalia o locatário. Depois do envio, a tela confirma o registro e mostra a nova média — que passa a aparecer no perfil do usuário e na página do veículo (RF24, RF25). Um estado "aguardando a outra parte" mantém a avaliação editável até o envio ou até o prazo definido.

---

## Segunda leva (já implementada no protótipo)

Com essas 10 telas o fluxo da demonstração final já fecha: login → busca → detalhes → reserva → pagamento → aceite do proprietário → check-in → check-out → avaliação, além do cadastro do carro.

As telas seguintes foram acrescentadas ao protótipo como **11–15**:

| # | Tela | Cobre |
|---|---|---|
| 11 | Perfil e verificação de documentos | RF03, RF04, RNF05 — selos de documento e CNH, notas, atalhos da conta e saída |
| 12 | Painel do proprietário | RF05, RF19 — faturamento, repasses com comissão de 15%, gráfico e status da frota |
| 13 | Área administrativa | RF26–RF29 — aprovações, comissão configurável, reservas/pagamentos e bloqueios |
| 14 | Recuperação de senha | RF02 — envio de link, prazo de expiração e orientações |
| 15 | Estado offline | RNF01, RNF02 — telas em cache e fila de alterações pendentes do PWA |

Todas as 15 telas existem em **duas versões**: mobile (`prototipo/`) e desktop (`prototipo/desktop/`). A descrição acima continua sendo a referência de conteúdo; a versão desktop apenas reorganiza o mesmo conteúdo em layout amplo (cabeçalho fixo, filtros laterais, tabelas e resumos fixos).
