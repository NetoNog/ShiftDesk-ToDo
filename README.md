<div align="center">

# ⚡ ShiftDesk v3

**Sistema de Gestão de Turnos NOC/SOC**

Uma aplicação React completa para equipes de operação gerenciarem atividades em tempo real, com sistema de alertas, recorrência semanal e painel de relatórios analíticos.

---

## 📋 Sobre o Projeto

O **ShiftDesk** é um sistema de gestão de tarefas projetado para ambientes de operação contínua (NOC/SOC, data centers, central de monitoramento), onde equipes trabalham em turnos de 12 horas e precisam rastrear atividades com controle rigoroso de prazo.

A aplicação roda **100% no navegador**, sem servidor ou banco de dados — os dados são persistidos no `localStorage` da máquina. Ideal para uso em terminais dedicados de sala de operações.

### Problema que resolve

Em ambientes NOC/SOC, é comum perder o controle de atividades em andamento — especialmente na virada de turno. O ShiftDesk oferece visibilidade em tempo real do status de cada tarefa, alerta automaticamente quando os prazos estão vencendo e mantém um histórico auditável com justificativas de encerramento.

---

## 🖥️ Screenshots

> **Tela Principal — Visão de Tarefas**
> Cards com barra de progresso em tempo real, badges de status e ticker de alertas no topo.

> **Login — Seleção de Operador**
> Lista de perfis cadastrados com avatar colorido gerado a partir do nome.

> **Painel de Relatórios — Dashboard**
> 10 KPIs, comparativo Diurno/Noturno e 7 gráficos interativos.

> **Histórico**
> Log pesquisável de todas as tarefas encerradas com filtros e exportação CSV.

---

## ✨ Funcionalidades

### 👤 Sistema de Operadores
- Login por **seleção de perfil** — sem senha, sem conta
- Múltiplos perfis cadastrados na mesma máquina (persiste entre sessões)
- **Avatar determinístico**: cor gerada automaticamente a partir da inicial do nome
- Iniciais calculadas (ex: "João Silva" → "JS") exibidas no avatar e no header
- Botão de troca de operador sem apagar os perfis existentes
- Validação: exige nome + sobrenome para criar perfil

### 📋 Gestão de Tarefas
- Criar tarefas com título, objetivo, duração, turno e participantes
- Durações predefinidas (30min, 1h, 2h, 4h, 6h, 8h) ou valor manual em minutos
- Seleção de turno manual (☀ Diurno / ☾ Noturno) com detecção automática por hora
- Encerramento obrigatório com **justificativa** — nenhuma tarefa é deletada silenciosamente
- Renovação de ciclo com **informativo** descritivo obrigatório
- Histórico completo de ciclos acessível pelo modal de detalhes
- Sem exclusão direta — toda ação fica registrada no log

### 🚨 Sistema de Alertas (3 níveis)

| Nível | Gatilho | Comportamento |
|---|---|---|
| **Normal** | < 75% do tempo | Barra verde, sem destaque |
| **◆ Alerta** | 75% – 99% | Pulso âmbar, badge no header |
| **● Crítico** | ≥ 100% (vencido) | Pulso vermelho com glow, badge piscando, notificação Teams |

- **Ticker scrolling** no topo listando todas as tarefas em alerta/crítico
- **Toast notifications** no canto superior direito com animação de entrada/saída
- Polling automático a cada **20 segundos**

### 🔄 Recorrência Semanal
- Qualquer tarefa pode ser marcada como recorrente
- Configuração por **dias da semana** (multi-seleção) e **horário de início** (HH:MM)
- Motor de verificação: a cada 20s checa se alguma tarefa recorrente precisa ser reiniciada
- Controle anti-duplicidade via `ultimoSpawn` — garante no máximo 1 spawn por dia
- Preview do resumo de recorrência antes de salvar

### 📊 Relatórios e Analytics

**Dashboard — 10 KPIs:**
- Total encerradas (com breakdown Diurno/Noturno)
- Tarefas ativas agora
- Contagem de críticas e em alerta
- Tarefas recorrentes ativas
- Tempo médio por turno (☀ e ☾ separados)
- Taxa de renovação e total de ciclos
- Percentual no prazo

**Gráficos:**
- Área: encerradas por dia (últimos 14 dias)
- Barras empilhadas: Diurno vs Noturno por dia (últimos 7 dias)
- Pizza: distribuição por turno
- Pizza: eficiência (no prazo vs renovadas)
- Barras por hora: pico de atividade nas 24h
- Barras horizontais: ranking de criadores e encerradores
- Radar: participação relativa da equipe

**Histórico:**
- Busca em tempo real por título, objetivo ou justificativa
- Filtros por turno, operador e ordenação (recente / antigo / maior duração)
- Expansão inline com justificativa e ciclos registrados
- **Exportação CSV** com todos os campos

### 🔔 Integração Microsoft Teams
- Notificação automática ao webhook configurado quando uma tarefa atinge nível Crítico
- Configura via variável de ambiente `VITE_TEAMS_WEBHOOK_URL`
- Mensagem formatada com título, operador, turno e objetivo

---

## 🛠️ Stack Técnica

| Tecnologia | Versão | Função |
|---|---|---|
| **React** | 18.3 | UI e gerenciamento de estado |
| **Vite** | 5.4 | Build tool e dev server |
| **Recharts** | 2.12 | Gráficos interativos (7 tipos) |
| **Inter** | — | Tipografia de corpo (Google Fonts) |
| **JetBrains Mono** | — | Tipografia de dados e código |
| **localStorage** | — | Persistência local (sem backend) |

**Sem dependências de backend.** Sem banco de dados. Sem autenticação real. Tudo roda no browser.

---

## 🚀 Instalação e Uso

### Pré-requisitos
- Node.js 18+ 
- npm ou yarn

### Instalação

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/shiftdesk.git
cd shiftdesk

# Instale as dependências
npm install

# Inicie o servidor de desenvolvimento
npm run dev
```

A aplicação estará disponível em `http://localhost:5173`.

### Build para produção

```bash
npm run build
# Arquivos gerados em /dist — sirva com qualquer servidor estático
```

### Variáveis de Ambiente (opcional)

Copie `.env.example` para `.env` e preencha:

```env
# Webhook do Microsoft Teams para notificações de tarefas críticas
VITE_TEAMS_WEBHOOK_URL=https://outlook.office.com/webhook/SEU_WEBHOOK_AQUI
```

Se `VITE_TEAMS_WEBHOOK_URL` não estiver configurado, a integração Teams é silenciada — apenas o toast interno é exibido.

---

## 📁 Estrutura do Projeto

```
shiftdesk/
├── index.html                         # Entry point HTML
├── vite.config.js                     # Configuração Vite + alias @/ para src/
├── package.json
├── .env.example                       # Template de variáveis de ambiente
├── .gitignore
│
├── public/
│   └── favicon.svg                    # Ícone ⚡ com gradiente azul/índigo
│
└── src/
    ├── main.jsx                       # Bootstrap — ReactDOM.createRoot
    ├── App.jsx                        # Componente raiz — orquestrador de toda a aplicação
    │
    ├── constants/
    │   └── index.js                   # SK_* (storage keys), STATUS, DIAS_SEMANA, TURNOS,
    │                                  # PALETTE, AVATAR_COLORS, getCurrentTurno()
    │
    ├── styles/
    │   ├── design-tokens.js           # Injeta Google Fonts + todas as CSS custom properties
    │   │                              # + keyframes de animação no <head> via JS
    │   └── shared.js                  # Objetos de estilo inline compartilhados:
    │                                  # iS (input), bP (botão primário), bGh (ghost),
    │                                  # mBase/mBody/mFoot (modal)
    │
    ├── utils/
    │   ├── storage.js                 # load/save localStorage genérico
    │   │                              # loadOps/saveOps para perfis de operadores
    │   ├── time.js                    # nowISO, minsElapsed, minsToH, fmtTs, fmtDay, durMin
    │   ├── task.js                    # genId, severity (ok/warn/critical),
    │   │                              # getInitials, getAvatarColors, buildOperator
    │   ├── analytics.js               # buildAnalytics(log, tasks) → todos os KPIs e
    │   │                              # datasets para os gráficos do ReportsPanel
    │   ├── recurrence.js              # checkWeeklyRecurrence(tasks, setTasks)
    │   │                              # Motor de spawn automático de tarefas recorrentes
    │   └── notifications.js           # pushNotif(msg, type) — API imperativa global
    │                                  # notifyTeams(task) — dispara webhook + toast
    │                                  # _registerNotifSetter / _unregisterNotifSetter
    │
    ├── hooks/
    │   ├── useClock.js                # Retorna Date atualizado a cada 1 segundo
    │   ├── useOperator.js             # Gerencia sessão do operador ativo (login/logout)
    │   │                              # Persiste em SK_USER como JSON
    │   └── useTasks.js                # Estado central de tarefas:
    │                                  # checkOverdue, createTask, closeTask, renewTask
    │
    └── components/
        ├── ui/
        │   ├── LoginScreen.jsx        # Tela de seleção/criação de perfil de operador
        │   │                          # Duas sub-telas: "select" e "new"
        │   │                          # Preview de avatar em tempo real durante digitação
        │   ├── NotifStack.jsx         # Stack de toasts no canto superior direito
        │   │                          # Registra-se como receptor global de pushNotif()
        │   └── primitives.jsx         # Overlay, ModalHeader, InfoBox, Field,
        │                              # ChartBox, EmptyState — componentes atômicos
        │
        ├── tasks/
        │   └── TaskCard.jsx           # Card de tarefa ativa
        │                              # Atualiza a cada 10s via setInterval interno
        │                              # Barra de progresso animada, 3 estados visuais
        │                              # Botões: Detalhes / Renovar / Encerrar
        │
        ├── modals/
        │   └── index.jsx              # Todos os modais da aplicação:
        │                              # CreateModal — formulário de nova tarefa + recorrência
        │                              # CloseModal  — encerramento com justificativa
        │                              # RenewModal  — renovação com informativo + nova duração
        │                              # DetailModal — visualização completa + histórico de ciclos
        │
        └── reports/
            ├── index.js               # Re-export de ReportsPanel
            └── ReportsPanel.jsx       # Painel de relatórios completo:
                                       # Tab Dashboard: KPIs + banner Diurno/Noturno + 7 gráficos
                                       # Tab Histórico: tabela filtrável + expansão inline + CSV
```

---

## 🗄️ Modelo de Dados

### Tarefa Ativa (`sdv3_tasks`)

```json
{
  "id": "lx4k2abc9",
  "titulo": "Cobrar Link Operadora VIVO — Amaggi Portuchuelo",
  "objetivo": "Verificar status do link e acionar o NOC da operadora caso não haja retorno",
  "duracaoMin": 120,
  "turno": "Diurno",
  "participantes": ["João Silva", "Maria Souza"],
  "recorrente": false,
  "diasSemana": [],
  "horarioMin": 480,
  "criadoPor": "João Silva",
  "criadoEm": "2025-03-11T08:30:00.000Z",
  "inicioCicloAtual": "2025-03-11T08:30:00.000Z",
  "status": "ativo",
  "ciclos": [],
  "ultimoSpawn": null
}
```

**Status possíveis:** `ativo` | `atrasado` | `critico`

### Entrada de Log (`sdv3_log`)

```json
{
  "id": "lx4k2abc9",
  "title": "Cobrar Link Operadora VIVO — Amaggi Portuchuelo",
  "objetivo": "Verificar status do link...",
  "criadoPor": "João Silva",
  "encerradoPor": "Maria Souza",
  "inicio": "2025-03-11T08:30:00.000Z",
  "fim": "2025-03-11T10:45:00.000Z",
  "justificativa": "Link restabelecido após acionamento do NOC VIVO. Ticket #123456.",
  "historico": "[{...ciclos anteriores...}]",
  "turno": "Diurno"
}
```

### Ciclo de Renovação (dentro de `historico`)

```json
{
  "inicio": "2025-03-11T08:30:00.000Z",
  "fim": "2025-03-11T10:30:00.000Z",
  "realizadoPor": "João Silva",
  "informativo": "Aguardando retorno do NOC VIVO. Ticket aberto.",
  "duracaoMin": 120
}
```

### Perfil de Operador (`sdv3_operators`)

```json
[
  {
    "nome": "João Silva",
    "iniciais": "JS",
    "avatarGrad": "linear-gradient(135deg,#3b82f6,#1d4ed8)"
  }
]
```

---

## 🎨 Design System

O ShiftDesk usa um tema **dark profissional** inspirado em ambientes NOC/SOC reais.

### Paleta de Cores

| Token | Valor | Uso |
|---|---|---|
| `--bg` | `#060810` | Fundo principal |
| `--surface` | `#0c1120` | Cards e painéis |
| `--accent` | `#3b82f6` | Azul principal (ações, links) |
| `--green` | `#22c55e` | Status normal / sucesso |
| `--amber` | `#f59e0b` | Alerta / Turno Diurno |
| `--red` | `#ef4444` | Crítico / erro |
| `--purple` | `#a855f7` | Recorrência |
| `--noturno` | `#818cf8` | Turno Noturno |

### Tipografia
- **Inter** — texto de corpo, labels, botões
- **JetBrains Mono** — timestamps, valores numéricos, badges de status

### Animações
- `criticalPulse` — pulso vermelho com glow para tarefas críticas
- `warnPulse` — pulso âmbar para tarefas em alerta
- `badgeBlink` — piscar do badge de crítico no header
- `tickerScroll` — rolagem horizontal do ticker de alertas
- `notifIn / notifOut` — entrada/saída dos toasts
- `fadeUp / modalIn` — animações de entrada de telas e modais

---

## ⚙️ Como Funciona Internamente

### Ciclo de Vida de uma Tarefa

```
Criação → [status: ativo]
    ↓ (a cada 20s, polling)
75% do tempo  → [status: atrasado] + toast "Atenção"
100% do tempo → [status: critico]  + toast "TEAMS" + webhook
    ↓
Renovar ciclo → [status: ativo, novo inicioCicloAtual, ciclo salvo]
    ↓
Encerrar      → removida de tasks[], entrada criada em log[]
```

### Motor de Recorrência

A cada 20 segundos, `checkWeeklyRecurrence()` percorre todas as tarefas com `recorrente: true` e verifica:

1. O dia da semana atual está em `diasSemana[]`?
2. O horário atual (em minutos) ≥ `horarioMin`?
3. `ultimoSpawn` é de um dia diferente de hoje?

Se todas forem verdadeiras, o ciclo é reiniciado e `ultimoSpawn` é atualizado para hoje.

### Sistema de Notificações

`pushNotif()` é uma função imperativa global que funciona fora da árvore React. O componente `NotifStack` registra seu `setState` numa variável de módulo ao montar, permitindo que qualquer `util` ou `hook` dispare toasts sem precisar de prop drilling ou context.

---

## 🔑 localStorage Keys

| Chave | Tipo | Conteúdo |
|---|---|---|
| `sdv3_tasks` | `Array<Task>` | Tarefas ativas no momento |
| `sdv3_log` | `Array<LogEntry>` | Histórico de tarefas encerradas |
| `sdv3_user` | `Operator (JSON)` | Operador da sessão atual |
| `sdv3_operators` | `Array<Operator>` | Todos os perfis cadastrados |

> **Nota:** Limpar o `localStorage` reseta completamente a aplicação. Exporte o CSV antes se precisar preservar o histórico.

---

## 📦 Scripts Disponíveis

```bash
npm run dev      # Servidor de desenvolvimento (http://localhost:5173)
npm run build    # Build de produção em /dist
npm run preview  # Preview do build de produção
npm run lint     # Lint com ESLint
```

---

## 🤝 Contribuindo

1. Fork o repositório
2. Crie uma branch para sua feature: `git checkout -b feature/minha-feature`
3. Commit suas mudanças: `git commit -m 'feat: adiciona minha feature'`
4. Push para a branch: `git push origin feature/minha-feature`
5. Abra um Pull Request

### Possíveis Melhorias (roadmap informal)
- [ ] Exportação para PDF
- [ ] Modo de passagem de turno (snapshot do estado ao encerrar turno)
- [ ] Suporte a múltiplas salas/equipes com dados compartilhados (via backend simples ou Firebase)
- [ ] Notificações por e-mail além do Teams
- [ ] Modo dark/light alternável
- [ ] PWA para instalação no desktop

---

## 📄 Licença

MIT — use livremente, atribua se quiser.

---

<div align="center">

Desenvolvido para equipes de operação que precisam de controle, visibilidade e histórico — sem burocracia.

**⚡ ShiftDesk v3**

</div>
