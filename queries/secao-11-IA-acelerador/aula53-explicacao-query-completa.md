# 📋 Explicação Técnica — Query de Auditoria Northwind

## Visão Geral
Esta query monta um painel completo de análise de produtos. Para cada produto ela retorna dados cadastrais, margem de lucro, preço com desconto, informações de categoria e fornecedor, métricas de avaliação, presença no carrinho, volume de pedidos e receita gerada. Ao final classifica o status de estoque e o status geral de disponibilidade, ordenando os resultados do produto que mais faturou para o que menos faturou.

## SELECT — Colunas e Cálculos

| Coluna / Expressão | O que retorna | Observação |
|--------------------|---------------|------------|
| `p.id AS produto_id` | Identificador único do produto | Chave primária |
| `p.name AS produto` | Nome do produto | — |
| `p.sku` | Código SKU | — |
| `p.price AS preco` | Preço de venda | — |
| `p.cost_price AS custo` | Preço de custo | — |
| `(p.price - p.cost_price)::numeric(10,2) AS margem` | Margem de lucro em reais | Preço menos custo, arredondado em 2 casas |
| `p.stock_quantity AS estoque` | Quantidade atual em estoque | — |
| `p.reorder_level AS nivel_minimo` | Nível mínimo de reposição | — |
| `p.is_active AS ativo` | Se o produto está ativo | Boolean |
| `p.is_featured AS destaque` | Se o produto está em destaque | Boolean |
| `p.discount_percentage AS desconto_pct` | Percentual de desconto | Pode ser NULL |
| `(p.price * (1 - COALESCE(p.discount_percentage,0) / 100))::numeric(10,2) AS preco_final` | Preço já com desconto aplicado | `COALESCE` transforma NULL em 0 para não quebrar o cálculo |
| `c.name AS categoria` | Nome da categoria | Vem do JOIN com categories |
| `c.is_active AS categoria_ativa` | Se a categoria está ativa | — |
| `s.company_name AS fornecedor` | Razão social do fornecedor | Vem do JOIN com suppliers |
| `s.email AS email_fornecedor` | E-mail do fornecedor | — |
| `s.city AS cidade_fornecedor` | Cidade do fornecedor | — |
| `s.is_active AS fornecedor_ativo` | Se o fornecedor está ativo | — |
| `COUNT(r.id) AS total_avaliacoes` | Quantidade de avaliações recebidas | — |
| `AVG(r.rating)::numeric(3,1) AS media_avaliacao` | Nota média das avaliações | Uma casa decimal |
| `COUNT(ci.id) AS vezes_no_carrinho` | Quantas vezes o produto entrou no carrinho | — |
| `COUNT(oi.id) AS total_pedidos` | Quantas vezes o produto foi vendido | Conta linhas de order_items |
| `SUM(oi.subtotal)::numeric(10,2) AS receita_total` | Soma de todo o faturamento gerado pelo produto | — |

## JOINs — Como as Tabelas se Conectam

```sql
FROM products p
INNER JOIN categories c  ON c.id  = p.category_id
INNER JOIN suppliers  s  ON s.id  = p.supplier_id
LEFT  JOIN reviews    r  ON r.product_id = p.id
LEFT  JOIN cart_items ci ON ci.product_id = p.id
LEFT  JOIN order_items oi ON oi.product_id = p.id
```

| JOIN | Tipo | Por que esse tipo? | O que acontece se trocar ou remover |
|------|------|--------------------|------------------------------------|
| `categories` | **INNER** | Todo produto deve ter uma categoria válida. Produto sem categoria não faz sentido no relatório. | Se virar LEFT: produtos órfãos de categoria aparecem. Se remover: colunas de categoria quebram a query. |
| `suppliers` | **INNER** | Todo produto deve ter um fornecedor válido. | Mesmo comportamento do JOIN de categorias. |
| `reviews` | **LEFT** | Um produto pode nunca ter recebido avaliação. Com LEFT ele continua aparecendo (com total_avaliacoes = 0). | Se virar INNER: produtos sem avaliação somem do relatório. |
| `cart_items` | **LEFT** | Um produto pode nunca ter entrado em nenhum carrinho. | Se virar INNER: produtos que nunca foram colocados no carrinho desaparecem. |
| `order_items` | **LEFT** | Um produto pode nunca ter sido vendido. | Se virar INNER: produtos sem vendas somem, distorcendo a análise de receita. |

**Regra prática:** use `INNER` quando a relação é obrigatória e `LEFT` quando a relação é opcional.

## GROUP BY e Agregações

```sql
GROUP BY
  p.id, p.name, p.sku, p.price, p.cost_price,
  p.stock_quantity, p.reorder_level, p.is_active,
  p.is_featured, p.discount_percentage,
  c.name, c.is_active,
  s.company_name, s.email, s.city, s.is_active
```

O `GROUP BY` é obrigatório porque a query usa funções de agregação (`COUNT`, `AVG`, `SUM`). Ele agrupa todas as linhas que pertencem ao **mesmo produto**, permitindo que as métricas sejam calculadas por produto e não de forma global.

Sem o `GROUP BY` o PostgreSQL retorna o erro:
> column "p.name" must appear in the GROUP BY clause or be used in an aggregate function

Todas as colunas que não estão dentro de uma função de agregação precisam aparecer no `GROUP BY`.

## CASE WHEN — Classificações de Status

### Status de Estoque
```sql
CASE
  WHEN p.stock_quantity = 0                    THEN '🔴 SEM ESTOQUE'
  WHEN p.stock_quantity <= p.reorder_level     THEN '🟡 ESTOQUE CRÍTICO'
  ELSE                                               '🟢 OK'
END AS status_estoque
```

| Condição | Resultado |
|----------|-----------|
| Estoque = 0 | 🔴 SEM ESTOQUE |
| Estoque ≤ nível mínimo de reposição | 🟡 ESTOQUE CRÍTICO |
| Qualquer outra situação | 🟢 OK |

### Status Geral
```sql
CASE
  WHEN p.is_active = false    THEN '❌ INATIVO'
  WHEN c.is_active = false    THEN '⚠️ CATEGORIA INATIVA'
  WHEN s.is_active = false    THEN '⚠️ FORNECEDOR INATIVO'
  ELSE                            '✅ DISPONÍVEL'
END AS status_geral
```

| Condição (ordem de prioridade) | Resultado |
|--------------------------------|-----------|
| Produto inativo | ❌ INATIVO |
| Categoria inativa | ⚠️ CATEGORIA INATIVA |
| Fornecedor inativo | ⚠️ FORNECEDOR INATIVO |
| Tudo ativo | ✅ DISPONÍVEL |

A ordem dos `WHEN` importa: o PostgreSQL para no primeiro verdadeiro. Por isso produto inativo tem prioridade sobre categoria ou fornecedor inativo.

## ORDER BY

```sql
ORDER BY receita_total DESC NULLS LAST;
```

- `DESC` → ordena do maior faturamento para o menor.
- `NULLS LAST` → produtos que nunca venderam (receita `NULL`) ficam no final da lista, e não no topo.

Sem o `ORDER BY` a ordem dos produtos fica indefinida (geralmente a ordem física da tabela).

## Como Usar Esta Query

Casos de uso práticos para QA:

1. **Auditoria de estoque** — filtrar ou ordenar por `status_estoque` para encontrar produtos sem estoque ou em nível crítico.
2. **Validação de cadastro** — verificar se existem produtos com `status_geral` diferente de `✅ DISPONÍVEL` (produto, categoria ou fornecedor inativo).
3. **Análise de performance** — usar `receita_total` e `total_pedidos` para identificar produtos que mais e menos vendem.
4. **Qualidade de avaliações** — cruzar `media_avaliacao` com `total_avaliacoes` para achar produtos com nota baixa ou poucos feedbacks.
5. **Teste de integridade** — confirmar que produtos sem vendas, sem avaliações ou sem passagem pelo carrinho **ainda aparecem** no resultado (graças aos `LEFT JOIN`).
6. **Regressão de dados** — rodar periodicamente e comparar se a quantidade de produtos em cada status de estoque/geral mudou de forma inesperada.
