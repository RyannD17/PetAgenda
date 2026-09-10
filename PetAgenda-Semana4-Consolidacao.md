# PetAgenda — Semana 4 (Consolidação e Reflexão)

**Dupla:** [Nome do Aluno A] e [Nome do Aluno B]
**Cliente fictício:** Renata Souza — rede "Tosa Boa" (banho e tosa, 3 unidades)

---

## 1. Documento de Requisitos (versão final 2.0)

A versão 2.0 mantém os requisitos definidos na Semana 1/2 (ver `documento-requisitos-v1.0.md`) e adiciona a coluna de rastreabilidade, ligando cada requisito ao caso de uso e à(s) classe(s) que o implementam.

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

## 2. Pacote de Modelos UML

Consolidação de todos os diagramas produzidos ao longo do projeto, entregues em PDF (visualização) e nos arquivos editáveis originais (draw.io/Mermaid):

| Diagrama | Arquivo editável | Sprint de origem |
|---|---|---|
| Diagrama de Casos de Uso | `PetAgenda-DiagramaCasosDeUso.svg` | Semana 2 |
| Modelo Conceitual | `PetAgenda-ModeloConceitual.mermaid` | Semana 2 |
| Diagrama de Classes (refinado) | `PetAgenda-DiagramaClasses-Refinado.svg` | Semana 3 |
| Diagrama de Sequência — Agendar Horário | `PetAgenda-DiagramaSequencia-AgendarHorario.svg` | Semana 3 |

Todos os diagramas foram exportados também em PDF único (`PetAgenda-PacoteUML.pdf`) para facilitar a apresentação no encontro final.

---

## 3. Matriz de Rastreabilidade

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

## 4. Relatório Final

### Resumo do escopo e decisões técnicas principais

O projeto PetAgenda tratou do levantamento de requisitos e da modelagem de um sistema de agendamento para a rede fictícia "Tosa Boa" (3 unidades de banho e tosa), com foco em resolver dois problemas centrais identificados na entrevista com a cliente: faltas de última hora e ausência de visão consolidada da agenda entre as unidades.

Entre as decisões técnicas mais relevantes:
- Optamos por modelar `Funcionario` como uma superclasse abstrata, generalizando `Recepcionista`, `Tosador` e `Gerente`, porque os três perfis compartilham atributos de identificação e login, mas têm comportamentos claramente distintos (ex.: apenas o `Tosador` bloqueia horários).
- Escolhemos tratar a "Fila de Espera" como um caso de uso do tipo `«extend»` de "Agendar Horário", em vez de um caso de uso independente, porque ela só existe no contexto de uma tentativa de agendamento sem sucesso.
- Definimos "Agendar Horário" como o caso de uso de maior risco por concentrar o maior número de regras de negócio concorrentes (bloqueio de tosador, concorrência entre clientes, política de cancelamento), o que o tornou prioridade de detalhamento no fluxo de exceção.

### Principais desafios e como foram superados

- **Ambiguidade sobre a política de cancelamento:** a cliente fictícia não tinha uma política formalizada. Resolvemos isso propondo uma regra de negócio explícita (RN04, cancelamento com menos de 2h de antecedência conta como falta) e registrando-a como uma decisão da dupla a ser validada com a cliente, em vez de deixar o requisito em aberto.
- **Concorrência no agendamento de horários:** identificamos, ao detalhar o caso de uso de maior risco, que dois clientes poderiam tentar reservar o mesmo horário simultaneamente. Isso não estava explícito nos requisitos da Semana 1/2, e tratamos como um fluxo alternativo (A1) no detalhamento da Semana 3.
- **Nível de detalhe do diagrama de classes:** equilibrar "funcional, não perfeito" com o pedido de detalhamento (atributos, métodos, visibilidade, multiplicidade) exigiu reduzir o escopo a apenas os métodos essenciais para os casos de uso já levantados, evitando um modelo genérico demais.

### Reflexão da dupla

**Aprendizado técnico:** ficou claro que boa parte do valor da modelagem UML não está em desenhar bonito, mas em forçar a dupla a explicitar regras de negócio que, na entrevista, ficaram implícitas (como a política de cancelamento e o conflito de horários) — o processo de detalhar o caso de uso de maior risco revelou lacunas que não apareciam nos requisitos de mais alto nível.

**Aprendizado sobre trabalho em equipe:** a divisão de responsabilidades combinada no Contrato de Colaboração (modelagem estática vs. dinâmica) funcionou bem para produzir os artefatos, mas exigiu revisão cruzada constante para manter os dois lados do modelo consistentes entre si (por exemplo, garantir que toda classe usada no diagrama de sequência já existisse no diagrama de classes) — reforçando que comunicação frequente é tão importante quanto a divisão de tarefas.

---

## 5. Repositório Git Atualizado

**Commits da Semana 3 e 4 (mensagens descritivas, padrão `[Semana X] ...`):**
- `[Semana 3] Adiciona diagrama de classes refinado`
- `[Semana 3] Detalha caso de uso de maior risco (Agendar Horário)`
- `[Semana 3] Adiciona diagrama de sequência do agendamento`
- `[Semana 3] Adiciona backlog do produto priorizado`
- `[Semana 4] Atualiza documento de requisitos para v2.0 com rastreabilidade`
- `[Semana 4] Adiciona matriz de rastreabilidade`
- `[Semana 4] Adiciona relatório final e finaliza README`

**README final (status do projeto):**
- Status: projeto de modelagem concluído (requisitos, casos de uso, modelo conceitual, classes refinadas, sequência do caso de uso crítico e backlog)
- Artefatos: disponíveis nas pastas `docs/requisitos`, `docs/modelos` e `docs/historias`, além dos arquivos `PetAgenda-SemanaN-*.md` na raiz
- Autoria: [Nome do Aluno A] e [Nome do Aluno B]
- Próximos passos (fora do escopo desta atividade): implementação do sistema a partir dos artefatos entregues

---

*Pontos para ajustar depois: nomes da dupla em [Nome do Aluno A] / [Nome do Aluno B]; a reflexão final deve ser revisada e reescrita pela dupla com as impressões reais de vocês sobre o processo, já que essa parte é pessoal e conta para a avaliação; conferir se o relatório final está dentro do limite de 3 páginas ao exportar para PDF/Word.*
