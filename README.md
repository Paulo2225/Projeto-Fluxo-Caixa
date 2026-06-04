# Projeto Fluxo de Caixa

## 1. Entendimento do Cenário de Negócio

O fluxo de caixa é uma ferramenta de gestão financeira que registra todas as entradas e saídas de dinheiro de uma empresa em determinado período. Ele mostra de forma organizada quanto a empresa recebe (como vendas, investimentos ou empréstimos) e quanto gasta (como despesas operacionais, salários, fornecedores e impostos). Esse controle permite visualizar se há saldo positivo ou negativo, ajudando a entender a saúde financeira do negócio.

A principal utilidade do fluxo de caixa é apoiar na tomada de decisões estratégicas, pois permite prever momentos de sobra ou falta de recursos e planejar ações para evitar problemas financeiros. É utilizado por gestores, empreendedores, contadores e até investidores, já que fornece uma visão clara da capacidade da empresa de honrar seus compromissos e gerar lucro. Em resumo, é uma ferramenta essencial para manter o equilíbrio financeiro e garantir a sustentabilidade do negócio.

---

## 2. Justificativa do Projeto

1. Será desenvolvido um modelo de fluxo de caixa, estruturado para organizar e acompanhar todas as entradas e saídas financeiras de uma empresa em determinado período. O projeto contemplará a construção de relatórios que permitam visualizar o saldo disponível, identificar padrões de movimentação e projetar cenários futuros.

2. A implementação de um fluxo de caixa é essencial para garantir o equilíbrio financeiro e apoiar a tomada de decisões estratégicas. Muitas organizações enfrentam dificuldades por não terem clareza sobre sua liquidez ou por dependerem apenas de registros manuais. Com o fluxo de caixa, é possível antecipar momentos de escassez ou sobra de recursos, planejar investimentos e evitar problemas como atrasos em pagamentos ou falta de capital de giro. O projeto agrega valor porque fortalece a gestão financeira e aumenta a transparência das informações.

3. O desenvolvimento será realizado utilizando dados financeiros estruturados em planilhas ou sistemas de gestão, que servirão como fonte para a construção do modelo. A ferramenta escolhida para a execução é o **Power BI**, por oferecer recursos avançados de integração de dados, atualização automática e visualizações interativas. O uso do Power BI se justifica porque transforma informações financeiras em relatórios dinâmicos e acessíveis, permitindo análises mais rápidas, comparativas e estratégicas, o que amplia significativamente o valor do fluxo de caixa para a gestão empresarial.

---

## 3. Documentação Técnica

### Fonte de Dados

Os dados utilizados neste projeto foram disponibilizados por meio de arquivos em formato Excel, representando as principais informações financeiras necessárias para a construção do fluxo de caixa. Cada arquivo possui uma finalidade específica dentro do modelo de dados, conforme descrito a seguir.

- **Bancos.xlsx** — arquivo responsável por armazenar o cadastro das instituições bancárias utilizadas no controle financeiro. Essa base fornece o contexto necessário para análises de saldo e movimentações por banco.

- **PlanoContas.xlsx** — arquivo que contém a estrutura de contas e subgrupos financeiros. Essa base é utilizada para classificar as movimentações, permitindo análises detalhadas por categoria e facilitando a organização do fluxo de caixa.

- **Movimentos.xlsx** — arquivo que registra as movimentações financeiras realizadas, incluindo entradas e saídas de recursos. Essa base constitui a principal fonte de dados do projeto, sendo utilizada como tabela fato no modelo dimensional.

- **SaldoAnterior.xlsx** — arquivo que armazena os saldos anteriores ao período analisado. Essa base é utilizada no cálculo do saldo inicial, garantindo continuidade e consistência na análise do fluxo de caixa ao longo do tempo.

---

### 3.1 Conectando na Fonte de Dados

No Power Query foi conduzido o processo de extração, tratamento e carregamento dos dados, assegurando que as informações financeiras estivessem limpas, organizadas e padronizadas antes de serem analisadas no Power BI. Essa etapa é fundamental para garantir a confiabilidade dos relatórios, já que elimina inconsistências e prepara a base de dados para gerar visualizações precisas e úteis ao negócio.

**Configuração do diretório de dados com uso de parâmetro**

Foi criado um parâmetro responsável por armazenar o caminho da pasta do projeto, permitindo que o Power Query se conecte automaticamente ao local onde estão os arquivos de origem (Gerenciar parâmetros > Novo parâmetro > PastaProjeto).

- **Perspectiva de negócio:** essa configuração garante que, caso os arquivos sejam movidos para outro diretório, basta atualizar o parâmetro, sem necessidade de refazer todas as conexões. Isso reduz esforço de manutenção e evita erros em relatórios financeiros.

Na sequência, configurou-se uma consulta auxiliar chamada **"Diretório"**, com a carga desativada, que funciona como referência para os arquivos do projeto. Essa prática simplifica a manutenção e torna o processo mais automatizado, já que todos os arquivos podem ser acessados a partir dessa consulta.

- **Perspectiva de negócio:** manter essa consulta apenas como referência evita sobrecarga no modelo e garante que o projeto permaneça leve, com tempo de atualização menor.

**Passos para criação da consulta "Diretório":**

- Acessar o diretório (Nova fonte > Mais > Pasta > Conectar > Parâmetros > PastaProjetoFluxoCaixa)
- Visualizar a lista de arquivos disponíveis
- Selecionar Transformar dados
- Renomear a consulta para "Diretório" e desabilitar a carga

> **Observação Importante:** A carga foi desativada para otimizar o desempenho e manter o modelo mais organizado. Como essa consulta serve apenas como referência, o Power BI não precisa armazená-la como tabela, o que reduz o tempo de atualização e economiza memória, deixando o projeto mais leve e eficiente.

---

### 3.2 Estruturação das Tabelas de Fato e Dimensão

Nesta etapa foram importadas e estruturadas as tabelas de Fato e Dimensão, aplicando boas práticas de parametrização, clareza na escrita das etapas e organização das consultas no Power Query.

**Tabela fatoMovimentos**
- Origem: consulta Diretório → arquivo Movimentos.xlsx → tabela tbMovimentos
- Nome da consulta: fatoMovimentos
- Boa prática: uso do Editor Avançado para renomear etapas da linguagem M, garantindo clareza e documentação.
- **Perspectiva de negócio:** essa tabela concentra os registros financeiros (entradas e saídas), sendo a base principal para análises de fluxo de caixa.

**Tabela dimBancos**
- Criada por duplicação da consulta fatoMovimentos
- Ajuste da origem: arquivo Bancos.xlsx → tabela tbBancos
- Nome da consulta: dimBancos
- **Perspectiva de negócio:** a dimensão de bancos permite segmentar os movimentos por instituição financeira, facilitando análises comparativas entre contas.

**Tabela dimContas**
- Criada por duplicação da consulta dimBancos
- Ajuste da origem: arquivo PlanoContas.xlsx → tabela tbContas
- Nome da consulta: dimContas
- **Perspectiva de negócio:** essa dimensão organiza os movimentos por tipo de conta (receita, despesa, ativo, passivo), permitindo análises estruturadas do plano contábil.

**Tabela fatoSaldoAnterior**
- Criada por duplicação da consulta dimContas
- Ajuste da origem: arquivo SaldoAnterior.xlsx → tabela tbSaldoAnterior
- Nome da consulta: fatoSaldoAnterior
- **Perspectiva de negócio:** essa tabela registra os saldos iniciais, essenciais para calcular corretamente o fluxo acumulado e validar a consistência dos relatórios financeiros.

**Tabela dimCalendario**

Nesta etapa foi construída a tabela de dimensão calendário, fundamental para análises financeiras, pois permite relacionar os movimentos de caixa com períodos (dias, meses, anos). Essa tabela é indispensável para cálculos de saldos acumulados, comparações mensais e análises de sazonalidade.

- Criado o parâmetro `DataInicial`, do tipo Data, com valor atual definido como 02/01/2023.
  - **Perspectiva de negócio:** define o início do período de análise, evitando lacunas nos relatórios e garantindo consistência nos acumulados.

![Calendario parte 1](Imagens/Calendariopart1.png)

- Criado o parâmetro `DataFinal`, do tipo Data, com valor dinâmico baseado na data atual do sistema, utilizando a expressão `Date.From(DateTime.LocalNow())`.
  - **Perspectiva de negócio:** ao usar a data atual, o calendário se atualiza automaticamente sem necessidade de intervenção manual.

![Calendario parte 2](Imagens/Calendariopart2.png)

- Com os dois parâmetros definidos, gera-se uma lista de números inteiros que representam cada dia dentro do intervalo utilizando a expressão `{ Number.From(DataInicial) .. Number.From(DataFinal) }`, etapa nomeada como **Lista**.
  - **Perspectiva de negócio:** essa abordagem garante que todos os dias do período sejam contemplados, evitando lacunas que poderiam comprometer cálculos acumulados.

![Calendario parte 3](Imagens/Calendariopart3.png)

- A lista numérica é convertida em datas legíveis com a função `Date.From`, utilizando a expressão `List.Transform(Lista, Date.From)`, etapa nomeada como **Datas**.
  - **Perspectiva de negócio:** essa conversão permite que os dados sejam reconhecidos como datas válidas pelo modelo, habilitando ordenações cronológicas e segmentações por período.

![Calendario parte 4](Imagens/Calendariopart4.png)

- Com a lista de datas pronta, estrutura-se a tabela final utilizando o Editor Avançado, por meio da função `#table`, criando as colunas: `Data`, `Ano`, `Mês`, `MesAbrev`, `MesNum` e `Dia`.
  - **Perspectiva de negócio:** o uso da cultura `pt-BR` assegura que os nomes e abreviações dos meses sejam exibidos em português, evitando inconsistências nos relatórios.

![Calendario parte 5](Imagens/Calendariopart5.png)

![Calendario parte 6](Imagens/Calendariopart6.png)

---

### 3.3 Transformações Aplicadas nas Tabelas

**Tabela dBancos**

Objetivo: alterar o tipo de dados da coluna `Banco_ID` de Número Decimal para Número Inteiro, garantindo que seja tratada como identificador numérico no modelo.

- Na tabela **dBancos**, localizar a coluna `Banco_ID`
- Clicar no ícone de tipo de dados ao lado do nome da coluna
- Selecionar a opção **Número Inteiro** no menu de tipos disponíveis

![dBancos 1](Imagens/dBancos1.png) ![dBancos 2](Imagens/dBancos2.png) ![dBancos 3](Imagens/dBancos3.png)

**Perspectiva de negócio:**
- A coluna `Banco_ID` representa um identificador único
- Para garantir consistência nos relacionamentos e evitar problemas em cálculos, o tipo de dados foi ajustado para Número Inteiro

---

**Tabela dContas**

Objetivo: alterar o tipo de dados das colunas `Subgrupo_ID` e `Conta_ID` de Número Decimal para Número Inteiro.

- Na tabela **dContas**, marcar as colunas `Subgrupo_ID` e `Conta_ID` juntas
- Ir na aba **Transformar**
- Ir em **Tipo de Dados**
- Alterar de Número Decimal para Número Inteiro

![dContas 1](Imagens/dContas1.png) ![dContas 2](Imagens/dContas2.png) ![dContas 3](Imagens/dContas3.png)

**Perspectiva de negócio:**
- As colunas `Subgrupo_ID` e `Conta_ID` representam identificadores únicos
- Para garantir consistência nos relacionamentos e evitar problemas em cálculos, o tipo de dados foi ajustado para Número Inteiro

---

**Tabela fMovimentos**

**Junção com a tabela dContas:**
- Foi utilizada a função `Table.Join` para unir a tabela Excel com a tabela dContas
- A junção foi feita pela coluna Conta (texto)
- Foram selecionadas apenas as colunas `Conta_ID` e `Conta` da tabela dContas

![fMovimentos 1](Imagens/fMovimentos1.png)

![fMovimentos 2](Imagens/fMovimentos2.png)

**Junção com a tabela dBancos:**
- Foi utilizada a função `Table.Join` para unir a tabela JoinContaID com a tabela dBancos
- A junção foi feita pela coluna Banco (texto)

![fMovimentos 3](Imagens/fMovimentos3.png)

![fMovimentos 4](Imagens/fMovimentos4.png)

**Limpeza e transformação de colunas:**
- Na aba Página Inicial, foi utilizada a função **Remover Colunas** para manter apenas: `Data`, `Banco_ID`, `Conta_ID`, `Tipo` e `Valor`

![fMovimentos 5](Imagens/fMovimentos5.png)

- Na aba **Transformar**, foi aplicada a função **Extrair → Primeiros caracteres** na coluna `Tipo`, configurando para extrair apenas 1 caractere
- Com essa mudança, a coluna passou a mostrar apenas **E** (Entradas) e **S** (Saídas)

![fMovimentos 6](Imagens/fMovimentos6.png)

- Na coluna `Valor`, foi alterado o tipo de dados para **Número decimal fixo**, garantindo precisão nos cálculos financeiros

![fMovimentos 7](Imagens/fMovimentos7.png)

**Perspectiva de negócio:**
- A junção com dContas trouxe o `Conta_ID`, permitindo que os relatórios se relacionem de forma confiável com a dimensão de contas
- A junção com dBancos trouxe o `Banco_ID`, garantindo que cada movimento esteja vinculado corretamente ao banco de origem
- A limpeza de colunas deixou a tabela fato apenas com os campos essenciais, reduzindo complexidade e melhorando performance
- A transformação da coluna Tipo em apenas E e S simplificou a categorização das transações
- O ajuste da coluna Valor para número decimal fixo assegurou precisão nos cálculos financeiros

---

**Tabela fSaldoAnterior**

- Na coluna `Banco_ID`, o tipo de dados foi alterado de Número Decimal para Número Inteiro
- Na coluna `Valor`, o tipo de dados foi alterado de Número Decimal para Número Decimal Fixo

![fSaldoAnterior 1](Imagens/fSaldoAnterior1.png)

![fSaldoAnterior 2](Imagens/fSaldoAnterior2.png)

**Perspectiva de negócio:** essas transformações asseguram que o saldo inicial seja corretamente incorporado ao fluxo de caixa, evitando distorções nos valores apresentados nos dashboards.

---

**Modelo de Dados**

O modelo foi estruturado em formato dimensional do tipo **estrela**, no qual a tabela `fMovimentos` atua como fato principal, concentrando os registros de entradas e saídas de caixa. As dimensões `dBancos`, `dContas` e `dCalendario` fornecem o contexto analítico para as movimentações. A tabela `fSaldoAnterior` se relaciona à dimensão `dBancos`, sendo utilizada para compor o saldo inicial do fluxo de caixa.

![Relacionamentos](Imagens/Relacionamentos.png)

---

### 3.4 Dicionário de Dados

**dBancos**
- Finalidade da tabela: Tabela dimensão que armazena o cadastro das instituições financeiras, permitindo consolidar e comparar saldos e movimentações por banco.

| Coluna | Tipo | Descrição | Relacionamentos |
|---|---|---|---|
| Banco_ID | Inteiro | Identificador do banco | Um para muitos com fMovimentos (sentido único) |
| Banco | Texto | Nome do banco | Relacionado via Banco_ID |

---

**dContas**
- Finalidade da tabela: Tabela dimensão que organiza as contas financeiras e seus subgrupos, viabilizando análises detalhadas das entradas e saídas por tipo de conta.

| Coluna | Tipo | Descrição | Relacionamentos |
|---|---|---|---|
| Conta_ID | Inteiro | Identificador da conta | Um para muitos com fMovimentos (sentido único) |
| Conta | Texto | Nome da conta | Relacionado via Conta_ID |
| Subgrupo | Texto | Classificação da conta | Utilizado em segmentações e análises |

---

**dCalendario**
- Finalidade da tabela: Tabela dimensão responsável por padronizar o controle temporal do modelo, permitindo análises por dia, mês e ano e garantindo consistência nos filtros de tempo.

| Coluna | Tipo | Descrição | Relacionamentos |
|---|---|---|---|
| Data | Data | Data de referência | Relacionamento um para muitos com fMovimentos (sentido único) |
| Ano | Inteiro | Ano da data | Derivado da coluna Data |
| Mes | Texto | Nome do mês | Derivado da coluna Data |
| MesAbrev | Texto | Abreviação do mês | Derivado da coluna Data |
| MesNum | Inteiro | Número do mês | Derivado da coluna Data |
| Dia | Inteiro | Dia do mês | Derivado da coluna Data |

---

**fMovimentos**
- Finalidade da tabela: Tabela fato responsável por registrar todas as movimentações financeiras do fluxo de caixa, incluindo entradas e saídas associadas a datas, bancos e contas.

| Coluna | Tipo | Descrição | Relacionamentos |
|---|---|---|---|
| Data | Data | Data do movimento | Muitos para um com dCalendario (sentido único) |
| Banco_ID | Inteiro | Banco relacionado | Muitos para um com dBancos (sentido único) |
| Conta_ID | Inteiro | Conta relacionada | Muitos para um com dContas (sentido único) |
| Tipo | Texto | Entrada (E) ou Saída (S) | Utilizado no cálculo das medidas |
| Valor | Decimal | Valor financeiro do movimento | Base para os cálculos financeiros |

---

**fSaldoAnterior**
- Finalidade da tabela: Tabela fato complementar utilizada para armazenar o saldo acumulado anterior ao período analisado, garantindo o cálculo correto do saldo inicial.

| Coluna | Tipo | Descrição | Relacionamentos |
|---|---|---|---|
| Banco_ID | Inteiro | Banco relacionado | Muitos para um com dBancos (sentido único) |
| Valor | Decimal | Saldo anterior ao período | Utilizado no cálculo do Saldo Inicial |

---

## 4. Mapeamento de KPIs e Medidas DAX

No projeto foi adotada uma metodologia estruturada para o mapeamento de KPIs, com o objetivo de garantir alinhamento entre os indicadores desenvolvidos e as necessidades do negócio. O processo iniciou-se pela identificação do stakeholder, ou seja, quem utilizará as informações geradas para tomada de decisão. Em seguida, foi definido o KPI a ser monitorado, considerando a principal pergunta de negócio que o projeto deveria responder. Após a definição do KPI, foi mapeado o processo de negócio relacionado ao indicador, assegurando que a métrica represente fielmente a operação analisada. Na sequência, foram identificadas as fontes de dados necessárias para o cálculo do KPI, garantindo consistência e confiabilidade das informações utilizadas. Por fim, foi definido o racional de cálculo, estabelecendo de forma clara a lógica aplicada na construção do indicador, facilitando o entendimento, a validação e a manutenção do modelo analítico.

---

### 4.1 Definição do KPI Principal

- O KPI principal definido para este projeto é o **Saldo Final de Caixa**
- Este indicador foi escolhido por representar diretamente a posição financeira da empresa ao final de cada período, sendo o principal termômetro da saúde do caixa

---

### 4.2 Processo de Negócio Monitorado

O Saldo Final monitora o processo de gestão do fluxo de caixa, considerando:

- Entradas de recursos (recebimentos)
- Saídas de recursos (pagamentos)
- Saldo disponível no início do período
- Resultado operacional ao longo do tempo

Esse processo é essencial para o controle financeiro e para a tomada de decisão.

---

### 4.3 Fontes de Dados Utilizadas

Os dados utilizados para o cálculo dos KPIs são provenientes das seguintes tabelas:

- **fMovimentos** — registros de entradas e saídas
- **fSaldoAnterior** — saldo existente antes do período analisado
- **dCalendario** — controle temporal (datas, meses, anos)

---

### 4.4 Regra de Cálculo do KPI Principal

**Saldo Final = Saldo Inicial + Saldo Operacional**

Onde:
- **Saldo Inicial** representa o valor disponível antes do início do período analisado
- **Saldo Operacional** representa o resultado líquido das movimentações do período

---

### 4.5 KPIs Utilizados no Projeto

- **Entradas** — total de valores recebidos
- **Saídas** — total de valores pagos
- **Saldo Operacional** — resultado líquido do período
- **Saldo Inicial** — posição inicial de caixa
- **Saldo Final** — posição final de caixa

---

### 4.6 Tabela de Mapeamento dos KPIs

![Tabela de Mapeamento dos KPIs](Imagens/Tabela_de_Mapeamento_dos_KPIs.png)

| KPI | Processo Monitorado | Fonte de Dados | Regra de Cálculo |
|---|---|---|---|
| Entradas | Recebimentos | fMovimentos | Soma de Valor onde Tipo = "E" |
| Saídas | Pagamentos | fMovimentos | Soma de Valor onde Tipo = "S" |
| Saldo Operacional | Resultado operacional do período | fMovimentos | Soma de todas as movimentações |
| Saldo Inicial | Saldo antes do período analisado | fMovimentos, fSaldoAnterior | Movimentos anteriores + Saldo Anterior |
| Saldo Final | Posição final de caixa | fMovimentos, fSaldoAnterior | Saldo Inicial + Saldo Operacional |

---

### 4.7 Documentação das Medidas DAX

**Medida: Entradas**
```dax
Entradas =
    CALCULATE(
        SUM(fMovimentos[Valor]),
        fMovimentos[Tipo] = "E"
    )
```
- **Explicação técnica:** esta medida utiliza a função CALCULATE para somar a coluna Valor da tabela fMovimentos, aplicando um filtro que considera apenas registros classificados como entradas.
- **Finalidade de negócio:** mede todos os valores recebidos no período, permitindo acompanhar a capacidade de geração de caixa.

---

**Medida: Saídas**
```dax
Saídas =
    CALCULATE(
        SUM(fMovimentos[Valor]),
        fMovimentos[Tipo] = "S"
    )
```
- **Explicação técnica:** a função CALCULATE é utilizada para somar os valores da tabela fMovimentos, considerando apenas os registros classificados como saídas.
- **Finalidade de negócio:** mede todos os pagamentos realizados, permitindo controlar despesas e compromissos financeiros.

---

**Medida: Saídas Abs**
```dax
Saídas Abs = ABS([Saídas])
```
- **Explicação técnica:** a função ABS converte os valores negativos das saídas em valores absolutos, facilitando a análise visual.
- **Finalidade de negócio:** evita interpretações incorretas em gráficos e relatórios, tornando a análise mais clara para usuários de negócio.

---

**Medida: Saldo Operacional**
```dax
Saldo Operacional = SUM(fMovimentos[Valor])
```
- **Explicação técnica:** esta medida soma todos os valores de movimentação no período, considerando entradas e saídas.
- **Finalidade de negócio:** representa o resultado líquido das movimentações no período, indicando se houve geração ou consumo de caixa.

---

**Medida: Saldo Inicial**
```dax
Saldo Inicial =
    CALCULATE(
        SUM(fMovimentos[Valor]),
        dCalendario[Data] < MIN(dCalendario[Data])
    )
    + SUM(fSaldoAnterior[Valor])
```
- **Explicação técnica:** a medida calcula o saldo acumulado antes do período selecionado e soma o saldo anterior, garantindo continuidade histórica.
- **Finalidade de negócio:** garante que a análise do fluxo de caixa comece com o valor real disponível antes do período selecionado.

---

**Medida: Saldo Final**
```dax
Saldo Final = [Saldo Inicial] + [Saldo Operacional]
```
- **Explicação técnica:** a medida soma o saldo inicial com o resultado operacional do período.
- **Finalidade de negócio:** indica a posição real de caixa ao final do período, sendo o principal indicador financeiro do projeto.

---

**Medida: Fluxo**
```dax
Fluxo =
VAR __GrupoID = SELECTEDVALUE(dGrupos[Grupo_ID])
RETURN
    IF(
        __GrupoID IN {3, 4, 5} && ISINSCOPE(dContas[Subgrupo]),
        BLANK(),
        SWITCH(
            SELECTEDVALUE(dGrupos[Grupo_ID]),
            1, [Entradas],
            2, [Saídas],
            3, [Saldo Operacional],
            4, [Saldo Inicial],
            5, [Saldo Final]
        )
    )
```
- **Explicação técnica:** esta medida utiliza variáveis, funções de contexto e controle de escopo para retornar dinamicamente diferentes métricas conforme o grupo selecionado. O uso de ISINSCOPE impede a exibição indevida de valores em níveis de detalhe onde a informação não é aplicável.
- **Finalidade de negócio:** centraliza a lógica do fluxo de caixa em uma única medida, permitindo a apresentação consolidada de entradas, saídas e saldos em uma mesma visualização, de forma organizada e coerente para análise gerencial.

---

## 5. Análise dos Gráficos e Entrega de Valor

### 5.1 Visão Geral do Dashboard

O dashboard de Demonstração de Fluxo de Caixa foi desenvolvido para consolidar, em uma única visualização, os principais indicadores financeiros e análises relacionadas às entradas, saídas e saldos da empresa ao longo do período analisado.

A partir dessa visão geral, é possível identificar rapidamente a posição financeira da empresa, compreender o comportamento do fluxo de caixa e direcionar a análise para os gráficos específicos apresentados nas seções seguintes.

![Dashboard](Imagens/Dashboard.png)

---

### 5.2 Entradas e Saídas por Ano e Mês

**Medidas e colunas utilizadas:**
- Medida Entradas
- Medida Saídas Abs
- Colunas da tabela dCalendario: Ano e Mês

![Entradas e Saídas por Ano e Mês](Imagens/Entradas_e_Sa%C3%ADdas_por_%20Ano_e_M%C3%AAs.png)

**O que o gráfico mostra:**
Este gráfico apresenta a evolução mensal das entradas e das saídas financeiras ao longo do período analisado. As entradas são exibidas como valores positivos e as saídas são apresentadas em valor absoluto, facilitando a comparação visual entre os fluxos de entrada e saída de caixa.

**Insights acionáveis:**
- A comparação direta entre entradas e saídas permite identificar meses em que o consumo de caixa se aproxima ou supera a geração de recursos
- Meses com saídas elevadas em relação às entradas indicam maior pressão financeira
- A diretoria pode utilizar essa informação para ajustar o planejamento do fluxo de caixa e antecipar ações de controle financeiro

---

### 5.3 Saldo por Banco

**Medidas e colunas utilizadas:**
- Medida Saldo Final
- Colunas da tabela dBancos: Banco

![Saldo por Banco](Imagens/Saldo_por_Banco.png)

**O que o gráfico mostra:**
O gráfico apresenta o saldo final consolidado por instituição bancária, evidenciando a distribuição dos recursos financeiros entre os bancos.

**Insights acionáveis:**
- Identificação de bancos com saldo negativo ou concentração excessiva de recursos
- Possibilidade de redistribuição de valores entre contas para reduzir riscos financeiros
- Apoio à tomada de decisão sobre manutenção, encerramento ou renegociação de contas bancárias

---

### 5.4 Entradas por Subgrupo

**Medidas e colunas utilizadas:**
- Medida Entradas
- Colunas da tabela dContas: Subgrupo e Conta

![Entradas por Subgrupo](Imagens/Entradas_por_Subgrupo.png)

**O que o gráfico mostra:**
Este gráfico apresenta a composição das entradas financeiras, agrupadas por subgrupo e conta, permitindo identificar quais tipos de receitas mais contribuem para a geração de caixa.

**Insights acionáveis:**
- Observa-se concentração das entradas em contas específicas dentro de determinados subgrupos
- Essa concentração indica dependência de poucas fontes de receita
- A diretoria pode utilizar essa informação para avaliar riscos e oportunidades de diversificação

---

### 5.5 Saídas por Subgrupo

**Medidas e colunas utilizadas:**
- Medida Saídas Abs
- Colunas da tabela dContas: Subgrupo e Conta

![Saídas por Subgrupo](Imagens/Sa%C3%ADdas_por_Subgrupo.png)

**O que o gráfico mostra:**
Este gráfico apresenta a composição das saídas financeiras, agrupadas por subgrupo e conta, permitindo identificar quais tipos de despesas mais impactam o caixa da empresa.

**Insights acionáveis:**
- Observa-se concentração das saídas em subgrupos e contas específicas
- Identificação clara dos principais centros de custo da operação
- A diretoria pode priorizar ações de controle de gastos e renegociação de despesas com maior impacto financeiro

---

### 5.6 Saldo Operacional por Ano e Mês

**Medidas e colunas utilizadas:**
- Medida Saldo Operacional
- Colunas da tabela dCalendario: Ano e Mês

![Saldo Operacional por Ano e Mês](Imagens/Saldo_Operacional_por_Ano_e_M%C3%AAs.png)

**O que o gráfico mostra:**
Este gráfico apresenta a evolução mensal do saldo operacional ao longo dos anos analisados, evidenciando os períodos em que a operação gerou resultado positivo ou negativo. O saldo operacional representa a diferença entre entradas e saídas em cada mês, permitindo avaliar a eficiência financeira da operação no curto prazo. Observa-se a alternância entre meses com resultado positivo e negativo, bem como a intensidade dessas variações ao longo do tempo.

**Insights acionáveis:**
- A recorrência de meses com saldo operacional negativo indica a necessidade de maior controle sobre despesas operacionais
- Meses com saldo positivo consistente podem servir como referência para identificar práticas financeiras mais eficientes
- O resultado negativo mais acentuado no período recente acende um alerta para revisão imediata do planejamento financeiro
- A diretoria pode utilizar essa análise para antecipar períodos críticos e adotar ações preventivas, como ajuste de custos ou reforço de caixa

---

### 5.7 Matriz de Detalhamento

**Medidas e colunas utilizadas:**
- Medidas: Fluxo, Saldo Inicial e Saldo Final
- Colunas da tabela dCalendario: Ano e Mês
- Colunas da tabela dContas: Subgrupo e Conta
- Coluna da tabela dGrupos: Grupo

![Matriz de Detalhamento](Imagens/Matriz_de_Detalhamento.png)

**O que a matriz mostra:**
A matriz apresenta o detalhamento mensal do fluxo de caixa, exibindo as entradas, saídas e saldos organizados por grupo, subgrupo e conta. Essa visualização permite acompanhar de forma estruturada como cada categoria financeira contribui para o resultado do caixa ao longo do período analisado.

**Insights acionáveis:**
- Permite identificar com precisão quais grupos e contas são responsáveis pela geração ou consumo de caixa em cada mês
- Facilita a detecção de variações relevantes em períodos específicos, apoiando análises de desvios financeiros
- Apoia a diretoria em decisões mais operacionais, como revisão de despesas específicas, controle de contas críticas e priorização de ações corretivas

---

## 6. Entrega de Valor

A entrega deste projeto proporciona uma visão clara, estruturada e confiável do fluxo de caixa da empresa, consolidando informações financeiras que antes poderiam estar dispersas ou de difícil interpretação. Por meio do dashboard desenvolvido, a diretoria passa a ter acesso rápido aos principais indicadores financeiros, como entradas, saídas, saldo operacional, saldo inicial e saldo final, facilitando o acompanhamento da saúde financeira do negócio.

O projeto contribui diretamente para a melhoria do processo de tomada de decisão, ao transformar dados operacionais em informações gerenciais. A análise por período, banco, conta e subgrupo permite identificar padrões de comportamento financeiro, pontos de atenção relacionados a custos elevados e oportunidades de otimização do uso do caixa. Dessa forma, decisões deixam de ser baseadas apenas em percepção e passam a ser sustentadas por dados consistentes.

Outro valor entregue é a padronização e organização dos dados financeiros. A construção de um modelo dimensional, aliada a um dicionário de dados bem documentado, garante maior confiabilidade das informações, reduz riscos de interpretações incorretas e facilita a manutenção e evolução do projeto no futuro. Isso também permite que outros usuários compreendam o modelo com mais facilidade, mesmo sem conhecimento prévio da base.

Por fim, o projeto estabelece uma base sólida para a implantação de uma cultura orientada a dados. Ao centralizar e estruturar as informações financeiras em um ambiente visual e interativo, a empresa passa a ter maior controle sobre seu fluxo de caixa, maior transparência nas análises e melhores condições para planejamento financeiro, contribuindo para uma gestão mais eficiente e sustentável.
