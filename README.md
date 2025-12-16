# Interactive Bézier Curve with Physics & Sensor Control

## Overview

This project implements an **interactive cubic Bézier curve** in the browser using **plain HTML Canvas and JavaScript**. The curve behaves like a **springy rope** whose shape changes smoothly in response to **mouse and touch input**. All Bézier math, tangent computation, and physics are implemented manually without using any built‑in Bézier or physics libraries.

---

## Bézier Curve Mathematics

The curve is defined by four control points:

* **P₀, P₃** – fixed endpoints
* **P₁, P₂** – dynamic control points

The cubic Bézier equation used is:

[ B(t) = (1 - t)^3 P_0 + 3(1 - t)^2 t P_1 + 3(1 - t) t^2 P_2 + t^3 P_3 ]

The curve is rendered by **sampling the curve at small intervals (Δt = 0.01)** and connecting consecutive points with straight line segments. No Canvas Bézier APIs are used.

---

## Tangent Visualization

To visualize the direction of the curve, tangent vectors are computed using the analytical derivative:

[ B'(t) = 3(1 - t)^2 (P_1 - P_0) + 6(1 - t)t (P_2 - P_1) + 3t^2 (P_3 - P_2) ]

At regular values of ( t ):

* The derivative vector is computed
* Normalized
* Rendered as a short line segment

This provides an intuitive view of the curve’s local direction.

---

## Physics Model (Spring–Damper)

The dynamic control points (**P₁ and P₂**) follow a simple spring–damper system to produce smooth, rope‑like motion.

**Acceleration model:**

[ a = -k (x - x_{target}) - c v ]

Where:

* ( k ) is the spring stiffness
* ( c ) is the damping coefficient
* ( x_{target} ) is derived from mouse or touch input

The motion is integrated each frame:

```
v += a * dt
x += v * dt
```

This results in natural overshoot and settling behavior similar to a flexible rope.

---

## Interaction & Rendering

* Mouse movement influences the target positions of P₁ and P₂
* Touch input is supported for mobile browsers
* Control points can be dragged directly
* Rendering uses `requestAnimationFrame` to maintain ~60 FPS

Each frame:

1. Input is read
2. Physics is updated
3. Bézier curve and tangents are redrawn

---

## Design Choices

* **Manual math** ensures full control and understanding of Bézier behavior
* **Polyline sampling** avoids reliance on built‑in Bézier APIs
* **Spring–damper physics** provides smooth, realistic motion with minimal complexity
* **Canvas + JavaScript** keeps the implementation lightweight and portable

---

## Compliance

* No prebuilt Bézier, animation, or physics libraries used
* All math and motion logic implemented from scratch
* Fully interactive and real‑time

---

## Demo

A short screen recording (≤30s) demonstrates:

* Interactive control via mouse/touch
* Smooth rope‑like motion
* Tangent visualization along the curve
