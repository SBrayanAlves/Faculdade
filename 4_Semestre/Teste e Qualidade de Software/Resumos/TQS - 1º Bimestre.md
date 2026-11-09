---
tags:
  - tqs
  - resumo
  - bimestre-1
bimestre: 1
fonte: claude
atualizado: 2026-11-08
---
# 🧪 TQS · 1º Bimestre
← [[TQS - Painel]] · Próximo: [[TQS - 2º Bimestre]]

> [!info] Como usar
> Cada seção termina com um **📄 Fonte**: clicando, o PDF do professor abre **na página exata**. No fim tem um **simulado de 10 questões** no formato da P1 (8 objetivas + 2 discursivas).

## 🗺️ Mapa do bimestre
| Aula | Data | Tema | Arquivo |
|---|---|---|---|
| 1 | 13/08 | Apresentação e o que é qualidade | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 1.pdf\|Aula 1]] |
| 2 | 20/08 | SQA, Processo × Produto, V&V, Modelo U | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 2.pdf\|Aula 2]] |
| 3 | 27/08 | Revisões e Inspeção de Fagan | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 3.pdf\|Aula 3]] |
| 4 | 03/09 | Garantia estatística, confiabilidade, Seis Sigma, GQM | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 4.pdf\|Aula 4]] |
| 5 | 17/09 | Métricas de código e de teste | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 5.pdf\|Aula 5]] |
| 6 | — | **Revisão para a P1** (o melhor resumo do professor) | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf\|Aula 6]] |

> [!tip] Formato da P1 (vale pra ter noção da P2 também)
> **10 objetivas estilo ENADE** (0,42 cada = 4,2) + **2 discursivas** (1,4 cada = 2,8), sem consulta. As discursivas pedem **desenho de grafo (CFG), cálculos e tabelas de teste**.
> 📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=3|Aula 6 · p. 3]]

---

## 1. O que é qualidade de software
Qualidade **depende do ponto de vista** (usuário, desenvolvedor, gerente). Por isso a engenharia troca a ideia vaga de "excelência" por **conformidade com requisitos e ausência de falhas**.

| Autor | Definição (essência) |
|---|---|
| **ISO 9000:2005** | Grau em que um conjunto de características **satisfaz os requisitos** |
| **Pressman** | Conformidade com requisitos **explícitos**, **padrões** documentados e **características implícitas** |
| **Sanders** | Grau de satisfação das necessidades dos clientes |
| **ISO 9126** | Totalidade de características que satisfazem necessidades declaradas ou implícitas |

**Os 4 aspectos que definem qualidade:**
1. **Requisitos:** falta de conformidade = falta de qualidade
2. **Padrões:** sem critério de como programar = falta de qualidade
3. **Requisitos implícitos:** o que ninguém escreveu, mas todo mundo espera (ex: manutenibilidade)
4. **Gerencial:** prazo e orçamento

> [!warning] Ideia central da matéria
> **Qualidade não se coloca no final.** Cada fase passa a sua qualidade (ou a falta dela) pra próxima, então ela precisa ser um objetivo **o tempo todo**.

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 1.pdf#page=14|Aula 1 · p. 14-19]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 4.pdf#page=3|Aula 4 · p. 3]]

---

## 2. SQA × QC: Processo × Produto
Software não se desgasta como hardware. Ele falha por **erro lógico escondido** que só aparece numa combinação específica de condições. Por isso existem duas frentes:

| | **SQA** (Garantia da Qualidade) | **QC** (Controle da Qualidade) |
|---|---|---|
| Foco | **PROCESSO** ("como fazemos") | **PRODUTO** ("o que entregamos") |
| Caráter | **Preventivo**, proativo | **Detectivo**, corretivo |
| Objetivo | **Evitar** que o defeito nasça | **Achar e corrigir** antes do cliente |
| Quem | Analistas de qualidade, gerentes, auditores | Testadores, devs, usuários beta |
| Exemplos | Padrões de código, auditoria ISO 9001, **análise estática no CI/CD** | **Testes** unitários, de estresse, de regressão |
| Resultado | Processo maduro e repetível | Binário testado, sem bugs críticos |

> [!warning] Pegadinha clássica
> **SonarQube/Cppcheck no pipeline = SQA**, porque é uma **regra de processo** que impede o código ruim de entrar. **Teste em homologação = QC**, porque **executa o produto** pra achar falha.

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 2.pdf#page=4|Aula 2 · p. 4-6]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=4|Aula 6 · p. 4-5]]

---

## 3. Verificação × Validação e o Modelo U
| | **Verificação** | **Validação** |
|---|---|---|
| Pergunta | "Estamos construindo o produto **corretamente**?" | "Estamos construindo o produto **correto**?" |
| Executa o software? | **NÃO**: análise **estática** de documentos e código | **SIM**: exige o software **rodando** |
| Exemplo | Revisar o documento de requisitos com os médicos da UTI | Teste de aceitação com a equipe de enfermagem |

### Modelo U (Zanin, adaptado de Bartié)
Cada fase de projeto tem uma atividade de teste correspondente:
```
 DESCIDA = VERIFICAÇÃO (estática)          SUBIDA = VALIDAÇÃO (dinâmica)
 1. Modelo de Negócio  ──────────────────  4. Disponibilização (produção)
 2. Requisitos         ──────────────────  3. Sistema
 3. Análise/Modelagem  ──────────────────  2. Integração
 4. Implementação      ──────────────────  1. Unidade
```

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 2.pdf#page=7|Aula 2 · p. 7-10]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=6|Aula 6 · p. 6-7]]

---

## 4. Pilares de SQA, auditoria e plano de testes
**3 pilares de SQA** (integrados ao ciclo de engenharia, 1 pra 1):
1. **Garantias da Qualidade:** definir padrões e checklists *antes*
2. **Análise das Garantias:** vistorias e métricas pra ver se o padrão é seguido *na prática*
3. **Métodos de Garantia:** técnicas **estáticas** (revisões) e **dinâmicas** (testes)

**Papéis do analista de SQA:** ajudar a definir processos, criar programas de medição (ex: McCabe, densidade de defeitos), achar fraquezas, engajar a alta gerência e **auditar de forma independente** se o QC funciona.

**ISO 9001 / ISO/IEC 90003:** a qualidade **não é fixada pela ISO**, é definida pela própria empresa. Os documentos que dizem como fazer cada atividade são os **procedimentos**. Recomenda-se **auditoria interna preventiva** antes da oficial.
**Fluxo da auditoria:** Reunião de **Abertura** → **Auditoria dos processos** (documentos, amostras, entrevistas) → Reunião de **Conclusão** (conformidades, não-conformidades e recomendações).

**Plano de Testes (ISTQB / ISO 29119-3):** documento vivo do QC, com 7 itens:
1. Escopo · 2. Critérios de **entrada** · 3. Critérios de **saída** · 4. Abordagem e ferramentas · 5. Recursos e equipe · 6. Cronograma · 7. Gerenciamento de riscos

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 2.pdf#page=11|Aula 2 · p. 11-16]]

---

## 5. Revisões de Software e a Inspeção de Fagan
**Por que revisar antes de compilar?** Um bug de requisito que chega à produção custa **até 100× mais** pra corrigir. Revisão é um **filtro estático**: acha defeito antes até de existir teste.

| Tipo | Como é |
|---|---|
| **Informal** | Conversa, sem checklist nem métrica |
| **Peer Review** | Dois devs revisam o código um do outro, alternando |
| **Walkthrough** | O **próprio autor conduz** 3 a 5 pessoas pelos casos de uso (~2h) |
| **Inspeção de Fagan** | **Rigor máximo**: moderador neutro, papéis fixos, formulário e métricas |

### Walkthrough × Fagan (o professor pediu pra saber!)
| | Walkthrough | Inspeção de Fagan |
|---|---|---|
| Quem conduz | **O autor** | **Moderador neutro** |
| Papéis formais | Não | **5 papéis** |
| Documentação | Informal | **Formulário de defeitos obrigatório** |
| Objetivo | Trocar conhecimento, validar rápido | **Achar defeitos e coletar métricas** |
| Treinamento | Não exige | **Exige** |

**Os 5 papéis de Fagan:**
| Papel | Faz o quê |
|---|---|
| **Moderador** | Lidera, controla o tempo, avalia o **artefato, nunca a pessoa** |
| **Autor** | Escreveu; apresenta o contexto e faz o retrabalho |
| **Leitor** | Lê o código **linha por linha** |
| **Inspetor** | Caça defeitos **com checklist** |
| **Secretário** | Registra linha, tipo e severidade. **Não propõe solução** |

**As 6 etapas:** **Planejamento → Apresentação → Preparação** (individual) **→ Reunião** (acha bugs, **não resolve**) **→ Retrabalho → Acompanhamento** (moderador confere as correções)

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 3.pdf#page=3|Aula 3 · p. 3-7]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=8|Aula 6 · p. 8]]

### Métricas de revisão
- **Esforços (pessoas-hora):** de **preparação** (individual), de **avaliação** (a reunião) e de **reformulação** (o autor corrigindo)
- **TAS** = tamanho do artefato (LOC, páginas, diagramas)
- **Erros totais** = erros **secundários** (baratos: warning, formatação) + erros **graves** (segurança, fluxo, cálculo)
- **Densidade de erros = Erros totais ÷ TAS**

### Revisão por amostragem
Sem tempo de revisar 100%? Revisa uma **amostra** e projeta:
> **Erros estimados = Erros na amostra ÷ Fração amostrada**
> Ex: revisou 20% e achou 12 → 12 ÷ 0,2 = **60 erros estimados**

⚠️ Se a amostra não for representativa, a projeção **erra feio** (pra mais ou pra menos).

> [!example] O exemplo do slide: checksum UDP em C
> `for (int i = 0; i <= pkt->data_len; i++)` → **off-by-one** (o `<=` passa do fim do vetor: buffer overflow)
> `return soma / pkt->data_len;` → **divisão por zero** se o pacote vier sem dados

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 3.pdf#page=8|Aula 3 · p. 8-13]]

---

## 6. Erro → Defeito → Falha (IEEE 610.12)
| Termo | O que é | Exemplo |
|---|---|---|
| **Erro** (engano) | **Ação humana** errada | Dev cansado entende o protocolo errado |
| **Defeito** (bug, *fault*) | O problema **escrito** no artefato | A linha `i <= tam` gravada no `.c` |
| **Falha** (*failure*) | O sistema **se comportando errado** em execução | Roteador trava com Segmentation Fault |

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=9|Aula 6 · p. 9]]

---

## 7. Métricas de SQA: o que medir e como medir bem
**Metas de Pressman & Maxim (4 categorias):**
| Categoria | Métricas |
|---|---|
| **Requisitos** | **Ambiguidade** (termos vagos: "rápido", "amigável") · **Completude** (nº de **TBD/TBA**) · **Volatilidade** (mudanças por tempo) · **Rastreabilidade** (requisito sem código) |
| **Projeto** | Integridade da arquitetura · completude de componentes · complexidade de interface (cliques por tarefa) |
| **Código** | Complexidade (McCabe) · manutenibilidade · reusabilidade |
| **Eficiência do QC** | Esforço real × estimado · erros achados por hora de revisão · defeitos achados antes da produção |

**Uma boa métrica (Koscianski & Soares):** **Significância** (útil e acionável) · **Custo-benefício** · **Repetibilidade** (mesmo resultado medindo de novo) · **Reprodutibilidade** (mesmo resultado com **outro avaliador**) · **Objetividade**

**Critérios de Weyuker** (pra métricas de complexidade):
- **Permutação:** reorganizar instruções que mudam o fluxo deve mudar a métrica
- **Implementação:** mesma saída com lógica diferente → complexidade diferente
- **Monotonicidade:** o todo **nunca** é menos complexo que uma das partes
- **Interação:** juntar dois programas pode dar **mais** que a soma das complexidades

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 4.pdf#page=4|Aula 4 · p. 4-7]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=15|Aula 6 · p. 15]]

---

## 8. Manutenção, confiabilidade e severidade
**4 tipos de manutenção:**
| Tipo | Quando | Exemplo |
|---|---|---|
| **Corretiva** | Reativa: corrige falha | Travamento por ponteiro nulo |
| **Adaptativa** | O **ambiente** mudou | Novo SO, nova API |
| **Incremental/Perfectiva** | Nova regra ou tela | Funcionalidade não prevista |
| **Preventiva** | Proativa | Refatorar código complexo antes de quebrar |

**Confiabilidade** = probabilidade de funcionar corretamente, nas condições especificadas, por um tempo determinado.
- **MTTF** (*Mean Time To Failure*): tempo médio operando **até a falha**
- **MTBF** (*Mean Time Between Failures*): tempo médio **entre** uma falha e a próxima (≈ MTTF + tempo de reparo)
- **Indisponibilidade (%)** = tempo parado ÷ tempo total × 100
  - 1 falha de 7h num dia de 24h → 7 ÷ 24 = **29,17%**
  - 1h parado a cada 7h operando → 1 ÷ (7 + 1) = **12,5%**

**Severidade das falhas (Koscianski & Soares):** **Catastrófica** (perda total) · **Crítica** (módulo vital ou vazamento de dados) · **Marginal** (funciona com limitação) · **Pequena** (cosmética)

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 4.pdf#page=8|Aula 4 · p. 8-10]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=10|Aula 6 · p. 10]]

---

## 9. Garantia estatística: Seis Sigma, GQM e Pareto
### Seis Sigma
Meta: **3,4 defeitos por milhão de oportunidades** (DPMO), atacando a **causa raiz** com dados.
| Ciclo | Etapas | Quando usar |
|---|---|---|
| **DMAIC** | Definir, Medir, Analisar, **Melhorar**, Controlar | **Aperfeiçoar** um processo que **já existe** |
| **DMADV** | Definir, Medir, Analisar, **Projetar**, Verificar | **Criar** um processo **novo, do zero** |

### GQM (Goal-Question-Metric, Basili 1994)
**Nenhuma métrica sem objetivo de negócio.** Abordagem **top-down**:
1. **Goal** (conceitual): objeto, propósito, foco de qualidade, ponto de vista e contexto
2. **Question** (operacional): perguntas quantitativas pra saber se a meta está sendo atingida
3. **Metric** (quantitativo): o dado exato que responde cada pergunta

> [!example] Exemplo do professor: memory leak em firmware C
> **G:** reduzir vazamentos de memória no firmware, do ponto de vista do SQA, no teste de integração
> **Q1:** quantos `malloc` ficam sem `free` por execução? → **M1:** alertas do Cppcheck por KLOC
> **Q2:** quantos MB de heap ficam retidos? → **M2:** MB medidos pelo Valgrind

### Pareto (Juran, 80/20)
~80% dos defeitos vêm de ~20% das causas. Ex: de 500 defeitos, requisitos ambíguos (210) + falhas de comunicação de escopo (165) = 375 = **75%**. **Decisão certa:** concentrar o investimento nessas duas causas (prototipagem, validação de escopo, checklists de requisitos).

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 4.pdf#page=11|Aula 4 · p. 11-16]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=11|Aula 6 · p. 11-14]]

---

## 10. Métricas de código-fonte
### LOC / KLOC
Contagem de linhas (KLOC = LOC ÷ 1000). Serve pra **estimar tamanho** e **normalizar** outras métricas (defeitos/KLOC). **Limites:** depende da linguagem e do estilo, e **não mede qualidade**.

### Halstead (1977): o código como operadores e operandos
| Símbolo | Significado |
|---|---|
| **n₁ / n₂** | operadores / operandos **distintos** |
| **N₁ / N₂** | **total de ocorrências** de operadores / operandos |
| **Vocabulário** n = n₁ + n₂ | **Comprimento** N = N₁ + N₂ |
| **Volume** V = N × log₂ n | **Dificuldade** D = (n₁ ÷ 2) × (N₂ ÷ n₂) |
| **Esforço** E = D × V | |

> Operadores: `+ - = if while return ; () {}`... · Operandos: variáveis, constantes, nomes de função

### Complexidade Ciclomática de McCabe: V(G) ⭐
Conta os **caminhos independentes** do grafo de fluxo (CFG). **V(G) = número mínimo de casos de teste** pra cobrir esses caminhos. As 3 fórmulas **têm que dar o mesmo valor**:
1. **V(G) = E − N + 2** (arestas − nós + 2)
2. **V(G) = P + 1** (P = nós predicados: `if`, `while`, `for`, `case`)
3. **V(G) = nº de regiões** do grafo (contando a externa)

| V(G) | Risco | Ação |
|:-:|---|---|
| **1 a 10** | Baixo | Ok ✅ (**regra prática: máximo 10**) |
| 11 a 20 | Moderado | Atenção nas revisões |
| 21 a 50 | Alto | Refatorar |
| > 50 | Não testável | Dividir obrigatoriamente |

> [!example]- Exemplo do slide: `verificar_sinal` (2 ifs aninhados)
> Predicados: `potencia > -70` e `ruido < -90` → **V(G) = 2 + 1 = 3** → 3 casos de teste (sinal excelente, razoável e sem sinal).

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 5.pdf#page=4|Aula 5 · p. 4-10]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=16|Aula 6 · p. 16-17]]

---

## 11. Métricas de teste
| Cobertura | Mede |
|---|---|
| **De instrução** (*statement*) | % das **linhas** executadas |
| **De ramo/decisão** (*branch*) | Se cada condição foi testada **verdadeira E falsa** → ramos exercitados ÷ (2 × predicados) |
| **De caminhos** (*path*) | Todas as combinações → **explosão combinatória** com loops. Solução: usar os caminhos independentes do McCabe |

> [!warning] Regra de ouro
> **100% de instrução NÃO garante 100% de ramo.** `if (pressao > 100) { abrir(); }` testado só com 120 executa todas as linhas (100% instrução), mas o lado falso nunca rodou (**50% de ramo**).

**DRE** (*Defect Removal Efficiency*) = **E ÷ (E + D)** · E = achados **antes** da entrega, D = achados **pelos clientes**
- **100%** = ideal · **abaixo de 80%** = processo de teste ineficiente

**Densidade de defeitos** = defeitos ÷ KLOC

**Ferramentas (C):** **Cppcheck** (análise estática) · **Lizard/PMD** (McCabe e LOC) · **Gcov** (cobertura no GCC) · **Lcov** (relatório HTML do Gcov)

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 5.pdf#page=11|Aula 5 · p. 11-19]] · [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=18|Aula 6 · p. 18-20]]

---

## 🧮 Formulário (cola mental)
| Assunto | Fórmula |
|---|---|
| Indisponibilidade | tempo parado ÷ tempo total × 100 |
| Densidade de erros (revisão) | erros totais ÷ TAS |
| Erros estimados | erros na amostra ÷ fração amostrada |
| Halstead | n = n₁ + n₂ · N = N₁ + N₂ · V = N·log₂n · D = (n₁/2)·(N₂/n₂) · E = D·V |
| McCabe | E − N + 2 = P + 1 = regiões |
| Cobertura de ramo | ramos exercitados ÷ (2 × predicados) |
| DRE | E ÷ (E + D) |
| Densidade de defeitos | defeitos ÷ KLOC |

> [!warning] Confira as fórmulas com o slide
> As fórmulas eram imagens nos PDFs. As acima são as da literatura. Compare com o checklist do professor: 📄 [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 6 - Revisao.pdf#page=21|Aula 6 · p. 21]]

---

## 📝 Simulado: 1º Bimestre (formato P1)

### Questão 1
Uma equipe configura o **SonarQube no pipeline de CI/CD** para bloquear pull requests com vulnerabilidades. Ao mesmo tempo, testadores executam **testes de estresse em homologação**. Essas atividades são, respectivamente:
- a) QC e SQA
- b) SQA e QC
- c) Ambas SQA
- d) Ambas QC
- e) Validação e Verificação

> [!success]- Resposta: **B**
> Análise estática no pipeline é **regra de processo, preventiva** (SQA). Teste de estresse **executa o produto** pra achar falhas (QC). → Seção 2

### Questão 2
A equipe revisa, junto com os médicos, o **documento de requisitos** de uma bomba de infusão de UTI, sem executar nenhum código. Essa atividade é:
- a) Validação de sistema
- b) Verificação
- c) Teste de aceitação
- d) Controle de Qualidade (QC)
- e) Validação de unidade

> [!success]- Resposta: **B**
> Não executa nada, só analisa um documento: **Verificação** ("construindo corretamente?"). É exatamente o exemplo do slide. → Seção 3

### Questão 3
O que diferencia o **Walkthrough** da **Inspeção de Fagan**?
- a) O Walkthrough é conduzido pelo próprio autor e não exige papéis formais nem treinamento
- b) O Walkthrough exige moderador neutro e formulário de defeitos
- c) A Inspeção de Fagan dispensa documentação
- d) Na Inspeção de Fagan, quem conduz a reunião é o autor do código
- e) O Walkthrough coleta métricas estatísticas obrigatórias

> [!success]- Resposta: **A**
> Fagan = **moderador neutro**, 5 papéis, formulário, métricas e treinamento. Walkthrough = **o autor conduz**, informal. → Seção 5

### Questão 4
Um programador cansado escreve `for (i = 0; i <= tam; i++)`. Em produção, o roteador trava com Segmentation Fault. Classifique: (1) a ação do programador, (2) a linha gravada no `.c`, (3) o travamento.
- a) Defeito, erro, falha
- b) Erro, defeito, falha
- c) Falha, defeito, erro
- d) Erro, falha, defeito
- e) Defeito, falha, erro

> [!success]- Resposta: **B**
> **Erro** = ação humana · **Defeito** = o que ficou escrito · **Falha** = o comportamento errado em execução. → Seção 6

### Questão 5
Uma empresa vai **criar uma nova divisão do zero**, sem nenhum processo de SQA definido. Qual ciclo Seis Sigma é o adequado?
- a) DMAIC
- b) DMADV
- c) PDCA
- d) GQM
- e) Pareto

> [!success]- Resposta: **B**
> **DMADV** (Definir, Medir, Analisar, **Projetar**, Verificar) cria processo novo. **DMAIC** melhora um processo que já existe. → Seção 9

### Questão 6
A função `if (pressao > 100) { abrir_valvula(); }` foi testada **apenas com pressao = 120**. A cobertura de **instrução** e a de **ramo** são, respectivamente:
- a) 100% e 100%
- b) 50% e 100%
- c) 100% e 50%
- d) 50% e 50%
- e) 100% e 0%

> [!success]- Resposta: **C**
> Todas as linhas rodaram (100% de instrução), mas só o lado **verdadeiro** do `if` foi testado: 1 de 2 ramos = **50%**. → Seção 11

### Questão 7
Um servidor ficou **3 horas fora do ar** num período de **24 horas**. A indisponibilidade foi de:
- a) 8%
- b) 12,5%
- c) 3%
- d) 87,5%
- e) 21%

> [!success]- Resposta: **B**
> 3 ÷ 24 × 100 = **12,5%**. Cuidado: 87,5% é a **disponibilidade**. → Seção 8

### Questão 8
Num trecho de código há **6 operadores distintos**, **5 operandos distintos**, **14 ocorrências de operadores** e **10 ocorrências de operandos**. O **vocabulário (n)** e o **comprimento (N)** de Halstead são:
- a) n = 24 e N = 11
- b) n = 11 e N = 24
- c) n = 30 e N = 35
- d) n = 11 e N = 11
- e) n = 20 e N = 15

> [!success]- Resposta: **B**
> n = n₁ + n₂ = 6 + 5 = **11** (os distintos) · N = N₁ + N₂ = 14 + 10 = **24** (as ocorrências). Bônus: V = 24 × log₂(11) ≈ 24 × 3,46 ≈ **83**. → Seção 10

### Questão 9 (discursiva)
```c
int verificar_acesso(int nivel, int bloqueado) {
    if (bloqueado == 1) {
        return -1;
    }
    if (nivel >= 5) {
        return 2;
    } else if (nivel >= 2) {
        return 1;
    }
    return 0;
}
```
a) Calcule a complexidade ciclomática V(G) por **duas** fórmulas.
b) Classifique o risco.
c) Monte a **suíte mínima** de casos de teste (entrada e saída esperada).

> [!success]- Resposta
> **a)** Predicados: `bloqueado == 1`, `nivel >= 5`, `nivel >= 2` → **V(G) = 3 + 1 = 4**.
> Pelo grafo: nós = (1) if bloqueado, (2) return −1, (3) if nivel≥5, (4) return 2, (5) if nivel≥2, (6) return 1, (7) return 0, (8) saída → **N = 8**. Arestas: 1→2, 1→3, 2→8, 3→4, 3→5, 4→8, 5→6, 5→7, 6→8, 7→8 → **E = 10**. **V(G) = 10 − 8 + 2 = 4** ✅
> **b)** 4 está entre 1 e 10: **baixo risco**, código simples.
> **c)** 4 casos:
> | # | Entrada | Saída |
> |:-:|---|:-:|
> | T1 | bloqueado = 1 (qualquer nível) | −1 |
> | T2 | bloqueado = 0, nivel = 7 | 2 |
> | T3 | bloqueado = 0, nivel = 3 | 1 |
> | T4 | bloqueado = 0, nivel = 0 | 0 |
> → Seção 10

### Questão 10 (discursiva)
Um sistema bancário de **80 KLOC** teve **360 defeitos** encontrados pelo QA antes da liberação e **40 defeitos** reportados por clientes nos 3 primeiros meses.
a) Calcule a DRE.
b) Calcule a densidade de defeitos pós-lançamento.
c) O processo de testes é eficiente? Justifique.

> [!success]- Resposta
> **a)** DRE = 360 ÷ (360 + 40) = 360 ÷ 400 = **90%**
> **b)** 40 ÷ 80 KLOC = **0,5 defeito/KLOC**
> **c)** **Sim.** A DRE ficou acima de 80% (o limite de ineficiência do slide): 9 em cada 10 defeitos foram pegos antes do cliente. Não é 100% (o ideal), então ainda há espaço pra melhorar, por exemplo com revisões formais (Fagan) nas áreas onde os 40 defeitos apareceram.
> → Seção 11
