---
tags:
  - bd
  - resumo
  - bimestre-2
bimestre: 2
fonte: claude
atualizado: 2026-11-08
---
# 🗄️ BD Avançado · 2º Bimestre
← [[BD - Painel]] · Anterior: [[BD - 1º Bimestre]]

> [!info] Como usar
> Do 2º bimestre, só a aula de **Índices** tinha slides disponíveis. **Otimização, Transações e Administração** (seções com **➕**) foram escritas a partir do **plano de ensino** e da bibliografia (Elmasri & Navathe). No fim, **simulado de 10 questões**.

> [!warning] Datas: confirmar!
> O cronograma do plano de ensino está com **datas de 2024**. Pela ordem: Índices → Processamento e Otimização → **Transações** → Administração de BD → **Apresentação de Trabalho** → **Prova B2** → ARA.
> 📄 [[Faculdade/4_Semestre/Banco de Dados Avancado/Plano de Ensino.pdf#page=4|Plano de Ensino · p. 4]]

## 🗺️ Mapa do bimestre
| Tema | Arquivo |
|---|---|
| Indexação | [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula07.pdf\|Aula 07]] |
| ➕ Processamento e otimização de consultas | *sem slides* · conteúdo no plano |
| ➕ Transações, concorrência e recuperação | *sem slides* · conteúdo no plano |
| ➕ Administração de BD | *sem slides* |
| Trabalho (seminário): BDs distribuídos, NoSQL e outros | Plano de Ensino |

📄 **Conteúdo oficial:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Plano de Ensino.pdf#page=2|Plano de Ensino · p. 2]]

---

## 1. Índices ⭐
> Estrutura **auxiliar** pra encontrar registros mais rápido, como o **índice remissivo de um livro**. Objetivo: **reduzir o I/O de disco**.

- Os campos usados formam a **chave de pesquisa**. Uma tabela pode ter **vários** índices
- Valem a pena em tabelas grandes (**mais de ~1000 linhas**)
- ⚠️ **Índice demais é ruim:** todo `INSERT/UPDATE/DELETE` precisa atualizar os índices também

### Denso × Esparso
| | **Denso** | **Esparso** |
|---|---|---|
| Entradas | **Uma por registro** | **Uma por bloco** (aponta pro 1º registro do bloco) |
| Busca | Mais **rápida** | Um pouco mais lenta |
| Espaço e manutenção | Mais | **Menos** |

### Clustered × Nonclustered
| | **Clustered** (agrupado) | **Nonclustered** (secundário) |
|---|---|---|
| Ordem física | **Ordena os dados fisicamente** | Estrutura **separada**: não mexe na ordem da tabela |
| Quantos por tabela | **Só 1** (só existe uma ordem física) | **Vários** |
| Estrutura | **B-Tree** (páginas ligadas por lista duplamente encadeada) | Separada das páginas de dados |
| Criado automaticamente | Pela **PRIMARY KEY** | Pela restrição **UNIQUE** |
| Bom para | Muitos valores distintos, **faixas** (`BETWEEN`, `>`, `<`), acesso sequencial, **JOIN e GROUP BY** | Buscas pontuais |
| Custo | Overhead nas escritas (pode reordenar fisicamente) | Mais leve |

> *"PK cria clustered" é o comportamento do **SQL Server**, como no slide. O PostgreSQL cria um índice B-tree pra PK, mas não reordena a tabela fisicamente.*

### Por que índice é eficiente
- O índice ocupa **poucos blocos** comparado aos dados
- As chaves ficam **ordenadas** → dá pra usar **busca binária**
- Pode **caber na memória** (buffer): a busca nem vai ao disco

### O ganho em JOINs (exemplo do slide)
3 tabelas **sem índice**, 1.000 linhas cada, juntadas por igualdade → o banco testa **1.000 × 1.000 × 1.000 = 1 bilhão** de combinações. **Com índice**, pra cada linha de t1 ele vai **direto** na linha correspondente de t2 e t3.

### Onde criar (e onde não criar)
✅ Colunas em **`WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`** · **chaves estrangeiras** (muito usadas em JOIN) · colunas com **muitos valores distintos**
❌ Índice raramente usado (só atrasa as escritas) · índices em excesso
💡 **PKs pequenas** e **tipos de dados pequenos** (`SMALLINT` em vez de `BIGINT`, `VARCHAR(25)` em vez de `VARCHAR(100)`): comparações mais rápidas, menos disco, mais registros no cache

### SQL e tipos por SGBD
```sql
CREATE INDEX idx_agencia_nome ON agencia (nome_agencia);
DROP INDEX idx_agencia_nome;
```
| SGBD | Tipos |
|---|---|
| **PostgreSQL** | **B-tree** (**padrão**: `<, <=, =, >=, >, LIKE, BETWEEN`) · **Hash** (só `=`, não recomendado) · **R-tree** (dados geométricos) · **GiST** (extensível) |
| **MySQL** | Todos em árvore B; `LIKE` só aproveita o índice com prefixo fixo (`'algo%'`) |
| **SQL Server** | B-Tree |

> **Consulta lenta?** Quase sempre faltam índices ou eles foram mal criados. **Indexação é a primeira alternativa** de ganho de desempenho.

📄 **Fonte:** [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula07.pdf#page=3|Aula 07 · p. 3-11]] · [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula07.pdf#page=12|clustered · p. 12-21]] · [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula07.pdf#page=25|benefícios, custos e dicas · p. 25-36]] · [[Faculdade/4_Semestre/Banco de Dados Avancado/Aula07.pdf#page=38|SQL e SGBDs · p. 38-45]]

---

## 2. ➕ Processamento e otimização de consultas
**O caminho de uma consulta dentro do SGBD:**
```
SQL → 1. Análise (sintaxe e nomes) → 2. Tradução para álgebra relacional
    → 3. Otimização → 4. Plano de execução → 5. Execução → resultado
```
### Otimização algébrica
Reescreve a consulta numa forma **equivalente e mais barata**, usando **regras de equivalência**:
- **Aplicar as seleções (filtros do WHERE) o mais cedo possível**, antes das junções → menos linhas pra juntar
- **Aplicar as projeções cedo** (levar só as colunas necessárias)
- Escolher a **ordem das junções** começando pelas que geram **menos linhas**

> [!example] Exemplo
> "Clientes que alugaram carro em 2026": juntar `cliente × aluguel` inteiros e **depois** filtrar o ano é caro. **Filtrar `aluguel` por 2026 primeiro** e só depois juntar com `cliente` dá o mesmo resultado com muito menos trabalho.

### Plano de execução
O otimizador usa o **catálogo** (estatísticas: nº de linhas, valores distintos, índices existentes) pra **estimar o custo** de cada alternativa e escolher, por exemplo:
- **Varredura sequencial** (lê a tabela toda) × **busca por índice**
- Algoritmo de junção: **nested loop**, **hash join** ou **merge join**

No PostgreSQL dá pra ver o plano com **`EXPLAIN`** (estimativa) e **`EXPLAIN ANALYZE`** (executa e mede).

---

## 3. ➕ Transações ⭐
> **Transação** = sequência de operações tratada como **uma unidade só**: ou acontece **tudo**, ou **nada**.

```sql
BEGIN;
UPDATE conta SET saldo = saldo - 100 WHERE id = 1;
UPDATE conta SET saldo = saldo + 100 WHERE id = 2;
COMMIT;        -- confirma tudo  (ou ROLLBACK; para desfazer tudo)
```

### ACID
| | Propriedade | Significado | Exemplo (PIX) |
|:-:|---|---|---|
| **A** | **Atomicidade** | Tudo ou nada | Debitou de A e caiu antes de creditar em B → **o débito é desfeito** |
| **C** | **Consistência** | De um estado válido pra outro válido | A soma do dinheiro das contas não muda |
| **I** | **Isolamento** | Transações simultâneas não interferem | Dois PIX ao mesmo tempo na mesma conta não "somem" com saldo |
| **D** | **Durabilidade** | Depois do **COMMIT**, sobrevive a falhas | Faltou energia logo após confirmar → o PIX continua lá |

**Estados:** Ativa → Parcialmente confirmada → **Confirmada** (*committed*) · ou Ativa → Falha → **Abortada** (rollback)

### Problemas de concorrência
| Problema | O que acontece |
|---|---|
| **Atualização perdida** (*lost update*) | Duas transações leem o mesmo valor e a 2ª gravação apaga a 1ª |
| **Leitura suja** (*dirty read*) | T1 lê um dado alterado por T2 **que ainda não fez COMMIT** (e T2 pode dar rollback) |
| **Leitura não repetível** | T1 lê o mesmo dado duas vezes e recebe valores diferentes |
| **Fantasma** (*phantom*) | Repetindo a mesma consulta, aparecem linhas novas |

**Escalonamento serializável:** a execução concorrente produz o **mesmo resultado** de alguma execução em série. É o "padrão ouro" de correção.

**Níveis de isolamento (SQL):** `READ UNCOMMITTED` → `READ COMMITTED` (**padrão do PostgreSQL**) → `REPEATABLE READ` → `SERIALIZABLE`. Quanto mais alto, menos anomalias e menos concorrência.

---

## 4. ➕ Controle de concorrência e recuperação de falhas
### Bloqueios (*locks*)
- **Compartilhado (S):** pra **ler**. Vários ao mesmo tempo
- **Exclusivo (X):** pra **escrever**. Ninguém mais lê nem escreve
- **2PL (bloqueio em duas fases):** primeiro a transação **só adquire** bloqueios (crescimento), depois **só libera** (encolhimento). Garante serializabilidade
- **Deadlock:** T1 bloqueia A e espera B; T2 bloqueia B e espera A → **ninguém anda**. O SGBD **detecta** (grafo de espera) e **aborta uma das transações** (a "vítima")
- Alternativas: **ordenação por timestamp** e **MVCC** (múltiplas versões: leitores não bloqueiam escritores, o que o PostgreSQL usa)

### Recuperação de falhas
- **Tipos de falha:** de **transação** (erro lógico, deadlock) · de **sistema** (queda de energia, travamento: perde a memória) · de **mídia** (disco queimou)
- **Log de transações:** registra cada alteração **antes** de gravar nos dados (*write-ahead log*)
- **UNDO** desfaz o que transações **não confirmadas** fizeram · **REDO** refaz o que transações **confirmadas** não chegaram a gravar no disco
- **Checkpoint:** ponto em que o buffer foi descarregado no disco, pra não precisar reler o log desde o início
- Falha de **mídia** → só **backup** + log salvam

---

## 5. ➕ Administração de BD
- **DBA:** instala e configura o SGBD, gerencia usuários e permissões, faz **backup e restore**, monitora o desempenho (índices, consultas lentas) e cuida da segurança
- **Permissões:**
```sql
CREATE ROLE estagiario LOGIN PASSWORD '...';
GRANT SELECT ON cliente TO estagiario;            -- concede
REVOKE DELETE ON cliente FROM estagiario;         -- retira
```
- **Princípio do menor privilégio:** cada usuário só com o que precisa (ex: o sistema não usa o superusuário `postgres`)
- **Backup no PostgreSQL:** `pg_dump` (gera o backup) e `pg_restore`/`psql` (restaura) · **manutenção:** `VACUUM` (limpa versões antigas das linhas) e `ANALYZE` (atualiza as estatísticas que o otimizador usa)

---

## 6. ➕ Tópicos do trabalho (seminário)
- **BD distribuído:** dados espalhados em vários servidores parecendo um banco só (**transparência**). **Fragmentação** horizontal (por linhas) ou vertical (por colunas), **replicação**, e **commit em duas fases (2PC)** pra garantir atomicidade entre servidores
- **NoSQL:** **chave-valor** (Redis) · **documento** (MongoDB) · **colunar** (Cassandra) · **grafo** (Neo4j). Troca parte do rigor do ACID por **escalabilidade e flexibilidade**
- Outros: orientados a objetos, **geográficos** (PostGIS), temporais, multimídia, semiestruturados (JSON/XML)

---

## 📝 Simulado: 2º Bimestre

### Questão 1
Quantos índices **clustered** uma tabela pode ter?
- a) Nenhum
- b) Apenas 1
- c) Até 2
- d) Um por coluna
- e) Ilimitados

> [!success]- Resposta: **B**
> O clustered define a **ordem física** dos dados, e só existe **uma** ordem física. Nonclustered podem ser vários. → Seção 1

### Questão 2
Qual coluna é a **melhor candidata** a receber um índice?
- a) Coluna `sexo`, com apenas 2 valores possíveis
- b) Chave estrangeira usada com frequência em JOINs
- c) Coluna de observações em texto longo, nunca filtrada
- d) Coluna de uma tabela com 30 linhas
- e) Coluna que nunca aparece em WHERE, JOIN, ORDER BY ou GROUP BY

> [!success]- Resposta: **B**
> **FKs em JOIN** são candidatas fortes. Coluna com poucos valores distintos, tabela pequena e coluna que nunca é consultada **não compensam** o custo do índice. → Seção 1

### Questão 3
Por que criar índices em **todas** as colunas de uma tabela é uma má ideia?
- a) Porque o SGBD só permite um índice por tabela
- b) Porque cada INSERT, UPDATE e DELETE precisa atualizar todos os índices, deixando as escritas lentas e gastando espaço
- c) Porque índices apagam os dados antigos
- d) Porque índices só funcionam no MySQL
- e) Porque índices impedem o uso de JOIN

> [!success]- Resposta: **B**
> Índice **acelera a leitura** e **atrasa a escrita**. Índice demais (ou nunca usado) só piora o desempenho. → Seção 1

### Questão 4
Qual é o tipo de índice **padrão do PostgreSQL**, que suporta `<`, `=`, `>`, `BETWEEN` e `LIKE` com prefixo?
- a) Hash
- b) R-tree
- c) B-tree
- d) GiST
- e) Bitmap

> [!success]- Resposta: **C**
> **B-tree** é o padrão. O **Hash** só serve pra igualdade (`=`) e o **R-tree** é pra dados geométricos. → Seção 1

### Questão 5
Na **otimização algébrica**, qual regra o otimizador aplica para reduzir o custo de uma consulta com JOIN e WHERE?
- a) Fazer todas as junções primeiro e filtrar no final
- b) Aplicar as seleções (filtros) o mais cedo possível, antes das junções
- c) Ignorar os índices existentes
- d) Sempre usar varredura sequencial
- e) Transformar a consulta em uma stored procedure

> [!success]- Resposta: **B**
> Filtrar cedo reduz as linhas que chegam às junções. O resultado é o mesmo, com muito menos trabalho. → Seção 2

### Questão 6
Uma transferência **debitou** da conta A, mas o sistema **caiu antes de creditar** em B. Ao reiniciar, o SGBD **desfez o débito**. Qual propriedade ACID foi garantida?
- a) Atomicidade
- b) Consistência
- c) Isolamento
- d) Durabilidade
- e) Disponibilidade

> [!success]- Resposta: **A**
> **Tudo ou nada.** Como a transação não terminou, nada dela pode ficar. (Durabilidade seria o caso **inverso**: confirmou e o dado sobreviveu à queda.) → Seção 3

### Questão 7
A transação **T1 lê um saldo alterado pela T2**, que **ainda não fez COMMIT**. Em seguida, T2 faz **ROLLBACK**. O problema que T1 sofreu é:
- a) Atualização perdida
- b) Leitura suja (dirty read)
- c) Deadlock
- d) Fantasma
- e) Fragmentação

> [!success]- Resposta: **B**
> Ler dado **não confirmado** = **leitura suja**. T1 trabalhou com um valor que "nunca existiu". → Seção 3

### Questão 8
Qual o resultado do `SELECT` final? (Antes, a conta 1 tinha saldo 500 e a conta 2 tinha 200.)
```sql
BEGIN;
UPDATE conta SET saldo = saldo - 100 WHERE id = 1;
UPDATE conta SET saldo = saldo + 100 WHERE id = 2;
ROLLBACK;
SELECT id, saldo FROM conta WHERE id IN (1, 2);
```
- a) 400 e 300
- b) 500 e 200
- c) 400 e 200
- d) 500 e 300
- e) Erro: não se pode fazer ROLLBACK depois de dois UPDATEs

> [!success]- Resposta: **B**
> O `ROLLBACK` desfaz **toda** a transação: os saldos voltam a **500 e 200**. → Seção 3

### Questão 9
**T1** bloqueou a linha A e espera pela linha B. **T2** bloqueou a linha B e espera pela linha A. Essa situação e a reação típica do SGBD são:
- a) Leitura suja; o SGBD ignora
- b) Deadlock; o SGBD detecta e aborta uma das transações (a vítima)
- c) Starvation; o SGBD aumenta a prioridade
- d) Thrashing; o SGBD adiciona memória
- e) Fantasma; o SGBD recria o índice

> [!success]- Resposta: **B**
> Espera circular = **deadlock**. O SGBD detecta (grafo de espera) e **aborta uma** pra liberar a outra. → Seção 4

### Questão 10
Para **retirar** do usuário `estagiario` a permissão de **apagar** registros da tabela `cliente`, o comando é:
- a) `GRANT DELETE ON cliente TO estagiario;`
- b) `REVOKE DELETE ON cliente FROM estagiario;`
- c) `DROP USER estagiario;`
- d) `DELETE FROM estagiario;`
- e) `ALTER TABLE cliente DISABLE estagiario;`

> [!success]- Resposta: **B**
> **GRANT** concede, **REVOKE** retira. (`DROP USER` apagaria o usuário inteiro, e não só a permissão.) → Seção 5
