---
tags:
  - poo
  - resumo
  - bimestre-2
bimestre: 2
fonte: claude
atualizado: 2026-11-08
---
# ☕ POO · 2º Bimestre
← [[POO - Painel]] · Anterior: [[POO - 1º Bimestre]] · Projeto: [[Projeto N2 - Guia]]

> [!info] Como usar
> Cada seção termina com um **📄 Fonte**. Seções marcadas com **➕** vêm de aulas **sem slides disponíveis** (04/11 e 11/11) e foram escritas a partir do cronograma do professor e do Java padrão. No fim tem um **simulado de 10 questões**.

> [!danger] Prova N2: **25/11** (unidades 4 a 7) · Atividades N2: **18/11** · Projeto: **02/12**
> **N2** = AI (1,0) + **Prova (5,0)** + **Projeto (2,0)** + Atividades (2,0)
> 📄 [[Faculdade/4_Semestre/POO/POO_Aula01.pdf#page=9|Aula 01 · p. 9]]

## 🗺️ Mapa do bimestre
| Data | OO | Tela e banco | Arquivo |
|---|---|---|---|
| 07/10 | Encapsulamento e invariantes | — | [[Faculdade/4_Semestre/POO/POO_Aula07.pdf\|Aula 07]] |
| 28/10 | Herança, sobrescrita, sobrecarga | Primeira janela (Swing) | [[Faculdade/4_Semestre/POO/POO_Aula08.pdf\|Aula 08]] |
| 04/11 | Polimorfismo, composição, abstratas | Conexão, gravar e listar (PreparedStatement) | ➕ *sem PDF* |
| 11/11 | Interfaces, exceção personalizada | DAO como interface, excluir, erro na tela | ➕ *sem PDF* |
| 18/11 | Revisão | Laboratório do projeto | — |

📄 **Fonte do cronograma:** [[Faculdade/4_Semestre/POO/Projeto Integrador N2 de POO.pdf#page=1|Projeto N2 · p. 1]]

---

## 1. Encapsulamento
> O estado do objeto **só muda por métodos que conhecem as regras**. Quem usa sabe **o que** o objeto faz, não **como** ele guarda os dados (*ocultação de informação*).

**O problema:** com atributo acessível, `conta.saldo = -5000.0;` **ignora a regra** que o `sacar()` respeitava.
**A analogia:** o caixa eletrônico. Você aperta botões (métodos `public`), mas **ninguém abre o cofre** (atributos `private`).

### Modificadores de acesso
| Modificador | UML | Quem acessa |
|---|:-:|---|
| `private` | **−** | Só a própria classe |
| *(nenhum)* | **~** | O **mesmo pacote** (não é público!) |
| `protected` | **#** | Pacote + subclasses |
| `public` | **+** | Qualquer classe |

**Receita:** atributos `private` · métodos que o main usa `public` · o que não muda depois do `new` é `final`.

```java
public class ContaBancaria {
    private final int numero;          // nunca muda: final e SEM setter
    private double saldo;

    public ContaBancaria(int numero) { this.numero = numero; this.saldo = 0.0; }

    public boolean sacar(double valor) {
        if (valor <= 0 || valor > saldo) return false;   // valida ANTES de mudar
        saldo = saldo - valor;
        return true;
    }
    public double getSaldo() { return saldo; }
}
// conta.saldo = -5000.0;  → error: saldo has private access in ContaBancaria
```

### Getter, setter ou método de negócio? Decida atributo por atributo
| Pergunta | Decisão | Exemplo |
|---|---|---|
| Precisa ser lido? | **getter** | `getSaldo()` |
| Pode receber qualquer valor válido? | **setter que valida** | `setCanal(int)`: só 1 a 99, senão `false` |
| Muda por uma ação do domínio? | **método de negócio** | `sacar()`, `depositar()` |
| Nunca muda? | **`final`, sem setter** | `numero` |

> [!warning] Getter e setter pra tudo NÃO é encapsulamento
> Um setter sem regra (`setSaldo(-5000)`) expõe o estado igual a um atributo `public`.

### Invariantes e contratos
- **Invariante:** regra que vale **sempre** (`saldo >= 0`, `quantidade >= 0`)
- **Pré-condição:** o que o método exige pra aceitar (`v > 0 && v <= saldo`)
- Proteja nos **dois lugares**: no **construtor** (nasce válido) e nos **métodos** (validam antes de mudar). **Recusa não muda nada.**

### Lista interna: devolva uma CÓPIA
```java
public ArrayList<String> getAlunos() { return new ArrayList<>(alunos); }
// turma.getAlunos().clear();  → limpa só a cópia, a turma continua igual
```

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula07.pdf#page=7|Aula 07 · p. 7-13]] · [[Faculdade/4_Semestre/POO/POO_Aula07.pdf#page=15|getters/setters e invariantes · p. 15-18]] · [[Faculdade/4_Semestre/POO/POO_Aula07.pdf#page=19|erros · p. 19]]

---

## 2. Herança
> A subclasse **"é um"** tipo da superclasse e **herda** atributos e métodos. O comum fica num lugar só.

- Gerente **é um** Funcionário ✅ → herança
- Carro **é um** Motor ❌ → **não herde** (Carro **tem um** Motor = composição)

```java
public class Gerente extends Funcionario {         // extends = herda
    private double bonus;
    public Gerente(String nome, double salario, double bonus) {
        super(nome, salario);                       // 1ª LINHA: chama o construtor da mãe
        this.bonus = bonus;
    }
    @Override                                       // avisa: estou sobrescrevendo
    public double calcularPagamento() {
        return super.calcularPagamento() + bonus;   // reaproveita a versão da mãe
    }
}
```
| Recurso | Função |
|---|---|
| `extends` | Declara a herança (só **uma** superclasse) |
| `super(...)` | Chama o **construtor da mãe**. Tem que ser a **1ª linha** |
| `super.metodo()` | Chama a **versão da mãe** de um método |
| `@Override` | O compilador confere a assinatura. Se você errar o nome, ele acusa |

- **UML:** seta com **triângulo vazado** apontando pra superclasse
- **Memória:** o objeto Gerente tem uma "parte Funcionario" (nome, salario) + uma "parte Gerente" (bonus)
- **`private` da mãe:** existe dentro do objeto, mas a filha **não acessa direto**. `return salario + bonus;` → *salario has private access*. Use os métodos da mãe (`super.calcularPagamento()`). `protected` abre pra todas as subclasses: **prefira `private`**
- **Ordem dos construtores:** de cima pra baixo. Criar um `SmartphoneDobravel` roda Produto → Eletronico → Celular → Smartphone → SmartphoneDobravel

### Sobrescrita × Sobrecarga (cai muito!)
| | **Sobrescrita** (*override*) | **Sobrecarga** (*overload*) |
|---|---|---|
| Onde | Na **subclasse** | Na **mesma classe** |
| Assinatura | **Igual** | **Parâmetros diferentes** |
| Quem escolhe | O **objeto**, em **execução** | O **compilador** |

**Erros comuns:** esquecer `super(...)` (não compila) · usar `private` da mãe (não compila) · **errar o nome sem `@Override`** (compila e roda errado, em silêncio) · herdar sem ser "é um" (compila, mas o modelo está errado)

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula08.pdf#page=6|Aula 08 · p. 6-15]] · [[Faculdade/4_Semestre/POO/POO_Aula08.pdf#page=30|lista da hierarquia · p. 30-32]]

---

## 3. Primeira janela: Swing
**A janela também é uma classe, e usa herança:** `TelaOla extends JFrame` (TelaOla **é um** JFrame).

```java
public class TelaOla extends JFrame {
    private JTextField campoNome;
    private JButton botaoSaudar;
    private JLabel mensagem;

    public TelaOla() {
        super("Primeira janela");                     // título
        setLayout(new FlowLayout());
        campoNome = new JTextField(10);
        botaoSaudar = new JButton("Saudar");
        mensagem = new JLabel("Digite seu nome");
        add(campoNome); add(botaoSaudar); add(mensagem);
        botaoSaudar.addActionListener(e ->             // EVENTO: só roda no clique
            mensagem.setText("Olá, " + campoNome.getText()));
        setSize(340, 95);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
    }
}
// main: new TelaOla().setVisible(true);
```
**Receita em 5 passos:** (1) `extends JFrame` + `super("Título")` → (2) componentes como atributos `private` → (3) no construtor: criar, `add(...)`, eventos → (4) `setSize` + `setDefaultCloseOperation` → (5) no main: `new Tela()` + `setVisible(true)`

| Componente | Pra quê |
|---|---|
| `JFrame` | A janela |
| `JLabel` | Texto fixo |
| `JTextField` | Campo de digitação |
| `JButton` | Botão |
| `JTextArea` | Área com várias linhas (ex: listar registros) |

- Número digitado: `Double.parseDouble(campo.getText())`
- 🔑 **A tela só monta e mostra. A regra de negócio fica nas classes do modelo** (o preço da meia-entrada é calculado em `MeiaEntrada`, não na tela)

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula08.pdf#page=16|Aula 08 · p. 16-18]] · [[Faculdade/4_Semestre/POO/POO_Aula08.pdf#page=25|bilheteria · p. 25-27]]

---

## 4. ➕ Polimorfismo
> **A mesma chamada, comportamento diferente**, conforme o **objeto real**.

```java
List<Ingresso> vendas = new ArrayList<>();
vendas.add(new Ingresso("Duna", 40.0));
vendas.add(new MeiaEntrada("Duna", 40.0));
vendas.add(new IngressoVip("Duna", 40.0));

for (Ingresso i : vendas) {
    System.out.println(i.getPreco());   // 40.0 · 20.0 · 60.0
}
```
- A variável é do tipo `Ingresso`, mas executa o `getPreco()` **da classe do objeto real** → **ligação dinâmica**, decidida **em execução**
- É exatamente o item **A4 do projeto**: sobrescrita + polimorfismo numa `List<Superclasse>`

---

## 5. ➕ Classe abstrata
> Classe que **não pode ser instanciada**. Serve de **modelo** pra família.

```java
public abstract class Atendimento {
    private final Animal animal;
    public Atendimento(Animal animal) { this.animal = animal; }
    public abstract double calcularValor();        // sem corpo: a filha É OBRIGADA a implementar
    public String resumo() { return animal.getNome() + ": R$ " + calcularValor(); }
}
public class Consulta extends Atendimento {
    public Consulta(Animal a) { super(a); }
    @Override public double calcularValor() { return 150.0; }
}
// new Atendimento(a);              → NÃO compila
// Atendimento x = new Consulta(a); → compila (variável do tipo abstrato, objeto concreto)
```
**Quando usar:** quando o conceito é **geral demais pra existir sozinho** ("um atendimento genérico" não existe, só consulta, vacina, banho). Pode ter atributos, construtor e métodos com corpo.

---

## 6. ➕ Interface
> Um **contrato**: diz **O QUE** a classe sabe fazer, não **COMO**.

```java
public interface Cobravel {
    double calcularValor();              // implicitamente public abstract
}
public class Consulta extends Atendimento implements Cobravel { ... }
public class Produto implements Cobravel { ... }   // classes sem relação de herança
```
| | Classe abstrata | Interface |
|---|---|---|
| Relação | **"é um"** | **"sabe fazer"** |
| Quantas por classe | **Só 1** (`extends`) | **Várias** (`implements A, B`) |
| Atributos | De instância | Só constantes |
| Construtor | Tem | Não tem |

---

## 7. ➕ Composição
> Um objeto **é formado por** outros: relação **"tem um"**.

```java
public class Pedido {
    private final Cliente cliente;                       // Pedido TEM UM Cliente
    private final List<Item> itens = new ArrayList<>();  // e TEM vários Itens
}
```
**Por que não herança?** Pedido **não é um** Cliente. Herdar criaria um modelo falso. *"Prefira composição à herança"* é um princípio clássico de mercado: herança acopla muito.

---

## 8. ➕ Exceções
```java
// 1. A exceção personalizada
public class SaldoInsuficienteException extends Exception {
    public SaldoInsuficienteException(String msg) { super(msg); }
}
// 2. No MODELO (regra de negócio): LANÇA
public void sacar(double valor) throws SaldoInsuficienteException {
    if (valor > saldo) throw new SaldoInsuficienteException("Saldo insuficiente");
    saldo -= valor;
}
// 3. Na TELA: CAPTURA e mostra
try {
    conta.sacar(valor);
} catch (SaldoInsuficienteException ex) {
    JOptionPane.showMessageDialog(this, ex.getMessage());
} finally {
    // roda SEMPRE (ex: fechar recurso)
}
```
| Termo | Significado |
|---|---|
| `throw` | **Lança** a exceção |
| `throws` | **Declara** na assinatura que pode lançar |
| `try / catch` | **Tenta** e **captura** |
| `finally` | Executa **sempre**, com ou sem erro |
| `extends Exception` | **Checked**: o compilador **obriga** a tratar |
| `extends RuntimeException` | **Unchecked**: não obriga |

Quando a exceção é lançada, **o resto do `try` é pulado**: vai direto pro `catch` e depois pro `finally`.

---

## 9. ➕ Banco: JDBC, DAO e PreparedStatement
```java
// DAO como interface: o contrato da persistência
public interface ClienteDAO {
    void inserir(Cliente c) throws SQLException;
    List<Cliente> listar() throws SQLException;
    void excluir(int id) throws SQLException;
}
// Implementação: TODO o SQL fica aqui
public void inserir(Cliente c) throws SQLException {
    String sql = "INSERT INTO cliente (nome, email) VALUES (?, ?)";
    try (Connection con = Conexao.obter();                  // try-with-resources fecha sozinho
         PreparedStatement ps = con.prepareStatement(sql)) {
        ps.setString(1, c.getNome());
        ps.setString(2, c.getEmail());
        ps.executeUpdate();                                 // INSERT/UPDATE/DELETE
    }
}
// SELECT: ps.executeQuery() devolve um ResultSet → while (rs.next()) { rs.getString("nome") ... }
```
- **Caminho do clique até o banco:** botão Salvar → listener da tela → cria o objeto (que **valida**) → `dao.inserir(obj)` → `PreparedStatement` → `executeUpdate()`
- **Por que PreparedStatement?** Os `?` são parâmetros: o que o usuário digita **nunca vira código SQL** → **impede SQL Injection** (OWASP A03). Com concatenação, digitar `' OR '1'='1` entraria sem senha
- **Por que a tela não tem SQL?** **Separação de responsabilidades**: trocou o banco, só o DAO muda
- **Senha fora do Git:** `config.properties` no `.gitignore` + `config.exemplo.properties` sem dados reais
- **Transação** (ponto extra): `con.setAutoCommit(false);` → os dois INSERTs → `con.commit();` ou, no erro, `con.rollback();` (desfaz os dois)
- **Banco fora do ar?** `getConnection` lança `SQLException` → a tela captura e mostra uma mensagem em vez de travar

📄 **Fonte (requisitos):** [[Faculdade/4_Semestre/POO/Projeto Integrador N2 de POO.pdf#page=2|Projeto N2 · p. 2-3]] · [[Faculdade/4_Semestre/POO/Projeto Integrador N2 de POO.pdf#page=7|config e drivers · p. 7]]

---

## 📝 Simulado: 2º Bimestre

### Questão 1
Um atributo declarado **sem modificador de acesso** (`int canal;`) pode ser acessado por:
- a) Qualquer classe do projeto
- b) Apenas a própria classe
- c) Apenas classes do mesmo pacote
- d) Apenas subclasses
- e) Nenhuma classe

> [!success]- Resposta: **C**
> É o acesso **padrão (~)**: mesmo pacote. **Não é público!** → Seção 1

### Questão 2
`getAlunos()` devolve `new ArrayList<>(alunos)`. Qual a saída?
```java
Turma t = new Turma(3);
t.matricular("Ana");
t.matricular("Bia");
ArrayList<String> lista = t.getAlunos();
lista.clear();
System.out.println(t.getQuantidade());
```
- a) 0
- b) 2
- c) 3
- d) Erro de compilação
- e) `NullPointerException`

> [!success]- Resposta: **B**
> O getter devolve uma **cópia**: o `clear()` limpa só a cópia, e a lista interna da turma continua com 2. → Seção 1

### Questão 3
Sobre `super(nome, salario);` no construtor de `Gerente extends Funcionario`:
- a) Cria um segundo objeto Funcionario separado
- b) Chama o construtor da superclasse e deve ser a primeira instrução do construtor
- c) Pode ficar em qualquer linha do construtor
- d) Só é necessário quando a superclasse é abstrata
- e) Sobrescreve o construtor da superclasse

> [!success]- Resposta: **B**
> O `super(...)` inicializa a "parte Funcionario" do mesmo objeto e precisa ser a **1ª linha**. → Seção 2

### Questão 4
`salario` é `private` em `Funcionario`. Dentro de `Gerente extends Funcionario`, o que acontece com `return salario + bonus;`?
- a) Funciona normalmente, pois a herança dá acesso a tudo
- b) Retorna só o bônus
- c) Não compila: `salario has private access in Funcionario`
- d) Lança exceção em execução
- e) Funciona só se o método tiver `@Override`

> [!success]- Resposta: **C**
> A filha **herda**, mas **não acessa** o `private` da mãe. Solução: `super.calcularPagamento() + bonus`. → Seção 2

### Questão 5
`Funcionario` tem `calcularPagamento()` e também `calcularPagamento(double horasExtras)`. Esse segundo método é:
- a) Sobrescrita
- b) Sobrecarga
- c) Polimorfismo de subclasse
- d) Herança múltipla
- e) Erro de compilação

> [!success]- Resposta: **B**
> **Mesma classe** e **parâmetros diferentes** = sobrecarga. Sobrescrita seria na subclasse, com a **mesma** assinatura. → Seção 2

### Questão 6
Com as classes da bilheteria (`Ingresso` devolve o valor, `MeiaEntrada` devolve metade e `IngressoVip` devolve valor + 20), qual a saída?
```java
List<Ingresso> lista = List.of(
    new Ingresso("Duna", 40.0),
    new MeiaEntrada("Duna", 40.0),
    new IngressoVip("Duna", 40.0));
double total = 0;
for (Ingresso i : lista) total += i.getPreco();
System.out.println(total);
```
- a) 120.0
- b) 100.0
- c) 140.0
- d) 40.0
- e) Erro: a lista é de Ingresso e não aceita subclasses

> [!success]- Resposta: **A**
> **Polimorfismo:** cada objeto executa o **seu** `getPreco()`: 40 + 20 + 60 = **120.0**. Uma lista da superclasse aceita objetos das subclasses. → Seção 4

### Questão 7
`Atendimento` é **abstrata** e `Consulta` é uma subclasse concreta. Qual linha **compila**?
- a) `Atendimento a = new Atendimento(animal);`
- b) `Atendimento a = new Consulta(animal);`
- c) `Consulta c = new Atendimento(animal);`
- d) `Consulta c = (Atendimento) new Consulta(animal);`
- e) Nenhuma

> [!success]- Resposta: **B**
> Não dá pra fazer `new` de classe abstrata (a, c). Mas uma **variável do tipo abstrato** pode guardar um **objeto concreto** da subclasse. A (d) não compila: atribui um `Atendimento` a uma variável `Consulta` sem cast pra baixo. → Seção 5

### Questão 8
Um `Pedido` possui um `Cliente` e uma lista de `Item`. A modelagem correta é:
- a) `Pedido extends Cliente`
- b) `Cliente extends Pedido`
- c) Composição: `Pedido` tem atributos `Cliente` e `List<Item>`
- d) `Item` deve ser uma interface implementada por `Pedido`
- e) `Pedido` deve ser abstrata

> [!success]- Resposta: **C**
> Pedido **não é um** Cliente: ele **tem** um Cliente. Relação "tem um" = **composição**. → Seção 7

### Questão 9
`conta.sacar(500)` lança `SaldoInsuficienteException` (o saldo é 100). Qual a saída?
```java
try {
    conta.sacar(500);
    System.out.println("A");
} catch (SaldoInsuficienteException e) {
    System.out.println("B");
} finally {
    System.out.println("C");
}
```
- a) A C
- b) B C
- c) A B C
- d) B
- e) C

> [!success]- Resposta: **B**
> A exceção é lançada e **pula o resto do try** (o "A" não imprime). Vai pro `catch` ("B") e o `finally` roda **sempre** ("C"). → Seção 8

### Questão 10
Por que o projeto exige `PreparedStatement` com `?` em vez de montar o SQL concatenando texto?
- a) Porque é a única forma de fazer SELECT em Java
- b) Porque deixa o código mais curto
- c) Porque o que o usuário digita é tratado como dado, e nunca como código SQL, o que impede SQL Injection
- d) Porque dispensa a conexão com o banco
- e) Porque permite colocar o SQL dentro da tela

> [!success]- Resposta: **C**
> É a defesa clássica contra **SQL Injection**. E o SQL fica **só no DAO**, nunca na tela. → Seção 9
