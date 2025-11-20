```mermaid
[graph TD
    subgraph Loja (Ponto de Venda/Tablet)
        A[Cliente Pede para pagar em EUR (€100)] --> B{Operador insere €100};
        B --> C[Chamada API para BTCPay (Valor: €100)];
        F[BTCPay Valida Recebimento] --> G{Ecrã: "PAGO"};
    end

    subgraph BTCPay Server (Gateway)
        C --> D[Gera Fatura/QR Code Dinâmico (Ex: 0.002 BTC)];
        D --> E{Cliente Paga 0.002 BTC};
        E --> F;
        F --> H[Webhook/Confirmação de Fatura (ID + Valor Original €100)];
    end

    subgraph Seu Backend de Liquidação (O Seu Serviço)
        H --> I(Nginx/Balanceador de Carga);
        I --> J[Fila de Mensagens (Redis/RabbitMQ)];
        J --> K(Serviço de Processamento SAGA);
        K --> L1(API Kraken: Ordem de Venda BTC);
        L1 --> L2(API Kraken: Transferência SEPA EUR (Valor - Fee));
        L2 --> M[Conta Bancária do Comerciante];
        M --> N(Notificação Final de Recebimento);
    end

    H --> K;
    G --> N;

    style A fill:#e0f7fa,stroke:#00bcd4
    style G fill:#4caf50,stroke:#2e7d32
    style K fill:#ffecb3,stroke:#ff6f00
    style M fill:#c8e6c9,stroke:#388e3c
    style H fill:#f8bbd0,stroke:#c2185b]
```
