# nassauTickets

Sistema de Controle de Atendimento para um Laboratório de Análises Clínicas, desenvolvido como projeto da disciplina (UNINASSAU).

## Descrição

O nassauTickets controla a emissão, a fila, a chamada e o atendimento de senhas em um laboratório de análises clínicas. Três agentes interagem com o sistema:

- **AS (Agente Sistema):** executa as regras, emite senhas, acessa o banco de dados e atualiza o painel.
- **AA (Agente Atendente):** chama o próximo cliente, inicia e encerra o atendimento no guichê.
- **AC (Agente Cliente):** emite a senha no totem (anonimamente) e acompanha a chamada no painel.

Tipos de senha: **SP** (prioritária), **SE** (retirada de exames) e **SG** (geral). Ordem de chamada: `[SP] → [SE|SG] → [SP] → [SE|SG]`.

## Objetivo

Consolidar conhecimentos de desenvolvimento Web, React, organização de projetos, Git/GitHub e documentação, implementando um sistema de filas de atendimento com regras de negócio reais (priorização, numeração `YYMMDD-PPSQ`, expediente das 7h às 17h, painel das 5 últimas senhas, máquina de estados e relatórios).

## Tecnologias utilizadas

| Camada | Tecnologia |
|--------|------------|
| Frontend | React 19 + Vite |
| Backend | Node.js LTS 22 + Express |
| Banco de dados | MySQL 8.0 |
| Versionamento | Git / GitHub |

**Justificativa do backend:** Node.js com Express usa JavaScript em toda a aplicação (front e back), reduz a curva de aprendizado do grupo, é uma das opções suportadas pela infraestrutura do laboratório e atende bem a comunicação assíncrona via API REST/JSON.

## Arquitetura (visão geral)

```
[Totem - AC] ──┐
[Terminal AA] ─┼──> API REST (Node/Express) ──> MySQL 8.0
[Painel] ──────┘          (Agente Sistema)
```

O frontend React consome a API REST em JSON. A API concentra as regras de priorização, o controle de concorrência na chamada da próxima senha, a máquina de estados e a auditoria.

## Estrutura do repositório

```
nassauTickets/
├── backend/
├── docs/
│   ├── branding/
│   ├── mer/
│   ├── mockups/
│   ├── models/
│   │   └── uml/
│   └── requirements/
├── frontend/
├── .gitignore
├── LICENSE
└── README.md
```

## Instalação

Pré-requisitos: Node.js 22 LTS e npm.

```bash
git clone https://github.com/<usuario>/nassauTickets.git
cd nassauTickets/frontend
npm install
```

## Execução

```bash
cd frontend
npm run dev
```

A aplicação ficará disponível em `http://localhost:5173`.

## Configuração

Variáveis de ambiente (serão documentadas em `.env.example` conforme o backend evoluir):

- `VITE_API_URL`: URL base da API (frontend)
- `DB_HOST`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`: conexão com o MySQL (backend)

## Branches

- `main`: versão estável, recebe o merge da `dev`.
- `dev`: branch de desenvolvimento; todo código é enviado primeiro para ela.

Padrão de commits: `chore:`, `docs:`, `feat:`, `fix:`.

## Membros

## Membros

| Nome | Matrícula | Papel |
|------|-----------|-------|
| Tarsílio Aureliano Soares Silva | 01803880 | Scrum Master |
| Tarsílio Aureliano Soares Silva | 01803880 | Documentador |
| Tarsílio Aureliano Soares Silva | 01803880 | Desenvolvedor |
| Tarsílio Aureliano Soares Silva | 01803880 | Testador |

> Papéis: **Scrum Master** (apenas um), **Documentador**, **Desenvolvedor** e **Testador** (podem ser vários).

## Licença

Distribuído sob a licença MIT. Veja o arquivo [LICENSE](LICENSE).
