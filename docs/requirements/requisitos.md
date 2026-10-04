# nassauTickets — Documento de Requisitos

**Projeto:** nassauTickets — Sistema de Controle de Atendimento (Laboratório de Análises Clínicas)
**Instituição:** UNINASSAU
**Autor:** Tarsílio Aureliano Soares Silva — Matrícula 01803880
**Versão:** 1.0 (Fase 1)

## 1. Requisitos Funcionais

| ID | Requisito |
|----|-----------|
| RF01 | O totem deve permitir ao cliente (AC), sem login, emitir senha dos tipos SP, SG ou SE. |
| RF02 | O sistema deve gerar a numeração no padrão `YYMMDD-PPSQ`, com sequência de 3 dígitos por tipo e reinício diário. |
| RF03 | O atendente (AA) deve acionar uma função para chamar a próxima senha, respeitando as regras de prioridade. |
| RF04 | O AA deve poder iniciar o atendimento após o cliente chegar ao guichê. |
| RF05 | O AA deve poder encerrar o atendimento. |
| RF06 | O AA deve dispor do botão "Chamar Novamente", que repete a chamada e o áudio precedido de "Última chamada". |
| RF07 | O painel deve exibir as 5 últimas senhas chamadas e o guichê, sem exibir a próxima senha. |
| RF08 | Cada chamada deve emitir áudio informando prioridade, sequencial da senha e guichê. |
| RF09 | Após duas chamadas sem comparecimento, a senha deve ir para NÃO_COMPARECEU e o sistema segue para a próxima. |
| RF10 | O sistema deve controlar o expediente (7h às 17h): fora dele não emite nem chama; ao fim, atendimentos em andamento são concluídos e as senhas restantes descartadas. |
| RF11 | O sistema deve ter login para o AA, com perfil adicional de gestor (único) responsável por cadastros e relatórios. |
| RF12 | O sistema deve gerar relatórios diário e mensal: total emitido e atendido (geral e por prioridade), detalhamento das senhas, tempo médio de atendimento (TM) e auditoria. |
| RF13 | O relatório de auditoria deve registrar AA, guichê, senha, 1ª e 2ª chamadas, início e fim do atendimento. |
| RF14 | O sistema deve acompanhar o desempenho dos atendimentos (indicadores de TM, volume e abandono). |

## 2. Regras de Negócio

| ID | Regra |
|----|-------|
| RN01 | Ordem de chamada: `[SP] → [SE\|SG] → [SP] → [SE\|SG]`; SE é chamada após SP e antes de SG, quando existir. |
| RN02 | Se uma fila estiver vazia, o sistema escolhe o próximo atendimento mantendo as regras de prioridade. |
| RN03 | Qualquer guichê atende qualquer tipo de senha. |
| RN04 | TM: SG 5 min (variação de ±3 min); SP 15 min (variação de ±5 min, distribuição uniforme); SE ≈ 1 min em 95% dos casos e 5 min em 5%. |
| RN05 | Cerca de 5% das senhas emitidas não são atendidas por responsabilidade do cliente e são descartadas sem SA. |
| RN06 | Máquina de estados: EMITIDA → AGUARDANDO → CHAMADA → CHAMADA_NOVAMENTE → EM_ATENDIMENTO → ATENDIDA; alternativa: NÃO_COMPARECEU. |
| RN07 | Duas chamadas sem comparecimento tornam a senha abandonada. |
| RN08 | A próxima senha não é exibida no painel, pois novas emissões podem alterar a sequência. |
| RN09 | O cliente interage apenas de forma anônima, pelo totem. |

## 3. Requisitos Não Funcionais

| ID | Categoria | Requisito |
|----|-----------|-----------|
| RNF01 | Segurança | Autenticação do AA e gestor com senha armazenada em hash e sessão/token com expiração; controle de acesso por perfil. |
| RNF02 | Concorrência | Chamadas simultâneas de dois ou mais AA devem ser tratadas com transação e bloqueio (ex.: `SELECT ... FOR UPDATE`), garantindo que uma senha seja atribuída a um único guichê. |
| RNF03 | Disponibilidade | Em falha do backend ou banco, frontend e painel exibem mensagem de indisponibilidade, mantêm a última informação válida e tentam reconexão automática. |
| RNF04 | Auditoria | Toda ação de chamada, início e fim de atendimento deve ser registrada com data/hora e responsável. |
| RNF05 | Desempenho | A emissão de senha e a chamada devem responder em até 2 s em condições normais; o painel atualiza automaticamente. |
| RNF06 | LGPD | Coleta mínima de dados (cliente anônimo); dados de atendentes protegidos e acessados apenas por perfis autorizados. |
| RNF07 | Acessibilidade | Interface conforme a legislação (Lei Brasileira de Inclusão) e WCAG: contraste, navegação por teclado, textos alternativos e áudio de chamada. |
| RNF08 | Tecnologia | Frontend em React; backend Node.js 22 com Express; banco MySQL 8.0; API REST em JSON. |
| RNF09 | Manutenibilidade | Código organizado em componentes, páginas e serviços; versionamento com branches `main` e `dev`. |

## 4. Casos de Uso (resumo)

| ID | Caso de uso | Ator |
|----|-------------|------|
| UC01 | Emitir senha | AC |
| UC02 | Autenticar-se | AA / Gestor |
| UC03 | Chamar próxima senha | AA |
| UC04 | Chamar novamente | AA |
| UC05 | Iniciar atendimento | AA |
| UC06 | Encerrar atendimento | AA |
| UC07 | Visualizar painel de chamadas | AC |
| UC08 | Gerar relatórios diário e mensal | Gestor |
| UC09 | Consultar auditoria | Gestor |

> Diagramas UML (casos de uso, estados, sequência), MER e mockups ficam em `docs/models/uml/`, `docs/mer/` e `docs/mockups/`.
