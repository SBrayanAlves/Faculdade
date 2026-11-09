---
tags:
  - so
  - resumo
  - bimestre-1
bimestre: 1
fonte: claude
atualizado: 2026-11-08
---
# 💻 SO · 1º Bimestre
← [[SO - Painel]] · Próximo: [[SO - 2º Bimestre]]

> [!info] Como usar
> Cada seção termina com um **📄 Fonte**. A principal fonte é a **apostila do professor** ([[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf|Notas_Sistemas_Operacionais]]), porque as **Aulas 1, 2, 3 e 5 são PDFs escaneados** (só imagem) e o conteúdo delas está na apostila. Seções com **➕** são complemento (fora dos slides). No fim, **simulado de 10 questões**.

## 🗺️ Mapa do bimestre
| Aula | Tema | Onde estudar |
|---|---|---|
| 01-02 | Conceito, evolução e tipos de SO | Apostila cap. 1-3 · Aulas 1-2 (escaneadas) |
| 03 | Processos e threads | Apostila cap. 5 · Aula 3 (escaneada) |
| 04-05 | Escalonamento de CPU, multiprocessadores, tempo real | [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 4.pdf\|Aula 4]] · Apostila cap. 6 |
| 06 | Gerência de memória: endereços lógico e físico | [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 6.pdf\|Aula 6]] |

> A pergunta que o professor quer que você saiba responder: *"diante de lentidão, travamento ou disputa por recursos, qual mecanismo do SO está envolvido?"*

---

## 1. O que é um SO
O SO fica **entre o hardware e as aplicações** e tem dois papéis:
1. **Facilitar o uso** (esconde os detalhes do hardware: você salva um arquivo, não escreve em setores do disco)
2. **Gerenciar recursos** (CPU, memória, disco, E/S) entre vários programas, de forma **organizada e protegida**

Sem o SO como mediador, qualquer aplicação poderia acessar todo o hardware e toda a memória: **sem proteção e sem ordem**.

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=3|Apostila · p. 3]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Plano de Ensino.pdf|Plano de Ensino]]

---

## 2. Tipos de sistemas operacionais
| Tipo | Ideia | Exemplo de uso |
|---|---|---|
| **Monoprogramável / monotarefa** | **Um programa por vez**, com todos os recursos. CPU fica ociosa esperando E/S | Primeiros PCs |
| **Multiprogramável / multitarefa** | **Vários programas na memória**, a CPU alterna entre eles | Todos os SOs modernos |
| ↳ **Batch** (lote) | Jobs em sequência, **sem interação** | Processamento noturno |
| ↳ **Tempo compartilhado** | Fatias de tempo (*time-slice*) pra vários usuários **interativos** | Terminais, PCs |
| ↳ **Tempo real** | **Cumprir prazos** (não é "ser rápido") | Controle industrial, freios |
| **Múltiplos processadores** | Mais de uma CPU | Servidores |
| ↳ **Fortemente acoplados** | CPUs **compartilham a memória**, um único SO | Multicore (SMP) |
| ↳ **Fracamente acoplados** | Cada um com **sua memória e seu SO**, ligados em rede | Sistemas de rede e **distribuídos** |

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=4|Apostila · p. 4-9]]

---

## 3. Conceitos dos sistemas multiprogramáveis
### Interrupção × Exceção (cai!)
| | **Interrupção** | **Exceção** |
|---|---|---|
| Origem | **Evento EXTERNO** ao programa | **Instrução do PRÓPRIO programa** |
| Depende da instrução? | Não | Sim |
| Exemplos | Fim de E/S, teclado, **fim da fatia de tempo** | **Divisão por zero**, **overflow** |
| Tratamento | **Sempre pelo SO** | Melhor tratada **pelo próprio programa** (senão, término anormal) |

**Mecanismo:** ao fim de **cada instrução**, a CPU verifica interrupções → **salva o contexto** (registradores) → identifica a origem → executa a **rotina de tratamento** → **restaura o contexto** e o programa continua.
> A interrupção é o que **permitiu a concorrência**: é a base da multiprogramação.

### Outras técnicas
| Técnica | O que é |
|---|---|
| **Buffer** | Área na **memória principal** que compensa a diferença de velocidade entre CPU e E/S, mantendo os dois ocupados |
| **Spool** | Área **em disco** que simula um dispositivo lento. O clássico é a **fila de impressão** |
| **Reentrância** | **Uma única cópia** de um código (editor, compilador) **compartilhada** por vários usuários, cada um com seus dados |
| **Proteção** | Com recursos compartilhados, o SO precisa garantir integridade e isolamento entre usuários e programas |

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=11|Apostila · p. 11-14]]

---

## 4. Estrutura do SO
### Modos de acesso e system calls
- **Instruções privilegiadas** podem comprometer o sistema (ex: acessar o disco direto) → só o SO executa
- **Modo usuário:** só instruções não privilegiadas · **Modo kernel (supervisor):** todas as instruções
- A aplicação pede um serviço ao SO por uma **system call**, que **muda o processador pra modo kernel**. Ao terminar, volta ao modo usuário

### Arquiteturas do núcleo
| Arquitetura | Como é | Prós | Contras |
|---|---|---|---|
| **Monolítica** | Um **único programa enorme**, módulos interagem livremente | **Rápida**, simples de implementar | Difícil de manter e evoluir |
| **Em camadas** | Níveis sobrepostos; cada camada só usa a de baixo; as internas são mais privilegiadas | **Isolamento**, manutenção fácil | **Desempenho** (muitas trocas de modo) |
| **Microkernel** | **Núcleo mínimo**; serviços (arquivos, memória...) viram **processos servidores** que conversam por **mensagens** (cliente × servidor) | Modular, falha de um serviço não derruba tudo | Overhead das mensagens |

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=19|Apostila · p. 19-23]]

---

## 5. Processos ⭐
> **Processo = programa em execução + tudo que o SO precisa saber sobre ele.** O processador só executa instruções; quem sabe "de quem" é cada instrução é o SO.

### As 3 partes de um processo
| Parte | Contém |
|---|---|
| **Contexto de software** | **Identificação** (quem criou) · **Quotas** (limites: arquivos abertos, memória, CPU) · **Privilégios** (prioridade) |
| **Contexto de hardware** | Conteúdo dos **registradores** da CPU. Salvá-lo e carregar o de outro processo = **troca de contexto** |
| **Espaço de endereçamento** | Área de memória com as instruções e os dados, **protegida** dos outros processos |

Tudo isso fica no **BCP/PCB** (Bloco de Controle do Processo), numa área de memória do SO.

### Estados e transições
```
Criação ──► PRONTO ◄──────────────┐
              │  ▲                 │
     escalonado│  │fatia de tempo   │ fim da E/S
              ▼  │ ou prioridade    │
           EXECUÇÃO ───pede E/S──► ESPERA (bloqueado)
              │
              ▼
            Saída
```
| Apostila | Slides | |
|---|---|---|
| Criação · Pronto · Execução · **Espera** · Saída | Novo · Pronto · Executando · **Bloqueado** · Terminado | Mesma coisa, nomes diferentes |

- **Só 1 processo em execução por CPU**; vários podem estar prontos ou em espera
- **Pronto ≠ Executando:** pronto é estar na fila esperando a CPU
- **Espera → Execução direto NÃO existe**: depois da E/S, o processo volta pra **fila de pronto**

### CPU-bound × I/O-bound
- **CPU-bound:** usa muita CPU, gasta a fatia inteira (cálculo científico, simulação)
- **I/O-bound:** vive esperando E/S, usa a CPU por pouco tempo (**banco de dados**, consultas a disco)

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=24|Apostila · p. 24-28]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 4.pdf#page=2|Aula 4 · p. 2]]

### ➕ Threads
- **Thread** = um fluxo de execução **dentro** de um processo. Várias threads do mesmo processo **compartilham o espaço de endereçamento** (memória, arquivos), mas cada uma tem seus **registradores e sua pilha**
- Criar e trocar thread é **mais barato** que processo. Ex: um navegador com uma thread pra baixar e outra pra desenhar a tela
- **Modelos:** **N:1** (várias threads de usuário em 1 do kernel: se uma bloqueia, todas param) · **1:1** (cada thread de usuário é uma do kernel: o usado no Windows e Linux) · **N:M** (híbrido)

---

## 6. Escalonamento de CPU ⭐⭐
> Vários processos **prontos** e uma CPU só: **quem executa primeiro?** A política de escalonamento decide.

- **Escalonador** = **decide** quem vai · **Dispatcher** = **executa** a decisão (troca de contexto e entrega a CPU)
- **Não preemptivo:** o processo só sai da CPU se **terminar ou pedir E/S**
- **Preemptivo:** o SO **pode interromper** (por tempo ou por prioridade) e devolvê-lo à fila de pronto

### Critérios
| Critério | Significado | Objetivo |
|---|---|---|
| Utilização da CPU | % de tempo ocupada (normal: 30 a 90%) | ⬆️ Maximizar |
| Throughput | Processos concluídos por tempo | ⬆️ Maximizar |
| **Turnaround** | Da **criação até o fim** | ⬇️ Minimizar |
| **Espera** | Tempo **na fila de pronto** | ⬇️ Minimizar |
| **Resposta** | Da requisição até a **primeira resposta** | ⬇️ Minimizar |

**Não existe algoritmo que otimize tudo:** lote valoriza throughput; interativo valoriza resposta.

### Algoritmos
| Algoritmo | Tipo | Regra | Problema |
|---|---|---|---|
| **FIFO / FCFS** | Não preemptivo | Ordem de chegada | **Efeito comboio**: um longo atrasa todos os curtos |
| **SJF** | Não preemptivo | **Menor rajada** primeiro | Precisa **estimar** a rajada; **starvation** dos longos |
| **Cooperativo** | Não preemptivo | O **processo libera** a CPU voluntariamente | Se um não liberar, os outros não rodam (Windows antigo) |
| **SRTF** | Preemptivo | Menor **tempo restante** (reavalia sempre) | Starvation |
| **Circular / Round Robin** | Preemptivo (**por tempo**) | FIFO com **quantum** (fatia de tempo) | Quantum pequeno = muita troca de contexto; grande = vira FIFO |
| **Prioridade** | Preemptivo (**por prioridade**) | Maior prioridade primeiro | **Starvation** → solução: **aging** |
| **Circular com prioridades** | Preemptivo | Fatia de tempo + prioridade | Usado no **Windows** |
| **Múltiplas filas** | Preemptivo | Várias filas de pronto; **a fila** tem a prioridade | O processo volta sempre à mesma fila |
| **Múltiplas filas com realimentação** | Preemptivo | O processo **muda de fila** conforme o comportamento | Mais complexo, mas generalista |

> [!tip] Starvation e aging
> **Starvation (inanição):** um processo pronto é **preterido indefinidamente**. **Aging (envelhecimento):** aumenta a prioridade **aos poucos**, conforme o tempo de espera.

> [!warning] Convenção de prioridade
> A apostila diz que **na maioria dos sistemas, número menor = prioridade maior**. O slide alerta que **varia**: leia o enunciado!

### Exemplos da apostila (A, B e C chegam juntos)
| Algoritmo | Dados | Ordem e término |
|---|---|---|
| **FIFO** | A = 10, B = 8, C = 9 | A (0-10) → B (10-18) → C (18-27) |
| **SJF** | os mesmos | **B** (0-8) → **C** (8-17) → **A** (17-27) |
| **Circular** (quantum 2) | A = 10, B = 6, C = 3 | Alterna de 2 em 2; **C termina em 11** |
| **Prioridade** (menor = maior) | A = 10 (p2), B = 4 (p1), C = 3 (p3) | **B** (0-4) → **A** (4-14) → **C** (14-17) |

### Exemplo do slide: P1 (0, 8), P2 (1, 4), P3 (2, 9), P4 (3, 5) *(chegada, rajada)*
**Espera = término − chegada − rajada**
| Algoritmo | Ordem | Espera média |
|---|---|:-:|
| **FCFS** | P1 → P2 → P3 → P4 | (0 + 7 + 10 + 18) ÷ 4 = **8,75** |
| **SJF** | P1 → P2 → P4 → P3 | (0 + 7 + 9 + 15) ÷ 4 = **7,75** |
| **SRTF** | P1 (0-1) → P2 (1-5) → P4 (5-10) → P1 (10-17) → P3 (17-26) | (9 + 0 + 15 + 2) ÷ 4 = **6,5** |
| **RR (q = 4)** | P1 → P2 → P3 → P4 → P1 → P3 → P4 → P3 | (12 + 3 + 15 + 17) ÷ 4 = **11,75** |

O RR tem a pior espera média, mas a **melhor resposta** (todos começam rápido): ideal pra sistemas interativos.

### Multiprocessadores e tempo real
- **Multiprocessador:** escolher **qual processo e qual CPU** · fila global × fila por CPU · **balanceamento de carga** · **afinidade** (manter o processo na mesma CPU aproveita a **cache quente**)
- **Tempo real = cumprir prazos.** **Hard** (perder é inaceitável: marca-passo, ABS, avião) × **Soft** (degrada: vídeo, jogos) · algoritmos **RMS** (*Rate Monotonic*) e **EDF** (*Earliest Deadline First*)

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 4.pdf#page=4|Aula 4 · p. 4-17]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=29|Apostila · p. 29-33]]

---

## 7. Gerência de memória: o começo
**Computador digital = 3 unidades funcionais:** CPU (ULA, unidade de controle, registradores) · **memória principal** · dispositivos de E/S.

**Registradores da memória:**
- **MAR** (*Memory Address Register*): guarda o **endereço**, ligado ao barramento de endereços. Com **n bits → no máximo 2ⁿ células**
- **MBR/MDR** (*Memory Buffer/Data Register*): guarda o **dado** lido ou a gravar, ligado ao barramento de dados

| Leitura | Escrita |
|---|---|
| 1. CPU põe o endereço no **MAR** | 1. CPU põe o endereço no **MAR** |
| 2. Sinal de leitura | 2. CPU põe o dado no **MBR** |
| 3. O conteúdo vai pro **MBR** | 3. Sinal de gravação |
| 4. Do MBR pra um registrador da CPU | 4. O dado do MBR vai pra célula |

**Hierarquia de memória:** registradores → cache → RAM → disco. Objetivo: **diminuir a diferença de velocidade** entre CPU e memória.

**Endereço lógico × físico:**
- **Lógico (virtual):** o que o **processo enxerga**, gerado pela CPU. Pode ser **maior** que a memória física
- **Físico:** a posição **real** na RAM
- A **MMU** (*Memory Management Unit*) é o **hardware** que traduz lógico → físico **em tempo de execução**. Ex: lógico 120 + base 4000 = físico **4120**
- Pra executar um programa do disco, ele precisa ser **carregado na memória principal** (com PCB e áreas de código, dados e pilha)

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 6.pdf#page=1|Aula 6 · p. 1-12]]

---

## 📝 Simulado: 1º Bimestre

### Questão 1
Durante a execução, o programa tenta uma **divisão por zero**. Esse evento é:
- a) Uma interrupção de hardware
- b) Uma exceção
- c) Uma system call
- d) Um spool
- e) Uma troca de contexto voluntária

> [!success]- Resposta: **B**
> Evento causado **por uma instrução do próprio programa** = **exceção**. Interrupção vem de evento **externo** (ex: fim de E/S). → Seção 3

### Questão 2
Uma aplicação precisa **ler um arquivo no disco**. Como isso acontece num SO com modos de acesso?
- a) A aplicação executa diretamente as instruções de acesso ao disco
- b) A aplicação faz uma system call, que muda o processador para o modo kernel e o SO executa a operação
- c) A aplicação muda sozinha para o modo kernel
- d) O disco envia os dados direto para a aplicação, sem o SO
- e) A aplicação usa reentrância

> [!success]- Resposta: **B**
> Acessar o disco é **instrução privilegiada**: só o SO executa, e a aplicação pede por **system call**. → Seção 4

### Questão 3
Uma arquitetura de SO com **núcleo mínimo**, em que serviços como gerência de arquivos e de memória rodam como **processos servidores** que se comunicam por **mensagens**, é chamada de:
- a) Monolítica
- b) Em camadas
- c) Microkernel
- d) Batch
- e) Fortemente acoplada

> [!success]- Resposta: **C**
> **Microkernel** (cliente × servidor). A monolítica é um programa único e enorme; a em camadas empilha níveis. → Seção 4

### Questão 4
Um processo em **execução** solicita a **leitura de um arquivo** em disco. Ele passa para o estado:
- a) Pronto
- b) Criação
- c) Espera (bloqueado)
- d) Saída
- e) Continua em execução

> [!success]- Resposta: **C**
> Pedir E/S = **Execução → Espera**. Quando a leitura terminar, ele vai pra **Pronto** (não volta direto pra execução). → Seção 5

### Questão 5
Qual transição de estado **NÃO** acontece diretamente?
- a) Pronto → Execução
- b) Execução → Pronto
- c) Execução → Espera
- d) Espera → Execução
- e) Espera → Pronto

> [!success]- Resposta: **D**
> Depois da E/S, o processo **volta pra fila de pronto** e disputa a CPU de novo. → Seção 5

### Questão 6
Um sistema de **banco de dados** que passa a maior parte do tempo fazendo consultas e atualizações em disco tem processos do tipo:
- a) CPU-bound
- b) I/O-bound
- c) Tempo real hard
- d) Reentrantes
- e) Batch

> [!success]- Resposta: **B**
> Mais E/S do que CPU = **I/O-bound** (é o exemplo da própria apostila). → Seção 5

### Questão 7
Os processos **A (10 u.t.)**, **B (8 u.t.)** e **C (9 u.t.)** chegam juntos. Qual o **tempo médio de espera no SJF**?
- a) 9,33
- b) 8,33
- c) 7,00
- d) 17,33
- e) 0

> [!success]- Resposta: **B**
> SJF: **B** (0-8) → **C** (8-17) → **A** (17-27). Esperas: B = 0, C = 8, A = 17 → 25 ÷ 3 = **8,33**.
> No FIFO (A, B, C) seria (0 + 10 + 18) ÷ 3 = **9,33**, a alternativa-pegadinha. → Seção 6

### Questão 8
Processos **A (10 u.t., prioridade 2)**, **B (4 u.t., prioridade 1)** e **C (3 u.t., prioridade 3)**. Considerando que **número menor = prioridade maior**, a ordem de execução é:
- a) A, B, C
- b) C, A, B
- c) B, A, C
- d) C, B, A
- e) B, C, A

> [!success]- Resposta: **C**
> Prioridade 1 (B) → 2 (A) → 3 (C). Términos: B em 4, A em 14, C em 17. → Seção 6

### Questão 9
No **escalonamento circular** com **quantum = 2**, os processos **A (10 u.t.)**, **B (6 u.t.)** e **C (3 u.t.)** chegam nessa ordem. Em que instante **C termina**?
- a) 6
- b) 9
- c) 11
- d) 17
- e) 19

> [!success]- Resposta: **C**
> A (0-2) → B (2-4) → C (4-6, falta 1) → A (6-8) → B (8-10) → **C (10-11) ✅** → A (11-13) → B (13-15, termina) → A (15-19, termina). → Seção 6

### Questão 10
Se o **MAR** de um computador tem **16 bits**, quantas células de memória ele consegue endereçar no máximo?
- a) 16
- b) 256
- c) 32.768
- d) 65.536
- e) 1.048.576

> [!success]- Resposta: **D**
> Com **n bits → 2ⁿ células**: 2¹⁶ = **65.536**. → Seção 7
