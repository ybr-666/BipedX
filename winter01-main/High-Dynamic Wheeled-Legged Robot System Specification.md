* * # High-Dynamic Wheeled-Legged Robot System Specification
      **Project Code**: Titan-Kick (105-125-70-273)
      **Configuration**: Asymmetric Double Parallel Linkage with Pneumatic Augmentation
    
      ---
    
      ## 1. Full Kinematic Parameter Matrix (全拓扑连杆参数矩阵)
    
      本系统由两个级联的闭链四连杆机构组成，以下为**完整的几何定义**，缺一不可。
    
      ### A. Active & Output Group (主动与输出组)
      这是机器人的骨架，决定了整体尺寸和离地间隙。
    
      | 构件名称 (Component)             | 定义 (Definition)                       | 长度 (Length) | 备注 (Notes)                                |
      | :------------------------------- | :-------------------------------------- | :------------ | :------------------------------------------ |
      | **大腿主曲柄 (Main Thigh)**      | 髋关节轴 $\leftrightarrow$ 膝关节轴     | **105 mm**    | 主动件，由电机直接驱动。                    |
      | **小腿力臂 (Shank Upper Lever)** | 膝关节轴 $\leftrightarrow$ 下拉杆连接点 | **70 mm**     | **[受力核心]** 承受极高拉力，需做结构补强。 |
      | **小腿主长 (Shank Main)**        | 膝关节轴 $\leftrightarrow$ 轮轴中心     | **273 mm**    | **[越障核心]** 决定最终离地高度。           |
    
      ### B. Transmission & Passive Group (传动与被动组)
      这是机器人的“肌肉传动链”，负责将电机的扭矩放大并传递给小腿。**（此处数据补全）**
    
      | 构件名称 (Component)              | 定义 (Definition)                         | 长度 (Length) | 备注 (Notes)                                      |
      | :-------------------------------- | :---------------------------------------- | :------------ | :------------------------------------------------ |
      | **上传动杆 (Upper Link)**         | 机架上挂点 $\leftrightarrow$ 摇臂上挂点   | **105 mm**    | **[必须等于大腿长]** 负责约束摇臂姿态。           |
      | **下传动杆 (Lower Link)**         | 摇臂下挂点 $\leftrightarrow$ 小腿力臂顶端 | **105 mm**    | 连接摇臂与小腿，传递拉力。                        |
      | **中间摇臂-上段 (Rocker Top)**    | 摇臂轴心 $\leftrightarrow$ 摇臂上挂点     | **70 mm**     | **[输入端]** 接收上传动杆的约束力。               |
      | **中间摇臂-下段 (Rocker Bottom)** | 摇臂轴心 $\leftrightarrow$ 摇臂下挂点     | **125 mm**    | **[输出端]** 速度放大级，长力臂输出。             |
      | **机架几何 (Chassis Geometry)**   | 髋关节轴 $\leftrightarrow$ 机架上挂点     | **70 mm**     | **[必须等于摇臂上段]** 确保构成第一级平行四边形。 |
    
      > **📐 拓扑逻辑验证 (Topology Check)**：
      > * **第一级闭链**：机架(70) + 大腿(105) + 上传动杆(105) + 摇臂上段(70) = **标准平行四边形**。
      > * **第二级闭链**：摇臂下段(125) + 下传动杆(105) + 小腿力臂(70) + 大腿(105) = **非线性放大四连杆**。
    
      ---
    
      ## 2. Pneumatic Gravity Compensation (气动重力补偿系统)
    
      采用 **挖掘机式跨膝关节布局 (Cross-Joint Excavator Layout)**，利用气动势能实现“零能耗站立”。
    
      ### 装配方案 (Assembly Architecture)
      * **锚点 A (定子端)**：
          * 位于 **中间摇臂与大腿连接关节 (Rocker-Thigh Pivot)**。
          * 采用 **同轴偏置设计**，轴销向外侧延伸 **15mm**，安装球头。
      * **锚点 B (动子端)**：
          * 位于 **小腿总成中下部** (根据 SW 仿真确定具体孔位)。
          * **必须加装内部实体铝柱补强**，防止碳管受压爆裂。
    
      ---
    
      ## 3. System Advantages (系统优势)
    
      1.  **二级变速效应 (Dual-Stage Velocity)**：
          * 通过摇臂 (70:125) 的杠杆比，将电机角速度物理放大，使末端获得极高的瞬时线速度，适合**爆发式跳跃**。
      2.  **力矩非线性优化 (Torque Optimization)**：
          * 70mm 的超长小腿力臂，在膝关节折叠状态下（起跳初期）能提供远超常规设计的有效力臂，改善起跳初段的加速性能。
      3.  **被动顺应缓冲 (Passive Compliance)**：
          * 气撑杆作为并联的“液压阻尼器”，在落地瞬间吸收高频冲击能量，保护电机减速箱齿轮。
    
      ---
    
      ## 4. Critical Risks (关键风险提示)
    
      > **⚠️ Engineering Warning**
    
      * **死点奇异性 (Kinematic Singularity)**：
          * 需严格校核深蹲极限位置，确保气撑杆轴线始终位于膝关节轴心的 **下方/外侧**，防止出现力学死点。
      * **结构屈曲风险 (Buckling Risk)**：
          * 由于 B 点位于小腿中部，气杆推力会产生巨大的弯矩。**小腿管件 B 点处必须进行内部实体填充**。
      * **行程匹配 (Stroke Matching)**：
          * 气撑杆行程必须预留 **10mm 以上的安全余量**，严禁活塞杆触底硬撞击。