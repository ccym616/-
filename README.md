# -
图表专用
```mermaid
graph LR
    I[输入序列] --> E_Block[Encoder 编码器堆叠 (xN)]
    
    T[目标序列 (Shifted Right)] --> D_Block_Start[Decoder 解码器开始部分]
    
    E_Block -- 编码信息 (K,V) --> D_Block_Mid[Decoder 交互注意力层]
    D_Block_Start --> D_Block_Mid
    D_Block_Mid --> D_Block_End[Decoder 前馈层部分 (xN)]
    
    D_Block_End --> Linear --> Softmax --> Output[预测下一个 Token]

    style E_Block fill:#e1f5fe,stroke:#01579b
    style D_Block_Start fill:#fff3e0,stroke:#e65100
    style D_Block_Mid fill:#ffccbc,stroke:#bf360c
    style D_Block_End fill:#fff3e0,stroke:#e65100
