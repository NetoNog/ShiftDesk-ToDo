# ⚡ ShiftDesk v3

Sistema NOC/SOC de gestão de atividades por turno — construído em React.

---

## Estrutura do Projeto

```
shiftdesk/
├── index.html                        # Entry point HTML
├── vite.config.js                    # Configuração Vite + alias @/
├── package.json                      # Dependências e scripts
├── .env.example                      # Variáveis de ambiente (Teams webhook)
├── .gitignore
│
├── public/
│   └── favicon.svg                   # Ícone ⚡ com gradiente azul/índigo
│
└── src/
    ├── main.jsx                      # ReactDOM.createRoot
    ├── App.jsx                       # Componente raiz — orquestrador geral
    │
    ├── constants/
    │   └── index.js                  # Storage keys, STATUS, DIAS_SEMANA, TURNOS, PALETTE
    │
    ├── styles/
    │   ├── design-tokens.js          # Injeta Google Fonts + CSS variables + keyframes
    │   └── shared.js                 # Objetos de estilo inline reutilizáveis (iS, bP, bGh, mBase…)
    │
    ├── utils/
    │   ├── storage.js                # load/save localStorage, loadOps/saveOps
    │   ├── time.js                   # nowISO, minsElapsed, minsToH, fmtTs, fmtDay, durMin
    │   ├── task.js                   # genId, severity, getInitials, buildOperator, avatarColors
    │   ├── analytics.js              # buildAnalytics() — deriva todos os KPIs e dados de gráficos
    │   ├── recurrence.js             # checkWeeklyRecurrence() — motor de spawn semanal
    │   └── notifications.js          # pushNotif(), notifyTeams(), registro do setter global
    │
    ├── hooks/
    │   ├── useClock.js               # Relógio em tempo real (atualiza a cada 1s)
    │   ├── useOperator.js            # Sessão do operador ativo (login/logout)
    │   └── useTasks.js               # Estado de tarefas + checkOverdue + CRUD actions
    │
    └── components/
        ├── ui/
        │   ├── LoginScreen.jsx       # Seleção de perfil + criação de novo operador
        │   ├── NotifStack.jsx        # Toast notifications (canto superior direito)
        │   └── primitives.jsx        # Overlay, ModalHeader, InfoBox, Field, ChartBox, EmptyState
        │
        ├── tasks/
        │   └── TaskCard.jsx          # Card de tarefa ativa com barra de progresso e alertas
        │
        ├── modals/
        │   └── index.jsx             # CreateModal, CloseModal, RenewModal, DetailModal
        │
        └── reports/
            ├── index.js              # Re-export de ReportsPanel
            └── ReportsPanel.jsx      # Dashboard KPIs + gráficos Recharts + Histórico filtrado
```

---

## Funcionalidades

### Gestão de Tarefas
- Criar tarefas com título, objetivo, duração, turno e participantes
- Monitoramento em tempo real com barra de progresso
- Sistema de alertas em 3 níveis: Normal → Alerta (75%) → Crítico (100%)
- Encerramento com justificativa obrigatória
- Renovação de ciclo com informativo obrigatório

### Recorrência Semanal
- Tarefas podem ser configuradas para reiniciar automaticamente
- Configura dias da semana e horário de início
- Motor de verificação a cada 20 segundos

### Operadores
- Login por seleção de perfil (sem senha)
- Perfis salvos no `localStorage` com avatar colorido determinístico
- Múltiplos operadores cadastrados na mesma máquina

### Relatórios
- Dashboard com 10 KPIs + banner comparativo Diurno vs Noturno
- Gráficos: área, barras empilhadas, pizza, barras horizontais, radar
- Histórico pesquisável com filtros por turno, operador e ordenação
- Exportação CSV

### Notificações
- Toast interno para alertas de tarefa
- Ticker scrolling no topo listando tarefas em alerta/crítico
- Integração Microsoft Teams via webhook (configurar `VITE_TEAMS_WEBHOOK_URL`)

---

## Instalação

```bash
npm install
npm run dev
```

## Variáveis de Ambiente

Copie `.env.example` para `.env` e preencha:

```env
VITE_TEAMS_WEBHOOK_URL=https://outlook.office.com/webhook/SEU_WEBHOOK_AQUI
```

---

## Stack

| Tecnologia | Uso |
|---|---|
| React 18 | UI e estado |
| Recharts | Gráficos e visualizações |
| Vite | Build e dev server |
| localStorage | Persistência local (sem backend) |
| Inter + JetBrains Mono | Tipografia |

---

## localStorage Keys

| Chave | Conteúdo |
|---|---|
| `sdv3_tasks` | Array de tarefas ativas |
| `sdv3_log` | Array de tarefas encerradas |
| `sdv3_user` | Operador da sessão atual (JSON) |
| `sdv3_operators` | Lista de perfis cadastrados (JSON) |
