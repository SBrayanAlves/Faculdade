---
tags:
  - bd
  - resumo
  - bimestre-1
bimestre: 1
fonte: claude
atualizado: 2026-11-08
---
# 🗄️ BD Avançado · 1º Bimestre
← [[BD - Painel]] · Próximo: [[BD - 2º Bimestre]]

> [!info] Como usar
> Conteúdo da **N1**. Vale revisar mesmo depois da prova, porque a **P2 e a Substitutiva cobrem TODA a matéria**. Cada seção termina com um **📄 Fonte**; seções com **➕** são complemento (fora dos slides). No fim, **simulado de 10 questões**.

## 🗺️ Mapa do bimestre
| Tema | Arquivo |
|---|---|
| Revisão de BD e Desafio BD | *sem slides na pasta* |
| Views e Triggers (+ Rules do PostgreSQL) | [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula03.pdf\|Aula 03]] |
| Stored Procedures e Functions | [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula04.pdf\|Aula 04]] |
| Funções embutidas, Sequences e BLOB | [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula05.pdf\|Aula 05]] |

**Avaliação:** N1 = P1 · N2 = **P2 + trabalho** (seminário, no máximo 30%) · MF = (N1 + N2) ÷ 2 ≥ 6 · **máximo de 10 faltas** · a Substitutiva troca **uma** nota (vale a maior).
📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Plano de Ensino.pdf#page=3|Plano de Ensino · p. 3]]

---

## 1. Views (visões)
> Uma **tabela derivada** de outras tabelas (ou de outras views). Funciona como uma **consulta armazenada** que, depois de criada, é **usada como tabela**. **Não fica necessariamente armazenada** no banco.

**Por quê?** **Segurança** (mostrar só o que o usuário pode ver) e **simplificar** consultas complexas pra quem usa.

```sql
-- Criar (exemplo do slide: funcionários do projeto "Produto X")
CREATE VIEW empregados_produtoX (nome, endereco) AS
SELECT f.pnome, f.endereco
FROM funcionario AS f
JOIN trabalha_em AS t ON f.cpf = t.fcpf
JOIN projeto AS p ON t.projnumero = p.projnumero
WHERE p.projnome = 'Produto X';

SELECT * FROM empregados_produtoX;                 -- usar como tabela
CREATE OR REPLACE VIEW empregados_produtoX ...;    -- alterar (redefine a consulta)
DROP VIEW empregados_produtoX;                     -- remover
```
- O comando de criar view **é o mesmo** em MySQL, PostgreSQL, SQL Server e Oracle

> [!example] No mundo real
> Dar a um analista acesso a uma view **sem CPF e salário**, em vez da tabela inteira, é o **princípio do menor privilégio** aplicado direto no banco.

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula03.pdf#page=2|Aula 03 · p. 2-16]]

---

## 2. Triggers (gatilhos)
> Código que o banco **executa sozinho** quando acontece um evento numa tabela.

| Parte | Opções |
|---|---|
| **Quando** | `BEFORE` (antes) · `AFTER` (depois) · `INSTEAD OF` (em vez de) |
| **Evento** | `INSERT` · `UPDATE` · `DELETE` |
| **Usos** | **Validações**, restrições de acesso, **auditoria/log**, consistência de dados |

Criar um trigger envolve **2 coisas**: o **comando que dispara** e a **ação** que ele executa. Assim, esses controles **saem da aplicação e vão pro banco**.

```sql
-- Sintaxe genérica do slide
CREATE TRIGGER <nome> ON <tabela> {FOR | AFTER | BEFORE | INSTEAD OF} {INSERT | UPDATE | DELETE} AS <comandos>;
ALTER TRIGGER <nome> ON <tabela> RENAME TO <novo_nome>;
DROP TRIGGER <nome> ON <tabela> [CASCADE | RESTRICT];
ALTER TABLE funcionario DISABLE TRIGGER tg_funcionario_insert;   -- desabilitar
ALTER TABLE funcionario ENABLE TRIGGER tg_funcionario_insert;    -- habilitar
```

**Limitações (segundo os slides):** não se cria trigger **em view**; dentro do trigger **não** se usa `CREATE/ALTER/DROP` (tabela, banco, índice, procedure...) nem `GRANT/REVOKE`; e **não dá pra ter dois triggers pro mesmo evento e momento** na mesma tabela (ex: dois `AFTER INSERT`).
> *Na prática alguns SGBDs flexibilizam isso (o PostgreSQL aceita `INSTEAD OF` em views, por exemplo). **Na prova, responda como o slide.***

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula03.pdf#page=17|Aula 03 · p. 17-31]]

### Triggers no PostgreSQL
- O trigger é **sempre associado a uma função** (o corpo fica na função, não no `CREATE TRIGGER`)
- Duas variáveis especiais: **`NEW`** (a linha **nova**) e **`OLD`** (a linha **antiga**)

| Evento | `NEW` | `OLD` |
|---|:-:|:-:|
| INSERT | ✅ | ❌ |
| UPDATE | ✅ | ✅ |
| DELETE | ❌ | ✅ |

```sql
CREATE TRIGGER tg_funcionario_insert
AFTER INSERT ON funcionario
FOR EACH ROW                                  -- uma vez por linha (ou FOR EACH STATEMENT)
EXECUTE PROCEDURE insert_funcionario();

SELECT * FROM INFORMATION_SCHEMA.TRIGGERS;    -- listar os triggers do banco
```

> [!example]- ➕ Exemplo completo: a função + o trigger (log de inserção)
> ```sql
> CREATE OR REPLACE FUNCTION insert_funcionario() RETURNS trigger AS $$
> BEGIN
>   INSERT INTO funcionario_log (cpf, comando, usuario, data)
>   VALUES (NEW.cpf, 'INSERT', current_user, now());
>   RETURN NEW;            -- obrigatório devolver a linha (em BEFORE, é a que será gravada)
> END;
> $$ LANGUAGE plpgsql;
> ```
> **Validação com BEFORE:** `IF NEW.salario < 0 THEN RAISE EXCEPTION 'Salário negativo'; END IF;` → o INSERT é **cancelado**.

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula03.pdf#page=33|Aula 03 · p. 33-39]]

### Rules (só PostgreSQL)
Parecidas com triggers, mas podem **substituir** (`DO INSTEAD`) ou **anular** (`DO INSTEAD NOTHING`) um comando **de forma transparente e sem abortar a transação**. São aplicadas **antes** do comando executar.
```sql
CREATE RULE rl_customers_delete AS ON DELETE TO customers
DO INSERT INTO customer_log (name, acao, usuario) VALUES (old.name, 'DELETE', CURRENT_USER);

DROP RULE rl_customers_delete ON customers CASCADE;
SELECT * FROM pg_rules WHERE tablename = 'customers';
```
**MySQL e SQL Server** também têm triggers, com o corpo escrito direto na declaração (sintaxes nas p. 45-56).

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula03.pdf#page=40|Aula 03 · p. 40-44]] · [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula03.pdf#page=45|MySQL e SQL Server · p. 45-56]]

---

## 3. Stored Procedures (procedimentos armazenados)
> Um **bloco de SQL guardado no banco**, com nome e parâmetros, pra **tarefas repetitivas**. Devolve um status de sucesso ou falha.

**Por que usar:** reduz **tráfego de rede**, melhora **desempenho** (o plano de execução não é recalculado a cada chamada, como acontece numa consulta solta), permite **tarefas agendadas** e **diminui riscos** (o usuário chama a procedure em vez de mexer nas tabelas).

```sql
CREATE PROCEDURE MaiorPedidoAno (@ano integer) AS
SELECT NomeFornecedor, NomePeca, Qtdade
FROM Fornecedor f, Peca p, Pedido pe
WHERE f.codigo = pe.codigo_fornecedor AND p.codigo = pe.codigo_peca
  AND Qtdade = (SELECT MAX(Qtdade) FROM Pedido WHERE ano = @ano);

EXEC MaiorPedidoAno 2003;          -- executar
ALTER PROCEDURE ...;               -- alterar: reescreve o comando INTEIRO
DROP PROCEDURE MaiorPedidoAno;     -- excluir
```
Parâmetro de **saída**: `@dataPedido datetime OUTPUT`.

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula04.pdf#page=3|Aula 04 · p. 3-13]]

### Por SGBD
| SGBD | Como é |
|---|---|
| **PostgreSQL** | **Procedures são tratadas como FUNCTIONS.** 3 formas: em **SQL**, em linguagem procedural (**PL/pgSQL**) ou em linguagens externas |
| **MySQL** | `CREATE PROCEDURE` (chamada com **`CALL`**, devolve valores só por parâmetros **`OUT`**; parâmetros `IN`, `OUT`, `INOUT`) e `CREATE FUNCTION` (devolve um valor e é usada **dentro de instruções**). Usa `DELIMITER //` |
| **SQL Server** | Linguagem **T-SQL**, parâmetros com `@` |

```sql
-- PostgreSQL: função em SQL ($1 e $2 = 1º e 2º parâmetros)
CREATE FUNCTION soma(INTEGER, INTEGER) RETURNS INTEGER AS
'SELECT $1 + $2;' LANGUAGE SQL;
SELECT soma(4, 6);    -- 10

-- PostgreSQL: função em PL/pgSQL
CREATE FUNCTION func_escopo() RETURNS integer AS $$
DECLARE quantidade integer := 30;
BEGIN
  RAISE NOTICE 'Aqui a quantidade é %', quantidade;
  quantidade := 50;
  RETURN quantidade;
END; $$ LANGUAGE plpgsql;
```
| | Procedure | Function |
|---|---|---|
| Retorno | Por parâmetros `OUT` | **Retorna um valor** (`RETURNS`) |
| Como chama | `CALL` / `EXEC` | **Dentro de um SELECT** ou expressão |

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula04.pdf#page=15|PostgreSQL · p. 15-20]] · [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula04.pdf#page=21|MySQL · p. 21-28]] · [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula04.pdf#page=29|SQL Server · p. 29-32]]

---

## 4. Funções embutidas
| Tipo | Principais |
|---|---|
| **Matemáticas** | `abs`, `ceil` (arredonda pra cima), `floor` (pra baixo), `round(x, casas)`, `mod`, `pow`, `sqrt`, `random` |
| **Data e hora** | `current_date`, `current_time`, `current_timestamp`, `now()`, `age(d1, d2)` (intervalo entre datas) |
| **Texto** | `length`, `lower`, `upper`, `initcap` (1ª letra maiúscula), `position(b in s)`, `lpad`/`rpad`, `translate` |
| **Sessão** | `current_database()`, `current_user`, `version()`, `current_setting('datestyle')` |

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula05.pdf#page=3|Aula 05 · p. 3-12]]

---

## 5. Sequences
> Estrutura que **devolve um valor diferente a cada acesso**. Usada pra gerar IDs.

```sql
CREATE SEQUENCE minha_seq INCREMENT BY 2 MINVALUE 100 MAXVALUE 110 CYCLE;
-- START WITH (padrão 1) · INCREMENT BY (padrão 1) · CYCLE = volta ao início ao chegar no máximo
ALTER SEQUENCE minha_seq INCREMENT BY 1;    -- altera tudo, menos o valor inicial
DROP SEQUENCE minha_seq;
```
| Comando (PostgreSQL) | Faz |
|---|---|
| `SELECT nextval('minha_seq');` | **Próximo** valor (o primeiro `nextval` **inicializa** a sequence) |
| `SELECT currval('minha_seq');` | Valor **atual** |
| `SELECT setval('minha_seq', 50);` | **Muda** o valor atual |
| Coluna **`SERIAL`** | **Equivale a criar uma sequence** |

- No INSERT: `INSERT INTO aluno VALUES (nextval('minha_seq'), 'Adriana', 'F');`
- ⚠️ **`NEXTVAL` nunca é desfeito**, nem com rollback (pra não travar transações concorrentes). Uma transação abortada **deixa buracos** na numeração. Isso é normal.
- **MySQL** não tem sequence: usa **`AUTO_INCREMENT`** · **SQL Server**: `SELECT NEXT VALUE FOR minha_seq;`

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula05.pdf#page=13|Aula 05 · p. 13-28]]

---

## 6. BLOB (*Binary Large Object*)
Guardar **dados binários** no banco: imagem, som, vídeo, executáveis.
- No PostgreSQL o tipo é **`bytea`**, ou o mecanismo de *large objects*
- ⚠️ Dificulta migrar pra outro SGBD e pode deixar o acesso mais lento. **Alternativa comum:** guardar no banco **só o caminho do arquivo**

```sql
-- PostgreSQL (large objects): a tabela guarda só o OID; os dados ficam em pg_largeobject
CREATE TABLE imagem (nome_imagem varchar(20), oid_imagem oid);
INSERT INTO imagem VALUES ('lago', lo_import('C:/lago.jpg'));              -- importar
SELECT lo_export(oid_imagem, 'C:/lago_export.jpg') FROM imagem WHERE nome_imagem = 'lago';  -- exportar
SELECT lo_unlink(oid_imagem) FROM imagem WHERE nome_imagem = 'lago';      -- apagar o objeto
DELETE FROM imagem WHERE nome_imagem = 'lago';                             -- depois, a linha
```
> [!warning] Pegadinha
> **Só o `DELETE` não apaga o large object** de `pg_largeobject`. É preciso `lo_unlink` antes.

**MySQL:** coluna `BLOB` + `LOAD_FILE('/arquivo')` e `SELECT ... INTO OUTFILE` · **SQL Server:** `varbinary(MAX)` + `OPENROWSET(BULK ..., SINGLE_BLOB)`

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula05.pdf#page=29|Aula 05 · p. 29-43]]

---

## 📝 Simulado: 1º Bimestre

### Questão 1
Sobre **views**, assinale a correta:
- a) Uma view sempre duplica fisicamente os dados das tabelas base
- b) É uma tabela derivada (uma consulta armazenada), usada como tabela, que não fica necessariamente armazenada no banco
- c) Só pode ser criada no PostgreSQL
- d) Uma view não pode usar JOIN
- e) Depois de criada, uma view não pode ser alterada

> [!success]- Resposta: **B**
> View = consulta armazenada tratada como tabela. Pode ter JOIN, pode ser alterada com `CREATE OR REPLACE` e a sintaxe é a mesma nos principais SGBDs. → Seção 1

### Questão 2
O estagiário precisa consultar **nome e departamento** dos funcionários, mas **não pode ver o salário**. A melhor solução no banco é:
- a) Criar um trigger que apaga o salário
- b) Criar uma view só com as colunas permitidas e dar acesso apenas a ela
- c) Criar uma sequence para o estagiário
- d) Copiar a tabela inteira para outro banco
- e) Usar um BLOB

> [!success]- Resposta: **B**
> É o uso de view por **segurança**: o usuário enxerga só o que pode. → Seção 1

### Questão 3
Você quer **impedir** que um funcionário seja inserido com salário negativo, **antes** de a linha ser gravada. O trigger adequado é:
- a) AFTER DELETE
- b) AFTER INSERT
- c) BEFORE INSERT
- d) BEFORE DELETE
- e) Uma view

> [!success]- Resposta: **C**
> Pra **validar e barrar**, o trigger precisa rodar **antes** da gravação: **BEFORE INSERT** (com `RAISE EXCEPTION` se for inválido). → Seção 2

### Questão 4
No **PostgreSQL**, onde fica o código executado por um trigger?
- a) Direto no comando CREATE TRIGGER, como no MySQL
- b) Numa função associada ao trigger, chamada pelo EXECUTE PROCEDURE
- c) Numa view
- d) Numa sequence
- e) No arquivo de configuração do servidor

> [!success]- Resposta: **B**
> No PostgreSQL, o trigger é **sempre associado a uma função**. Nos outros SGBDs, o corpo vai na própria declaração. → Seção 2

### Questão 5
Num trigger de **DELETE** no PostgreSQL, qual variável guarda os dados da **linha que está sendo removida**?
- a) NEW
- b) OLD
- c) TG_TABLE
- d) CURRENT_ROW
- e) DELETED_ROW

> [!success]- Resposta: **B**
> **OLD** = linha antiga (DELETE e UPDATE). **NEW** = linha nova (INSERT e UPDATE). → Seção 2

### Questão 6
Qual recurso **exclusivo do PostgreSQL** permite **substituir** (`INSTEAD`) ou **anular** (`NOTHING`) um comando de forma transparente, **sem abortar a transação**?
- a) Sequence
- b) View
- c) Rule
- d) BLOB
- e) Stored procedure

> [!success]- Resposta: **C**
> **Rules** (`CREATE RULE ... DO [INSTEAD] ação`), aplicadas **antes** do comando executar. → Seção 2

### Questão 7
No **MySQL**, como se executa uma **stored procedure** e como ela devolve valores?
- a) Com SELECT, devolvendo com RETURN
- b) Com CALL, devolvendo valores por parâmetros OUT
- c) Com EXEC, devolvendo um ResultSet obrigatório
- d) Automaticamente a cada INSERT
- e) Com NEXTVAL

> [!success]- Resposta: **B**
> **Procedure:** `CALL` + parâmetros `OUT`. A **function** é que devolve um valor (`RETURNS`) e pode ser usada dentro de um SELECT. → Seção 3

### Questão 8
Dada a função abaixo, qual o resultado de `SELECT soma(4, 6);` e o que significa `$1`?
```sql
CREATE FUNCTION soma(INTEGER, INTEGER) RETURNS INTEGER AS
'SELECT $1 + $2;' LANGUAGE SQL;
```
- a) 46; `$1` é uma variável global
- b) 10; `$1` é o primeiro parâmetro recebido
- c) Erro; funções SQL não aceitam parâmetros
- d) 10; `$1` é o primeiro registro da tabela
- e) 4; `$1` é o valor de retorno

> [!success]- Resposta: **B**
> `$1` e `$2` são os parâmetros posicionais: 4 + 6 = **10**. → Seção 3

### Questão 9
Uma sequence começa em 1 com incremento 1. Uma transação executa `nextval` (recebe **5**) e depois faz **ROLLBACK**. O próximo `nextval` retorna:
- a) 5
- b) 6
- c) 1
- d) 4
- e) Erro, pois a sequence foi desfeita

> [!success]- Resposta: **B**
> O **`NEXTVAL` nunca é desfeito**: o 5 é considerado usado, e a numeração fica com um **buraco**. Isso evita que transações concorrentes se bloqueiem. → Seção 5

### Questão 10
No PostgreSQL, uma imagem foi gravada com `lo_import`. Depois, alguém executou apenas `DELETE FROM imagem WHERE nome_imagem = 'lago';`. O que acontece com o arquivo binário?
- a) É apagado junto com a linha
- b) Continua armazenado em `pg_largeobject`, pois era preciso usar `lo_unlink` antes
- c) Vai para a lixeira do sistema operacional
- d) É exportado automaticamente para o disco
- e) Vira uma sequence

> [!success]- Resposta: **B**
> O DELETE apaga só a linha com o OID. O objeto continua em `pg_largeobject` até um **`lo_unlink`**. → Seção 6
