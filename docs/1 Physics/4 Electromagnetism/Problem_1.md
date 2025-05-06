# 📘 Simulating the Effects of the Lorentz Force




### 🎓 Motivation

For university students studying physics or engineering, mastering the behavior of charged particles under electric and magnetic fields is essential. This topic forms the basis of key technologies like MRI machines, cyclotrons, and plasma confinement in fusion energy. Through computational simulations and visualizations of the Lorentz force, we can deepen our understanding of theoretical concepts while also building practical skills in numerical methods and scientific programming.



## 🔎 1. Introduction

The **Lorentz force** is fundamental to the motion of charged particles in electric and magnetic fields. It is defined as:

$$
\vec{F} = q(\vec{E} + \vec{v} \times \vec{B})
$$

Where:

- $\vec{F}$: Force on the particle (N)  
- $q$: Charge of the particle (C)  
- $\vec{E}$: Electric field (V/m)  
- $\vec{v}$: Velocity of the particle (m/s)  
- $\vec{B}$: Magnetic field (T)

---

## ⚙️ 2. Why Is It Important?

The Lorentz force plays a key role in systems such as:

- 🔬 **Particle accelerators**  
- 🧪 **Mass spectrometers**  
- 🌌 **Astrophysical plasmas**  
- ⚡ **Plasma confinement in fusion devices**

---

## 🧭 3. Types of Motion

| Field Configuration         | Resulting Motion            |
|-----------------------------|-----------------------------|
| Only $\vec{B}$              | Circular / helical path     |
| Only $\vec{E}$              | Linear acceleration         |
| $\vec{E} \parallel \vec{B}$ | Spiral acceleration         |
| $\vec{E} \perp \vec{B}$     | Drift motion ($\vec{E} \times \vec{B}$ drift) |

---

## 📐 4. Key Physical Concepts

- **Larmor Radius**:

$$
r_L = \frac{mv_\perp}{qB}
$$

- **Cyclotron Frequency**:

$$
\omega_c = \frac{qB}{m}
$$

- **Drift Velocity (for crossed fields):**

$$
\vec{v}_d = \frac{\vec{E} \times \vec{B}}{B^2}
$$

---

## 🚀 5. Real-World Systems Using the Lorentz Force

### ✅ Cyclotrons

Particles spiral outward in a magnetic field while gaining energy from electric fields.

### ✅ Mass Spectrometers

Charged particles are separated based on curvature of their paths in known fields.

### ✅ Magnetic Confinement (Tokamaks)

Charged particles follow helical trajectories to remain confined inside plasma reactors.

---

## 📌 6. Simulation Goals

We aim to simulate and visualize:

1. Uniform magnetic field → Circular motion  
2. Combined electric and magnetic fields → Helical motion  
3. Crossed $\vec{E}$ and $\vec{B}$ fields → Drift motion  
4. Effects of varying:
   - $|\vec{E}|$, $|\vec{B}|$
   - Initial velocity
   - Particle charge and mass

---

## 📈 7. What Will Be Delivered

- ✅ Clean Python code for simulations  
- ✅ Clear 2D and 3D trajectory plots  
- ✅ Physical interpretation of motion  
- ✅ Extension ideas: non-uniform fields, particle beams, collisions

---


## 🌀 Circular Motion in a Uniform Magnetic Field

```python
import numpy as np
import matplotlib.pyplot as plt

# Lorentz force function
def lorentz_force(q, m, E, B, v):
    return (q / m) * (E + np.cross(v, B))

# Euler integration
def simulate_particle_motion(q, m, E, B, v0, r0, dt=1e-5, steps=5000):
    r = np.zeros((steps, 3))
    v = np.zeros((steps, 3))
    r[0], v[0] = r0, v0

    for i in range(1, steps):
        a = lorentz_force(q, m, E, B, v[i - 1])
        v[i] = v[i - 1] + a * dt
        r[i] = r[i - 1] + v[i] * dt

    return r

# Parameters (user-specified)
q = 1.0                 # charge in Coulombs
m = 0.001               # mass in kg (1 gram)
B = np.array([0, 0, 1]) # magnetic field in z-direction (T)
E = np.array([0, 0, 0]) # no electric field
v0 = np.array([1.0, 0, 0]) # initial velocity in x-direction (m/s)
r0 = np.array([0, 0, 0])   # initial position

# Run simulation
trajectory = simulate_particle_motion(q, m, E, B, v0, r0)

# Plot result
plt.figure(figsize=(6, 6))
plt.plot(trajectory[:, 0], trajectory[:, 1], color='blue')
plt.title("Circular Motion in a Uniform Magnetic Field")
plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.grid(True)
plt.axis('equal')
plt.show()

```

![alt text](image-8.png)


---

### 🔍 Explanation: Circular Motion in a Uniform Magnetic Field

This plot demonstrates the **circular trajectory** of a charged particle moving in a uniform magnetic field when no electric field is present.

* The magnetic field \$\vec{B}\$ is directed **out of the screen** (along the z-axis).

* The particle starts with an initial velocity \$\vec{v}\_0\$ in the x-direction.

* Since the **Lorentz force** is always perpendicular to the velocity:

  $$
  \vec{F} = q \vec{v} \times \vec{B}
  $$

  the particle undergoes **uniform circular motion** in the x-y plane.

* The radius of the circular path is determined by:

  $$
  r = \frac{m v}{q B}
  $$

This behavior is fundamental in cyclotrons, mass spectrometers, and other charged particle control systems.

---




## 🧵 Helical Motion in a Uniform Magnetic Field

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Parameters for the helical motion
charge = 1.0  # Charge of the particle (Coulombs)
mass = 1.0    # Mass of the particle (kg)
velocity = 1.0  # Initial velocity of the particle (m/s)
B_field = np.array([0, 0, 1])  # Magnetic field direction (along z-axis)
time_steps = 1000  # Number of time steps in the simulation
radius = 1.0      # Radius of the circular path
pitch = 0.05      # The rate at which the spiral rises

# Time array
t = np.linspace(0, 20, time_steps)

# Velocity components (circular motion + linear motion)
omega = velocity / radius  # Angular frequency (rad/s)
x = radius * np.cos(omega * t)  # x component (circular motion)
y = radius * np.sin(omega * t)  # y component (circular motion)
z = pitch * t  # z component (linear motion)

# Plotting the spiral
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

ax.plot(x, y, z, label='Helical Motion', color='b')

# Labels and title
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
ax.set_title('Helical Motion in a Uniform Magnetic Field')

# Show the plot
plt.show()
```
![alt text](image-1.png)


Here's a Python code to simulate **Helical Motion in a Uniform Magnetic Field** with the specified values for charge $q = 1 \, \text{C}$ and mass $m = 1 \, \text{g}$:

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Lorentz force function
def lorentz_force(q, m, E, B, v):
    return (q / m) * (E + np.cross(v, B))

# Euler integration
def simulate_particle_motion(q, m, E, B, v0, r0, dt=1e-11, steps=2000):
    r = np.zeros((steps, 3))
    v = np.zeros((steps, 3))
    r[0], v[0] = r0, v0

    for i in range(1, steps):
        a = lorentz_force(q, m, E, B, v[i-1])
        v[i] = v[i-1] + a * dt
        r[i] = r[i-1] + v[i] * dt

    return r

# Parameters
q = 1  # charge (C)
m = 1e-3  # mass (kg) - 1g = 1e-3 kg
B = np.array([0, 0, 1])  # magnetic field (T), in the z-direction
E = np.array([0, 0, 0])  # no electric field
v0 = np.array([1e5, 0, 1e5])  # initial velocity with components in x and z directions
r0 = np.array([0, 0, 0])  # initial position at origin

# Run simulation
trajectory = simulate_particle_motion(q, m, E, B, v0, r0)

# Plot in 3D
fig = plt.figure(figsize=(8, 6))
ax = fig.add_subplot(111, projection='3d')
ax.plot(trajectory[:, 0], trajectory[:, 1], trajectory[:, 2], color='green')
ax.set_title("Helical Motion in a Magnetic Field")
ax.set_xlabel("x (m)")
ax.set_ylabel("y (m)")
ax.set_zlabel("z (m)")
plt.show()
```

### Explanation:

* The particle's initial velocity $\vec{v_0}$ has components in both the $x$- and $z$-directions.
* The magnetic field $\vec{B}$ is in the $z$-direction, causing the particle to move in a **helical path** around the magnetic field lines.
* The **Lorentz force** $q(\vec{v} \times \vec{B})$ acts perpendicular to the velocity of the particle, resulting in circular motion in the $x$-$y$ plane, while the motion along the $z$-axis is due to the initial velocity component in the $z$-direction.
* The **radius of the circular motion** and the **pitch** of the helix depend on the initial velocity components, charge, and magnetic field strength.

This code will generate a 3D plot showing the helical path of a charged particle moving in the magnetic field.


## 🔀 Drift Motion in Crossed Electric and Magnetic Fields

Here's the **Python code** and all **explanations translated into English**, showing the **drift motion of a charged particle** in **crossed electric and magnetic fields**. The visualization includes **multiple lines and plots** to clearly display both the **circular (gyration) motion** and the **overall drift**.

---

### ✅ Python Code: Drift Motion in Crossed $\vec{E}$ and $\vec{B}$ Fields

```python
import numpy as np
import matplotlib.pyplot as plt

# Physical constants and initial parameters
q = 1.0       # Charge (C)
m = 1.0       # Mass (kg)
E = np.array([0, 5, 0])    # Electric field (V/m)
B = np.array([0, 0, 2])    # Magnetic field (T)

# Initial conditions
r0 = np.array([0.0, 0.0, 0.0])  # Initial position
v0 = np.array([1.0, 0.0, 0.0])  # Initial velocity

# Time settings
dt = 0.01
t_max = 30
steps = int(t_max / dt)

# Arrays to store position and velocity over time
r = np.zeros((steps, 3))
v = np.zeros((steps, 3))
t = np.linspace(0, t_max, steps)

r[0] = r0
v[0] = v0

# Solve motion using a simple time-stepping method (Euler method)
for i in range(steps - 1):
    # Lorentz force: F = q(E + v × B)
    a = (q / m) * (E + np.cross(v[i], B))
    v[i + 1] = v[i] + a * dt
    r[i + 1] = r[i] + v[i + 1] * dt

# Theoretical drift velocity: v_d = (E × B) / B^2
v_drift = np.cross(E, B) / np.linalg.norm(B)**2
print("Drift velocity (E x B / B^2):", v_drift)

# Plotting
fig, axs = plt.subplots(2, 2, figsize=(12, 10))

# x-y plane motion: shows spiral (gyration) + drift
axs[0, 0].plot(r[:, 0], r[:, 1], label='Particle Path', color='blue')
axs[0, 0].quiver(r[::200, 0], r[::200, 1], v[::200, 0], v[::200, 1], color='red', scale=20, width=0.003)
axs[0, 0].set_title('Motion in x-y Plane (Spiral + Drift)')
axs[0, 0].set_xlabel('x')
axs[0, 0].set_ylabel('y')
axs[0, 0].grid()
axs[0, 0].legend()

# x position over time
axs[0, 1].plot(t, r[:, 0], label='x(t)', color='green')
axs[0, 1].set_title('x Position Over Time')
axs[0, 1].set_xlabel('Time (s)')
axs[0, 1].set_ylabel('x (m)')
axs[0, 1].grid()
axs[0, 1].legend()

# y position over time
axs[1, 0].plot(t, r[:, 1], label='y(t)', color='purple')
axs[1, 0].set_title('y Position Over Time')
axs[1, 0].set_xlabel('Time (s)')
axs[1, 0].set_ylabel('y (m)')
axs[1, 0].grid()
axs[1, 0].legend()

# Optional: 3D trajectory
from mpl_toolkits.mplot3d import Axes3D
ax3d = fig.add_subplot(224, projection='3d')
ax3d.plot(r[:, 0], r[:, 1], r[:, 2], label='3D Trajectory')
ax3d.set_title('3D Motion')
ax3d.set_xlabel('x')
ax3d.set_ylabel('y')
ax3d.set_zlabel('z')
ax3d.legend()

plt.tight_layout()
plt.show()
```

![alt text](image-2.png)
---

### 🔍 Key Features:

* **Spiral + Drift** motion in the x-y plane is clearly shown.
* Multiple **subplots** give you time-evolution views of x(t), y(t), and 3D trajectory.
* The **drift velocity** $\vec{v}_d = \frac{\vec{E} \times \vec{B}}{B^2}$ is computed and printed.
* **Vector arrows** (`quiver`) indicate particle velocity at sampled times.
* You can increase the drift effect by increasing the strength of the electric field.











This plot shows the drift motion of a charged particle when an electric field $\vec{E}$ is crossed with a magnetic field $\vec{B}$. The particle doesn’t spiral but instead moves in a straight line, drifting with a velocity given by:

$$
\vec{v}_\text{drift} = \frac{\vec{E} \times \vec{B}}{B^2}
$$

This velocity is perpendicular to both fields. This phenomenon is crucial in devices like magnetic confinement in fusion reactors.


---



Here's a Python simulation of an **interesting trajectory**: a *complex 3D motion* of a charged particle where both **electric and magnetic fields are present and not aligned**. This setup leads to a **non-trivial spiraling drift** — a motion that combines rotation, drift, and acceleration.

---

### 🔁 Complex 3D Trajectory with Non-Perpendicular E and B Fields

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Lorentz force function
def lorentz_force(q, m, E, B, v):
    return (q / m) * (E + np.cross(v, B))

# Euler integration for particle motion
def simulate_particle_motion(q, m, E, B, v0, r0, dt=1e-4, steps=5000):
    r = np.zeros((steps, 3))
    v = np.zeros((steps, 3))
    r[0], v[0] = r0, v0

    for i in range(1, steps):
        a = lorentz_force(q, m, E, B, v[i-1])
        v[i] = v[i-1] + a * dt
        r[i] = r[i-1] + v[i] * dt

    return r

# Parameters
q = 1.0                  # charge in Coulombs
m = 0.001                # mass in kg (1 g)
E = np.array([100, 50, 0])    # electric field (V/m)
B = np.array([0, 0, 1])       # magnetic field (T)
v0 = np.array([10, 10, 5])    # initial velocity (m/s)
r0 = np.array([0, 0, 0])      # initial position

# Simulate trajectory
trajectory = simulate_particle_motion(q, m, E, B, v0, r0)

# Plot the trajectory in 3D
fig = plt.figure(figsize=(10, 7))
ax = fig.add_subplot(111, projection='3d')
ax.plot(trajectory[:, 0], trajectory[:, 1], trajectory[:, 2], color='purple')
ax.set_title("Complex 3D Trajectory in Combined E and B Fields")
ax.set_xlabel("x (m)")
ax.set_ylabel("y (m)")
ax.set_zlabel("z (m)")
plt.show()
```

---

![alt text](image-7.png)

### ✨ What Does This Show?

In this configuration:

* The **electric field** is not perpendicular to the magnetic field.
* The **initial velocity** also has components in multiple directions.

This causes the particle to spiral while simultaneously being **accelerated along a drift path**, leading to a **non-circular, non-helical** trajectory that reflects the true complexity of Lorentz-force dynamics in real-world plasmas.




### ✅ Conclusion

The Lorentz force is a fundamental concept that governs the behavior of charged particles in electromagnetic fields. Through simulations and visualizations, we've seen how particles move in circular, helical, or drifting paths depending on the configuration of electric and magnetic fields. These dynamics are not just theoretical — they underpin real-world technologies such as particle accelerators, fusion devices, and mass spectrometers. By adjusting parameters like field strength and initial velocity, we gain deeper insight into particle control and confinement, which is essential for advancing both scientific research and practical applications in modern physics and engineering.

---

