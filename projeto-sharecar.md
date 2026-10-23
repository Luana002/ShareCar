# Aluguel de Carro — Orientações do Projeto

> Documento-base do projeto **ShareCar**, um marketplace peer-to-peer de aluguel de veículos.
> Gerado a partir da leitura de `projetoShareCar.txt`.

---

## 1. Visão geral

A equipe vai desenvolver o **Aluguel de Carro**, um **PWA em Django** que funciona como marketplace de aluguel de carros entre pessoas (modelo **peer-to-peer**, semelhante ao Airbnb, mas para veículos).

- O **proprietário** cadastra seu carro, define preço e disponibilidade.
- O **locatário** busca, reserva, paga e devolve.
- A plataforma **intermedeia** a negociação e **retém uma comissão**.

> **Estudo prévio obrigatório:** antes de começar, estudar como funcionam plataformas reais do setor — **Turo, Getaround, MyCyC e PegaCar**. Entender o problema faz parte do trabalho.

---

## 2. Escopo

O escopo obrigatório é o **MVP completo**; as evoluções opcionais contam como **diferencial**.

### 2.1 Obrigatório (MVP)

1. Cadastro, login e perfis de usuário
2. Cadastro de veículos com fotos, preço e disponibilidade
3. Busca e filtros
4. Reserva com cálculo de valores
5. Pagamento simulado
6. Check-in e check-out com fotos
7. Avaliações entre as partes
8. Área administrativa
9. Funcionamento como PWA instalável

### 2.2 Opcional (diferencial)

- Painel do proprietário com indicadores (reservas, receita, avaliação média)
- Painel administrativo com indicadores gerais
- Notificações (e-mail ou push)
- Contrato de locação gerado ao confirmar a reserva
- Aluguel por horas, além de diárias
- Política de cancelamento com reembolso parcial

### 2.3 Fora do escopo

- GPS, mapas e rastreamento do veículo
- Integração com gateway de pagamento real
- Abertura do veículo pelo celular, IoT ou telemetria
- Validação real de documentos junto a órgãos oficiais

---

## 3. Perfis de usuário

O sistema tem **três perfis**. Um mesmo usuário pode ser **proprietário e locatário ao mesmo tempo** — a dupla deve decidir como modelar isso.

| Perfil | Papel |
|---|---|
| **Proprietário** | Oferece veículos, define preço e agenda, aceita ou recusa reservas, recebe repasses |
| **Locatário** | Busca veículos, reserva, paga, faz check-in e check-out, avalia |
| **Administrador** | Aprova usuários e veículos, acompanha reservas e pagamentos, configura a comissão, bloqueia contas |

---

## 4. Requisitos funcionais

### 4.1 Contas e acesso

| Código | Requisito |
|---|---|
| RF01 | O usuário deve se cadastrar, entrar e sair do sistema. |
| RF02 | O usuário deve poder recuperar a senha. |
| RF03 | O proprietário deve enviar documento de identidade para aprovação. |
| RF04 | O locatário deve informar os dados da CNH, incluindo a validade. |

### 4.2 Veículos

| Código | Requisito |
|---|---|
| RF05 | O proprietário deve cadastrar, editar e desativar seus veículos. |
| RF06 | O cadastro deve incluir características, preço da diária e fotos. |
| RF07 | O proprietário deve definir períodos de disponibilidade e bloqueio. |
| RF08 | Um veículo só aparece na busca após aprovação do administrador. |

### 4.3 Busca

| Código | Requisito |
|---|---|
| RF09 | O locatário deve buscar veículos por cidade e período desejado. |
| RF10 | A busca deve permitir filtros como preço, categoria, câmbio, combustível e passageiros. |
| RF11 | A busca deve mostrar apenas veículos livres no período informado. |
| RF12 | O locatário deve ver a página de detalhes com fotos, características e avaliações. |

### 4.4 Reservas

| Código | Requisito |
|---|---|
| RF13 | O locatário deve solicitar uma reserva informando o período. |
| RF14 | O sistema deve calcular e exibir o valor das diárias, as taxas e o total antes da confirmação. |
| RF15 | O proprietário deve aceitar ou recusar a solicitação. |
| RF16 | Ambas as partes devem poder cancelar, conforme regras definidas pela dupla. |
| RF17 | Cada parte deve ver seu histórico de reservas e o status de cada uma. |

### 4.5 Pagamento simulado

| Código | Requisito |
|---|---|
| RF18 | O locatário deve escolher um meio (Pix ou cartão) e concluir um pagamento simulado. |
| RF19 | O sistema deve registrar o valor bruto, a comissão, o repasse ao proprietário e o status. |
| RF20 | Deve ser possível simular pagamento aprovado, recusado e estornado. |

### 4.6 Check-in e check-out

| Código | Requisito |
|---|---|
| RF21 | Na retirada, o locatário registra fotos, quilometragem e nível de combustível. |
| RF22 | Na devolução, registra os mesmos dados. |
| RF23 | O sistema deve exibir lado a lado os registros de retirada e devolução. |

### 4.7 Avaliações

| Código | Requisito |
|---|---|
| RF24 | Após a locação, locatário e proprietário devem avaliar um ao outro. |
| RF25 | A nota média deve aparecer no perfil do usuário e na página do veículo. |

### 4.8 Administração

| Código | Requisito |
|---|---|
| RF26 | O administrador deve aprovar ou rejeitar documentos e veículos. |
| RF27 | O administrador deve bloquear usuários e veículos. |
| RF28 | O administrador deve configurar o percentual de comissão. |
| RF29 | O administrador deve consultar reservas e pagamentos com filtros. |

---

## 5. Requisitos não funcionais

| Código | Requisito |
|---|---|
| RNF01 | **PWA:** instalável no celular, com manifesto, ícones e service worker. |
| RNF02 | **Offline:** sem conexão, exibir página amigável e as telas já visitadas que fizer sentido manter em cache. |
| RNF03 | **Responsividade:** funcionar bem em celular e desktop, priorizando o celular. |
| RNF04 | **Segurança:** senhas com hash, proteção CSRF, controle de permissão por perfil e nenhum segredo no repositório. |
| RNF05 | **Privacidade:** documentos e CNH só podem ser vistos pelo dono e pelo administrador. |
| RNF06 | **Integridade:** nunca confirmar duas reservas sobrepostas para o mesmo veículo, nem com acessos simultâneos. |
| RNF07 | **Uploads:** imagens com tipo e tamanho validados. |
| RNF08 | **Desempenho:** listagem de busca paginada. |
| RNF09 | **API:** o frontend deve consumir uma API REST documentada. |
| RNF10 | **Testes:** regras de negócio críticas com testes automatizados. |
| RNF11 | **Configuração:** variáveis sensíveis em `.env`, com `.env.example` no repositório. |
| RNF12 | **Reprodutibilidade:** outra pessoa deve subir o projeto seguindo apenas o README. |
| RNF13 | **Idioma:** interface em português do Brasil, datas e valores no padrão brasileiro (R$). |

---

## 6. Regras de negócio essenciais

> Estas regras **serão verificadas na avaliação**. Garantam que funcionem mesmo nos casos-limite.

| Código | Regra |
|---|---|
| RN01 | Não pode haver duas reservas ativas com períodos sobrepostos para o mesmo veículo. |
| RN02 | Dois locatários reservando o mesmo carro ao mesmo tempo: **só um pode ter sucesso**. |
| RN03 | A reserva só pode ocorrer em período marcado como disponível pelo proprietário. |
| RN04 | O locatário não pode reservar com CNH vencida ou não aprovada. |
| RN05 | O proprietário não pode alugar o próprio veículo. |
| RN06 | Comissão padrão de **15%** sobre o valor das diárias, configurável pelo administrador. |
| RN07 | Exemplo de referência: 4 diárias de R$ 150 = **R$ 600**; comissão **R$ 90**; repasse **R$ 510**. |
| RN08 | A reserva tem um ciclo de vida com status bem definidos; transições inválidas devem ser bloqueadas. |
| RN09 | Avaliação só é liberada após o check-out, uma vez por parte e por reserva. |
| RN10 | A quilometragem da devolução não pode ser menor que a da retirada. |

---

## 7. Tecnologias, pacotes e infraestrutura

Usar as **versões estáveis mais recentes** de cada item e **fixá-las no `requirements.txt`**. Pacotes extras são permitidos, desde que justificados no README.

### 7.1 Obrigatórios

| Camada | Tecnologia / pacote | Uso |
|---|---|---|
| Linguagem | Python 3 | Base do projeto |
| Framework | Django | Aplicação, ORM, admin |
| API | `djangorestframework` | API REST |
| Banco de dados | PostgreSQL | Servidor de banco |
| Driver | `psycopg` (v3) | Conexão Django ↔ PostgreSQL |
| Imagens | Pillow | Upload de fotos |
| Configuração | `django-environ` ou `python-decouple` | Leitura do `.env` |
| Frontend | HTML, CSS, Bootstrap 5, JavaScript (Fetch API) | Interface |
| PWA | `manifest.json`, Service Worker, Cache API | Instalação e offline |
| Versionamento | Git + GitHub | Repositório da dupla |

### 7.2 Recomendados

| Pacote | Uso |
|---|---|
| `django-filter` | Filtros da busca |
| `drf-spectacular` | Documentação da API (Swagger/OpenAPI) |
| `djangorestframework-simplejwt` | Autenticação por token, se optarem por ela |
| `pytest-django` | Testes automatizados |
| `whitenoise` | Arquivos estáticos |
| `gunicorn` | Servidor de aplicação |
| Docker + Docker Compose | Subir Django e PostgreSQL juntos |

### 7.3 Servidor de banco

- **PostgreSQL** rodando localmente ou em container — **nunca SQLite na entrega**.
- Banco, usuário e senha **próprios do projeto**, definidos no `.env`.
- O projeto deve trazer **migrations** e um **comando ou fixture** para popular dados de exemplo.

### 7.4 Ambiente

- Service Worker exige **HTTPS ou localhost** — testar a instalação do PWA nessas condições.

---

## 8. Uso do agente de IA

- O uso de agente de IA é **permitido**, mas o **prompt é responsabilidade da dupla** e faz parte da avaliação.
- Escrever o prompt **a partir deste documento**; ele **não deve ser colado sem adaptação**.
- O prompt deve definir **contexto, escopo, stack, restrições e critérios de aceite**.
- **Dividir o trabalho em etapas** — um prompt único para o sistema inteiro tende a gerar código frágil.
- Guardar cada versão do prompt em **`docs/prompts/`**, com uma nota curta do que mudou e por quê.
- **Revisar e testar** todo código gerado; a dupla deve saber **explicar qualquer trecho** na apresentação.
- Quando o agente errar uma regra de negócio, **registrar o erro e como o prompt foi corrigido**.

---

## 9. Entregáveis e marcos

O projeto tem **cinco marcos**; cada um termina com uma entrega no repositório e uma demonstração curta. As datas serão definidas pelo professor.

### Marco 1 — Planejamento
- Repositório criado, com README inicial e divisão de tarefas da dupla
- Primeira versão do prompt e plano de etapas
- Esboço das telas principais (papel ou ferramenta de wireframe)

### Marco 2 — Base do sistema
- Projeto Django conectado ao PostgreSQL, com `.env`
- Cadastro, login e perfis (RF01–RF04)
- Cadastro de veículos com fotos e disponibilidade (RF05–RF08)

### Marco 3 — Busca e reservas
- Busca e filtros (RF09–RF12)
- Reservas e pagamento simulado (RF13–RF20)
- Testes de conflito e concorrência (RN01–RN03)

### Marco 4 — Locação e PWA
- Check-in, check-out e avaliações (RF21–RF25)
- Área administrativa (RF26–RF29)
- PWA instalável e página offline (RNF01–RNF02)

### Marco 5 — Entrega final
- Sistema completo e opcionais escolhidos
- README com instalação, dados de exemplo e usuários de teste
- Documentação da API
- Histórico de prompts em `docs/prompts/`
- Apresentação com **demonstração ao vivo do fluxo completo**: cadastro do carro, reserva, pagamento, check-in, check-out e avaliação

---

## 10. Resumo dos pontos críticos

| Tema | O que será cobrado |
|---|---|
| **Concorrência** | Reservas sobrepostas jamais podem ser confirmadas, inclusive com acessos simultâneos (RN01/RN02/RNF06) |
| **Validações de reserva** | Período disponível, CNH válida/aprovada, proprietário ≠ locatário (RN03/RN04/RN05) |
| **Financeiro** | Comissão de 15% configurável, com cálculo explícito de bruto/comissão/repasse (RN06/RN07) |
| **Ciclo de vida** | Status da reserva bem definidos; transições inválidas bloqueadas (RN08) |
| **Avaliações** | Apenas após check-out, uma por parte e por reserva (RN09) |
| **Check-in/out** | KM de devolução ≥ KM de retirada; registro lado a lado (RN10/RF23) |
| **Privacidade** | Documentos e CNH restritos ao dono e ao administrador (RNF05) |
| **API** | REST documentada, consumida pelo frontend (RNF09) |
| **Testes** | Regras críticas com testes automatizados (RNF10) |
| **Reprodutibilidade** | README suficiente para subir o projeto; PostgreSQL obrigatório (RNF12) |
