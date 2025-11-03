# 🏦 Eschema de Banco de Dados PostgreSQL: Gerenciamento de Entidades

Este repositório contém o script SQL para a criação do esquema de um banco de dados PostgreSQL. O modelo é projetado para gerenciar diversas entidades relacionadas, como empresas, funcionários, dados bancários, transações, membros associados e auditorias.

## ✨ Destaques do Esquema

O esquema foi cuidadosamente estruturado com as seguintes tabelas e seus relacionamentos:

* **Entidades Principais:** `empresa`, `funcionario`, `membro_associado`, `auditor`, `jogador`.
* **Dados Estruturais:** `endereco`.
* **Dados Financeiros/Transacionais:** `banco`, `mensalidade`, `transacao`, `auditoria`.
* **Documentação:** `documento`.

## 🛠️ Como Utilizar

Este script é pronto para ser executado em qualquer ambiente PostgreSQL.

### 1. Requisitos

* Um servidor **PostgreSQL** ativo (versão 9.5 ou superior é recomendada).
* Acesso a uma ferramenta de execução de scripts SQL (como **pgAdmin**, **DBeaver** ou o cliente de linha de comando `psql`).

### 2. Execução

1.  Copie o conteúdo do arquivo `schema_creation.sql` (ou o nome que você usou).
2.  Conecte-se ao seu banco de dados PostgreSQL.
3.  Execute o script SQL.

O script realiza as seguintes ações:
* Cria o *schema* chamado **`mydb`** (se não existir).
* Define `mydb` como o caminho de busca (`SET search_path TO mydb;`).
* Cria todas as tabelas com suas colunas, tipos de dados e restrições (`NOT NULL`, `UNIQUE`).
* Estabelece as **chaves primárias (`SERIAL PRIMARY KEY`)** e as **chaves estrangeiras (`FOREIGN KEY`)** para manter a integridade referencial, utilizando a política `ON DELETE NO ACTION` e `ON UPDATE NO ACTION`.
* Cria **índices** para otimizar as consultas em colunas de chaves estrangeiras.

## 🔗 Estrutura do Relacionamento de Entidades

O modelo segue um design robusto para interligar os dados:

* **1:N (Um para Muitos):** Uma `empresa` tem muitos `funcionarios` e `membro_associado`.
* **N:N (Muitos para Muitos):** A relação entre `auditor` e `transacao` é registrada na tabela `auditoria`.
* **Chaves de Autoincremento:** Todas as tabelas principais usam `SERIAL` para chaves primárias auto-geradas.

---
**Adaptado de MySQL Workbench - Data: 03 de Novembro de 2025**
