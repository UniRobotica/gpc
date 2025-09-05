# Redes de Petri – Conceitos Fundamentais

## 1. Introdução
As **Redes de Petri** são um formalismo matemático e gráfico para modelar sistemas dinâmicos, introduzido por **Carl Adam Petri** em 1962.  
São amplamente usadas em engenharia de software, sistemas distribuídos, manufatura, telecomunicações e biologia.  

Principais características:
- Descrevem **concorrência, sincronização e causalidade**.  
- Representam graficamente **estados e eventos**.  
- Permitem **análise formal rigorosa**.  

---

## 2. Estrutura Formal

Uma rede de Petri é um **grafo bipartido direcionado** composto por:
- **Posições (places)**: estados possíveis do sistema, representados por círculos.  
- **Transições (transitions)**: eventos ou atividades, representados por retângulos/barras.  
- **Arcos (arcs)**: conectam posições e transições, nunca elementos do mesmo tipo.  
- **Tokens (fichas)**: pontos pretos nas posições, representam a **marcação** (estado atual).  

![Exemplo de Rede de Petri](figuras/rede_basica.png)

---

## 3. Marcação e Dinâmica

- O **estado** de uma rede é dado pela **marcação** `M`, isto é, a distribuição de tokens nas posições.  
- Uma **transição é habilitada** se todas as suas posições de entrada têm tokens suficientes (conforme peso dos arcos).  
- Ao **disparar**:
  - Consome tokens das entradas.  
  - Produz tokens nas saídas.  

### Exemplo:
![Exemplo de disparo](figuras/disparo.png)

1. Transição `t1` habilitada porque `p1` contém um token.  
2. Após disparo de `t1`, token é removido de `p1` e adicionado em `p2`.  

---

## 4. Extensões

### 4.1 Arcos ponderados
- Arcos podem ter peso `w > 1`.  
- Ex.: para disparar, a transição precisa de `w` tokens na posição de entrada.  

### 4.2 Arcos inibidores
- Representados com **bolinha** na ponta.  
- Uma transição **não pode disparar** se houver token na posição conectada por arco inibidor.  

![Arco Inibidor](figuras/arco_inibidor.png)

---

## 5. Exemplo Prático – Sistema Produtor/Consumidor

![Produtor-Consumidor](figuras/produtor_consumidor.png)

- `pBuffer`: representa o buffer.  
- `tProduzir`: adiciona item no buffer (token).  
- `tConsumir`: remove item do buffer.  
- Se o buffer estiver vazio (`pBuffer` sem token), `tConsumir` fica desabilitada.  
- Se o buffer estiver cheio (n tokens), `tProduzir` é desabilitada.  

Esse modelo simples já evidencia **sincronização e exclusão mútua**.  

---

## 6. Formalismo Matemático

Uma rede de Petri é definida pelo quádruplo:

