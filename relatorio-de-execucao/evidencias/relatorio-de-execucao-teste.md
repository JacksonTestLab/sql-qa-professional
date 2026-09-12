# 🧪 Relatório de Execução de Testes — QA

![QA](https://img.shields.io/badge/QA-Test%20Execution-blue)
![Status](https://img.shields.io/badge/Status-4%20PASS%20%7C%204%20FAIL-red)
![Aprovação](https://img.shields.io/badge/Aprovação-50%25-orange)

> **Execução realizada em:** 12/09/2026 às 10:08  
> **Total de cenários:** 8  
> **Resultado:** ⚠️ Execução com falhas

---

## 📋 1. Visão geral

Este documento apresenta o resultado da execução das regras de validação aplicadas aos dados da base de testes.

O objetivo da execução é verificar se os registros atendem aos critérios definidos para **preço, categoria, SKU, fornecedor e demais regras de consistência**.

### Resultado da execução

| Métrica | Resultado |
|---|---:|
| Cenários executados | **8** |
| ✅ Aprovados | **4** |
| ❌ Reprovados | **4** |
| ⚠️ Warnings | **0** |
| **Taxa de aprovação** | **50,0%** |

---

## 🖼️ 2. Evidência da execução

O resultado visual da execução está apresentado abaixo.

![Relatório visual da execução](./conversao-de-html-via-IA.png)

> **Evidência:** `conversao-de-html-via-IA.png`

---

## 🧩 3. Matriz de resultados

| ID | Regra de validação | Status | Evidência |
|---|---|:---:|---|
| `regra_preco_custo_valida` | Produtos ativos não devem possuir preço de custo superior ao preço de venda | ✅ PASS | Nenhuma inconsistência encontrada |
| `regra_existem_inativos` | Verificar a existência de produtos inativos | ❌ FAIL | Foram encontrados 50 produtos e a regra esperava a existência de inativos |
| `regra_produto_categoria_valida` | Validar se os produtos possuem categoria válida | ✅ PASS | Nenhum produto sem categoria válida |
| `regra_preco_positivo` | Produtos ativos devem possuir preço positivo | ✅ PASS | Nenhum produto ativo com preço menor ou igual a zero |
| `regra_sku_unico` | Validar unicidade dos SKUs | ✅ PASS | Nenhum SKU duplicado encontrado |
| `regra_categoria_informatica_ativa` | Validar existência de categoria `informatica` ativa | ❌ FAIL | Nenhuma categoria `informatica` ativa encontrada |
| `regra_nome_categoria_obrigatorio` | Validar preenchimento do nome das categorias | ❌ FAIL | 6 categorias com nome vazio |
| `regra_produto_tem_supplier` | Validar associação de fornecedor aos produtos | ❌ FAIL | 8 produtos sem `supplier_id` |

---

## 📊 4. Indicadores

### Taxa de aprovação

**50,0%**

```text
Aprovados       ████████████████████  4 / 8
Reprovados      ████████████████████  4 / 8
Warnings                             0 / 8
```

### Distribuição

- 🟢 **PASS:** 4 testes — 50,0%
- 🔴 **FAIL:** 4 testes — 50,0%
- 🟡 **WARNING:** 0 testes — 0,0%

---

## ❌ 5. Detalhamento das falhas

### `regra_existem_inativos`

**Status:** ❌ FAIL

**Descrição:** Verificar a existência de produtos inativos.

**Resultado observado:** Foram encontrados **50 produtos**, porém a regra esperava a existência de produtos inativos.

---

### `regra_categoria_informatica_ativa`

**Status:** ❌ FAIL

**Descrição:** Validar a existência de uma categoria `informatica` ativa.

**Resultado observado:** Nenhuma categoria `informatica` ativa foi encontrada.

---

### `regra_nome_categoria_obrigatorio`

**Status:** ❌ FAIL

**Descrição:** Validar se as categorias possuem nome preenchido.

**Resultado observado:** Foram identificadas **6 categorias** com nome vazio:

```text
10, 11, 12, 13, 14, 15
```

---

### `regra_produto_tem_supplier`

**Status:** ❌ FAIL

**Descrição:** Validar se todos os produtos possuem fornecedor.

**Resultado observado:** Foram identificados **8 produtos sem `supplier_id`**:

```text
22, 23, 16, 17, 18, 19, 20, 21
```

---

## ✅ 6. Cenários aprovados

Os seguintes cenários foram executados sem inconsistências:

- `regra_preco_custo_valida`
- `regra_produto_categoria_valida`
- `regra_preco_positivo`
- `regra_sku_unico`

---

## 🔍 7. Análise de qualidade

A execução apresentou **50% de aprovação**, portanto o conjunto de dados avaliado **não atende integralmente aos critérios estabelecidos pelas regras de validação**.

As principais inconsistências encontradas estão relacionadas a:

| Categoria | Problema identificado |
|---|---|
| Produtos | Ausência de produtos inativos conforme esperado pela regra |
| Categorias | Ausência de categoria `informatica` ativa |
| Categorias | Registros com nome vazio |
| Fornecedores | Produtos sem `supplier_id` |

---

## 🚦 8. Critério de aceite

### Resultado: ❌ NÃO APROVADO

Considerando que foram identificadas **4 falhas em 8 cenários executados**, a execução não deve ser considerada totalmente aprovada.

**Taxa de aprovação atual:** `50,0%`

As inconsistências identificadas devem ser analisadas e corrigidas antes de uma nova execução de validação.

---

## 📝 9. Próximos passos recomendados

1. 🔎 Investigar a causa das 4 regras reprovadas.
2. 🛠️ Corrigir os dados inconsistentes identificados.
3. 🔄 Executar novamente as regras de validação.
4. 📊 Comparar o novo resultado com esta execução.
5. ✅ Considerar a execução aprovada somente após o atendimento dos critérios definidos.

---

## 📁 10. Evidências do projeto

Estrutura recomendada para publicação no GitHub:

```text
sql-qa-professional/
│
├── relatorio-execucao-testes-qa.md
├── conversao-de-html-via-IA.png
│
└── ...
```

A imagem é referenciada diretamente pelo Markdown:

```markdown
![Relatório visual da execução](./conversao-de-html-via-IA.png)
```

---

## 🏁 Conclusão

> **Execução concluída com 8 cenários avaliados, sendo 4 aprovados e 4 reprovados, resultando em uma taxa de aprovação de 50,0%.**

O relatório fornece a visão consolidada da execução, as evidências das inconsistências encontradas e os principais pontos que devem ser tratados antes de uma nova rodada de testes.

---

<div align="center">

**🧪 QA Test Execution Report**

*Validação de qualidade e consistência de dados*

**12/09/2026**

</div>
