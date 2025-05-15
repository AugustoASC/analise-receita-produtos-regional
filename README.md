# 📊 Desafio DAX - Ranking de Produtos por Receita com Filtro de Região

Este projeto apresenta um desafio prático de DAX no Power BI, cujo objetivo é calcular o **ranking de produtos por receita**, levando em consideração a aplicação (ou não) de **filtros por região**. O exercício tem como foco o uso das funções `RANKX`, `ALL`, `VALUES`, `CALCULATE` e `ISFILTERED`.

---

## 🎯 Desafio Proposto

**Calcular o ranking de produtos com base na receita total, considerando duas situações:**

1. **Sem filtro de região aplicado:**  
   O ranking deve ser **global**, considerando a receita total de todos os produtos em todas as regiões.

2. **Com filtro de região aplicado:**  
   O ranking deve ser **local**, considerando apenas os produtos vendidos na **região filtrada**.

---

## 🧠 Lógica DAX Utilizada

### 🔹 Medida 1 – Receita Total

```dax
Receita Total = SUM('Vendas'[Receita])
```

### 🔹 Medida 2 – Ranking Receita Produto

```dax
Ranking Receita Produto = 
VAR RegiaoFiltrada = ISFILTERED('Vendas'[Regiao])
VAR TabelaRanking = 
    IF(
        RegiaoFiltrada,
        VALUES('Vendas'[Produto]),       -- Produtos na região filtrada
        ALL('Vendas'[Produto])           -- Todos os produtos (ranking global)
    )
RETURN
RANKX(
    TabelaRanking,
    CALCULATE([Receita Total]),
    ,
    DESC,
    DENSE
)
```

---

## 🧪 Como Testar no Power BI

1. Importe a base de dados `Desafio_DAX_Vendas.xlsx` incluída neste repositório para o Power BI.
2. Crie um visual de **tabela** com as seguintes colunas:
   - Produto  
   - Região  
   - Receita Total  
   - Ranking Receita Produto
3. Adicione um **filtro (slicer)** de Região no relatório.

📌 **Comportamento esperado:**
- **Sem seleção de região:** O ranking é global.
- **Com região selecionada:** O ranking é recalculado apenas para os produtos daquela região (ranking local).

---
