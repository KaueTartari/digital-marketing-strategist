# Playbook B2C, e-commerce e mídia automatizada

## Sumário
1. Arquitetura Google Ads
2. Arquitetura Meta Ads
3. Creative-first media buying
4. TikTok, WhatsApp e negócios locais
5. Value-Based Bidding, POAS e pLTV
6. Métricas de e-commerce

---

## 1. Arquitetura Google Ads
| Campanha | Papel |
|---|---|
| **Performance Max** | Cobertura de todo o inventário Google (Search, Shopping, YouTube, Display, Discover, Gmail, Maps) com lances automáticos; motor de escala |
| **Search por categoria** | Controle de termos de alta intenção, marca e categorias estratégicas, com copy e página específicas |
| **Shopping (Standard)** | Controle de produtos/margens específicos, testes e proteção de itens estratégicos |
| **Demand Gen / YouTube** | Topo e meio de funil (See/Think), públicos semelhantes e remarketing visual |

Boas práticas:
- **Feed de produtos** é a base (Merchant Center): títulos com atributos buscados, GTIN, imagens boas, preço e estoque corretos, rótulos personalizados (`custom_label`) por margem, sazonalidade e giro.
- Separe PMax por **margem ou papel** (campeões, margem alta, liquidação, novos produtos) em vez de uma campanha única com tudo.
- **Grupos de recursos** por categoria/público com criativos próprios; sinais de público com listas de clientes e visitantes.
- Proteja a marca: exclusão de marca na PMax quando houver campanha de marca em Search, e acompanhe canibalização.
- **Aquisição de novos clientes:** use a meta de novos clientes (lance maior ou somente novos) com listas de clientes atualizadas.
- Relatórios de termos de pesquisa, canais e insights da PMax semanalmente; negativas no nível da conta quando necessário.

## 2. Arquitetura Meta Ads
- **Campanhas de vendas Advantage+** (evolução das Advantage+ Shopping Campaigns — ASC): automação de público, posicionamento e criativo, com o algoritmo encontrando compradores. Estrutura enxuta: poucas campanhas, poucos conjuntos, muitos criativos.
- **Controle de clientes existentes:** informe a lista de clientes e defina limite de verba para público existente quando o objetivo for aquisição.
- **Campanha de testes criativos** separada (ou dentro da estrutura, conforme maturidade) para validar novos conceitos antes de escalar.
- **Remarketing dedicado** só quando houver volume e mensagem diferente (oferta, carrinho, recompra); com Advantage+ muito do remarketing já acontece automaticamente.
- Catálogo conectado para anúncios dinâmicos e coleções.
- Nomes e opções de campanha da Meta mudam com frequência; confira a nomenclatura atual no Gerenciador de Anúncios.

## 3. Creative-first media buying
Com segmentação automatizada, **o criativo é a segmentação**: cada ângulo encontra um público diferente.
- **Volume e diversidade:** muitos conceitos diferentes (não 10 variações da mesma peça): dores, desejos, objeções, provas, formatos (UGC, depoimento, demonstração, comparação, bastidor, estático com oferta).
- **Rotina de rotação:** introduza novos conceitos continuamente. Como referência inicial, renove o conjunto a cada **3–4 semanas**, mas decida pelos sinais de fadiga, não pelo calendário:
  - frequência subindo com CTR e taxa de conversão caindo;
  - CPA subindo com CPM estável;
  - retenção de vídeo caindo.
- **Framework de teste:** hipótese → 3–5 criativos por conceito → verba mínima e prazo definidos → critério de vitória (CPA/ROAS, não CTR) → escalar vencedor e iterar variações dele.
- Veja `copy-criativos.md` para briefing, roteiro e limites.

## 4. TikTok, WhatsApp e negócios locais
- **TikTok Ads:** criativo nativo (parece conteúdo, não anúncio), gancho nos primeiros 2 segundos, criadores (Spark Ads, TikTok One), campanhas de vendas com catálogo quando e-commerce. Pixel + Events API como na Meta.
- **Anúncios de clique para WhatsApp** (Meta): fortes no Brasil para serviços, varejo local, alto envolvimento. Otimize para conversas e, sempre que possível, envie eventos de venda da conversa de volta (CAPI para mensagens / integração do CRM de WhatsApp) para não otimizar só por "oi". Tenha atendimento rápido e roteiro de qualificação.
- **Negócios locais:** Perfil da Empresa no Google (avaliações, fotos, posts), campanhas de Search com raio geográfico e extensões de local/chamada, PMax para lojas físicas quando houver metas de visita, Meta com raio e oferta local.
- **Marketplaces** (Mercado Livre, Amazon, Shopee): ads dentro da plataforma com ACOS/TACOS como métrica e cuidado com canibalização do próprio site.

## 5. Value-Based Bidding, POAS e pLTV
Lances por valor fazem o algoritmo buscar **clientes que valem mais**, não só mais conversões.

**Níveis de sinal de valor**
| Nível | Valor enviado | Quando usar |
|---|---|---|
| 1 | Receita do pedido | Mínimo para e-commerce |
| 2 | **Lucro bruto / margem de contribuição** (receita − custo do produto − frete subsidiado − taxas) | Mix com margens muito diferentes |
| 3 | **pLTV** (valor previsto do cliente em 6–12 meses) | Recompra/assinatura relevante e dados suficientes para modelar |

**Fórmulas**
- ROAS = receita ÷ investimento
- **POAS** = lucro bruto dos pedidos ÷ investimento (POAS > 1 = mídia se paga em lucro bruto)
- ROAS de equilíbrio = 1 ÷ margem de contribuição (margem de 40% → ROAS mínimo 2,5)
- MER (Marketing Efficiency Ratio) = receita total da loja ÷ investimento total em mídia

**Implementação**
- Calcule margem/pLTV no servidor ou no ERP e envie como `value` da conversão via **server-side** (Meta CAPI, Google Enhanced Conversions/conversões offline, GTM server-side). Não exponha margem no navegador.
- Google: estratégias ROAS desejado / maximizar valor da conversão; regras de valor para ajustar por público, local ou dispositivo.
- Meta: otimização por valor (valor mais alto / ROAS mínimo) quando houver volume de compras com valores variados.
- Mantenha consistência: se mudar de receita para lucro, redefina metas de ROAS (os números mudam de escala) e dê tempo de reaprendizado.
- Descontar estornos e cancelamentos (ver `sinais-rastreamento.md`).

## 6. Métricas de e-commerce
| Métrica | Uso |
|---|---|
| Receita e lucro após mídia | Resultado real |
| POAS / ROAS por campanha | Eficiência tática |
| MER / ROAS combinado | Saúde geral, independente de atribuição |
| CAC de novos clientes (nCAC) | Custo de crescer a base |
| Taxa de novos clientes | Se a mídia está só recomprando clientes antigos |
| Taxa de conversão, ticket médio | Página, oferta e preço |
| LTV 90/180/365 dias por coorte e canal | Qualidade do cliente adquirido |
| Taxa de reembolso/devolução por campanha | Promessa desalinhada ou público errado |
