# Engenharia de sinais e rastreamento privacy-first

## Sumário
1. Arquitetura recomendada
2. Plano de eventos
3. Meta Pixel + Conversions API
4. Google: Enhanced Conversions e conversões offline
5. Estornos, cancelamentos e ajustes
6. Consentimento e LGPD
7. UTMs e padrão de nomenclatura
8. Checklist de auditoria

---

## 1. Arquitetura recomendada
```
Site/App ──► GTM Web ──► GTM Server-Side (subdomínio próprio, ex.: sgtm.marca.com.br)
                              ├──► GA4
                              ├──► Meta Conversions API
                              ├──► Google Ads (conversões + Enhanced Conversions)
                              └──► TikTok Events API / LinkedIn CAPI
CRM / ERP ───────────────────────► Conversões offline (lead qualificado, venda, estorno)
```
- **Server-side** reduz perda por bloqueadores e restrições de navegador, dá controle sobre quais dados saem e melhora a qualidade de correspondência.
- Hospedagem do GTM server-side: Google Cloud (Cloud Run), ou provedores gerenciados (Stape e similares).
- Integrações nativas (Shopify, WooCommerce, VTEX, Nuvemshop, RD Station, HubSpot) já oferecem CAPI e conversões avançadas; verifique antes de construir do zero.

## 2. Plano de eventos
Documente antes de implementar:
| Evento | Gatilho | Parâmetros | Destinos | Conversão principal? |
|---|---|---|---|---|
| `view_content` / `ViewContent` | Página de produto/serviço | id, categoria, valor | GA4, Meta, TikTok | Não |
| `add_to_cart` / `AddToCart` | Clique confirmado | itens, valor | GA4, Meta | Não |
| `begin_checkout` / `InitiateCheckout` | Início do checkout | itens, valor | GA4, Meta | Não |
| `purchase` / `Purchase` | Pedido **pago** confirmado no servidor | transaction_id, valor, moeda, itens, dados do cliente | Todos | Sim (e-commerce) |
| `generate_lead` / `Lead` | Envio de formulário válido | form, origem, dados do cliente | Todos | Secundária |
| `qualified_lead` (personalizado) | Lead aprovado no CRM | valor estimado, ids de clique | Google, Meta, LinkedIn | Sim (B2B) |
| `close_won` (personalizado) | Contrato fechado | valor | Google, Meta, LinkedIn | Sim para lance por valor |

Marque como conversão principal (a que o algoritmo otimiza) só o que representa valor real.

## 3. Meta Pixel + Conversions API
### Deduplicação
Quando o mesmo evento é enviado pelo navegador (Pixel) e pelo servidor (CAPI):
- `event_name` **igual** nos dois envios.
- `event_id` **igual** nos dois envios do mesmo evento e **diferente** entre eventos distintos (ex.: o ID do pedido para `Purchase`, um UUID gerado no clique para `Lead`).
- A Meta usa o par `event_name` + `event_id` para descartar a duplicata (preferência pelo primeiro recebido dentro da janela de deduplicação).
- Alternativa quando não há `event_id`: `external_id`/`fbp` combinados, menos confiável.
- Verifique no Gerenciador de Eventos → Visão geral → deduplicação e na ferramenta de teste de eventos.

### Qualidade da correspondência (Event Match Quality)
| Parâmetro | Chave | Hash SHA-256? |
|---|---|---|
| E-mail | `em` | **Sim** (minúsculo, sem espaços, antes do hash) |
| Telefone | `ph` | **Sim** (só dígitos com código do país: 5511999999999) |
| Nome, sobrenome | `fn`, `ln` | **Sim** (minúsculo, sem acento conforme orientação da Meta) |
| Cidade, estado, CEP, país | `ct`, `st`, `zp`, `country` | **Sim** |
| ID do cliente | `external_id` | Recomendado (hash) |
| Cookie do navegador | `fbp` (cookie `_fbp`) | **Não** — enviar o valor como está |
| Identificador de clique | `fbc` (cookie `_fbc` ou montado a partir do `fbclid`) | **Não** |
| IP | `client_ip_address` | **Não** |
| Navegador | `client_user_agent` | **Não** |

Enviar hash em `fbp`, `fbc`, IP ou user agent quebra a correspondência. Envie o máximo de parâmetros com base legal e consentimento; acompanhe a nota de EMQ por evento.

### Parâmetros do evento
`event_time` (Unix, segundos), `action_source` (`website`, `system_generated`, `physical_store`, `chat`…), `event_source_url`, `custom_data.value`, `custom_data.currency` (`BRL`), `content_ids`, `order_id`.

## 4. Google: Enhanced Conversions e conversões offline
- **Enhanced Conversions (web):** envia e-mail/telefone/endereço com hash junto da conversão da tag, recuperando conversões perdidas por cookies.
- **Enhanced Conversions for Leads:** o lead é identificado pelo e-mail/telefone com hash no formulário; quando vira venda no CRM, a conversão offline é enviada com os mesmos dados. É a abordagem recomendada pelo Google para B2B e serviços.
- **Conversões offline por clique:** capturar e salvar `gclid` (web), `gbraid` (app/iOS web→app) e `wbraid` (iOS app→web) no CRM e enviar a conversão com o identificador e o horário.
- Envio: integrações nativas de CRM, Google Ads API, Data Manager / uploads programados, Zapier/Make ou GTM server-side.
- **Consent Mode v2** precisa estar correto para que os dados sejam aceitos e modelados (seção 6).
- Conversões offline podem levar horas para aparecer e devem ser enviadas dentro da janela de conversão configurada.

## 5. Estornos, cancelamentos e ajustes
O algoritmo precisa aprender que um pedido cancelado não era um bom cliente.
- **Google Ads:** **ajustes de conversão** — *retração* (anula a conversão) e *reapresentação* (corrige o valor), por `order_id`/GCLID, via upload ou API.
- **Meta:** não há "evento negativo" que desfaça o aprendizado de uma compra. Boas práticas: só enviar `Purchase` quando o pagamento for aprovado (não no pedido gerado), enviar o valor líquido, e registrar estornos como evento personalizado para análise e ajuste de relatórios. Confira se a Meta lançou recurso de ajuste nativo desde a criação desta skill.
- **Todas as plataformas:** para produtos com cancelamento alto (boleto, Pix não pago, contratos com período de teste), otimizar para o evento confirmado no ERP em vez do evento do checkout.
- Automatize a sincronização ERP/CRM → plataformas diariamente.

## 6. Consentimento e LGPD
- Banner de consentimento (CMP) com opções reais de aceitar e recusar cookies não essenciais, registro do consentimento e política de privacidade clara.
- **Google Consent Mode v2:** parâmetros `ad_storage`, `analytics_storage`, `ad_user_data`, `ad_personalization`. Obrigatório para anunciantes que atingem o Espaço Econômico Europeu e boa prática no Brasil para respeitar escolhas e permitir modelagem.
- Server-side **não é atalho para ignorar consentimento**: a decisão do usuário vale também para os envios do servidor.
- Listas de clientes (Customer Match, públicos personalizados) exigem base legal para uso dos dados e aviso ao titular; envie sempre com hash.
- Contrato com fornecedores de tracking e mídia como operadores de dados.
- Minimização: não envie a plataformas de anúncio dados sensíveis (saúde, religião, orientação etc.) nem parâmetros de URL com dados pessoais em claro.

## 7. UTMs e padrão de nomenclatura
```
utm_source   = plataforma        (google, meta, linkedin, tiktok, newsletter)
utm_medium   = tipo              (cpc, paid_social, email, organic_social, referral)
utm_campaign = campanha          (2026-09_b2b_leadgen_diagnostico)
utm_content  = criativo/anúncio  (video-dor-v2)
utm_term     = palavra/público   (software-gestao-exata)
```
- Tudo em minúsculas, sem espaços e acentos, com padrão documentado e compartilhado.
- Nome de campanha no painel = `utm_campaign` para cruzar dados.
- Salvar UTMs e ids de clique no CRM em campos de primeira e última origem.
- Google Ads: auto-tagging ligado (GCLID) e UTMs via modelo de acompanhamento/sufixo, se necessário.

## 8. Checklist de auditoria
- [ ] Conversão principal representa valor real (compra paga, lead qualificado), sem duplicidade de tags
- [ ] Pixel + CAPI deduplicados (`event_name` + `event_id` iguais), EMQ acompanhada
- [ ] Hash correto em `em`/`ph`/`fn`/`ln`, sem hash em `fbp`/`fbc`/IP/user agent
- [ ] Enhanced Conversions ativas no Google Ads e diagnóstico sem erros
- [ ] `gclid`/`gbraid`/`wbraid`, `fbc`/`fbp`, `li_fat_id` e UTMs salvos no CRM
- [ ] Conversões offline de SQL/venda sendo enviadas automaticamente
- [ ] Estornos/cancelamentos tratados (ajustes no Google, valor líquido na Meta)
- [ ] Consent Mode v2 e CMP funcionando (testar aceitar e recusar)
- [ ] GA4 com `transaction_id` e sem compras duplicadas
- [ ] Valores e moeda corretos (BRL, com decimal certo)
- [ ] Números das plataformas batem com o ERP/CRM dentro de uma margem aceitável
