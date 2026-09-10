# PetAgenda — Semana 3 (Design Técnico e Detalhamento)

## 1. Diagrama de Classes Refinado

Arquivo: `PetAgenda-DiagramaClasses-Refinado.svg` (versão detalhada, com atributos, métodos, visibilidade e multiplicidade — evolução do Modelo Conceitual da Semana 2)

**Pesquisa aplicada:** antes de refinar o diagrama, pesquisamos boas práticas de modelagem de classes UML (convenção de visibilidade `+` público / `-` privado / `#` protegido, como representar herança e associações com multiplicidade). Aplicamos essas convenções abaixo.

### Especificação das classes

**Cliente**
- `- id: int`
- `- nome: String`
- `- telefone: String`
- `- email: String`
- `+ cadastrar(): void`
- `+ agendarHorario(servico, unidade, tosador, dataHora): Agendamento`
- `+ cancelarAgendamento(agendamento): void`
- `+ avaliarAtendimento(agendamento, nota, comentario): Avaliacao`

**Pet**
- `- id: int`
- `- nome: String`
- `- raca: String`
- `- porte: {Pequeno, Médio, Grande}`
- `- alergias: String`
- `- temperamento: String`
- `+ adicionarHistorico(agendamento): void`
- `+ consultarHistorico(): List<Agendamento>`

**Unidade**
- `- id: int`
- `- nome: String`
- `- endereco: String`
- `+ consultarAgendaConsolidada(data): List<Agendamento>`

**Funcionario** (classe abstrata)
- `# id: int`
- `# nome: String`
- `# cargo: String`
- `+ login(): boolean`

**Recepcionista** (herda de Funcionario)
- `+ criarAgendamentoManual(cliente, pet, servico, tosador, dataHora): Agendamento`

**Tosador** (herda de Funcionario)
- `- especialidades: List<String>`
- `+ bloquearHorario(dataInicio, dataFim, motivo): Bloqueio`
- `+ visualizarDetalhesPet(pet): void`

**Gerente** (herda de Funcionario)
- `+ gerarRelatorio(unidade, periodo): Relatorio`
- `+ visualizarAgendaConsolidada(): List<Agendamento>`

**Servico**
- `- id: int`
- `- nome: String`
- `- preco: decimal`
- `- duracaoEstimadaMin: int`

**Agendamento**
- `- id: int`
- `- dataHora: DateTime`
- `- status: {Confirmado, Cancelado, Concluído, EmFilaDeEspera}`
- `+ confirmar(): void`
- `+ cancelar(): void`
- `+ reagendar(novaDataHora): void`

**Bloqueio**
- `- id: int`
- `- dataInicio: DateTime`
- `- dataFim: DateTime`
- `- motivo: String`

**Avaliacao**
- `- id: int`
- `- nota: int (1-5)`
- `- comentario: String`

### Relações e multiplicidade

| Classe A | Multiplicidade | Relação | Multiplicidade | Classe B |
|---|---|---|---|---|
| Cliente | 1 | possui | 1..* | Pet |
| Pet | 1 | gera | 0..* | Agendamento |
| Cliente | 1 | solicita | 0..* | Agendamento |
| Unidade | 1 | emprega | 1..* | Funcionario |
| Unidade | 1 | recebe | 0..* | Agendamento |
| Tosador | 1 | atende | 0..* | Agendamento |
| Tosador | 1 | define | 0..* | Bloqueio |
| Agendamento | 1..* | inclui | 1..* | Servico |
| Agendamento | 1 | pode gerar | 0..1 | Avaliacao |
| Funcionario | — | generalização | — | Recepcionista, Tosador, Gerente |

**Regra de negócio refletida no modelo:** um `Agendamento` só pode ser confirmado se não houver `Bloqueio` do `Tosador` que sobreponha a `dataHora` solicitada (validada em tempo de criação).

---

## 2. Caso de Uso de Maior Risco Detalhado: "Agendar Horário"

**Por que é o de maior risco:** envolve a maior quantidade de regras concorrentes (disponibilidade do tosador, conflito entre unidades, política de cancelamento e fila de espera) e é o caminho mais usado pelo cliente — uma falha aqui compromete diretamente o objetivo de negócio (reduzir faltas e horários ociosos).

**Ator principal:** Cliente
**Atores secundários:** Sistema de Notificação (WhatsApp/e-mail)

**Pré-condições:**
- Cliente está cadastrado e autenticado no aplicativo/site
- Pelo menos um pet do cliente está cadastrado
- Há ao menos uma unidade, serviço e tosador ativos no sistema

**Pós-condições (sucesso):**
- Um novo `Agendamento` é criado com status "Confirmado"
- O horário escolhido fica indisponível para outros clientes na mesma unidade/tosador
- Uma confirmação é enviada ao cliente por WhatsApp ou e-mail

**Fluxo Principal:**
1. Cliente seleciona o pet para o qual deseja agendar o serviço.
2. Cliente escolhe a unidade "Tosa Boa" desejada.
3. Sistema exibe os serviços disponíveis e seus preços (RF10).
4. Cliente seleciona o(s) serviço(s) desejado(s).
5. Sistema exibe os horários disponíveis, cruzando a agenda da unidade com os bloqueios dos tosadores (RF09).
6. Cliente seleciona um horário disponível (e, opcionalmente, um tosador de preferência).
7. Sistema valida a disponibilidade em tempo real (evita conflito de concorrência entre dois clientes escolhendo o mesmo horário).
8. Sistema cria o `Agendamento` com status "Confirmado".
9. Sistema envia confirmação automática por WhatsApp ou e-mail (RF04) — *inclui* o caso de uso "Enviar Confirmação Automática".
10. Sistema exibe a confirmação na tela do cliente.

**Fluxos Alternativos:**

- **A1 — Horário escolhido ficou indisponível entre os passos 6 e 7 (condição de corrida):**
  1. Sistema informa que o horário não está mais disponível.
  2. Sistema retorna ao passo 5 com a lista de horários atualizada.

- **A2 — Não há horários disponíveis no dia/unidade desejados:**
  1. Sistema oferece a opção de entrar na Fila de Espera (*extend* do caso de uso "Entrar na Fila de Espera").
  2. Se o cliente aceitar, o sistema cria um registro com status "EmFilaDeEspera" e notifica o cliente caso um horário seja liberado (por exemplo, após um cancelamento).

- **A3 — Falha no envio da confirmação automática (ex.: WhatsApp fora do ar):**
  1. Sistema mantém o `Agendamento` como "Confirmado" (o agendamento em si não depende da notificação).
  2. Sistema registra a falha de envio em log (RNF06) e tenta reenviar a notificação em uma janela de tempo posterior.

**Regras de Negócio:**
- RN01: Um tosador não pode ter dois agendamentos confirmados com o mesmo horário sobreposto.
- RN02: O sistema não permite agendar em horários marcados como `Bloqueio` pelo tosador.
- RN03: O agendamento só pode ser feito com, no mínimo, 1 hora de antecedência em relação ao horário desejado.
- RN04: Cancelamentos com menos de 2 horas de antecedência são registrados como "falta" para fins de relatório (RF11), conforme política de cancelamento a ser formalizada com a cliente Renata.

---

## 3. Diagrama de Sequência — "Agendar Horário"

Arquivo: `PetAgenda-DiagramaSequencia-AgendarHorario.svg`

**Participantes:** `Cliente`, `App/Site (Interface)`, `ControladorAgendamento`, `Unidade`, `Tosador`, `Agendamento`, `ServicoNotificacao`

**Resumo da interação (fluxo principal):**

1. `Cliente` → `Interface`: seleciona pet, unidade e serviço
2. `Interface` → `ControladorAgendamento`: solicitarHorariosDisponiveis(unidade, servico, data)
3. `ControladorAgendamento` → `Unidade`: consultarAgenda(data)
4. `ControladorAgendamento` → `Tosador`: consultarBloqueios(data)
5. `ControladorAgendamento` → `Interface`: retorna lista de horários livres
6. `Cliente` → `Interface`: seleciona horário
7. `Interface` → `ControladorAgendamento`: confirmarAgendamento(cliente, pet, servico, tosador, dataHora)
8. `ControladorAgendamento` → `Agendamento`: create(status = "Confirmado")
9. `ControladorAgendamento` → `ServicoNotificacao`: enviarConfirmacao(cliente, agendamento)
10. `ServicoNotificacao` → `Cliente`: envia WhatsApp/e-mail
11. `ControladorAgendamento` → `Interface`: retorna confirmação
12. `Interface` → `Cliente`: exibe tela de sucesso

**Fluxo alternativo representado no diagrama:** se, no passo 7, o `ControladorAgendamento` detectar que o horário deixou de estar livre, ele retorna uma mensagem de erro para a `Interface`, que reinicia a seleção de horário (equivalente ao fluxo A1 descrito acima).

---

## 4. Backlog do Produto

Backlog priorizado combinando as Histórias de Usuário (Semana 2) com itens adicionais do brainstorming (Semana 1), com estimativa relativa de esforço (P = Pequeno, M = Médio, G = Grande).

| Prioridade | Item | Esforço | Origem |
|---|---|---|---|
| 1 | HU01 — Agendar horário pelo celular | G | RF03 |
| 2 | HU05 — Ver agenda consolidada das 3 unidades | M | RF07 |
| 3 | HU03 — Cancelar/reagendar horário | M | RF06 |
| 4 | HU02 — Receber lembrete automático | P | RF05 |
| 5 | Bloqueio de horário por folga/férias do tosador | M | RF09 |
| 6 | HU06 — Ver detalhes do pet antes do atendimento | P | RF01 |
| 7 | HU04 — Ver histórico de serviços do pet | P | RF02 |
| 8 | Cadastro de serviços e preços | P | RF10 |
| 9 | Fila de espera para horários lotados | M | Brainstorming (item 13) |
| 10 | HU07 — Gerar relatório de faltas/cancelamentos | M | RF11 |
| 11 | HU08 — Avaliar atendimento | P | RF12 |
| 12 | Programa de fidelidade (pontos/cashback) | G | Brainstorming (item 17) — backlog futuro, fora do escopo do MVP |

**Critério de priorização:** itens que atacam diretamente o problema relatado na entrevista (faltas e falta de visão consolidada da agenda) vieram primeiro; itens de valor agregado (fidelidade, avaliações) ficaram no final do backlog.

---

**Conteúdo do README.md:**
- Descrição do projeto: sistema de agendamento "PetAgenda" para a rede fictícia "Tosa Boa" (3 unidades de banho e tosa)
- Escopo: atividade de engenharia de software (requisitos, modelagem UML e documentação) — sem implementação de código
- Instruções de navegação: os artefatos de cada sprint estão organizados nas pastas `docs/requisitos`, `docs/modelos` e `docs/historias`, com um arquivo Markdown por sprint na raiz do projeto (`PetAgenda-SemanaN-*.md`)
- Autoria: [Nome do Aluno A] e [Nome do Aluno B]

**Convenção de commits adotada pela dupla:** mensagens descritivas no padrão `[Semana X] descrição da alteração` (ex.: `[Semana 3] Adiciona diagrama de classes refinado`), com autoria identificável de cada integrante.