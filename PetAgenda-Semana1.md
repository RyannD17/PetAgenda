# PetAgenda — Semana 1 (Fundamentos e Requisitos)

**Dupla:** [Douglas Pierry] e [Ryann Deyv's]
**Cliente fictício:** Renata Souza — rede "Tosa Boa" (banho e tosa, 3 unidades)

---

## 1. Cliente Fictício

**Perfil:** Renata Souza, dona da rede "Tosa Boa", 3 unidades de banho e tosa na cidade, cerca de 8 tosadores ao todo.

**Contexto:** Hoje o agendamento é feito por telefone, WhatsApp e caderno físico em cada unidade, sem integração entre as lojas. Isso gera faltas frequentes, dificuldade de visualizar a agenda consolidada das 3 lojas e nenhum histórico digital dos pets (raça, alergias, comportamento), o que atrasa o atendimento.

**Objetivo de negócio:** Reduzir faltas e horários ociosos, centralizar a agenda das 3 unidades em um só lugar, permitir que o cliente marque e cancele online, e manter o histórico do pet acessível para qualquer tosador da rede.

## 2. Entrevista Simulada

Entrevista com Renata Souza (proprietária da Tosa Boa):

1. **Como funciona o agendamento hoje?**
   Cada unidade tem sua própria agenda, por telefone, WhatsApp ou caderno físico — não existe um sistema único.

2. **Qual o maior problema que vocês enfrentam?**
   Faltas de última hora: o cliente marca e não aparece, e não dá tempo de preencher aquele horário.

3. **Como está organizada a equipe de tosadores?**
   Cerca de 8 tosadores distribuídos nas 3 unidades, com escala feita manualmente por cada gerente de loja.

4. **Existe alguma política de cancelamento hoje?**
   Não formalizada — os clientes cancelam em cima da hora sem nenhuma cobrança.

5. **Que informações vocês gostariam de manter sobre cada pet?**
   Raça, porte, temperamento, alergias e histórico de serviços — hoje isso fica só na memória do tosador habitual.

6. **O que seria um diferencial importante para os clientes?**
   Marcar e cancelar pelo celular, sem precisar ligar, e receber um lembrete antes do horário.

7. **Quem vai usar o sistema no dia a dia, além dos clientes?**
   As recepcionistas de cada loja, os tosadores e eu, para acompanhar as 3 unidades ao mesmo tempo.

8. **Existe alguma preocupação com os dados dos clientes?**
   Sim — guardamos telefone e às vezes dados de pagamento, e isso precisa ficar protegido.

9. **Vocês já usaram algum sistema parecido?**
   Já ouvimos falar de alguns aplicativos de agendamento para pet shop, mas nunca usamos nenhum de verdade.

10. **Se pudessem resolver uma única coisa primeiro, qual seria?**
    Parar de perder horário por falta — isso já mudaria bastante o nosso faturamento.

## 3. Brainstorming de Funcionalidades

1. Cadastro de clientes e pets (raça, porte, alergias, temperamento)
2. Histórico de serviços por pet
3. Agendamento online pelo cliente (self-service)
4. Confirmação automática do agendamento (WhatsApp/e-mail)
5. Lembrete automático antes do horário marcado
6. Cancelamento/reagendamento pelo próprio cliente
7. Política de cancelamento com prazo mínimo
8. Agenda consolidada das 3 unidades
9. Atribuição de tosador por horário e unidade
10. Bloqueio de horário por folga/férias do tosador
11. Cadastro de serviços e preços (banho, tosa, hidratação etc.)
12. Duração estimada por tipo de serviço/porte do pet
13. Fila de espera para horários lotados
14. Avaliação do atendimento pelo cliente
15. Painel de ocupação por unidade para a gerência
16. Relatório de faltas e cancelamentos
17. Programa de fidelidade (pontos/cashback)
18. Lembrete de vacina/vermífugo pendente
19. Perfis de acesso (recepção, tosador, gerente)
20. Cadastro de novas unidades da rede

## 4. Benchmarking de Sistemas Similares

Dois sistemas brasileiros de agendamento para pet shops foram usados como referência:

| Critério | Appet.tosa | Banhosoft |
|---|---|---|
| Agendamento online pelo cliente | Sim — o tutor agenda sozinho pelo app | O site destaca acesso online, mas o foco divulgado é a agenda da equipe; não fica claro se o tutor agenda diretamente |
| Gestão de múltiplas unidades | Não indicado nas informações públicas | Não indicado nas informações públicas |
| Lembretes automáticos | Sim, via integração com WhatsApp | Sim — lembretes automáticos são apresentados como recurso central para reduzir faltas |

**Leitura para o PetAgenda:** nenhum dos dois concorrentes deixa claro suporte à gestão de múltiplas unidades numa visão só — isso pode ser um diferencial real do PetAgenda para a Tosa Boa.

*Fontes consultadas: Appet.tosa (appettosa.com.br); Banhosoft (banhosoft.com.br).*

## 5. Lista de Requisitos

**Pesquisa aplicada:** antes de escrever a lista, pesquisamos boas práticas de elicitação de requisitos. Os requisitos funcionais foram escritos com verbo de ação no formato "O sistema deve...", prática comum para deixar claro o que o sistema precisa realizar. Os requisitos não-funcionais foram descritos de forma mensurável (com números e limites), já que um requisito não-funcional só é útil se puder ser testado e verificado — não bastam adjetivos vagos como "rápido" ou "seguro".

*Fontes consultadas: "Engenharia de Software Moderna", Cap. 3 – Requisitos (engsoftmoderna.info); "Boas práticas para escrever requisitos de software" – Lyncas (lyncas.net).*

### Requisitos Funcionais

- RF01: O sistema deve permitir o cadastro de clientes e seus pets (nome, raça, porte, alergias, temperamento).
- RF02: O sistema deve manter o histórico de serviços realizados para cada pet.
- RF03: O sistema deve permitir que o cliente agende um horário de banho/tosa pelo aplicativo ou site.
- RF04: O sistema deve enviar confirmação automática do agendamento por WhatsApp ou e-mail.
- RF05: O sistema deve enviar um lembrete automático antes do horário agendado.
- RF06: O sistema deve permitir que o cliente cancele ou reagende um horário já marcado.
- RF07: O sistema deve exibir a agenda consolidada das 3 unidades em uma única tela para a gerência.
- RF08: O sistema deve permitir a atribuição de um tosador específico a cada agendamento.
- RF09: O sistema deve permitir o bloqueio de horários por folga, férias ou ausência do tosador.
- RF10: O sistema deve permitir o cadastro dos serviços oferecidos e seus respectivos preços.
- RF11: O sistema deve gerar um relatório de faltas e cancelamentos por período e por unidade.
- RF12: O sistema deve permitir que o cliente avalie o atendimento após o serviço.

### Requisitos Não-Funcionais

- RNF01 (Desempenho): a agenda do dia deve carregar em no máximo 2 segundos, mesmo consolidando as 3 unidades.
- RNF02 (Segurança): dados pessoais (telefone, dados de pagamento) devem ser armazenados de forma criptografada, em conformidade com a LGPD.
- RNF03 (Disponibilidade): o agendamento online deve funcionar 24h por dia, 7 dias por semana, com no mínimo 99% de uptime mensal.
- RNF04 (Usabilidade): um cliente novo deve conseguir concluir um agendamento em no máximo 3 telas/passos.
- RNF05 (Compatibilidade): o sistema deve funcionar nos navegadores mais usados (Chrome, Safari) e em Android e iOS.
- RNF06 (Confiabilidade): toda alteração em um agendamento deve ser registrada em log (quem alterou, quando e o quê).

## 6. Contrato de Colaboração da Dupla

**Integrantes:** [Douglas Pierry] e [Ryann Deyv's]

**Comunicação**
- Ferramenta principal: [a definir — ex. WhatsApp/Discord]
- Reuniões síncronas: pelo menos 2x por semana, com pauta curta e registro das decisões
- Documentação compartilhada em [Google Docs/Drive], com histórico de edições visível

**Divisão de tarefas**
- [Douglas Pierry]: requisitos, documentação e modelagem estática (modelo conceitual, diagrama de classes)
- [Ryann Deyv's]: modelagem dinâmica (casos de uso, diagramas de sequência), backlog e versionamento no Git
- Revisão cruzada obrigatória antes de cada entrega semanal

**Resolução de conflitos**
- Divergências técnicas são discutidas e decididas juntos nas reuniões síncronas
- Sem consenso após uma reunião, a decisão é registrada com a justificativa de ambos e levada ao professor no encontro semanal
- Atrasos ou dificuldades são avisados com antecedência ao outro integrante
