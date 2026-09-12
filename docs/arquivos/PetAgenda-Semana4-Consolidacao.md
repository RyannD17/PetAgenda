# PetAgenda — Semana 4 (Consolidação e Reflexão)

## 1. Documento de Requisitos (versão final 2.0)

A versão 2.0 mantém os requisitos definidos na Semana 1/2 e adiciona a coluna de rastreabilidade, ligando cada requisito ao caso de uso e à(s) classe(s) que o implementam.

### Requisitos Funcionais — rastreabilidade

| ID | Descrição | Caso de Uso | Classe(s) relacionada(s) |
|---|---|---|---|
| RF01 | Cadastro de clientes e pets | Cadastrar Cliente / Cadastrar Pet | Cliente, Pet |
| RF02 | Histórico de serviços por pet | Consultar Histórico do Pet | Pet, Agendamento |
| RF03 | Agendamento online pelo cliente | **Agendar Horário** | Cliente, Agendamento, Unidade, Tosador, Servico |
| RF04 | Confirmação automática do agendamento | Enviar Confirmação Automática *(include de Agendar Horário)* | ServicoNotificacao, Agendamento |
| RF05 | Lembrete automático antes do horário | Enviar Lembrete Automático | ServicoNotificacao, Agendamento |
| RF06 | Cancelar/reagendar horário | Cancelar/Reagendar Horário | Agendamento |
| RF07 | Agenda consolidada das 3 unidades | Consultar Agenda Consolidada | Unidade, Gerente, Agendamento |
| RF08 | Atribuição de tosador ao agendamento | Agendar Horário / Atribuir Tosador | Tosador, Agendamento |
| RF09 | Bloqueio de horários do tosador | Bloquear Horário | Tosador, Bloqueio |
| RF10 | Cadastro de serviços e preços | Cadastrar Serviço | Servico |
| RF11 | Relatório de faltas e cancelamentos | Gerar Relatório | Gerente, Relatorio, Agendamento |
| RF12 | Avaliação do atendimento pelo cliente | Avaliar Atendimento | Cliente, Avaliacao, Agendamento |

### Requisitos Não-Funcionais — rastreabilidade

| ID | Descrição | Onde é validado/observado |
|---|---|---|
| RNF01 (Desempenho) | Agenda carrega em até 2s | Caso de uso "Consultar Agenda Consolidada"; classe `Unidade` |
| RNF02 (Segurança/LGPD) | Dados pessoais criptografados | Classes `Cliente` (telefone) — atributo tratado como dado sensível |
| RNF03 (Disponibilidade) | 99% uptime, 24x7 | Caso de uso "Agendar Horário" (canal online) |
| RNF04 (Usabilidade) | Agendamento em até 3 telas | Fluxo principal do caso de uso "Agendar Horário" |
| RNF05 (Compatibilidade) | Chrome/Safari, Android/iOS | Camada de interface (não modelada em classes de domínio) |
| RNF06 (Confiabilidade) | Log de alterações no agendamento | Classe `Agendamento` (métodos `confirmar()`, `cancelar()`, `reagendar()`) |

---

## 2. Matriz de Rastreabilidade

Visão consolidada ligando requisito → caso de uso → classe(s) → história de usuário correspondente (quando aplicável):

| Requisito | Caso de Uso | Classe(s) | História de Usuário |
|---|---|---|---|
| RF03 | Agendar Horário | Cliente, Agendamento, Unidade, Tosador | HU01 |
| RF05 | Enviar Lembrete Automático | ServicoNotificacao, Agendamento | HU02 |
| RF06 | Cancelar/Reagendar Horário | Agendamento | HU03 |
| RF02 | Consultar Histórico do Pet | Pet, Agendamento | HU04 |
| RF07 | Consultar Agenda Consolidada | Unidade, Gerente, Agendamento | HU05 |
| RF01 | Visualizar Detalhes do Pet | Pet, Tosador | HU06 |
| RF11 | Gerar Relatório | Gerente, Relatorio | HU07 |
| RF12 | Avaliar Atendimento | Cliente, Avaliacao | HU08 |

Essa matriz evidencia que todos os requisitos essenciais (RF01, RF03, RF06, RF07) têm cobertura completa em caso de uso, classe e história de usuário — nenhum requisito ficou "órfão" de artefato de design.

---

## 3. Principais desafios e como foram superados

- **Ambiguidade sobre a política de cancelamento:** a cliente fictícia não tinha uma política formalizada. Resolvemos isso propondo uma regra de negócio explícita (RN04, cancelamento com menos de 2h de antecedência conta como falta) e registrando-a como uma decisão da dupla a ser validada com a cliente, em vez de deixar o requisito em aberto.
- **Concorrência no agendamento de horários:** identificamos, ao detalhar o caso de uso de maior risco, que dois clientes poderiam tentar reservar o mesmo horário simultaneamente. Isso não estava explícito nos requisitos da Semana 1/2, e tratamos como um fluxo alternativo (A1) no detalhamento da Semana 3.
- **Nível de detalhe do diagrama de classes:** equilibrar "funcional, não perfeito" com o pedido de detalhamento (atributos, métodos, visibilidade, multiplicidade) exigiu reduzir o escopo a apenas os métodos essenciais para os casos de uso já levantados, evitando um modelo genérico demais.
