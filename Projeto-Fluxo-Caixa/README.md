# 💰 DFC — Demonstração de Fluxo de Caixa | Power BI

Dashboard financeiro desenvolvido para consolidar e analisar as movimentações de caixa de uma empresa, permitindo acompanhamento de entradas, saídas e saldos por período, banco e categoria de conta.

---

## 🛠️ Ferramentas Utilizadas

- **Power BI** — visualizações interativas e dashboard
- **Power Query (linguagem M)** — extração, transformação e carga dos dados (ETL)
- **DAX** — criação das medidas e KPIs financeiros
- **Excel** — fonte de dados (.xlsx)

---

## 📁 Estrutura do Repositório

```
fluxo-de-caixa-powerbi/
├── dados/
│   ├── Bancos.xlsx
│   ├── PlanoContas.xlsx
│   ├── Movimentos.xlsx
│   └── SaldoAnterior.xlsx
├── imagens/
│   └── (prints do dashboard)
├── docs/
│   └── documentacao.pdf
├── FluxoCaixa.pbix
└── README.md
```

---

## 🗂️ Modelagem de Dados

O modelo segue a arquitetura **estrela (star schema)**:

- **fMovimentos** — tabela fato principal com registros de entradas e saídas
- **fSaldoAnterior** — tabela fato complementar com saldo inicial por banco
- **dBancos** — dimensão das instituições financeiras
- **dContas** — dimensão do plano de contas (subgrupos e contas)
- **dCalendario** — dimensão temporal gerada via Power Query (dia, mês, ano)

---

## 📊 KPIs e Medidas DAX

| KPI | Descrição |
|---|---|
| Entradas | Total de valores recebidos no período |
| Saídas | Total de valores pagos no período |
| Saídas Abs | Saídas em valor absoluto (para visualização) |
| Saldo Operacional | Resultado líquido do período (Entradas + Saídas) |
| Saldo Inicial | Saldo acumulado antes do período selecionado + saldo anterior |
| Saldo Final | Saldo Inicial + Saldo Operacional |
| Fluxo | Medida dinâmica que centraliza toda a lógica do DFC em uma única expressão |

---

## 📈 Visões do Dashboard

- **Entradas e Saídas por Ano e Mês** — evolução mensal comparativa
- **Saldo por Banco** — distribuição do saldo final por instituição financeira
- **Entradas por Subgrupo** — composição das receitas por categoria
- **Saídas por Subgrupo** — composição das despesas por categoria
- **Saldo Operacional por Ano e Mês** — resultado líquido mensal (operacional e acumulado)
- **Matriz de Detalhamento** — visão analítica por grupo, subgrupo, conta e mês

---

## ⚙️ Destaques Técnicos

- Parâmetro `PastaProjeto` para portabilidade das conexões — basta atualizar o caminho sem refazer as queries
- Calendário gerado dinamicamente via Power Query com `DataInicial` fixo e `DataFinal` baseado em `DateTime.LocalNow()`
- Junções realizadas com `Table.Join` no Power Query para enriquecer a tabela fato com IDs das dimensões
- Medida `Fluxo` com controle de escopo via `ISINSCOPE` para exibição condicional por nível hierárquico

---

## 📌 Sobre o Projeto

Projeto desenvolvido como parte do portfólio de análise de dados, com foco em gestão financeira. O objetivo foi transformar dados operacionais dispersos em informação gerencial estruturada, apoiando a tomada de decisão da diretoria com base em indicadores confiáveis e visualizações claras.
