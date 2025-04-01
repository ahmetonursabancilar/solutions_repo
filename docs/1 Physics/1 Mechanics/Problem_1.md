**Theoretical Foundation of Projectile Motion**

### Derivation of Governing Equations

Projectile motion can be described by decomposing the motion into horizontal and vertical components. The motion follows Newton’s second law of motion:

$$F = ma$$

For a projectile launched with an initial velocity $v_0$ at an angle $\theta$, the velocity components are:

$$v_{0x} = v_0 \cos\theta$$  
$$v_{0y} = v_0 \sin\theta$$

Assuming constant gravitational acceleration $g$ acting downward, the equations of motion arise from Newton’s second law:

#### Horizontal Motion:
Since there is no acceleration in the horizontal direction (neglecting air resistance), the equation of motion simplifies to:

$$\frac{d^2x}{dt^2} = 0$$

Integrating twice gives:

$$x(t) = v_0 \cos\theta \cdot t$$

#### Vertical Motion:
The vertical motion follows:

$$\frac{d^2y}{dt^2} = -g$$

Integrating once:

$$\frac{dy}{dt} = v_0 \sin\theta - g t$$

Integrating again:

$$y(t) = v_0 \sin\theta \cdot t - \frac{1}{2} g t^2$$

### Time of Flight
To determine the total time the projectile remains in the air, we set $y = 0$ (assuming launch and landing at the same height):

$$0 = v_0 \sin\theta \cdot t - \frac{1}{2} g t^2$$

Solving for $t$:

$$t = \frac{2 v_0 \sin\theta}{g}$$

### Range of the Projectile
The horizontal range $R$ is found by substituting the time of flight into the horizontal motion equation:

$$R = v_{0x} t$$

$$R = (v_0 \cos\theta) \times \frac{2 v_0 \sin\theta}{g}$$

Using the identity $2 \sin\theta \cos\theta = \sin 2\theta$:

$$R = \frac{v_0^2 \sin 2\theta}{g}$$

### Effect of Launch Angle on Range
The range equation shows that $R$ depends on $\sin 2\theta$. The maximum range occurs when $\sin 2\theta = 1$, which happens at $2\theta = 90^\circ$ or $\theta = 45^\circ$. Therefore, the optimal launch angle for maximum range is **45 degrees**.

### Influence of Initial Conditions
Several factors influence the projectile’s trajectory:

1. **Initial Velocity $v_0$**: Higher speeds result in a longer range.
2. **Gravitational Acceleration $g$**: Increased gravity shortens the range.
3. **Launch Angle $\theta$**: Different angles yield different parabolic trajectories, with 45° providing maximum range.
4. **Launch Height**: If the projectile starts from a height $h$, the time of flight increases, thereby affecting the range.

### Investigation of Horizontal Range Dependence
#### Dependence on Angle of Projection
From the range equation:

$$R = \frac{v_0^2 \sin 2\theta}{g}$$

- The function $\sin 2\theta$ dictates how $R$ varies with angle.
- $R$ increases as $\theta$ moves from 0° to 45° and decreases thereafter up to 90°.
- This symmetric behavior results in the same range for complementary angles (e.g., 30° and 60°).

#### Effect of Other Parameters
- **Initial Velocity ($v_0$)**: Since range is proportional to $v_0^2$, doubling the velocity quadruples the range.
- **Gravitational Acceleration ($g$)**: Range is inversely proportional to $g$, meaning that stronger gravity (such as on Jupiter) reduces range, while weaker gravity (such as on the Moon) increases range.
- **Launch Height ($h$)**: An increased launch height extends the flight time, thereby increasing the range.

### Family of Solutions
By varying initial conditions, a family of parabolic trajectories emerges:
- **Different launch angles** create different paths, with the same range for complementary angles.
- **Different initial velocities** scale the trajectory while maintaining its shape.
- **Different gravitational accelerations** alter both the height and range.
- **Different launch heights** modify the total flight time and final landing position.

### Conclusion
Projectile motion is governed by fundamental principles of kinematics and provides a versatile framework to analyze various physical scenarios. The dependence of range on the launch angle follows a sinusoidal relationship, with an optimal angle of 45° in the absence of other factors like air resistance or variable terrain. Variations in velocity and gravity significantly influence the trajectory, making projectile motion a crucial concept in physics applications such as ballistics, sports, and space travel.

