# Guia: montando os agentes de IA da agência

Este guia vai do mais simples (sem programar) ao mais avançado (WhatsApp automático).
Faça uma etapa de cada vez. Cada uma já entrega valor sozinha.

---

## Como funciona (em 1 minuto)

Um **agente** = instruções (o "manual do funcionário") + **ferramentas** (acessos).

- **Instruções** → arquivos em `.claude/agents/*.md` (já criados neste repositório).
- **Ferramentas** → **conectores** (Meta Ads, Google, Gmail, Agenda, Canva, Drive).
- **Memória do cliente** → `clientes/<nome>/cliente.md`.
- **Regras gerais** → `CLAUDE.md` (o Claude lê sempre que abre este projeto).

```
Você (pedido)
   │
   ▼
gerente-agencia ──► relatorio-campanhas ──► Meta Ads / Windsor (Google Ads, GA4)
                ──► rastreamento        ──► Pixel Meta / JSON do GTM
                ──► atendimento-cliente ──► Gmail / WhatsApp
                ──► agenda              ──► Google Agenda
                ──► design              ──► Canva
```

---

## Etapa 1 — Conectar as contas (sem código, ~30 min)

Em **claude.ai → Configurações → Conectores**, conecte:

| Conector | Para quê | Status nesta sessão |
|---|---|---|
| Facebook / Meta Ads | Campanhas, insights, pixel | ✅ conectado |
| Windsor.ai | Google Ads, GA4, Meta num lugar só | ✅ conectado |
| Google Drive | Salvar relatórios | ✅ conectado |
| Canva | Criativos | ✅ conectado |
| Gmail | Rascunhar/enviar e-mails | conectar / verificar |
| Google Agenda (Calendar) | Marcar reuniões | conectar / verificar |

No Windsor.ai, adicione a conta **Google Ads** e a **GA4** de cada cliente
(peça ao Claude: "me dá o link para conectar o Google Ads no Windsor").

## Etapa 2 — Preencher a ficha de cada cliente (~10 min por cliente)

1. Copie `clientes/_modelo/` para `clientes/kg-home-remodeling/` (por exemplo).
2. Preencha IDs das contas, contato, meta de CPL.
   Dica: peça "agente gerente-agencia, cria a ficha do cliente X" e ele pergunta o que falta.

## Etapa 3 — Usar os agentes no Claude Code

Abra este repositório no Claude Code (app, web em claude.ai/code ou terminal) e peça em português:

- "Use o **relatorio-campanhas** e faça o relatório de outubro da KG Home Remodeling."
- "**rastreamento**: audita o pixel da KG e monta o JSON do GTM com Meta + Google Ads + GA4."
- "**atendimento-cliente**: rascunha um e-mail pedindo acesso ao Google Ads para o cliente X."
- "**agenda**: marca reunião de resultados com a KG semana que vem."
- "**design**: cria 3 criativos para stories com a oferta de orçamento grátis."
- "**gerente-agencia**: faz o onboarding do cliente novo Y."

Para criar novos agentes: digite `/agents` no Claude Code, ou copie um `.md` de `.claude/agents/` e edite.

## Etapa 4 — Automatizar no horário (tarefas agendadas)

No Claude Code dá para criar **tarefas agendadas** que rodam sozinhas. Exemplos:

- Toda segunda 8h40: "relatório semanal de todos os clientes + lista de alertas".
- Todo dia 1º: "relatório mensal de cada cliente, salvar no Drive e rascunhar e-mail".
- Todo dia 10h: "verificar anúncios reprovados e contas sem saldo".

É só pedir: *"cria uma tarefa agendada toda segunda às 8h40 para gerar o relatório semanal"*.

## Etapa 5 — Rastreamento (Pixel + Google + GTM)

O que é automático e o que é manual:

| Item | Como |
|---|---|
| Achar ID do pixel e checar eventos/qualidade | Agente via conector Meta ✅ |
| Pegar ID de conversão Google Ads (AW-) e rótulo | Você copia no Google Ads (o agente explica onde) |
| Montar tags, gatilhos e variáveis do GTM | Agente gera **JSON de importação** ✅ |
| Subir no GTM | Você importa: Administrador → Importar contêiner → **Mesclar** → Visualizar → Publicar |
| Testar | Tag Assistant + Meta Pixel Helper + "Eventos de teste" (agente guia) |

> Subir no GTM 100% automático é possível com a **API do Google Tag Manager**, mas exige
> criar projeto no Google Cloud e credenciais. Deixe para depois que o fluxo manual estiver redondo.

## Etapa 6 — WhatsApp (o mais avançado)

O Claude não tem conector nativo de WhatsApp. O caminho recomendado:

```
Cliente manda msg no WhatsApp
      │
      ▼
WhatsApp Business Cloud API (oficial da Meta)
      │  webhook
      ▼
n8n (automação, tipo Zapier)  ──►  API do Claude (o "cérebro" do agente)
      │                                   │
      └──── resposta ◄────────────────────┘
```

Passo a passo:
1. **Número**: use um número só para a agência (não o pessoal) — ele sai do app WhatsApp comum.
2. **Meta for Developers** → criar App → adicionar produto **WhatsApp** → vincular ao seu Business Manager → verificar número. Mensagens que você inicia precisam de **template aprovado**; respostas em até 24h são livres.
3. **n8n**: crie conta em n8n.cloud (ou instale num servidor). Ele tem nós prontos de WhatsApp, Gmail, Google Calendar e Anthropic (Claude).
4. **Chave da API do Claude**: console.anthropic.com → API Keys. (Cobrança por uso, separada da assinatura.)
5. **Primeiro fluxo** (comece por aqui): WhatsApp recebe msg → Claude classifica (dúvida, pedido de reunião, envio de material) → responde ou cria rascunho → manda notificação para você aprovar.
6. **Segundo fluxo**: "quero marcar reunião" → Claude lê a agenda → oferece 3 horários → cliente escolhe → cria evento no Google Agenda.

Alternativas não oficiais (Z-API, Evolution API) são mais fáceis de ligar, mas há risco de banimento do número. Para agência com cliente, prefira a API oficial.

## Etapa 7 — E-mail

- **Agora**: o agente `atendimento-cliente` cria rascunhos no Gmail pelo conector; você revisa e envia.
- **Depois**: no n8n, gatilho "novo e-mail de cliente" → Claude resume e sugere resposta → você aprova.

---

## Ordem recomendada (4 semanas)

| Semana | Entrega |
|---|---|
| 1 | Conectores + fichas dos clientes + primeiro relatório com `relatorio-campanhas` |
| 2 | Auditoria de pixel/GTM de todos os clientes com `rastreamento` |
| 3 | Tarefas agendadas (relatório semanal, alertas) + rascunhos de e-mail + agenda |
| 4 | n8n + WhatsApp Cloud API, começando só com notificações para você |

## Regra de ouro

Os agentes **preparam**; você **aprova**. Nada de mexer em orçamento, publicar tag
ou mandar mensagem para cliente sem o seu OK. Depois de algumas semanas confiando
no resultado, libere a automação aos poucos.
