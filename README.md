# -
图表专用
```mermaid
graph TD
    subgraph Inputs
        I["输入序列 (Inputs)"] --> IE["输入 Embedding"]
        IE --> PE1["位置编码 (Positional Encoding)"]
        PE1 --> E_Input["编码器输入"]
    end

    subgraph Encoder_Stack ["Encoder (编码器堆叠 N次)"]
        style Encoder_Stack fill:#e1f5fe,stroke:#01579b,stroke-width:2px
        E_Input --> MHA1["多头注意力机制\n(Multi-Head Attention)"]
        MHA1 --> AN1["残差连接 & 层归一化\n(Add & Norm)"]
        E_Input -.-> AN1
        
        AN1 --> FFN1["前馈神经网络\n(Feed Forward)"]
        FFN1 --> AN2["残差连接 & 层归一化\n(Add & Norm)"]
        AN1 -.-> AN2
    end

    subgraph Decoder_Stack ["Decoder (解码器堆叠 N次)"]
        style Decoder_Stack fill:#fff3e0,stroke:#e65100,stroke-width:2px
        
        OD_Input["解码器输入"] --> MMHA2["掩码多头注意力\n(Masked Multi-Head Attention)"]
        MMHA2 --> AN3["残差连接 & 层归一化\n(Add & Norm)"]
        OD_Input -.-> AN3
        
        AN3 --> MHA2["编码器-解码器注意力\n(Multi-Head Attention)"]
        AN2 -- "K, V 矩阵" --> MHA2
        MHA2 --> AN4["残差连接 & 层归一化\n(Add & Norm)"]
        AN3 -.-> AN4

        AN4 --> FFN2["前馈神经网络\n(Feed Forward)"]
        FFN2 --> AN5["残差连接 & 层归一化\n(Add & Norm)"]
        AN4 -.-> AN5
    end

    subgraph Outputs_Flow
        O["目标序列 (Outputs) - shifted right"] --> OE["输出 Embedding"]
        OE --> PE2["位置编码 (Positional Encoding)"]
        PE2 --> OD_Input
    end
    
    subgraph Final_Projection
        AN5 --> Linear["线性层 (Linear)"]
        Linear --> Softmax["Softmax"]
        Softmax --> Output_Probs["输出概率 (Output Probabilities)"]
    end

    %% Styling connections
    linkStyle 3 stroke:#01579b,stroke-width:2px,fill:none
    linkStyle 6 stroke:#01579b,stroke-width:2px,fill:none
    linkStyle 11 stroke:#e65100,stroke-width:2px,fill:none
    linkStyle 15 stroke:#e65100,stroke-width:2px,fill:none
    linkStyle 18 stroke:#e65100,stroke-width:2px,fill:none
    linkStyle 13 stroke-width:3px,fill:none,stroke:red
