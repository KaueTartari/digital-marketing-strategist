# Playbook B2B e ABM

## Sumário
1. Métricas de pipeline
2. LinkedIn Ads
3. Google Ads Search para B2B
4. ABM (Account-Based Marketing)
5. Remarketing multicanal de 90 dias
6. Conversões offline e qualidade de lead
7. Estrutura de campanha modelo

---

## 1. Métricas de pipeline
Otimize para **reuniões com decisores, oportunidades e contratos**, não para volume de formulários.

| Métrica | Fórmula / uso |
|---|---|
| Custo por SQL | Investimento ÷ SQLs |
| Taxa MQL→SQL | SQLs ÷ MQLs (abaixo de ~20% costuma indicar segmentação, oferta ou formulário frouxo) |
| Pipeline gerado | Soma do valor das oportunidades originadas por marketing |
| CAC | (Mídia + marketing + vendas) ÷ novos clientes no período |
| LTV | Receita mensal por cliente × margem bruta × vida média em meses |
| **LTV/CAC** | Referência de sustentabilidade: **≥ 3** |
| Payback do CAC | CAC ÷ (receita mensal por cliente × margem bruta); em SaaS, até ~12 meses é considerado saudável |
| Velocidade do pipeline | (Oportunidades × ticket × taxa de ganho) ÷ ciclo em dias |

Como o ciclo B2B é longo, acompanhe indicadores antecedentes (SQLs, oportunidades) semanalmente e receita mensalmente.

## 2. LinkedIn Ads
**Segmentação**
- Cargo (título), função, senioridade (Diretor, VP, C-Level, Head), tamanho da empresa, setor, nomes de empresas (listas ABM), competências.
- Evite públicos minúsculos: a própria plataforma recomenda públicos na casa de dezenas de milhares para campanhas de performance; em ABM de poucas contas, aceite custo maior.
- Exclua funcionários, clientes atuais (quando o objetivo é aquisição) e concorrentes.
- Desative a expansão automática de público quando precisar de precisão.

**Formatos**
- **Lead Gen Forms nativos:** formulário pré-preenchido com dados do perfil, sem sair do LinkedIn. Costumam reduzir bastante o custo por lead em relação a landing page, mas podem trazer leads menos intencionais — use 1–2 perguntas de qualificação e meça custo por **SQL**, não só por lead. (Reduções percentuais divulgadas por agências e pela plataforma variam muito; valide no próprio teste A/B.)
- **Document Ads** (conteúdo em PDF baixável), **Thought Leader Ads** (post de executivo impulsionado), vídeo, conversation ads.
- **Oferta por etapa:** Think = guia, benchmark, webinar; Do = diagnóstico, demo, calculadora de ROI.

**Integração:** envie leads do formulário direto ao CRM (integração nativa ou automação) para contato em minutos; use a Conversions API do LinkedIn para mandar etapas do CRM de volta.

## 3. Google Ads Search para B2B
- Palavras de intenção comercial com **correspondência exata e de frase**; ampla só com lances inteligentes, conversões offline de qualidade e lista negativa madura.
- **Lista negativa robusta e viva:** termos de emprego (vaga, salário, estágio, curso), gratuito/grátis, pessoa física, "o que é" (se a campanha for Do), download, concorrentes irrelevantes, locais fora de atendimento. Revise o relatório de termos de pesquisa semanalmente; contas B2B maduras chegam a centenas de negativas.
- Grupos por tema de intenção (solução, problema, comparação, marca própria, concorrente).
- Anúncios responsivos com qualificação na copy ("para empresas a partir de 50 funcionários", "a partir de R$ X/mês") para afastar clique sem fit.
- Landing page específica por tema, com prova B2B (logos, cases, números), formulário curto + opção de agendar direto.
- Lances: começar com maximizar conversões ou CPA desejado otimizando para lead qualificado (não qualquer envio); migrar para valor quando houver conversões offline suficientes.

## 4. ABM (Account-Based Marketing)
| Nível | Contas | Abordagem |
|---|---|---|
| 1:1 (estratégico) | 5–50 | Conteúdo e oferta personalizados por conta, eventos, envio físico, vendas + marketing juntos |
| 1:few | 50–500 | Personalização por setor ou cluster de dor |
| 1:many | 500+ | Segmentação por lista de empresas e sinais de intenção, mensagens por segmento |

Passos: lista de contas-alvo com vendas → mapear comitê de compra (decisor, influenciador, usuário, financeiro, jurídico) → anúncios para as contas (LinkedIn por nome de empresa, listas de e-mail corporativo) → cadência de vendas coordenada → métricas por conta (contas engajadas, reuniões, oportunidades, receita).

## 5. Remarketing multicanal de 90 dias
Janela ampla porque a decisão B2B é demorada. Segmentar por tempo desde a última interação e por nível de engajamento.

| Fase | Janela | Objetivo | Conteúdo |
|---|---|---|---|
| 1 | 1–30 dias | Reconhecimento e autoridade | Insights do setor, vídeo do fundador/especialista, dados de mercado, prova social |
| 2 | 31–60 dias | Comparação técnica | Estudos de caso do setor, comparativos, webinar, "como escolher", objeções respondidas |
| 3 | 61–90 dias | Decisão comercial | Demonstração, diagnóstico gratuito, calculadora de ROI, condição com prazo real |

- Canais: LinkedIn (visitantes do site, engajamento com anúncios e lead forms abertos), Google (Display/YouTube/Demand Gen e listas de remarketing em Search), Meta (visitantes e engajados).
- **Exclua** quem já virou oportunidade ou cliente, e limite frequência.
- Remarketing no LinkedIn e no Google exige tag/insight tag e consentimento válido.

## 6. Conversões offline e qualidade de lead
Sem devolver o resultado de vendas às plataformas, os algoritmos otimizam para quem preenche formulário, não para quem compra.

1. **Capturar identificadores** no formulário (campos ocultos) e salvar no CRM: `gclid`, `gbraid`, `wbraid` (Google), `fbclid`/cookie `_fbc` e `_fbp` (Meta), `li_fat_id` (LinkedIn), UTMs, e-mail e telefone.
2. **Definir etapas** como conversões: Lead → MQL → SQL → Oportunidade → Contrato fechado (com valor).
3. **Enviar de volta**:
   - Google Ads: conversões offline por clique (GCLID/GBRAID/WBRAID) e **Enhanced Conversions for Leads** (e-mail/telefone com hash), que o Google recomenda por não depender só do clique; via integração nativa do CRM (HubSpot, Salesforce, RD Station, Pipedrive via automação), Google Ads API/Data Manager ou ferramenta de terceiros.
   - Meta: Conversions API com eventos do CRM (lead qualificado, venda).
   - LinkedIn: Conversions API.
4. **Otimizar** a campanha para a etapa com volume suficiente (geralmente SQL ou oportunidade) e usar valor quando possível.
5. **Janela:** envie dentro do prazo aceito pela plataforma (no Google, conversões de clique valem até 90 dias por padrão); automatize diariamente.

Os limites de janela, formatos e nomes de integração mudam; confirme na documentação atual antes de implementar.

## 7. Estrutura de campanha modelo
| Campanha | Etapa | Segmentação | Oferta | Conversão otimizada | KPI |
|---|---|---|---|---|---|
| LinkedIn — Thought Leadership | See/Think | Cargos-alvo × porte × setor | Post do especialista, Document Ad | Engajamento / visualização | Custo por engajado no ICP |
| LinkedIn — Lead Gen | Think/Do | Mesmo público + remarketing | Guia / diagnóstico | SQL (via CAPI) | Custo por SQL |
| Google Search — Solução | Do | Palavras comerciais exatas/frase | Demo / orçamento | SQL (offline) | Custo por SQL, pipeline |
| Google Search — Marca | Do | Marca própria | Contato | Oportunidade | Parcela de impressões, CPA |
| Remarketing 90 dias | Think/Do | Visitantes e engajados por fase | Case → demo | Oportunidade | Oportunidades influenciadas |
