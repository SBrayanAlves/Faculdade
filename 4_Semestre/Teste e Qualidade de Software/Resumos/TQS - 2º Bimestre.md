---
tags:
  - tqs
  - resumo
  - bimestre-2
bimestre: 2
fonte: claude
atualizado: 2026-11-08
---
# 🧪 TQS · 2º Bimestre
← [[TQS - Painel]] · Anterior: [[TQS - 1º Bimestre]]

> [!info] Como usar
> Cada seção termina com um **📄 Fonte** que abre o PDF **na página exata**. No fim, um **simulado de 10 questões novas**. Pra treinar mais, use o [[Banco de Questões TQS]] (30 questões dos simulados do Moodle).

> [!danger] P2: **26/11** · Revisão: **19/11** (não falte!)

## 🗺️ Mapa do bimestre
| Data | Tema | Arquivo |
|---|---|---|
| 08/10 | Justificativas de Qualidade | *sem slides na pasta* |
| 15/10 | Normas NBR ISO/IEC 12207 e ISO 9000 | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 7.pdf\|Aula 7]] |
| 22/10 | SPICE (ISO/IEC 15504) e CMM/CMMI | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 8.pdf\|Aula 8]] |
| 29/10 | CMMI na prática, MPS.BR, PSP e TSP | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 9.pdf\|Aula 9]] |
| 05/11 | ISO/IEC 25010 e domínios de aplicação | [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 10.pdf\|Aula 10]] |

> [!abstract] O fio da meada do bimestre
> **Normas** (como organizar) → **modelos de maturidade** (o quão bem a empresa trabalha: SPICE, CMMI, MPS.BR) → **disciplina individual e de equipe** (PSP, TSP) → **qualidade do produto** (ISO 25010).

---

## 1. Por que normas?
Sem normas, cada empresa chama "requisito", "teste" e "aceite" de um jeito → **conflito de contrato** e dependência de "talentos individuais".
Com normas: **linguagem universal**, **responsabilidades claras** (adquirente × fornecedor × desenvolvedor) e **auditabilidade**.
> A norma não engessa: ela cria **confiança técnica** entre contratante e contratado.

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 7.pdf#page=2|Aula 7 · p. 2]]

---

## 2. NBR ISO/IEC 12207: processos do ciclo de vida
Cobre o software **da concepção à descontinuação** (*retirement*), em **3 grupos**:

### 🟦 Fundamentais (Primary): os 5 que fazem o software acontecer
| Processo | Quem / o quê |
|---|---|
| **Aquisição** | O **cliente (adquirente)**: edital, escolha do fornecedor, aceite |
| **Fornecimento** | O **fornecedor**: proposta, contrato, gestão do projeto |
| **Desenvolvimento** | Requisitos, arquitetura, código, testes unitários, integração |
| **Operação** | Rodar em produção, suporte, métricas de falha |
| **Manutenção** | Corretiva, adaptativa, perfectiva/evolutiva, preventiva |

### 🟩 Apoio (Supporting): garantem que ficou bom
**Garantia da Qualidade (SQA)** · **Verificação** · **Validação** · **Gerência de Configuração** · **Auditoria e Revisão Conjunta** · **Resolução de Problemas** · Documentação
- Não geram código, mas dão **qualidade, rastreabilidade e integridade**
- **SQA na 12207:** garantia **independente e objetiva** por meio de auditorias de processo e revisões de produto
- **Gerência de Configuração:** registra a **baseline**, controla mudanças e garante rastreabilidade requisito ↔ código
- **Revisão Conjunta:** reunião formal **adquirente + fornecedor** pra avaliar o status

### 🟨 Organizacionais: a estrutura da empresa
**Gerência** (projetos, prazos) · **Infraestrutura** (servidores, IDEs, CI/CD) · **Melhoria** (métricas, ligação com CMMI/MPS.BR) · **Treinamento**

> [!tip] V&V na 12207
> **Verificação:** os artefatos de uma fase atendem à **fase anterior**? → estática
> **Validação:** o sistema atende à **necessidade real** do cliente? → dinâmica

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 7.pdf#page=3|Aula 7 · p. 3-11]]

---

## 3. Família ISO 9000 e o SGQ
- **SGQ** (Sistema de Gestão da Qualidade): políticas, objetivos e processos pra atingir qualidade
- **ISO 9000** = vocabulário e fundamentos · **ISO 9001** = **a única auditável e certificável**

**Os 7 princípios da ISO 9001:** Foco no **cliente** · **Liderança** · **Engajamento** das pessoas · Abordagem por **processos** · **Melhoria contínua** (PDCA) · Decisão baseada em **evidências** · Gestão de **relacionamentos**

**PDCA no software:**
| Etapa | No software |
|---|---|
| **Plan** | Definir metas de qualidade e planejar os testes |
| **Do** | Codificar, revisar, executar as suítes |
| **Check** | Coletar métricas e avaliar desvios |
| **Act** | Refatorar e ajustar o processo |

**Mentalidade de risco:** agir **antes** de virar falha (ex: risco de memory leak → análise estática obrigatória no pipeline).

**ISO/IEC 90003:** a ISO 9001 foi feita pra indústria ("matéria-prima", "linha de montagem"). A 90003 **traduz a 9001 pro software** e a liga à 12207.

| | ISO 9001 | ISO/IEC 12207 |
|---|---|---|
| Nível | **Organizacional (macro)** | **Operacional (engenharia)** |
| Certifica? | **Sim** | Não: referência técnica e contratual |
| Pergunta | "A empresa tem um SGQ eficaz?" | "Como executar o desenvolvimento e o SQA?" |
| Resumo | **O QUÊ** | **COMO** |

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 7.pdf#page=12|Aula 7 · p. 12-18]]

---

## 4. SPICE (ISO/IEC 15504) ⭐
- *Software Process Improvement and Capability dEtermination*. Hoje modernizado na família **ISO/IEC 33000**
- **Propósito duplo:** melhoria **interna** + avaliar **fornecedores** (licitações)
- 🔑 **Não define processos**: avalia processos de um **modelo de referência externo (PRM)**, como a **ISO 12207**

**Estrutura bidimensional:**
| Dimensão | Pergunta |
|---|---|
| **Processos** | **O QUE** é feito (vem da 12207) |
| **Capacidade** | **QUÃO BEM** é feito (níveis 0 a 5, **processo por processo**) |

**Os 6 níveis de capacidade:**
| Nível | Nome | Ideia | Atributos (PA) |
|:-:|---|---|---|
| 0 | Incompleto | Não atinge o objetivo | — |
| 1 | Executado | Atinge, **sem planejar** | PA 1.1 Execução |
| 2 | Gerenciado | **Planejado, monitorado, ajustado** | PA 2.1 Gerência da execução · PA 2.2 Gerência dos produtos de trabalho |
| 3 | Estabelecido | Segue o **padrão da organização** | PA 3.1 Definição · PA 3.2 Implantação |
| 4 | Previsível | Limites **estatísticos** | PA 4.1 Medição · PA 4.2 Controle |
| 5 | Em Otimização | **Melhoria contínua** | PA 5.1 Inovação · PA 5.2 Otimização |

**Escala NPLF:** **N** 0-15% · **P** >15-50% · **L** >50-85% · **F** >85-100%
**Regra de progressão:** pra atingir o nível N → **F** em todos os atributos dos níveis anteriores **e pelo menos L** nos atributos do nível N.

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 8.pdf#page=3|Aula 8 · p. 3-7]]

---

## 5. CMM → CMMI ⭐
- **CMM** (1987-91): **SEI/Carnegie Mellon**, **Watts Humphrey**, financiado pelo **DoD** (que sofria com fornecedores estourando prazo)
- Vários CMMs separados (SW, SE, IPD, SA) → incompatíveis → **CMMI** (*Integration*) unificou
- Modelos: **CMMI-DEV** (desenvolvimento) · **CMMI-SVC** (serviços) · **CMMI-ACQ** (aquisição)

### Os 5 níveis de maturidade (representação por estágios)
| Nível | Nome | Característica | Foco |
|:-:|---|---|---|
| 1 | **Inicial** | Caótico, depende de **heróis**, **abandona o processo sob pressão** | Sobrevivência |
| 2 | **Gerenciado** | Planejado e controlado **por projeto**; requisitos e configuração geridos | Projeto |
| 3 | **Definido** | **Processo padrão da organização (OSSP)**, adaptado por **tailoring** | Organização |
| 4 | **Gerenciado Quantitativamente** | Metas numéricas + **Controle Estatístico de Processo** | Estatística |
| 5 | **Em Otimização** | **Análise de causa raiz (CAR)** + inovação | Melhoria contínua |

### Estágios × Contínua
| | **Por Estágios** | **Contínua** |
|---|---|---|
| Mede | **Maturidade da organização** (1 a 5) | **Capacidade de cada área** (0 a 3/5) |
| Caminho | **Fixo**: pra ir ao 3, cumpre as PAs do 2 e do 3 | **Livre**: escolhe as PAs (ex: REQM e VER primeiro) |
| Vantagem | **Comparar empresas** (contratos) | **ROI rápido** em problemas específicos |

### 22 Áreas de Processo (v1.3), em 4 categorias
| Categoria | PAs principais |
|---|---|
| **Gerenciamento de Projetos** | PP, PMC, SAM, **REQM**, IPM, RSKM, QPM |
| **Engenharia** | RD, TS, PI, **VER**, **VAL** |
| **Gestão de Processos** | **OPD**, **OPF**, OT, OPP, OPM |
| **Suporte** | **CM**, **PPQA**, MA, DAR, **CAR** |

### SPICE × CMMI
| | SPICE | CMMI |
|---|---|---|
| Tipo | **Norma aberta** (ISO) | **Modelo proprietário** (SEI/ISACA) |
| Representação | Só contínua | **Estágios e contínua** |
| Processos | **Usa PRM externo** (12207) | **Tem PAs próprias** |
| Escala | **NPLF** | Metas genéricas e específicas |

### Tailoring
Adaptar o **processo padrão (OSSP)** ao projeto (tamanho, risco, tecnologia). **Exigido no nível 3.** Muda **a forma**, nunca **a meta de qualidade**. ❌ Pular teste por falta de tempo **não é tailoring**.

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 8.pdf#page=8|Aula 8 · p. 8-17]]

---

## 6. CMMI na prática
| Componente | Tipo | O que é |
|---|---|---|
| **SG / GG** | Obrigatório | Objetivos específicos / genéricos |
| **SP** | Esperado | **O QUE** fazer naquela área (ex: SP 1.1 de REQM: entender os requisitos) |
| **GP** | Esperado | **Institucionalizar**: planejado, monitorado, com recursos, **repetível** (ex: GP 2.2 Planejar o processo) |

**v1.3 × v2.0:** a 2.0 troca "Áreas de **Processo**" por "Áreas de **Prática**" (categorias *Doing, Managing, Enabling, Improving*), **integra nativamente o ágil** e foca em **resultado de negócio**, não só em documentação.

**SCAMPI** (método de avaliação): **A** = oficial, dá o nível (Lead Appraiser) · **B** = diagnóstico antes da oficial · **C** = micro-auditoria

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 9.pdf#page=3|Aula 9 · p. 3-5]]

---

## 7. MPS.BR ⭐
- Criado em **2003** pela **SOFTEX** (apoio MCTI e FINEP), pra **micro, pequenas e médias empresas** brasileiras: **mais barato** que o CMMI
- Alinhado à **12207** e ao **SPICE** (usa os **APs** e a escala **NPLF**)
- **Tríade:** **MR-MPS** (Modelo de **Referência**) · **MA-MPS** (Método de **Avaliação**) · **MN-MPS** (Modelo de **Negócio**)

| Nível | Nome | Adiciona | ≈ CMMI |
|:-:|---|---|:-:|
| **G** | Parcialmente Gerenciado | **Gerência de Projetos + Gerência de Requisitos** | 2 (parcial) |
| **F** | Gerenciado | Configuração, Garantia da Qualidade, Medição, Aquisição | **2** |
| **E** | Parcialmente Definido | Treinamento, Tailoring, Definição do processo | 3 (parcial) |
| **D** | Definido | Projeto/Construção, Integração, Verificação, Validação | 3 (parcial) |
| **C** | Definido Otimizado | Riscos, Reutilização | **3** |
| **B** | Gerenciado Quantitativamente | Gestão quantitativa | **4** |
| **A** | Em Otimização | Causa raiz, Inovação | **5** |

> [!tip] Macete
> **Começa no G, termina no A.** Âncoras: **F = 2 · C = 3 · B = 4 · A = 5**

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 9.pdf#page=6|Aula 9 · p. 6-9]]

---

## 8. PSP e TSP (Watts Humphrey)
### PSP: Personal Software Process
**Premissa:** a qualidade depende da **disciplina e da medição do próprio trabalho de cada dev**.
| Nível | Foco | Prática chave |
|---|---|---|
| **PSP0 / 0.1** | Base | **Time Log** (tempo por fase) + **Defect Log** (cada defeito, tipo e fase onde nasceu) + padrão de código |
| **PSP1 / 1.1** | Planejamento | **PROBE**: proxies (nº de métodos, telas) + **regressão** sobre o **histórico pessoal** |
| **PSP2 / 2.1** | Qualidade | **Revisão pessoal de código e de projeto com checklists**, antes de compilar (−80% de bugs) |
| **PSP3** | Escala | Sistemas grandes quebrados em módulos feitos com PSP2 |

Métricas: LOC, tempo por fase, defeitos/KLOC, *phase yield* e **DRE pessoal**.

### TSP: Team Software Process
Time **autogerenciado** com **5 papéis**: **Líder** (time e gerência) · **Planejamento** (estimativas, cronograma) · **Qualidade/Processo** (metas de qualidade, **DRE**, adesão ao processo) · **Desenvolvimento** (arquitetura) · **Suporte** (ferramentas, SCM)
**TSP Launch:** workshop de **3 a 4 dias** em que **o próprio time** entende o negócio, estima (com dados do PSP), faz o plano de qualidade e **assina o compromisso**.

### A sinergia
**CMMI/MPS.BR (top-down)**: governança e políticas → **TSP**: o time executa → **PSP (bottom-up)**: a disciplina do dev. Sem PSP/TSP, o CMMI vira **"processo no papel"**.

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 9.pdf#page=10|Aula 9 · p. 10-16]]

---

## 9. ISO/IEC 25010: qualidade do PRODUTO ⭐
- Família **SQuaRE (ISO 25000)**. A **25010:2011 substituiu a ISO 9126** (que tinha 6 características)
- 🔑 Mudança principal: **Segurança e Compatibilidade viraram características próprias**, e o modelo foi separado em **Produto** e **Uso**
- Níveis de avaliação: **interna** (código: McCabe, Halstead) → **externa** (testes) → **em uso** (usuário real)

### As 8 características do produto
| # | Característica | Subcaracterísticas |
|:-:|---|---|
| 1 | **Adequação Funcional** | Completude · Correção · Adequação (aptidão) |
| 2 | **Eficiência de Desempenho** | **Comportamento Temporal** (tempo de resposta) · **Utilização de Recursos** (CPU/RAM) · **Capacidade** (limite de carga) |
| 3 | **Compatibilidade** | **Coexistência** (dividir ambiente) · **Interoperabilidade** (trocar dados) |
| 4 | **Usabilidade** | Reconhecimento de adequação · Aprendizagem · Operabilidade · Proteção contra erros do usuário · **Acessibilidade** |
| 5 | **Confiabilidade** | **Maturidade** (falha pouco) · **Disponibilidade** · **Tolerância a Falhas** (segue funcionando) · **Recuperabilidade** (volta depois) |
| 6 | **Segurança** | **Confidencialidade** · **Integridade** · **Não Repúdio** · **Responsabilização** · **Autenticidade** |
| 7 | **Manutenibilidade** | **Modularidade** · Reusabilidade · **Analisabilidade** · Modificabilidade · Testabilidade |
| 8 | **Portabilidade** | **Adaptabilidade** · Instalabilidade · **Substituibilidade** |

> [!tip] Macete das 8: **AECU-CSMP**
> **A**dequação · **E**ficiência · **C**ompatibilidade · **U**sabilidade · **C**onfiabilidade · **S**egurança · **M**anutenibilidade · **P**ortabilidade

> [!warning] Pares que confundem
> - **Tolerância a falhas** (continua rodando *durante* a falha) × **Recuperabilidade** (volta e recupera *depois*)
> - **Coexistência** (rodar junto) × **Interoperabilidade** (trocar informação)
> - **Modularidade** (mudar sem efeito colateral) × **Analisabilidade** (fácil diagnosticar)
> - **Adaptabilidade** (outro ambiente sem mudar código) × **Substituibilidade** (substituir outro software)
> - **Não repúdio** (não dá pra negar) × **Autenticidade** (quem é) × **Integridade** (não foi alterado) × **Responsabilização** (rastrear quem fez)

**Trade-offs:** Eficiência × Manutenibilidade · **Segurança × Usabilidade** (várias autenticações no app do banco) · Portabilidade × Eficiência · Confiabilidade × recursos · Adequação × Usabilidade

### Qualidade em Uso (5)
**Eficácia** (atingiu a meta?) · **Eficiência** (com quanto tempo/esforço?) · **Satisfação** (confiança, conforto) · **Isenção de Risco** · **Cobertura de Contexto**
> Ex: médico preenche sem erro (**alta eficácia**), mas demora e se estressa (**baixa eficiência e satisfação**).

**Métricas (ISO 25023):** cobertura de requisitos (aprovados ÷ especificados × 100) · tempo de resposta (JMeter) · disponibilidade · vulnerabilidades por KLOC · McCabe médio · falhas de instalação

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 10.pdf#page=2|Aula 10 · p. 2-11]]

---

## 10. Domínios de aplicação
| Domínio | Prioridades |
|---|---|
| **Corporativo** (ERP, CRM, banco) | Adequação funcional, **Segurança**, Manutenibilidade |
| **Embarcado / Tempo real** | **Eficiência de desempenho**, **Confiabilidade** |
| **Crítico** (safety-critical) | **Isenção de risco**, confiabilidade extrema, rastreabilidade |
| **Web / Mobile** | **Usabilidade**, Portabilidade, Eficiência sob carga |

- **Tempo real:** **Hard** (perder o prazo é fatal: airbag, ABS) × **Soft** (degrada: streaming) · **MISRA C** (proíbe `malloc`, recursão) · **WCET** (pior tempo) · testes **HIL**
- **Normas críticas:** **ISO 26262** (carros, ASIL A-D) · **IEC 62304** (médico, classes A-C) · **DO-178C** (aviação, níveis A-E). Os níveis máximos exigem **100% de MC/DC**
- **Web/Mobile:** fragmentação de dispositivos, **WCAG** (acessibilidade), **OWASP Top 10**
- **IA** (ISO 42001, 24029): **não-determinismo**, **viés** nos dados (ISO 25012), explicabilidade, ataques adversariais

📄 **Fonte:** [[Faculdade/4_Semestre/Teste e Qualidade de Software/Aula 10.pdf#page=12|Aula 10 · p. 12-16]]

---

## 📝 Simulado: 2º Bimestre (questões novas)

### Questão 1
Na ISO/IEC 12207, a **Gerência de Configuração** pertence a qual grupo de processos?
- a) Fundamentais
- b) De Apoio
- c) Organizacionais
- d) De Aquisição
- e) De Fornecimento

> [!success]- Resposta: **B**
> Gerência de Configuração não gera código, garante **rastreabilidade e integridade**: é um processo **de Apoio**, junto com SQA, Verificação, Validação, Auditoria e Resolução de Problemas. → Seção 2

### Questão 2
Uma empresa quer obter um **certificado auditável** do seu Sistema de Gestão da Qualidade. A norma adequada é:
- a) ISO/IEC 12207
- b) ISO/IEC 15504
- c) ISO 9001
- d) ISO/IEC 25010
- e) ISO/IEC 90003

> [!success]- Resposta: **C**
> A **ISO 9001** é a única da família 9000 que é **certificável**. A 12207 é referência técnica, a 90003 é um guia de interpretação, a 15504 avalia processos e a 25010 trata do produto. → Seção 3

### Questão 3
Após executar as suítes de teste, a equipe **coleta métricas de defeitos e compara com as metas definidas**. Essa atividade corresponde a qual etapa do PDCA?
- a) Plan
- b) Do
- c) Check
- d) Act
- e) Nenhuma, pois PDCA não se aplica a software

> [!success]- Resposta: **C**
> **Check** = coletar métricas e avaliar desvios. O **Act** viria depois: corrigir e ajustar o processo. → Seção 3

### Questão 4
Numa avaliação SPICE de um processo, os resultados foram: **PA 1.1 = F**; **PA 2.1 = F, PA 2.2 = F**; **PA 3.1 = L, PA 3.2 = P**. Qual nível de capacidade esse processo atinge?
- a) Nível 0
- b) Nível 1
- c) Nível 2
- d) Nível 3
- e) Nível 4

> [!success]- Resposta: **C**
> Pra o nível 3, os atributos do nível 3 precisariam ser **pelo menos L**, mas o PA 3.2 = **P**. Pro nível 2: os níveis anteriores estão em **F** e os atributos do nível 2 também estão em F (≥ L) ✅. → **Nível 2 (Gerenciado)**. → Seção 4

### Questão 5
Uma empresa quer melhorar **apenas Gerenciamento de Requisitos (REQM) e Verificação (VER)**, que são seus maiores problemas, buscando um **retorno rápido**. Qual representação do CMMI é a mais adequada?
- a) Por Estágios, pois mede a organização inteira
- b) Contínua, pois permite escolher áreas de processo específicas
- c) Por Estágios, pois exige começar pelo nível 5
- d) Nenhuma, pois o CMMI não permite melhoria parcial
- e) SCAMPI classe A

> [!success]- Resposta: **B**
> A **Contínua** permite escolher as PAs e melhorar a capacidade de cada uma, com ROI mais rápido. A por Estágios tem caminho fixo e serve pra comparar empresas. → Seção 5

### Questão 6
Uma fábrica de software foi avaliada no **nível C do MPS.BR**. Sua maturidade equivale a qual nível do CMMI-DEV?
- a) Nível 1
- b) Nível 2
- c) Nível 3
- d) Nível 4
- e) Nível 5

> [!success]- Resposta: **C**
> Âncoras: **F = 2 · C = 3 · B = 4 · A = 5**. → Seção 7

### Questão 7
Um desenvolvedor passa a **registrar o tempo gasto em cada fase** e **cada defeito que comete**, com o tipo e a fase em que foi injetado. Ele está praticando qual nível do PSP?
- a) PSP0
- b) PSP1
- c) PSP2
- d) PSP3
- e) TSP Launch

> [!success]- Resposta: **A**
> **PSP0** é a base: **Time Log + Defect Log**. O PSP1 traz o PROBE (estimativa) e o PSP2 traz a revisão com checklists. → Seção 8

### Questão 8
O sistema de internet banking precisa **continuar funcionando mesmo quando um dos servidores do datacenter falha**. Pela ISO/IEC 25010, isso corresponde a:
- a) Recuperabilidade
- b) Tolerância a Falhas
- c) Disponibilidade
- d) Coexistência
- e) Maturidade

> [!success]- Resposta: **B**
> Seguir funcionando **durante** a falha = **Tolerância a Falhas** (Confiabilidade). **Recuperabilidade** seria voltar e restaurar os dados **depois** da queda. → Seção 9

### Questão 9 (discursiva)
Classifique cada exigência de um **app bancário** na **característica e subcaracterística** da ISO/IEC 25010:
a) O app deve responder em até 2 segundos.
b) Pessoas com deficiência visual devem conseguir usá-lo com leitor de tela.
c) Alterar o módulo de pagamentos não pode afetar o módulo de cadastro.
d) O app deve rodar em Android e iOS sem reescrever o código.
e) Nenhum cliente pode negar uma transferência que realizou.

> [!success]- Resposta
> | | Característica | Subcaracterística |
> |---|---|---|
> | a | Eficiência de Desempenho | Comportamento Temporal |
> | b | Usabilidade | Acessibilidade |
> | c | Manutenibilidade | Modularidade |
> | d | Portabilidade | Adaptabilidade |
> | e | Segurança | Não Repúdio |
> → Seção 9

### Questão 10 (discursiva)
Uma empresa brasileira de software com **15 funcionários** quer melhorar seus processos e **participar de licitações**, com orçamento limitado.
a) Você recomendaria CMMI ou MPS.BR? Por quê?
b) Qual seria o primeiro nível e quais processos ele exige?
c) Como o PSP poderia complementar essa iniciativa?

> [!success]- Resposta
> **a)** **MPS.BR**: foi criado pra micro, pequenas e médias empresas brasileiras, tem **custo de avaliação bem menor** que o CMMI, é aceito em licitações e tem **7 níveis**, que permitem avançar em degraus menores.
> **b)** **Nível G (Parcialmente Gerenciado)**, que exige **Gerência de Projetos** e **Gerência de Requisitos**.
> **c)** O MPS.BR organiza a empresa de cima pra baixo (**top-down**). O **PSP** dá a **disciplina individual** de baixo pra cima (**bottom-up**): registro de tempo e defeitos, estimativas com PROBE e revisões com checklist. Assim o processo **não fica só no papel**.
> → Seções 7 e 8
