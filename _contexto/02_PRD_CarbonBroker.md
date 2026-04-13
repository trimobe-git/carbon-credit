# PRD — TRIMOBE CarbonBroker

## Product Requirements Document

*Versão revisada — Abril 2026*

---

## 1. Visão do Produto

O CarbonBroker não é um produto de software — é um serviço operado com IA. O "produto" consiste em cinco componentes integrados:

1. **Toolkit de IA** — agentes, prompts e workflows que permitem operar como broker de carbono com 8-12h/semana
2. **Materiais de venda** — templates e agentes que geram fichas de projeto, decks, propostas e contratos
3. **Motor de conteúdo** — sistema automatizado de curadoria, newsletter e posts LinkedIn
4. **CRM e pipeline** — sistema de gestão de leads, deals e relacionamentos no Notion
5. **Sistema de inteligência de mercado** — coleta e análise contínua de dados do mercado de carbono

---

## 2. Componente 1: Toolkit de IA (Agentes)

### 2.1 Agente de Prospecção de Cooperativas (Lado Vendedor)

**Objetivo:** identificar continuamente cooperativas com projetos de carbono e gerar outreach personalizado.

**Inputs:** cadeia produtiva (café, soja, pecuária), estado, tamanho, existência de projeto de carbono. Fontes: Google News, LinkedIn, sites de cooperativas, base CNA/OCB, Programa GHG Protocol, Iniciativa Carbono Agro.

**Processo:**
1. Busca semanal automatizada (n8n + Claude API com web search)
2. Para cada cooperativa: resumo (50 palavras), projetos identificados, contato do gestor de sustentabilidade, score de qualificação (1-5), evidência (link)
3. Outreach personalizado usando template base + contexto específico
4. Salva no Notion CRM com status "pronto para enviar"

**Score de qualificação:**

| Critério | Pontos |
|---|---|
| Projeto de carbono ativo ou certificado | +2 |
| Gestor de sustentabilidade identificado | +1 |
| Cooperativa de grande porte (200+ cooperados) | +1 |
| Exportadora (potencial CBAM) | +1 |

**Output:** 5-10 novos prospects/semana. Tempo do fundador: 30min/semana (revisão e envio).

**Prompt base:**

```
Você é um assistente de prospecção para a TRIMOBE, empresa de intermediação de créditos de carbono agrícola no Brasil.

Pesquise cooperativas brasileiras de [cadeia] que tenham mencionado projetos de carbono, sustentabilidade, créditos de carbono, GHG Protocol ou ESG nos últimos 2 anos.

Para cada cooperativa encontrada, retorne:
1. Nome da cooperativa
2. Estado / cidade
3. Site (se disponível)
4. Nome do gestor de sustentabilidade ou diretor técnico
5. LinkedIn do contato (se encontrado)
6. Projetos de carbono identificados (tipo, estágio, volume se mencionado)
7. Fonte da informação (link)
8. Score de qualificação (1-5)

Gere um outreach personalizado para LinkedIn InMail (máximo 300 caracteres):
- Referência específica ao projeto/iniciativa da cooperativa
- Proposta de valor: "Ajudo cooperativas a encontrar compradores para créditos de carbono — sem custo fixo"
- CTA: "Posso contar mais em 15 minutos?"
```

### 2.2 Agente de Prospecção de Compradores (Lado Comprador)

**Objetivo:** identificar empresas com metas ESG que busquem créditos de carbono agrícolas.

**Inputs:** setor, meta ESG pública, menção a compra de créditos, relatório de sustentabilidade. Fontes: CDP, sites de sustentabilidade, notícias, LinkedIn.

**Processo:**
1. Busca mensal (compradores mudam menos)
2. Perfil: meta declarada, tipo de crédito preferido, volume estimado, contato
3. Outreach focando em narrativa agro

**Output:** 5-10 compradores/mês adicionados ao CRM.

**Prompt base:**

```
Identifique empresas brasileiras e multinacionais com operação no Brasil que tenham metas públicas de carbono neutro ou net-zero e mencionem compra de créditos de carbono.

Priorizar setores: alimentos/bebidas, cosméticos, varejo, financeiro, tecnologia.

Para cada empresa:
1. Nome e setor
2. Meta ESG declarada
3. Link do relatório de sustentabilidade
4. Contato do head de ESG (nome + LinkedIn)
5. Evidência de interesse em créditos agrícolas
6. Volume estimado de compensação (tCO2e/ano)

Gere outreach personalizado (máximo 400 caracteres):
- Referência à meta ESG da empresa
- Oferta: créditos de carbono de cooperativas agrícolas brasileiras com rastreabilidade
- CTA: propor call de 15min
```

### 2.3 Agente de Research Pré-Call

**Objetivo:** preparar o fundador para cada call em 3 minutos em vez de 30.

**Input:** nome da cooperativa/empresa + site.

**Output:** resumo da organização (100 palavras), projetos de sustentabilidade/carbono, notícias recentes (6 meses), dores inferidas, pontos de rapport, 3 perguntas sugeridas.

### 2.4 Agente de Inteligência Pós-Call

**Objetivo:** extrair inteligência de cada conversa, automatizar follow-up, atualizar CRM.

**Input:** transcrição da call.

**Output:**
1. Resumo estruturado: dores, volume disponível, preço esperado, decisores, objeções, próximos passos, nível de urgência (1-5)
2. E-mail de follow-up personalizado
3. Atualização de CRM (campos a preencher no Notion)
4. Alertas se ação imediata é necessária

**Tempo do fundador:** 5min por call (aprovação do follow-up).

### 2.5 Agente de Matching Vendedor-Comprador

**Objetivo:** identificar os melhores compradores para cada projeto disponível.

**Input:** dados do projeto (tipo, volume, vintage, certificação, preço, localização, co-benefícios) + base de compradores do CRM.

**Output:** top 5 compradores com maior fit, justificativa para cada match, preço de mercado para comparação, sugestão de abordagem customizada.

---

## 3. Componente 2: Materiais de Venda

### 3.1 Ficha Técnica de Projeto (1 página, PDF)

| Seção | Conteúdo |
|---|---|
| Header | Nome do projeto + logo TRIMOBE |
| Localização | Estado, município, mapa |
| Cooperativa | Nome, porte, cadeia, nº de cooperados |
| Prática agrícola | ILPF, biochar, pecuária regenerativa, etc. |
| Metodologia | Verra VM0042, Gold Standard, outra |
| Volumes | Por vintage: estimado, certificado, disponível |
| Co-benefícios | Gerados pela IA (biodiversidade, solo, renda, água) |
| Preço indicativo | Faixa ou "sob consulta" |
| Contato | Dados da TRIMOBE |

**Geração:** fundador preenche 10 campos ou cola texto livre → Claude gera conteúdo → monta no template. Tempo: 20-30min (vs. 3-4h manualmente).

### 3.2 Deck de Apresentação (8-10 slides)

| # | Slide | Conteúdo |
|---|---|---|
| 1 | Capa | Nome do projeto + TRIMOBE |
| 2 | O Projeto | Descrição, localização, cooperativa |
| 3 | A Prática | Como gera créditos |
| 4 | Impacto | Co-benefícios: ambiental, social, econômico |
| 5 | Números | Volumes, área, cooperados |
| 6 | Certificação | Metodologia, status |
| 7 | Rastreabilidade | Cadeia do crédito |
| 8 | Por Que Comprar | Narrativa ESG, diferencial agro |
| 9 | Próximos Passos | Timeline, due diligence, contato |

### 3.3 Proposta Comercial

Resumo do projeto, volume e vintage, preço proposto (por tCO2e), condições de pagamento, timeline (reserva → due diligence → transferência → pagamento), termos de exclusividade, dados da TRIMOBE como intermediário.

### 3.4 Contrato de Intermediação (minuta)

Cláusulas: partes, objeto (intermediação), comissão (% sobre transação no fechamento), não-exclusividade, obrigações mútuas, prazo (6-12 meses renovável), confidencialidade, foro. Deve ser revisada por advogado antes do primeiro uso.

---

## 4. Componente 3: Motor de Conteúdo

### 4.1 Newsletter "Carbono no Agro"

| Aspecto | Definição |
|---|---|
| Frequência | Semanal (terça-feira, 10h) |
| Formato | 3-5 destaques + 1 mini-análise + CTA |
| Tamanho | 600-1.000 palavras |
| Tom | Informativo, direto, com opinião |
| Produção | IA gera rascunho, fundador revisa |
| Tempo do fundador | 45min/semana |
| Ferramenta | Resend + React Email |
| CTA principal | "Sua cooperativa tem créditos para vender? Responda este e-mail." |

**Workflow automatizado:**
- Seg-Dom: Google Alerts → n8n → Notion
- Segunda 8h: Claude classifica relevância e gera rascunho
- Terça 20h: fundador revisa, edita, publica (45min)

### 4.2 LinkedIn (3 posts/semana)

| Tipo | Frequência | Exemplo |
|---|---|---|
| Insight de mercado | 1/semana | "O SBCE começa a tomar forma. O Serpro já está desenvolvendo o sistema. O que isso muda." |
| Opinião/análise | 1/semana | "O CBAM já está em vigor. Exportadores brasileiros estão prontos?" |
| Bastidor/jornada | 1/semana | "Essa semana conversei com uma cooperativa de café que gera créditos com biochar." |

**Produção:** IA gera 3 rascunhos toda segunda. Fundador edita e agenda (20min).

---

## 5. Componente 4: CRM e Pipeline (Notion)

### 5.1 Database 1: Cooperativas (Lado Vendedor)

Campos: nome, estado, cadeia, porte, site, contato principal, LinkedIn, e-mail, telefone, projetos de carbono, volume disponível (tCO2e), certificação, score (1-5), status (Prospect → Contactado → Em conversa → Qualificado → Em negociação → Fechado → Perdido), próximo passo, data próximo passo, notas, fonte, data criação.

### 5.2 Database 2: Compradores (Lado Comprador)

Campos: empresa, setor, meta ESG, relatório de sustentabilidade (URL), contato ESG, LinkedIn, e-mail, tipo crédito preferido, volume estimado, budget estimado, status, notas.

### 5.3 Database 3: Deals (Pipeline)

Campos: nome do deal ("[Cooperativa] → [Comprador]"), cooperativa (relation), comprador (relation), projeto, volume (tCO2e), preço unitário, valor total (fórmula), comissão %, comissão estimada (fórmula), estágio (Identificado → Material gerado → Apresentado → Em negociação → Due diligence → Contrato → Fechado → Perdido), datas, próximo passo, notas.

### 5.4 Views do CRM

| View | Filtro | Uso |
|---|---|---|
| Pipeline ativo | Estágio ≠ Fechado/Perdido | Gestão semanal |
| Cooperativas para outreach | Score ≥ 3, status = Prospect | Segunda: selecionar outreaches |
| Follow-ups pendentes | Data próximo passo ≤ hoje | Diário |
| Deals fechados | Estágio = Fechado | Relatório de receita |
| Compradores ativos | Status = Interessado/Negociação | Matching com novos projetos |

---

## 6. Componente 5: As 3 Máquinas de IA

### Máquina 1 — Curadoria e Conteúdo Automático

Google Alerts (7 termos) → n8n → Claude classifica relevância → Notion "Curadoria" → Segunda 8h: Claude gera rascunho newsletter + 3 posts LinkedIn → Fundador revisa (1h/semana).

Termos de alerta: "crédito carbono Brasil", "SBCE regulamentação", "cooperativa carbono", "CBAM exportador", "inventário emissões GHG", "mercado voluntário carbono", "carbono agro café".

### Máquina 2 — Prospecção Contínua

Semanalmente: Claude + web search → identifica cooperativas → research completo → outreach personalizado → score → salva em Notion CRM → Fundador revisa e envia (30min/semana).

### Máquina 3 — Inteligência Pós-Call

Call gravada → transcrição (Whisper) → Claude extrai insights → gera follow-up + atualização CRM + alertas → Fundador aprova (5min/call).

---

## 7. Alocação de Tempo Semanal

| Bloco | Horas | Quando |
|---|---|---|
| Revisão de prospecção + outreach | 1.5h | Segunda noite |
| Newsletter + LinkedIn | 1.5h | Terça noite |
| Calls (discovery/negociação) | 2h | Quarta ou quinta |
| Materiais sob demanda | 1.5h | Quinta noite |
| Pipeline + follow-ups | 1h | Sexta noite |
| Weekly review + estratégia | 1h | Sábado manhã |
| Buffer | 1.5h | Flexível |

### O que a IA faz (fundador NÃO faz)

| Atividade | Sem IA | Com IA |
|---|---|---|
| Pesquisar cooperativas | 4h/sem | 0h (automático) |
| Redigir outreach | 2h/sem | 0h (automático) |
| Monitorar notícias | 3h/sem | 0h (automático) |
| Escrever newsletter | 3h/sem | 0h (rascunho automático) |
| Posts LinkedIn | 2h/sem | 0h (rascunho automático) |
| Transcrever/resumir calls | 1h/call | 0h (automático) |
| Follow-up | 20min/email | 0h (automático) |
| Research pré-call | 30min/prospect | 0h (automático) |
| Ficha de projeto | 3h | 20min (revisão) |
| Deck de apresentação | 4h | 30min (revisão) |
| Proposta comercial | 1h | 10min (revisão) |

**Economia: ~20h/semana automatizadas.**
