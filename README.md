# Ray Tracing in One Weekend

教程内容

- [Ray Tracing in One Weekend](https://raytracing.github.io/books/RayTracingInOneWeekend.html)

## 4. Rays, a Simple Camera, and Background

### Sending Rays Into the Scene

一个插值颜色技巧:

- $\text{blendedValue} = (1-t)\cdot\text{startValue} + t\cdot\text{endValue}$

绘制天空时, 虽然是按照 Ray 的 $y$ 值分配颜色, 但这是归一化的向量, 所以是天空颜色会有弧度的.

```c++
color ray_color(const ray& r) {
    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```

## 5. Adding a Sphere

> In graphics, you almost always want your formulas to be in terms of vectors

### 1. 球内, 球上, 球外 依次满足以下式子

$$
x^2 + y^2 + z^2 < R^2 \\ 
x^2 + y^2 + z^2 = R^2 \\
x^2 + y^2 + z^2 > R^2 
$$

### 2. 球心 $C(C_x, C_y, C_z)$ 的表达式

$$(x-C_x)^2 + (y-C_y)^2 + (z-Cz)^2 = R^2$$

### 3. 对于一般的点, 判断它在球上的方式
   
- 点 $P$, 是 $\bold{A}$, $\vec{t}$ 组成的直线上的一点$, 满足 $\bold{P}(t) =\bold{A} + t\vec{b}$ 
- 球心 $C(C_x, C_y, C_z)$
- 点 $P$ 在球面上

$$
(P-C)\cdot(P-C) = (x-C_x)^2 + (y-C_y)^2 + (z-C_z)^2 = r^2\\
(P-C)\cdot(P-C) = r^2 \\ 
(A+tb-C)\cdot(A+tb-C) = r^2 \\
 t^2 \mathbf{b} \cdot \mathbf{b}
     + 2t \mathbf{b} \cdot (\mathbf{A}-\mathbf{C})
     + (\mathbf{A}-\mathbf{C}) \cdot (\mathbf{A}-\mathbf{C}) - r^2 = 0
$$

根据上述二次方程根的个数, 可以判断出直线是否与球相交

![img](https://raytracing.github.io/images/fig-1.04-ray-sphere.jpg)

> any point P that satisfies this equation is on the sphere

## 6. Antialiasing

- Generating Pixels with Multiple Samples
- 多次在目标点附近随机采样, 结果取均值获取最终结果

## 7. Diffuse Materials

- mix and match geometry and materials
  - we can assign a material to multiple spheres, or vice versa
- geometry and material are tightly bound
  - useful for procedural objects where the geometry and material are linked

原理:
- Diffuse objects that don’t emit light merely take on the color of their surroundings, but they modulate that with their own intrinsic color. 
- Really any algorithm that randomizes direction will produce surfaces that look matte.


pick a random point in a unit radius sphere
- a rejection method



## summary

ray tracing, 记录 ray 路径, 计算获取 ray 的颜色