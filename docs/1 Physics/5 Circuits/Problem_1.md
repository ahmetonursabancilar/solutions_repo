

## 🎓 **Slide 1: Title Slide**

* **Title**: **Equivalent Resistance Using Graph Theory**
* **Subtitle**: *A Systematic Approach to Simplifying Complex Circuits*


📽 *Animation Suggestion*: Fade-in for title and subtitle, with a background image of a complex circuit.

---

## 💡 **Slide 2: Motivation**

### **Why Use Graph Theory for Equivalent Resistance?**

* Traditional series/parallel rules become messy in complex circuits.
* Graph theory offers a structured, algorithmic approach.
* Powerful for:

  * Circuit simulation tools
  * Optimization engines
  * Educational purposes

📈 *Visual*: Show a tangled resistor network vs. a neat graph.

---

## 🔗 **Slide 3: Key Concept — Circuits as Graphs**

### **Graph Representation:**

* **Nodes** → Junctions
* **Edges** → Resistors (weights = resistance)

🖼️ *Visual Aid*: Circuit diagram on the left, graph on the right (linked by arrows).

🧠 *Analogy*: Think of it like a road network with travel cost = resistance.

---

## 🧪 **Slide 4: Case Study Introduction**

### **We’ll Walk Through an Example Circuit**

* Circuit with resistors: $R_1, R_2, R_3, R_4, R_5$
* **Source**: $B+$, **Sink**: $B-$

🖼️ *Visual*: Realistic resistor diagram with labels.

---

## 📊 **Slide 5: Graph Representation**

### **Graph Version of the Circuit**

* **Nodes**: $B+, n_1, n_2, n_3, n_4, B-$
* **Edges**: Represent resistors with weights

👁️ *Visual*: `networkx`-based graph visualization

---

## 🧑‍💻 **Slide 6:Presentation: Step-by-Step Simplification of a Complex Resistor Circuit**

## Introduction

We start with a complex circuit between terminals **B+** and **B-** involving multiple resistors connected in series and parallel. Our goal is to simplify the circuit step-by-step and find the total equivalent resistance $R_{final}$.

---

## Step 1: Initial Circuit

The initial circuit looks like this:

$$
B^+ - R_1 - (R_2 \parallel R_3) - R_4 - (R_5 \parallel R_6) - B^-
$$

Here, $R_2$ and $R_3$ are in parallel, as are $R_5$ and $R_6$. Other resistors are in series.

---

### Code: Drawing the Initial Circuit

```python
import schemdraw
import schemdraw.elements as elm

with schemdraw.Drawing() as d:
    d += elm.SourceV().label('B+').up()
    d += elm.Line().right().length(1)
    
    d += elm.Resistor().right().label('R1')
    
    # Parallel R2 and R3 branches
    d += elm.Dot()
    d.push()
    d += elm.Line().up().length(1)
    d += elm.Resistor().right().label('R2')
    d += elm.Line().down().length(1)
    d.pop()
    d += elm.Line().down().length(1)
    d += elm.Resistor().right().label('R3')
    d += elm.Line().up().length(1)
    d += elm.Dot()
    
    d += elm.Resistor().right().label('R4')
    
    # Parallel R5 and R6 branches
    d += elm.Dot()
    d.push()
    d += elm.Line().up().length(1)
    d += elm.Resistor().right().label('R5')
    d += elm.Line().down().length(1)
    d.pop()
    d += elm.Line().down().length(1)
    d += elm.Resistor().right().label('R6')
    d += elm.Line().up().length(1)
    d += elm.Dot()
    
    d += elm.Line().right().length(1)
    d += elm.Ground().label('B-')
```

![alt text](<adim 1.png>)


---

## Step 2: Calculate $R_{23}$ - Parallel Combination of $R_2$ and $R_3$

$$
R_{23} = \left( \frac{1}{R_2} + \frac{1}{R_3} \right)^{-1}
$$

---

### Code: Compute $R_{23}$

```python
from sympy import symbols, simplify

R1, R2, R3, R4, R5, R6 = symbols('R1 R2 R3 R4 R5 R6')

R23 = simplify(1 / (1/R2 + 1/R3))
print(f"R23 = {R23}")
```

$$
R_{23} = \frac{R_2 \times R_3}{R_2 + R_3}
$$



---

### Code: Draw Circuit with $R_{23}$ instead of $R_2 \parallel R_3$

```python
with schemdraw.Drawing() as d:
    d += elm.SourceV().label('B+').up()
    d += elm.Line().right().length(1)

    d += elm.Resistor().right().label('R1')
    d += elm.Resistor().right().label('R23')
    d += elm.Resistor().right().label('R4')

    # Parallel R5 and R6 remain
    d += elm.Dot()
    d.push()
    d += elm.Line().up().length(1)
    d += elm.Resistor().right().label('R5')
    d += elm.Line().down().length(1)
    d.pop()
    d += elm.Line().down().length(1)
    d += elm.Resistor().right().label('R6')
    d += elm.Line().up().length(1)
    d += elm.Dot()

    d += elm.Line().right().length(1)
    d += elm.Ground().label('B-')
```

![alt text](<adim 2.png>)

---

## Step 3: Calculate $R_{56}$ - Parallel Combination of $R_5$ and $R_6$

$$
R_{56} = \left( \frac{1}{R_5} + \frac{1}{R_6} \right)^{-1}
$$

---

### Code: Compute $R_{56}$

```python
R56 = simplify(1 / (1/R5 + 1/R6))
print(f"R56 = {R56}")
```
$$
R_{56} = \frac{R_5 \times R_6}{R_5 + R_6}
$$

---

### Code: Draw Circuit with $R_{56}$ instead of $R_5 \parallel R_6$

```python
with schemdraw.Drawing() as d:
    d += elm.SourceV().label('B+').up()
    d += elm.Line().right().length(1)

    d += elm.Resistor().right().label('R1')
    d += elm.Resistor().right().label('R23')
    d += elm.Resistor().right().label('R4')
    d += elm.Resistor().right().label('R56')

    d += elm.Line().right().length(1)
    d += elm.Ground().label('B-')
```

![alt text](<adim 3.png>)

---

## Step 4: Calculate Total Equivalent Resistance $R_{final}$

Since now all resistors are in series:

$$
R_{final} = R_1 + R_{23} + R_4 + R_{56}
$$

---

### Code: Compute $R_{final}$

```python
R_final = simplify(R1 + R23 + R4 + R56)
print(f"Total Equivalent Resistance, R_final = {R_final}")
```
$$
R_{final} = R_1 + \frac{R_2 \times R_3}{R_2 + R_3} + R_4 + \frac{R_5 \times R_6}{R_5 + R_6}
$$

---

### Code: Draw Final Simplified Circuit Showing $R_{final}$

```python
with schemdraw.Drawing() as d:
    d += elm.SourceV().label('B+').up()
    d += elm.Line().right().length(1)

    d += elm.Resistor().right().label(r'$R_{final}$', fontsize=14)

    d += elm.Line().right().length(1)
    d += elm.Ground().label('B-')

    # Label explaining the formula under the circuit
    d += elm.Label().down().at((0, -2)).label(r'$R_{final} = R_1 + R_{23} + R_4 + R_{56}$', fontsize=14)
```
![alt text](<adim 4.png>)

---

# Summary

* We started with a complex resistor network.
* Simplified parallel resistor pairs step-by-step.
* Finally expressed the entire circuit as a single equivalent resistor $R_{final}$.
* Visualized each step with clear schematics to aid understanding.

---













---

## 🔁 **Slide 7: More Complex Cases**

### **Loops, Bridges, and Combinations**

* Can handle:

  * Nested branches
  * Multi-path topologies
* Fully systematic!

🧠 *Analogy*: Like simplifying a maze using rules.

---

## ⏱️ **Slide 8: Efficiency & Optimization**

### **Performance & Scaling**

* Time complexity: Up to $O(n^2)$
* Optimizations:

  * Priority queues for bottlenecks
  * Memoization for subgraphs
  * Use of `networkx.MultiGraph` for true parallel edges

⚙️ *Advanced Tip*: Integrate with SPICE or circuit solver engines.

---

## 🧠 **Slide 9: Conclusion**

### **Key Takeaways**

* Graph theory enables systematic, programmable simplification.
* Great for teaching, software, and automation.
* Bridges physics with computation.

💬 *Quote*:

> "By treating circuits as graphs, we turn intuition into algorithms."

---

## 📚 **Slide 10: References & Acknowledgments**

* **Resources**:

  * *Introduction to Graph Theory* by Douglas West
  * *The Art of Electronics* by Horowitz & Hill
  * `networkx` and `matplotlib` documentation
* **Thanks**:

  * Collaborators, mentors, and open-source libraries

---

```python 

# Gerekli kütüphaneler (Colab için)
!pip install schemdraw

import schemdraw
import schemdraw.elements as elm
from sympy import symbols, simplify
import matplotlib.pyplot as plt

# Direnç sembolleri
R1, R2, R3, R4, R5, R6 = symbols('R1 R2 R3 R4 R5 R6')

# Sayısal değerler (Ohm cinsinden)
values = {
    R1: 10,
    R2: 20,
    R3: 30,
    R4: 15,
    R5: 25,
    R6: 50
}

# Paralel direnç formülleri (sadelestirmeler)
R23 = simplify((R2 * R3) / (R2 + R3))
R56 = simplify((R5 * R6) / (R5 + R6))
R_final = simplify(R1 + R23 + R4 + R56)

# Sembolikten sayısala dönüş
R23_val = float(R23.subs(values))
R56_val = float(R56.subs(values))
Rfinal_val = float(R_final.subs(values))

# --- STEP 1: Orijinal devre ---
def draw_step1():
    with schemdraw.Drawing() as d:
        d += elm.SourceV().label('B+').up()
        d += elm.Resistor().right().label(f'R1 = {values[R1]}Ω')
        d += elm.Dot()
        d.push()
        d += elm.Line().up().length(1)
        d += elm.Resistor().right().label(f'R2 = {values[R2]}Ω')
        d += elm.Line().down().length(1)
        d.pop()
        d += elm.Line().down().length(1)
        d += elm.Resistor().right().label(f'R3 = {values[R3]}Ω')
        d += elm.Line().up().length(1)
        d += elm.Dot()
        d += elm.Resistor().right().label(f'R4 = {values[R4]}Ω')
        d += elm.Dot()
        d.push()
        d += elm.Line().up().length(1)
        d += elm.Resistor().right().label(f'R5 = {values[R5]}Ω')
        d += elm.Line().down().length(1)
        d.pop()
        d += elm.Line().down().length(1)
        d += elm.Resistor().right().label(f'R6 = {values[R6]}Ω')
        d += elm.Line().up().length(1)
        d += elm.Dot()
        d += elm.Line().right()
        d += elm.Ground().label('B-')
        plt.title("Step 1: Original Circuit")
        d.draw()

# --- STEP 2: R2 || R3 = R23 ---
def draw_step2():
    with schemdraw.Drawing() as d:
        d += elm.SourceV().label('B+').up()
        d += elm.Resistor().right().label(f'R1 = {values[R1]}Ω')
        d += elm.Resistor().right().label(f'R23 = {round(R23_val, 2)}Ω')
        d += elm.Resistor().right().label(f'R4 = {values[R4]}Ω')
        d += elm.Dot()
        d.push()
        d += elm.Line().up().length(1)
        d += elm.Resistor().right().label(f'R5 = {values[R5]}Ω')
        d += elm.Line().down().length(1)
        d.pop()
        d += elm.Line().down().length(1)
        d += elm.Resistor().right().label(f'R6 = {values[R6]}Ω')
        d += elm.Line().up().length(1)
        d += elm.Dot()
        d += elm.Line().right()
        d += elm.Ground().label('B-')
        plt.title("Step 2: R2 || R3 simplified to R23")
        d.draw()

# --- STEP 3: R5 || R6 = R56 ---
def draw_step3():
    with schemdraw.Drawing() as d:
        d += elm.SourceV().label('B+').up()
        d += elm.Resistor().right().label(f'R1 = {values[R1]}Ω')
        d += elm.Resistor().right().label(f'R23 = {round(R23_val, 2)}Ω')
        d += elm.Resistor().right().label(f'R4 = {values[R4]}Ω')
        d += elm.Resistor().right().label(f'R56 = {round(R56_val, 2)}Ω')
        d += elm.Ground().label('B-')
        plt.title("Step 3: R5 || R6 simplified to R56")
        d.draw()

# --- STEP 4: Rfinal (toplam) ---
def draw_step4():
    with schemdraw.Drawing() as d:
        d += elm.SourceV().label('B+').up()
        d += elm.Resistor().right().label(f'Rfinal = {round(Rfinal_val, 2)}Ω')
        d += elm.Ground().label('B-')
        plt.title("Step 4: Final Equivalent Resistance")
        d.draw()

# --- HEPSİNİ ÇALIŞTIR ---
draw_step1()
draw_step2()
draw_step3()
draw_step4()

print("Summary:")
print(f"R23 = (R2 * R3) / (R2 + R3) = {round(R23_val, 2)} Ω")
print(f"R56 = (R5 * R6) / (R5 + R6) = {round(R56_val, 2)} Ω")
print(f"Rfinal = R1 + R23 + R4 + R56 = {round(Rfinal_val, 2)} Ω")
```

![alt text](dsadasd.png)