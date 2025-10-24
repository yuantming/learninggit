
# Sentry Mode 2.0 Cloud Architecture

## High-Level Architecture Diagram

```mermaid
graph TB
    subgraph "Vehicle Layer"
        A[Vehicles with Sentry Mode] --> B[Edge Device/Sensor System]
    end

    subgraph "AWS Cloud Infrastructure"
        subgraph "Ingress Layer"
            C[API Gateway] --> D[Load Balancer]
        end

        subgraph "Compute Layer"
            D --> E[Spring Boot Application - EKS Cluster]
            E --> F[Video Processing Service]
            E --> G[AI Analysis Orchestration Service]
            E --> H[Result Aggregation Service]
        end

        subgraph "Storage Layer"
            F --> I[S3 - Video Storage]
            F --> J[S3 - Keyframe Storage]
            E --> K[DynamoDB - Event Metadata]
        end

        subgraph "AI/ML Services"
            G --> L[Multimodal AI Model - Sensenova]
            L --> M[Frame Analysis Results]
            M --> H
        end

        subgraph "Data & Analytics"
            H --> N[Processed Results Database]
            N --> O[Query API Service]
        end
    end

    subgraph "Client Access"
        P[Mobile/Web Clients] --> Q[Client API Interface]
        Q --> C
        O --> Q
    end

    style A fill:#e1f5fe
    style B fill:#e1f5fe
    style C fill:#f3e5f5
    style D fill:#f3e5f5
    style E fill:#e8f5e8
    style F fill:#e8f5e8
    style G fill:#e8f5e8
    style H fill:#e8f5e8
    style I fill:#fff3e0
    style J fill:#fff3e0
    style K fill:#fff3e0
    style L fill:#fce4ec
    style M fill:#fce4ec
    style N fill:#f1f8e9
    style O fill:#f1f8e9
    style P fill:#e0f2f1
    style Q fill:#e0f2f1
```

## Component Descriptions

### Vehicle Layer
- **Vehicles with Sentry Mode**: Tesla vehicles with activated sentry mode
- **Edge Device/Sensor System**: Onboard systems that detect events and capture video

### AWS Cloud Infrastructure

#### Ingress Layer
- **API Gateway**: Entry point for all client requests
- **Load Balancer**: Distributes incoming traffic across application instances

#### Compute Layer (Spring Boot on EKS)
- **Spring Boot Application**: Main application deployed on AWS EKS
- **Video Processing Service**: Handles video upload, validation, and preprocessing
- **AI Analysis Orchestration Service**: Manages the AI analysis workflow
- **Result Aggregation Service**: Combines frame-level analysis into event-level results

#### Storage Layer
- **S3 - Video Storage**: Stores original 30-second event videos
- **S3 - Keyframe Storage**: Stores extracted keyframes from videos
- **DynamoDB - Event Metadata**: Stores event metadata and processing status

#### AI/ML Services
- **Multimodal AI Model (Sensenova)**: Performs semantic analysis on videos and keyframes
- **Frame Analysis Results**: Intermediate results from AI model processing

#### Data & Analytics
- **Processed Results Database**: Stores final structured event analysis results
- **Query API Service**: Provides endpoints for clients to retrieve analysis results

### Client Access
- **Mobile/Web Clients**: End-user applications that display alerts and analysis
- **Client API Interface**: Interface for client applications to interact with the system
