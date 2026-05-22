# Modelagem da Construção Civil com Redes de Petri Coloridas (CPN)

## Introdução

As **Redes de Petri Coloridas (Colored Petri Nets — CPN)** constituem uma extensão formal das Redes de Petri clássicas voltada para modelagem de sistemas concorrentes, distribuídos, sincronizados e temporalmente dependentes. Enquanto que nas **Redes de Petri ordinárias os tokens são indistinguíveis**,  as **CPN introduzem tipagem forte, variáveis, expressões, listas, estruturas compostas e temporização**, permitindo representar sistemas reais de elevada complexidade sem gerar explosão estrutural da rede.

Na construção civil, isso é particularmente importante porque **a execução de uma obra envolve simultaneamente precedência lógica, compartilhamento de equipes, sincronização operacional, concorrência entre frentes de trabalho, propagação temporal e restrições de recursos**. Assim, a CPN deixa de representar apenas um fluxo abstrato de atividades e passa a representar dinamicamente o estado operacional da obra.

O modelo utilizado nesta aula implementa uma **integração entre Redes de Petri Coloridas** e o **Método do Caminho Crítico (CPM — Critical Path Method)**, permitindo representar dependências entre atividades, propagação temporal automática, sincronização entre fluxos paralelos, compartilhamento de equipes, cálculo dinâmico do caminho crítico e execução concorrente de atividades.

A CPN utilizada no exemplo de aplicação utilizado está no arquivo [GPCrev*.cpn](https://raw.githubusercontent.com/UniRobotica/Petri-nets/refs/heads/main/Exemplos/GPCrev9_Aula.cpn) disponível no repositório deste curso.
Este modelo está relacionado ao exemplo de PERT/CPM utilizado em [gerenciamento de projetos](https://gpc.unirobotica.com.br/notebooks/dominios/planejamento/pertcpm-construcao-civil.html).

---

# Redes de Petri Coloridas (CPN)

As CPN adicionam uma camada semântica importante às Redes de Petri tradicionais. Em vez de tokens (marcas) equivalentes e sem significado interno, os **tokens passam a transportar informação estruturada**. Dessa forma, o estado do sistema deixa de estar exclusivamente representado na topologia da rede e passa a ser encapsulado nos próprios tokens.

Isso reduz significativamente a complexidade estrutural do modelo. Em problemas reais, como **planejamento de obras**, uma Rede de Petri clássica exigiria uma quantidade muito grande de lugares e transições para representar diferentes estados operacionais. Nas CPN, grande parte dessa lógica é abstraída através de tipos, variáveis e funções.

O modelo [GPCrev*.cpn](https://raw.githubusercontent.com/UniRobotica/Petri-nets/refs/heads/main/Exemplos/GPCrev9_Aula.cpn) não representa apenas o fluxo e precedências entre atividades como no tradicional CPM. Ele representa simultaneamente estado da obra, equipes alocadas, duração das atividades, tempo acumulado, sincronização operacional, propagação temporal e restrições de recursos. Assim, a rede torna-se um simulador discreto de execução da obra.

---

# Estrutura Semântica dos Tokens

No modelo utilizado, o token representa uma entidade operacional completa da obra. O estado do projeto é transportado dentro do próprio token através da estrutura:

```sml
(id, ativ, eqp_list, dur, t)
```

Essa estrutura representa simultaneamente:

| Campo | Significado |
|---|---|
| `id` | identificador da instância do projeto |
| `ativ` | atividade atual |
| `eqp_list` | equipes alocadas |
| `dur` | duração planejada |
| `t` | instante temporal acumulado |

Nessa abordagem, o token deixa de ser apenas uma marca abstrata e passa a representar efetivamente o estado operacional da obra.
Isso transforma a CPN em um modelo temporal, workflow executável, simulador de eventos discretos e representação dinâmica do CPM.

---

# Declarações do Modelo

No CPN IDE, as declarações são realizadas na aba `Declarations`. Essas declarações definem os tipos, variáveis, listas, funções e estruturas compostas utilizadas na rede. O modelo [GPCrev*.cpn](https://raw.githubusercontent.com/UniRobotica/Petri-nets/refs/heads/main/Exemplos/GPCrev9_Aula.cpn) utiliza declarações relativamente avançadas para representar a lógica da construção civil.

---

## Conjunto de Atividades

```sml
colset ATIV =
with INICIO | A | B | C | D | E | E1 | F | G | H | I | J | K | L | M | N | FIM;
```

Essa declaração define um conjunto enumerado de estados possíveis da obra. Cada valor representa uma etapa do projeto.

| Código | Atividade |
|---|---|
| `INICIO` | início do projeto |
| `A` | escavação |
| `B` | fundação |
| `C` | paredes |
| `D` | telhado |
| `E` | encanamento exterior |
| `E1` | refinamento/subetapa hidráulica |
| `F` | encanamento interior |
| `G` | muros |
| `H` | pintura exterior |
| `I` | instalação elétrica |
| `J` | divisórias |
| `K` | piso |
| `L` | pintura interior |
| `M` | acabamento exterior |
| `N` | acabamento interior |
| `FIM` | conclusão da obra |

Um aspecto importante é que a atividade não está representada apenas estruturalmente na rede. O estado da atividade viaja dentro do token. Isso reduz redundâncias estruturais e permite criar modelos muito mais compactos e escaláveis.

---

## Modelagem das Equipes

```sml
colset EQUIPE =
with NSA | ENG | MESTRE | PEDR | ENC | PINTOR | CARP | ELET;
```

Essa declaração representa os recursos humanos da obra.

| Código | Equipe |
|---|---|
| `NSA` | nenhuma equipe associada |
| `ENG` | engenheiro |
| `MESTRE` | mestre de obras |
| `PEDR` | pedreiro |
| `ENC` | encanador |
| `PINTOR` | pintor |
| `CARP` | carpinteiro |
| `ELET` | eletricista |

Essa parte do modelo introduz restrições operacionais. **Em modelos simplificados de precedência de um CPM, normalmente apenas as dependências lógicas são representadas. No entanto, em uma obra real, uma atividade depende não apenas da conclusão da predecessora, mas também da disponibilidade das equipes necessárias.** Assim, a o modelo CPN passa a representar simultaneamente precedência lógica, disponibilidade operacional, compartilhamento de recursos, troca de equipes e sincronização entre frentes de trabalho.

---

# Estruturas Compostas e Produto Cartesiano

O núcleo semântico do modelo é definido por:

```sml
colset PROJ = product ID * ATIV * EQP_LIST * DUR * TEMPO;
```

Essa declaração define o tipo principal dos tokens do sistema.

Formalmente:

```text
PROJ = ID × ATIV × EQP_LIST × DUR × TEMPO
```

Cada token passa a representar simultaneamente:

- identidade do projeto;
- estado atual da execução;
- equipes alocadas;
- duração da atividade;
- instante temporal acumulado.

Essa estrutura é fundamental para compreender por que o modelo implementa uma versão dinâmica do CPM.
O token deixa de representar apenas “uma marca na rede” e passa a representar uma entidade operacional completa da obra.

---

# Listas de Equipes

O modelo também utiliza listas de equipes, por exemplo:

```sml
[ENG,MESTRE,PEDR,PEDR,PEDR]
```

Essa estrutura representa explicitamente a composição operacional necessária para determinada atividade.

Por exemplo:

```sml
(id,A,[ENG,MESTRE,PEDR,PEDR,PEDR],2,t)
```

representa:

- atividade de escavação;
- participação de engenheiro;
- mestre de obras;
- três pedreiros;
- duração igual a 2 unidades de tempo;
- início no instante `t`.

Esse nível de detalhamento aproxima o modelo da realidade operacional da construção civil, permitindo analisar restrições de recursos, compartilhamento de equipes e impactos operacionais de atrasos.

---

# Estrutura Temporal e CPM

O modelo implementa explicitamente conceitos do Método do Caminho Crítico (CPM). O CPM tradicional calcula:

- Early Start (ES);
- Early Finish (EF);
- Late Start (LS);
- Late Finish (LF);
- folgas;
- caminho crítico.

No **Método do Caminho Crítimo (CPM) clássico, através do avanço direto da rede calcula-se os tempos mais cedo**. **O avanço reverso calcula os tempos mais tarde.** O caminho crítico corresponde às atividades cuja folga é zero.
No **modelo CPN, essa propagação temporal é realizada dinamicamente** através dos próprios tokens e funções temporais.

---

# Declaração das Durações

```sml
var durA,durB,durC,durD,durE,durE1,durF,durG,
durH,durI,durJ,durK,durL,durM,durN: DUR;
```

As durações foram parametrizadas para permitir:

- simulação de cenários;
- análise de atrasos;
- alteração de produtividade;
- análise de sensibilidade;
- recalculação automática do caminho crítico.

Isso é extremamente importante porque o modelo não fica rigidamente preso a uma única configuração temporal.

---

# Função `maxDur`

```sml
fun maxDur(a:int, b:int) =
if a > b then a else b;
```

Essa função implementa o conceito central do CPM relacionado à propagação dos tempos mais cedo.

Quando uma atividade possui múltiplas predecessoras, seu início mais cedo depende do maior término entre todas as predecessoras.

Formalmente:

```text
ES(j) = max(EF predecessoras)
```

Como:

```text
EF = ES + duração
```

a função `maxDur` passa a representar a lógica de seleção do maior caminho temporal.

---

# Função `max_sum_time`

```sml
fun max_sum_time [(t1, d1), (t2, d2)] =
if t1 + d1 >= t2 + d2 then t1 + d1 else t2 + d2;
```

Essa função implementa explicitamente o cálculo do término mais cedo entre dois fluxos concorrentes.

Ela calcula:

```text
max(t1+d1, t2+d2)
```

onde:

| Variável | Significado |
|---|---|
| `t1` | início da atividade 1 |
| `d1` | duração da atividade 1 |
| `t2` | início da atividade 2 |
| `d2` | duração da atividade 2 |

O resultado representa o instante no qual todas as predecessoras já concluíram suas execuções.

Isso implementa diretamente a lógica de sincronização temporal do CPM.

---

# Relação com o Caminho Crítico

Considere o trecho do modelo:

```text
F -> J
I -> J
```

A atividade `J` depende simultaneamente de:

- encanamento interno (`F`);
- instalação elétrica (`I`).

Supondo:

| Atividade | ES | Duração | EF |
|---|---|---|---|
| F | 8 | 10 | 18 |
| I | 10 | 5 | 15 |

A atividade `J` somente poderá iniciar quando ambas terminarem. Logo:

```text
ES(J)=max(18,15)=18
```

Esse mecanismo é exatamente o cálculo do avanço direto do CPM.

A função `max_sum_time` implementa automaticamente essa propagação temporal dentro da rede.

---

# Algoritmo do CPM no Modelo

O modelo implementa implicitamente o algoritmo clássico do CPM através da propagação temporal dos tokens.

O avanço direto ocorre quando os tokens percorrem a rede acumulando:

```text
tempo + duração
```

A sincronização seleciona automaticamente o maior término acumulado entre fluxos concorrentes.

Assim, o modelo executa dinamicamente:

```text
EF = ES + duração
```

e:

```text
ES sucessora = max(EF predecessoras)
```

Em estruturas de convergência, o token sucessor somente é produzido quando o maior tempo acumulado é atingido.

O avanço reverso do CPM pode ser interpretado através da análise do tempo máximo admissível sem atrasar o projeto. Assim, o modelo permite analisar implicitamente:

```text
LF = LS + duração
```

e:

```text
Folga = LS - ES
```

As atividades pertencentes ao caminho crítico possuem folga igual a zero, ou seja, qualquer atraso nessas atividades impacta diretamente a duração total do projeto.

Portanto, a própria dinâmica da rede implementa o cálculo do caminho crítico.

---

# Paralelismo e Concorrência

Após a atividade `C`, o modelo permite múltiplos fluxos paralelos:

```text
C -> D
C -> E
C -> I
```

Isso representa concorrência operacional real.

Após a construção das paredes:

- telhado pode iniciar;
- instalações hidráulicas podem iniciar;
- instalações elétricas podem iniciar.

Esse paralelismo reduz o tempo total da obra e representa corretamente a dinâmica de execução em canteiros reais.

---

# Compartilhamento de Recursos

O modelo também implementa troca e reutilização de equipes.

Exemplos identificados:

```text
F>J TROCA EQUIPE
E>H TROCA EQUIPE
```

Isso representa:

- realocação operacional;
- compartilhamento de recursos;
- restrições de capacidade;
- dependência entre frentes de trabalho.

Esse tipo de modelagem normalmente não aparece em exemplos acadêmicos básicos de Redes de Petri.

---

# Relação entre CPM e CPN

O modelo implementa uma extensão dinâmica do CPM.

| CPM clássico | Modelo CPN |
|---|---|
| grafo estático | rede executável |
| cálculo matemático | simulação dinâmica |
| foco temporal | foco temporal e operacional |
| recursos implícitos | recursos explícitos |
| precedência | precedência + sincronização |
| sem concorrência explícita | concorrência explícita |

Assim, a CPN permite representar simultaneamente:

- lógica;
- tempo;
- recursos;
- concorrência;
- sincronização;
- propagação temporal.

---

# Aplicações Avançadas

Esse tipo de modelagem possui aplicações importantes em:

- BIM 4D;
- Process Mining;
- Digital Twins;
- simulação operacional;
- monitoramento de obras;
- planejamento inteligente;
- análise preditiva;
- controle de execução.

A integração entre CPN, CPM e Digital Twins permite transformar o cronograma em um modelo executável e monitorável em tempo real.

---

# Conclusão

O modelo apresentado demonstra como Redes de Petri Coloridas podem ser utilizadas para representar sistemas complexos de construção civil com elevado nível de realismo operacional.

O modelo implementa simultaneamente:

- precedência;
- propagação temporal;
- sincronização;
- compartilhamento de equipes;
- concorrência;
- restrições operacionais;
- mecanismos equivalentes ao CPM;
- cálculo dinâmico do caminho crítico.

Portanto, o modelo em rede Petri Colorida [GPCrev*.cpn](https://raw.githubusercontent.com/UniRobotica/Petri-nets/refs/heads/main/Exemplos/GPCrev9_Aula.cpn), disponível no repositório deste curso,  **aproxima-se significativamente da dinâmica real de execução de obras, permitindo representar não apenas o fluxo lógico das atividades, mas também os aspectos temporais e operacionais envolvidos no gerenciamento da construção civil**.

## Referências

- Jensen, K. *Colored Petri Nets*.
- Murata, T. *Petri Nets: Properties, Analysis and Applications*.
- van der Aalst, W. *Process Mining*.
- PMBOK 8ª edição.
- CPN Tools Documentation.