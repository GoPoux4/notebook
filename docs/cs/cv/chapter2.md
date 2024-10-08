# 图像的形成

## Camera Model

### Pinhole Camera

小孔成像原理。孔径越小，成像越清晰，但是光线越暗

### Lens Camera

透镜成像原理。

Focal Length: 焦距，记作 f

高斯公式：$\frac{1}{f} = \frac{1}{o} + \frac{1}{i}$。其中 o 为物距，i 为像距（要投影到光轴上）

#### Image Magnification

光学变焦：焦距越大，成像越大。长焦镜头（焦距大），广角镜头（焦距小）

#### Field of View (FOV)

视场角，广角镜头视场角大，长焦镜头视场角小。还取决于传感器的大小（sensor size）

<div align="center"> <img src="/assets/img/CS/cv/chapter2/fov.png" width="50%"/> </div>

#### Aperture

光圈，控制进入相机的光线量。光圈越大，进入的光线越多，成像越亮。光圈越小，进入的光线越少，成像越暗。

f-number: 光圈大小的标准，\(N = \frac{f}{D}\)，其中 f 为焦距，D 为光圈直径

#### Lens Defocus

失焦，光线不聚焦在传感器上，导致图像模糊。通过调整焦距，使得物体成像在传感器上。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/defocus.png" width="50%"/> </div>

通过调整焦距，使得物体成像在传感器上。

失焦会产生 blur circle，其大小为 \(b = \frac{D}{i'} \cdot |i‘ - i|\)，其中 D 为光圈直径，i' 为像距，i 为传感器到透镜的距离。

减小光圈直径，可以减小失焦的影响。

#### Depth of Field (DOF)

景深，物体成像清晰的范围。DOF 受光圈大小、焦距、物距、传感器大小等因素影响。产生原因：传感器是离散的。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/dof.png" width="50%"/> </div>

特写：光圈大，焦距小，DOF 小，背景模糊；全景：光圈小，焦距大，DOF 大，背景清晰。

## Geometric Image Formation

三维到二维的投影过程。

### Perspective Projection

<div align="center"> <img src="/assets/img/CS/cv/chapter2/pinhole_projection.png" width="50%"/> </div>

\(u = x \cdot \frac{f}{z}, v = y \cdot \frac{f}{z}\)

!!! note "Homogeneous Coordinates"
    
    Converting to homogeneous coordinates:

    \[
    (x, y) \Rightarrow
    \begin{bmatrix}
    x \\
    y \\
    1
    \end{bmatrix}
    \]

    \[
    (x, y, z) \Rightarrow
    \begin{bmatrix}
    x \\
    y \\
    z \\
    1
    \end{bmatrix}
    \]

    Converting from homogeneous to Cartesian:

    \[
    \begin{bmatrix}
    x \\
    y \\
    w
    \end{bmatrix}
    \Rightarrow (x/w, y/w)
    \]

    \[
    \begin{bmatrix}
    x \\
    y \\
    z \\
    w
    \end{bmatrix}
    \Rightarrow (x/w, y/w, z/w)
    \]

    上面的变换可以写成：

    \[
    \begin{bmatrix}
    f & 0 & 0 & 0 \\
    0 & f & 0 & 0 \\
    0 & 0 & 1 & 0
    \end{bmatrix}
    \begin{bmatrix}
    x \\
    y \\
    z \\
    1
    \end{bmatrix}
    =
    \begin{bmatrix}
    fx \\
    fy \\
    z
    \end{bmatrix}
    \cong
    \begin{bmatrix}
    x \cdot \frac{f}{z} \\
    y \cdot \frac{f}{z} \\
    1
    \end{bmatrix}
    \]

### Vanishing Points

灭点，平行线在成像平面上的交点。只取决于线的方向。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/vanishing_points.png" width="50%"/> </div>

### Vanishing Line 

灭线，两组平行线的灭点的连线，对应三维空间中的平面，同一平面上的平行线的灭点在同一灭线上。

### Perspective Distortion

畸变，由于透镜的形状和位置，导致图像中的物体形状发生变化。比如两个平行线在图像中不再平行。

轴移相机：透镜和传感器不在同一轴上，消除畸变。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/perspective_distortion.png" width="30%"/> </div>

!!! example "Radial distortion"

    径向畸变，由于透镜的形状，导致图像中的物体形状发生变化。比如圆形变成椭圆。分为枕形畸变和桶形畸变。

    <div align="center"> <img src="/assets/img/CS/cv/chapter2/radial_distortion.png" width="50%"/> </div>

## Photometric Image Formation

图像的亮度和颜色。

### Image Sensor

每个 pixel 记录光强和颜色。使用 CMOS 传感器进行光电转换。

### Shutter 

快门，控制光线进入传感器的时间。快门时间（Shutter speed）越长，进入的光线越多，成像越亮。

#### Rolling Shutter

滚动快门，逐行扫描，导致快门时间不同，产生图像畸变。

与之相对的是 Global Shutter，同时扫描所有像素。

### Color Sensing

#### HSV Space

Hue（色相）、Saturation（饱和度）、Value（亮度）。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/hsv.png" width="50%"/> </div>

#### RGB Space

用 RGB 表示颜色。每个 pixel 记录三个通道的光强。

#### Bayer Filter

贝尔滤波器，使用 Bayer Filter 进行颜色传感。每个 pixel 只记录一个通道的光强，通过插值得到其他通道的光强。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/bayer_filter.png" width="50%"/> </div>

两个绿色通道：人眼对绿色敏感。

### Shading

着色，根据光源、物体表面、相机位置等因素，计算反射光的颜色。

#### BRDF

双向反射分布函数，描述物体表面的反射特性。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/brdf.png" width="50%"/> </div>

#### Diffuse Reflection

漫反射，光线均匀反射，与观测角度无关。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/diffuse_reflection.png" width="60%"/> </div>

#### Specular Reflection

镜面反射，光线按照反射角度反射，只与观测角度有关。

<div align="center"> <img src="/assets/img/CS/cv/chapter2/specular_reflection.png" width="60%"/> </div>

#### Ambient Light

环境光，和物体表面无关。

\(L_a = k_a \cdot I_a\)，其中 \(k_a\) 为环境光系数，\(I_a\) 为环境光强度。

#### Blinn-Phong Reflection Model

Blinn-Phong 反射模型，结合漫反射、镜面反射和环境光，近似表示 BRDF。

\[
    \begin{aligned}
    L &= L_a + L_d + L_s \\
    &= k_a I_a + k_d (I / r^2) \max(0, \mathbf{n} \cdot \mathbf{l}) + k_s (I / r^2) \max(0, \mathbf{n} \cdot \mathbf{h})^p
    \end{aligned}
\]