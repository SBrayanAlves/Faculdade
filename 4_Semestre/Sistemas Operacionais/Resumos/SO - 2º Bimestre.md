---
tags:
  - so
  - resumo
  - bimestre-2
bimestre: 2
fonte: claude
atualizado: 2026-11-08
---
# 💻 SO · 2º Bimestre
← [[SO - Painel]] · Anterior: [[SO - 1º Bimestre]]

> [!info] Como usar
> Cada seção termina com um **📄 Fonte**. A aula 12 (E/S e arquivos, 06/11) **não tinha slides disponíveis**: essa parte vem da **apostila**. Seções com **➕** são complemento (fora dos slides). No fim, **simulado de 10 questões** (com cálculos).

> [!danger] Seminário (AI): **13/11** · P2: **27/11** · Substitutiva: **11/12**

## 🗺️ Mapa do bimestre
| Aula | Tema | Onde estudar |
|---|---|---|
| 08 | Memória contígua: partições e swapping | [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 7.pdf\|Aula 7]] · Apostila 7.3-7.8 |
| 09 | Memória não contígua: paginação, segmentação, proteção | [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf\|Aula 8]] |
| 10 | Memória virtual | [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf\|Aula 8]] · Apostila 7.9 |
| 11 | Alocação e substituição de páginas | [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf\|Aula 8]] · Apostila 7.10 |
| 12 | E/S, arquivos, proteção e segurança | Apostila 3.6 e cap. 8 *(sem slides)* |

> [!abstract] O bimestre em uma frase
> O SO toma **3 decisões** sobre a memória: **onde colocar** (First/Best/Worst fit), **o que manter na RAM** (localidade, working set) e **o que tirar** quando ela enche (FIFO, LRU...).
> 📄 [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf#page=20|Aula 8 · p. 20]]

---

## 1. Alocação contígua simples e overlay
- **Contígua simples:** a memória é dividida em **área do SO + área de um único programa** (monotarefa)
  - Sem proteção (o usuário pode acessar a área do SO) · programa menor que a área = **desperdício** · maior = **não executa**
- **Overlay:** divide o programa em **módulos** que **revezam a mesma área** da memória
  - Quem define é o **programador** · a área de overlay tem o tamanho do **maior módulo**
  - Vantagem: "expande" a memória · desvantagem: difícil de implementar

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 7.pdf#page=4|Aula 7 · p. 4-6]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=34|Apostila · p. 34-35]]

---

## 2. Alocação particionada
### Estática (partições fixas, definidas no boot)
| Tipo | Regra |
|---|---|
| **Absoluta** | O programa só roda **na partição pra qual foi compilado** (código absoluto), mesmo com outra livre |
| **Relocável** | Código relocável: roda em **qualquer partição onde caiba** |

- Mudar o tamanho das partições exige **reiniciar** o sistema
- **Proteção:** registradores com os **limites inferior e superior** da partição
- Problema: **fragmentação INTERNA** = sobra espaço **dentro** da partição (programa de 5 KB numa partição de 8 KB desperdiça 3 KB)

### Dinâmica (a partição tem o tamanho do programa)
- Acaba com a fragmentação interna, mas os programas que terminam deixam **buracos espalhados** → **fragmentação EXTERNA**
- Ex: 7 KB livres no total, mas em pedaços de 2 + 3 + 2 KB → um programa de 4 KB **não cabe**
- **Soluções:** (A) **realocar/compactar** as partições ocupadas (caro: CPU, disco, complexidade) · (B) **juntar os espaços adjacentes** quando um programa termina · (C) **estratégias de alocação** ⬇️

> [!tip] Interna × Externa
> **Interna** = desperdício **dentro** do bloco alocado (partição fixa, última página).
> **Externa** = memória livre **picotada entre** os blocos (partição dinâmica, segmentação).

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 7.pdf#page=7|Aula 7 · p. 7-13]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=36|Apostila · p. 36-38]]

---

## 3. Estratégias de alocação ⭐ (cai cálculo)
Espaços livres: **100, 500, 200, 300, 600 KB**. Chega um processo de **212 KB**:

| Estratégia | Regra | Escolhe | Sobra | Efeito |
|---|---|:-:|:-:|---|
| **First-fit** | O **primeiro** que couber | 500 | 288 | Busca rápida; o início acumula fragmentos |
| **Best-fit** | O **menor** que couber | 300 | **88** | Preserva os blocos grandes, mas cria **sobras pequenas e inúteis** |
| **Worst-fit** | O **maior** de todos | 600 | 388 | Sobra ainda utilizável, mas **consome os blocos grandes** |

**Nenhuma vence sempre:** a sequência de pedidos e liberações muda o resultado.

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf#page=3|Aula 8 · p. 3-7]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=39|Apostila · p. 39]]

---

## 4. Compactação e swapping
- **Compactação:** move os processos pra juntar o espaço livre. Custa **cópias de memória e atualização de endereços**
- **Swapping:** move o **processo inteiro** da RAM pro disco (*swap-out*) e de volta (*swap-in*). Libera muita RAM, mas é **muito E/S**: usado com cautela

| | Swapping | Paginação sob demanda |
|---|---|---|
| Unidade movida | **Processo inteiro** | **Uma página** |
| Transferência | Grande | Pequena |
| Uso | Liberar muita RAM | Trazer **só o necessário** |

> A pergunta que leva à memória virtual: **o processo precisa estar inteiro na RAM?**

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf#page=8|Aula 8 · p. 8-10]]

---

## 5. Paginação e segmentação
### Paginação
- O processo é dividido em **páginas** e a RAM em **quadros (frames)**, **do mesmo tamanho fixo**
- As páginas podem ficar em quadros **espalhados**: a ordem virtual **não precisa** coincidir com a física
- A **tabela de páginas** (uma por processo) diz em que quadro está cada página
- ✅ Acaba com a fragmentação externa · ⚠️ **fragmentação interna** (última página) · transparente ao programador
- **Copy-on-Write:** processos compartilham as mesmas páginas até alguém escrever; aí copia

### Segmentação
- Divide em **segmentos lógicos de tamanho variável**: **código, dados, pilha**
- ✅ **Proteção por segmento**, reflete a lógica do programa · ⚠️ **fragmentação externa**

| | Paginação | Segmentação |
|---|---|---|
| Tamanho | **Fixo** | **Variável** |
| Divisão | Física | **Lógica** |
| Fragmentação | **Interna** | **Externa** |
| Visível ao programador | Não | Sim |

➕ **Segmentação paginada:** divide em segmentos lógicos e **cada segmento em páginas**. Junta a proteção da segmentação com a ausência de fragmentação externa da paginação.

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf#page=13|Aula 8 · p. 13-15]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 7.pdf#page=1|Aula 7 · p. 1-2]]

---

## 6. Memória virtual ⭐
> As memórias principal e secundária são combinadas pra dar ao processo a **impressão de ter muito mais memória** do que a RAM real. **Só as partes necessárias ficam carregadas.**

| Conceito | Significado |
|---|---|
| **Espaço virtual × real** | Endereços que o processo pode usar × endereços reais da RAM. Não têm relação direta |
| **Mapeamento** | Tradução virtual → real (feita pela **MMU** com a **tabela de páginas**) |
| **Páginas virtuais / reais** | As reais se chamam **frames** |
| **Page fault** | A página **pertence ao processo, mas não está na RAM**. **Não é erro do programa**: o SO busca no disco |
| **Taxa de paginação** | Page faults por intervalo. Se for alta, o desempenho despenca |

**Vantagens:** programas maiores que a RAM, **mais processos ao mesmo tempo** (só parte de cada um fica residente) e quase nenhuma fragmentação.

### Tratamento do page fault
1. A CPU referencia a página → 2. a tabela indica **presente = 0** → 3. o kernel localiza a página no disco → 4. escolhe um **quadro livre ou uma vítima** → 5. carrega, atualiza a tabela e **reinicia a instrução**
(Enquanto isso, o processo fica em **espera** e outro usa a CPU.)

**Quando não há quadro livre:** **página limpa** (não foi alterada) pode ser descartada · **página suja** (foi alterada) precisa ser **gravada no disco antes**, o que custa mais.

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf#page=11|Aula 8 · p. 11-17]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=40|Apostila · p. 40-41]]

---

## 7. Localidade, working set e thrashing
- **Localidade temporal:** o que foi acessado **agora** tende a ser acessado **de novo logo** (variáveis de um loop)
- **Localidade espacial:** depois de um endereço, tende a acessar os **vizinhos** (percorrer um vetor)
- **Working set:** o conjunto de páginas **ativamente usadas** numa fase da execução
- **Thrashing:** **excesso de page faults**: o sistema passa mais tempo trocando páginas do que executando
  - **Do programa:** código mal escrito (ignora a localidade) ou working set pequeno → reescrever ou aumentar o working set
  - **Do sistema:** mais páginas pedidas do que a RAM aguenta → **aumentar a memória física**

### Políticas da memória virtual
| Política | Opções |
|---|---|
| **Busca** | **Por demanda** (só quando referenciada) × **antecipada** (carrega várias antes) |
| **Alocação** | **Fixa** (mesmo limite pra todos: injusta) × **variável** (limite por processo) |
| **Substituição** | **Local** (só páginas do próprio processo) × **global** (qualquer página da RAM) |
| **Tamanho de página** | Entre 512 B e 128 KB · **menor = mais compartilhamento** de memória e CPU |

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf#page=12|Aula 8 · p. 12]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=42|Apostila · p. 42-43]]

---

## 8. Algoritmos de substituição de páginas ⭐⭐
O objetivo é tirar a página com **menos chance de ser usada em breve**, pra reduzir page faults futuros.

| Algoritmo | Tira a página que... | Observação |
|---|---|---|
| **Ótimo** | Vai demorar **mais** pra ser usada | **Impossível** (exige prever o futuro). Serve de referência |
| **Aleatório** | Qualquer uma | Barato e ineficiente |
| **FIFO** | **Entrou há mais tempo** | Simples, mas pode tirar uma página muito usada. Tem a **anomalia de Belady** (mais quadros → mais faltas) |
| **LRU** | Está **há mais tempo sem uso** | Aproveita a localidade, mas é **caro** registrar todos os acessos |
| **LFU** | Foi **menos vezes** usada | Usa contador |
| **NRU** | **Não foi usada recentemente** | Aproximação barata |
| **Clock** | FIFO circular com **bit de referência** (2ª chance) | Aproximação de LRU usada nos sistemas reais |

### Exemplo do slide: refs **1, 2, 3, 1, 4, 1, 2, 5** com **3 quadros**
**FIFO**
| Ref | 1 | 2 | 3 | 1 | 4 | 1 | 2 | 5 |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| Q1 | 1 | 1 | 1 | 1 | **4** | 4 | 4 | **5** |
| Q2 | | 2 | 2 | 2 | 2 | **1** | 1 | 1 |
| Q3 | | | 3 | 3 | 3 | 3 | **2** | 2 |
| | F | F | F | **H** | F | F | F | F |
→ **7 faltas**

**LRU**
| Ref | 1 | 2 | 3 | 1 | 4 | 1 | 2 | 5 |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| Q1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| Q2 | | 2 | 2 | 2 | **4** | 4 | 4 | **5** |
| Q3 | | | 3 | 3 | 3 | 3 | **2** | 2 |
| | F | F | F | **H** | F | **H** | F | F |
→ **6 faltas**. O LRU ganhou porque **manteve a página 1**, que era muito usada.

> [!tip] Como montar a tabela na prova
> Escreva as referências no topo, uma coluna por referência. Pra cada uma: **está na memória? H (acerto).** Não está? **F (falta)**: se houver quadro vazio, ocupa; senão, aplica a regra (FIFO = a mais antiga **a entrar**; LRU = a mais antiga **a ser usada**).

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf#page=18|Aula 8 · p. 18-20]] · [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=43|Apostila · p. 43-44]]

---

## 9. Gerência de E/S
| Técnica | Como funciona | A CPU fica |
|---|---|---|
| **E/S programada** (*polling*) | A CPU fica **testando o dispositivo** até a operação acabar | ❌ **Ocupada o tempo todo** |
| **Por interrupção** | A CPU inicia a operação e **faz outra coisa**; o dispositivo **avisa** quando termina | ✅ Livre (permitiu a multiprogramação) |
| **DMA** | Um controlador transfere **um bloco inteiro** entre o dispositivo e a memória **sem a CPU** | ✅ Só participa no **início e no fim** |
| **Canal de E/S** | Processador dedicado que gerencia **vários controladores** | ✅ |

➕ **Camadas:** aplicação → system call → subsistema de E/S do SO → **driver** (software específico do dispositivo) → **controlador** (hardware) → dispositivo.

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=14|Apostila · p. 14-16]]

---

## 10. Gerência de arquivos
### Estrutura de diretórios
| Tipo | Como é | Problema |
|---|---|---|
| **Nível único** | Um diretório pra tudo | Não aceita dois arquivos com o mesmo nome |
| **Diretório pessoal** | Um por usuário + um "master" | Sem subpastas |
| **Árvore** (múltiplos níveis) | Pastas dentro de pastas | ✅ **Usado hoje** |

### Sistemas de arquivos
| Sistema | Características |
|---|---|
| **FAT** | MS-DOS/Windows antigo, **listas encadeadas**, partições até **2 GB**, desempenho baixo |
| **FAT32** | Mesma organização, partições até **2 TB** |
| **NTFS** | Windows NT+, **árvore binária**, nomes até 255 caracteres, **proteção, criptografia**, bom desempenho |
| **UNIX** | Hierárquico a partir da raiz; **arquivo = sequência de bytes** (a aplicação interpreta) |

➕ **Métodos de alocação no disco:** **contígua** (blocos seguidos: rápida, mas fragmenta) · **encadeada** (cada bloco aponta pro próximo: a ideia do FAT) · **indexada** (um bloco de índice lista todos os blocos: os i-nodes do UNIX)

### Espaço livre
1. **Mapa de bits:** 1 bit por bloco (0 = livre, 1 = ocupado). Gasta memória proporcional ao disco
2. **Lista encadeada** dos blocos livres
3. **Tabela de blocos livres** (primeiro bloco + quantidade de blocos contíguos livres)

### Proteção de acesso
**Senha** por arquivo · **grupos de usuários** (dono/grupo/outros: o `rwx` do Linux) · **ACL** (lista de quem pode o quê: permissões do NTFS)

📄 **Fonte:** [[Faculdade/4_Semestre/Sistemas Operacionais/Notas_Sistemas_Operacionais.pdf#page=45|Apostila · p. 45-47]]

---

## 💼 No mundo real (e em cyber)
- **PC lento com 50 abas** = **thrashing** (o disco trabalhando sem parar é o sintoma). `pagefile.sys` e o swap do Linux são a memória virtual na prática
- A separação de espaços de endereço pela MMU é o que **impede um processo de ler a memória de outro**. **Meltdown/Spectre** atacaram essa fronteira, e o **ASLR** só existe graças ao endereçamento virtual
- **ACL mal configurada** e `chmod 777` são portas de entrada clássicas em pentest

---

## 📝 Simulado: 2º Bimestre

### Questão 1
Um programa de **5 KB** foi carregado numa **partição fixa de 8 KB**, e os 3 KB restantes ficaram sem uso. Esse desperdício é chamado de:
- a) Fragmentação externa
- b) Fragmentação interna
- c) Thrashing
- d) Page fault
- e) Swapping

> [!success]- Resposta: **B**
> Sobra **dentro** do bloco alocado = **interna**. A externa é a memória livre picotada **entre** os blocos. → Seção 2

### Questão 2
Os espaços livres são **150, 400, 90, 250 e 500 KB**, nessa ordem. Chega um processo de **220 KB**. Qual bloco o **Best-fit** escolhe e quanto sobra?
- a) 400 KB, sobram 180 KB
- b) 250 KB, sobram 30 KB
- c) 500 KB, sobram 280 KB
- d) 150 KB, sobram 70 KB
- e) 90 KB, não sobra nada

> [!success]- Resposta: **B**
> Best-fit = o **menor que couber**: 150 e 90 não cabem; dos que cabem (400, 250, 500), o menor é **250** → sobra **30**.
> First-fit escolheria **400** (o primeiro que cabe) e Worst-fit escolheria **500** (o maior). → Seção 3

### Questão 3
A técnica que divide a memória em **unidades de tamanho fixo**, é **transparente ao programador** e pode gerar **fragmentação interna** é a:
- a) Segmentação
- b) Paginação
- c) Partição dinâmica
- d) Overlay
- e) Alocação contígua simples

> [!success]- Resposta: **B**
> **Paginação**. A segmentação usa tamanhos **variáveis**, é lógica (código/dados/pilha) e gera fragmentação **externa**. → Seção 5

### Questão 4
Sobre o **page fault**, assinale a correta:
- a) É um erro de programação que encerra o processo
- b) Ocorre quando a página referenciada pertence ao processo, mas não está na RAM, e é tratado pelo SO
- c) Ocorre quando a RAM está totalmente vazia
- d) Só acontece em sistemas sem memória virtual
- e) É resolvido pelo programador com overlay

> [!success]- Resposta: **B**
> É um **evento normal** da memória virtual. O SO busca a página no disco e reinicia a instrução. → Seção 6

### Questão 5
O sistema está com uma **taxa altíssima de page faults** e a CPU fica a maior parte do tempo esperando o disco, mesmo com programas bem escritos. Esse fenômeno e a solução adequada são:
- a) Deadlock; reiniciar os processos
- b) Thrashing de sistema; aumentar a memória física
- c) Fragmentação interna; compactar a memória
- d) Starvation; usar aging
- e) Swapping; desligar a memória virtual

> [!success]- Resposta: **B**
> Excesso de page faults = **thrashing**. Se o problema é do **sistema** (mais páginas do que a RAM comporta), a solução é **aumentar a RAM**. → Seção 7

### Questão 6
Um programa **percorre um vetor sequencialmente**, acessando posições vizinhas. Isso ilustra a:
- a) Localidade temporal
- b) Localidade espacial
- c) Fragmentação externa
- d) Anomalia de Belady
- e) Política de alocação fixa

> [!success]- Resposta: **B**
> Acessar **vizinhos** = **espacial**. Temporal é **reacessar o mesmo** endereço logo depois (variável de um loop). → Seção 7

### Questão 7
Referências: **2, 3, 2, 1, 5, 2, 4, 5**, com **3 quadros** inicialmente vazios. Quantas **faltas de página** ocorrem no **FIFO** e no **LRU**, respectivamente?
- a) 6 e 5
- b) 5 e 6
- c) 6 e 6
- d) 7 e 5
- e) 4 e 4

> [!success]- Resposta: **A**
> **FIFO:** 2F · 3F · 2H · 1F → [2,3,1] · 5F (sai 2, a mais antiga) → [3,1,5] · 2F (sai 3) → [1,5,2] · 4F (sai 1) → [5,2,4] · 5H → **6 faltas**
> **LRU:** 2F · 3F · 2H · 1F → [2,3,1] · 5F (sai 3, usada há mais tempo) → [2,1,5] · 2H · 4F (sai 1) → [2,5,4] · 5H → **5 faltas**
> → Seção 8

### Questão 8
Qual sistema de arquivos usa **estrutura em árvore binária** e oferece **permissões (ACL) e criptografia**?
- a) FAT
- b) FAT32
- c) NTFS
- d) Nível único
- e) Diretório pessoal

> [!success]- Resposta: **C**
> **NTFS**. FAT e FAT32 usam listas encadeadas e não têm esse sistema de proteção. → Seção 10

### Questão 9
A técnica de E/S em que um controlador transfere **um bloco inteiro de dados** entre o dispositivo e a memória **sem a participação da CPU**, exceto no início e no fim, é:
- a) E/S programada
- b) E/S por interrupção
- c) DMA
- d) Spool
- e) Buffer

> [!success]- Resposta: **C**
> **DMA** (*Direct Memory Access*). Na E/S programada a CPU fica presa testando o dispositivo; na por interrupção ela é interrompida a cada evento. → Seção 9

### Questão 10 (discursiva: desafio do professor)
Explique **por que o LRU pode ser melhor que o FIFO**, mas é **mais caro de implementar**. Cite também por que o algoritmo **Ótimo** não é usado na prática.

> [!success]- Resposta
> **Melhor:** o LRU usa a **localidade temporal**: mantém as páginas usadas recentemente, que tendem a ser usadas de novo. O FIFO só olha a **ordem de chegada** e pode tirar uma página antiga **mas muito usada** (como a página 1 do exemplo do slide).
> **Mais caro:** o LRU precisa **registrar a ordem de todos os acessos** (um timestamp ou uma pilha atualizada a cada referência), o que tem custo alto de hardware e tempo. Por isso os sistemas reais usam **aproximações**, como o **Clock** e o **NRU**.
> **Ótimo:** exigiria **saber o futuro** (quais páginas serão referenciadas), o que é impossível. Ele só serve como **referência** pra comparar os outros.
> 📄 [[Faculdade/4_Semestre/Sistemas Operacionais/Aula 8.pdf#page=20|Aula 8 · p. 20]]
