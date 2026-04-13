# Estratégia de Tecnologia e IA: TRIMOBE CarbonBroker

## Aceleração e Automação com IA para One Person Business

*Abril 2026*

---

## 1. Filosofia: IA como Multiplicador de Capacidade

O CarbonBroker é viável como one person business porque IA e automação comprimem ~30h/semana de trabalho operacional em ~4h de revisão e decisão. O fundador faz apenas o que exige julgamento humano: calls, decisões estratégicas e aprovações.

**Princípio central:** a IA prepara, o humano decide. Nenhum output crítico (proposta, contrato, e-mail para cliente) sai sem revisão humana. Mas o tempo entre "preciso de um material" e "material pronto para revisão" cai de horas para minutos.

---

## 2. Stack de IA e Tecnologia

### 2.1 IA Generativa — Cérebro das Operações

| Ferramenta | Uso principal | Custo |
|---|---|---|
| **Claude API (Sonnet 4)** | Agentes de prospecção, geração de materiais, análise pós-call, matching, newsletter | ~R$ 150-300/mês |
| **Claude Code** | Desenvolvimento e manutenção de automações, scripts, landing page, templates de e-mail | Incluso na API |
| **Gemini API** | Segundo modelo para validação cruzada, web search avançado, processamento de PDFs longos | Free tier / ~R$ 50/mês |
| **Whisper API (OpenAI)** | Transcrição de calls | ~R$ 20/mês |

**Por que Claude como IA principal:**
- Qualidade superior em texto longo e estruturado (materiais de venda, propostas)
- System prompts robustos para agentes especializados
- Claude Code para desenvolvimento rápido de automações
- API com bom custo-benefício para volume moderado

**Por que Gemini como complementar:**
- Google Search integrado (prospecção web)
- Processamento de documentos grandes (relatórios de sustentabilidade)
- Free tier generoso para validação cruzada

### 2.2 Automação — Motor Operacional

| Ferramenta | Uso | Custo |
|---|---|---|
| **n8n** (self-hosted) | Orquestração de workflows: coleta de alertas → classificação → Notion | Free (self-hosted no Railway/Render) |
| **Notion** | CRM, pipeline de conteúdo, base de conhecimento, hub operacional | Free |
| **Resend** | Envio de newsletter + e-mails transacionais | Free tier (3.000/mês) |
| **Buffer** | Agendamento de posts LinkedIn | Free tier (3 canais) |

### 2.3 Desenvolvimento e Infraestrutura

| Ferramenta | Uso | Custo |
|---|---|---|
| **Claude Code** | Desenvolvimento de scripts, automações, landing page, templates | Incluso |
| **Vercel** | Hospedagem da landing page (Next.js) | Free tier |
| **Railway/Render** | Hospedagem do n8n self-hosted | Free tier / ~R$ 30/mês |
| **GitHub** | Repositório de código e prompts | Free |

### 2.4 Produtividade e Design

| Ferramenta | Uso | Custo |
|---|---|---|
| **Canva** | Templates de fichas de projeto, decks | Free |
| **Google Slides** | Decks de apresentação | Free |
| **Zoom/Google Meet** | Calls de discovery e negociação | Free |

---

## 3. Automações Detalhadas (As 3 Máquinas)

### 3.1 Máquina 1: Curadoria e Conteúdo Automático

**Implementação técnica:**

```
[Google Alerts - 7 termos]
    │ RSS/e-mail, diariamente
    ▼
[n8n Workflow 1]
    │ Parse e-mails/RSS → extrai links e títulos
    ▼
[Claude API - Classificação]
    │ System prompt: classificar relevância 1-10
    │ Gerar resumo de 2 linhas
    │ Categorizar (regulação/mercado/agro/deal/tech)
    ▼
[Notion - Database "Curadoria"]
    │ Armazena com metadata
    ▼
[n8n Workflow 2 - Segunda 8h, cron]
    │ Busca top 5 da semana (score ≥ 7)
    ▼
[Claude API - Geração]
    │ Gera rascunho newsletter completo
    │ Gera 3 rascunhos de posts LinkedIn
    ▼
[Notion - "Pipeline de Conteúdo"]
    │ Status: "Aguardando revisão"
    ▼
[Fundador revisa - 1h/semana]
    │ Edita → publica newsletter (Resend)
    │ Agenda posts (Buffer)
```

**Termos de alerta:** "crédito carbono Brasil", "SBCE regulamentação", "cooperativa carbono", "CBAM exportador", "inventário emissões GHG", "mercado voluntário carbono", "carbono agro café".

**Como construir com Claude Code:**
1. Script Python para parsear Google Alerts (RSS ou e-mail IMAP)
2. Integração com Claude API para classificação batch
3. Notion API para inserção automática
4. Script de geração semanal (cron no n8n)
5. Template React Email para newsletter (gerenciado via Resend)

### 3.2 Máquina 2: Prospecção Contínua

**Implementação técnica:**

```
[n8n Cron - Semanal]
    │
    ▼
[Claude API + Web Search]
    │ Busca novas cooperativas com projetos de carbono
    │ Cruza com base existente (Notion) para evitar duplicatas
    ▼
[Para cada novo prospect:]
    │
    ├─ Claude API → Research completo
    │   - Resumo da organização
    │   - Projetos de carbono identificados
    │   - Contato do gestor de sustentabilidade
    │   - Score de qualificação (1-5)
    │
    ├─ Claude API → Outreach personalizado
    │   - LinkedIn InMail (300 chars)
    │   - E-mail alternativo (400 chars)
    │
    └─ Notion API → Criar entrada no CRM
        - Status: "Pronto para enviar"
        - Todos os campos preenchidos
```

**Como construir com Claude Code:**
1. Script que usa Claude com tool_use (web_search) para prospecção
2. Deduplicação contra base Notion existente
3. Geração de outreach com few-shot examples de mensagens que funcionaram
4. Inserção automática no Notion via API

### 3.3 Máquina 3: Inteligência Pós-Call

**Implementação técnica:**

```
[Call gravada (Zoom/Meet)]
    │
    ▼
[Whisper API → Transcrição]
    │
    ▼
[Claude API → Extração estruturada]
    │ Output JSON:
    │ {
    │   "resumo": "...",
    │   "dores": ["..."],
    │   "volume_creditos": "...",
    │   "preco_esperado": "...",
    │   "decisores": ["..."],
    │   "objecoes": ["..."],
    │   "proximos_passos": ["..."],
    │   "urgencia": 4,
    │   "follow_up_email": "...",
    │   "crm_update": {...}
    │ }
    ▼
[n8n → Notion API]
    │ Atualiza CRM automaticamente
    │ Cria follow-up como tarefa pendente
    ▼
[Fundador revisa - 5min]
    │ Aprova e envia follow-up
```

**Como construir com Claude Code:**
1. Script para download de gravação (Zoom API ou manual)
2. Whisper API para transcrição
3. Claude API com structured output (JSON) para extração
4. Notion API para atualização automática do CRM
5. Draft de e-mail salvo em rascunhos (Resend ou Gmail)

---

## 4. Uso de Claude Code para Implementação Acelerada

Claude Code é o acelerador principal na fase de setup. Com conhecimento avançado em desenvolvimento, o fundador pode usar Claude Code para:

### 4.1 Semana 1: Setup técnico

| Tarefa | Estimativa com Claude Code |
|---|---|
| Landing page Next.js + Tailwind + Vercel | 2-3h (Claude Code gera, fundador ajusta) |
| Templates React Email para newsletter | 1h |
| Setup Notion (databases, relations, views) | 1h (manual, mas Claude Code gera scripts de API) |
| Scripts de integração n8n ↔ Claude API ↔ Notion | 2-3h |
| Script de transcrição (Whisper) | 30min |
| Script de classificação de notícias | 1h |
| Prompts base dos 5 agentes (testados) | 2h |

**Total setup técnico: ~10-12h** (vs. ~30-40h sem Claude Code).

### 4.2 Manutenção contínua

- Ajustar prompts com base nos resultados (Claude Code para iterar rápido)
- Adicionar novos workflows no n8n conforme necessidade
- Debugar automações que falham
- Gerar novos templates de materiais
- Evoluir a landing page conforme tração

---

## 5. Uso de MCPs (Model Context Protocol)

MCPs permitem conectar Claude a ferramentas externas de forma nativa. Aplicações para o CarbonBroker:

| MCP | Uso |
|---|---|
| **Gmail MCP** | Ler/enviar e-mails de follow-up direto do Claude |
| **Google Calendar MCP** | Agendar calls e follow-ups |
| **Notion MCP** (quando disponível) | Claude lê/atualiza CRM diretamente |
| **Web Search** (nativo Claude) | Prospecção e research em tempo real |

**Workflow com MCPs:** em uma sessão de Claude, o fundador pode dizer "pesquise novas cooperativas de café com projetos de carbono, adicione ao CRM no Notion, e prepare outreaches" — e Claude executa tudo via MCPs, sem scripts intermediários.

---

## 6. Uso de Notion como Hub Operacional

Notion é o sistema nervoso central do CarbonBroker:

| Função | Database/Page |
|---|---|
| CRM vendedores | Database "Cooperativas" |
| CRM compradores | Database "Compradores" |
| Pipeline de deals | Database "Deals" |
| Curadoria de notícias | Database "Curadoria" |
| Pipeline de conteúdo | Database "Conteúdo" |
| Templates de prompts | Page "Prompts Base" |
| Métricas e KPIs | Database "Métricas" com views de dashboard |
| Base de conhecimento | Page com links, documentos, aprendizados |
| Tarefas semanais | Database "Tarefas" com due dates |

**Automações Notion:**
- Views filtradas para rotina semanal (follow-ups pendentes, outreaches prontos, etc.)
- Fórmulas para calcular comissão estimada nos deals
- Relations entre cooperativas ↔ deals ↔ compradores
- Rollups para métricas agregadas

---

## 7. Estratégia de Evolução Técnica

### Mês 1-3: MVP técnico
- Automações manuais ou semi-automáticas (scripts rodados localmente)
- Claude via chat para tarefas sob demanda
- n8n com workflows básicos (alertas → Notion)
- Landing page estática

### Mês 3-6: Automação robusta
- Todas as 3 máquinas rodando automaticamente
- MCPs integrados para workflow mais fluido
- Métricas sendo coletadas automaticamente
- Templates de materiais refinados com base no feedback

### Mês 6-12: Inteligência acumulada
- Dashboard de mercado com dados históricos de preços e volumes
- Modelo preditivo simples de matching (qual comprador para qual projeto)
- Base de conhecimento enriquecida com insights de calls
- Avaliar se há demanda para transformar em produto (CarbonAgro SaaS)

---

## 8. Custos Técnicos Consolidados

| Categoria | Custo mensal |
|---|---|
| Claude API (principal) | R$ 150-300 |
| Gemini API (complementar) | R$ 0-50 |
| Whisper API (transcrição) | R$ 20 |
| Hospedagem (Railway/Render para n8n) | R$ 0-30 |
| Domínio + DNS | R$ 30 |
| Tudo mais (Notion, Vercel, Buffer, Resend, Canva) | R$ 0 (free tiers) |
| **Total** | **R$ 200-430/mês** |

Um único deal (R$ 15.000-40.000) cobre 3-16 anos de custos técnicos.

---

## 9. Segurança e Privacidade

| Aspecto | Abordagem |
|---|---|
| Dados de cooperativas/compradores | Armazenados no Notion (conta pessoal, acesso único) |
| Transcrições de calls | Processadas via API e descartadas; apenas resumo fica no CRM |
| Informações financeiras de deals | Nunca expostas publicamente; dados anonimizados na newsletter |
| Contratos e minutas | Revisados por advogado; IA gera rascunho, humano valida |
| Dados sensíveis na API | APIs da Anthropic e OpenAI não treinam com dados da API |
