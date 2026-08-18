# ☕ Rede de Cafeterias — Análise de Faturamento e Alavancas de Crescimento

🤖 **Projeto desenvolvido com Claude AI como co-analista** — do contexto de negócio ao dashboard final.

Análise de dados aplicada a uma rede de 3 cafeterias em Nova York, com o
objetivo de entender se há crescimento de faturamento e quais são suas
principais alavancas. Projeto conduzido com apoio de IA como co-analista,
seguindo um processo estruturado em 6 etapas: contexto de negócio →
entendimento da base → hipóteses → teste de hipóteses → insights →
recomendações.

**🔗 Dashboard interativo:** [rafael7-costa.github.io/rede_cafeterias_faturamento_IA](https://rafael7-costa.github.io/rede_cafeterias_faturamento_IA/)

---

## Problema de Negócio

A rede de cafeterias opera 3 lojas em Nova York (Hell's Kitchen, Lower
Manhattan e Astoria). O Gerente de Operações precisa entender a
trajetória de faturamento da rede ao longo do primeiro semestre de 2023
e identificar quais fatores — loja, categoria de produto, sazonalidade
(mês/dia/hora), ticket médio ou volume de transações — estão puxando
esse crescimento (ou eventual queda).

## Premissas

- O dataset cobre apenas 6 meses (jan–jun/2023), portanto "crescimento"
  é analisado como tendência dentro do semestre, não como comparação
  ano contra ano.
- Cada linha da base representa um item vendido dentro de uma transação
  (granularidade item, não pedido).
- Valores de `unit_price` e `Total_Bill` vieram como texto em formato
  BRL e foram convertidos para numérico antes da análise.
- ~30% dos registros têm `Size = "Not Defined"`, correspondendo a
  produtos sem variação de tamanho (grãos, itens de padaria).

## Contexto

- **Consumidor da análise:** Gerente de Operações
- **Dor principal:** entender se há crescimento de faturamento na rede e
  quais são suas principais alavancas
- **Decisão a embasar:** nenhuma decisão específica e imediata — análise
  de caráter diagnóstico/exploratório, voltada a orientar prioridades
  operacionais futuras

## Perguntas de Negócio

1. O faturamento da rede está crescendo, caindo ou estável ao longo do semestre?
2. Esse crescimento é puxado por mais transações (volume) ou por ticket médio maior?
3. Alguma loja está performando muito melhor ou pior que as outras?
4. Quais categorias de produto mais contribuem para o faturamento?
5. Existem dias da semana ou horários de pico que concentram a maior parte da receita?
6. O padrão de sazonalidade muda por localização de loja?
7. O tamanho da bebida (Small/Regular/Large) influencia o ticket médio?
8. Quais produtos específicos são "campeões de venda"?

## Estratégia da Solução

1. **Contexto de negócio** — alinhamento do público, da dor principal e da ausência de decisão imediata a embasar.
2. **Entendimento da base** — inventário de colunas, tipos, granularidade e limitações (período de 6 meses, formato dos valores).
3. **Hipóteses analíticas** — 10 hipóteses testáveis cobrindo tendência, decomposição de crescimento, performance por loja, mix de produto e sazonalidade.
4. **Teste de hipóteses** — cada hipótese testada com dado quantitativo + visualização + veredito (confirmada / parcialmente confirmada / refutada).
5. **Priorização por acionabilidade** — hipóteses confirmadas reavaliadas pelo quanto permitem uma ação operacional concreta (Alto / Médio / Baixo).
6. **Dashboard final** — reestruturado pela lógica de decomposição do faturamento (Visão Geral → Decomposição do Crescimento → Alavancas por Dimensão → Oportunidades), não pela ordem das hipóteses.

## Hipóteses Testadas

| # | Hipótese | Veredito | Nível Acionável |
|---|---|---|---|
| H1 | Faturamento cresce ao longo do semestre | ✅ Confirmada | 🔴 Baixo |
| H2 | Crescimento vem de volume, não de ticket médio | ✅ Confirmada | 🟢 Alto |
| H3 | Lojas têm crescimento desigual | ❌ Refutada | 🔴 Baixo |
| H4 | Poucas categorias concentram a receita (Pareto) | ⚠️ Parcial | 🟢 Alto |
| H5 | Crescimento puxado por categorias específicas | ⚠️ Parcial | 🟡 Médio |
| H6 | Concentração de receita em dias específicos | ❌ Refutada | 🔴 Baixo |
| H7 | Existe horário de pico de movimento | ✅ Confirmada | 🟢 Alto |
| H8 | Padrão de sazonalidade varia entre lojas | ❌ Refutada | 🟡 Médio |
| H9 | Tamanho da bebida influencia o crescimento | ❌ Refutada | 🔴 Baixo |
| H10 | Poucos produtos concentram a receita | ✅ Confirmada | 🟢 Alto |

## Insights

- **O crescimento é real e forte:** faturamento saiu de R$ 81,7k (Jan) para R$ 166,5k (Jun) — **+103,8%** no semestre.
- **O motor do crescimento é volume, não preço:** transações cresceram +104,2%, enquanto o ticket médio ficou praticamente estável (-0,2%).
- **O crescimento é homogêneo entre lojas:** as três unidades cresceram em ritmos muito próximos (101,7% a 105,1%) — nenhuma loja específica puxa ou trava o resultado.
- **Fevereiro distorce a leitura de curto prazo:** houve uma queda pontual de -6,8% em Fevereiro (provável sazonalidade), o que infla a taxa de crescimento se Janeiro for usado como base. Usando Março como baseline, o crescimento até Junho é de +68,4% — ainda forte, mas sem o efeito degrau artificial.
- **A demanda não tem padrão de dia da semana:** a receita se distribui de forma quase uniforme entre os 7 dias (13,9%–14,6% cada).
- **Existe um pico matinal claro (7h–10h):** essa janela concentra ~46% da receita diária, mas cresceu abaixo da média da rede — sinal de possível limite de capacidade de atendimento nesse horário.
- **A receita é concentrada em poucos produtos:** apenas 9 de 45 produtos (20%) respondem por ~50% do faturamento total; Coffee e Tea sozinhas somam 67% do crescimento do período.

## Resultado

Com base nas hipóteses de maior nível de acionabilidade, foram priorizadas 4 recomendações operacionais:

1. **Reforçar capacidade de atendimento, não estratégias de ticket** — o crescimento não vem de venda mais cara, e sim de mais clientes; o investimento deve ir para caixa, fila e preparo, não para upsell.
2. **Redimensionar equipe para o pico das 7h–10h**, sem descuidar do restante do dia (almoço e tarde crescem acima da média).
3. **Blindar o estoque de Coffee, Tea e dos 9 produtos-âncora**, que concentram a maior parte da receita e do crescimento.
4. **Revisar o horário de funcionamento por loja** — o padrão de demanda é igual entre as três unidades, mas os horários operacionais diferem, o que pode gerar custo sem retorno proporcional.

## Próximos Passos

- Incorporar dados de custo/CMV para transformar a análise de faturamento em análise de margem.
- Avaliar cesta de compras (`transaction_id`) para identificar combinações de produtos com potencial de venda casada.
- Estender a coleta de dados além do primeiro semestre, para permitir comparação ano contra ano e confirmar se a sazonalidade de Fevereiro se repete.
- Cruzar dados de clima/eventos locais para testar se picos e quedas pontuais têm causa externa identificável.

---

## Como a IA foi utilizada

Este projeto foi construído com o **Claude AI atuando como co-analista** ao
longo de todo o processo — não apenas para gerar gráficos, mas conduzindo
o raciocínio analítico junto comigo, etapa por etapa:

1. **Contexto de negócio** — a IA ajudou a estruturar o problema a partir
   de três perguntas-chave (público, dor principal, decisão a embasar),
   transformando respostas soltas em um parágrafo de contexto claro.
2. **Entendimento da base** — inventário automático das colunas, tipos,
   granularidade e limitações do dataset, incluindo o significado de
   negócio de cada campo (não só a estrutura técnica).
3. **Geração de hipóteses** — 10 hipóteses testáveis derivadas das
   perguntas de negócio, cada uma com lógica explícita e método de teste.
4. **Teste de hipóteses** — para cada hipótese, a IA processou os dados
   (Python/pandas), gerou uma visualização e emitiu um veredito
   (confirmada / parcialmente confirmada / refutada).
5. **Priorização por acionabilidade** — reavaliação das hipóteses
   confirmadas pelo quanto cada uma permite uma ação operacional
   concreta, não apenas um achado estatístico.
6. **Construção do dashboard** — o HTML/CSS/JS do dashboard interativo
   foi escrito integralmente pela IA, com iteração de design guiada por
   feedback visual (prints de tela apontando problemas de layout,
   redundância de gráficos e ajustes de proporção).

O papel humano no processo foi **definir o problema, validar cada etapa e
guiar as decisões de negócio** — a IA executou a análise técnica, testou
as hipóteses e traduziu tudo em um artefato visual pronto para uso.

## Ferramentas utilizadas

- **Claude AI (Anthropic)** — co-analista: condução do processo analítico ponta a ponta, da definição de hipóteses ao código do dashboard
- **HTML/CSS/JavaScript (Chart.js)** — dashboard interativo

## Estrutura do repositório

```
rede-cafeterias-faturamento/
├── index.html              → dashboard interativo (GitHub Pages)
├── README.md                → este documento
```
