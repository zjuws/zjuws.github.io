<section id="notes" markdown="1">

<div class="section-heading" markdown="1">

<div markdown="1">

02 / FIELD NOTES
{: .eyebrow }

## 游戏图形技术笔记

</div>

先看管线，再拆原理，最后回到性能。

</div>

<div class="notes" markdown="1">

<article class="note-featured" markdown="1">

<span class="note-type">插帧 / GPU 实现 · 深入笔记</span>

### 轻量级 GPU 插帧：光流、重投影与融合

相位法与金字塔 LK，从运动估计、Shader 求解，到误差可信度和帧 Warp 融合。

<a class="note-link" href="notes/frame-interpolation.html">阅读完整算法笔记</a>

</article>

<article markdown="1">

<span class="note-type">纹理采样 / 基础</span>

### MIP 级别：屏幕梯度如何决定采样精度

纹理坐标变化率把屏幕像素映射到纹素覆盖范围。

<details markdown="1">

<summary>阅读笔记 · 约 2 分钟</summary>

<div class="detail" markdown="1">

设归一化纹理坐标为 (u, v)，纹理尺寸为 W × H。先把屏幕方向的 UV 梯度换成纹素单位：

~~~
gx = (W · du/dx, H · dv/dx)
gy = (W · du/dy, H · dv/dy)
ρ  = max(length(gx), length(gy))
λ  ≈ log₂(ρ)
~~~

ρ = 4 表示一个屏幕像素在较大的方向上覆盖约 4 个纹素，因此 λ ≈ 2，对应缩小到原尺寸 1/4 的 MIP。ρ = 1 时 λ = 0。

这是常见的近似理解。实际采样还受 LOD bias、范围限制、过滤模式和各向异性过滤影响；它们决定最终如何选择和混合纹素。

</div>

</details>

</article>

<article markdown="1">

<span class="note-type">性能分析 / 方法</span>

### 帧率之外，为什么更应该观察帧时间

平均值掩盖卡顿，帧时间分布更接近玩家的实际体验。

<details markdown="1">

<summary>阅读笔记 · 约 2 分钟</summary>

<div class="detail" markdown="1">

60 FPS 对应约 16.67 ms 的单帧预算。平均帧率相同的两组数据，可能有完全不同的长帧数量和连续卡顿。

优化前先固定场景、分辨率与测试过程，再对比帧时间曲线和高分位帧时间。进一步拆分 CPU 与 GPU 时间，避免只凭整体帧率猜测瓶颈。

系统层优化还需要关注接入开销、同步等待和跨游戏兼容性。一次优化是否有效，应由可重复的测量和画面检查共同判断。

</div>

</details>

</article>

<article markdown="1">

<span class="note-type">AI 渲染 / 工程</span>

### Color-only 模型：少一份输入，多一份推断

降低接入成本的同时，必须面对运动和遮挡信息的缺失。

<details markdown="1">

<summary>阅读笔记 · 约 2 分钟</summary>

<div class="detail" markdown="1">

深度提供几何线索，运动矢量帮助定位历史像素。只使用颜色帧时，模型需要从图像变化中估计一部分运动与结构，这会增加歧义。

工程上可以先建立单帧基线，再加入短时历史与轻量对齐模块。测试应覆盖快速运动、遮挡露出、透明物体、镜头切换和曝光变化。

训练时可以使用额外信息辅助监督，但需要保证推理阶段不依赖这些输入。画质收益、稳定性和实际设备延迟应分别验证。

</div>

</details>

</article>

</div>

</section>
