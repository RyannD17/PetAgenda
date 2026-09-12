# PetAgenda — Semana 2 (Modelagem e Análise)

## 1. Documento de Requisitos (v1.0)

### Requisitos Funcionais

| ID | Descrição | Prioridade |
|---|---|---|
| RF01 | O sistema deve permitir o cadastro de clientes e seus pets (nome, raça, porte, alergias, temperamento). | Essencial |
| RF02 | O sistema deve manter o histórico de serviços realizados para cada pet. | Importante |
| RF03 | O sistema deve permitir que o cliente agende um horário de banho/tosa pelo aplicativo ou site. | Essencial |
| RF04 | O sistema deve enviar confirmação automática do agendamento por WhatsApp ou e-mail. | Importante |
| RF05 | O sistema deve enviar um lembrete automático antes do horário agendado. | Importante |
| RF06 | O sistema deve permitir que o cliente cancele ou reagende um horário já marcado. | Essencial |
| RF07 | O sistema deve exibir a agenda consolidada das 3 unidades em uma única tela para a gerência. | Essencial |
| RF08 | O sistema deve permitir a atribuição de um tosador específico a cada agendamento. | Essencial |
| RF09 | O sistema deve permitir o bloqueio de horários por folga, férias ou ausência do tosador. | Importante |
| RF10 | O sistema deve permitir o cadastro dos serviços oferecidos e seus respectivos preços. | Essencial |
| RF11 | O sistema deve gerar um relatório de faltas e cancelamentos por período e por unidade. | Desejável |
| RF12 | O sistema deve permitir que o cliente avalie o atendimento após o serviço. | Desejável |

### Requisitos Não-Funcionais

| ID | Descrição | Prioridade |
|---|---|---|
| RNF01 (Desempenho) | A agenda do dia deve carregar em no máximo 2 segundos, mesmo consolidando as 3 unidades. | Importante |
| RNF02 (Segurança) | Dados pessoais (telefone, dados de pagamento) devem ser armazenados de forma criptografada, em conformidade com a LGPD. | Essencial |
| RNF03 (Disponibilidade) | O agendamento online deve funcionar 24h/dia, 7 dias/semana, com no mínimo 99% de uptime mensal. | Importante |
| RNF04 (Usabilidade) | Um cliente novo deve conseguir concluir um agendamento em no máximo 3 telas/passos. | Importante |
| RNF05 (Compatibilidade) | O sistema deve funcionar nos navegadores mais usados (Chrome, Safari) e em Android/iOS. | Desejável |
| RNF06 (Confiabilidade) | Toda alteração em um agendamento deve ser registrada em log (quem, quando, o quê). | Desejável |

## 2. Diagrama de Casos de Uso

Atores: **Cliente**, **Funcionário** (generaliza **Recepcionista**, **Tosador**, **Gerente**). O diagrama cobre 12 casos de uso, incluindo os relacionamentos `«include»` (Agendar Horário → Enviar Confirmação Automática; Cancelar/Reagendar Horário → Aplicar Política de Cancelamento), `«extend»` (Entrar na Fila de Espera → Agendar Horário) e generalização (Recepcionista/Tosador/Gerente → Funcionário).

## 3. Modelo Conceitual

Entidades do domínio: Cliente, Pet, Unidade, Funcionário (e subtipos), Serviço, Agendamento, Avaliação — com as relações principais entre elas (sem atributos/métodos detalhados, foco na estrutura).

## 4. Histórias de Usuário

- HU01: Como cliente, quero agendar um horário de banho/tosa pelo celular, para não precisar ligar pra loja.
- HU02: Como cliente, quero receber um lembrete antes do meu horário, para não esquecer o compromisso.
- HU03: Como cliente, quero cancelar ou reagendar um horário, para me ajustar caso não possa comparecer.
- HU04: Como cliente, quero ver o histórico de serviços do meu pet, para acompanhar o que já foi feito.
- HU05: Como recepcionista, quero ver a agenda consolidada das 3 unidades, para reorganizar horários quando necessário.
- HU06: Como tosador, quero ver os detalhes do pet antes do atendimento (alergias, temperamento), para me preparar adequadamente.
- HU07: Como gerente, quero gerar relatórios de faltas e cancelamentos, para entender o impacto no faturamento.
- HU08: Como cliente, quero avaliar o atendimento após o serviço, para dar feedback sobre a experiência.

## 5. Critérios de Aceitação (3 histórias prioritárias)

**HU01 — Agendar horário pelo celular**
- Given que sou um cliente cadastrado e autenticado
- When seleciono um pet, serviço, unidade e um horário disponível
- Then o sistema cria o agendamento com status "Confirmado" e exibe a confirmação

**HU02 — Receber lembrete automático**
- Given que tenho um agendamento confirmado
- When faltam poucas horas para o horário marcado
- Then o sistema envia automaticamente um lembrete por WhatsApp ou e-mail

**HU03 — Cancelar/reagendar horário**
- Given que tenho um agendamento confirmado
- When solicito o cancelamento ou reagendamento
- Then o sistema aplica a política de cancelamento vigente e atualiza o status do agendamento
