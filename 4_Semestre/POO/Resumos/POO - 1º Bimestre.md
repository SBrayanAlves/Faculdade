---
tags:
  - poo
  - resumo
  - bimestre-1
bimestre: 1
fonte: claude
atualizado: 2026-11-08
---
# ☕ POO · 1º Bimestre
← [[POO - Painel]] · Próximo: [[POO - 2º Bimestre]]

> [!info] Como usar
> Cada seção termina com um **📄 Fonte** que abre o PDF **na página exata**. No fim tem um **simulado de 10 questões** (leitura de código, conceito e uma prática).

> [!warning] Por que estudar isso?
> É o conteúdo da **Prova N1** (unidades 1 a 3), que também cai numa **segunda chamada**. E tudo da N2 se apoia aqui.

## 🗺️ Mapa do bimestre
| Aula | Data | Tema | Arquivo |
|---|---|---|---|
| 1 | 12/08 | Panorama das linguagens: tipagem, memória, execução | [[Faculdade/4_Semestre/POO/POO_Aula01.pdf\|Aula 01]] |
| 2 | 19/08 | Paradigmas, classe, objeto, atributo, método | [[Faculdade/4_Semestre/POO/POO_Aula02.pdf\|Aula 02]] |
| 3 | 26/08 | Fundamentos de Java: JDK, tipos, Scanner, casting | [[Faculdade/4_Semestre/POO/POO_Aula03.pdf\|Aula 03]] |
| 4 | 02/09 | Construtores, `this`, construtor padrão | [[Faculdade/4_Semestre/POO/POO_Aula04.pdf\|Aula 04]] |
| 5 | 09/09 | Parâmetros, String imutável, arrays e ArrayList | [[Faculdade/4_Semestre/POO/POO_Aula05.pdf\|Aula 05]] |
| 6 | 16/09 | Sobrecarga de métodos + o quadro completo | [[Faculdade/4_Semestre/POO/POO_Aula06.pdf\|Aula 06]] |
| — | — | Lista de Revisão (3 exercícios no estilo da prova) | [[Faculdade/4_Semestre/POO/POO_Lista_Revisao-1.pdf\|Lista de Revisão]] |

**N1** = AD (1,0) + **Prova (7,0)** + Atividades (2,0). Aprovação: média ≥ 6 (**de 5,75 a 5,99 vira 6,0**) e 75% de frequência. **A segunda chamada substitui só a Prova N1 ou só a N2.**
📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula01.pdf#page=8|Aula 01 · p. 8-12]]

---

## 1. Panorama das linguagens (os 3 eixos)
Uma linguagem é uma **camada de tradução** entre a sua intenção e a máquina. Subir de nível (máquina → Assembly → alto nível → domínio específico, como SQL) **perde desempenho e controle**, mas **ganha legibilidade, portabilidade e velocidade de entrega**.

| Eixo | Pergunta | Java |
|---|---|---|
| **Tipagem** | Quando e com que rigor o tipo é checado? | **Estática e forte** |
| **Memória** | Quem aloca e libera? | **Coletor de lixo** (você faz `new`, nunca libera) |
| **Execução** | Como o código chega à CPU? | **Bytecode na JVM**, otimizado pelo **JIT** |

- **Estática × dinâmica** = **quando** o tipo é checado (compilação × execução)
- **Forte × fraca** = **rigor na conversão implícita**. JavaScript faz `1 + "1"` = `"11"` (fraca). Java recusa somar `boolean` com `int` (forte)
- `var total = 10;` **continua estático**: é só **inferência**, o compilador fixa `int`
- **Java vaza memória?** Sim, se você **segurar referências sem precisar** (ex: lista estática que só cresce)
- **Java é compilada ou interpretada?** **As duas**: `javac` gera bytecode e a JVM o executa, compilando os trechos mais usados com o **JIT**

| | C | Java | Python | JavaScript | Rust |
|---|---|---|---|---|---|
| Tipagem | Estática, fraca | **Estática, forte** | Dinâmica, forte | Dinâmica, fraca | Estática, forte |
| Memória | Manual | **Coletor de lixo** | Contagem + coletor | Coletor | Propriedade |
| Execução | Nativo | **Bytecode na JVM** | Interpretada | Interpretada + JIT | Nativo |

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula01.pdf#page=15|Aula 01 · p. 15-25]]

---

## 2. Paradigmas e por que OO
| Paradigma | Unidade central | Ideia | Linguagens |
|---|---|---|---|
| Procedural | Procedimento | Passo a passo que altera o estado | C, Pascal |
| **Orientado a Objetos** | **Objeto** | **Dados + comportamento juntos**, estado protegido | **Java**, C# |
| Funcional | Função | Dados imutáveis, sem efeito colateral | Haskell, Elixir |
| Lógico | Regra e fato | Você declara, o motor deduz | Prolog |

Procedural e OO são **imperativos** (descrevem o caminho). Funcional e lógico são **declarativos** (descrevem o resultado). Java é **híbrida**: OO com estilo funcional (lambdas) desde o Java 8.

**O problema que a OO resolve:** sem objetos, os dados de um aluno ficam soltos (`nomeAluno1`, `notaAluno1`...). Com 200 alunos vira caos. Com objetos, **cada aluno é uma coisa só**.

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula02.pdf#page=5|Aula 02 · p. 5-8]]

---

## 3. Classe, objeto, atributo e método
| Termo | O que é | Exemplo |
|---|---|---|
| **Classe** | O **molde**: define dados e comportamento. Sozinha não guarda valor | `class Aluno { ... }` |
| **Objeto** | A **instância** criada com `new`, com estado próprio | `Aluno a = new Aluno();` |
| **Atributo** | Variável **da classe**, fora dos métodos: o **estado** | `String nome;` |
| **Método** | O **comportamento** | `void exibir()` |
| **Referência** | A variável que **aponta** pro objeto | `a` |

```java
public class Aluno {            // arquivo Aluno.java (mesmo nome da classe!)
    String nome;                // atributo
    double nota;
    void exibir() {             // método sem retorno
        System.out.println(nome + " - " + nota);
    }
    double dobro() {            // método com retorno
        return nota * 2;
    }
}
```
- **Valores padrão** de atributo não preenchido: `null` (String e objetos), `0`, `0.0`, `false`
- **`void`** = não devolve nada · **tipo antes do nome** = o que o `return` devolve
- Cada `new` cria um espaço próprio: alterar `a1` **não muda** `a2`

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula02.pdf#page=9|Aula 02 · p. 9-14]] · [[Faculdade/4_Semestre/POO/POO_Aula02.pdf#page=21|glossário · p. 21]]

---

## 4. Fundamentos de Java
### Ambiente e ciclo
- **JDK ⊃ JRE ⊃ JVM.** **JDK** compila (tem o `javac`). **JRE** só roda. **JVM** executa o bytecode e torna o Java portável
- `javac Ola.java` → gera `Ola.class` (bytecode) → `java Ola` (**sem** `.class`)
- A **classe pública** precisa estar num arquivo **com o mesmo nome**
- `main` é o ponto de entrada: `public static void main(String[] args)`
- **Convenções:** `ClassePascalCase` · `metodoEVariavelCamelCase` · `CONSTANTE_MAIUSCULA` · `pacote.minusculo`

### Tipos, constantes e conversões
- **8 primitivos:** `byte, short, int, long, float, double, char, boolean`. **String NÃO é primitivo** (é classe)
- Literais: `long p = 8100000000L;` · `float t = 2.5f;` · `char c = 'A';`
- **Variável local sem valor não compila** (o valor padrão só vale para atributos)
- **`final`** = constante, atribuída uma vez só. Evita "número mágico"
- **Casting:** automático pra cima (`int → double`), explícito pra baixo (`(int) 3.9` = **3**, corta)
- ⚠️ **Divisão inteira:** `7 / 2` = **3** · `7 / 2.0` = **3.5** · `9 / 5 * 25 + 32` dá 57 (errado!), `9.0 / 5 * 25 + 32` dá 77.0

### Entrada e saída
- `println` (pula linha) · `print` (não pula) · `printf("%.2f%n", x)` (formata: `%d` int, `%s` texto, `%.2f` 2 casas)
- **Scanner:** `import java.util.Scanner;` · ⚠️ **depois de `nextInt()`, chame `nextLine()`** antes de ler texto · com o sistema em português, `nextDouble` espera **vírgula**

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula03.pdf#page=6|Aula 03 · p. 6-15]] · [[Faculdade/4_Semestre/POO/POO_Aula03.pdf#page=21|erros comuns · p. 21]]

---

## 5. Construtores
**Problema:** sem construtor, o objeto nasce vazio e nada obriga a preencher (preço 0,00 na nota fiscal, sem erro nenhum).

```java
public class Produto {
    String nome;
    double preco;

    Produto(String nome, double preco) {   // mesmo nome da classe, SEM void
        this.nome = nome;                   // this.nome = atributo · nome = parâmetro
        this.preco = preco;
    }
    Produto(String nome) {                  // sobrecarga de construtor
        this(nome, 0.0);                    // reaproveita o de cima
    }
}
```
**O que o `new` faz:** (1) reserva o espaço com os valores padrão → (2) executa o construtor → (3) devolve a referência.

| Regra | Detalhe |
|---|---|
| **Construtor padrão** | Existe **só enquanto você não escreve nenhum**. Escreveu um com parâmetro → `new Produto()` **para de compilar** |
| **`this`** | O próprio objeto. Resolve o conflito de nomes entre atributo e parâmetro |
| **`nome = nome;` sem this** | ⚠️ **Compila, roda e o objeto sai com `null`**: erro silencioso! |
| **`void Produto(...)`** | ⚠️ Vira **método comum** e deixa de ser construtor |
| **Construtor que valida** | `if (saldoInicial >= 0) ... else this.saldo = 0;` → **nenhum objeto inválido chega a existir** |

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula04.pdf#page=8|Aula 04 · p. 8-16]] · [[Faculdade/4_Semestre/POO/POO_Aula04.pdf#page=27|erros · p. 27]]

---

## 6. Parâmetros, String e imutabilidade
> **O Java sempre passa CÓPIA.** Pra primitivo, copia o **valor**. Pra objeto, copia a **referência** (que aponta pro mesmo objeto).

| Situação | O original muda? |
|---|---|
| `void dobrar(int n) { n = n * 2; }` | ❌ Não (mexeu na cópia) |
| `void renomear(Animal a) { a.nome = "Leão"; }` | ✅ **Sim** (mesmo objeto) |
| `void trocar(Animal a) { a = new Animal(...); }` | ❌ Não (só a cópia local aponta pro novo) |

**Regra prática:** mexer no **conteúdo**, sim. Trocar **a caixa inteira**, só devolvendo com `return`.

- **String é imutável:** `nome.toUpperCase();` sozinho **não muda nada**. Faça `nome = nome.toUpperCase();`
- **Compare texto com `equals()`**, nunca com `==` (`==` compara se é o **mesmo objeto**)
- **`final` em objeto** impede trocar a referência, mas **os atributos podem mudar**

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula05.pdf#page=6|Aula 05 · p. 6-10]]

---

## 7. Coleções: array e ArrayList
| | **Array** | **ArrayList** |
|---|---|---|
| Tamanho | **Fixo** no `new` | **Cresce e diminui** |
| Criar | `new Animal[3]` | `new ArrayList<>()` |
| Adicionar | `v[0] = a` | `lista.add(a)` |
| Ler | `v[0]` | `lista.get(0)` |
| Tamanho | `v.length` (**sem** parênteses) | `lista.size()` (**com** parênteses) |
| Remover | Não remove (só sobrescreve) | `lista.remove(0)` (as posições **se deslocam**) |

- Índice vai de **0 a length − 1**. Acessar fora → `ArrayIndexOutOfBoundsException`
- Posição de array não preenchida vale `null` → chamar método nela dá `NullPointerException`
- `for (int i = 0; i < v.length; i++)` quando precisa da **posição** · `for (Animal a : lista)` (for-each) quando só **visita todos**
- **A lista guarda referências, não cópias:** `lista.get(0).nome = "Leão"` altera o objeto original
- **Ciclo de vida:** nasce (`new`) → é usado → fica sem referência → **o coletor de lixo recolhe**

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula05.pdf#page=12|Aula 05 · p. 12-18]] · [[Faculdade/4_Semestre/POO/POO_Aula05.pdf#page=28|erros · p. 28]]

---

## 8. Sobrecarga de métodos
Mesmo nome, **listas de parâmetros diferentes**, na **mesma classe**.

**Assinatura** = nome + **quantidade, tipo e ordem** dos parâmetros. **NÃO entram:** tipo de retorno, nome dos parâmetros, corpo.

| Par de métodos | Compila? | Por quê |
|---|:-:|---|
| `somar(int a, int b)` e `somar(int a, int b, int c)` | ✅ | Quantidade diferente |
| `somar(int a, int b)` e `somar(double a, double b)` | ✅ | Tipo diferente |
| `mover(int x, String s)` e `mover(String s, int x)` | ✅ | Ordem diferente |
| `int somar(int a, int b)` e `double somar(int a, int b)` | ❌ | **O retorno não conta** |
| `somar(int a, int b)` e `somar(int x, int y)` | ❌ | **O nome do parâmetro não conta** |

**Como o Java escolhe:** pelo nome → pela quantidade → pelo tipo exato → se precisar, **converte** (um `int` cabe em `double`). Com `somar(int,int)` e `somar(double,double)`, a chamada `somar(2, 3.0)` usa a versão **double**. **A escolha é feita na compilação.**

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula06.pdf#page=6|Aula 06 · p. 6-10]]

---

## 9. O quadro completo e os erros que mais custam nota
```java
public class Animal {
    String nome; String som;
    Animal(String nome, String som) { this.nome = nome; this.som = som; }  // construtor
    Animal(String nome) { this(nome, "..."); }                             // sobrecarga de construtor
    void falar() { System.out.println(nome + " faz " + som); }            // void
    void falar(int vezes) { for (int i = 0; i < vezes; i++) falar(); }     // sobrecarga de método
    boolean temSom(String s) { return som.equals(s); }                     // retorno + parâmetro
}
```
**Os 6 erros campeões:**
1. `nome = nome;` sem `this` → **silencioso**, objeto com `null`
2. `void Animal(...)` → deixa de ser construtor (**silencioso**)
3. `new Animal()` depois de criar outro construtor → não compila
4. Dois métodos com a mesma assinatura → não compila
5. Texto comparado com `==` → resultado errado
6. `animais[3]` num array de 3 → `ArrayIndexOutOfBoundsException`

> [!tip] Treine com a Lista de Revisão
> 3 exercícios no formato de prova: **Musica** (construtor com `this` e método boolean) · **Aluno** (dois construtores com `this(...)` e bônus limitado a 10) · **Biblioteca** (ArrayList, `adicionar` sobrecarregado, buscar com `equals`, menu com `do while` + `switch`).
> 📄 [[Faculdade/4_Semestre/POO/POO_Lista_Revisao-1.pdf|Lista de Revisão]]

📄 **Fonte:** [[Faculdade/4_Semestre/POO/POO_Aula06.pdf#page=13|Aula 06 · p. 13-17]] · [[Faculdade/4_Semestre/POO/POO_Aula06.pdf#page=28|resumo · p. 28]]

---

## 📝 Simulado: 1º Bimestre

### Questão 1
Sobre `var total = 10;` em Java, é correto afirmar:
- a) Torna o Java dinamicamente tipado naquela variável
- b) É inferência de tipo: o compilador fixa `int` e a tipagem continua estática
- c) Permite depois fazer `total = "dez";`
- d) Só funciona em Java 8
- e) Torna a variável uma constante

> [!success]- Resposta: **B**
> `var` só evita escrever o tipo. O compilador deduz `int` e **o tipo nunca muda**. → Seção 1

### Questão 2
Sobre JDK, JRE e JVM, assinale a correta:
- a) A JVM compila o código-fonte `.java`
- b) O JRE contém o compilador `javac`
- c) O JDK contém o JRE, que contém a JVM; para compilar é preciso o JDK
- d) O bytecode é código de máquina nativo do processador
- e) Para executar, usa-se `java Ola.class`

> [!success]- Resposta: **C**
> Quem compila é o `javac` (do JDK). O bytecode **não** é nativo: quem o executa é a JVM. E roda-se com `java Ola`, **sem** `.class`. → Seção 4

### Questão 3
Qual a saída?
```java
int a = 7, b = 2;
System.out.println(a / b + " " + a / (double) b);
```
- a) `3.5 3.5`
- b) `3 3.5`
- c) `3 3`
- d) `3.0 3.5`
- e) Erro de compilação

> [!success]- Resposta: **B**
> `7 / 2` entre inteiros = **3** (descarta o resto). Com um lado `double`, vira divisão real: **3.5**. → Seção 4

### Questão 4
Qual a saída?
```java
class Produto {
    String nome; double preco;
    Produto(String nome, double preco) {
        nome = nome;
        preco = preco;
    }
}
// no main:
Produto p = new Produto("Caneta", 3.5);
System.out.println(p.nome + " " + p.preco);
```
- a) `Caneta 3.5`
- b) `null 0.0`
- c) Erro de compilação
- d) `NullPointerException`
- e) ` 0`

> [!success]- Resposta: **B**
> Sem `this`, o **parâmetro recebe ele mesmo** e os atributos ficam com o valor padrão. **Compila e roda em silêncio.** É o erro nº 1 do professor. → Seção 5

### Questão 5
A classe `Animal` tem **apenas** o construtor `Animal(String nome)`. O que acontece com `Animal a = new Animal();`?
- a) Cria o animal com nome `null`
- b) Cria o animal com nome vazio `""`
- c) Não compila, pois o construtor padrão deixou de existir
- d) Lança `NullPointerException` em execução
- e) O Java cria o construtor padrão automaticamente

> [!success]- Resposta: **C**
> O construtor padrão só existe **enquanto nenhum construtor foi escrito**. → Seção 5

### Questão 6
Qual a saída?
```java
static void dobrar(int n) { n = n * 2; }
static void renomear(Animal a) { a.nome = "Leão"; }

int x = 5;
Animal b = new Animal("Gato");
dobrar(x);
renomear(b);
System.out.println(x + " " + b.nome);
```
- a) `10 Leão`
- b) `5 Gato`
- c) `5 Leão`
- d) `10 Gato`
- e) Erro de compilação

> [!success]- Resposta: **C**
> `int` vai por **cópia do valor** (x continua 5). O objeto vai por **cópia da referência**, que aponta pro mesmo Animal, então o nome muda. → Seção 6

### Questão 7
Qual par de métodos **NÃO compila** na mesma classe?
- a) `void somar(int a, int b)` e `void somar(int a, int b, int c)`
- b) `int somar(int a, int b)` e `double somar(int a, int b)`
- c) `int somar(int a, int b)` e `double somar(double a, double b)`
- d) `void mover(int x, String s)` e `void mover(String s, int x)`
- e) `Animal(String nome)` e `Animal(String nome, String som)`

> [!success]- Resposta: **B**
> Mesma assinatura (nome + `int, int`). **O tipo de retorno não diferencia.** → Seção 8

### Questão 8
A classe `Calculadora` tem `int somar(int a, int b)` e `double somar(double a, double b)`. O que imprime `System.out.println(c.somar(2, 3.0));`?
- a) `5`
- b) `5.0`
- c) Erro de compilação por ambiguidade
- d) `23.0`
- e) `5.03`

> [!success]- Resposta: **B**
> Não há versão `(int, double)`. O `2` é convertido para `double`, e a versão `double` é escolhida: **5.0**. → Seção 8

### Questão 9
Qual a saída?
```java
ArrayList<String> lista = new ArrayList<>();
lista.add("A");
lista.add("B");
lista.add("C");
lista.remove(0);
System.out.println(lista.get(0) + " " + lista.size());
```
- a) `A 3`
- b) `A 2`
- c) `B 2`
- d) `B 3`
- e) `IndexOutOfBoundsException`

> [!success]- Resposta: **C**
> Remover a posição 0 tira o "A" e **desloca** o resto: "B" vira a posição 0. Sobram 2 itens. → Seção 7

### Questão 10 (prática)
Escreva a classe **`Conta`** com `titular` e `saldo`, contendo:
1. Um construtor que receba titular e saldo inicial e **não permita saldo negativo** (nesse caso, começa com 0);
2. `depositar(double valor)`, que só aceita valores positivos;
3. `sacar(double valor)`, que **não deixa o saldo ficar negativo** e devolve `boolean` informando se deu certo.

No `main`: crie uma conta com saldo inicial −50, deposite 100, tente sacar 30 e depois 500, e imprima o saldo final.

> [!success]- Resposta (uma solução possível)
> ```java
> public class Conta {
>     String titular;
>     double saldo;
>
>     Conta(String titular, double saldoInicial) {
>         this.titular = titular;
>         if (saldoInicial >= 0) {
>             this.saldo = saldoInicial;
>         } else {
>             this.saldo = 0;            // objeto nasce válido
>         }
>     }
>
>     void depositar(double valor) {
>         if (valor > 0) {
>             saldo = saldo + valor;
>         }
>     }
>
>     boolean sacar(double valor) {
>         if (valor <= 0 || valor > saldo) {
>             return false;              // recusa sem mexer no saldo
>         }
>         saldo = saldo - valor;
>         return true;
>     }
> }
>
> // Principal.java
> Conta c = new Conta("Ana", -50);       // saldo começa 0
> c.depositar(100);                      // 100
> System.out.println(c.sacar(30));       // true  → 70
> System.out.println(c.sacar(500));      // false → continua 70
> System.out.println("Saldo: " + c.saldo); // Saldo: 70.0
> ```
> **Saída:** `true`, `false`, `Saldo: 70.0`. → Seção 5 e [[Faculdade/4_Semestre/POO/POO_Aula04.pdf#page=16|Aula 04 · p. 16]]
