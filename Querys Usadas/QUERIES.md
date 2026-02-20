# 🔍 Documentação das Queries Power Query (M)

Documentação completa das queries M utilizadas nas tabelas do modelo **Gestão à Vista - 2026**.

As tabelas estão agrupadas por origem dos dados: **Firebird (ODBC)**, **SharePoint / Excel — Metas** e **Outras**.

---

## 📡 Fonte de Dados

### Conexão Principal — Firebird via ODBC
A maioria das tabelas dimensão e fato utiliza a conexão ODBC com DSN `POWERBI_FIREBIRD`, que aponta para o banco de dados Firebird do ERP. Todas as queries seguem o padrão:

```m
Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "<SQL>")
```

### Conexão Secundária — SharePoint / Excel
As tabelas de metas são carregadas a partir de planilhas Excel hospedadas no OneDrive/SharePoint pessoal, via `Web.Contents`.

---

## 🗄️ Tabelas — Origem Firebird (ODBC)

---

### `Cidades`
**View:** `VW_BI_GER_CIDADES`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT CID.*
FROM VW_BI_GER_CIDADES CID"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"CID_COD",        "Código"},
        {"CID_HABITANTES", "Quantidade de Habitantes"},
        {"CID_NOME",       "Cidade"},
        {"CID_UF",         "UF"},
        {"CID_COD_PAIS",   "Código País"},
        {"PA_DESC",        "País"},
        {"ES_NOME",        "Estado"}
    })
in
    Personalizar1
```

---

### `Empresas`
**View:** `VW_BI_GER_EMPRESAS`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT EMP.*
FROM VW_BI_GER_EMPRESAS EMP"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"EMP_COD", "Código"},
        {"EMP_NOM", "Empresa"}
    })
in
    Personalizar1
```

---

### `Clientes`
**View:** `VW_BI_GER_CLIDEST`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT CLI.*
FROM VW_BI_GER_CLIDEST CLI"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"CLI_DESC",      "Clientes"},
        {"CFC_GRUPOEMP",  "Chave Grupo Empresarial"},
        {"NEWLOCALIZACAO","Localização"},
        {"SITUACAO",      "Cliente - Situação"},
        {"CLI_COD",       "Código Cliente"},
        {"CLI_CODEMP",    "Código Empresa"},
        {"CLI_CNPJCPF",   "CPF ou CNPJ"},
        {"CHAVE_SEG",     "Chave Segmento"},
        {"CHAVE",         "Chave Cliente"},
        {"CHAVE_CID",     "Chave Cidade"},
        {"CHAVE_REG",     "Chave Região"}
    })
in
    Personalizar1
```

---

### `Regiões`
**View:** `VW_BI_GER_REGIAO`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT REG.*
FROM VW_BI_GER_REGIAO REG"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"REG_COD",  "Região - Código"},
        {"REG_DESC", "Região"},
        {"CHAVE",    "Chave Região"}
    })
in
    Personalizar1
```

---

### `Segmento dos Clientes`
**View:** `VW_BI_DBM_SEGMENTO`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CHAVE   AS ""Chave Segmento"",
                SEG_COD AS ""Código Segmento"",
                SEG_DESC AS ""Segmento""
         FROM VW_BI_DBM_SEGMENTO")
in
    Fonte
```

---

### `Canais de Venda`
**View:** `VW_BI_EST_CANALVENDA`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT CNL.*
FROM VW_BI_EST_CANALVENDA CNL"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"CNL_COD",  "Canal de Venda - Código"},
        {"CNL_DESC", "Canal de Venda"}
    })
in
    Personalizar1
```

---

### `Vendas`
**View:** `VW_BI_VENDAS_HASH`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT SP.*
FROM VW_BI_VENDAS_HASH SP"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"VEN_DTHR",       "Data"},
        {"VEN_EMPCOD",     "Código Empresa"},
        {"CHAVE",          "Chave Venda"},
        {"VEN_DOCUMENTO",  "Documento"},
        {"CHAVE_ALMOX",    "Chave Almoxarifado"},
        {"CHAVE_CLIENTE",  "Chave Cliente"},
        {"VEN_CNLCOD",     "Código Canal de Venda"},
        {"CHAVE_AGC",      "Chave Agente de Cobrança"},
        {"CHAVE_AGV",      "Chave Agente de Venda"},
        {"CHAVE_AGVTELE",  "Chave Agente de Tele-venda"},
        {"CHAVE_CONDPAG",  "Chave Condição de Pagamento"}
    })
in
    Personalizar1
```

---

### `Movimentos`
**View:** `VW_BI_EST_MOVIMENTO_HASH`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT SP.*
FROM VW_BI_EST_MOVIMENTO_HASH SP"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"MOV_QTD",               "Quantidade"},
        {"MOV_VLDESC",            "Desconto Comercial"},
        {"VALOR_LIQUIDO",         "Valor Líquido"},
        {"VALOR_LUCRO",           "Valor Lucro"},
        {"VALOR_TOTAL_FINAL",     "Valor de Venda"},
        {"OUTROS_VALORES",        "Outros Valores"},
        {"MXL_PERMLL",            "Percentual MLL"},
        {"DESC_PROMO",            "Desconto Promocional"},
        {"DESC_FINAN",            "Desconto Financeiro"},
        {"VALOR_BRUTO",           "Valor Bruto"},
        {"CHAVE_TABVEN",          "Chave Tabela de Venda"},
        {"MOV_TVEVLUNI",          "Tabela de Venda - Valor Unitário"},
        {"MOV_VLDESCCOFINS",      "Descontos - Cofins"},
        {"MOV_VLDESCPIS",         "Descontos - PIS"},
        {"MOV_VLDESCICMS",        "Descontos - ICMS"},
        {"MOV_VLICMSSUB",         "Impostos/Outros - ICMS Substituição"},
        {"MOV_FCPSTVAL",          "Impostos/Outros - FCP"},
        {"MOV_VLIPI",             "Impostos/Outros - IPI"},
        {"MOV_VLFRETE",           "Impostos/Outros - Frete"},
        {"MOV_VLOUTDESP",         "Impostos/Outros - Outras Despesas"},
        {"MOV_VLSEGU",            "Impostos/Outros - Seguro"},
        {"MOV_VLICMSADITOT",      "Impostos/Outros - ICMS Adicional"},
        {"MXL_CUSMEDIO",          "Custo Médio"},
        {"MXL_CUSPRODVL",         "Custo de Produção"},
        {"MXL_ULTCMPVL",          "Custo da Última Compra"},
        {"MXL_VLRCUSREP",         "Custo de Reposição de Estoque"},
        {"MOV_VALCUSREPPROD",     "Custo de Reposição x Produção"},
        {"PESO_BRUTO",            "Peso Bruto"},
        {"PESO_LIQUIDO",          "Peso Líquido"},
        {"MOV_QTD_PE",            "Quantidade - Padrão de Estoque"},
        {"CHAVE_PROD",            "CHAVE_PRODUTO"}
    })
in
    Personalizar1
```

---

### `Produtos`
**View:** `VW_BI_EST_PRODUTO_SAIDA`

> Inclui remoção de duplicatas pela chave do produto.

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT *
FROM VW_BI_EST_PRODUTO_SAIDA"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"CHAVE_DIV",        "Chave Divisão"},
        {"CHAVE_LINHA",      "Chave Linha"},
        {"CHAVE_MARCA",      "Chave Marca"},
        {"PRO_TIPO",         "Tipo do Produto"},
        {"PRO_REFERENCIA",   "Referência"},
        {"PRO_DESC",         "Produto/Serviço"},
        {"DESCRICAO",        "Descrição"},
        {"APR_DESC",         "Unidade de Medida"},
        {"PRO_COD",          "Produto/Serviço - Código"},
        {"CHAVE_CATEGPROD",  "Chave Categoria do Produto"},
        {"CHAVE_PRO",        "Chave Produto"},
        {"CHAVE_PRO_APR",    "Chave Produto APR"},
        {"PRO_ATIINA",       "Status Produto"},
        {"PRO_SEQCATALOGO",  "Sequência Catálogo"},
        {"APR_CODBAR",       "Código de Barras"}
    }),
    #"Duplicatas Removidas" = Table.Distinct(Personalizar1, {"Chave Produto"})
in
    #"Duplicatas Removidas"
```

---

### `Categoria Produtos`
**View:** `VW_BI_EST_CATEGPROD`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CHAVE_CATEGPROD   AS ""Chave Categoria Produto"",
                CHAVE_PAICAPCOD  AS ""Chave Pai Categoria Produto"",
                CAP_DESC         AS ""Categoria Produto"",
                CAP_COD          AS ""Código Categoria Produto""
         FROM VW_BI_EST_CATEGPROD CT")
in
    Fonte
```

---

### `Marca de Produtos`
**View:** `VW_BI_EST_MARCA`

> Adiciona coluna calculada `Marca Produto` que agrupa em Philozon, Ozoncare ou Outros.

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT MAR_COD  AS ""Código Marca"",
                MAR_DESC AS ""Marca""
         FROM VW_BI_EST_MARCA MA"),
    Personalizar1 = Table.AddColumn(Fonte, "Marca Produto", each
        if [Marca] = "Philozon" then "Philozon"
        else if [Marca] = "Ozoncare" then "Ozoncare"
        else "Outros"
    )
in
    Personalizar1
```

---

### `Linha de Produtos`
**View:** `VW_BI_EST_LINHAPROD`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT LIN.*
FROM VW_BI_EST_LINHAPROD LIN"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"LNP_COD",         "Linha de Produtos - Código"},
        {"LNP_DESC",        "Linha de Produtos"},
        {"CHAVE",           "Chave Linha"},
        {"LNP_CLAS",        "Classificação"},
        {"LNP_NIVEL",       "Nível"},
        {"LNP_PAILNPCOD",   "Linha de Produtos Superior - Código"},
        {"CHAVE_PAILNP",    "Chave Linha de Produto Superior"},
        {"CHAVE_AGECOM",    "Chave Agente de Compras"},
        {"LNP_PAILNPDESC",  "Linha de Produtos Superior"}
    })
in
    Personalizar1
```

---

### `Divisões de Produtos`
**View:** `VW_BI_EST_DIVEST`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT DIV.*
FROM VW_BI_EST_DIVEST DIV"),
    Personalizar1 = Table.RenameColumns(Fonte,{
        {"DIV_COD",       "Divisão de Produtos - Código"},
        {"DIV_DESC",      "Divisão de Produtos"},
        {"CHAVE",         "Chave Divisão"},
        {"DIV_CLAS",      "Classificação"},
        {"DIV_NIVEL",     "Nível"},
        {"DIV_PAIDIVCOD", "Divisão de Produtos Superior - Código"},
        {"DIV_PAIDIVDESC","Divisão de Produtos Superior"}
    })
in
    Personalizar1
```

---

### `Agentes de Cobrança`
**View:** `VW_BI_CRC_AGECOB`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CHAVE      AS ""Chave Agente de Cobrança"",
                AGECOB_COD AS ""Código Agente de Cobrança"",
                AGECOB_DESC AS ""Agente de Cobrança""
         FROM VW_BI_CRC_AGECOB AGC")
in
    Fonte
```

---

### `Agentes de Vendas`
**View:** `VW_BI_CRC_AGEVEN`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CHAVE      AS ""Chave Agente de Venda"",
                AGEVEN_COD AS ""Código Agente de Venda"",
                AGEVEN_DESC AS ""Agente de Venda"",
                SITUACAO
         FROM VW_BI_CRC_AGEVEN AGV")
in
    Fonte
```

---

### `Agentes de TeleVendas`
**View:** `VW_BI_CRC_AGEVEN_TELEVENDAS`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CHAVE      AS ""Chave Agente TeleVenda"",
                AGEVEN_COD AS ""Código Agente de TeleVendas"",
                AGEVEN_DESC AS ""Agente de TeleVendas"",
                SITUACAO
         FROM VW_BI_CRC_AGEVEN_TELEVENDAS TELE")
in
    Fonte
```

---

### `Supervisores`
**View:** `VW_BI_SUPERVISORES`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD", "SELECT SPV.*
FROM VW_BI_SUPERVISORES SPV")
in
    Fonte
```

---

### `Supervisores x Agentes`
**View:** `VW_BI_SUPERVISORESXAGV`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CHAVE_AGV AS ""Chave Agente de Venda"",
                CHAVE_SPV AS ""Chave Supervisor"",
                SUP_CODEMP AS ""Código Empresa"",
                SUP_CODAGE AS ""Código Agente"",
                SUP_CODSUP AS ""Código Supervisor""
         FROM VW_BI_SUPERVISORESXAGV SPV")
in
    Fonte
```

---

### `Agentes de Venda X Clientes`
**View:** `VW_BI_GER_CLIDESTXAGEVEN`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CLAV_EMPCOD AS ""Código Empresa"",
                CLAV_CLICOD AS ""Código Cliente"",
                CLAV_AGVCOD AS ""Código Agente de Venda"",
                CHAVE_CLI   AS ""Chave Cliente"",
                CHAVE_AGV   AS ""Chave Agente de Venda""
         FROM VW_BI_GER_CLIDESTXAGEVEN CLIXAGV")
in
    Fonte
```

---

### `Condição de Pagamento`
**View:** `VW_BI_EST_CONDPAG`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CHAVE  AS ""Chave Condição de Pagamento"",
                CP_DESC AS ""Condição de Pagamento""
         FROM VW_BI_EST_CONDPAG CP")
in
    Fonte
```

---

### `Tipos de Movimentos`
**Tabela:** `EST_TIPMOV` (acesso direto, sem view)

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT TIP_COD  AS ""Código Tipo de Movimento"",
                TIP_DESC AS ""Tipo de Movimento""
         FROM EST_TIPMOV TPM")
in
    Fonte
```

---

### `Tabela de Venda`
**View:** `VW_BI_EST_TABVEN`

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
        "SELECT CHAVE_TABVEN       AS ""Chave Tabela de Venda"",
                TVE_COD            AS ""Código Tabela de Venda"",
                TVE_DESC           AS ""Tabela de Venda"",
                TVE_DTVALIDADE     AS ""Data de Validade"",
                STATUS_TABELA_VEN  AS ""Status Tabela de Venda""
         FROM VW_BI_EST_TABVEN VW")
in
    Fonte
```

---

### `Valor a Faturar`
**Tabela:** `EST_PEDVEN` (acesso direto, sem view)

> Filtra apenas pedidos com `PEV_STATDIS = 2` (pedidos aprovados/liberados a faturar). Converte a data de emissão para `date` e o valor total para `Currency`.

```m
let
    Fonte = Odbc.Query("dsn=POWERBI_FIREBIRD",
"SELECT
  PEV_EMPCOD      AS ""Codigo Empresa"",
  PEV_ID          AS ""ID Venda"",
  PEV_DTHREMI     AS ""Data Emissão"",
  PEV_OPECOD      AS ""Código Operação"",
  PEV_CLICOD      AS ""Código Cliente"",
  PEV_AGVCOD      AS ""Código Agente de Venda"",
  PEV_AGVTELECOD  AS ""Código Agente de Televenda"",
  PEV_AGCCOD      AS ""Código"",
  PEV_VLTOTIFI    AS ""Valor Total"",
  PEV_STATDIS     AS ""Status""
FROM
  EST_PEDVEN
WHERE
  PEV_STATDIS = 2"),
    #"Data Extraída"  = Table.TransformColumns(Fonte, {{"Data Emissão", DateTime.Date, type date}}),
    #"Tipo Alterado"  = Table.TransformColumnTypes(#"Data Extraída", {{"Valor Total", Currency.Type}})
in
    #"Tipo Alterado"
```

---

## 📊 Tabelas — Origem Excel / SharePoint

Todas as planilhas são lidas via `Excel.Workbook(Web.Contents(...))` diretamente do OneDrive pessoal.

**Arquivos utilizados:**

| Arquivo | Tabelas carregadas |
|---|---|
| `Metas Canais - Negócios - Mês e Ano.xlsx` | Distribuição Metas 2026 Canais, Metas Diárias Mensais |
| `Metas Vendedores - Canais de Venda.xlsx` | Metas Vendedores (E-commerce, Outside Sales BC, Philozon BC, Philozon SP, Prescritor, V D Ozoncare) |
| `Agente de Venda x Unidade de Venda x Negócio.xlsx` | Agente de Venda X UV x Negócio |

---

### `Distribuição Metas 2026 Canais`
**Arquivo:** `Metas Canais - Negócios - Mês e Ano.xlsx` → aba `Distribuição Metas 2026 Canais`

```m
let
    Fonte = Excel.Workbook(Web.Contents("https://philozon-my.sharepoint.com/...Metas Canais - Negócios - Mês e Ano.xlsx"), null, true),
    Sheet = Fonte{[Item="Distribuição Metas 2026 Canais", Kind="Sheet"]}[Data],
    #"Cabeçalhos Promovidos" = Table.PromoteHeaders(Sheet, [PromoteAllScalars=true]),
    #"Tipo Alterado" = Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{
        {"Canal", type text}, {"Negócio", type text}, {"Código Agente Líder", Int64.Type},
        {"Líder", type text}, {"Marca", type text}, {"Meta 2026", Int64.Type},
        {"Janeiro", Currency.Type}, {"Fevereiro", Currency.Type}, {"Março", Currency.Type},
        {"Abril", Currency.Type}, {"Maio", Currency.Type}, {"Junho", Currency.Type},
        {"Julho", Currency.Type}, {"Agosto", Currency.Type}, {"Setembro", Currency.Type},
        {"Outubro", Currency.Type}, {"Novembro", Currency.Type}, {"Dezembro", Currency.Type}
    }),
    #"Linhas em Branco Removidas" = Table.SelectRows(#"Tipo Alterado",
        each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null})))
in
    #"Linhas em Branco Removidas"
```

---

### `Metas Diárias Mensais`
**Arquivo:** `Metas Canais - Negócios - Mês e Ano.xlsx` → tabela `Tabela1`

```m
let
    Fonte = Excel.Workbook(Web.Contents("https://philozon-my.sharepoint.com/...Metas Canais - Negócios - Mês e Ano.xlsx"), null, true),
    Tabela1_Table = Fonte{[Item="Tabela1", Kind="Table"]}[Data],
    #"Tipo Alterado" = Table.TransformColumnTypes(Tabela1_Table,{
        {"Mês Num",     Int64.Type},
        {"Mês",         type text},
        {"Meta Mensal", Currency.Type},
        {"Dias Úteis",  Int64.Type},
        {"Meta Diária", Currency.Type}
    })
in
    #"Tipo Alterado"
```

---

### `Agente de Venda X UV x Negócio`
**Arquivo:** `Agente de Venda x Unidade de Venda x Negócio.xlsx` → aba `Agente de Veda X UV x Negócio`

```m
let
    Fonte = Excel.Workbook(Web.Contents("https://philozon-my.sharepoint.com/...Agente de Venda x Unidade de Venda x Negócio.xlsx"), null, true),
    Sheet = Fonte{[Item="Agente de Veda X UV x Negócio", Kind="Sheet"]}[Data],
    #"Cabeçalhos Promovidos" = Table.PromoteHeaders(Sheet, [PromoteAllScalars=true]),
    #"Tipo Alterado" = Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{
        {"Agente de Venda - Código",   Int64.Type},
        {"Agente de Venda",            type text},
        {"Agente de Venda - Situação", type text},
        {"Unidade de Venda",           type text},
        {"Negócio",                    type any},
        {"Atendimento",                type text}
    }),
    #"Linhas em Branco Removidas" = Table.SelectRows(#"Tipo Alterado",
        each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null})))
in
    #"Linhas em Branco Removidas"
```

---

### `Metas Vendedores E-commerce`
**Arquivo:** `Metas Vendedores - Canais de Venda.xlsx` → aba `E-commerce`

```m
let
    Fonte = Excel.Workbook(Web.Contents("https://philozon-my.sharepoint.com/...Metas Vendedores - Canais de Venda.xlsx"), null, true),
    Sheet = Fonte{[Item="E-commerce", Kind="Sheet"]}[Data],
    #"Cabeçalhos Promovidos" = Table.PromoteHeaders(Sheet, [PromoteAllScalars=true]),
    #"Tipo Alterado" = Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{
        {"Código Agente", Int64.Type}, {"Nome Agente", type text},
        {"Data de início", type date}, {"Situação", type text},
        {"Meta Janeiro", Currency.Type}, {"Meta Fevereiro", Currency.Type},
        {"Meta Março", Currency.Type},  {"Meta Abril", Currency.Type},
        {"Meta Maio", Currency.Type},   {"Meta  Junho", Currency.Type},
        {"Meta  Julho", Currency.Type}, {"Meta Agosto", Currency.Type},
        {"Meta  Setembro", Currency.Type}, {"Meta Outubro", Currency.Type},
        {"Meta Novembro", Currency.Type}, {"Meta Dezembro", Currency.Type}
    }),
    #"Linhas em Branco Removidas" = Table.SelectRows(#"Tipo Alterado",
        each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null})))
in
    #"Linhas em Branco Removidas"
```

> **Nota:** O mesmo padrão se aplica às tabelas abaixo, variando apenas o nome da aba lida.

---

### `Metas Vendedores Outside Sales BC`
**Aba:** `Outside Sales BC`

---

### `Metas Vendedores Philozon BC`
**Aba:** `Philozon Bc`

---

### `Metas Vendedores Philozon SP`
**Aba:** `Philozon SP`

---

### `Metas Vendedores Prescritor`
**Aba:** `Prescritor`

> Diferença: colunas de meta tipadas como `Int64.Type` (em vez de `Currency.Type`).

---

### `Metas Vendedores V D Ozoncare`
**Aba:** `V.D Ozoncare`

---

## 📝 Resumo dos Padrões Utilizados

| Padrão | Descrição |
|---|---|
| `Odbc.Query("dsn=POWERBI_FIREBIRD", ...)` | Leitura via ODBC do banco Firebird |
| `Table.RenameColumns(...)` | Renomeia colunas do ERP para nomes amigáveis |
| `Table.Distinct(..., {"Chave Produto"})` | Remove duplicatas (usado em Produtos) |
| `Excel.Workbook(Web.Contents(...))` | Leitura de Excel do SharePoint/OneDrive |
| `Table.PromoteHeaders(...)` | Promove primeira linha como cabeçalho |
| `Table.TransformColumnTypes(...)` | Define tipos explícitos das colunas |
| `Table.TransformColumns(..., DateTime.Date)` | Extrai somente a parte de data de um DateTime |
| `Table.SelectRows(... RemoveMatchingItems ...)` | Remove linhas completamente em branco |
| `Table.AddColumn(...)` | Adiciona coluna calculada no Power Query (ex.: `Marca Produto`) |
| `WHERE PEV_STATDIS = 2` | Filtro SQL para pedidos a faturar (status 2 = liberado) |

---

*Documentação gerada automaticamente via MCP Power BI Modeling — 20/02/2026*
