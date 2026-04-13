# Guia de Tarefas de Implementação: TRIMOBE CarbonBroker

## Execução Sequencial — One Person Business

*Abril 2026*

---

## Fase 0: Setup Técnico e Presença Digital

### Semana 1 — Fim de semana intenso (8-10h)

**Bloco A: Identidade e presença (2h)**

- [ ] 1. Configurar e-mail contato@trimobe.com (Zoho Mail free ou Google Workspace) — 15min
- [ ] 2. Configurar DNS do domínio trimobe.com — 15min
- [ ] 3. Criar Company Page TRIMOBE no LinkedIn — 30min
- [ ] 4. Otimizar perfil pessoal no LinkedIn: headline "Intermediação de créditos de carbono agrícola | TRIMOBE", about com proposta de valor, experiência TRIMOBE — 45min
- [ ] 5. Configurar assinatura de e-mail com link para newsletter — 15min

**Bloco B: Hub operacional (3h)**

- [ ] 6. Criar workspace Notion dedicado ao CarbonBroker — 15min
- [ ] 7. Criar Database "Cooperativas" com todos os campos definidos no PRD — 30min
- [ ] 8. Criar Database "Compradores" com todos os campos — 20min
- [ ] 9. Criar Database "Deals" com relations para Cooperativas e Compradores, fórmulas de comissão — 30min
- [ ] 10. Criar Database "Curadoria" (notícias classificadas) — 15min
- [ ] 11. Criar Database "Conteúdo" (pipeline de newsletter e posts) — 15min
- [ ] 12. Criar Views do CRM: Pipeline ativo, Cooperativas para outreach, Follow-ups pendentes, Deals fechados — 30min
- [ ] 13. Criar Page "Prompts Base" com os 5 prompts dos agentes de IA — 30min

**Bloco C: Landing page + newsletter (2h)**

- [ ] 14. Usar Claude Code para gerar landing page Next.js + Tailwind — 1h
  - Hero: tagline + formulário de captura de e-mail
  - Seção: o que fazemos (3 bullets)
  - Seção: para quem (cooperativas + compradores)
  - Footer: LinkedIn + contato
- [ ] 15. Deploy no Vercel + conectar domínio trimobe.com — 15min
- [ ] 16. Configurar Resend: verificar domínio, criar API key — 15min
- [ ] 17. Criar template de e-mail da newsletter com React Email (via Claude Code) — 30min

**Bloco D: Primeiro mapeamento (2h)**

- [ ] 18. Usar Claude (chat) para mapear 15 cooperativas de café com projetos de carbono — 45min
  - Inserir cada uma no Notion CRM com todos os campos preenchidos
- [ ] 19. Usar Claude para mapear 15 cooperativas de pecuária/soja — 45min
- [ ] 20. Usar Claude para mapear 20 potenciais compradores (empresas com metas ESG) — 30min

---

### Semana 2 (10h)

**Bloco A: Automações de IA (3h)**

- [ ] 21. Configurar Google Alerts para os 7 termos de monitoramento — 15min
- [ ] 22. Usar Claude Code para criar script de classificação de notícias (Claude API + Notion API) — 1h
  - Input: lista de links/títulos
  - Output: score de relevância + resumo + inserção no Notion
- [ ] 23. Usar Claude Code para criar script de geração semanal de newsletter (Claude API) — 1h
  - Input: top 5 notícias da semana do Notion
  - Output: rascunho completo em markdown
- [ ] 24. Configurar n8n: workflow Google Alerts → parse → script de classificação → Notion — 45min

**Bloco B: Conteúdo inaugural (3h)**

- [ ] 25. Escrever Newsletter #1 manualmente (primeira edição deve ser pessoal e autêntica) — 1.5h
  - Tema sugerido: "O mercado de carbono brasileiro em 2026: SBCE, regulamentação e o que muda para o agro"
- [ ] 26. Enviar Newsletter #1 via Resend para base inicial (contatos pessoais + primeiros assinantes) — 15min
- [ ] 27. Escrever e publicar 3 posts LinkedIn inaugurais — 1h
  - Post 1 (insight): dado sobre o SBCE/regulamentação
  - Post 2 (opinião): por que créditos agrícolas são o futuro
  - Post 3 (bastidor): "Comecei um projeto para conectar cooperativas a compradores de carbono"
- [ ] 28. Configurar Buffer e agendar posts — 15min

**Bloco C: Primeiro outreach (3h)**

- [ ] 29. Selecionar top 5 cooperativas do CRM (score ≥ 3) — 15min
- [ ] 30. Usar Claude para gerar outreach personalizado para cada uma — 30min
- [ ] 31. Revisar e personalizar cada outreach — 30min
- [ ] 32. Enviar 5 outreaches via LinkedIn InMail ou e-mail — 30min
- [ ] 33. Registrar envios no Notion CRM (status → "Contactado") — 15min
- [ ] 34. Selecionar top 5 compradores e gerar outreach — 30min
- [ ] 35. Enviar 5 outreaches para compradores — 30min

**Bloco D: Templates de materiais (1h)**

- [ ] 36. Criar template de Ficha Técnica de Projeto no Canva (identidade TRIMOBE) — 30min
- [ ] 37. Criar template de Deck de Apresentação no Google Slides ou Canva — 30min

---

## Fase 1: Prospecção e Discovery

### Semana 3-4 (10h/semana cada)

**Rotina semanal estabelecida:**

- [ ] 38. **Segunda noite (1.5h):** revisar outputs da Máquina 2 (novos prospects da IA), selecionar top 5, personalizar e enviar outreaches. Follow-ups dos lotes anteriores.
- [ ] 39. **Terça noite (1.5h):** revisar rascunho da newsletter gerado pela IA, editar, publicar via Resend. Revisar e agendar 3 posts LinkedIn.
- [ ] 40. **Quarta ou quinta (2h):** agendar e realizar primeiras calls de discovery (2-3 por semana). Gravar com permissão.
- [ ] 41. **Quinta noite (1h):** processar calls com Máquina 3 (transcrição → insights → follow-up). Revisar e enviar follow-ups.
- [ ] 42. **Sexta noite (1h):** gestão de pipeline no Notion. Mover deals. Responder e-mails pendentes.
- [ ] 43. **Sábado manhã (1h):** weekly review — métricas, o que funcionou, o que ajustar.

**Metas da semana 3-4:**
- [ ] 10+ outreaches enviados (total acumulado: 15-20)
- [ ] 3-5 respostas recebidas
- [ ] 2-4 calls de discovery realizadas
- [ ] Newsletter #2 e #3 enviadas
- [ ] 6 posts LinkedIn publicados

### Semana 5-6

- [ ] 44. Continuar rotina semanal
- [ ] 45. Iniciar outreach para lado comprador com mais intensidade (5-8 contatos)
- [ ] 46. Com base nas calls de discovery, identificar 2-3 cooperativas com créditos disponíveis para venda
- [ ] 47. Gerar primeira Ficha Técnica de Projeto para cooperativa qualificada (usando template + Claude)
- [ ] 48. Refinar prompts dos agentes com base nos primeiros resultados reais

### Semana 7-8

- [ ] 49. Fazer matching: identificar compradores para os projetos qualificados
- [ ] 50. Usar Agente de Matching para selecionar top 3 compradores para cada projeto
- [ ] 51. Gerar deck de apresentação para o primeiro projeto
- [ ] 52. Apresentar projeto a 2-3 compradores (e-mail com ficha + proposta de call)
- [ ] 53. Newsletter #6-7 enviadas, base crescendo
- [ ] 54. Avaliar: a Máquina 1 está gerando conteúdo de qualidade? Ajustar.
- [ ] 55. Avaliar: a Máquina 2 está trazendo prospects relevantes? Ajustar score/fontes.

**Checkpoint Fase 1 (fim semana 8):**
- [ ] 20+ outreaches enviados
- [ ] 5-10 calls de discovery realizadas
- [ ] 3-5 cooperativas qualificadas (com créditos para vender)
- [ ] 2-3 projetos apresentados a compradores
- [ ] 50-100 assinantes newsletter
- [ ] 15+ posts LinkedIn publicados

---

## Fase 2: Primeiro Deal

### Mês 3-4 (10h/semana)

- [ ] 56. Manter rotina semanal de prospecção + conteúdo
- [ ] 57. Focar gestão de pipeline: 5-10 conversas ativas simultaneamente
- [ ] 58. Calls de negociação (1-2/semana) com compradores interessados
- [ ] 59. Gerar materiais sob demanda: propostas comerciais, fichas atualizadas, FAQs
- [ ] 60. Negociar termos: preço por tCO2e, volume, condições de pagamento
- [ ] 61. Se necessário, preparar minuta de contrato de intermediação (IA gera, advogado revisa)
- [ ] 62. Manter pipeline fresco: novos outreaches (3-5/semana) para não depender de poucos deals

### Mês 5-6 (10h/semana)

- [ ] 63. Fechar ou estar em negociação avançada no primeiro deal
- [ ] 64. Due diligence: auxiliar comprador a validar certificação, vintage, volumes
- [ ] 65. Facilitar contrato entre cooperativa e comprador
- [ ] 66. Cobrar comissão conforme acordo
- [ ] 67. Documentar o processo inteiro do primeiro deal (aprendizados, timeline real, gargalos)
- [ ] 68. Newsletter com 200-500 assinantes
- [ ] 69. Criar mini-case do primeiro deal (anonimizado) para usar em conteúdo e prospecção

**Checkpoint Fase 2 (fim mês 6):**
- [ ] 1-2 deals fechados ou em negociação avançada
- [ ] Receita bruta acumulada: R$ 10.000+
- [ ] Pipeline ativo: 5-10 conversas
- [ ] Newsletter: 200-500 assinantes
- [ ] 30+ posts LinkedIn publicados
- [ ] Máquinas de IA rodando com ajustes mínimos

---

## Fase 3: Operação Estável

### Mês 7-9 (10h/semana — ritmo de cruzeiro)

- [ ] 70. Rotina semanal estabilizada conforme tabela do PRD
- [ ] 71. Leads inbound começam a chegar via newsletter e LinkedIn
- [ ] 72. Reduzir outreach ativo (de 5-10/semana para 3-5) conforme inbound cresce
- [ ] 73. Focar em deals de maior valor (volume > 10.000 tCO2e)
- [ ] 74. Expandir nicho: testar pecuária e soja se café está saturado
- [ ] 75. **Mês 9 — Primeira avaliação estratégica:**
  - O modelo funciona? Fechei 2+ deals?
  - Onde está o gargalo? (cooperativas, compradores, matching, fechamento?)
  - 10h/semana são suficientes?
  - Há demanda adjacente? (ferramenta, consultoria, relatórios)

### Mês 10-12 (10h/semana)

- [ ] 76. Manter operação e pipeline
- [ ] 77. Newsletter com 500-1.000 assinantes
- [ ] 78. Avaliar: newsletter premium (CarbonData) tem demanda?
- [ ] 79. Avaliar: cooperativas pedem ferramenta de gestão? (sinal para CarbonAgro SaaS)
- [ ] 80. **Mês 12 — Decisão de evolução:**
  - **Manter side project** se 2-3 deals/ano geram receita satisfatória
  - **Escalar full-time** se pipeline é grande demais para 10h/semana
  - **Lançar SaaS** se há demanda validada por 3+ cooperativas
  - **Pivotar** se modelo não funciona mas há demanda adjacente
  - **Encerrar** se kill criteria foram atingidos

**Checkpoint Final (mês 12):**
- [ ] 4-8 deals fechados no ano
- [ ] Receita bruta acumulada: R$ 50.000-200.000
- [ ] Pipeline: 10-20 conversas ativas
- [ ] Newsletter: 1.000+ assinantes
- [ ] Leads inbound: 2-5/mês
- [ ] Decisão informada sobre próximo passo

---

## Resumo: O Que Fazer Primeiro

Se você tem 2 horas agora, faça exatamente isto:

1. **Configurar e-mail contato@trimobe.com** — 15min
2. **Criar Company Page TRIMOBE no LinkedIn** — 15min
3. **Atualizar perfil pessoal no LinkedIn** com headline TRIMOBE — 15min
4. **Usar Claude para mapear 10 cooperativas de café com projetos de carbono** — 30min
5. **Escrever e enviar 2 outreaches personalizados** para gestores de sustentabilidade — 30min
6. **Publicar post de lançamento no LinkedIn** — 15min

O primeiro deal pode estar a 8 semanas de distância. Começa com um e-mail para uma cooperativa de café — enviado hoje.
