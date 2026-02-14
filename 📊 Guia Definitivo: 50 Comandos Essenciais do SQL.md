# 📊 Guia Definitivo: 50 Comandos Essenciais do SQL

Este guia utiliza o padrão SQL universal para manipulação de bancos de dados relacionais.

---

## 🏗️ 1. Definição de Dados (DDL - Estrutura)
| Comando | Descrição |
| :--- | :--- |
| `CREATE DATABASE db_nome;` | Cria um novo banco de dados |
| `CREATE TABLE tb (id INT, nome TEXT);` | Cria uma nova tabela com colunas e tipos |
| `ALTER TABLE tb ADD col tipo;` | Adiciona uma nova coluna a uma tabela existente |
| `ALTER TABLE tb DROP COLUMN col;` | Remove uma coluna de uma tabela |
| `ALTER TABLE tb RENAME TO novo_nome;` | Renomeia uma tabela |
| `DROP TABLE tb;` | Exclui permanentemente uma tabela e seus dados |
| `DROP DATABASE db_nome;` | Exclui permanentemente um banco de dados |
| `TRUNCATE TABLE tb;` | Remove todos os dados de uma tabela, mantendo a estrutura |
| `CREATE INDEX idx_nome ON tb(col);` | Cria um índice para acelerar buscas em colunas específicas |

---

## ✍️ 2. Manipulação de Dados (DML)
| Comando | Descrição |
| :--- | :--- |
| `INSERT INTO tb (c1, c2) VALUES (v1, v2);` | Insere um novo registro na tabela |
| `UPDATE tb SET c1 = v1 WHERE id = 1;` | Atualiza dados existentes (sempre use WHERE!) |
| `DELETE FROM tb WHERE id = 1;` | Remove registros específicos |
| `COPY tb FROM 'arquivo.csv';` | Importa dados de um arquivo externo (PostgreSQL) |

---

## 🔍 3. Consultas de Dados (DQL - O "Coração" do SQL)
| Comando | Descrição |
| :--- | :--- |
| `SELECT * FROM tb;` | Seleciona todas as colunas de uma tabela |
| `SELECT DISTINCT col FROM tb;` | Retorna apenas valores únicos (remove duplicados) |
| `SELECT col AS apelido FROM tb;` | Define um nome temporário (alias) para a coluna |
| `SELECT * FROM tb WHERE col = 'X';` | Filtra registros com base em uma condição |
| `SELECT * FROM tb WHERE col LIKE 'A%';` | Busca valores que começam com a letra "A" |
| `SELECT * FROM tb WHERE col IN (1, 2, 3);` | Filtra valores que estejam dentro de uma lista |
| `SELECT * FROM tb WHERE col BETWEEN 10 AND 20;` | Filtra valores dentro de um intervalo |
| `SELECT * FROM tb WHERE col IS NULL;` | Busca registros onde o valor é nulo |
| `SELECT * FROM tb ORDER BY col DESC;` | Ordena o resultado (ASC para crescente, DESC para decrescente) |
| `SELECT * FROM tb LIMIT 10;` | Limita a quantidade de linhas retornadas |

---

## 🔢 4. Funções de Agregação e Agrupamento
| Comando | Descrição |
| :--- | :--- |
| `SELECT COUNT(*) FROM tb;` | Conta o número total de registros |
| `SELECT SUM(col) FROM tb;` | Soma os valores de uma coluna numérica |
| `SELECT AVG(col) FROM tb;` | Calcula a média dos valores |
| `SELECT MIN(col) / MAX(col) FROM tb;` | Retorna o menor ou maior valor da coluna |
| `SELECT col, COUNT(*) FROM tb GROUP BY col;` | Agrupa registros e aplica uma função de contagem |
| `SELECT col FROM tb GROUP BY col HAVING COUNT(*) > 1;` | Filtra grupos (o WHERE dos agrupamentos) |

---

## 🔗 5. Joins (Relacionamentos)
| Comando | Descrição |
| :--- | :--- |
| `INNER JOIN tb2 ON tb1.id = tb2.fk;` | Retorna apenas registros que possuem correspondência em ambas |
| `LEFT JOIN tb2 ON tb1.id = tb2.fk;` | Retorna tudo da esquerda + o que houver correspondência na direita |
| `RIGHT JOIN tb2 ON tb1.id = tb2.fk;` | Retorna tudo da direita + o que houver correspondência na esquerda |
| `FULL OUTER JOIN tb2 ON tb1.id = tb2.fk;` | Retorna todos os registros de ambas, mesmo sem par |
| `CROSS JOIN tb2;` | Cria um produto cartesiano (combina todas as linhas com todas) |



[Image of SQL Joins Venn Diagram]


---

## 🔐 6. Controle e Transações (DCL / TCL)
| Comando | Descrição |
| :--- | :--- |
| `BEGIN TRANSACTION;` | Inicia uma transação (protege contra erros) |
| `COMMIT;` | Salva permanentemente as alterações no banco |
| `ROLLBACK;` | Descarta as alterações feitas desde o último BEGIN |
| `GRANT SELECT ON tb TO usuario;` | Dá permissão de leitura para um usuário |
| `REVOKE UPDATE ON tb FROM usuario;` | Remove a permissão de atualização |

---

## 🛠️ 7. Operadores e Condicionais
| Comando | Descrição |
| :--- | :--- |
| `AND / OR / NOT` | Operadores lógicos para combinar filtros |
| `UNION` | Une o resultado de dois SELECTs (remove duplicados) |
| `UNION ALL` | Une o resultado de dois SELECTs (mantém duplicados) |
| `EXISTS (SELECT...)` | Verifica se uma subquery retorna algum dado |
| `CASE WHEN cond THEN v1 ELSE v2 END;` | Lógica de "IF/ELSE" dentro do SELECT |
| `CAST(col AS tipo)` | Converte um valor de um tipo para outro |
| `COALESCE(col, 0)` | Retorna o primeiro valor não nulo da lista |