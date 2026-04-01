# Analise-SQL-TheLook-Ecommerce
Análise exploratória de dados de e-commerce utilizando SQL no Google BigQuery. Resolução de problemas de negócio, limpeza de dados e cálculos estatísticos.
# Análise de Dados: E-commerce TheLook (SQL)

Este projeto contém uma série de análises realizadas em SQL utilizando o Google BigQuery sobre o dataset público `thelook_ecommerce`. O objetivo é responder perguntas de negócio e extrair insights sobre o comportamento dos clientes e saúde financeira da empresa.

## Tecnologias Utilizadas
* **Google BigQuery** (Processamento de dados em nuvem)
* **SQL** (Linguagem de consulta)
* **Dataset:** `bigquery-public-data.thelook_ecommerce`

## Desafios Resolvidos

### 1. Volume de Usuários e Idade Média por País
**Pergunta:** Quais os 5 principais mercados e qual o perfil de idade dos usuários?

```sql
-- Query para identificar os top 5 países por volume de clientes
SELECT 
    country, 
    COUNT(id) AS volume, 
    ROUND(AVG(age), 0) AS media_idade
FROM `bigquery-public-data.thelook_ecommerce.users`
GROUP BY 1
ORDER BY 2 DESC 
LIMIT 5;
```

Insight: A China lidera o mercado em volume de usuários, porém a faixa etária média (41 anos) é muito consistente entre todos os principais países, permitindo campanhas de marketing globais mais unificadas.


### 2. O Erro da Samantha (Limpeza de Dados)
**Pergunta:** Qual a diferença entre a média real de vendas e a média se os zeros fossem removidos por erro de digitação?

```sql
SELECT 
    ROUND(
        ABS(
            AVG(sale_price) - 
            AVG(CAST(REPLACE(CAST(sale_price AS STRING), '0', '') AS FLOAT64))
        ), 2) AS diferenca_erro
FROM `bigquery-public-data.thelook_ecommerce.order_items`;

Insight: O erro de digitação dos zeros impactaria o faturamento médio em 7.71 unidades monetárias, demonstrando a importância da integridade dos dados para relatórios financeiros.
```
