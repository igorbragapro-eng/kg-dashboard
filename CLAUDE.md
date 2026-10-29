# Agência — Central de Agentes

Este repositório é a "sede" da agência. O Claude Code lê este arquivo sempre que abre aqui.

## Quem sou eu
Gestor de tráfego, dono de agência de marketing. Atendo clientes em Meta Ads e Google Ads.
Estou começando em programação: explique os passos de forma simples e em português.

## Regras para todos os agentes
- Responda sempre em português do Brasil.
- **Nunca** altere campanhas, orçamentos, públicos, pixels ou tags sem mostrar o que vai mudar e eu confirmar.
- **Nunca** envie e-mail, WhatsApp ou convite de agenda sem eu aprovar o texto final.
- Dados de cada cliente ficam em `clientes/<nome-do-cliente>/` (ver `clientes/_modelo/`).
- Relatórios e dashboards seguem o visual de `index.html` (dashboard KG Home Remodeling).

## Equipe de agentes (`.claude/agents/`)
| Agente | Faz o quê |
|---|---|
| `gerente-agencia` | Recebe o pedido e distribui para o agente certo |
| `relatorio-campanhas` | Puxa dados Meta/Google e monta relatório para o cliente |
| `rastreamento` | Pixel Meta, tag Google Ads, GA4 e Google Tag Manager |
| `atendimento-cliente` | Pede materiais/acessos/aprovações, follow-up, e-mail e WhatsApp |
| `agenda` | Encontra horário livre e marca reunião no Google Agenda |
| `design` | Criativos e peças no Canva a partir do briefing |

Agentes que já existem fora deste repo: análise de concorrentes (skill `marketing:competitive-brief`)
e edição de vídeo para Instagram.

## Conectores usados
- Meta Ads (Facebook) — campanhas, insights, pixel/datasets
- Windsor.ai — Google Ads, GA4, Meta e outras fontes num lugar só
- Gmail e Google Agenda — e-mail e reuniões
- Google Drive — salvar relatórios
- Canva — design
