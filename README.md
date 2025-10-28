# mdtest1

```mermaid
graph LR
    subgraph "AWS Cloud"
        subgraph "Virtual Private Cloud (VPC)"
            A[Internet Gateway (IGW)]
            R[Routing Table]
            S1[Security Group: APP]
            S2[Security Group: DB]

            A --> R
            R --> S1
            R --> S2

            subgraph "Application Tier"
                VM_APP[EC2: App Server - Ubuntu]
                D[Docker Engine]
                C[Container: Flask App/ONNX]

                S1 --> VM_APP
                VM_APP --> D
                D --> C
            end

            subgraph "Data Tier"
                VM_DB[EC2: DB Server - PostgreSQL]
                S2 --> VM_DB
            end

            C -->|Private IP/Port 5432| VM_DB
        end
    end

    U[Appraiser/User] -->|HTTPS/Port 8080| VM_APP

    subgraph "Authentication & Access"
        IAM[IAM Service: RBAC]
        K[SSH Key Pair: flask-key.pem]
    end


    IAM -->|Controls Resource Access| "AWS Cloud"
    K -->|SSH Access (Port 22)| VM_APP
    K -->|SSH Access (Port 22)| VM_DB

```
