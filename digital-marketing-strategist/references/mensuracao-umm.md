# Mensuração unificada de marketing (UMM)

## Sumário
1. Por que triangular
2. MMM — Media Mix Modeling
3. Testes de incrementalidade
4. Atribuição (MTA e modelos das plataformas)
5. Como combinar as três
6. Fórmulas essenciais
7. Modelo de relatório para cliente

---

## 1. Por que triangular
Cada plataforma se atribui o crédito da mesma venda; somar os números dos painéis costuma dar mais vendas do que a empresa fez. Nenhum método isolado responde tudo:

| Método | Pergunta que responde | Horizonte | Limitação |
|---|---|---|---|
| **MMM** | Quanto cada canal contribui e onde colocar o próximo real de verba? | Trimestral / anual | Precisa de histórico (idealmente 2+ anos semanais) e variação de investimento |
| **Incrementalidade** | Esta campanha causa vendas que não aconteceriam sem ela? | Pontual, por teste | Custo de oportunidade do grupo de controle, requer desenho cuidadoso |
| **Atribuição (MTA)** | Qual anúncio, palavra ou criativo otimizar esta semana? | Diário / semanal | Correlação, não causalidade; cega para offline e para o que não é clicado |

## 2. MMM — Media Mix Modeling
Modelo estatístico (regressão, geralmente bayesiana) que relaciona vendas a investimento por canal, controlando sazonalidade, preço, promoções, tendência e fatores externos.
- **Ferramentas abertas:** Google **Meridian**, Meta **Robyn**, PyMC-Marketing.
- **Dados:** vendas e investimento por canal em série semanal, impressões/alcance, preço, promoções, sazonalidade, eventos, concorrência, macro (quando disponível).
- **Saídas:** contribuição por canal, ROI marginal, curvas de saturação (onde mais verba rende pouco), efeito residual (adstock) e cenário de alocação ótima.
- **Calibração:** usar resultados de testes de incrementalidade como informação prévia melhora muito o modelo.
- **Porte:** faz sentido a partir de verba e histórico relevantes; para contas pequenas, use MER e testes simples de pausa/geografia.

## 3. Testes de incrementalidade
Comparam um grupo exposto com um grupo de controle não exposto.

| Tipo | Como funciona | Quando usar |
|---|---|---|
| **Conversion Lift da plataforma** (Meta, Google) | A plataforma separa usuários em teste e controle | Medir uma plataforma/campanha com volume suficiente; requer elegibilidade |
| **Geo holdout / GeoLift** | Regiões semelhantes recebem ou não a mídia; compara vendas | Medir canal inteiro, incluindo efeito offline; ferramenta aberta GeoLift (Meta) |
| **Teste de pausa (on/off)** | Pausar canal por período e comparar com tendência esperada | Contas menores; menos rigoroso, cuidado com sazonalidade |
| **Holdout de público** | Excluir parte da base de clientes do remarketing/CRM | Medir remarketing e retenção |

Desenho mínimo:
1. Hipótese e métrica (vendas, receita, novos clientes).
2. Tamanho e duração com poder estatístico suficiente (calcular antes; em geral 2–6 semanas + período de efeito residual).
3. Grupos comparáveis e sem contaminação.
4. Não mudar outras variáveis grandes durante o teste.
5. Resultado com intervalo de confiança, não só a média.

**Métricas:**
- Conversões incrementais = conversões do teste − conversões esperadas sem mídia (controle ajustado).
- **iCPA** = investimento ÷ conversões incrementais.
- **iROAS** = receita incremental ÷ investimento.
- Fator de incrementalidade = conversões incrementais ÷ conversões atribuídas pela plataforma (use para ajustar o ROAS do painel).

## 4. Atribuição (MTA e modelos das plataformas)
- **GA4:** modelo baseado em dados (data-driven) como padrão; os modelos baseados em regra como primeiro clique e linear foram descontinuados em 2023, restando último clique e data-driven.
- **Google Ads:** atribuição baseada em dados.
- **Meta:** janelas de atribuição (ex.: 7 dias após o clique e 1 dia após a visualização); resultados modelados.
- **MTA própria / ferramentas de terceiros:** úteis para jornada de clique, cada vez mais limitadas por privacidade.
- Use atribuição para **otimização tática dentro do canal** (criativos, palavras, públicos), não para decidir a divisão de verba entre canais.
- Compare sempre com a verdade do negócio: ERP/CRM e MER.

## 5. Como combinar as três
| Decisão | Método principal | Checagem |
|---|---|---|
| Orçamento anual/trimestral por canal | MMM | Testes de incrementalidade recentes |
| "Este canal/campanha vale a pena?" | Teste de incrementalidade | MMM e tendência de MER |
| Otimização diária de anúncios, lances e criativos | Atribuição da plataforma | Ajuste pelo fator de incrementalidade |
| Saúde geral | MER, nCAC, lucro após mídia | Todas acima |

Rotina sugerida:
- **Semanal:** atribuição e KPIs táticos, com qualidade de lead/venda do CRM.
- **Mensal:** MER, CAC, LTV por coorte, fechamento com financeiro, ajustes de verba.
- **Trimestral:** MMM (ou análise de tendência para contas menores) e planejamento dos próximos testes de incrementalidade.

## 6. Fórmulas essenciais
| Métrica | Fórmula |
|---|---|
| CPA | Investimento ÷ conversões |
| CPL / custo por SQL | Investimento ÷ leads / SQLs |
| ROAS | Receita atribuída ÷ investimento |
| POAS | Lucro bruto atribuído ÷ investimento |
| ROAS de equilíbrio | 1 ÷ margem de contribuição |
| MER | Receita total ÷ investimento total em mídia |
| CAC | (Mídia + custos de marketing e vendas) ÷ novos clientes |
| nCAC | Investimento em aquisição ÷ novos clientes |
| LTV | Ticket médio × margem bruta × frequência de compra × tempo de vida |
| LTV/CAC | LTV ÷ CAC (referência: ≥ 3) |
| Payback do CAC | CAC ÷ margem bruta mensal por cliente |
| iROAS | Receita incremental ÷ investimento |

## 7. Modelo de relatório para cliente
```
1. Resumo (3–5 linhas)
   - Resultado de negócio do período vs. meta (receita, vendas, SQLs)
   - Principal ganho, principal problema, decisão recomendada

2. Resultado de negócio
   | Métrica | Meta | Realizado | Período anterior | Variação |
   Receita / contratos · Novos clientes · CAC · LTV/CAC ou POAS · Pipeline

3. Mídia por canal
   | Canal | Investimento | Conversões principais | CPA | ROAS/POAS | Tendência |

4. Funil (STDC)
   Alcance qualificado → engajamento → leads/MQL → SQL/vendas → recompra

5. O que explicou os números
   Criativos e públicos vencedores, sazonalidade, mudanças de oferta, problemas de rastreamento

6. Aprendizados e testes
   Hipótese · resultado · decisão

7. Próximos passos
   Ação · responsável · prazo · impacto esperado
```
Destaque discrepâncias entre plataformas e CRM em vez de escondê-las; explique qual número é usado para decisão.
