# Aula — Modelagem de Sistemas (Controle Programável)

## Objetivos
- Entender o que são Sistemas a Eventos Discretos (SED) e por que modelá-los.
- Aplicar um fluxo de modelagem coerente do requisito ao PLC.
- Conhecer três formas clássicas de modelagem: **máquinas de estados/statecharts**, **redes de Petri** e **GRAFCET/SFC**.
- Relacionar modelos formais com a implementação em linguagens da **IEC 61131-3 (Ed. 4, 2025)**.

---

## 1. Conceitos fundamentais
- **SED**: sistemas cujo estado muda por eventos discretos no tempo (sensores, comandos).
- **Controle programável**: algoritmo de controle para execução em PLCs.
- **Vantagens da modelagem**: clareza, detecção de impasses, verificação de segurança.

<figure>
  <img src="figures/fig1.png" width="400"/>
  <figcaption>Figura 1 — Diagrama de blocos ilustrativo (sistema com realimentação).</figcaption>
</figure>

---

## 2. Fluxo de modelagem
1. Requisitos  
2. Fronteira e I/O  
3. Decomposição funcional  
4. Escolha de formalismo  
5. Validação  
6. Geração do controle → PLC  
7. Teste / comissionamento  

---

## 3. Modelos de comportamento

### 3.1 Máquinas de estados / Statecharts
<figure>
  <img src="figures/fig2.png" width="450"/>
  <figcaption>Figura 2 — Exemplo de máquina de estados (lógica de FSM).</figcaption>
</figure>

---

### 3.2 Redes de Petri
<figure>
  <img src="figures/fig3.png" width="450"/>
  <figcaption>Figura 3 — Rede de Petri básica.</figcaption>
</figure>

---

### 3.3 GRAFCET / SFC
<figure>
  <img src="figures/fig4.png" width="450"/>
  <figcaption>Figura 4 — Exemplo de GRAFCET (SFC).</figcaption>
</figure>

---

## 4. Do modelo ao PLC
<figure>
  <img src="figures/fig5.png" width="450"/>
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
