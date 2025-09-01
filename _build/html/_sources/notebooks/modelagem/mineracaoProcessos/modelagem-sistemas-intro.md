# Aula — Modelagem de Sistemas (baseada em *Controle Programável* de Paulo Eigi Miyagi)

## Objetivos
- Entender o que são Sistemas a Eventos Discretos (SED) e por que modelá-los {cite:t}`Miyagi1996`.
- Aplicar um fluxo de modelagem coerente com {cite:t}`Miyagi1996`, do requisito ao PLC.
- Conhecer três formas clássicas de modelagem: **máquinas de estados/statecharts** {cite:t}`Harel1987`, **redes de Petri** {cite:t}`Murata1989` e **GRAFCET/SFC** {cite:t}`IEC60848-2013`.
- Relacionar modelos formais com a implementação em linguagens da **IEC 61131-3 (Ed. 4, 2025)** {cite:t}`IEC61131-3-2025`.

---

## 1. Conceitos fundamentais
- **SED**: sistemas cujo estado muda por eventos discretos no tempo (sensores, comandos) {cite:t}`Miyagi1996`.
- **Controle programável**: algoritmo de controle para execução em PLCs {cite:t}`Miyagi1996`.
- **Vantagens da modelagem**: clareza, detecção de impasses, verificação de segurança {cite:t}`Murata1989`.

<figure>
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Block_Diagram_for_Feedback.svg/800px-Block_Diagram_for_Feedback.svg.png" width="400"/>
  <figcaption>Figura 1 — Diagrama de blocos ilustrativo (sistema com realimentação).</figcaption>
</figure>

---

## 2. Fluxo de modelagem
1. Requisitos  
2. Fronteira e I/O  
3. Decomposição funcional  
4. Escolha de formalismo  
5. Validação  
6. Geração do controle → PLC {cite:t}`IEC61131-3-2025`  
7. Teste / comissionamento  

---

## 3. Modelos de comportamento

### 3.1 Máquinas de estados / Statecharts
<figure>
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/41/Finite_State_Machine_Logic.svg/383px-Finite_State_Machine_Logic.svg.png" width="450"/>
  <figcaption>Figura 2 — Exemplo de máquina de estados (lógica de FSM).</figcaption>
</figure>

---

### 3.2 Redes de Petri
<figure>
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/64/Petri_net_diagram.png/640px-Petri_net_diagram.png" width="450"/>
  <figcaption>Figura 3 — Rede de Petri básica.</figcaption>
</figure>

---

### 3.3 GRAFCET / SFC
<figure>
  <img src="https://upload.wikimedia.org/wikipedia/commons/9/9d/Example_Grafcet_SFC-fr.svg" width="450"/>
  <figcaption>Figura 4 — Exemplo de GRAFCET (SFC).</figcaption>
</figure>

---

## 4. Do modelo ao PLC
<figure>
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Ladder_logic_diagram.png/640px-Ladder_logic_diagram.png" width="450"/>
  <figcaption>Figura 5 — Mapeamento conceitual GRAFCET → Ladder.</figcaption>
</figure>

---

## 5. Exemplo Didático — Porta Automática de Garagem

- **Entradas**: sensor de presença, botão.  
- **Saídas**: motor, luz.

---

## Referências

```{bibliography}
:filter: docname in docnames
