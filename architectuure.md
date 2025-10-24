graph TB
    subgraph "Vehicles"
        A[Vehicles with Sentry Mode]
    end

    subgraph "AWS Cloud"
        subgraph "Ingress"
            B[API Gateway]
            C[Application Load Balancer]
        end

        subgraph "Compute"
            D[cabin-sentry-service - EKS]
        end

        subgraph "Storage"
            E[MySQL Database - RDS]
            F[S3 Storage]
        end

        subgraph "AI/ML"
            G[Sensenova AI Model]
        end
    end

    subgraph "Client Applications"
        H[Mobile/Web Clients]
    end

    A --> B
    B --> C
    C --> D
    D --> E
    D --> F
    D --> G
    G --> D
    D --> E
    E --> H
    H --> B
