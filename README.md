# 🗄️ SQL & Banco de Dados para QA

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
![DBeaver](https://img.shields.io/badge/DBeaver-372923?style=for-the-badge&logo=dbeaver&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Azure DevOps](https://img.shields.io/badge/Azure%20DevOps-0078D7?style=for-the-badge&logo=azuredevops&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

> Projeto desenvolvido durante o curso SQL & Banco de Dados para QA — onde aprendi a validar dados direto na fonte,
> investigar bugs que a tela esconde e documentar evidências no padrão de mercado.

---

## ✅ O que foi feito

- ✅ Suite de testes SQL com PASSED/FAILED automatizado
- 🔍 Investigações de bugs direto no banco (API vs Banco)
- 🐛 Bug reports documentados no padrão de mercado
- 📊 Relatórios HTML publicados via GitHub Pages

---

## 📊 Relatórios ao Vivo

> 🔗 [Relatório de Categorias](https://[usuario].github.io/qa-sql-northwind/evidencias/relatorio-categorias.html)
> 🔗 [Relatório de Fornecedores](https://[usuario].github.io/qa-sql-northwind/evidencias/relatorio-fornecedores.html)

---

## 🧪 Suite de Testes

| Teste | Status |
|---|---|
| regra_preco_custo_valida | ✅ PASSED |
| regra_existem_inativos | ✅ PASSED |
| regra_produto_categoria_valida | ✅ PASSED |
| regra_preco_positivo | ✅ PASSED |
| regra_sku_unico | ✅ PASSED |
| regra_categoria_informatica_ativa | ✅ PASSED |
| regra_nome_categoria_obrigatorio | ❌ FAILED |
| regra_produto_tem_supplier | ❌ FAILED |

---

## 🔍 Investigações Realizadas

| ID | Investigação | Status | Evidência |
|---|---|---|---|
| INV-001 | CSV dos testes em excel | 🔴 Bug confirmado | [ver](/prints-gerais/csv-dos-teste-em-excel.png) |
| INV-002 | Html via DBeaver | 🔴 Bug confirmado | [ver](/prints-gerais/exoport-html-via-dbeaver.png) |

---

## 📁 Estrutura do Projeto

```
sql-qa-professional/
├── docs/
│   ├── dicionario-produtos.md
│   ├── guia-dbeaver.md
├── prints-gerais/
│       ├── conversao-de-html-via-IA.png
│       ├── csv-dos-teste-em-excel.png
│       ├── exoport-html-via-dbeaver.png
│       └── export-html-via-dbeaver-sql01.png
├── queries/
│   ├── inserts-específicos/
│   ├── secao-07-sql-essencial/
│   ├── secao-08-dml/
│   ├── secao-09-qa-investigativo/
│   ├── secao-10-asserts/
│   ├── secao-11-IA-acelerador/
│   ├── .gitkeep
│   └── inserts.sql
├── relatorio-de-execucao/
│   └── relatorio-de-execucao-teste.md
├── utils/
│   └── dump-postgres-202606300850.sql
├── .gitignore
└── README.md
```


---

## 📚 Documentação Técnica
Consulte a pasta [`/docs`](docs/) para:
dicionário de dados, guia de referência do DBeaver
e documentação da suite de testes.

---

## 👤 Autor

**Jackson Mendes**
Formação: SQL & Banco de Dados para QA
Plataforma: [Gotas de Tecnologia](https://gotasdetecnologia.com.br)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/jackson-mendes-a4731a2b3)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/JacksonTestLab)