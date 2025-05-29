# Investigating the Range as a Function of the Angle of Projection

## Motivation

Projectile motion, while seemingly simple, offers a rich playground for exploring fundamental principles of physics. The problem is straightforward: analyze how the range of a projectile depends on its angle of projection. Yet, beneath this simplicity lies a complex and versatile framework.

This notebook explores how variations in launch parameters affect projectile range and how this fundamental concept extends to real-world scenarios such as sports, engineering, and aerospace.

---

## 1. Theoretical Foundation

### Governing Equations of Motion

For a projectile launched with initial velocity \( v_0 \) at an angle \( \theta \) from horizontal on flat ground, neglecting air resistance:

**Horizontal motion:**
\[
x(t) = v_0 \cos(\theta) \cdot t
\]

**Vertical motion:**
\[
y(t) = v_0 \sin(\theta) \cdot t - \frac{1}{2} g t^2
\]

### Time of Flight

Solve for \( t \) when the projectile returns to the ground \( (y = 0) \):

\[
T = \frac{2 v_0 \sin(\theta)}{g}
\]

### Range

Plugging time of flight into horizontal displacement:

\[
R(\theta) = \frac{v_0^2 \sin(2\theta)}{g}
\]

### Symmetry of Range

- The function \( \sin(2\theta) \) is symmetric around \( 45^\circ \).
- Pairs of angles that sum to 90° (e.g. 30° and 60°) yield the same range.

---

## 2. Analysis of the Range

### Visualizing Range for Various Initial Velocities

```python
import numpy as np
import matplotlib.pyplot as plt

g = 9.81  # gravitational acceleration (m/s^2)

def range_function(v0, theta_deg):
    theta_rad = np.radians(theta_deg)
    return (v0**2 * np.sin(2 * theta_rad)) / g

angles = np.linspace(0, 90, 500)
v0_values = [10, 20, 30, 40]  # m/s

plt.figure(figsize=(10, 6))
for v0 in v0_values:
    ranges = range_function(v0, angles)
    plt.plot(angles, ranges, label=f'v₀ = {v0} m/s')

plt.title('Range vs. Angle of Projection')
plt.xlabel('Launch Angle (degrees)')
plt.ylabel('Range (meters)')
plt.legend()
plt.grid(True)
plt.show()
````

### Observations:

* Maximum range occurs at 45°
* Ranges are symmetric about 45°
* Doubling the velocity increases range **fourfold** (quadratic dependence on $v_0$)

---

## 3. Practical Applications

### Sports

* Soccer: Optimizing free kicks for maximum reach
* Basketball: Shooting angles for consistent scoring
* Javelin/shot put: Athletes aim for optimal launch angle close to 45°

### Engineering

* Military ballistics: Launch angles affect target reach
* Water fountains: Parabolic arcs rely on ideal launch mechanics

### Astrophysics

* Launching probes or rockets: Launch trajectory and gravitational escape
* Planetary environments: Adjusting for different values of gravity (e.g. Moon, Mars)

---

## 4. Implementation

### General Simulation Function

```python
def simulate_projectile(v0, theta_deg, g=9.81, steps=1000):
    theta_rad = np.radians(theta_deg)
    vx = v0 * np.cos(theta_rad)
    vy = v0 * np.sin(theta_rad)

    t_flight = 2 * vy / g
    t = np.linspace(0, t_flight, steps)
    x = vx * t
    y = vy * t - 0.5 * g * t**2

    return x, y

# Example visualization
x, y = simulate_projectile(30, 45)
plt.figure(figsize=(8, 5))
plt.plot(x, y)
plt.title("Projectile Trajectory at 45°")
plt.xlabel("Distance (m)")
plt.ylabel("Height (m)")
plt.grid(True)
plt.show()
```

### Varying Angle Trajectories

```python
angles_to_plot = [15, 30, 45, 60, 75]
v0 = 25

plt.figure(figsize=(10, 6))
for angle in angles_to_plot:
    x, y = simulate_projectile(v0, angle)
    plt.plot(x, y, label=f'{angle}°')

plt.title("Trajectories at Various Launch Angles (v₀ = 25 m/s)")
plt.xlabel("Distance (m)")
plt.ylabel("Height (m)")
plt.legend()
plt.grid(True)
plt.show()
```

---

## 5. Model Extensions and Limitations

### Limitations

* Assumes **no air resistance**: In real life, drag significantly reduces range.
* Assumes **flat terrain**: Uneven surfaces affect final position and time of flight.
* Assumes **constant gravity**: Not accurate for very large-scale trajectories.
* Assumes **point-mass projectile**: Neglects shape, spin, and aerodynamic effects.

### Possible Extensions

* **Air resistance model:**

  Drag force: $F_d = \frac{1}{2} C_d \rho A v^2$

* **Launch from height $h$:**

  $$
  y(t) = h + v_0 \sin(\theta)t - \frac{1}{2}gt^2
  $$

* **Numerical simulation**: Euler or Runge-Kutta methods for complex forces

* **Different planets**:

```python
# Comparison on Earth vs. Moon
planet_g = {'Earth': 9.81, 'Moon': 1.62}
theta = 45
v0 = 20

plt.figure(figsize=(8, 5))
for planet, g_val in planet_g.items():
    x, y = simulate_projectile(v0, theta, g=g_val)
    plt.plot(x, y, label=planet)

plt.title("Projectile Motion on Earth vs. Moon")
plt.xlabel("Distance (m)")
plt.ylabel("Height (m)")
plt.legend()
plt.grid(True)
plt.show()
```

---

## Conclusion

Projectile motion is a cornerstone concept in classical physics that demonstrates how mathematics models real-world phenomena. The idealized parabolic motion provides essential insights, and its extensions help solve complex problems in sports, engineering, and space science. Through visualization and simulation, we deepen our understanding of these fundamental dynamics.


